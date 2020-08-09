---
title: "MySql_tkMapper_spring优化记录"
date: 2020-08-04T14:31:42+08:00
description: ""
tags: [ "mysql" ]
categories: [ "java" ]
weight: 40
draft: false
---


# mysql
## 建表

## sql优化


# spring事务相关
- 事务注解
- 
- 锁类型注解


# tkMapper 
- 只需要部分列，可以用 ex.selectProperties("id","uuid")
- limit 可以直接写在 setOrderByClause 里
  ```java
  Example ex = new Example(BusinessnumBind.class);
  ex.setOrderByClause("createdTime desc limit 1");
  ```

