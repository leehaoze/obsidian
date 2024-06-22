---
date created: 星期四, 六月 20日 2024, 2:01:20 下午
date modified: 星期六, 六月 22日 2024, 11:26:15 上午
source: [https://wiki.debian.org/MachineId]
tags: 
---

# Debian修改machine Id

## 生成新的machine Id

```shell
rm -f /etc/machine-id /var/lib/dbus/machine-id
dbus-uuidgen --ensure=/etc/machine-id
dbus-uuidgen --ensure
```