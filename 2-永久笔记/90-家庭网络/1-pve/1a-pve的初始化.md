---
date created: 星期六, 六月 15日 2024, 9:54:03 晚上
date modified: 星期日, 六月 16日 2024, 12:19:20 凌晨
tags: 
---

# 1a-pve的初始化

![[1a1-更换源]]

## 基础软件

```shell
apt install vim htop lm-sensors iperf3
```

## 删除订阅弹窗

```shell
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service
```

## 修改分区

```shell
lvremove pve/data
lvextend -l +100%FREE -r pve/root
```

点击数据中心-存储，删除`local-lvm`，编辑`local`，将所有的类目勾选

![[1a2-开启硬件直通]]