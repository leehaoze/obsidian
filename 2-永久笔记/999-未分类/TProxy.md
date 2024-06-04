---
date created: 星期四, 四月 25日 2024, 7:19:34 晚上
date modified: 星期二, 四月 30日 2024, 11:16:34 上午
tags: 
---

# TProxy

TProxy（Transparent Proxy）是Linux内核支持的一种透明代理方式。
NAT是通过修改数据包的目的地址或者源地址实现重定向，TProxy则不会对数据包进行任何修改，对于客户端，TProxy服务器会假装自己是服务端，对于服务端，TProxy会假装自己是客户端。