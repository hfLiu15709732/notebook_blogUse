

## 数据库常用查询操作：



### 数据库基本结构如下：



```sql
-- ----------------------------
-- 课程表的结构状态
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

-- ----------------------------
-- 选课信息表结构状态：
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

-- ----------------------------
-- 学生信息表信息状态：
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

-- ----------------------------
-- 教师信息表状态：
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
```







### 表格数据信息如下图所示：

```sql

-- 向Students表如下数据
INSERT INTO liuhf_students
VALUES ('S01','王建平','WJP@zjut.edu.cn', 23.1,'男'),
        ('S02','刘华','LH@zjut.edu.cn', 24.6,'女'),
        ('S03','范林军','FLJ@zjut.edu.cn', 16.6,'女'),
        ('S04','李伟','LW@zjut.edu.cn', 15.8,'男'),
        ('S26','黄河','HUanghe@zjut.edu.cn', 13.4,'男'),
        ('S52','长江','Changjiang@zjut.edu.cn', 12.4,'男');

-- 向Teachers表如下数据
INSERT INTO liuhf_teachers
VALUES ('T01','刘涛','LT@zjut.edu.cn', 4300),
        ('T02','吴碧艳','WBY@zjut.edu.cn', 2500),
        ('T03','张莹','ZY@zjut.edu.cn', 3000),
        ('T04','张宁雅','ZNY@zjut.edu.cn', 5500),
        ('T05','叶帅','YS@zjut.edu.cn', 3800),
        ('T06','杨光美','YGM@zjut.edu.cn', 3500),
        ('T07','程潜','CQ@zjut.edu.cn', 5000);

-- 向Courses表如下数据
INSERT INTO liuhf_courses
  VALUES ('C01','C++',4),
        ('C02','UML',4),
        ('C03','JAVA',3),
        ('C04','算法分析与设计',3),
        ('C05','数据库原理及应用',3),
        ('C06','数据结构与算法',4),
        ('C07','计算机组成原理',4),
        ('C08','英语',6),
        ('C09','数字生活',2),
        ('C10','音乐鉴赏',2),
        ('C11','体育1',2);
-- 向Reports表如下数据
INSERT INTO liuhf_reports 
VALUES ('S01','T01', 'C01',83),
      ('S01','T03', 'C03',85),
      ('S02','T01', 'C01',75),
      ('S02','T02', 'C02',45),
      ('S02','T03', 'C03',NULL),
      ('S02','T04', 'C04',NULL),
      ('S02','T05', 'C05',70),
      ('S02','T04', 'C06',83),
      ('S02','T05', 'C07',90),
      ('S02','T01', 'C08',83),
      ('S02','T02', 'C09',77),
      ('S02','T07', 'C10',83),
      ('S02','T06', 'C11',88),
      ('S03','T01', 'C08',63),
      ('S03','T02', 'C02',93),
      ('S03','T01', 'C01',78),
      ('S04','T06', 'C06',89),
      ('S04','T05', 'C05',93),
      ('S26','T07', 'C10',45),
      ('S26','T04', 'C04',86),
      ('S52','T07', 'C10',91),
      ('S52','T06', 'C11',90),
      ('S52','T05', 'C05',NULL),
      ('S52','T01', 'C08',64),
      ('S52','T02', 'C09',81);

```





### 相关数据库查询需求如下所示：

```apl
（1）	查询性别为“男”的所有学生的名称并按学号升序排列。
（2）	查询学生的选课成绩合格的课程成绩，并把成绩换算为积分。积分的计算公式为：[1+(考试成绩-60)*0.1]*Ccredit。考试成绩>=60 否则=0
（3）	查询学分是3或4的课程的名称。
（4）	查询所有课程名称中含有“算法”的课程编号。
（5）	查询所有选课记录的课程号（不重复显示）。
（6）	统计所有老师的平均工资。
（7）	查询所有教师的编号及选修其课程的学生的平均成绩，按平均成绩降序排列。
（8）	统计各个课程的选课人数和平均成绩。
（9）	查询至少选修了三门课程的学生编号和姓名。
（10）	查询编号S26的学生所选的全部课程的课程名和成绩。
（11）	查询所有选修了“数据库原理及应用”课程的学生编号和姓名。
（12）	求出选修了同一个课程的学生对。
（13）	求出至少被两名学生选修的课程编号。
（14）	查询选修了编号S26的学生所选的某个课程的学生编号。
（15）	查询学生的基本信息及选修课程编号和成绩。
（16）	查询学号S52的学生的姓名和选修的课程名称及成绩。
（17）	查询和学号S52的学生同性别的所有学生资料。
（18）	查询所有选课的学生的详细信息。
（19）	查询没有学生选的课程的编号和名称。
（20）	查询选修了课程名为C++的学生学号和姓名。
（21）	找出选修课程UML或者课程C++的学生学号和姓名。
（22）	找出和课程UML或课程C++的学分一样课程名称。
（23）	查询所有选修编号C01的课程的学生的姓名。
（24）	查询选修了所有课程的学生姓名。
（25）	利用集合并运算，查询选修课程C++或选择课程JAVA的学生的编号、姓名和积分。
（26）	实现集合交运算，查询既选修课程C++又选修课程JAVA的学生的编号、姓名和积分。
（27）	实现集合减运算，查询选修课程C++而没有选修课程JAVA的学生的编号。

```







### 相关需求的实现SQL代码：

```sql
SELECT lhf_sname FROM liuhf_students
WHERE lhf_ssex='男' ORDER BY lhf_sno
-- （1）	查询性别为“男”的所有学生的名称并按学号升序排列。




SELECT liuhf_students.lhf_sno,lhf_sname,TRunc(CASE WHEN lhf_score>=60 THEN (1+(lhf_score-60)*0.1)*lhf_ccredit  ELSE 0  END,1) AS 积分 
FROM liuhf_reports,liuhf_students,liuhf_courses
WHERE liuhf_students.lhf_sno=liuhf_reports.lhf_sno AND liuhf_courses.lhf_cno=liuhf_reports.lhf_cno AND liuhf_reports.lhf_score>=60;
-- （2）	查询学生的选课成绩合格的课程成绩，并把成绩换算为积分。积分的计算公式为：[1+(考试成绩-60)*0.1]*Ccredit。考试成绩>=60 否则=0
-- （2）	查询学生的选课成绩合格的课程成绩，并把成绩换算为积分。积分的计算公式为：[1+(考试成绩-60)*0.1]*Ccredit。考试成绩>=60 否则=0




SELECT lhf_cname FROM liuhf_courses WHERE lhf_ccredit=3 OR lhf_ccredit=4;
-- （3）	查询学分是3或4的课程的名称。



SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname LIKE '%算法%';
-- （4）	查询所有课程名称中含有“算法”的课程编号。


SELECT DISTINCT lhf_cno FROM liuhf_reports;
-- （5）	查询所有选课记录的课程号（不重复显示）。


SELECT AVG(lhf_tsalary) FROM liuhf_teachers;
-- （6）	统计所有老师的平均工资。



SELECT lhf_tno,AVG(lhf_score) FROM liuhf_reports
GROUP BY lhf_tno
ORDER BY AVG(lhf_score) DESC;
-- （7）	查询所有教师的编号及选修其课程的学生的平均成绩，按平均成绩降序排列。


SELECT lhf_cno,COUNT(lhf_cno),AVG(lhf_score) FROM liuhf_reports
GROUP BY lhf_cno;
-- （8）	统计各个课程的选课人数和平均成绩。



SELECT lhf_sno,lhf_sname FROM liuhf_students
WHERE lhf_sno IN 
(
SELECT lhf_sno FROM liuhf_reports GROUP BY lhf_sno 
HAVING(COUNT(lhf_sno)>=3)
);
-- （9）	查询至少选修了三门课程的学生编号和姓名。



SELECT lhf_cname,lhf_score FROM liuhf_courses,liuhf_reports
WHERE lhf_sno='S26' AND liuhf_courses.lhf_cno=liuhf_reports.lhf_cno;
-- （10）	查询编号S26的学生所选的全部课程的课程名和成绩。




SELECT DISTINCT a.lhf_sno,b.lhf_sno
FROM liuhf_reports a,liuhf_reports b
WHERE a.lhf_cno=b.lhf_cno
AND a.lhf_sno<b.lhf_sno;
-- （12）	求出选修了同一个课程的学生对。




SELECT liuhf_students.lhf_sno,lhf_sname FROM liuhf_reports,liuhf_students,liuhf_courses
WHERE lhf_cname='数据库原理及应用'
AND liuhf_reports.lhf_cno=liuhf_courses.lhf_cno
AND liuhf_reports.lhf_sno=liuhf_students.lhf_sno
-- （14）	查询选修了编号S26的学生所选的某个课程的学生编号。






SELECT lhf_cno FROM liuhf_reports GROUP BY lhf_cno HAVING COUNT(lhf_sno)>=2;
-- （13）	求出至少被两名学生选修的课程编号。




SELECT DISTINCT lhf_sno FROM liuhf_reports 
WHERE lhf_cno IN
( SELECT lhf_cno FROM liuhf_reports WHERE lhf_sno='S26');
-- （14）	查询选修了编号S26的学生所选的某个课程的学生编号。




SELECT liuhf_students.lhf_sno,lhf_sname,lhf_semail,lhf_cno,lhf_score
FROM liuhf_reports,liuhf_students
WHERE liuhf_students.lhf_sno=liuhf_reports.lhf_sno;
-- （15）	查询学生的基本信息及选修课程编号和成绩。





SELECT lhf_sname,lhf_cname,lhf_score
FROM liuhf_reports,liuhf_courses,liuhf_students
WHERE liuhf_reports.lhf_sno='S52' 
AND liuhf_reports.lhf_sno=liuhf_students.lhf_sno 
AND liuhf_reports.lhf_cno=liuhf_courses.lhf_cno;
-- （16）	查询学号S52的学生的姓名和选修的课程名称及成绩。



SELECT liuhf_students.lhf_sno,lhf_sname,lhf_semail,lhf_scredit,lhf_ssex,lhf_cno,lhf_score
FROM liuhf_reports,liuhf_students
WHERE lhf_ssex IN 
(SELECT lhf_ssex FROM liuhf_students WHERE lhf_sno='S52')
AND liuhf_students.lhf_sno=liuhf_reports.lhf_sno;
-- （17）	查询和学号S52的学生同性别的所有学生资料。





SELECT * FROM liuhf_students
WHERE lhf_sno IN (SELECT DISTINCT lhf_sno FROM liuhf_reports);
-- （18）	查询所有选课的学生的详细信息。




SELECT lhf_cname,lhf_cno FROM liuhf_courses
WHERE lhf_cno NOT IN 
(SELECT DISTINCT lhf_cno FROM liuhf_reports);
-- （19）	查询没有学生选的课程的编号和名称。




SELECT liuhf_students.lhf_sno,lhf_sname
FROM liuhf_students,liuhf_courses,liuhf_reports
WHERE lhf_cname='C++' 
AND liuhf_reports.lhf_cno=liuhf_courses.lhf_cno
AND liuhf_reports.lhf_sno=liuhf_students.lhf_sno;
-- （20）	查询选修了课程名为C++的学生学号和姓名。



SELECT lhf_sno,lhf_sname FROM liuhf_students
WHERE lhf_sno IN
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
( SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='C++'));
-- （20）	查询选修了课程名为C++的学生学号和姓名。


SELECT DISTINCT liuhf_students.lhf_sno,lhf_sname
FROM liuhf_students,liuhf_courses,liuhf_reports
WHERE lhf_cname='C++' OR lhf_cname='UML'
AND liuhf_reports.lhf_cno=liuhf_courses.lhf_cno
AND liuhf_reports.lhf_sno=liuhf_students.lhf_sno;
-- （21）	找出选修课程UML或者课程C++的学生学号和姓名。
-- （错的 错的· 错的）



SELECT lhf_sno,lhf_sname FROM liuhf_students
WHERE lhf_sno IN
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
( SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='C++' OR lhf_cname='UML'));
-- （21）	找出选修课程UML或者课程C++的学生学号和姓名。





SELECT lhf_cname FROM liuhf_courses WHERE lhf_ccredit IN
(SELECT lhf_ccredit FROM liuhf_courses WHERE lhf_cname='UML' OR lhf_cname='C++');
-- （22）	找出和课程UML或课程C++的学分一样课程名称。





SELECT lhf_sname FROM liuhf_students,liuhf_reports WHERE lhf_cno='C01'
AND liuhf_reports.lhf_sno=liuhf_students.lhf_sno;
-- （23）	查询所有选修编号C01的课程的学生的姓名。





SELECT  lhf_sname FROM liuhf_students
WHERE NOT EXISTS (SELECT * FROM liuhf_courses WHERE NOT EXISTS 
(SELECT * FROM liuhf_reports WHERE lhf_sno=liuhf_students.lhf_sno
 AND liuhf_reports.lhf_cno=liuhf_courses.lhf_cno));
-- （24）	查询选修了所有课程的学生姓名。





SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='C++'))

UNION

SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='JAVA'));
-- （25）	利用集合并运算，查询选修课程C++或选择课程JAVA的学生的编号、姓名和积分。








SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='C++'))

INTERSECT

SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='JAVA'));
-- （26）	实现集合交运算，查询既选修课程C++又选修课程JAVA的学生的编号、姓名和积分。





SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='C++'))

EXCEPT

SELECT lhf_sno,lhf_sname,lhf_scredit FROM liuhf_students
WHERE lhf_sno IN 
(SELECT lhf_sno FROM liuhf_reports WHERE lhf_cno IN 
(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='JAVA'));
-- （27）	实现集合减运算，查询选修课程C++而没有选修课程JAVA的学生的编号。


```



