---
title: "linuxMint安装 IDEA2019.03.5"
date: 2020-07-25T21:31:42+08:00
description: "linuxMint安装 IDEA 19"
tags: [ "idea","linuxMint" ]
categories: [ "java" ]
weight: 40
draft: false
---

# 安装idea
```sh
axel https://download.jetbrains.com/idea/ideaIU-2019.3.5.tar.gz
# 速度还蛮快的
tar -zxvf ideaIU-2019.3.5.tar.gz
```

# 调整
```sh
cp jetbrains-agent.jar  idea-IU-193.7288.26/bin/
```
把jetbrains-agent.jar包里META-INF下的import.txt也要放在jetbrains-agent.jar相同位置
然后打开 /idea-IU-193.7288.26/bin/idea64.vmoptions
添加下面内容
```sh
-javaagent:/home/xxxxxx/idea-IU-193.7288.26/bin/jetbrains-agent.jar
```
link:https://www.jiweichengzhu.com/article/2940ed65c94f4671ae3f3aa72e168673

# 图标
新建一个:IDEA.desktop文件,丢到/usr/share/applications/下就好 
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Name=IDEA
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/home/xxxxxx/soft/idea-IU-193.7288.26/bin/idea.sh %F
Icon=/home/xxxxxx/soft/idea-IU-193.7288.26/bin/idea.svg
Type=Application
StartupNotify=false
StartupWMClass=Code
Categories=Utility;TextEditor;Development;IDE;
MimeType=text/plain;inode/directory;
Actions=new-empty-window;
Keywords=;

X-Desktop-File-Install-Version=0.24
```



