+++
date = '2025-12-27T21:58:16+08:00'
weight = 2
draft = false
title = '收官章节(2)_io流'
description = '笔记性质的文章-文件之间的数据流通'
+++

**引言：**    
io流，就是**input输入流 和 output输出流**     
讲的是**文件**的**读取、编写**     
我们在操作系统上所有可视化的**文件**的挪动、复制、粘贴，就是改变**文件的路径**      
那这个过程底层其实就是**io流操作**     
例如，我们想把【文件1】复制到其他路径下，实际上是在新的路径下创建一个【空文件】，再把【文件1】的内容复制到【空文件】的过程         
把这个过程**用编程语言实现**就是【程序】读取【文件1】input，【程序】把内容写到【文件2】output

这些进行io流操作的文件在java中也就是对应的类、对象    

**File类**    
直接继承【Obeject】，是**文件类**的顶级父类
这个类创建的对象就是我们要操作的**文件**和**文件夹**    

* **创建对象：**
【File对象】的创建都是依赖【路径】的
```java
    File file = new File("C:\\Users\\92373\\Desktop\\io_Test\\io_study");
        // 直接拿完整的一行【路径】创建，最常用
    
    String string = "C:\\Users\\92373\\Desktop\\io_Test";
    File file3 = new File ( string,"io_study.txt");

    File file1 = new File("C:\\Users\\92373\\Desktop\\io_Test","io_study.txt");

    File file2 = new File("C:\\Users\\92373\\Desktop\\io_Test");
    File file3 = new File ( file2,"io_study");
    // 后面这几个创建方法都是先定位到【父路径】，也就是文件夹，  
    // 进而定位到文件夹下的具体【文件】/【文件夹】
```
【File对象】的创建中，如果**路径错误**，也就是路径指向的文件不存在，    
也不会报错，此时，对应路径上**不存在文件**，也就是说【File对象】指向的地方**没有文件** ，   
而大部分【File对象】**成员方法**，都是返回boolean值     
那【File对象】的对文件进行操作的**成员方法**都会返回【false】，但不会报错         
也就是说**操作会执行，只不过没有成功，**


* **File的成员方法：**  
```java
    File file = new File("C:\\Users\\92373\\Desktop\\io_Test\\io_study");

> 操作形 成员方法：
先操作，再判断操作是否成功

    file.createNewFile();
    //  路径下“【文件】不存在”时会在当前的【目录】下创建【新的文件】，返回boolean值
    //  有创建【新文件】返回true,否则返回false

    file.mkdir();
    //  路径下“【文件夹】不存在”时会在当前的【目录】下创建【新的文件夹】，返回boolean值
    //  有创建【新文件夹】返回true,否则返回false 
    // 【mkdir()】方法只能创建 “一级目录”

    file.mkdirs();
    // 这个是能创建 “多层级的目录”

    file.delete();
    // 删除路径指向的【文件】/【文件夹】，根据该操作的成功与否，返回【boolean】值

> 判断形成员方法：

    file.exists();
    // 判断路径指向的【文件】/【文件夹】是否存在，然后返回【boolean】值

    file.isDirectory();
    // 判断路径指向的是否是【文件夹】，然后返回【boolean】值
    
    file.isFile();
    // 判断路径指向的是否是【文件】，然后返回【boolean】值  

> 获取目录下所有【文件】和【文件夹】

    File[] files = file.listFiles();
    // 把目录下所有的【文件】【文件夹】创建成File对象，存储成【数组】返回

    file.getName();
    // 返回【文件】/【文件】的名称

>重写Obeject的方法：

    重写了继承自Object的【toString()】方法
    file.toString() 
    -> 返回【file对象】的里面输入的参数，也就是路径
    不管这个路径上是不是有【文件】/【文件夹】存在，
    都会返回“C:\\Users\\92373\\Desktop\\io_Test\\io_study”这个字符串
```

* **【File对象】是指向【文件】/【文件夹】的**   
也就是说它允许的操作对象可以是【文件】/【文件夹】    
这些操作就是实现了**windows系统上对文件的创建、查看【文件】/【文件夹】的操作**


**路径：**   
路径就是**文件/文件夹**的地址    
路径的**分隔符：**     
`\\`转移符+反反斜杠 和 `/`正斜杠 都可以，     
更推荐`/`，支持跨平台

* **绝对路径：**    
这个路径的**参照物**是**电脑**     
`C:\Users\92373\Desktop\工程\项目\io_Test`，带【磁盘符】的路径，     
从电脑的磁盘开始一层层往下找     

* **相对路径：**    
路径的**参照物**是**工作目录**     
`工作目录下某个目录/io_Test`，路径不能有**工作目录**，而是从**工作目录下的某个目录开始的**  

例如：   
```java
    // Java的一个工程下的完整路径：
    // 工程名\\项目名\\包名\\文件名
> 当你打开的是一个【工程】的时候，你的工作目录就是这个【工程】
    那你在【操作文件】里使用的相对路径就必须是【工程名开头】
    File("工程下对应文件夹/某个文件") 

> 当你打开的是一个【项目】的时候，你的工作目录就是这个【项目】
    那你在【操作文件】里使用的相对路径就必须是【项目名开头】
    File("项目下对应文件夹/某个文件")     
```

我们一般用的都是**相对路径**，因为**绝对路径**在不同的电脑下是不同的     
而我们在**jar打包**一般都是打包【项目】，给到外界，      
此时**相对路径**的【工作目录】就会是【项目】      

***

**引言：**    
我们上面学的【File类】是对【文件】/【文件夹】进行**路径上的操作**   
还谈不上说是**io流操作**    

接下来，我们就学习两大**io流**，**字节流和字符流**，真正对文件的内容进行**输入输出**
* **字节流 也只能操作【文件】，不能操作【文件夹】**    
也就是说**路径**一定是指向**文件**，如果指向**已经存在的文件夹**会报错 ！！

**字节流**        
两个 顶级**抽象类**【OutputStream】【InputStream】**字节流**    
以**字节**为单位对文件进行io操作      
我们要学习的是他们的子类【FileOutputStream】【FileInputStream】   

* **【FileOutputStream】:**    

**构造器：**
```java
    FileOutputStream fos = new FileOutputStream("LiHuaStudy_03\\io_Test\\io_study");   
    // 创建对象的时候，会在文件的【父目录】中寻找这个文件   
    // 如果找到该文件，会“清空文件里面的内容”   
    // 如果没找到该文件，会“在【父目录】创建这个文件”

    FileOutputStream fos = new FileOutputStream("LiHuaStudy_03\\io_Test\\io_study",true);
    // 形参【append】，输入实参【true】,此时，
    // 如果没找到该文件，会“在【父目录】创建这个文件”
    // 但是找到该文件的时候，“不会清空里面的数据”

    fos.close();
```

**成员方法：**
```java
    FileOutputStream fos = new FileOutputStream("LiHuaStudy_03\\io_Test\\io_study");   

> 成员方法都是操作形方法：

    fos.write(byte b);
    // 一次写入一个字节

    byte[] b = {'1','2','3'};

    fos.write(byte[] b);
    // 根据【字节数组】，一次写入多个字节
    fos.write(byte[] b,int begin_index,int length);  
    // 可以控制【字符数组b】的“起始的字符”和“写入的字符个数”
    // 从 b[begin_index] 开始，取 length个 字符存入文件
```

* **【FileInputStream】**

**构造器：**
```java
    FileInputStream fis = new FileInputStream("LiHuaStudy_03\\io_Test\\io_study");   
    // 路径指向的【文件】一定要存在，不然会报错

    fis.close();
```

**成员方法：**   
```java
    FileInputStream fis = new FileInputStream("LiHuaStudy_03\\io_Test\\io_study");   

> 成员方法：

    read = fis.read();
    // 一次读取一个字节，并返回【int】值
    // 读不到数据就会返回 -1   

【fis.read()】读取整个【文件】的格式：
    int len;
    if((len = fis.read()) != -1){
        // 使用read方法判断的同时，用len把 第一个字节 记录下来
        // 这样就不会遗漏 第一个字节了
        System.out.print((char) len);
        // 此时，这个【len】值就是我们要打印的【字节】对应ASCII的数值
    }

    byte[] b = new byte[2];

    read = fis.read(b);
    // 把读取的【文件】中的字符，放到【字符数组】里，并返回“读取到的字节”的个数
     // 读不到数据时就会返回 -1   
    fis.read(byte[] b,int begin_index,int length);  
    // 可以控制【文件】的“起始的字符”和“写入的字符个数”
    // 从 字节数组的b[begin_index] 开始存入，取【文件】的 length个 字节存入b

【fis.read(byte[] b)】读取整个【文件】的格式：
    // 每次读取【文件】内容，写到【字节数组b】里面，都是拿同一个b
    // 也就是说在b原本内容上做覆盖，而不是清空

    int len;
    if((len = fis.read(b)) != -1){
        // len在这里记录的是“读取字节的个数”
        String str = new String(b,0,len);
        // 用String 构造器返回 字节数组 的字符串内容 
        // 此时，用len对转化字符串进行长度限制，
        // 才能保证不会打印出b上一次内容中未被覆盖的 旧字节
        System.out.print(str);
    }

```

* **文件的读取操作**需要注意，`\n`换行符占**两个字节**，控制台显示**换行的效果**，不会打印出`\n`,      
但是在**字节流**中只占用一个字节，所以总的来说，在输入流是不用考虑`\n`的影响的

***

**字符流**
字节流（别名**万能流**）用于读取**字母、符号**都是ok的，但是**汉字**的读取会出现乱码    
所以我们更常用的还是**字符流**

* **字符集：**
是 读取（解码）/写出（编码） 的**规范**，编码和解码规范达成统一，中文就不会呈现乱码

    * **UTF-8**
一个汉字占3个字节

    * **GBK**
一个汉字占2个字节

* **字符流的族谱：**
抽象类【InputStreamReader】【OutputStreamReader】 主要学习两个子类【FileReader】【FileWriter】

```java
    FileReader fr = new FileReader("LiHuaStudy_03\\io_Test\\io_copyright");

    System.out.println((char)fr.read());
    //读取一个字符，返回scall值
    // 没读取到就返回-1 

    char[] chars = new char[100];

    fr.read(chars)
    // 返回读取的字符的个数
    String s = new String(chars);
    System.out.println(s);
    // 通过 字符数组 创建字符串

    fr.close();

> 完整读取文件的方法是和【字节流】一样的  
    while ((len = fr.read()) != -1){
        System.out.print((char)len);
    }

    char[] chars = new char[3];
    int len;
    while ((len = fr.read(chars)) != -1){
        System.out.print(new String(chars,0,len));
    }
```

**【FileWriter】**
```java
    FileWriter fw = new FileWriter("LiHuaStudy_03\\io_Test\\io_copyright",true);
    // 创建对象的时候，会在文件的【父目录】中寻找这个文件   
    // 如果找到该文件，会“清空文件里面的内容”   
    // 如果没找到该文件，会“在【父目录】创建这个文件”
        
        fw.write('A');
//        写入字符
        fw.write("陈雨轩");
//        可以直接加入字符串
        fw.flush();
        // FileWriter创建的对象自带内存缓存区，
        // 所以要用【flush()】刷新，才能把内容保存到文件里面

        fw.close();
```

* **文件的读取、写入的套路：**  
相当于实现了【文件】的**复制**

    * **用什么流读就用什么流写**
    * **所有的io流创建对象，最后都要进行close()释放**
    * **用byte[num]时，num大小一般用1024及其倍数，你根据文件内存的大小自己判断**   


```java
// 字节流
    public static void main(String[] args) throws IOException {
//        读取的【文件】，输入流
        FileInputStream fis = new FileInputStream("C:\\Users\\92373\\Desktop\\io_Test2");
//        写入的【文件】，输出流
        FileOutputStream fos = new FileOutputStream("LiHuaStudy_03\\io_Test\\io_copyright",true);

> 两个方法：
        int len;
        while((len = fis.read()) != -1){
            fos.write((char)len);
        }

        byte[] b = new byte[1024];
        while ((len = fis.read(b)) != -1){
            fos.write(new String(b,0,len));
        }

> 释放资源：
        fos.close();
        fis.close();        
    }

// 字符流
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("C:\\Users\\92373\\Desktop\\io_Test2");

        FileWriter fw = new FileWriter("LiHuaStudy_03\\io_Test\\io_copyright",true);

> 两种方法：
        int len;
        while((len = fr.read()) != -1){
            fw.write((char)len);
            fw.flush();
        }

        byte[] b = new byte[1024];
        while ((len = fr.read(b)) != -1){
            fw.write(new String(b,0,len));
            fw.flush();
        }

> 释放资源：
        fr.close();
        fw.close();
    }
```


