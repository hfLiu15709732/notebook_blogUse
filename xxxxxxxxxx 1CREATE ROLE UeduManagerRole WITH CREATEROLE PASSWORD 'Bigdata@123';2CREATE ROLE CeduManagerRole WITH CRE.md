# 数据库权限操作（数据库基础操作终章）

![](https://opengauss.org/assets/banner.7b5305a6.png)

## 基本要求：

### 简介：

> 为简化数据库db_uni的管理，老师的信息和学生的信息由校级教务教学部门来设置，课程信息和选课信息由院级教务教学部门来设置，教师可以设置学生成绩，学生查看课程成绩。以系统管理员zjutuser的身份登陆数据库db_uni，分别建立如下用户和角色。
>
> 用户包括校级教务教学主管UeduSL，
>
> 院级教务教学部门主管CeduSJ、CeduSA，
>
> 教师TeaY、TeaW
>
> 和学生StuG、StuL。
>
> 角色包括UeduManagerRole、CeduManagerRole和TeacherRole、StudentRole。
>
> 验证权限分配之前，请备份好数据库，针对不同用户所具有的权限，分别以系统管理员zjutuser身份或以上用户身份登陆到数据库db_uni中（用gsql终端工具登陆更方便切换用户，具体命令可参考安装手册中的命令），设计相应的SQL 语句，进行操作加以验证，并记录操作结果。 



### 具体需求：

1. **创建用户**
2. **创建角色并分配权限**
3. **为用户分配角色及权限**
4. **分别以UeduSL、CeduSJ、TeaY和StuG的身份登陆数据库db_uni，验证按角色授予的权限，用SQL语言查询zjutuser.Teachers、zjutuser.Students、zjutuser.Courses和zjutuser.Reports表，查询结果如何？并分析原因。**
5. **用系统管理员zjutuser授予用户"CeduSA"对表zjutuser.Students插入和更新的权限，但不授予删除权限，并且授予用户"CeduSA"传播这两个权限的权利。以"CeduSA"的身份登陆，用SQL语言插入和更新Students表，结果如何？（注意更新操作的授权）**
6. **用系统管理员zjutuser授予允许用户"TeaW"在表Reports中插入元组，更新Score列，可以查询除了Sno以外的所有列。以"TeaW"的身份登陆，用SQL语言插入更新并查询reports表，结果如何？（注意更新操作的授权）**
7. **用户"CeduSA"授予用户"TeaW"对表Students插入和更新的权限，并且授予用户"TeaW"传播插入和更新操作的权利。分别以"CeduSA"和"TeaW"的身份登陆，用SQL语言验证以上授权操作，结果如何？**
8. **收回用户"CeduSA"对表Courses查询权限的授权。分别以"CeduSA"和"TeaW"的身份登陆，用SQL语言查询Courses表，查询结果如何？**
9. **由上面（6）和（7）的授权，再由用户"TeaW"对用户"StuL"授予表Students插入和更新的权限，并且授予用户"StuL"传播插入和更新操作的权力。这时候，如果由"StuL"对"CeduSA"授予表Students的插入和更新权限是否能得到成功？如果能够成功，那么如果由用户"TeaW"取消"StuL"的权限，对"CeduSA"会有什么影响？如果再由zjutuser取消"CeduSA"的权限，对"TeaW"有什么影响？**





## 涉及相应技术点:

### 将表中字段的访问权限赋予指定的用户或角色

```sql
GRANT { {{ SELECT | INSERT | UPDATE | REFERENCES } ( column_name [, ...] )} [, ...] 
      | ALL [ PRIVILEGES ] ( column_name [, ...] ) }
    ON [ TABLE ] table_name [, ...]
    TO { [ GROUP ] role_name | PUBLIC } [, ...]
    [ WITH GRANT OPTION ];
```



### 将数据库的访问权限赋予指定的用户或角色

```sql
GRANT { { CREATE | CONNECT | TEMPORARY | TEMP } [, ...]
      | ALL [ PRIVILEGES ] }
    ON DATABASE database_name [, ...]
    TO { [ GROUP ] role_name | PUBLIC } [, ...]
    [ WITH GRANT OPTION ];
```





### 回收指定表和视图上权限。

```sql
REVOKE [ GRANT OPTION FOR ]    { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES }[, ...]     | ALL [ PRIVILEGES ] }    ON { [ TABLE ] table_name [, ...]       | ALL TABLES IN SCHEMA schema_name [, ...] }    FROM { [ GROUP ] role_name | PUBLIC } [, ...]    [ CASCADE | RESTRICT ];
```

### 回收表上指定字段权限。

```sql
REVOKE [ GRANT OPTION FOR ]    { {{ SELECT | INSERT | UPDATE | REFERENCES } ( column_name [, ...] )}[, ...]     | ALL [ PRIVILEGES ] ( column_name [, ...] ) }    ON [ TABLE ] table_name [, ...]    FROM { [ GROUP ] role_name | PUBLIC } [, ...]    [ CASCADE | RESTRICT ];
```

### 回收指定数据库上权限。

```sql
REVOKE [ GRANT OPTION FOR ]    { { CREATE | CONNECT | TEMPORARY | TEMP } [, ...]     | ALL [ PRIVILEGES ] }    ON DATABASE database_name [, ...]    FROM { [ GROUP ] role_name | PUBLIC } [, ...]    [ CASCADE | RESTRICT ];
```





### 管理员更换更换

**在自己的相应DBS中切换连接时更改用户名及密码即可**

![](https://tmp-bucket-1316646528.cos.ap-nanjing.myqcloud.com/Inked4.1_LI.jpg)

## 当前数据库基本结构：

### sql文件结构：

```sql


-- ----------------------------
-- Table structure for liuhf_courses
-- ----------------------------
DROP TABLE IF EXISTS "zjutuser"."liuhf_courses";
CREATE TABLE "zjutuser"."liuhf_courses" (
"lhf_cno" varchar(6) COLLATE "default" NOT NULL,
"lhf_cname" varchar(30) COLLATE "default" NOT NULL,
"lhf_ccredit" numeric(5,1)
)
WITH (OIDS=FALSE)

;

-- ----------------------------
-- Records of liuhf_courses
-- ----------------------------
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C01', 'C++', '4.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C02', 'UML', '4.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C03', 'JAVA', '3.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C04', '算法分析与设计', '3.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C05', '数据库原理及应用', '3.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C06', '数据结构与算法', '4.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C07', '计算机组成原理', '4.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C08', '英语', '6.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C09', '数字生活', '2.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C10', '音乐鉴赏', '2.0');
INSERT INTO "zjutuser"."liuhf_courses" VALUES ('C11', '体育1', '2.0');

-- ----------------------------
-- Table structure for liuhf_reports
-- ----------------------------
DROP TABLE IF EXISTS "zjutuser"."liuhf_reports";
CREATE TABLE "zjutuser"."liuhf_reports" (
"lhf_sno" varchar(6) COLLATE "default" NOT NULL,
"lhf_tno" varchar(6) COLLATE "default" NOT NULL,
"lhf_cno" varchar(6) COLLATE "default" NOT NULL,
"lhf_score" numeric(5,1)
)
WITH (OIDS=FALSE)

;

-- ----------------------------
-- Records of liuhf_reports
-- ----------------------------
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S01', 'T01', 'C01', '84.5');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S01', 'T03', 'C03', '86.5');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T01', 'C01', '75.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T01', 'C08', '83.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T02', 'C02', '45.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T02', 'C09', '77.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T03', 'C03', null);
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T04', 'C04', null);
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T04', 'C06', '83.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T05', 'C05', '70.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T05', 'C07', '90.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T06', 'C11', '88.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S02', 'T07', 'C10', '83.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S03', 'T01', 'C01', '78.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S03', 'T01', 'C08', '63.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S03', 'T02', 'C02', '93.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S04', 'T05', 'C05', '93.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S04', 'T06', 'C06', '89.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S26', 'T04', 'C04', '86.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S26', 'T07', 'C10', '45.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S52', 'T01', 'C08', '64.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S52', 'T02', 'C09', '81.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S52', 'T05', 'C05', null);
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S52', 'T06', 'C11', '90.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S52', 'T07', 'C10', '91.0');

-- ----------------------------
-- Table structure for liuhf_students
-- ----------------------------
DROP TABLE IF EXISTS "zjutuser"."liuhf_students";
CREATE TABLE "zjutuser"."liuhf_students" (
"lhf_sno" varchar(6) COLLATE "default" NOT NULL,
"lhf_sname" varchar(30) COLLATE "default" NOT NULL,
"lhf_semail" varchar(50) COLLATE "default",
"lhf_scredit" numeric(5,1),
"lhf_ssex" varchar(3) COLLATE "default"
)
WITH (OIDS=FALSE)

;

-- ----------------------------
-- Records of liuhf_students
-- ----------------------------
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S01', '王建平', 'WJP@zjut.edu.cn', '23.1', '男');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S02', '刘华', 'LH@zjut.edu.cn', '24.6', '女');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S03', '范林军', 'FLJ@zjut.edu.cn', '16.6', '女');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S04', '李伟', 'LW@zjut.edu.cn', '15.8', '男');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S26', '黄河', 'HUanghe@zjut.edu.cn', '13.4', '男');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S52', '长江', 'Changjiang@zjut.edu.cn', '12.4', '男');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S66', '张三', 'LHF@zjut.edu.cn', '24.1', '男');
INSERT INTO "zjutuser"."liuhf_students" VALUES ('S77', '王五', 'wwwa@zjut.edu.cn', '24.2', '男');

-- ----------------------------
-- Table structure for liuhf_teachers
-- ----------------------------
DROP TABLE IF EXISTS "zjutuser"."liuhf_teachers";
CREATE TABLE "zjutuser"."liuhf_teachers" (
"lhf_tno" varchar(6) COLLATE "default" NOT NULL,
"lhf_tname" varchar(30) COLLATE "default" NOT NULL,
"lhf_temail" varchar(50) COLLATE "default",
"lhf_tsalary" numeric(5,1)
)
WITH (OIDS=FALSE)

;

-- ----------------------------
-- Records of liuhf_teachers
-- ----------------------------
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T01', '刘涛', 'LT@zjut.edu.cn', '4300.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T02', '吴碧艳', 'WBY@zjut.edu.cn', '2500.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T03', '张莹', 'ZY@zjut.edu.cn', '3000.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T04', '张宁雅', 'ZNY@zjut.edu.cn', '5500.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T05', '叶帅', 'YS@zjut.edu.cn', '3800.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T06', '杨光美', 'YGM@zjut.edu.cn', '3500.0');
INSERT INTO "zjutuser"."liuhf_teachers" VALUES ('T07', '程潜', 'CQ@zjut.edu.cn', '5000.0');

-- ----------------------------
-- View structure for liuhf_cs_view_opt
-- ----------------------------
CREATE OR REPLACE VIEW "zjutuser"."liuhf_cs_view_opt" AS 
SELECT liuhf_reports.lhf_sno AS "学生编号", liuhf_reports.lhf_tno AS "教师编号", liuhf_reports.lhf_cno AS "所选课程号", liuhf_reports.lhf_score AS "课程成绩" FROM zjutuser.liuhf_reports;

-- ----------------------------
-- Alter Sequences Owned By 
-- ----------------------------

-- ----------------------------
-- Primary Key structure for table liuhf_courses
-- ----------------------------
ALTER TABLE "zjutuser"."liuhf_courses" ADD PRIMARY KEY ("lhf_cno");

-- ----------------------------
-- Primary Key structure for table liuhf_reports
-- ----------------------------
ALTER TABLE "zjutuser"."liuhf_reports" ADD PRIMARY KEY ("lhf_sno", "lhf_tno", "lhf_cno");

-- ----------------------------
-- Indexes structure for table liuhf_students
-- ----------------------------
CREATE UNIQUE INDEX "lhf_students_sname" ON "zjutuser"."liuhf_students" USING btree (lhf_sname);

-- ----------------------------
-- Primary Key structure for table liuhf_students
-- ----------------------------
ALTER TABLE "zjutuser"."liuhf_students" ADD PRIMARY KEY ("lhf_sno");

-- ----------------------------
-- Primary Key structure for table liuhf_teachers
-- ----------------------------
ALTER TABLE "zjutuser"."liuhf_teachers" ADD PRIMARY KEY ("lhf_tno");

-- ----------------------------
-- Foreign Key structure for table "zjutuser"."liuhf_reports"
-- ----------------------------
ALTER TABLE "zjutuser"."liuhf_reports" ADD FOREIGN KEY ("lhf_tno") REFERENCES "zjutuser"."liuhf_teachers" ("lhf_tno") ON DELETE NO ACTION ON UPDATE NO ACTION;
ALTER TABLE "zjutuser"."liuhf_reports" ADD FOREIGN KEY ("lhf_sno") REFERENCES "zjutuser"."liuhf_students" ("lhf_sno") ON DELETE NO ACTION ON UPDATE NO ACTION;
ALTER TABLE "zjutuser"."liuhf_reports" ADD FOREIGN KEY ("lhf_cno") REFERENCES "zjutuser"."liuhf_courses" ("lhf_cno") ON DELETE NO ACTION ON UPDATE NO ACTION;

```



## 具体操作语句:

### 第一题：

```sql
CREATE USER "UeduSL" WITH CREATEROLE PASSWORD 'Bigdata@123';
CREATE USER "CeduSJ" WITH CREATEROLE PASSWORD 'Bigdata@123';
CREATE USER "CeduSA" WITH CREATEROLE PASSWORD 'Bigdata@123';
-- 创建校院两级教务教学主管用户，要求具有创建用户或角色的权利。





CREATE USER "TeaY"  IDENTIFIED BY  'Bigdata@123';
CREATE USER "TeaW"  IDENTIFIED BY  'Bigdata@123';
CREATE USER "StuG"  IDENTIFIED BY  'Bigdata@123';
CREATE USER "StuL"  IDENTIFIED BY  'Bigdata@123';
-- 创建教师和学生用户。
```

### 第二题：

```sql
CREATE ROLE "UeduManagerRole" WITH CREATEROLE PASSWORD 'Bigdata@123';
CREATE ROLE "CeduManagerRole" WITH CREATEROLE PASSWORD 'Bigdata@123';
CREATE ROLE "TeacherRole" WITH  PASSWORD 'Bigdata@123';
CREATE ROLE "StudentRole" WITH  PASSWORD 'Bigdata@123'; 
-- 分别创建校院两级教务教学管理角色UeduManagerRole和CeduManagerRole、教师角色TeacherRole和学生角色StudentRole。



GRANT USAGE ON SCHEMA zjutuser to "UeduManagerRole" WITH GRANT OPTION;
GRANT USAGE ON SCHEMA zjutuser to "CeduManagerRole" WITH GRANT OPTION;
GRANT USAGE ON SCHEMA zjutuser to "TeacherRole" WITH GRANT OPTION;
GRANT USAGE ON SCHEMA zjutuser to "StudentRole" WITH GRANT OPTION;
GRANT USAGE ON SCHEMA zjutuser to "UeduSL","CeduSJ","CeduSA","TeaY","TeaW","StuG","StuL" WITH GRANT OPTION;
-- 把数据库db_uni的模式zjutuser的使用权限授予所有角色与部分用户。



GRANT ALL ON TABLE liuhf_teachers,liuhf_students to "UeduManagerRole" WITH GRANT OPTION;
GRANT ALL ON TABLE liuhf_courses,liuhf_reports to "CeduManagerRole" WITH GRANT OPTION;
GRANT SELECT,UPDATE ON TABLE liuhf_reports to "TeacherRole" WITH GRANT OPTION;
GRANT SELECT ON TABLE liuhf_reports to "StudentRole" WITH GRANT OPTION;
-- 把Teachers、Students、Courses和Reports的相应权限分别授权给不同的角色
```

### 第三题：

```sql
GRANT "UeduManagerRole" TO "UeduSL" WITH ADMIN OPTION;
GRANT "CeduManagerRole" TO "CeduSJ" WITH ADMIN OPTION;
GRANT "TeacherRole" TO "TeaY" WITH ADMIN OPTION;
GRANT "StudentRole" TO "StuG" WITH ADMIN OPTION;
-- 为用户按角色分配并授予权限



GRANT SELECT ON TABLE liuhf_teachers,liuhf_students, liuhf_courses,liuhf_reports to "CeduSA","TeaW", "StuL" WITH GRANT OPTION;
-- 按用户单独赋予权限
```

### 第四题：

```sql
SELECT * FROM zjutuser.liuhf_courses;
SELECT * FROM zjutuser.liuhf_teachers;
SELECT * FROM zjutuser.liuhf_students;
SELECT * FROM zjutuser.liuhf_reports;
```

### 第五题：

```sql
GRANT Insert,UPDATE ON TABLE zjutuser.liuhf_students to "CeduSA" WITH GRANT OPTION;
-- 授予用户"CeduSA"传播这两个权限的权利




INSERT INTO liuhf_students
VALUES ('S66','刘浩峰','LHF@zjut.edu.cn', 24.1,'男'); --插入数据操作


UPDATE liuhf_students SET lhf_sname='张三' WHERE lhf_sno='S66';-- 修改数据操作

-- 以"CeduSA"的身份登陆，用SQL语言插入和更新Students表，结果如何
```





### 第六题：

```sql
GRANT INSERT,UPDATE(lhf_score),SELECT(lhf_tno,lhf_score,lhf_cno)ON liuhf_reports TO "TeaW";
-- (6)	用系统管理员zjutuser授予允许用户"TeaW"在表Reports中插入元组，更新Score列，可以查询除了Sno以外的所有列


SELECT  FROM zjutuser.liuhf_reports;
UPDATE zjutuser.liuhf_reports SET lhf_score= lhf_score+0.5 WHERE lhf_sno='S01';

-- 以"TeaW"的身份登陆，用SQL语言插入更新并查询reports表，结果如何？
```

### 第七题：

```sql
GRANT Insert,UPDATE ON TABLE zjutuser.liuhf_students to "TeaW" WITH GRANT OPTION;
-- 授予用户"TeaW"传播这两个权限的权利



INSERT INTO zjutuser.liuhf_students
VALUES ('S77','李四','wwwa@zjut.edu.cn', 24.2,'男'); --插入数据操作


UPDATE zjutuser.liuhf_students SET lhf_sname='王五' WHERE lhf_sno='S77';-- 修改数据操作

-- 分别以"CeduSA"和"TeaW"的身份登陆，用SQL语言验证以上授权操作，结果如何？
```

### 第八题：

```sql
REVOKE SELECT ON liuhf_courses FROM "CeduSA";
-- (8)	收回用户"CeduSA"对表Courses查询权限的授权

SELECT * FROM zjutuser.liuhf_courses;
-- 登录CudeSA  用SQL语句查询Courses表
```

### 第九题：

```SQL
GRANT Insert,UPDATE ON TABLE zjutuser.liuhf_students to "StuL" WITH GRANT OPTION;
-- 再由用户"TeaW"对用户"StuL"授予表Students插入和更新的权限，并且授予用户"StuL"传播插入和更新操作的权力


GRANT Insert,UPDATE ON TABLE zjutuser.liuhf_students to "CeduSA" WITH GRANT OPTION;
-- 如果由"StuL"对"CeduSA"授予表Students的插入和更新权限是否能得到成功？





REVOKE INSERT,UPDATE ON liuhf_students FROM "CeduSA" CASCADE;
-- 如果再由zjutuser取消"CeduSA"的权限，对"TeaW"有什么影响？



INSERT INTO liuhf_students
VALUES ('S88','testPeople','test@zjut.edu.cn', 24.1,'男'); -- 插入数据操作
UPDATE liuhf_students SET lhf_sname='张三' WHERE lhf_sno='S66';-- 修改数据操作

-- 如果再由zjutuser取消"CeduSA"的权限，对"TeaW"有什么影响？
```









## 相关文档链接:



[OpenGause文档（revoke部分）](https://docs.opengauss.org/zh/docs/3.1.0/docs/Developerguide/dolphin-REVOKE.html)

[OpenGause文档 (grant部分)](https://docs.opengauss.org/zh/docs/3.1.0/docs/Developerguide/dolphin-GRANT.html)

