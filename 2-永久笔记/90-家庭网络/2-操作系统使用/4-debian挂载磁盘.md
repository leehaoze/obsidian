---
date created: 星期五, 六月 21日 2024, 9:07:38 晚上
date modified: 星期五, 六月 21日 2024, 9:23:51 晚上
tags: 
---

# 4-debian挂载磁盘

## 查看磁盘类型

挂载磁盘时，需要指定磁盘类型， [[3-查看磁盘文件系统类型]]后方可进行挂载。

## 挂载

- 创建挂载点

挂载点就是磁盘挂载到哪个文件夹下，我们创建一个文件夹即可。

```shell
cd ~
mkdir media
```

- 进行挂载

```shell
mount -t {DISK_TYPE} /dev/{DISK} {MOUNT_PATH}
```

- 添加自动挂载
向`/etc/fstab`中写入该挂载信息，实现系统开机时自动挂载对应磁盘到对应目录。

```shell
/dev/sdb1  /root/media ext4 defaults 0 0
```