---
title: "linux Mint安装redis-desktop-manager和snapd"
date: 2020-08-07T12:31:42+08:00
description: ""
tags: [ "Mint","blog" ]
categories: [ "Linux" ]
weight: 40
draft: true
---

- redis-desktop-manager新版本收费了，但是发现snap商店版本不收费
- 发现Linux Mint20.04无法安装Snapd,出现如下提示
  ```text
    正在读取软件包列表... 完成
    正在分析软件包的依赖关系树       
    正在读取状态信息... 完成       
    没有可用的软件包 snapd，但是它被其它的软件包引用了。
    这可能意味着这个缺失的软件包可能已被废弃
  ```
- 百度无解/bing国际发现ubuntu官方有解，原因是LinuxMint屏蔽了
- 执行下面命令解禁,然后即可安装
  ```sh
  sudo rm /etc/apt/preferences.d/nosnap.pref
  sudo apt update
  sudo apt install snapd  
  ```
- 然后安装 redis-desktop-manager
  ```sh
  sudo snap install redis-desktop-manager
  ```
- 参考：
  - https://snapcraft.io/install/snap-store/mint
  - https://snapcraft.io/redis-desktop-manager
  - 


