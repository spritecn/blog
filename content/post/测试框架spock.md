---
title: "测试框架spock"
date: 2020-11-05T01:31:42+08:00
description: ""
tags: [ "junit","java" ]
categories: [ "shitLife" ]
weight: 40
draft: false
---

# 测试框架spock
主要是springboot下测试用
## maven引入
``` xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spock-spring</artifactId>
        <!-- 版本不建议写，由springboot指定，不然会报莫名的错
        <version>1.0-groovy-2.4</version>
         -->
        <scope>test</scope>
    </dependency>
```
*下面这些不需要写，写上可能因为版本问题报错*
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
## spock相关定义：
### 语义块
-  given,setup:初始消息创建(setup is an alias of thg given);
-  when:触发测试动作
-  then:验证测试结果
-  expect:简单版本的then
-  where:参数流测试
-  cleanup:类似finally,清理资源
-  and:续上一个语句块，（出现在given后面就是given块，出现在then后面就是then块，个人理解只是为了分割逻辑让代码清淅存在的)
### 方法：
-  setup(),在每一个测试方法执行前执行
-  setupSpec()，在测试方法执行前执行，只执行一次
-  cleanup(), 在每一个测试方法执行后执行
-  cleanupSpec(),所有测试方法执行结束后执行
-  语法糖方法：
   -  old(),取when或expect之前的数据，主要用于测试前后对比
   -  expect()，用于标记验证，无实际效果，主要为了可读性，写在then块内
   -  that(),同上，写在and块或expct块里
### 注解：
-  注释类注解
   -  @Subject(Class) 用于标记正在测试的类，也可以直接写在类初始化语句上，仅影响可读性
   -  @Narrative("""balabalabala""") Longer description of test
   -  @Title("test")，标题
   -  @Issue(),用来标解决哪个问题
   -  @See(),同上，用来写连接
   -  **惊，居然写了这么多用来解释这个测试是干啥的注解**
- 其他注解
  -  @Shared 标记一个对象是共享的，只初始化一次
  -  @Unroll 第一个where输入做为一个单独的测试执行
### 断言
- spock 在then和expect中,默认断言所有返回为boolean型的语句
- 如果需要在其他语句块里断言,可以显示的 assert
- 异常断言可以用thrown
- 验证没有抛出某种异常可以用notThown
   ``` groovy
   given:
      def stack = new Stack()
      assert stack.empty
   when:
      stack.pop()
   then:
      null != stack
      def e = thrown(EmptyStackException)
      e.printStack()
      notThown(NullPointerException)
   ```

## 测试方法编写
- 需要继承Specification类
- 
- given,when,then 测试流，when和then需要搭配使用,
  - given:给写测试数据
  - when:执行待测试的函数
  - then:校验结果是否符合预期
- given,expect,where 测试流，适用于多个条件测试
  - expect 是简写的(when then),通过boolen类断言来校验结果
  - where 语义块必须放在expect后面，不能放在(when,then)后面
   ``` groovy
      @Unroll
      def "maximum of #a and #b should be #c"() {
         expect:
            Math.max(a, b) == c
   
         where:
            a | b || c
            3 | 5 || 5
            7 | 0 || 7
            0 | 0 || 0
      }
   ```

### springboot集成
- springboot集成测试基础接口，这样实际写测试类的时候，直接实现这个类就好
``` java
@WebAppConfiguration
@ContextConfiguration(classes = Application.class)
@SpringBootTest
interface BaseTestSpock {
}
```
  - 注意：**不要在基础接口里写@RunWith(SpringRunner.class)**,因为会覆盖spock的runwith导致下面报错，且大部分功能无法正常使用



## tips
- idea设置groovy的Label indent缩进改为4(默认0),看起来更舒服



