+++
date = '2025-12-22T21:51:10+08:00'
draft = false
weight = 72
title = '第二章_操作数据库的语言_SQL'
description = '笔记性质的文章-因为使用的MySQL数据库，是一种【关系型数据库】，所以我们学习SQL操作这种类型的数据库'
+++
引言：我们直接在这里标记语法主要事项：  
* **values(除了数值类型数据、bool值、null值、其他数据都要加'单引号')，主键的数值填入null即可**
* **语句记得以 ';' 结尾！！画龙点睛之笔！**
* **大小写没有语法区分，但是建议统一关键字用大写；库名、表名、字段名统一用小写**


### SQL语言
操作[【关系型数据库】](#关系型数据库)的语言  
SQL语言分为四大类：  
1. DDL，创建 库-表 的语句，我们一般直接用可视化工具
2. **DML，增删改数据**
* 增加数据 -> （参照字段，操作单位是数据条）
```SQL 
    >语法样式 INSERT into 表名称（选中字段）VALUES(对应数值);
-- 全字段增加
INSERT into produce VALUES('手套',1500,null,'LiHua有限公司');
-- 选中字段增加
INSERT into produce(product_name) VALUES('手套');
========================================================
-- 探讨“增加”的细节
INSERT into produce(price,product_name) VALUES(1500,'手套');
-- 结论一：在“选中字段增加”条件下，增加的行为可以自己调整顺序
INSERT into produce VALUES('手套',1500,null,'LiHua有限公司');
-- 结论二：在“全字段增加”条件下，横在数据之间的‘主键’填写null
-- 注意事项：   
-- VALUES（括号内的数值：除了bool、数值类型、null，其他的类型都要加'单引号'）
```  

* where筛选语句 -> 通过 WHERE 限制字段的范围
```SQL
    > 语法样式 在 VALUES 后面加入 WHERE 条件，【表示可加可不加】  
    # 除了增加数据的操作，其他DML语言都能增加
========================================================= 
条件的逻辑符：（条件的判断对象是“字段”） 

字段（数值类型） BETWEEN A AND B 
-- 》》取[A,B]范围内
字段(任意类型) IN (A,B,C)
-- 》》x=A / x=B / x=C ,条件成立
字段（字符串类型） LIKE '匹配字符'（'%'无限个通配符；'_'是一个通配符）
-- 》》当字段符合匹配字符时，条件成立

null值的判断用 IS / IS NOT  
其他数值判断用 = / !=
-- 【大小判断】和【逻辑运算符】照旧
```

* 修改数据 -> （参照字段，操作单位是字段行）
```SQL
-- 选字段行修改
    > 语法样式 UPDATE 表名称 SET 字段 = 数值,字段 = 数值;【WHERE】
--  实列：
 UPDATE produce set price = 2500 WHERE brand in ('LiHua有限公司'); 
```

*  删除数据 -> （参照字段，操作单位是数据条）
```SQL
    > 语法样式 DELETE FROM 表名称 【WHERE】 
DELETE FROM produce WHERE id BETWEEN 4 AND 6;
```

3. **DQL，查询数据**
* 查询数据 -> （参照字段，操作单位是字段行）
```SQL
    > 语法样式 SELECT (字段名) FROM 表名称 【WHERE】;  
	SELECT * FROM 表名称 【WHERE】	-> '*' 表示“全部字段”
-- 会返回结果,查询不到时会返回空表						 
==========================================================						 
-- 查询中才会出现的语法，基于 SELECT FROM 语句


-- 1. 排序 -> 参照字段
    > 语法样式 SELECT (字段名) FROM 表名称 【WHERE】ORDER BY 字段 【ASC | DESC】
--  查询结果出来才能排序，所以语句在最末尾。
-- 默认是 ASC 升序,NULL 空值放在最前面


-- 2. 聚合查询 -> 有【聚合函数】就是【聚合查询】，
-- 返回“聚合函数”的计算结果，生成的字段行返回,
-- 字段名 就是“函数（字段）”，数据 就是“计算结果”
聚合函数：
count(单个具体字段 / * )
-- 》》返回查询到的 数据条的数量
sum(数值型字段)
avg(数值型字段)
max(数值型字段)
min(数值型字段)
实例：
SELECT count(price) AS count_price,count(*) AS count_items
    FROM produce WHERE id > 3 && id < 12 ORDER BY price;
--  聚合函数是对查询的字段进行运算，参数就是“字段”，
-- 可以起别名: 聚合函数（字段） AS a 
--  一个聚合函数就是一个“字段行”，且只能存放一个数据


-- 3. 分组查询 -> 本质是“分组聚合查询”，只有在【聚合函数】中使用才有价值,
-- 是按照字段的情况，分开计算不同情况下的聚合函数的结果 
实例：
 SELECT price,count(product_name),count(*),count(price) 
    FROM produce GROUP BY price ;
--  按哪个“字段”分组（只能用一个字段分组），就查询那个字段，以及返回【聚合函数】的结果,
-- 此时，'count(*)'和'count(price)'是等价的
-- 筛选：
1. 特有的筛选【HAVING】条件，是对分组之后有的字段进行筛选
2. 【WHERE】,是对分组之前的字段进行筛选 
实例：
 SELECT price,count(product_name),count(*) AS count 
    FROM produce 
        WHERE brand = 'LiHua有限公司' GROUP BY price HAVING count > 2;


-- 4. 分页查询 -> 是对最终总表的截取，语法处在 ORDER BY 排序的后面，
-- 且排序是必要的，不然每次截取的结果可能不同
 -> 语法样式 SELECT * FROM 表名称 ORDER BY 字段 limit m,n;
--  把整个表的数据条 1 ~ 最后 
--  当前这一页起始的编号【m】，“m+1”则本页第一数据条的编号 ； 当前页面数据条条数【n】
-- 设计技巧：我们把整个第一条数据编号 0，
-- 每页的其实编号【m】 = (当前页数 - 1)* 当前页面的数据条数【n】
实例：
 SELECT * FROM produce ORDER BY price limit 0,5; 
 SELECT * FROM produce ORDER BY price limit 5,5; 
```

```SQL
    -- 语法逻辑的先后
    语句 【where】 group by 【having】 order by limit
```

```SQL
    -- 查询的嵌套
    SELECT count(*) FROM produce -> 聚合查询返回的是【函数结果】，
    所以他可以在嵌套在另一个查询的where条件中作为数值存在，
    实例：
    `SELECT * FROM produce WHERE 字段 > (SELECT count(*) FROM produce) `
```

4. DCL，权限设置，我们无需操作



### 扩展知识点
#### 关系型数据库
本质就是“二维表”，字段和数据相对。  
-MySQL就是关系型数据库