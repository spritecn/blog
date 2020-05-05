---
title: "hugo博客搭建"
date: 2020-05-02T23:31:42+08:00
description: ""
tags: [ "hugo","blog" ]
categories: [ "shitLife" ]
weight: 40
draft: false
---

# 目标
1. 用hugo生成博客，放在腾讯Cos上，替换之前typero的动态博客
2. 实现git push到github后自动更新博客文章
3. 将之前博客的内容转过来  （暂缓,后面看心情)

# 过程
## hugo搭建
1. 在github上建一个项目
2. 在hugo官网下载二进制包
3. 用hugo 创建一个站点，然后把二进制包(hugo.exe)放到生成好的blog目录下
    ``` bash
    hugo new site blog #blog 为你的博客目录
    ``` 
4. 在模板列表里 https://github.com/spf13/hugoThemes 找个自己觉得过得去的模板放到themes目录下
5. 配置模板，模板的README都有写怎么配，并且每个模板下都有一个example示例目录
6. 在post目录下写篇文章，比如你现在看到的，就是我写的第一篇，写文章的时候要注意头部内容，要弄成这个格式，不然解析不了
    ``` markdown
    ---
    title: "hugo博客搭建"
    date: 2020-05-02T23:31:42+08:00
    description: "描述"
    keywords: [ "Hugo", "keyword" ] #可用于seo
    tags: [ "tag1","tag2" ]
    categories: [ 分类1"，"分类2" ]
    weight: 40  #排序比重
    draft: false #是否为草稿
    ---
    ```
7. cd到blog目录下 运行 hugo server 然后打开 http://localhost:1313 就可以看到效果了
8. 在腾讯云开一个Cos开启网站服务

## 自动部署
1. 部署逻辑:
    - 推送文章到gitHub
    - 触发webhooks,通知vps服务器
    - vps服务器接收并验证webhooks信息，然后pull更新的文件
    - 执行hugo程序生成静态网站目录
    - zip静态网站目录，然后上传到腾讯Cos(对象云存储)
    - Cos自带的zip云函数服务解压zip包到根目录下（自动生成的那个云函数默认会解压到zip包同名目录下，需要小改一下task.js)
2. 用groovy + sparkJava + 命令操作，在vps上开一个web服务器开实现上面2-5步,见：https://github.com/spritecn/hugoBlogAutoDeploy2Cos.git
3. 第六步更改zip自带云函数解决服务的地方为的unzipTask.js 106行,就是去掉了basenmae
    ``` javascript
        const Key = path.join(targetPrefix, task.entry.fileNameStr).replace(/\\/g, '\/')
    ```

# 完工
- 注：
    1. 腾讯cos当静态服务器用要绑已备案域名，不然不好用
    2. 不想用腾讯可以用阿里oss啥的，对接差不多，没有自动解压功能，可以直接一个一个上传
    3. 想不起来了