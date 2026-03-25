+++
date = '2025-12-01T19:20:55+08:00'
draft = false
weight = -38
title = '数据库的套路性操作'
description = '记一些数据库相关的套路'
+++

**字段的设计**    
必要的【字段】    
* **id**   
设计【主键】就行    
**主键约束：**    
    `点击 【键】` + `【默认约束】的地方勾选【自动递增】` -> 主键就是专门用来设置**id**字段的，让**id**拥有**非空、唯一、自增**这三个特性    
    拥有**自增**特性的数据，会**默认从1给数据填写数值**，    
    如果你删除了某几行数据，它会从**最大的id号**开始往下默认填写数值，防止**重复的情况**出现      

* **【时间】**   
**时间相关的字段就直接设置两个**，   
一个是【create_time】记录数据的创建时间 ,     
一个是【update_time】记录数据的修改时间    
* 我们直接进行**设置默认填写时间的字段：**  
`【默认约束】填写【current_timestamp】` -> 填写数据时默认**当前的时间戳**，这个用来设置【create_time】   
`【默认约束】填写【current_timestamp】` + `【默认约束】勾选【根据当前时间戳】` -> 更改**这一行任意数据时**会默认修改时间字段的数据，改成**当前的时间戳**，这个用来设置【update_time】      

***   

**DQL 中的【分页查询】设置**   
* **分页查询**   
只会返回**一个结果**，也就是说它返回的是完整表的**一个切片**  

```sql
    select * from xxx_yyy limit a , b ;  
    -- 会显示【第a+1行】的数据到【第a+b行】数据，也就是（a,a+b]
    -- a+1 就是起始的数据，b 是显示出来的数据的个数
> 分页查询设置的套路： 
    因为每一个【分页查询】都是完整的表的切片，   
    我们为了让切片能完整的对接上，就必须设置好每次【分页查询】的【a】起始值    
    公式：  
    【页面起始数据的标识】a = (当前的页面-1)*页面的大小【b】 
```  

* **DQL 查询数据的【关键字】很多，我们要理清一下它的逻辑上的先后**   
* **正常的查询**

```sql
    select 字段1，字段2 from xxx_yyy where 条件 order by 字段 asc limit a,b ;
```

* **聚合查询**
```sql
    select 字段1，聚合函数(字段2) as A ，聚合函数(字段3) from xxx_yyy where 条件 group by 字段 having 条件 order by 字段 asc limit a,b ;
```

***

* **连接并操作数据库的方法的套路写法：**

```java
    public static void xxxYyy() throws ClassNotFoundException, SQLException {
        String sql_01 = "select * from people_age where id = ?;";
        String sql_02 = "insert into people_age(age) values(?);";

        Connection conn = null ;
        PreparedStatement ps = null ;
        ResultSet rs_01 = null  ;
        int rs_02 ;
// 【占用资源的对象】作为方法中的全局变量，才能确保在【finally】中释放对象
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/study_datebase","root","@CHen918512");
           
            ps = conn.prepareStatement(sql_01);
            ps.setInt(1,88);
            rs_01 = ps.executeQuery();
            if (rs_01.next()){ 
                System.out.println("查询成功了");
                映射类对象.setXxx(rs_01.getString("字段名"))
            }

            ps = conn.prepareStatement(sql_02);
            ps.setInt(1,88);
            int rs_02 = ps.executeUpdate();
            if(rs_02 > 0){
                System.out.println("操作成功了");
            }

        }catch(Exception e){
            throw e;
        } finally {
            
        // 预防空指针报错
            if (rs != null) {
                rs.close();
            }
            if (ps != null){
                    ps.close();
            }
            if (conn != null) {
                conn.close();
            }
        }
    }
```

* **数据库里面数据的查重：**  
使用数据库注册时，用户名要求不同

```java
    int player_name = "陈雨轩" ;  
    String sql = "select * from 表 where id = ?"
    ps.setString(1,player_name);

    rs = ps.executeQuery();
    // 拿到查询的结果
    if(rs.next()){
        System.out.println("该数据已经存在");
    }
```

***

* **配置文件的使用**    
如果只是**程序里的数据**，我们想要增加它的复用性，创建**工具类**就行    
但是由于**数据库**这个第三方软件的加入，我们的程序中的数据库地址、密码、用户名、驱动的地址，这些数据都是会因为**数据库**的不同而改变的    
所以我们要用到**配置文件**，在外部改变这些数据，同时结合**工具类**把这些数据作用大到程序内     

* **先设置一个正常的工具类：**
```java
    public final class Test{
        private Test(){

        }

        public static final  XXX_YYY = 100;
        // 静态常量
        public static void xxxYyy() {
        // 静态方法
        }    
        // 这些通用数据都是【程序内数据】
    }
```

* **设置配置文件：**    
在项目的目录下新建文件夹config
这里面放的文件必须是用.properties后缀结尾（也就是配置文件）
```properties
 key = values
    <!-- 用这个结构存储数据 -->

    url = jdbc:mysql://127.0.0.1:3306/数据库 
    driver = com.mysql.cj.jdbc.Driver
    <!-- 字符串不需要使用`""`引号 -->
```

* **在【工具类】基础上，获取我们的【配置文件】的数据:**
```java
    public final class Test{
        static String driver;
        static String url;
        // 工具类创建【静态属性】

        static{
            Properties prop = new Properties();
            // 创建配置文件Properties对象
            prop.load(new FileInputStream("config/具体的配置文件"));
            // 导入【输入流】对象，也就是拿到配置文件的数据    
            driver = prop.getProperty("键名");
            // 依赖【键名】拿到【值】，并存入工具类的属性
        }

        private Test(){

        }
        public static final  XXX_YYY = 100;
        public static void xxxYyy() {

        }    
// 在【静态代码块】里面执行的语句，如果要处理异常，你只能用异常的捕获，因为跟main方法一样，再往上抛就是抛给jvm虚拟机
    }
```