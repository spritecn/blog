---
title: ""
date: 2020-0８-１６T01:31:42+08:00
description: ""
tags: [ "java","算法" ]
categories: [ "java" ]
weight: 40
draft: false
---

### 直接上代码
```java
package main;


public class Gcd {
    //穷举法
    //复杂度　O(b)即O(N)
    public static  int loopGcd(int a,int b){
        for (int c = a>b?b:a;c >0;c--){
            if(a%c ==0 && b%c ==0){
                return c;
            }
            println(c);
        }
        return 1;
    }

    //更相减损求大公约数
    //以少减多,更相减损
    //复杂度　O(N),极端情况下和上面一致，比如b=1
    public static int minusGcd(int a,int b){
        int tmp;
        while (a%b!=0){
            tmp = b;
            b = a-b;
            if(b>0){
                a = tmp;
            }else {
                println(b);
                b = -b;
            }
        }
        return b;
    }


    //取余优化:取一次余然后走gcd
    //复杂度　O(N/2)
    public static  int modMinusGcd(int a,int b){
        if(a <1 || b< 1){
            return 0;
        }
        int tmp;
        if(a<b){
            b = b%a;
            if(b == 0){
                return a;
            }
            return minusGcd(a,b);
        }else {
            tmp = b;
            b = a%b;
            if(b == 0){
                return tmp;
            }
            return minusGcd(tmp,b);
        }
    }

    //使用辗转相除法
    //复杂度　O(lgN),可以用二分法理解
    public static  int modGcd2(int a,int b){
        println("a:"+ Integer.valueOf(a).toString() +" b:" + Integer.valueOf(b).toString());
        if(b == 0){
            return a;
        }
        if(a<b) {
           return modGcd2(a,b%a);
        }else {
            return modGcd2(b, a % b);
        }
    }
    //~~仅在 a>b的情况下辗转相除法简化~~
    //由于　3%5 余5 所以 a<b的情况下也适用只是多第一次取余
    //复杂度　O(lgN)
    public static  int modStordIntGcd(int a, int b){
        println("a:"+ Integer.valueOf(a).toString() +" b:" + Integer.valueOf(b).toString());
        return b==0?a:modStordIntGcd(b,a%b);
    }

    public static void main(String[] args){
        println(loopGcd(34,119));
    }
}

```
