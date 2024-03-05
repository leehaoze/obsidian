---
date created: 星期二, 三月 5日 2024, 4:45:29 下午
date modified: 星期二, 三月 5日 2024, 5:34:26 下午
tags: 
---

# GDP 启动流程

`main`函数整体比较简洁

```go
func main() {  
    flag.Parse()  
  
    boot.MustLoadAppConfig(*appConfig)  
  
    ctx, shutdown := bootstrap.SignalContext()  
    defer shutdown()  
  
    bootstrap.MustInit(ctx)  
  
    err := servers.Start(ctx)  
    shutdown()  
  
    log.Fatalln("server exit:", err)  
}
```

## 应用配置文件解析

第一步是解析应用配置文件`app.toml`的位置，可以通过启动时带参数传入具体配置文件的位置。

框架会根据此确定`conf`目录对应的实际目录（`app.toml`文件所在的目录），以及应用的根目录(`conf`目录上一层)。

同时框架会将`app.toml`中的配置加载到全局变量`boot.defaultAppConfig`中

```go
// setEnvByConfDir 设置环境信息和路径相关的部分  
func setEnvByConfDir(confDir string) {  
    rootDir := filepath.Dir(confDir)  
    opt := env.Option{  
       RootDir: rootDir,  
       DataDir: filepath.Join(rootDir, "data"),  
       LogDir:  filepath.Join(rootDir, "log"),  
       ConfDir: confDir,  
    }  
    env.WithOption(opt)  
}
```

## 监听系统信号量

第二步启动一个协程监听系统信号量，在此创建了一个`CancelContext`，监听系统发送的信号量。

`SignalContext`中创建了一个框架使用的`ctx`，启动了一个协程监听系统发生的信号量用于退出流程，同时将`cancelFunc`封装了一下，实现了`BeforeShutDown`钩子。

```go
// SignalContext 融合了信号量的 context//  
//  返回的 fn 在进程退出前执行  
func SignalContext() (context.Context, func()) {  
    ctx, cancel := context.WithCancelCause(context.Background())  
    ch := make(chan os.Signal, 1)  
    signal.Notify(ch, syscall.SIGINT, syscall.SIGTERM, syscall.SIGQUIT)  
  
    go safely.RunVoid(func() {  
       sig := <-ch  
       errCause := fmt.Errorf("%w: received signal %s(%#v) at %s", context.Canceled, sig.String(), sig, time.Now().String())  
       cancel(errCause)  
    })  
  
    fn := func() {  
       cancel(fmt.Errorf("%w: before invoke shutdown", context.Canceled))  
       starter.InvokeBeforeShutdown()  
    }  
    return ctx, fn  
}
```

比较有意思的是`main`函数中对于`SignalContext`返回的`cancelFunc`函数的使用：

```go
ctx, shutdown := bootstrap.SignalContext()  
defer shutdown()  
  
bootstrap.MustInit(ctx)  
  
err := servers.Start(ctx)  
shutdown()
```

`defer`可以保证接下来`bootstrap.MustInit`出现`panic`时，可以通过`shutdown`来取消`ctx`，同时触发`BeforeShutdown`钩子来保证**日志落盘**等

而在`servers.Start`后，也手动触发了一次`shutdown`函数，这个原意是指`server`出现问题退出后，调用`shutdown`来触发框架的整体退出。这里其实也会触发`defer shutdown`，其实可以不需要手动调用一次。

**shutdown函数反复调用会有问题吗?**

`shutdown`函数中主要做了两件事情:
1. 调用`ctx`的`cancelFunc`
2. 调用`starter.InvokeBeforeShutdown()`

第一个`context`的`cancelFunc`是幂等的，本身支持多此重复调用，后面的调用不会有任何影响。

第二个内部函数内部使用了`sync.Once`，因此多此调用也没有关系

## 各类初始化

第三步便是`bootstrap.MustInit(ctx)`了，通过函数名字不难看出，内部进行的各类初始化必须成功。