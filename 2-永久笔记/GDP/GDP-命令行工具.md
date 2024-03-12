---
date created: 星期二, 三月 5日 2024, 3:36:26 下午
date modified: 星期二, 三月 5日 2024, 3:42:13 下午
tags: 
---

# GDP-命令行工具

> `gdp` 命令提供了创建新应用、热编译运行、生成配置和代码等能力。^[原始文档]

## 初始化新项目

````bash
gdp new baidu/searchbox/myapp
gdp new baidu/searchbox/myapp -comps=httpServer,nsheadServer
````

其中`comps`选项有以下可选值，一次添加多个组件可以用`,`隔开，默认选择`httpServer`
- `httpServer`
- `nsheadServer`
- `pbrpcServer`
- `exgraph`
- `hulk`

[原始文档]:https://gdp.baidu-int.com/gdp3/docs/quickstart/gdp/