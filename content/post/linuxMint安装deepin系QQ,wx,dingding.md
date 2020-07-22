---
title: "linuxMint安装deepin系QQ,wx,dingding"
date: 2020-07-22T22:31:42+08:00
description: ""
tags: [ "linuxMint","deepin" ]
categories: [ "linux" ]
weight: 40
draft: false
---

# 安装框架deepin-wine
```sh
git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git
cd deepin-wine-for-ubuntu/
./install.sh #会安装一大堆i386的支持包，及deepin-wine环境
# 安装托盘扩展，安装过程中出现lightDM提示直接OK就好
sudo apt install gnome-shell-extension-top-icons-plus gnome-tweaks
# 重启生效，测试注销完重进不生效的，原因不知
```

# 安装QQ轻聊版
```sh
wget  https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im.light/deepin.com.qq.im.light_7.9.14308deepin8_i386.deb
sudo dpkg -i deepin.com.qq.im.light_7.9.14308deepin8_i386.deb
```
- 装完如果应用菜单里没有的话，执行一下，应该是因为各发行版的应用菜单的目录不一至吧(我以为装失败了)
```sh
sudo cp /usr/local/share/applications/deepin.com.qq.im.light.desktop /usr/share/applications/
```

# 安装wx
```
wget https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/deepin.com.wechat_2.6.2.31deepin0_i386.deb
sudo dpkg -i deepin.com.wechat_2.6.2.31deepin0_i386.deb
```
安装完测试没问题

# 安装dingding(这个版本好像和deepin-Wine没啥关系)
```
sudo apt install libappindicator1 libdbusmenu-gtk4
wget https://mirrors.aliyun.com/deepin/pool/non-free/d/dingtalk/dingtalk_2.0.13-145_amd64.deb
sudo dpkg -i dingtalk_2.0.13-145_amd64.deb 
```
测试安装完会出现版本提示，介意的话可以安装github的最新版本,github下载速度稍慢
```
https://github.com/nashaofu/dingtalk/releases/download/v2.1.5/dingtalk-2.1.5-latest-amd64.deb
```

# foxmail等
```
wget https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.foxmail/deepin.com.foxmail_7.2deepin3_i386.deb
```
更多软件可以在 https://mirrors.aliyun.com/deepin/pool/non-free/d/ 找到

# 遗留问题
- wx无法发送图片，听说安装这个包：libjpeg62:i386可以解决，但这个包和向日葵冲突，工作更依赖向日葵，wx只用来吹水，还是算了

# 感谢
- https://github.com/wszqkzqk/deepin-wine-ubuntu
- https://linux.zone/1319
