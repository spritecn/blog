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
  -  @Unroll 每一个where输入都做为一个单独的测试执行
  -  @AutoCleanup("close") 自动执行清理方法,默认是close()方法
  -  @Ignore 忽略此测试和Junit的 Ignore 一致
  -  @IgnoreIf()/@Requires 如果满足条件就忽略/导入,需传一个predicate Closure {} (***谓词闭包?***)
  -  @Timeout(int) 方法超时时间
  -  @Stepwise 严格按照方法编写顺序测试，如果前面方法失败，将跳过后面方法，方法间有依赖关系的话助于避免方法失败后的连续错误
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
### MOCK/Stub/Spy
- MOCK/Stub/Spy 这部分单独写一个文档

## 测试方法编写
- 测试类需要继承Specification类
- given,when,then 测试流，when和then需要搭配使用,
  - given:给写测试数据
  - when:执行待测试的函数
  - then:校验结果是否符合预期
   ```groovy
   class MathTest extends Specification{
      def "math.max test"(){
         given:
            def a= 3
            def b= 5
         when:
            def c = Math.max(a, b)
         then:
            c == 5
   }
  }
   ```
- given,expect,where 测试流，适用于多个条件测试
  - expect 是简写的(when then),通过boolen类断言来校验结果
  - where 语义块必须放在expect后面，不能放在(when,then)后面
  - 如果觉得expect不分执行测试和校验结果，可以用and将执行的校验分开
   ```groovy
      @Unroll
      def "maximum of #a and #b should be #c"() {
         expect:
            resutl = Math.max(a, b)
         and:
            result == c
         where:
            a | b || c
            3 | 5 || 5
            7 | 0 || 7
            0 | 0 || 0
         }
   ```


## springboot集成

### maven引入
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
### 基础类
- springboot集成测试基础类，实际写的时候继承这个类
   ``` java
   @WebAppConfiguration
   @ContextConfiguration(classes = Application.class)
   @SpringBootTest
   @Slf4j
   class BaseTestSpock extends Specification{}
   ```
- 注意：**不要在基础接口里写@RunWith(SpringRunner.class)**,因为会覆盖spock的runwith导致spock无法正常运行
- 如果是单独测一部分功能，比如仅测一个service,可以在启动类里，仅扫描dao层包，然后@import(Serivce.class),然的就可以autowaried这个类了
如仅启动dao层的启动类
   ```java
   @SpringBootApplication(scanBasePackages = {"com.*.*.dao.*"})
   //去除swagger
   @EnableAutoConfiguration(exclude = SwaggerAutoConfiguration.class)
   @MapperScan("com.*.*billing*.dao.mapper")
   public class ApplicationTest{
      public static void main(String[] args) {
         SpringApplication.run(ApplicationTest.class, args);
      }
   }
   ```
- 测试单个service
   ```groovy
   @SpringBootTest
   @ContextConfiguration(classes = ApplicationTest.class)
   //导入单个service
   @Import(MealServiceImpl.class)
   class MealDaoTest extends Specification {
      @Autowired
      MealServiceImpl mealService
      def "test listMeal"() {
         given:
            ListMealDTO listMealDTO = new ListMealDTO()
            listMealDTO.setUuid("3423fsfrsfsd")
         when:
            def resultList = mealService.listMeal(listMealDTO)
         then:
            resultList.isEmpty()
      }
   }
```


## tips
- idea设置groovy的Label indent缩进改为4(默认0),看起来更舒服
- groovy3.0 以下版本不支持 java8的 lambda语法，需要转为Closure 方法,不然后报错
   下面java
   ```java
   list.stream().filter(x->!x.getDisabled()).count() > 1;
   ```
   需要转为
   ```groovy
   list.stream().filter({x->!x.getDisabled()}).count() > 1
   ```



