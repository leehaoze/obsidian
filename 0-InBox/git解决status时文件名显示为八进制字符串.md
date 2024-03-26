---
date created: 星期一, 三月 4日 2024, 6:22:34 晚上
date modified: 星期一, 三月 18日 2024, 7:49:59 晚上
state: F
tags: [git]
---

# Git解决status时文件名显示为八进制字符串

将`core.quotepath`设置为`false`即可。

```
git config --global core.quotepath false
```