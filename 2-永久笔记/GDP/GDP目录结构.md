---
date created: 星期二, 三月 5日 2024, 3:46:44 下午
date modified: 星期二, 三月 5日 2024, 4:45:25 下午
tags: 
---

# GDP目录结构

## 整体概览

```bash
├── bin         // 编译后的binary位置
├── bootstrap   // 初始化相关代码，框架默认生成
├── cmd         // 其他的使用 go 实现的子功能，每个目录下一个功能，编译产出目录为bin
├── conf        // 配置文件，线下测试开发
├── conf_online // 配置文件，线上环境
├── data        // 项目的数据文件目录,如项目用到的词典文件、程序产生的临时文件。
├── document    // 设计、用户使用、部署等文档
├── library     // 模块自己的公共基础库，没有 RPC 调用的可以放在这里
├── model       // 业务模型层代码
├── script      // 其他相关脚本，如 php、python、shell 脚本
├── servers     // 将一个应用的所有的 server 都放在此目录下
├── webroot     // 当对外提供 HTTP 服务时，此目录存放 css、js 等资源文件
├── .gitignore
├── ci.yml
├── Dockerfile
├── go.env
├── go.mod
├── main.go
├── Makefile
└── README.md
```
