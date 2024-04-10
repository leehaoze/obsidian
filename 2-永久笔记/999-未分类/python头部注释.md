---
date created: 星期三, 四月 10日 2024, 10:31:30 上午
date modified: 星期三, 四月 10日 2024, 10:39:41 上午
source: https://docs.python.org/3/reference/lexical_analysis.html#encoding-declarations
tags: 
---

# Python头部注释

python文件头部往往有两行注释，第一行为shell脚本可识别的注释，用于制定执行器位置。

```
#!/usr/local/bin/python
# -*- coding: utf-8 -*-
```

第二行类似于`# -*- coding: <encoding-name> -*-`的注释，是用来声明此文件的编码类型的。其头部注释（第一行或第二行）只要能被正则`coding[=:]\s*([-\w.]+)`匹配，就将此注释处理为编码声明。

如果没有定义，则默认为UTF-8