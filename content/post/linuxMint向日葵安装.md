---
title: "linuxMint向日葵安装"
date: 2020-07-22T00:31:42+08:00
description: "自考高数笔记"
tags: [ "linux","远程控制" ]
categories: [ "linux" ]
weight: 40
draft: false
---

# linuxMint向日葵安装记
- 官方下载地址 https://sunlogin.oray.com/download/
- 直接安装会报少 libwebkitgtk 3.0-0 依赖
- 依赖安装(抄自知乎，但知乎写的有点问题,libicu版本下载不到改了一下版本号)
```sh
sudo apt install libegl1-mesa enchant
wget -c http://ftp.br.debian.org/debian/pool/main/w/webkitgtk/libwebkitgtk-3.0-0_2.4.11-3_amd64.deb
wget -c http://ftp.br.debian.org/debian/pool/main/i/icu/libicu57_57.1-6+deb9u4_amd64.deb
wget -c http://ftp.br.debian.org/debian/pool/main/w/webkitgtk/libjavascriptcoregtk-3.0-0_2.4.11-3_amd64.deb
wget -c http://ftp.br.debian.org/debian/pool/main/libj/libjpeg-turbo/libjpeg62-turbo_1.5.1-2_amd64.deb
wget -c http://download.oray.com/sunlogin/linux/sunloginclient-10.0.1.24347_amd64.deb
sudo dpkg -i libjavascriptcoregtk-3.0-0_2.4.11-3_amd64.deb
sudo dpkg -i libicu57_57.1-6+deb9u4_amd64.deb
sudo dpkg -i libjpeg62-turbo_1.5.1-2_amd64.deb
sudo dpkg -i libwebkitgtk-3.0-0_2.4.11-3_amd64.deb
sudo dpkg -i sunloginclient-10.0.1.24347_amd64.deb
```
- 安装完会提示，安装成功但配置失败，unknow Os it not impl
- 这时候需要改一下 
```sh
#/usr/local/sunlogin/scripts/common.sh,从后往前，大概在56行，改为
elif grep -Eqi "Mint" /etc/issue || grep -Eqi "Ubuntu" /etc/issue || grep -Eq "Ubuntu" /etc/*-release; then
```
```sh
#/usr/local/sunlogin/scripts/start.sh,64-74行，改为,删掉了几行多余的
if [ $os_name == 'ubuntu' ]; then
	if [ $isinstalled == true ]; then
			isinstalledubuntu_hv
			systemctl start runsunloginclient.service
	fi
elif [ $os_name == 'deepin' ]; then
```
- 然后重新配置
```
sudo dpkg-reconfigure sunloginclient 
```

## 现在可以打开客户端了，结束 
