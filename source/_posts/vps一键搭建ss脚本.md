---
title: vps一键搭建ss脚本
date: 2019-07-27 00:28:37
tags: ['vps', 'ss']
---
### 安装脚本
```
wget --no-check-certificate http://down.whsir.com/downloads/shadowsocks-go.sh
chmod +x shadowsocks-go.sh
./shadowsocks-go.sh 2>&1 | tee shadowsocks-go.log
```
<!-- more -->
### 卸载方法
```
./shadowsocks-go.sh uninstall
```
### 用户密码配置
```
vi /etc/shadowsocks/config.json

{
    "server":"0.0.0.0",  //你的主机IP
    "server_port":xxxx,  //端口
    "local_port":1080,
    "password":"xxxx",
    "method":"aes-256-cfb",
    "timeout":600
}
```

### 启动
```
/etc/init.d/shadowsocks restart
```
### 常用命令
```
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
```
