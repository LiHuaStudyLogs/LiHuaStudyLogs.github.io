+++
date = '2025-12-14T15:35:36+08:00'
draft = false
weight = 6
title = '第五章_官方API文档使用'
description = '工具性质文章-我们学会构建自己的规范类，也要学会使用其他人写的规范'
+++
引言：这篇文章记录了
【String】以及【String其他】类的使用，
【System】类的使用

## 学习查阅官方文档

# API，官方提供的功能类
* 查阅功能类
    1. 看【构造器】，了解对象是如何创建的，有哪些【属性】
    2. 看【所有方法】，了解这个类的方法怎么使用

### API中的的常用包
1. **java.lang包**
    > 核心包，包会有Java虚拟机<sup>jvm</sup>自动发导入，包含System类、String类    
2. **java.util包**
    > 工具包，主要包含【工具类】【集合类】，包含Scanner类、Random类、List类
3. **java.io包**
    > 输入输出包，提供读写文件相关的类，包含如FileputStream类、FileOutStream类
4. **java.net包**
    > 网络包，提供大量网络编程相关类，包含如ServerSocket类、Socket类
5. **java.spl包**
    > 数据包，提供大量的数据类和接口，包含如DriverManager类、Connection接口    
    
### 关于一个容器的学习
* 我们掌握这个容器，主要遵循CURD学习思路：
    1. 增 
    2. 删 
    3. 改 
    4. 查（获取）

## 学习API提供的类
* #### String类
    java.lang包下，直接继承自【Obeject】

```java
    > 如果直接继承自【Object】,一般会出现重写【Object】方法的行为
        1. 重写了[toString] -> 返回对象的地址（*return this*），
            而这里会直接拿到String对象的字符串内容，并返回String
        2. 重写了[equals] -> 比较字符串的内容是否相等，返回boolean值
    ------------------------------------------------------------
    > 观察构造器
        1. 构造对象的方式
            |new String()    
            |new String(`byte[]`) 
            |new String(`char[]`)    
            |new String(`String`)
        * 这些都是通过构造器构建对象，new方式开辟空间
      -String name = "陈雨轩"；
      * 特殊的构造字符串的方式  
        这样的构造对象方式，是在【字符串常量池】中开辟空间，
        他会先在常量池中寻找"陈雨轩"，如果有，则复用|
       String name02 = "陈雨轩" -> 是把上一个放"陈雨轩"的空间给到【name02】
        当常量池中没有这个字符串时，才会在常量池开辟新的空间
    --------------------------------------------------------------
    > 观察方法[取常用的]，（*str表示字符串对象*）
        1. 查
            str.length() -> 返回字符串长度
            str.charAt(int index) -> 根据[索引]返回字符
           -str.indexOf(String str) -> 根据[目标字符串]，返回首次出现的首字母索引
           -str.lastIndexOf(String str) -> 返回最后出现的首字母索引 
            * indexOf方法找不到“子字符串”时返回-1

        2. 增、删、（改）-> string类中，本质是开辟新空间,返回【新字符串】
            str.concat(String str) -> 与`+`等价，效率更高
           -str.replace(char old,char new) -> 新的换旧的
           -Str.replace(String old,Strting new)
                举例："abaab".repalce("ab","xx") -> "xxaxx"
           -str.toUpperCase()->全大写
           -str.toLowerCase()->全小写
            str.strip() -> 去除首尾所有的空字符（*\n、空格等等*）

        3. 截取 -> 获得新的字符串
           -str.subString(int beginIndex) -> 返回[beginIndex,末尾]的部分
           -str.substring(int beginIndex,int endIndex)
            ->[beginIndex,endIndex)的部分  
         . 拆分    
           -str.split(String regex) -> 返回String[]数组   
           -str.split(String regex,int limit) 
            -> 返回String[]数组(*limit表示数组元素个数上线*) 

        4. 判断 -> 返回boolean值
            str.equalsIgnoreCase(String str) -> 判断字符串内容是否一致(*忽略大小写*)
            str.isEmpty() -> 判断是否为`""`,完全的空字符串
           -str.startsWith(String prefix) -> 判断前缀字符串是否一致  
           -str.endsWith(String prefix) -> 判断后缀字符串是否一致
            str.contains(子字符串) -> 判断是否包含子字符串  

        5. 类型转换 -> 返回其他类型
            str.toCharArray() -> 把字符串转换成char[]数组
        【静态方法】String.valueOf(参数) -> 把参数转换成字符串
            * 参数可以是(基本数据类型|引用数据类型),但如果是`null`会报错
            * 相对于`""+数据`转换字符串的方法，`valueOf`效率更高，但是不能转换`null`
    --------------------------------------------------------------
    > String类的特点
        1. 他的底层实际是【字符数组】作为【类属性】，
            导致长度空间是固定，所以【String类】的字符串内容不能改变
          -String a = "123"; 开辟空间给到【a】           
            a = "123" + "a" ; 开辟新空间，地址给到【a】，
            原本存放“123”的空间内容不受影响。改变的是【a】指向的地址
          -String a = "123";
            str.replace("1","a");
            System.out.println(str); -> 返回的还是"123";     
```            

    * > String类的平级类【StringBuffer】和【StringBuilder】
<img src="./d64452f7587dca9376a2546cc3dc6964.jpg">
* >【StringBuffer】【StringBuilder】相对于【String】类，创建的字符串是可变的，  
    底层初始化字符数组容器空间为16，当字符串内容改变，空间会随之扩容 | 【StringBuffer】和 【StringBuilder】之间的区别则是线程安全问题，【构造器】和【方法】都是一致的。

* #### StringBuffer类和StringBuilder类
```java
    > 直接继承自【Object】
        1. 重写了[toString] -> 返回字符串内容
    --------------------------------------------------------------
    > 观察构造器
        1.  new StringBuffer()
        2. StringBuffer(String str)
        3. StringBuffer(CharSequence seq) 
    * 这些都是初始容量默认16，且如果有传入参数，16则是预留的空间
        4. StringBuffer(int capacity) 
    * 这个是指定初始化的空间大小  
      # 没有【字符串常量池】构造法
    -------------------------------------------------------------   
    > 观察方法[取常用的]，（*str表示字符串对象*）   
        1. 查找（获取）
            sb.charAt(int index) -> 根据索引，返回字符
           -sb.indexOf(String 子字符串) -> 返回首个的首字母的索引
           -sb.lastIndexOf(String 子字符串) -> 返回末尾的首个的首字母的索引
            * indexOf方法查找不到返回-1
            sb.length() -> 返回字符数
            sb.capacity() -> 返回底层字符数组的容量

        2. 增、删、（改） -> 直接改动对象的空间里存放的内容
            sb.append(任意类型 a) -> 作用和`+`一样
            sb.insert(int index,任意类型) -> 根据索引位置增加
            sb.delete(int a,int b) -> 删除[a,b)部分
            sb.deleteCharAt(int index) -> 根据索引删除字符
            sb.replace(int a,int b,String new) -> 先删除[a,b)部分，在[a]处插入new
            sb.setChar(int index,char c) -> 根据索引替换字符

        3. 特殊方法
            sb.trimToSize() -> 把容量缩成实际的字符长度
            sb.reverse() -> 反转字符串    

        4. 判断方法和【String】类基本一致
            * 除了【StringBuffer/StringBuilder】继承的【equals】方法没有重写    
```
    * 三者之间的类型转换潜藏在【方法】中
        【StringBuffer】【StringBuilder】类 -> 转换成【String】类
        | 这两个【】.toString -> 返回的就是字符串String
        | 这两个【】的构造器，参数存入字符串 -> 就是【String】变【】
        | 这两个【】之间的转换，只需要【String】做桥梁就行


***
* #### System类
    java.lang包下，直接继承自【Object】,【工具类】
```java
    > System类是【工具类】，【成员】纯静态，类.成员实现访问
        不能实例化
    -----------------------------------------------------------
    > 静态【成员属性】
        1. System.in -> 标准输入流
        2. System.out -> 标准输出流
        3. System.err -> 标准错误流
      -都是不同的类对象
    ----------------------------------------------------------
    > 静态【成员方法】
        1. 时间相关
            - System.currentTimeMillis() -> 返回系统时间戳（*毫秒级*）    
            - System.nanoTiem() -> 返回纳秒级时间，精度更高，但是起始时间无意义
```    