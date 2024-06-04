---
date created: 星期三, 四月 10日 2024, 10:50:40 上午
date modified: 星期三, 四月 10日 2024, 10:56:29 上午
source: https://docs.python.org/zh-cn/3/library/__main__.html
tags: 
---

# Python顶层代码环境

写python代码时，经常看到入口函数是这样定义的

```python
# -*- coding: utf-8 -*-  
  
import sys  
  
  
def main() -> int:  
pass  
  
  
if __name__ == '__main__':  
	sys.exit(main())
```

当写了`if __name__ == '__main__'`时，IDE才会识别出一个运行按钮。

首先解释`__name__ == '__main__'`：

当python模块被导入时，`__name__`会被设置为包名，一般是文件本身的名字去掉`.py`后缀：

```python
import configparser
print(configparser.__name__)
# ->'configparser'
```

如果模块是直接运行的，或者说是在顶层代码环境运行的，`__name__`值就是`__main__`。

## 什么是顶层代码环境

顶层代码时用户指定的入口文件，当你使用`python helloworld.py`运行时，`helloworld.py`就是入口文件，其所处环境就是顶层代码环境，`__name__`值就是`__main__`。

因此每个模块可以通过`if __name__ == '__main__'`来判断自己是不是入口文件，如果是入口文件，则执行一些初始化逻辑，如果自己是作为一个包被导入到其他模块，自己不是顶层环境，则不执行这些逻辑。