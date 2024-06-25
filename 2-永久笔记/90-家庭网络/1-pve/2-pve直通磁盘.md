---
date created: 星期五, 六月 21日 2024, 8:49:20 晚上
date modified: 星期二, 六月 25日 2024, 8:50:32 晚上
tags: 
---

# 2-pve直通磁盘

## 查看磁盘列表

```shell
ls -la /dev/disk/by-id | grep -v dm | grep -v dvm | grep -v part
```

输出类似于如下

```shell
root@pve:~# ls -la /dev/disk/by-id/  | grep -v part | grep -v dm | grep -v dvm
total 0
drwxr-xr-x 2 root root 560 Jun 16 00:13 .
drwxr-xr-x 9 root root 180 Jun 16 00:13 ..
lrwxrwxrwx 1 root root   9 Jun 16 00:13 ata-HGST_HTS721010A9E630_JR1000BNJ3KVPE -> ../../sdb
lrwxrwxrwx 1 root root   9 Jun 16 00:13 ata-SOYO_SSD_WC2T_AA20230307000050 -> ../../sda
lrwxrwxrwx 1 root root  15 Jun 16 00:13 lvm-pv-uuid-HuTVJN-0Yyh-IW5b-Z3YR-79j2-0s5G-v10P0Q -> ../../nvme0n1p3
lrwxrwxrwx 1 root root  13 Jun 16 00:13 nvme-J.ZAO_5_SERIES_1TB_SSD_NKK240R000371 -> ../../nvme0n1
lrwxrwxrwx 1 root root  13 Jun 16 00:13 nvme-J.ZAO_5_SERIES_1TB_SSD_NKK240R000371_1 -> ../../nvme0n1
lrwxrwxrwx 1 root root  13 Jun 16 00:13 nvme-nvme.1e4b-4e4b4b32343052303030333731-4a2e5a414f2035205345524945532031544220535344-00000001 -> ../../nvme0n1
lrwxrwxrwx 1 root root   9 Jun 16 00:13 usb-HGST_HTS_721010A9E630_123458811460-0:0 -> ../../sdb
lrwxrwxrwx 1 root root   9 Jun 16 00:13 wwn-0x5000cca8e6ddcf2b -> ../../sdb
```

ata是stat通道的硬盘，nvme是nvme硬盘。

## 直通硬盘

```shell
qm set ${VM_ID} --scsiX /dev/disk/by-id/xxxx
```

- VM_ID是要直通的虚拟机的ID，一般是100往后的序号。
- scsiX 是选择直通为的磁盘类型，一般使用scsi即可，X是序号，从0开始递增，如果虚拟机有创建的虚拟磁盘，则一般scsi0已被使用

