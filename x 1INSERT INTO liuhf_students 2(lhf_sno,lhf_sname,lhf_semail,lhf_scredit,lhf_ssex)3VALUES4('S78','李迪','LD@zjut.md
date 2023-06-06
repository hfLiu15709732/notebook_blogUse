## 数据库常用查询操作：





### 数据库基本结构如下：



```sql

-- ----------------------------
-- Table structure for liuhf_course_counting
-- ----------------------------
DROP TABLE IF EXISTS "zjutuser"."liuhf_course_counting";
CREATE TABLE "zjutuser"."liuhf_course_counting" (
"lhf_cno" varchar(6) COLLATE "default",
"count" int8,
"avg" numeric
)
WITH (OIDS=FALSE)

;

-- ----------------------------
-- Records of liuhf_course_counting
-- ----------------------------
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C01', '3', '78.6666666666666667');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C01', '3', '78.6666666666666667');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C02', '2', '69.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C02', '2', '69.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C03', '2', '85.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C03', '2', '85.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C04', '2', '86.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C04', '2', '86.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C05', '3', '81.5000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C05', '3', '81.5000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C06', '2', '86.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C06', '2', '86.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C07', '1', '90.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C07', '1', '90.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C08', '3', '70.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C08', '3', '70.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C09', '2', '79.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C09', '2', '79.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C10', '3', '73.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C10', '3', '73.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C11', '2', '89.0000000000000000');
INSERT INTO "zjutuser"."liuhf_course_counting" VALUES ('C11', '2', '89.0000000000000000');

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
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S01', 'T01', 'C01', '83.0');
INSERT INTO "zjutuser"."liuhf_reports" VALUES ('S01', 'T03', 'C03', '85.0');
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





### 数据操作的相关要求：



```apl
(1)	使用SQL语句向Students表中插入元组(Sno：S78; Sname：李迪; Semail:LD@zjut.edu.cn; Scredit：0；Ssex：男)。
(2)	对每个课程，求学生的选课人数和学生的平均成绩，并把结果存入数据库。使用SELECT INTO 和INSERT INTO 两种方法实现。
(3)	在Students表中使用SQL语句将姓名为李迪的学生的学号改为S70。
(4)	在Teachers表中使用SQL语句将所有教师的工资加500元。
(5)	将姓名为刘华的学生的课程“数据库原理及其应用”的成绩加上6分。
(6)	在Students表中使用SQL语句删除姓名为李迪的学生信息。
(7)	删除所有选修课程JAVA的选修课记录。
(8)	对Courses表做删去学分<=4的元组操作，讨论该操作所受到的约束。

```



### 数据操作的SQL语句信息：

```sql

INSERT INTO liuhf_students 
(lhf_sno,lhf_sname,lhf_semail,lhf_scredit,lhf_ssex)
VALUES
('S78','李迪','LD@zjut.edu.cn',0,'男');
-- 向Students表中插入元组(Sno：S78; Sname：李迪; Semail:LD@zjut.edu.cn; Scredit：0；Ssex：男)。


SELECT lhf_cno,COUNT(lhf_cno),AVG(lhf_score)
INTO liuhf_course_counting FROM liuhf_reports GROUP BY lhf_cno;
-- (2)	对每个课程，求学生的选课人数和学生的平均成绩，并把结果存入数据库。SELECT INTO的实现方式。



INSERT INTO liuhf_course_counting (lhf_cno,count,avg)
   SELECT lhf_cno,count,avg FROM liuhf_course_counting;
-- (2)	对每个课程，求学生的选课人数和学生的平均成绩，并把结果存入数据库。INSERT INTO的实现方式。)
--  将之前的自动创建的表再复制一遍。



UPDATE liuhf_students SET lhf_sno='S70' WHERE lhf_sname='李迪';
-- (3)	在Students表中使用SQL语句将姓名为李迪的学生的学号改为S70。




UPDATE liuhf_teachers SET lhf_tsalary=lhf_tsalary+500;
-- (4)	在Teachers表中使用SQL语句将所有教师的工资加500元。




UPDATE liuhf_reports SET lhf_score=lhf_score+6 
WHERE lhf_cno=(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='数据库原理及其应用' )
AND lhf_sno=(SELECT lhf_sno FROM liuhf_students WHERE lhf_sname='李华');
-- (5)	将姓名为刘华的学生的课程“数据库原理及其应用”的成绩加上6分。



DELETE FROM liuhf_students WHERE lhf_sname='李迪';
-- (6)	在Students表中使用SQL语句删除姓名为李迪的学生信息。




DELETE FROM liuhf_reports
WHERE lhf_cno=(SELECT lhf_cno FROM liuhf_courses WHERE lhf_cname='JAVA');
-- (7)	删除所有选修课程JAVA的选修课记录。




DELETE FROM liuhf_reports WHERE lhf_cno IN (SELECT lhf_cno FROM liuhf_courses WHERE lhf_ccredit<=4);
DELETE FROM liuhf_courses WHERE lhf_ccredit<=4;
-- (8)	对Courses表做删去学分<=4的元组操作，讨论该操作所受到的约束。

-- 由于该表已经与liuhf_reports表产生关联，
-- 所有要先删除，liuhf_reports中的与学分小于等于4的课程的相关记录才可以
```







