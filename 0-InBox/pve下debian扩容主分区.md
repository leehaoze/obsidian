---
date created: 星期一, 六月 17日 2024, 12:42:56 下午
date modified: 星期一, 六月 17日 2024, 2:09:48 下午
tags: 
---

# Pve下debian扩容主分区

通过PVE创建Debian虚拟机后担心后续会遇到磁盘空间不够用的情况，因此先行演练一把，即便出了问题也不会丢失数据。

## Pve界面调整磁盘大小

点击`硬件-硬盘`，点击菜单来磁盘操作，选择调整大小操作即可。

## Debian内操作

### 禁用SWAP分区

```shell
# 查看swap分区位置
swapon --show 

> root@main:~# swapon --show
> NAME      TYPE      SIZE USED PRIO
> /dev/sda5 partition 975M   0B   -2

swapoff /dev/sda5
```

## 删除现有swap分区

```shell
fdisk /dev/sda
```

- 输入 `d` 选择删除分区
- 输入分区号`5`
- 输入`w`保存

## 扩展主分区，重新创建swap

```shell
fdisk /dev/sda
```

- 输入`d`删除分区
- 输入`n`创建新分区
- 输入`p`选择主分区
- 输入分区号`1`
- 设置开始扇区为默认值
- 设置结束扇区为，开始扇区 + 新空间大小对应的簇（一簇默认512字节）
- 输入`n`创建新分区
- 输入`e`选择拓展分区，其余选默认值
- 输入`t`修改分区类型，输入`82`修改为swap类型
- 输入`w`保存结束

## 重新读取分区表

```shell
partprobe /dev/sda
```

## 调整文件系统大小

```shell
resize2fs /dev/sda1
```

## 格式化swap分区，启用swap

```shell
mkswap /dev/sda2
swapon /dev/sda2
```

## 更新fstab文件

```shell
/dev/sda2 none swap sw 0 0
```