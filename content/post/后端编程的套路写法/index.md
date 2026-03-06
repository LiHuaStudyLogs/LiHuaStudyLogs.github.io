+++
date = '2026-03-01T19:26:38+08:00'
weight = -51
draft = false
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

***

**解耦合思想**
* 方法的创建中：  
```java
    public static void xxxYyy(父类 a) {
// 形参用【父类】，【子类】的对象都能作为 实参 
    }
```  
* 对象的创建中：   
```java
    父类 xxxYyy = new 子类（ ） ；  
```
这样创建对象的方式，你只需要**改动右半边的子类类型**就能创建新的对象。  
但是，这样的创建方式有个弊端：  
你的目的是创建一个子类的实例化对象，但此时这个创建方式会导致你的对象只能访问**从【父类】那里继承的成员**。子类自己的成员不能被访问  
处理方式如下：  
```java
    子类 aaaBbb= (子类) xxxYyy；
    // 类型的强制转换，这样aaaBbb就能跟正常创建的对象一样，使用子类自己的成员   
```
但是在这个时候，如果创建的是【子类1】的对象，在强制转换时却是用的【子类2】，是会报错的   
**但是，这个报错并不会在编译时显示，**   
所以当你的方法传入一个实参时，他又是解耦合形式创建的，你可能会不小心传入**不是你想强制转换的子类**的实参，所以你要加个保险：
```java
    public void 方法(父类 a){
        if(a instanceof XxxYyy){
            XxxYyy b = (XxxYyy) a；
        }
    }    
```

***

**CURD业务**
* 我们掌握一个**容器**相关的知识点，主要遵循CURD学习思路   
    解决问题，创建功能也是围绕这四个点考虑：
1. 增
2. 删
3. 改
4. 查（获取）   


***

**想让用户按照我们的要求输入内容**

* **当我们的用户输入错误需要重新输入：**
我们给他无限次机会，直到他输入正确为止，整个程序在用户输对之前一直卡住他   
输入正确就往下执行 
```java
    whlie(true){
        // 死循环
        if(正确要求){
            break ;
        }
    }
```    

*  **当用户输入的内容会与我们程序产生冲突，例如重名**
这个是程序上的错误，我们必须终止他的进一步操作,于是我们使用**自定义异常**   
因为**异常**最后肯定会被**捕获**，并在【catch】中告诉你错误的具体信息，不会再让你输入了
```java
    public class LiHuaException extends RuntimeException{
        // 自定义的异常类必须继承自Exception
        public LiHuaException(String Message){
            //    创建构造器,有参数【Message】，就是报错信息
            super(Message);
        }
}
```

**异常类**创建好之后，你还要设计**某种情况**会被认定是这个异常，也就是**什么时候会异常**   
必须在程序中用【if】框架实现一次【throw 自定义异常的对象】

```java         
    public class XxxYyy{
        public void func_00 (){
            try{
                func_01();
            }catch(LiHuaException e){
                e.printStakeTrace() ;
                throw e ;
            }    
        }

        public void func_01 ( ) throws LiHuaException{   
            int xxxYyy;
            if(xxxYyy > 10){
                throw new LiHuaException("xxxYyy不应该大于10 ！");
        }
    }
```

***

**对容器的遍历**

**容器的遍历方法**   
1. **迭代器：**   
    【集合】专用
```java
    ArrayList<Integer> al = new ArrayList<>() ;
    // 创建集合对象

    Iterator<Integer> it = al.iterator();
    //  `集合.iterator().var` -> 简写创建迭代器对象
    // al.iterator()这个成员方法，是所有集合类都有的
    // 【迭代器】和【集合】的【泛型】必须一致

    while(it.hasNext()){
        Integer next = it.next();
        // 获取每一个元素
    }    

> 迭代器的常用方法
        it.hasNext() -> 判断当前指针后移后有没有数据
        it.next() -> 指针后移，并获取指针的数据
        it.remove() -> 删除当前指针指向的元素
    // 【迭代器】是一次性遍历工具，指针位置调整之后就没办法复原了 
```    

2. **索引遍历法：**  
    根据【逻辑索引】遍历，【有序集合】【数组】都能通过这个方法遍历

3. **增强for循环**<sup>也称for-each</sup>:    
    这是一种语法糖   
    遍历数组 -> 就是使用【索引遍历法】  
    遍历集合 -> 就是使用【迭代器遍历】 
```java
    for(每一项的类型 item : 【数组】/【集合】){  
        // 此时拿到每一个元素并赋值给【item】
    }
``` 

* **遍历的选择：**   
    **索引遍历法**，能进行跳跃遍历、倒序遍历等操作
    **增强for循环**,一般用这个就行 

