---
date created: 星期日, 六月 16日 2024, 10:21:17 晚上
date modified: 星期日, 六月 16日 2024, 11:06:12 晚上
tags: 
---

# 1-debian允许root登录

编辑`/etc/ssh/sshd_config`文件，将`PermitRootLogin`注释取消，值修改为`Yes`即可。

修改后重新启动sshd服务

```shell
systemctl restart sshd
```