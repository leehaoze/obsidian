---
date created: 星期三, 六月 19日 2024, 7:55:16 晚上
date modified: 星期六, 六月 22日 2024, 11:27:40 上午
tags: 
---

# Dae手动部署安装

## 准备环境

个人比较将安装的第三方软件的binary放到一起，所以都是选择手动部署安装，这里准备一个`dae`的目录

```shell
# 如果是非root用户，记得用sudo才能执行安装哦
apt install -y curl unzip
cd ~
mkdir -p env/dae
```

## 安装dae

这里主要参考`dae`自己的[文档](https://github.com/daeuniverse/dae/blob/main/docs/en/user-guide/run-as-daemon.md)

### 下载 Geo Data

```shell
cd ~/env/dae
## 创建资源目录，存放geo data
mkdir asset
pushd asset
curl -L -o geoip.dat https://github.com/v2fly/geoip/releases/latest/download/geoip.dat
curl -L -o geosite.dat https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat
popd
```

### 下载默认配置文件

这里下载一个默认配置文件，基于此文件进行修改

```shell
cd ~/env/dae
curl -L -o config.dae https://github.com/daeuniverse/dae/raw/main/example.dae

# dae 要求此文件权限不能太过开放
chmod 0640 config.dae
```

### 下载binary

从[此处](https://github.com/daeuniverse/dae/releases/tag/v0.6.0)下载最新的binary

```shell
wget https://github.com/daeuniverse/dae/releases/download/v0.6.0/dae-linux-x86_64.zip

unzip dae-linux-x86_64.zip
# 删除不需要的内容
rm -rf dae-linux-x86_64.zip geoip.data geosite.data example.dae
# 给binary改个名字
mv dae-linux-x86_64 dae
# 授予执行权限
chmod +x dae
```

### 安装为系统服务

解压`dae-linux-x86_64.zip`时会得到一个`dae.service`文件，由于我们是自己定义的文件位置，因此我们需要修改一下该文件。

```shell
[Unit]
Description=dae Service
Documentation=https://github.com/daeuniverse/dae
After=network-online.target docker.service systemd-sysctl.service
Wants=network-online.target

[Service]
Type=notify
User=root
LimitNPROC=512
LimitNOFILE=1048576
# 这里改为自己定义的asset地址
Environment="DAE_LOCATION_ASSET=/root/env/dae/asset/"
# 这里改为自己的路径地址
ExecStartPre=/root/env/dae/dae validate -c /root/env/dae/config.dae
ExecStart=/root/env/dae/dae run --disable-timestamp -c /root/env/dae/config.dae --logfile /root/env/dae/dae.log
ExecReload=/root/env/dae/dae reload $MAINPID
Restart=on-abnormal
TimeoutStartSec=120

[Install]
WantedBy=multi-user.target
```

修改后将文件放入到系统的systemd目录

```shell
mv dae.service /etc/systemd/system/

# reload 
systemctl daemon-reload
systemctl enable dae --now
```

### 定时更新geo Data

编辑crontab

```shell
crontab -e

```

在新打开的文件中添加以下内容，实现每天凌晨3点更新文件：

```
0 3 * * * /usr/bin/curl -L -o /root/env/dae/asset/geoip.dat https://github.com/v2fly/geoip/releases/latest/download/geoip.dat
0 3 * * * /usr/bin/curl -L -o /root/env/dae/asset/geosite.dat https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat
```