+++
date = '2025-12-23T19:04:18+08:00'
draft = false
title = '第三章_JDBC接口'
weight = 73
description = '笔记性质的文章-java提供的接口，不同的数据库提供不同的【实现类】,我们在java中创建实现类的对象，就可以操作数据库(**嫁接的桥梁的语言依然是SQL**)'
+++
引言：  
jdbc是java设计的接口规范，不同的数据库实现接口，创建实现类。  
数据库： MySQL 、 oracle 、 db2   
实现类：需要用jar打包 -> 导入idea<sup>这个过程叫“导入驱动”</sup>,就能使用   

### jdbc的操作过程 
1. **拿到数据库的jar包，导入驱动**  
    放置：  
    * 在idea的【项目】/【模块】目录下创建文件夹命名为`lib`<sup>专门放.jar的文件</sup>
    * 把数据库的jar包放入`lib`目录下  

    导入：  
    * 右键`lib`文件夹，选择`add as library`,选择`项目/模块`，点击`添加`

2. **加载驱动**
    把jar包加载到程序中：
    * 使用反射语法,【Class】类的静态方法  
        `Class.forName("Driver的全路径")`<sup>有对应的“强制异常”,需要抛出</sup>
        * Driver全路径：在MySQL的jar目录下  
            "com.mysql.cj.jdbc.Driver"

3. **创建链接<sup>java-数据库</sup>,本质是创建对象**<sup>**占用资源的行为，待释放**</sup>      
    * 【类方法】`DriverManager.getConnect(url,user,password)`   
        url:"jdbc:mysql://公网IP:端口/数据库<sup>结构上的</sup>名"  
        user: 数据库名称  
        password: 数据库的密码  
        * 返回成功连接数据库的对象，【Connect】类对象 

    创建对象：  
    * Connect conn = DriverManager.getConnect(url,user,password)       

4. **写SQL语句**
    * String sql = "SELECT * FROM 表名称【WHERE】字段 = ?"<sup>不同语言之间的互通基本都是用字符串</sup>
        * ?:占位符，可以通过【set】方法填入Java的数据
        * sql语句在字符串中不加';'

5. **创建“执行者”对象**<sup>**占用资源的行为，待释放**</sup>
    * 【Connect】类对象的【实例方法】`conn.prepareStatement(sql)`
        * 返回执行者对象，【PreparedStatement】类对象

    创建对象：  
    * PreparedStatement ps = conn.prepareStatement(sql);     
```java
    > 执行对象的相关操作
    `ps.setXXX(1,数据)`-> 拿数据替换？占位符，完善SQL语句
```

6. **拿到执行的结果并处理，本质是创建“结果”对象**<sup>**占用资源的行为，待释放**</sup>
    * 【PreparedStatement】类对象的【实例方法】`ps.executeQuery()` -> 拿到查询的结果对象    
        * 返回结果对象，【ResultSet】类对象

    创建对象：  
    * ResultSet reSet = ps.executeQuery();   
```java
    > 结果对象的相关操作
    `reSet.next()` -> 与迭代器的指针工作原理一致，  
    判断指针后一位有没有数据，并后移指针，返回的是boolean值

    `reSet.getXXX("字段")` -> 拿到当前指针所在的数据条的数据
    -> 实现数据库的信息拿到程序中，用【实体类】接住：  
        类名：表名  |  成员属性：表字段  |  
        本质上是一种ORM映射
    =================================================
    只有【sql的查询语句】才需要上述的方法，构建对象
    【sql的增删改语句】只需要使用`ps.executeUpdate()`方法，

        【PreparedStatement】类对象的【实例方法】`ps.executeUpdate()`  
    -> 拿到增删改的影响行数，返回的是 int  

```  

7. **释放资源**
    * 资源保持链接状态而不释放，会导致安全隐患  
    * 后创建的对象先释放，自上而下的释放
    * reSet.close();  
      ps.close();  
      conn.close();      


***
## jdbc实现”打开数据库 - 把数据拿出来 - 关上数据库“
* **基于上述的七大基本操作，完整的链接数据库实现：**  
```java
    由于jdbc存在【释放资源】的步骤
    我们用【try-catch-final】框架保证【释放资源的实行】
    ============================================
    1. 导入驱动
    Connection conn = null;
    PreparedStatement ps = null;
    try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/lihua_study",
                    "root",
                    "@CHen918512");
            String sql = "INSERT INTO player_data(player_name,player_password) VALUES (?,?)";
            ps = conn.prepareStatement(sql);
            ps.setString(1,"陈雨轩");
            ps.setString(2,"chen12345");
            if ( ps.executeUpdate()>0){
                System.out.println("注册成功!");
            }
        } catch (Exception e) {
            throw new Exception(e);
        } finally {
            if(ps != null){
                ps.close();
            }
            if(conn != null){
                conn.close();
            }
        }        
```
