# SQL基本操作总结及Egg.js操纵数据库

## SQL基本语句与操作



### 1.对数据库的操作：

```sql
-- 创建库
create database dbTest1;

-- 创建库是否存在，不存在则创建
create database if not exists dbTest1;

-- 查看某个数据库的定义信息 
show create database dbTest1; 

-- 删除数据库
drop database dbTest1; 


-- 查看所有数据库
show databases;

-- 修改数据库字符信息
alter database dbTest1 character set utf8; 
```





### 2.对数据表的整体操作：

```sql
--选择数据库
USE dbtest1;

--创建表
create table student(
    id int,
    name varchar(32),
    age int ,
    score double(4,1),//
    birthday date,
    insert_time timestamp//
);


 
-- 查看表结构
desc 表名;


-- 查看创建表的SQL语句
show create table 表名;


-- 修改表名
alter table 表名 rename to 新的表名;


-- 添加一列
alter table 表名 add 列名 数据类型;


-- 删除列
alter table 表名 drop 列名;


-- 删除表
drop table 表名;
drop table  if exists 表名 ;
```



### 3.对数据表的具体操作：



#### 3.1--插入语句：

```sql
-- 写全所有列名
insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);

-- 不写列名（所有列全部添加）
insert into 表名 values(值1,值2,...值n);

-- 插入部分数据
insert into 表名(列名1,列名2) values(值1,值2);
```





#### 3.2--删除语句：

```sql
-- 删除表中数据
delete from 表名 where 列名  = 值;

-- 删除表中所有数据
delete from 表名;

-- 删除表中所有数据（高效 先删除表，然后再创建一张一样的表。）
truncate table 表名;
```





#### 3.3--修改语句：

```sql
-- 不带条件的修改(会修改所有行)
update 表名 set 列名 = 值;

-- 带条件的修改
update 表名 set 列名 = 值 where 列名=值;
```



#### 3.4--查询语句：

>  ***\*BETWEEN...AND\** （在什么之间）和 \**IN\**( 集合)**



```sql
-- 查询年龄大于等于20 小于等于30				
SELECT * FROM student WHERE age >= 20 &&  age <=30;
SELECT * FROM student WHERE age >= 20 AND  age <=30;
SELECT * FROM student WHERE age BETWEEN 20 AND 30;
				
				
-- 查询年龄22岁，18岁，25岁的信息
SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
SELECT * FROM student WHERE age IN (22,18,25);
```







## 使用Egg.js操作mysql

### 1.安装npm包：

```js
npm i --save egg-mysql//安装npm包


// config/plugin.js文件中开启插件
exports.mysql = {
  enable: true,
  package: 'egg-mysql',
};


//在config/config.default.js文件中配置访问mysql的信息
 config.mysql = {
    client: {
      host: '127.0.0.1',
      port: '3306',
      user: 'root',
      password: '12345678',
      database: 'exam',//本项目所需要连接的数据库
    },
    // 是否加载到 app 上，默认开启
    app: true,
    // 是否加载到 agent 上，默认关闭
    agent: false,
  };
```





### 2.基本查询操作：



```javascript
const user = await this.app.mysql.get('dataTest',{username："1111"});//查询单条信息

const userAll = await this.app.mysql.get('dataTest');//查询表中的所用信息


//根据条件筛选多条信息时，可以采用Select进行查询

 const user = await this.app.mysql.select('data_person', { // 搜索 post 表
  where: { title: 'Hello World',  age: [10, 11]  }, // WHERE 条件
  columns: ['title'], // 要查询的表字段
  orders: [['person_id','desc']], // 排序方式
  limit: 10, // 返回数据量
  offset: 0, // 数据偏移量
});  


//上述信息等同于下面这些sql语句：
SELECT `author`, `title` FROM `posts`
  WHERE `title` = 'Hello World' AND `age` IN(10,11)
  ORDER BY `person_id` DESC,  LIMIT 0, 10;


```





### 3.插入数据与删除数据操作：



```javascript
await this.app.mysql.insert('data_person', { title: 'Hello World' });//插入操作


const result = await this.app.mysql.delete('data_person', {
  person_id: '5'
});

=> DELETE FROM `data_person` WHERE `person_id` = '5';

//把所有person_id为'5'的都删除
```



### 4.修改数据：



```javascript
//如果有主键，直接整体写入替换即可，
const row = {
  id: 6,
  title: '不是主键',
  create_time: this.app.mysql.literals.now,
};
const result = await this.app.mysql.update('posts', row); 

=> UPDATE `data_person` SET `title` = '不是主键', `create_time` = NOW() WHERE id = 6 ;



//如果没有主键，需要单独说明

const row = {
  title: '更新主键',
  create_time: this.app.mysql.literals.now, 
};

// 如果主键是自定义的 ID 名称，如 person_id，则需要在 `where` 里面配置
const options = {
  where: {
    person_id: '6'
  }
};
const result = await this.app.mysql.update('data_person', row, options); 

=> UPDATE `data_person` SET `title` = '更新主键', `create_time` = NOW() WHERE person_id = '6' ;

// 判断更新成功
const updateSuccess = result.affectedRows === 1;
```

