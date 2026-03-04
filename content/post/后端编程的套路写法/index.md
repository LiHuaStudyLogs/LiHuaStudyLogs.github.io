+++
date = '2026-03-01T19:26:38+08:00'
weight = -51
draft = true
title = '后端编程的套路写法'
description = '由于后端的语法限制，一些情况下我们就有特定的应对方法。我们把这些情况收集起来'
+++

**使用Scanner类时判断输入流的值是否符合要求的模板**    
* **相关特性：**   
    堵塞原理 ：只要执行到Scanner类的方法,就会**先暂停程序的执行**，
    等待数据的到位后才能继续运行程序，

    **读取型方法**   
    `对象名.next()` --> 读取输入流中的数据<String>并把数据拿走，作为返回值<String>   
    `对象名.nextInt()` --> 读取的类型可规定，类型不符合会直接异常
    所以可以用 **判断型方法** 实现异常的处理

    **判断型方法**  
    `对象.hasNextInt()` --> 仅读取的类型，判断后返回<boolean>，并不会消费数据，
    如果不用 **读取方法** 消耗，输入流的数据会一直残余到本次程序运行结束。

* **应对框架：**  
```java
        Scanner sc = new Scanner(System.in);
        while (true){
// 死循环，保证获得满意的答案时结束
            System.out.print(tips);

            if (sc.hasNextInt()){
                int key = sc.nextInt();
        // 是数字类型，消耗掉，确保下一次循环时sc.hasNextInt判断的是新填写的数值
                if (key >= min && key <= max){
                    return key;
                }else {
                    System.out.println("输入的数字不在要求范围内！");
                }
            }else {
                String key = sc.next();
        // 不是数字类型，是字符，消耗掉
                System.out.println("输入的数字非法！");
            }
        }
```

***

**求取指定范围的随机数** 

* **相关的类**   
    Math/Random

```java
        public static void main(String[] args) {
//        要1~100之间的随机数
        int random_num = (int) (Math.random()*100+1);
        System.out.println(random_num);
    // Math.random()等价rd.nextDouble() ,但是Math.random()不用创建对象，我更爱用
    }
```    

* 想要 a~b   
    就先假定 [a,b]- a   
    此时，就找到了重要节点[0,b]   
    随机数方法构建出[0,b+1)   
    再`[0,b+1)+a`运算即可


***

**创建一个类的套路**  
起手创建一个类，就要按照下面这个**模板**思考： 
```
    class XxxYyy{
        1.【成员属性】
        2.【构造器】
        3.【成员方法】  
        4.【get/set】方法 
    }
```    

* **这些结构不用自己写！**  
    `insert(fn + delete触法) + alt`/`右键 + Generate`-->快速构建【类】的基本结构   

* 私有封装设计：  
【成员变量】全部`private`,   
统一用【getter/setter】方法，来设计可以被外界访问或改变的【成员变量】