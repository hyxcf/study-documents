##### 修改mysql的主机名

```mysql
update user set host='192.168.1.120' where user='root';// 设置主机名为 192.168.1.120
flush privileges;// 每一次都要刷新权限
```





![image-20240809085955706](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240809085955706.png)

# 				Mysql笔记



# 				基础篇

## 					1.Mysql概述

- 启动与停止（以管理员身份运行cmd）

  ```mysql
  启动  net start mysql80 
  停止	net stop mysql80
  注意：默认 mysql 是开机自启动的
  ```

- 客户端连接

  ```mysql
  系统自带的命令行工具执行命令 
  mysql [-h 127.0.0.1] [-p 3306] -u root -p
  ```

Mysql 数据库

- 关系型数据（RDBMS）：通过表结构存储数据的数据库被称为关系型数据库。

  概念:建立在关系模型基础上，由多张相互连接的二维表组成的数据库。

  特点:

  1.使用表存储数据，格式统一，便于维护

  2.使用SQL语言操作，标准统一，使用方便

# 2.SQL

- #### SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾。

2. SQL语句可以使用空格/缩进来增强语句的可读性。

3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。

4. 注释:

   ```mysql
   ·单行注释: --注释内容 或 # 注释内容(MySQL特有)
   	多行注释:/*注释内容*/
   ```

- #### SQL分类

1. DDL	Data Definition Language	数据定义语言，用来定义数据库对象(数据库，表，字段)
2. DML	Data Manipulation Language	数据操作语言，用来对数据库表中的数据进行增删改
3. DQL	Data Query Language	  数据查询语言，用来查询数据库中表的记录
4. DCL	Data Control Language	数据控制语言，用来创建数据库用户、控制数据库的访问权限

- #### DDL

##### DDL-数据库操作

查询

```mysql
查询所有数据库		SHOW DATABASES;
查询当前数据库		SELECT DATABASE();
```

创建

```mysql
CREATE DATABASE [IF NOT EXISTS]数据库名「DEFAULT CHARSET 字符集][COLLATE 排序规则];
create database itheima default charset utf8mb4;#指定默认字符集为utf8mb4
```

删除

```mysql
DROP DATABASE [IF EXISTS] 数据库名;
```

使用

```mysql
USE 数据库名;
```

##### DDL-表操作-查询

查询当前数据库所有表

```mysql
SHOW TABLES;
```

查询表结构

```mysql
DESC 表名;
```

查询指定表的建表语句

```mysql
SHOW CREATE TABLE 表名;
```

##### DDL-表操作-创建

```mysql
CREATE TABLE表名(
		字段1 字段1类型 [COMMENT字段1注释],
    	字段2 字段2类型 [COMMENT字段2注释],
    	字段3 字段3类型 [COMMENT字段3注释],
		......
		字段n 字段n类型 [COMMENT字段n注释]
) [COMMENT 表注释];
```

DDL-表操作-数据类型

数值类型

![image-20240609022508755](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240609022508755.png)

```
age tinyint unsigned #设置为无符号
```

字符串类型

![image-20240609022955947](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240609022955947.png)

日期时间类型

![image-20240609023540729](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240609023540729.png)

**DDL-表操作-修改**

添加字段

```mysql
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT注释] [约束];
```

修改数据类型

```mysql
ALTER TABLE 表名 modify 字段名 新数据类型(长度);
```

修改字段名和字段类型

```mysql
ALTER TABLE 表名 change 旧字段名 新字段名 类型(长度) [COMMENT注释] [约束];
```

删除字段

```mysql
ALTER TABLE 表名 drop 字段名;
```

修改表名

```mysql
ALTER TABLE 表名 rename to 新表名;
```

**DDL-表操作-删除**

删除表

```mysql
drop table [if exists] 表名;
```

删除指定表，并重新创建该表( truncate  :vt.截断；截短，缩短，删节(尤指掐头或去尾)  )

```mysql
truncate table 表名; #清空
```



- #### DML

  - DML-添加数据

    1) 给指定字段添加数据

       ```mysql
       INSERT INTO 表名(字段名1,字段名2,...) VALUES (值1,值2,...);
       ```

    2) 给全部字段添加数据

       ```mysql
       INSERT INTO 表名 VALUES (值1,值2,....);
       ```

    3) 批量添加数据

       ```mysql
       INSERT INTO 表名(字段名1，字段名2,...） VALUES (值1,值2,...),(值1,值2,...);
       INSERT INTO 表名 VALUES (值1,值2,...),(值1,值2,..),(值1，,值2,...);
       ```

    注意:
    插入数据时，指定的字段顺序需要与值的顺序是一 一对应的。
    字符串和日期型数据应该包含在引号中。
    插入的数据大小，应该在字段的规定范围内

  - DML-修改数据

    ```mysql
    UPDATE 表名 SET 字段名1=值1，字段名2=值2 ,....[ WHERE 条件];
    ```

    注意:修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。

  - DML-删除数据

    ```mysql
    DELETE FROM 表名 [WHERE 条件]
    ```

    注意:
    DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。

    DELETE 语句不能删除某一个字段的值(可以使用UPDATE)。




- #### DQL

  查询关键字：select

  - DQL-语法（编写顺序）

    ```mysql
    select 
    	字段列表
    from
    	表名列表	
    where
    	条件列表
    group by 
    	分组字段列表
    having
    	分组后条件列表
    order by
    	排序字段列表
    limit
    	分页参数
    ```

    - 基本查询
    - 条件查询(WHERE)
    - 聚合函数(count、max、min、 avg、sum)
    - 分组查询(GROUP BY)
    - 排序查询(ORDER BY)
    - 分页查询( LIMIT)

  - 基本查询

    1) 查询多个字段

       ```mysql
       select 字段1,字段2,字段3 ...from 表名;
       select * from 表名;
       ```

    2) 设置别名

       ```mysql
       SELECT 字段1[AS 别名1], 字段2[AS别名2]... FROM 表名;
       ```

    3) 去除重复记录

       ```mysql
       SELECT DISTINCT 字段列表 FROM 表名;
       ```

       ```mysql
       -- 基本查询
       -- 1，查询指定字段name，workno, age返回
       SELECT name,workno,age FROM emp;
       -- 2，查询所有字段返回
       SELECT * FROM emp;
       -- 3，查询所有员工的工作地址,起别名
       SELECT workaddress as '工作地址' FROM emp;
       -- 4，查询公司员工的上斑地址(不要重复)
       SELECT DISTINCT workaddress '工作地址' FROM emp;
       ```

       

  - DQL-条件查询

    1. 语法

       ```mysql
       SELECT 字段列表 FROM 表名 WHERE 条件列表;
       ```

    2. 条件

       |     比较运算符     |                  功能                   |
       | :----------------: | :-------------------------------------: |
       |         >          |                  大于                   |
       |         >=         |                大于等于                 |
       |         <          |                  小于                   |
       |         <=         |                小于等于                 |
       |         =          |                  等于                   |
       |      <>或 !=       |                 不等于                  |
       | BETWEEN ...AND ... |     在某个范围之内(含最小、最大值)      |
       |      lN(...)       |       在in之后的列表中的值,多选一       |
       |    LIKE 占位符     | 模糊匹配(_匹配单个字符,%匹配任意个字符) |
       |      IS NULL       |                 是NULL                  |

       | 逻辑运算符 |            功能            |
       | :--------: | :------------------------: |
       | AND 或 &&  |   并且(多个条件同时成立)   |
       | OR 或 \|\| | 或者(多个条件任意一个成立) |
       | NOT 或 ！  |          非，不是          |

       ```mysql
       --条件查询
       -- 1，查询年龄等于88的员工
       select * FROM emp where age = 88
       -- 2．查询年龄小于20的员工信息
       select * from emp where age < 20
       -- 3．查询年龄小于等于20的员工信息
       select * from emp WHERE age = 20
       -- 4.查询没有身份证号的员工信息
       SELECT * FROM EMP WHERE idcard is NULL
       -- 5．查询有身份证号的员工信息
       SELECT * FROM EMP WHERE idcard is not NULL
       -- 6．查询年龄不等于88 的员工信息
       select * FROM emp where age != 88
       select * FROM emp where age <> 88
       -- 7．查询年龄在15岁(包含)到 20岁(包含)之间的员工信息
       select * FROM emp where age >= 15 && age<=20
       select * FROM emp where age BETWEEN 15 AND 20
       -- 8．查询性别为女且年龄小于25岁的员工信息
       select * from emp where age < 25 AND GENDER = '女'
       -- 9．查询年龄等于18 或20或40的员工信息
       select * FROM emp where age=18 || age=20 || age=40
       select * from emp where age IN(18,20,40)
       -- 10．查询姓名为两个字的员工信息
       select * from emp where name LIKE '__'
       -- 11，查询身份证号最后一位是X的员工信息
       select * from emp where idcard LIKE '%X'
       ```

  - 聚合函数(count、max、min、 avg、sum)

    1) 语法

       ```mysql
       SELECT 聚合函数 (字段列表) FROM 表名;
       ```

       ```mysql
       -- 聚合函数
       -- 1，统计该企业员工数量
       SELECT count(*) FROM EMP;
       SELECT count(ID) FROM EMP;
       -- 2．统计该企业员工的平均年龄
       SELECT AVG(AGE) FROM EMP;
       -- 3．统计该企业员工的最大年龄
       SELECT MAX(AGE) FROM EMP;
       -- 4．统计该企业员工的最小年龄
       SELECT MIN(AGE) FROM EMP;
       -- 5.统计西安地区员工的年龄之和
       SELECT SUM(AGE) FROM EMP WHERE WORKADDRESS = '西安';
       ```

       ***注意：所有NULL值不参与聚合函数的运算。***

  - DQL-分组查询

    1. 语法

       ```mysql
        SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名[HAVING 分组后过滤条件];
       ```

    2. where与having区别

       执行时机不同: where是分组之前进行过滤，不满足where条件，不参与分组;而having是分组之后对结果进行过滤。

       判断条件不同:where不能对聚合函数进行判断，而having可以。

       ```mysql
       -- 分组查询
       -- 1．根据性别分组，统计男性员工和女性员工的数量
       SELECT gender,COUNT(*) FROM EMP GROUP BY gender; 
       -- 2．根据性别分组，统计男性员工和女性员工的平均年龄
       SELECT gender,AVG(AGE) FROM EMP GROUP BY gender; 
       -- 3，查询年龄小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址
       SELECT WORKADDRESS,COUNT(*) as address_count FROM EMP WHERE AGE < 45 GROUP BY WORKADDRESS HAVING address_count >= 3; 
       ```

       注意

       执行顺序: where > 聚合函数 > having .

       分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

  -  DQL-排序查询

    1. 语法

       ```mysql
       SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1，字段2 排序方式2;
       ```

    2. 排序方式

       ASC：升序（默认值）

       DESC：降序

       注意:如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。

       ```mysql
       -- 排序查询
       -- 1．根据年龄对公司的员工进行升序排序
       SELECT * FROM EMP ORDER BY AGE ASC;
       -- 2．根据入职时间，对员工进行降序排序
       SELECT * FROM EMP ORDER BY ENTRYDATE DESC;
       -- 3．根据年龄对公司的员工进行升序排序，年龄相同，再按照入职时间进行降序排序
       SELECT * FROM EMP ORDER BY AGE ASC,ENTRYDATE DESC;
       ```

  - DQL-分页查询

    1. 语法

       ```mysql
       SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
       ```

       注意

       起始索引从0开始，起始索引= (查询页码-1) * 每页显示记录数。

       分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT.

       如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10。

       ```mysql
       -- 分页查询
       -- 1．查询第1页员工数据，每页展示10条记录
       SELECT * FROM EMP LIMIT 0,10;
       -- 2．查询第2页员工数据，每页展示10条记录
       SELECT * FROM EMP LIMIT 10,20;
       ```

  - DQL-执行顺序

    ![image-20240618043345099](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240618043345099.png)

    


- #### DCL

  DCL英文全称是Data Control Language(数据控制语言)，用来管理数据库用户、控制数据库的访问权限

  - DCL-管理用户

    1. 查询用户

       ```mysql
       USE mysql;
       SELECT *FROM User;
       ```

    2. 创建用户(注意:主机名可以使用%通配)

       ```mysql
       CREATE USER '用户名'@'主机名'  IDENTIFIED BY '密码';
       ```

    3. 修改用户密码

       ```mysql
       ALTER USER '用户名'@'主机名' IDENTFIED WTH mysql_ native_password BY '新密码';
       ```

    4. 删除用户

       ```mysql
       DROP USER '用户名'@'主机名';
       ```

    ```mysql
    -- DCL
    -- 创建用户 itcast，只能够在当前主机localhost访问，密码123456;
    CREATE USER 'ITCAST'@'LOCALHOST' IDENTIFIED BY '123456';
    -- 创建用户 heima ，可以在任意主机访问该数据库，密码123456 ;
    CREATE USER 'heima'@'%' IDENTIFIED BY '123456';
    -- 修改用户 heima的访问密码为1234 ;
    ALTER USER 'heima'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
    -- 删除itcast@localhost用户
    DROP USER 'ITCAST'@'LOCALHOST' ;
    ```

  - DCL-权限控制

    MySQL中定义了很多种权限，但是常用的就以下几种:

    |        权限        |        说明        |
    | :----------------: | :----------------: |
    | ALL,ALL PRIVILEGES |      所有权限      |
    |       SELECT       |      查询数据      |
    |       INSERT       |      插入数据      |
    |       UPDATE       |      修改数据      |
    |       DELETE       |      删除数据      |
    |       ALTER        |       修改表       |
    |        DROP        | 删除数据库/表/视图 |
    |       CREATE       |   创建数据库/表    |

    1. 查询权限

       ```mysql
       show GRANTS FOR '用户名'@'主机名';
       ```

    2. 授予权限

       ```mysql
       GRANT 权限列表 ON 数据库名.表名 To '用户名'@'主机名';
       ```

    3. 撤销权限

       ```mysql
       REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
       ```

## 						3.函数

- 大纲
  1. 字符串函数
  2. 数值函数
  3. 日期函数
  4. 流程函数

- ##### 字符串函数

  MySQL中内置了很多字符串函数，常用的几个如下:

  |           函数           |                           功能                            |
  | :----------------------: | :-------------------------------------------------------: |
  |    CONCAT(S1,S2,.…Sn)    |        字符串拼接，将S1,S2,... Sn拼接成一个字符串         |
  |        LOWER(str)        |                  将字符串str全部转为小写                  |
  |        UPPER(str)        |                  将字符串str全部转为大写                  |
  |     LPAD(str,n,pad)      | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
  |     RPAD(str,n,pad)      | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
  |        TRIM(str)         |                去掉字符串头部和尾部的空格                 |
  | SUBSTRING(str,star1,len) |      返回从字符串sr从slar1位置起的len个长度的字符串       |

  ```mysql
  -- 字符串函数
  -- CONCAT(S1,S2,.…Sn)
  SELECT CONCAT('HELLO','WORLD');
  -- LOWER(str)
  SELECT LOWER('HELLO');
  -- UPPER(str)
  SELECT UPPER('HELLO');
  -- LPAD(str,n,pad)左填充，用字符串pad对str的左边进行填充，达到n个字符串长度
  SELECT LPAD('01',5,'-');
  
  SELECT RPAD('01',5,'-');
  -- SUBSTRING(str,star1,len)
  SELECT SUBSTRING('helloworld',6,5);
  /*
  ·根据需求完成以下SQL编写
  由于业务需求变更，企业员工的工号，统一为5位数，目前不足5位数的全部在前面补0。比如:1号员工的工号应该为00001
  */
  UPDATE emp set workno = LPAD(workno,5,'0');
  ```

- 数值函数

  常见的数值函数如下:

  |    函数    |               功能                |
  | :--------: | :-------------------------------: |
  |  CEIL(x)   |             向上取整              |
  |  FLOOR(x)  |             向下取整              |
  |  MOD(x,y)  |        返回x/y的模（取余）        |
  |  RAND()，  |         返回0~1内的随机数         |
  | ROUND(x.y) | 求参数x的四舍五入的值,保留y位小数 |

  ```mysql
  SELECT CEIL(1.1);# 2
  SELECT FLOOR(1.9); # 1
  SELECT MOD(3,2);# 1
  SELECT RAND();
  SELECT ROUND(1.546,2);# 1.55
  -- 通过数据库的函数，生成一个六位数的随机验证码。
  SELECT LPAD(ROUND(RAND()*1000000,0),6,0);
  ```

- 日期函数

  常见的日期函数如下:

  |                函数                 |                       功能                        |
  | :---------------------------------: | :-----------------------------------------------: |
  |              CURDATE()              |                   返回当前日期                    |
  |              CURTIME()              |                   返回当前时间                    |
  |                NOW()                |                返回当前日期和时间                 |
  |             YEAR(dale)              |                获取指定date的年份                 |
  |             MONTH(date)             |                获取指定date的月份                 |
  |              DAY(date)              |                获取指定date的日期                 |
  | DATE_ADD(date, INTFRVAl expr ltype) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
  |        DATEDIFF(date1,date2)        |    返回起始时间date1和结束时间date2之间的天数     |

- 流程函数

  流程函数也是很常用的一类函数，可以在SQL语句中实现条件筛选，从而提高语句的效率。

  |                             函数                             |                          功能                           |
  | :----------------------------------------------------------: | :-----------------------------------------------------: |
  |                      lF(value , t , f)                       |           如果value为true。则返回t。否则返回f           |
  |                   IFNULL(value1 , value2)                    |       如果value1不为空,返回value1，否则返回value2       |
  |    CASE WHEN [ vall1] THEN [res1] ...ELSE [ default ] END    |    如果val1为true。返回res1,...否则返回default默认值    |
  | CASE [ expr ] WHEN [ val1] THEN [res1] ...ELSE [ default ] END | 如果expr的值等于val1，返回res1,...否则返回default默认值 |

  

## 4.约束

- 大纲：
  1. 概述
  2. 约束演示
  3. 外键约束

概述：

1. 概念:约束是作用于表中字段上的规则，用于限制存储在表中的数据。
2. 目的:保证数据库中数据的正确、有效性和完整性。
3. 分类:

![image-20240621005519399](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621005519399.png)

​	注意:约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。

- 约束演示(auto_increment：自增)

  ![image-20240621010331717](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621010331717.png)

```mysql
-- 约束演示
CREATE TABLE user(
	id int PRIMARY KEY auto_increment COMMENT '主键',
	name varchar(10) not null unique COMMENT '姓名',
	age int CHECK(age > 0 && age <= 120 ) COMMENT '年龄',
	status char(1) default '1' comment '状态',
	gender char(1) comment '状态'
) COMMENT '用户表';
```

- 外键约束

  - 概念

    外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

  ![image-20240621011935174](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621011935174.png)

  - 语法

    - 添加外键

      ```mysql
      # 第一种方法
      CREATE TABLE表名{
      	字段名 数据类型,
      	...
      	[CONSTRAINT][外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
      	
      # 第二种方法
      ALTER TABLE  表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES    主表(主表列名);
      ```

      ```mysql
      -- 添加外键
      ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id);
      ```

    - 删除外键

      ```mysql
      ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
      ```

      ```mysql
      -- 删除外键
      alter table emp drop foreign key fk_emp_dept_id
      ```

  - 删除/更新行为

    ![image-20240621013805034](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621013805034.png)

    ```mysql
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段) REFERENCES 主表名(主表字段名）ON UPDATE CASCADE ON DELETE CASCADE;
                                                                        
    -- cascade : 级联（外键的删除和更新行为）
    ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id) on update cascade on delete cascade;
    -- set null                                                                    
    ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id) on update set null on delete set null;                                
    ```

    

## 5.多表查询

##### 多表关系

- 概述

  项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种:

  - 一对多(多对一)
  - 多对多
  - 一对一

- 一对多（多对一）

  案例:部门与员工的关系

  关系:一个部门对应多个员工，一个员工对应一个部门

  实现:**在多的一方建立外键，指向一的一方的主键**

- 多对多

  案例:学生与课程的关系

  关系:一个学生可以选修多门课程，一门课程也可以供多个学生选择

  实现:**建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**

- 一对一

  案例:用户与用户详情的关系

  关系;一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率

  实现:在任意一方加入外键，关联另外一方的主键，并且设置外键为**唯一的(UNIQUE)**

##### 多表查询

概述：指从多张表中查询数据

笛卡尔积：笛卡尔积是指在数学中，两个集合A集合和B集合的所有组合情况。**（在多表查询时，需要消除无效的笛卡尔积）**

```mysql
select * from emp,dept where emp.dept_id = dept.id;
```



##### 多表查询分类

- 连接查询

  1. 内连接:相当于查询A、B交集部分数据

  2. 外连接:
     		        左外连接:查询**左表**所有数据，以及两张表交集部分数据

     ​		右外连接:查询**右表**所有数据，以及两张表交集部分数据

  3. 自连接:当前表与自身的连接查询，自连接必须使用表别名

- 子查询

- 连接查询-内连接

  - 内连接查询语法:

    1. 隐式内连接

       ```mysql
       SELECT 字段列表 FROM 表1，表2 WHERE 条件...;
       ```

       ```mysql
       select emp.name,dept.name from emp, dept where emp.dept_id = dept.id;
       -- 使用别名
       select e.name,d.name  from emp e, dept d where e.dept_id = d.id;
       ```

    2. 显式内连接

       ```mysql
       SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件...;
       ```

       ```mysql
       -- 2．查询每一个员工的姓名，及关联的部门的名称（显式内连接实现)
       select e.name,d.name from emp e join dept d on e.dept_id = d.id;
       ```

  

**内连接查询的是两张表交集的部分**

- 连接查询-外连接

  外连接查询语法

  - 左外连接

    ```mysql
    # 相当于查询表1(左表)的所有数据包含表1和表2交集部分的数据
    SELECT字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件...;
    ```

    ```mysql
    -- 1．查询emp表的所有数据，和对应的部门信息(左外连接)
    select e.*,d.name from emp e left join dept d on  e.dept_id = d.id;
    ```

  - 右外连接

    ```mysql
    # 相当于查询表2(右表)的所有数据包含表1和表2交集部分的数据
    SELECT字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件...;
    ```

    ```mysql
    -- 2．查询dept表的所有数据，和对应的员工信息(右外连接)
    select d.*,e.name from dept d right join emp e  on d.id = e.dept_id;
    ```

- 连接查询-自连接**(起别名)**

  自连接查询语法

  ```mysql
  SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...;
  ```

  自连接查询，可以是内连接查询，也可以是外连接查询。

  ```mysql
  -- 自连接
  -- 1．查询员工及其所属领导的名字--表结构: emp
  select a.name,b.name from emp a, emp b where a.managerid = b.id
  -- 2．查询所有员工 emp及其领导的名字 emp ，如果员工没有领导，也需要查询出来
  select a.name,b.name from emp a left join  emp b on a.managerid = b.id
  ```

- 联合查询-union , union all

  对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

  ```mysql
  SELECT 字段列表 FROM 表A....
  UNION [ ALL ]
  SELECT 字段列表 FROM 表B .....
  ```

  ```mysql
  -- union all:直接将查询的结果合并
  -- 1．将薪资低于5000的员工，和年龄大于50岁的员工全部查询出来.
  select * from emp where salary < 5000
  union all
  select * from emp where age > 50
  -- union:将查询的结果进行去重
  select * from emp where salary < 5000
  union
  select * from emp where age > 50
  ```

  对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。

  union all会将全部的数据直接合并在一起，union会对合并之后的数据去重。



### 子查询

- 概念:SQL语句中嵌套SELECT语句，称为**嵌套查询**，又称**子查询**。

  ```mysql
  SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2 );
  ```

  子查询外部的语句可以是INSERT/ UPDATE/DELETE /SELECT的任何一个。

- **根据子查询结果不同，分为:**

  - 标量子查询（子查询结果为单个值〉
  - 列子查询(子查询结果为一列)
  - 行子查询(子查询结果为一行)
  - 表子查询(子查询结果为多行多列)

- 根据子查询位置，分为:WHERE之后、FROM之后、SELECT 之后。


1. 标量子查询

   子查询返回的结果是*单个值*（数字、字符串、日期等），最简单的形式，这种子查询成为标量子查询。

   常用的操作符:  =  <>   >     >=      <      <=

   ```mysql
   -- 标量子查询
   -- 1.查询"销售部”的所有员工信息
   -- a.查询“销售部”部门ID
   select id from dept where name = '销售部'
   -- b．根据销售部部门ID，查询员工信息
   select * from emp where dept_id = (select id from dept where name = '销售部');
   
   -- 2.查询在"方东白”入职之后的员工信息
   select entrydate from emp where name = '方东白'
   select * from emp where entrydate > '2009-02-12'
   select * from emp where entrydate > (select entrydate from emp where name = '方东白')
   
   ```

2. 列子查询

   子查询返回的结果是一列（可以是多行)，这种子查询称为**列子查询**。

   常用的操作符: IN . NOT IN ，ANY ， SOME ，ALL

![image-20240621101907184](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621101907184.png)

```mysql
-- 列子查询
-- 1．查询“销售部”和“市场部”的所有员工信息(使用 in)
-- a．查询“销售部”和“市场部”的部门ID
select * FROM dept where name in('销售部','市场部')
-- b．根据部门ID，查询员工信息
select * from emp where dept_id = 2 or dept_id = 4
-- 合并
select * from emp where dept_id in (select id FROM dept where name in('销售部','市场部'))

-- 2．查询比财务部所有人工资都高的员工信息(使用all)
select id from dept where name = '财务部'
select salary from emp where dept_id = (select id from dept where name = '财务部')

select * from emp where salary > all ( select salary from emp where dept_id = (select id from dept where name = '财务部'))

-- 3.查询比研发部其中任意一人工资高的员工信息(使用 any)
select salary from emp where dept_id = (select id from dept where name = '研发部')

select * from emp where salary > any (select salary from emp where dept_id = (select id from dept where name = '研发部'))
```

3. 行子查询

   子查询返回的结果是一行（可以是多列)，这种子查询称为**行子查询**。

   常用的操作符:= , <> , IN ，NOT IN

   ```mysql
   -- 行子查询
   -- 1．查询与“张无忌”的薪资及直属领导相同的员工信息;
   select salary,managerid from emp where name = '张无忌'
   select * from emp where (salary,managerid) = (12500,1)
   
   select * from emp where (salary,managerid) = (select salary,managerid from emp where name = '张无忌')
   ```

4. 表子查询

   子查询返回的结果是多行多列，这种子查询称为表子查询。

   常用的操作符:IN

   ```mysql
   -- 1．查询与“鹿杖客”，"宋远桥”的职位和薪资相同的员工信息
   select job,salary from emp where name = '鹿杖客' or name = '宋远桥'
   select * from emp where (job,salary) in (select job,salary from emp where name = '鹿杖客' or name = '宋远桥')
   
   -- 2．查询入职日期是"2006-01-01”之后的员工信息，及其部门信息(把查询出来当作一个临时表起别名使用)
   select * from emp where entrydate > '2006-01-01'
   select e.*,d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id=d.id;
   
   ```

   



## 6.事务

- 事务简介
- 事务操作
- 事务四大特性
- 并发事务问题
- 事务隔离级别

1. 事务简介

   事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提

   交或撤销操作请求，即这些操作**要么同时成功，要么同时失败。**

   默认MySQL的事务是自动提交的，也就是说，当执行一条DML语句，MySQL会立即隐式的提交事务:

2. 事务操作

   1.查看/设置事务提交方式

   ```mysql
   select @@autocommit;
   set @@autocommit = 0; -- 设置为手动提交
   ```

   提交事务

   ```mysql
   commit;
   ```

   回滚事务

   ```mysql
   rollback;
   ```

   ```mysql
   set @@autocommit = 0;# 手动提交事务
   update account set money = money - 1000 where name = '张三';
   update account set money = money + 1000 where name = '李四';
   commit;
   ```

   2.手动开启事务提交事务

   ```mysql
   -- 开启事务
   start TRANSACTION; // begin;
   update account set money = money + 1000 where name = '张三';
   update account set money = money - 1000 where name = '李四';
   
   commit;
   rollback;
   ```

3. **事务四大特性（面试题ACID）**

   - 原子性(Atomicity)∶事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
   - 一致性(Consistency):事务完成时，必须使所有的数据都保持一致状态。
   - 隔离性(lsolation):数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
   - 持久性( Durability):事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

4. 并发事务问题

![image-20240621152513511](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621152513511.png)

​	幻读：同一个事务，前后两次查询相同的范围时，得到的数据条数不一致

5. 事务的隔离级别

   ![image-20240621154205169](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621154205169.png)

   ```mysql
   --查看事务隔离级别
   SELECT @@TRANSACTION_ISQLATION;
   --设置事务隔离级别
   SET[SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMTTED | READ COMMITTED
   REPEATABLE READ |SERIALIZABLE}
   ```

   

#   				进阶篇

- 存储引擎
- 索引
- SQL优化
- 视图/存储过程/触发器
- 锁
- lnnoDB引擎
- MySQL管理



### 存储引擎

MySQL体系结构：连接层、服务层、引擎层、存储层

1. 存储引擎层

   - 简介

     存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型。

     1. 在创建表时，指定存储引擎

     ```mysql
     CREATE TABLE表名(
         字段1字段1类型[COMMENT 字段1 注释],
         ...
         字段n字段n类型[COMMENT 字段n注释]
         )ENGINE= INNODB [COMMENT 表注释];
     ```

     2. 查看当前数据库支持的存储引擎

        ```mysql
        SHOW ENGINES;
        ```

   - **InnoDB**

     - 介绍

       InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在MySQL 5.5之后，InnoDB是默认的 MySQL 存储引擎。

     - 特点

       1. DML操作遵循ACID模型，支持**事务**;
       2. **行级锁**，提高并发访问性能;
       3. 支持**外键FOREIGN KEY约束**，保证数据的完整性和正确性;

     - 文件

       xxx.ibd: xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm、sdi) 、数据和索引。参数: innodb_file_per_table

      ![image-20240621161519314](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621161519314.png)

   - MylSAM

     - 介绍

       MyISAM是MySQL早期的默认存储引擎。

     - 特点

       不支持事务，不支持外键

       支持表锁，不支持行锁

       访问速度快

   - Memory

     - 介绍

       Memory引擎的表数据时存储在内存中的，由于受到硬件问题、或断电问题的影响，只能将这些表作为临时表或缓存使用。

     - 特点

       内存存放

       hash索引（默认)

     - 文件

       Xxx.sdi:存储表结构信息

#### InnoDB 和 MylSAM 的区别（三个区别）（面试题）

![image-20240621162040291](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621162040291.png)

- 存储引擎选择

  在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

  - InnoDB:是Mysql的默认存储引擎，支持事务、外键。如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，那么InnoDB存储引擎是比较合适的选择。
  -  :如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择这个存储引擎是非常合适的。
  - MEMORY:将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中,而且无法保障数据的安全性。



### 索引

- 索引概述

- 索引结构
- 索引分类
- 索引语法

- SQL性能分析
- 索引使用
- 索引设计原则

1. 索引概述

   - 介绍

     索引（index）是帮助MySQL,**高效获取数据**的**数据结构(有序)**。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。

   - 优缺点

     - 优点：
       1. 提高数据检索的效率，降低数据库的IO成本
       2. 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗。
     - 缺点
       1. 索引列也是要占用空间的。
       2. 索引大大提高了查询效率，同时却也降低更新表的速度，如对表进行INSERT、UPDATE、DELETE时，效率降低。

2. 索引结构

   MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的结构，主要包含以下几种:

   ![image-20240621163604716](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621163604716.png)

   ![image-20240621163707900](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621163707900.png)

   - 二叉树

     ![image-20240621163945282](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621163945282.png)

   - B-Tree（**多路**平衡查找树）

     以一颗最大度数（max-degree)为5(5阶)的b-tree为例(每个节点最多存储4个key，5个指针):

     ![image-20240621164115913](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621164115913.png)

   - B+Tree

     以一颗最大度数（ max-degree）为4(4阶）的b+tree为例:

     ![image-20240621164733955](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621164733955.png)

     相对于B-Tree区别:
     ①. 所有的数据都会出现在叶子节点

     ②. 叶子节点形成一个单向链表

   - B+Tree

     MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能。

     ![image-20240621165303792](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621165303792.png)

   - Hash

     哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。

     如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

     - 特点
       1. Hash索引只能用于对等比较(=，in)，不支持范围查询（between，>，<，...）
       2. 无法利用索引完成排序操作
       3. 查询效率高，通常只需要一次检索就可以了，效率通常要高于B+tree索引

     存储引擎支持
     在MySQL中，支持hash索引的是Memory引擎，而InnoDB中具有自适应hash功能，hash索引是存储引擎根据B+ Tee索引在指定条件下自动构建的。

     **面试题：**

     为什么InnoDB存储引擎选择使用B+tree索引结构?

     1. 相对于二叉树，层级更少，搜索效率高;

     2. 对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，

        指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低;

     3. 相对Hash索引，B+tree支持范围匹配及排序操作;

3. 索引分类

   ![image-20240621171452760](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621171452760.png)

   在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种:

   ![image-20240621171649661](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621171649661.png)

   聚集索引选取规则;

   1. 如果存在主键，主键索引就是聚集索引。
   2. 如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引。
   3. 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引。

   **回表查询(涉及sql优化)**

   ![image-20240621172151734](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621172151734.png)

   二级索引下的叶子节点存放的是对应的主键值，将数据和索引分开存放

   聚集索引下的叶子节点存放的是行数据，将数据和索引放到了一起

4. 索引语法

   - 创建索引

     ```mysql
     CREATE [UNIQUE|FULLTEXT] INDEX index_name ON table_name (index_col_name...);
     ```

   - 查看索引

     ```mysql
     SHOW INDEX FROM table_name ;
     ```

   - 删除索引

     ```mysql
     DROP INDEX index_name ON table_name ;
     ```

5. **SQL性能分析（面试高频）**

   1. SQL执行频率

       MySQL客户端连接成功后，通过show [session l global]status命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次:

      ```mysql
      SHOW GLOBAL STATUS LIKE 'com_______';
      ```

   2. 慢查询日志

      慢查询日志记录了所有执行时间超过指定参数(long_query_time，单位:秒，默认10秒）的所有SQL语句的日志。

      MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件（/etc/my.cnf〉中配置如下信息:

      ```mysql
      # 开启MySQL慢日志查询开关
      slow_query _log=1
      # 设置慢日志的时间为2秒，SQI语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
      long_query_time=2
      ```

      配置完毕之后，通过以下指令重新启动MySQL服务器进行测试，查看慢日志文件中记录的信

      息/var/lib/mysql/locahost-slow.log.

   3. profile详情

      show profiles能够在做SQL优化时帮助我们了解时间都耗费到哪里去了。通过have_profiling参数，能够看到当前MySQL是否支持profile操作:

      ```mysql
      SELECT @@have_profiling ;
      ```

      默认profiling是关闭的，可以通过set语句在session/global级别开启profiling:

      ```mysql
      SET profiling= 1;
      ```

      执行一系列的业务SQL的操作，然后通过如下指令查看指令的执行耗时:

      ```mysql
      #查看每一条SQL的耗时基本情况
      show profiles;
      #查看指定query_id的SQL语句各个阶段的耗时情况
      show profile for query query_id;
      #查看指定query_id的SQL语句CPU的使用情况
      show profile cpu for query query_id;
      ```

   4. **explain执行计划**

      EXPLAIN或者DESC命令获取MySQL如何执行SELECT语句的信息，包括在SELECT语句执行过程中表如何连接和连接的顺序。
      语法:

      ```mysql
      # 直接在select语句之前加上关键字explain/desc
      EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件;
      ```

      ![image-20240621181913275](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240621181913275.png)

      EXPLAIN 执行计划各字段含义:

      - id 

        select查询的序列号，表示查询中执行select子句或者是操作表的顺序(id相同，执行顺序从上到下; 

        id不同，值越大，越先执行)。

      - select_type

        表示SELECT的类型，常见的取值有SIMPLE〈简单表，即不使用表连接或者子查询）、PRIMARY(主查询，即外层的查询)、UNION(UNION中的第二个或者后面的查询语句)、SUBQUERY (SELECT/WHERE之后包含了子查询）等

      - type

        表示连接类型，性能由好到差的连接类型为NULL、system、const、eq_ref、ref、range、index、all 。

      - possible_key

        显示可能应用在这张表上的素引，一个或多个。

      - key

        实际使用的索引，如果为NULL，则没有使用索引。

      - Key_len

        表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

      - rows

        MySQL认为必须要执行查询的行数，在innodb引擎的表中，是一个估计值，可能并不总是准确的。

      - filtered

        表示返回结果的行数占需读取行数的百分比,filtered的值越大越好。

6. 索引使用

   - 验证索引效率

     在未建立索引之前，执行如下SQL语句，查看SQL的耗时。

     ```mysql
     SELECT * FROM tb_sku WHERE Sn = '100000003145001 ';
     ```

     针对字段创建索引

     ```mysql
     create index idx_sku_sn on tb_sku(sn) ;
     ```

     然后再次执行相同的SQL语句，再次查看SQL的耗时。

   - 最左前缀法则

     如果索引了多列(联合索引)，要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。只要查询语句中存在索引中的列就满足最左前缀法则，位置顺序不重要。

     **如果跳跃某一列，索引将部分失效(后面的字段索引失效)。**

     ```mysql
      explain select * from tb_user where profession= '软件工程' and age =31 and status = '0';
     explain select * from tb_user where profession= '软件工程' and age = 31;
     explain select * from tb_user where profession= '软件工程';
     explain select * from tb_user where age= 31 and status = '0';
     explain select * from tb_user where status = '0';
     ```
     
   - 范围查询

     联合索引中，出现范围查询(>,<)，**范围查询右侧（不包括范围查询）的列索引失效**

     ```mysql
    explain select * from tb_user where profession= '软件工程' and age > 30 and status = '0';
     ```
   
     . >= ，<= 可以避免这个方法

     ```mysql
    explain select * from tb_user where profession='软件工程' and age >=  30 and status = '0';
     ```
   
   - 索引列运算

     不要在索引列上进行运算操作，**索引将失效**。

     ```mysql
    explain select * from tb_user where substring(phone,10,2) = '15';
     ```
   
   - 字符串 

     字符串类型字段使用时，不加引号，**索引将失效。**

   - 模糊查询

     如果仅仅是尾部模糊查询，索引不会失效 。如果是头部模糊匹配，索引失效。

     要规避这种头部模糊匹配，索引失效的情况，因为是进行全表扫描。

   - or连接的条件

     用or分割开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。

     由于age没有索引，所以即使id、phone有索引，索引也会失效。所以需要针对于age也要建立索引。

   - 数据分布影响 

     如果MySQL评估使用索引比全表更慢，则不使用索引。

   - SQL提示 

     SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

     use index（建议）:

     ```mysql
    explain select * from tb_user upe index(idx_user_pro) where profession= '软件工程";
     ```
   
     ignore index:

     ```mysql
    explain select * from tb_user ignore index(idx_user_pro) where profession='软件工程';
     ```
   
     force index（强制）:

     ```mysql
    explain select * from tb_user force index(idx_user_pro) where profession='软件工程";
     ```
   
   - 覆盖索引

     尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到)，减少

     select *。

     划重点：查询时不要随便用select*，因为这样容易回表查询，耗损资源和降低效率，除非你已将建立了一个该表所有字段的联合索引。

     ```mysql
    explain select id, profession from tb_user where profession='软件工程' and age = 31 and status ='0' ;
     
     explain select id profession,age, staltus frorn tb_user where profession ='软件工程' and age=31 and status = '0' ;
     
     explain select id ,profession ,age,status, name from tb_user where profession='软件工程' and age=31 and status = '0';
     
     explain select * from tb_user where profession ="软件工程' and age = 31 and status = '0 ;
     ```
   
     知识小贴士:

     using index condition :查找使用了索引,但是需要回表查询数据

     using where; using index:查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表

     查询数据。

   - 前缀索引

     当字段类型为字符串(varchar, text等）时，有时候需要索引*很长的字符串*，这会让索引变得很大，查

     询时，浪费大量的磁盘lO，影响查询效率。此时可以只将字符串的一部分前缀，建立索引，这样可以

     大大节约索引空间，从而提高索引效率。

     - 语法：

     ```mysql
    create index idx_xxxx on table_name(column(n)) ;
     ```
   
     -  前缀长度

       可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高，唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

     ```mysql
    select count(distinct email) / count() from tb_user ;
     select count(distinct substring(email, 1,5)) / count(*) from tb_user ;
     ```
   
   - 单例索引与联合索引

     - 单列索引:即一个索引只包含单个列。
  - 联合索引:即一个索引包含了多个列。
   
     在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。

     单列索引情况:

     ```mysql
    explain select id, phone, name from tb_user where phone = '17799990010' and name = ‘韩信';
    ```
   
     **多条件联合查询时，MySQL优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询。**

7. 索引设计原则
   1. 针对于数据量较大，且查询比较频繁的表建立索引。
   2. 针对于 常作为查询条件(where) 、排序(orderby) .分组(group by)操作的字段建立索引。
   3. 尽量选择区分度高的列作为索引， 尽量建立唯一索引，区分度越高,使用索引的效率越高。
   4. 如果是字符串类型的字段, 字段的长度较长，可以针对于字段的特点，建立前缀索引。
   5. 尽量使用联合索引， 减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率。
   6. 要控制索引的数量,索引并不是多多益善，索引越多,维护索引结构的代价也就越大，会影响增删改的效率。
   7. 如果索引列不能存储NULL值,请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询。



### SQL优化

1. 插入数据
2. 主键优化
3. order by优化
4. group by优化
5. limit优化
6. count优化
7. update优化



- insert 优化

  - 批量插入

    ```mysql
    Insert into tb_test values(1,'Tam"),(2,'Cat').(3,'Jerry');
    ```

  - 手动提交事务

    ```mysql
    start transaction;
    insert into tb_test values(1,'Tom"),(2,'Cat'),(3.,"Jerry');
    insert into tb_test values(4,'Tom'),(5,'Cat'),(6.'Jerry');
    insert into tb_test values(7,'Tom'),(8,'cat'),(9.'Jerry');
    commit;
    ```

  - 主键顺序插入

  - ##### 大批量插入数据

  如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。操作如下:

  ```mysql
  #客户端连接服务端时，加上参数--local-infile
  mysql --local-infile -u root -p
  #设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
  set global local_infile= 1;
  #执行load指令将准备好的数据，加载到表结构中
  load data local infile '/root/sql1.log' into table 'tb_user’ fields terminated by ',' lines terminated by '\n' ;
  ```

  主键顺序插入性能高于乱序插入



- 主键优化

  - 数据组织方式

    在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表(index organized table lOT)。

  - 页分裂
    页可以为空，也可以填充一半，也可以填充100%。每个页包含了2-N行数据(如果一行数据多大，会行溢出)，根据主键排列。

  - 页合并

    当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged)为删除并且它的空间变得允许被其他记录声明使用。

    当页中删除的记录达到MERGE_THRESHOLD(默认为页的50%)，InnoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。

    MERGE_THRESHOLD:合并页的阈值，可以自己设置，在创建表或者创建索引时指定。

  - 主键设计原则

    1. 满足业务需求的情况下，尽量降低主键的长度。
    2. 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
    3. 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
    4. 业务操作时，避免对主键的修改。

- order by优化（order by必须有顺序，使用最左前缀法则）

  1. Using filesort :通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫FileSort 排序。
  2. Using index:通过有序索引顺序扫描直接返回有序数据，这种情况即为using index，不需要额外排序，操作效率高。

  - 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。
  - 尽量使用覆盖索引。
  - 多字段排序,一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC)。
  - 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size(默认256k)。

- group by 优化

  1. 在分组操作时，可以通过索引来提高效率。
  2. 分组操作时,索引的使用也是满足最左前缀法则的。

- limit优化

  优化思路:一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以通过**覆盖索引加子查询形式**进行优化。

  ```mysql
  explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;
  ```

- count 优化

  1. MyISAM引擎把一个表的总行数存在了磁盘上，因此执行count(*)的时候会直接返回这个数，效率很高;
  2. InnoDB引擎就麻烦了，它执行count(*)的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。

  优化思路:自己计数。

  - count的几种用法
    count()是一个聚合函数，对于返回的结果集，一行行地判断，如果count 函数的参数不是NULL，累计值就加1，否则不加，最后返回累计值。

  - 用法: count (*) ，count（主键)，count（字段)，count (1)

    - count（主键)
      InnoDB引擎会遍历整张表，把每一行的主键id值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加(主键不可能为null)。

    - count（字段)

      没有not null约束: InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加。有not null约束: InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。

    - count(1)
      InnoDB引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”进去，直接按行进行累加。

    - count (*)

      InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。

    按照效率排序的话，count(字段)< count(主键id)<count(1) = count( * )，所以尽量使用count(*)。

- update优化

  InnoDB的行锁是针对索引加的锁，不是针对记录加的锁,并且该索引不能失效，否则会从行锁升级为表锁，降低数据库的并发访问性能。



### 视图/存储过程/触发器

1. 视图
2. 存储过程
3. 存储函数
4. 触发器



#### 视图

- 介绍

  视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

  通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

- 创建

  ```mysql
  CREATE [OR REPLACE] VIEW 视图名称 (列名列表)] AS SELECT语句 [WITH [ CASCADED | LOCAL ] CHECK OPTION]
  ```

  ```mysql
  -- 创建视图
  create or replace view stu_v_1 as select id,name from student where id <= 10;
  ```

- 查询

  ```mysql
  查看创建视图语句: SHOW CREATE VIEW 视图名称;
  查看视图数据: SELECT * FROM 视图名称....... ;
  ```

  ```mysql
  -- 查询视图
  show create view stu_v_1;
  
  select * from stu_v_1;
  ```

- 修改

  ```mysql
  方式一:CREATE [OR REPLACE] VIEW 视图名称(列名列表)] AS SFIECT 语句[WITH [ CASCADED | LOCAL ] CHECK OPTION]
  方式二:ALTER VIEW 视图名称[(列名列表)]AS SELECT语句[WITH [ CASCADED | LOCAL ] CHECK OPTION ]
  ```

  ```mysql
  -- 修改视图
  create or replace view stu_v_1 as select id,name,no from student where id <= 10;
  
  alter view stu_v_1 as select id,name from student where id <= 10;
  ```

- 删除

  ```mysql
  DROP VIEW [IF EXISTS] 视图名称 [视图名称].......
  ```

  ```mysql
  drop view if exists stu_v_1;
  ```

- 视图的检查选项

  当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如插入，更

  新，删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则

  以保持一致性。为了确定检查的范围，mysql提供了两个选项;CASCADED 和LOCAL，默认值为CASCADED

  CASCADED:

  ```mysql
  -- cascaded
  create or replace view stu_v_1 as select id,name from student where id <= 20;
  create or replace view stu_v_2 as select id,name from stu_v_1 where id >= 10 with cascaded check option;
  ```

  LOCAL:

  递归我们当前视图和所依赖的视图是否定义了检查选项，如果定义了，则进行条件的检查。如果没有，则不进行。

- 视图的更新

  要使视图可更新，视图中的行与基础表中的行之间必须存在**一对一**的关系。如果视图包含以下任何一项，则该视图不可更新;

  1. 聚合函数或窗口函数(SUM()、MIN()、MAX()、COUNT()等)
  2. DISTINCT
  3. GROUP BY
  4. HAVING

  5. UNION或者UNION ALL

- 作用
  1. 简单
     视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。
  2. 安全
     数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据
  3. 数据独立
     视图可帮助用户屏蔽真实表结构变化带来的影响。
  4. 

#### 存储过程

- 介绍

  存储过程是事先经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程可以简化应用开发人员

  的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

  存储过程思想上很简单，就是数据库SQL语言层面的代码封装与重用。

- 特点

  封装，复用

  可以接收参数，也可以返回数据

  减少网络交互，效率提升

- ##### 基本语法

1. 创建

   ```mysql
   CREATE PROCEDURE 存储过程名称([参数列表])
   BEGIN
   		-- SQL语句
   END ;
   ```

2. 调用

   ```mysql
   call 名称 ([参数]);
   ```

3. 查看

   ```mysql
   SELECT * FRDM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SQHEMA='xxx ';--查询指定数据库的存储过程及状态信息
   SHOW CREATE PROCEDURE 存储过程名称;--查询某个存储过程的定义
   ```

4. 删除

   ```mysql
   drop procedure [if exists] 存储过程名称;
   ```

   ```mysql
   -- 存储过程基本语法
   -- 创建
   create procedure p1()
   begin
   	select count(*) from stu;
   end;
   -- 调用
   call p1();
   -- 查看
   select * from information_schema.ROUTINES where routine_schema = 'itcast';
   show create procedure p1;
   -- 翻除
   drop procedure if exists p1;
   ```

   注意:在命令行中，执行创建存储过程的SQL时，需要通过关键字delimiter指定SQL语句的结束符。

- ##### 变量

  1. 系统变量

     系统变量是MySQL服务器提供，不是用户定义的，属于服务器层面。分为全局变量（GLOBAL)、会话变量(SESSION).

  - 查看系统变量

    ```mysql
    SHOW [SESSION | GLOBAL] VARIABLES ; -- 查看所有系统变量
    SHOW [SESSION | GLOBAL] VARIABLES LIKE '...';-- 可以通过LIKE模糊匹配方式查找变量
    SELECT @@[SESSION | GLOBAL]系统变量名; -- 查看指定变量的值
    ```

  - 设置系统变量

    ```mysql
    SET [SESSION|GLOBAL]系统变量名 = 值;
    SET @@[SESSION|GLOBAL]系统变量名 = 值;
    ```

    注意:
    如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。

    mysql服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在/etc/my.cnf 中配置。

  

  2. 用户定义变量

     用户定义变量是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用“@变量

     名”使用就可以。其作用域为当前连接。

  - 赋值

    ```mysql
    SET @var_name = expr [, @var_name = expr] ...;
    SET @var_name := expr [, @var_name ;= expr] ...;
    SELECT @var_name := expr [, @var_name := expr ] ... ;
    SELECT 字段 INTO @var_name FROM 表名;
    ```

  - 使用

    ```mysql
    SELECT @var_name ;
    ```

    ```mysql
    -- 变量:用户变量
    -- 赋值
    set @myname = 'hyx';
    set @myage := 23;
    set @mygender := '男'
    
    select @mycolor := 'red';
    select count(*) into @mycount from tb_user;
    -- 使用
    select @myname,@myage,@mygender;
    select @mycolor,@mycount
    ```

    注意:

    用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL.

  3. 局部变量

     局部变量是根据需要定义的在局部生效的变量，访问之前，需要DECLARE声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN ...END块。

  - 声明

    ```mysql
    DECLARE 变量名 变量类型 [DEFAULT ...];
    ```

    变量类型就是数据库字段类型:INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。

  - 赋值

    ```mysql
    SET变量名=值;
    SET 变量名:=值;
    SELECT 字段名 INT 变量名 FROM 表名...;
    ```

- if

  语法：

  ```mysql
  IF 条件1 THEN
  	...
  ELSEIF 条件2 THEN -- 可选
  	...
  ELSE			 -- 可选
  	...
  END IF;
  ```

- 参数

  ![image-20240622095840231](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622095840231.png)

  用法

  ```mysql
  CREATE PROCEDURE 存储过程名称 ([IN/OUT/INOUT 参数名 参数类型])
  BEGIN
      --SQL语句
  END ;
  ```

  ```mysql
  /*
  --根据传入(in)参数score，判定当前分数对应的分数等级，并返回(out)。
  1. score >=85分，等级为优秀。
  2.score >= 60分且score <85分，等级为及格。
  3. score <60分，等级为不及格。
  */
  create procedure p4(in score int,out result varchar(10))
  begin
  	if score >= 85 then
  		set result := '优秀';
  	elseif score >=60 then
  		set result := '及格';
  	else 
  		set result := '不及格';
  	end if;
  end;
  
  call p4(99,@result);
  select @result;
  
  
  create procedure p5(inout score double)
  begin
  	set score := score * 0.5;
  end;
  
  set @score = 70;
  call p5(@score);
  select @score;
  ```

- case

  语法一

  ```mysql
  CASE case_value
  	WHEN when_value1 THEN statement_list1
  	[WHEN when_value2 THEN statement_list 2] ...
  	[ ELSE statement_list ]
  END CASE;
  ```

  语法二

  ```mysql
  CASE
  	WHEN search_condition1 THEN statement_list1
  	[WHEN search_condition2 THEN statement_list2] ...
  	[ELSE statement_list]
  END CASE;
  ```

- while

  while 循环是有条件的循环控制语句。满足条件后，再执行循环体中的SQL语句。具体语法为:

  ```mysql
  #先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
  WHILE 条件 DO
  	SQL逻辑...
  END WHILE;
  ```

- repeat（类似于Java中的do...while...）

  repeat是有条件的循环控制语句,当满足条件的时候退出循环。具体语法为:

  ```mysql
  #先执行一次逻辑，然后判定逻辑是否满足，如果满足，则退出。如果不满足，则继续下一次循环
  REPEAT
  	SQL 逻辑...
  	UNTIL 条件
  END REPEAT;
  ```

- loop

  LOOP实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合一下两个语句使用:

  - LEAVE:配合循环使用，退出循环。 (break)
  - ITERATE:必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环(continue)

  ```mysql
  [begin_label:] LOOP
  	SQL逻辑...
  END LOOP[end_label];
  
  LEAVE label; -- 退出指定标记的循环体
  ITERATE label; -- 直接进入下一次循环
  ```

  ```mysql
  create procedure p9(in n int)
  begin
  	declare total int default 0;
  	
  	sum : loop
  		if n<=0 then
  			leave sum;
  		end if;
      	set total := total + n;
  		set n := n - 1;
  	end loop sum;
  	select total;
  end;
  ```

- 游标 cursor

  游标(CURSOR）是用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环的处理。游标的使用包括游标的声明、OPEN、FETCH和 CLOSE，其语法分别如下。

  声明游标

  ```mysql
  DECLARE 游标名称 CURSOR FOR 查询语句;
  ```

  打开游标

  ```mysql
  OPEN 游标名称;
  ```

  获取游标记录

  ```mysql
  FETCH 游标名称 INTO 变量[变量];
  ```

  关闭游标

  ```mysql
  CLOSE 游标名称;
  ```

- 条件处理程序

  条件处理程序（Handler)可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。具体语法为:

  ![image-20240622104525806](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622104525806.png)

  ```mysql
  -- 游标
  -- 根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名(name）和专业(profession ) ，
  -- 并将用户的姓名和专业插入到所创建的一张新表(id , name , profession)中。
  -- 逻辑:
  -- A．声明游标，存储查询结果集
  -- B．准备:创建表结构
  -- C．开启游标
  -- D．获取游标中的记录
  -- E．插入数据到新表中
  -- F．关闭游标
  
  create procedure p11(in uage int)
  begin 
  	declare uname varchar(100);
  	declare upro varchar(100);
  	declare u_cursor cursor for select name,profession from tb_user where age <= uage;
  	# declare exit handler for SQLSTATE '02000' close u_cursor;
  	declare exit handler for SQLSTATE not found close u_cursor;
  
  	drop table if exists tb_user_pro;
  	create table if not exists tb_user_pro(
  			id int primary key auto_increment,
  			name varchar(100),
  			profession varchar(100)
  	);
  	
  	open u_cursor;
  	
  	while true do 
  			fetch u_cursor into uname,upro;
  			insert into tb_user_pro values (null,uname,upro);
  	end while;
  	
  	close u_cursor;
  end;
  
  call p11(30)
  
  select * from tb_user_pro;
  ```

  

##### 存储函数(使用较少)

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。具体语法如下:

```mysql
CREATE FUNCTION 存储函数名称([参数列表])
-- 指定返回值的类型
RETURNS type [characteristic ...] 
BEGIN
	-- SQL语句
	RETURN ...;
END;

characteristic说明:
. DETERMINISTIC:相同的输入参数总是产生相同的结果
. NO SQL:不包含SQL语句。
. READS SQL DATA:包含读取数据的语句，但不包含写入数据的语句。
```

```mysql
-- 存储函数
create function fun1(n int)
returns int DETERMINISTIC
begin
	declare total int default 0;
	
	while n>0 do
		set total := total + n;
		set n := n - 1;
	end while;
	return total;
end;

SELECT fun1(20);
```



##### 触发器

- 介绍

  触发器是与表有关的数据库对象，指在insert/update/delete 之前或之后，触发并执行触发器中定义的SQL

  语句集合。触发器的这种特性可以协助应用在数据库端*确保数据的完整性，日志记录，数据校验*等操作。

  使用别名OLD和NEW来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在**触发器还只**

  **支持行级触发**，不支持语句级触发。

  ![image-20240622110301332](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622110301332.png)

- 语法

  - 创建

    ```mysql
    CREATE TRIGGER trigger_name
    BEFORE/AFTER INSERT/UPDATE/DELETE ON tbl_name FOR EACH ROW --行级触发器
    BEGIN
    	trigger_stmt ;
    END;
    ```

  - 查看

    ```mysql
    show triggers;
    ```

  - 删除

    ```mysql
    DROP TRIGGER [schema_name.] trigger_name ;
    -- 如果没有指定 schema_name，默认为当前数据库。
    ```

  ```mysql
  -- 插入数据触发器, UPDATE 型触发器,DELETE型触发器类似
  create trigger tb_user_insert_trigger
  	after insert on tb_user for each row
  begin
  	insert into user_logs(id,operation,operate_time,operate_id,operate_params) values
  	(null,'insert',NOW(),new.id,concat('插入的数据为id=' , new .id , ' , name=' , new.name,', phone= ' ,new.phone,' , email= ' , NEW.email,', profession=', new.profession));
  end;
  
   -- 查看触发器
   show TRIGGERS;
   -- 删除触发器
  drop triggers tb_user_insert_trigger;
  ```

  

# 锁

- 介绍

  锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源（CPU、RAM、I/0)的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。

- 分类

  MySQL中的锁，按照锁的**粒度**分，分为以下三类:

  1. 全局锁:锁定数据库中的所有表。
  2. 表级锁:每次操作锁住整张表。
  3. 行级锁:每次操作锁住对应的行数据。

##### 全局锁

- 介绍

  MySQL全局锁是针对整个数据库的锁。最常用的全局锁是读锁和写锁。

  全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，

  已经更新操作的事务提交语句都将被阻塞。

  其典型的***使用场景***是做**全库的逻辑备份**，对所有的表进行锁定，从而获取一致性视图，保证数据的完整

  性。   

  ```mysql
  -- 加锁
  flush tables with read lock;
  ```

  ```mysql
  -- 数据备份
  mysqldump -uroot -p123456 itcast > itcast.sql;
  ```

  ```mysql
  -- 解锁
  unlock tables;
  ```

- 特点

  数据库中加全局锁，是一个比较重的操作，存在以下问题:

  1. 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得**停摆**。
  2. 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志（binlog)，会导致**主从延迟。**

  在InnoDB引擎中，我们可以在备份时加上参数--single-transaction 参数来完成*不加锁的一致性数据备份*。

  ```mysql
  mysqldump --single-transaction -uroot -p123456 itcast > itcast.sql
  ```

##### 表级锁

- 介绍

  表级锁，每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、InnoDB、BDB等存储引擎中。

  对于表级锁，主要分为以下三类:

  1. 表锁
  2. 元数据锁（ meta datalock，MDL)
  3. 意向锁

  - 表锁

    对于表锁，分为两类：

    1. 表共享读锁（read lock）
    2. 表独占写锁（write lock）

    语法：

    1. 加锁：lock tables 表名...   read/write

       ```mysql
       lock TABLES employee read;
       show OPEN TABLES where In_use > 0;
       UPDATE employee set name ='张三' WHERE id = 1;
       unlock tables;
       ```

    2. 释放锁：unlock tables / 客户端断开连接

    **读锁不会阻塞其他客户端的读，但是会阻塞写。**

    **写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写。**

  - 元数据锁（ meta datalock，MDL)

    MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是

    维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。**为了避免**

    **DML与DDL冲突，保证读写的正确性。**

    在MySQL5.5中引入了MDL，**当对一张表进行增删改查的时候，加MDL读锁(共享)**；**当对表结构进行变更操作的时候，加MDL写锁(排他)**。

    ![image-20240622133026098](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622133026098.png)

    查看元数据锁:

    ```mysql
    select object type,object_schema,object_name,Jock_ type.lock duration from performance_schema.metadata_locks ;
    ```

  - 意向锁

    为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。会自动添加意向锁。

    分类：

    1. 意向共享锁（lS):由语句select ... lock in share mode添加。
    2. 意向排他锁（IX)∶由insert、update、delete、select ... for update添加。
    
    兼容性：

    1. 意向共享锁（lS)︰与表锁共享锁( read）兼容，与表锁排它锁（write)互斥。
    2. 意向排他锁（lX)︰与表锁共享锁（read)及排它锁(write）都互斥。意向锁之间不会互斥。
    
    可以通过以下SQL，查看意向锁及行锁的加锁情况:

    ```mysql
    select object schema,object_name,index_name,Jock_type,lock_mode,lock_data from performance schema.data_locls;
    ```

##### 行级锁

- 介绍

  行级锁，每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中。

  InnoDB的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类:

  1. 行锁（Record Lock)︰锁定单个行记录的锁，防止其他事务对此行进行update和delete。在RC、RR隔离级别下都支持。

  2. 间隙锁(Gap Lock)∶锁定索引记录间隙（不含该记录)，确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下都支持。

  3. 临键锁（Next-Key Lock)∶行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。在RR隔离级别下支持。

  - 行锁

    InnoDB实现了以下两种类型的行锁:

    1. 共享锁(S)∶也称为读锁，允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁。
    2. 排他锁(X)∶也称为写锁，允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。
  
    ![image-20240622141806281](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622141806281.png)

    - 行锁-演示

      默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，**InnoDB使用next-key锁进行搜索和索引扫描，以防止幻读。**

      1. 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。

      2. InnoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，**此时就会升级为表锁。**

    
  ```mysql
    -- 可以通过以下SQL，查看意向锁及行锁的加锁情况:
    select object_Schema,pbject_name,jindex_name,lock_type,ock_mode,lock data from perfarmance _schema.data _locks;
  ```
  
  - 间隙锁/临键锁-演示
  
  加间隙锁可以防止幻读，它是将插入不存在的区域时，将区域加锁。
  
  默认情况下，InnoDB在REPEATABLE READ事务隔离级别运行，InnoDB使用临键锁进行搜索和索引扫描，以防止幻读。
  
  1. 索引上的等值查询(唯一索引)，给不存在的记录加锁时,优化为间隙锁。
    2. 索引上的等值查询(普通索引)，向右遍历时最后一个值不满足查询需求时,next-key lock退化为间隙锁。
    3. 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。
  
    注意:间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。
  




### InnoDB引擎（了解）

1. 逻辑存储结构
2. 架构
3. 事务原理
4. MVCC



#### 逻辑存储结构（表空间-段-区-页-行）

![image-20240622151500484](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622151500484.png)

表空间（ibd文件)，一个mysql实例可以对应多个表空间，用于存储记录、索引等数据。

段，分为数据段(Leaf node segment)、索引段(Non-leaf node segment)、回滚段（Rollback segment)，InnoDB是索引组织表，数据段就是B+树的叶子节点，索引段即为B+树的非叶子节点。段用来管理多个Extent(区)

区，表空间的单元结构，每个区的大小为1M。默认情况下，InnoDB存储引擎页大小为16K，即一个区中一共有64个连续的页。

页，是InnoDB存储引擎磁盘管理的最小单元，每个页的大小默认为16KB。为了保证页的连续性，InnoDB存储引擎每次从磁盘申请4-5个区。

行，InnoDB存储引擎数据是按行进行存放的。



#### 架构

MySQL5.5版本开始，默认使用InoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常

广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构。

![image-20240622152309967](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622152309967.png)

##### 内存结构

- Buffer Pool：缓冲池是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存)，然后再以一定频率刷新到磁盘，从而减少磁盘IO，加快处理速度。
- 缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型:.
  - free page:空闲page，未被使用。
  - clean page:被使用page，数据没有被修改过。
  - dirty page:脏页，被使用page，数据被修改过，也中数据与磁盘的数据产生了不一致。

- Change Buffer:更改缓冲区（针对于非唯一二级索引页)，在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区Change Buffer中，在未来数据被读取时，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。

- Adaptive Hash Index：自适应hash索引，用于优化对Buffer Pool数据的查询。InnoDB存储引擎会监控对表上各索引页的查询，如果观察到hash索引可以提升速度，则建立hash索引，称之为自适应hash索引

  **自适应哈希索引，无需人工干预，是系统根据情况自动完成。**

  参数: adaptive_hash_index

  

- Log Buffer:日志缓冲区，用来保存要写入到磁盘中的log日志数据（redg log , undo log)，默认大小为16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘I/O。

  参数:

  innodb_log_buffer_size:缓冲区大小

  innodb_flush_log_at_trx_commit:日志刷新到磁盘时机



##### 磁盘结构

- System Tablespace：系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)

  参数:innodb_data_file_path

- File-Per-Table Tablespaces:每个表的文件表空间包含单个InnoDB表的数据和索引，并存储在文件系统上的单个数据文件中。

  参数:innodb_file_per_table

- General Tablespaces:通用表空间，需要通过CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间。

- Undo Tablespaces:撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M),用于存储undo log日志。

- Temporary Tablespaces: InnoDB使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。

- Doublewrite Buffer Files:双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。

- Redo Log:重做日志，是用来实现事务的持久性。该日志文件由两部分组成:重做日志缓冲(redo log buffer）以及重做日志文件(redo log),前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中,用于在刷新脏页到磁盘时,发生错误时,进行数据恢复使用。

##### 后台线程

1. Master Thread
   核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中，保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收。
2. IO Thread
   在InnoDB存储引擎中大量使用了AIO来处理IO请求,这样可以极大地提高数据库的性能，而IO Thread主要负责这些IO请求的回调。
3. Purge Thread
   主要用于回收事务已经提交了的undo log，在事务提交之后，undo log可能不用了，就用它来回收。
4. Page Cleaner Thread
   协助Master Thread 刷新脏页到磁盘的线程，它可以减轻Master Thread的工作压力，减少阻塞。



#### 事务原理

- 事务

  事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撒销操作请求，即这些操作要么同时成功，要么同时失败。

- 特性

  - 原子性(Atomicity):事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
  - 一致性(Consistency) :事务完成时，必须使所有的数据都保持一致状态。
  - 隔离性(lsolation)︰数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
  - 持久性(Durability):事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

![image-20240622160240155](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622160240155.png)

- redo log（持久性）

  重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。

  该日志文件由两部分组成：重做日志缓冲(redo log buffer)以及重做日志文件(redo log file) ,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中,用于在刷新脏页到磁盘,发生错误时,进行数据恢复使用。

- undo log (原子性)

  回滚日志，用于记录数据被修改前的信息，**作用包含两个:提供回滚和MVCC(多版本并发控制)**。

  undo log和redo log记录物理日志不一样，它是**逻辑日志**。可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

  Undo log销毁: undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。

  Undo log存储: undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。

- MVCC + 锁 （隔离性）

- redo  log + undo log 共用保证（一致性）



### MVCC多版本并发控制（高频面试题！！！）

- 当前读

  读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。

  对于我们日常的操作，如:select ... lock in share mode(共享锁)，select ... for update、update、insert、

  delete(排他锁)都是一种当前读。

- 快照读

  简单的select(不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加

  锁，是非阻塞读。

  - Read Committed:每次select，都生成一个快照读。
  - Repeatable Read:开启事务后第一个select语句才是快照读的地方。
  - Serializable:快照读会退化为当前读。

- MVCC

  全称Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的**三个隐式字段、undo log日志、readView.**


##### MVCC-实现原理

- 记录中的隐藏字段

| 隐藏字段    | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| DB_TRX_ID   | 最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID。 |
| DB_ROLL_PTR | 回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版本。 |
| DB_ROW_ID   | 隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段。       |

- undo log 日志

  回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。

  当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。

  而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除。

- undo log **版本链**

  不同事务或相同事务对同一条记录进行修改，会导致该记录的undo log生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。

- readview

  ReadView(读视图）是快照读SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务（未提交的) id。

  ReadView中包含了四个核心字段:

  ![image-20240622163325914](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622163325914.png)

  ![image-20240622163531551](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622163531551.png)

  不同的隔离级别，生成Readview的时机不同:

  - **READ COMMITTED∶在事务中每一次执行快照读时生成Readview。**

    **实现原理：**

    ![image-20240622164237831](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622164237831.png)

  - **REPEATABLE READ:仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。**

    **实现原理：**

    ![image-20240622165326522](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622165326522.png)



#### MySQL管理

- 系统数据库

  Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下:

  ![image-20240622171707880](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622171707880.png)

- 常用工具

  该mysql不是指mysql服务，而是指mysql的客户端工具

  ![image-20240622171827625](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622171827625.png)

  -e选项可以在Mysql客户端执行sQL语句，而不用连接到MysQL数据库再执行，对于一些批处理脚本，这种方式尤其方便。

  ```mysql
  mysql -uroot -p123456 db01 -e "select * from stu";
  ```

  ![image-20240622172213545](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622172213545.png)





## MySQL运维篇

1. 日志
2. 主从复制
3. 分库分表
4. 读写分离



#### 日志

- 错误日志
- 二进制日志
- 查询日志
- 慢查询日志



##### 错误日志

错误日志是My5QL中最重要的日志之一，它记录了当mysqld启动和停止时，以及服务器在运行过程中发生任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日志。

该日志是默认开启的，默认存放目录/var/log/l，默认的日志文件名为 mysqld.log。

查看日志位置:

```mysql
show variables like '%log_error%';
```

##### 二进制日志

- 介绍

  二进制日志（BINLOG)记录了所有的DDL(数据定义语言）语句和DML（数据操纵语言）语句，但不包括数据查询（SELECT、SHOW)语句。

  作用:①.灾难时的数据恢复;②.My5QL的主从复制。在My5QL8版本中，默认二进制日志是开启着的，涉及到的参数如下:

  ```mysql
  show variables like '%log_bin%' -- log_bin_basename  log_bin_index  
  ```

- 日志格式

  MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下:

  ![image-20240622180409789](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622180409789.png)

  ```mysql
  show variables like '%binlog_format%';
  ```

- 日志查看

  由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具mysqlbinlog来查看，具体语法:

  ![image-20240622180640557](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622180640557.png)

- 日志删除

  对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间。可以通过以下几种方式清理日志;

  ![image-20240622181350813](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622181350813.png)

  也可以在mysql的配置文件中配置二进制日志的过期时间，设置了之后，二进制日志过期会自动删除。

  ```mysql
  show variables like '%binlog_expire_logs_seconds%';
  ```

  

##### 查询日志

- 介绍

  查询日志中记录了客户端的**所有操作语句**，而二进制日志不包含查询数据的SQL语句。默认情况下，查询日志是未开启的。如果需要开启查询日志，可以设置以下配置∶

  ```mysql
  show variables like '%general_log%';
  ```

  ![image-20240622181947828](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622181947828.png)

##### 慢查询日志

- 介绍

  慢查询日志记录了所有执行时间超过参数long_query_time设置值并且扫描记录数不小于min_examined_row_limit的所有的SQL语句的日志，默认未开启。long_query_time默认为10秒，最小为0，精度可以到微秒。

  ```mysql
  #慢查询日志
  slow_query_log=1
  #执行时间参数
  long_query_time=2
  ```

  默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用log_slow_admin_statements和更改此行为log_queries_not_using_indexes，如下所述。

  ```mysql
  # 记录执行较慢的管理语句
  log_slow_admin_statements = 1
  # 记录执行较慢的未使用索引的语句			my.cnf配置文件
  log_queries_not_using_indexes = 1
  ```

  

## 主从复制

- 概述
- 原理
- 搭建

##### 概述

主从复制是指将主数据库的DDL和DML操作通过二进制日志传到从库服务器中，然后在从库上对这些日志重新

执行（也叫重做)，从而使得从库和主库的数据保持同步。

MySQL支持一台主库同时向多台从库进行复制，从库同时也可以作为其他从服务器的主库，实现链状复制。

MySQL复制的优点主要包含以下三个方面:

1. 主库出现问题,可以快速切换到从库提供服务。
2. 实现读写分离，降低主库的访问压力。
3. 可以在从库中执行备份,以避免备份期间影响主库服务。

##### 原理

MySQL的主从复制原理如下。（Relay log：中继日志）

1. Master主库在事务提交时，会把数据变更记录在二进制日志文件Bin log 中
2. 从库读取主库的二进制日志文件 Binlog，写入到从库的中继日志Relay Log 
3. slave重做中继日志中的事件，将改变反映它自己的数据。

如图所示：

![image-20240622183140830](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622183140830.png)

##### 搭建

- 服务器准备

![image-20240622183533880](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622183533880.png)

- 主库配置

  1. 修改配置文件/etc/ my.cnf

     ```mysql
     #mysql服务ID，保证整个集群环境中唯一，取值范围:1-232-1，默认为1
     server-id=1
     #是否只读,1代表只读,0代表读写
     read-only=O
     #忽略的数据,指不需要同步的数据库
     #binlog-ignore-db=mysql
     #指定同步的数据库
     #binlog-do-db=db01
     ```

  2. 重启MySQL服务器

     ```mysql
     systemctl restart mysqld;
     ```

  3. 登录mysql，创建远程连接的账号，并授予主从复制权限

     ```mysql
     #创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务
     CREATE USER 'itcast'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456';
     # 为'itcast'@'%'用户分配主从复制权限
     GRANT REPLICATION SLAVE ON *.* TO'itcast'@'%';
     ```

  4. 通过指令，查看二进制日志坐标

     ```mysql
     show master status
     ```

     字段含义说明：

     file :从哪个日志文件开始推送日志文件

     position :从哪个位置开始推送日志

     binlog_ignore_db:指定不需要同步的数据库

- 从库配置

![image-20240622185627659](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622185627659.png)

![image-20240622185818551](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622185818551.png)

![image-20240622185843767](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622185843767.png)

![image-20240622185919694](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622185919694.png)





### 分库分表（面试高频）

1. 介绍 
2. Mycat概述
3. Mycat入门
4. Mycat配置
5. Mycat分片
6. Mycat管理及监控

### 介绍

##### 问题分析

- 随着互联网及移动互联网的发展，应用系统的数据量也是成指数式增长，若采用单数据库进行数据存储，存在以下性能瓶颈;

  1. IO瓶颈:热点数据太多，数据库缓存不足，产生大量磁盘IO，效率较低。请求数据太多，带宽不够，网络IO瓶颈。

  2. CPU瓶颈:排序、分组、连接查询、聚合统计等SQL会耗费大量的CPU资源，请求数太多，CPU出现瓶颈。

     **分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能问题，从而达到提升数据库性能的目的。**

##### 拆分策略

![image-20240622191601439](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622191601439.png)



- 垂直拆分

  **垂直分库：**以表为依据，根据业务将不同表拆分到不同库中。

  特点：

  1. 每个库的表结构都不一样。

  2. 每个库的数据也不一样。

  3. 所有库的并集是全量数据。

     ![image-20240622191818444](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622191818444.png)

  **垂直分表：**以字段为依据，根据字段属性将不同字段拆分到不同表中。

  特点:

  1. 每个表的结构都不—样。
  2. 每个表的数据也不一样，一般通过一列（主键/外键）关联。
  3. 所有表的并集是全量数据。

![image-20240622192508531](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622192508531.png)

- 水平拆分

  **水平分库**：以字段为依据，按照一定策略，将一个库的数据拆分到多个库中。

  特点：

  1. 每个库的表结构都一样。 
  2. 每个库的数据都不—样。
  3. 所有库的并集是全量数据。

  ![image-20240622193000406](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622193000406.png)

  **水平分表**：以字段为依据，按照一定策略，将一个表的数据拆分到多个表中。
  特点:

  1. 每个表的表结构都一样。
  2. 每个表的数据都不一样。
  3. 所有表的并集是全量数据。

![image-20240622193058781](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240622193058781.png)

- 实现技术

  - shardingJDBC：基于AOP原理，在应用程序中对本地执行的SQL进行拦截，解析、改写、路由处理。

    需要自行编码配置实现，只支持java语言，性能较高。

  - MyCat：数据库分库分表中间件，不用调整代码即可实现分库分表，支持多种语言，性能不及前者。

  

##### Mycat概述

- 介绍

  Mycat是开源的、活跃的、基于Java语言编写的MySQL**数据库中间件**。可以像使用mysql一样来使用mycat，

  对于开发人员来说根本感觉不到mycat的存在。

  优势：

  1. 性能可靠稳定
  2. 强大的技术团队
  3. 体系完善
  4. 社区活跃

- 安装

。。。



### 读写分离

- 介绍
- 一主一从
- 一主一从读写分离
- 双主双从
- 双主双从读写分离



##### 介绍

读写分离,简单地说是把对数据库的读和写操作分开,以对应不同的数据库服务器。主数据库提供写操作，从数据库

提供读操作，这样能有效地减轻单台数据库的压力。

通过MyCat即可轻易实现上述功能，不仅可以支持MySQL，也可以支持Oracle和SQL Server。

##### 一主一从

- 原理

  MySQL的主从复制，是基于二进制日志( binlog）实现的。















































































































































































