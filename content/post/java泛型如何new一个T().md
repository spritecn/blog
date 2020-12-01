---
title: "java泛型如何new一个T()"
date: 2020-11-30T21:31:42+08:00
description: ""
tags: [ "java","blog" ]
categories: [ "java" ]
weight: 40
draft: false
---

# java泛型如何new一个T()

## 问题
```java
class TestClass<T>{
    T get(){
        T t = new T();
        return t;
    }
}
```
想当然的写，然后就报错了，不得不说，java的泛型支持，着实...
## 分析
- t = new T();idea 提示 Cannot resolve symbol 'T'，泛型的这个T，在运行时是会被擦掉的，java不知道new一个怎么样的class出来
- 百度t = new T(),第一个是 https://blog.csdn.net/qq_35584878/article/details/101275076
    我帖一下这个网站的代码
    ```java
    ParameterizedType pt = (ParameterizedType) this.getClass().getGenericSuperclass();
    Class<T> clazz = (Class<T>) pt.getActualTypeArguments()[0];
    // 通过反射创建model的实例
    T t = clazz.newInstance();
    ```
    用上面代码替换T = new T()这个测试是报错的，clazz是null不能给null创建一个实例，这此没头没尾的文章很是误导读者，getGenericSuperclass是返回父类的泛型类，参考[https://www.cnblogs.com/maokun/p/6773203.html]，仅在父类对象T为非泛型时可以正常返回父类泛型参数的真实对象
## 解决
- 给get方法 传入class,让jvm知道new一个什么对象
    ```java
    T get(Class<T> clazz){
        return clazz.newInstance()
    }
    ```
- 构造函数传入class，同理，但这样写相对好看一点，且new 的时候可以不用写泛型类标记，可以直接new TestClass(String)
    ```java
    class TestClass<T>{
        Class<T> t;
        private ConnectDao(Class<T> t){} //封闭默认构造函数
        ConnectDao(Class<T> t) {
            this.t = t
        }

        T get(){
            return t.newInstance;
        }
    }
    ```
- java8 提供了一个工厂类接口Supplier<T>
  ```java
  class TestClass<T>  implements Supplier<T>
  ```
  参考<<on java 8>>:[https://lingcoder.github.io/OnJava8/#/book/20-Generics?id=%e8%a1%a5%e5%81%bf%e6%93%a6%e9%99%a4]

## by a way
- <<on java 8>> 还是有蛮多干货，有必要细看一下
