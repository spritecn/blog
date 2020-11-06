---
title: "测试框架junit&spock"
date: 2020-11-05T01:31:42+08:00
description: ""
tags: [ "junit","java" ]
categories: [ "shitLife" ]
weight: 40
draft: false
---

# 测试框架junit&spock
主要是springboot下测试用
## maven引入
``` xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.spockframework</groupId>
        <artifactId>spock-spring</artifactId>
        <!-- 版本不建议写，由springboot指定，不然会报莫名的错
        <version>1.0-groovy-2.4</version>
         -->
        <scope>test</scope>
    </dependency>
```
*下面这些不需要写，写上可能会有问题*
``` xml
<dependency>
        <groupId>org.spockframework</groupId>
        <artifactId>spock-core</artifactId>
        <version>1.3-groovy-2.5</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.codehaus.groovy</groupId>
        <artifactId>groovy-all</artifactId>
        <version>2.5.13</version>
        <scope>test</scope>
</dependency>
```

## 基础测试类
- 注意这是一个groovy类，扩展名是groovy
- 只需要集成Specification
- springboot集成基础测试类，其他类可继承这个类
``` groovy
@WebAppConfiguration
@ContextConfiguration(classes = Application.class)
@SpringBootTest
class BaseTestSpock extends Specification{
}
```
## 注意事项
- **不要用@RunWith(SpringRunner.class)**,因为会覆盖spock的runwith导致下面报错，且大部分功能无法正常使用
``` java
 java.lang.Exception: Method $spock_feature_1_0 should have no parameters
 ```
## 编写测试类
-  spock定义了8个语义块
   -  given,setup:初始消息创建(setup is an alias of thg given);
   -  when:触发测试动作
   -  then:验证测试结果
   -  expect:简单版本的then
   -  where:参数流测试
   -  cleanup:类似finally,清理资源
   -  and:续上一个语句块，（出现在given后面就是given块，出现在then后面就是then块，个人理解只是为了分割逻辑让代码清淅存在的)
- given,when,then 测试流
  - given:给写测试数据
  - when:执行测试
  - then:校验结果
- 



