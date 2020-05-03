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
1. 用hugo生成博客，替换之前typero的动态博客
2. 实现git push到github后自动更新博客文章
3. 将之前博客的内容转过来  （暂缓,后面看心情)

# 过程
## hugo搭建
1. 在github上建一个项目
2. 在hugo官网下载二进制包
3. ``` bash
    hugo new site blog #blog 为你的博客目录
    ``` 
    生成博客，将并二进制包放到生成好的blog目录下
4. 在模板列表里 https://github.com/spf13/hugoThemes 找个自己觉得过得去的模板放到themes目录下
5. 配置模板，模板的README都有写怎么配，并且每个模板下都有一个example示例目录
6. 在post目录下写篇文章，比如你现在看到的，就是我写的第一篇，写文章的时候要注意头部内容，要弄成这个格式，不然解析不了
 ``` toml
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


## 自动部署



