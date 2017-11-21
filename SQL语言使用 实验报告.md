#实验报告——SQL语言使用
----

##一、实验目的

##二、实验平台

###1.数据库管理系统    
**MySQL 5.7.20**

###2.可视化管理工具

###3.操作系统
**Windows 10（企业版）**

##三、实验准备
**创建用户:**     

![](images\start.png)    

**中文编码问题：**   

![](images\utf8.png)    

##四、实验内容
###3.3数据定义

###模式的定义与删除
【3.1】为用户 定义一个学生-课程模式S-T。   

![3.1](images\3.1.1.png)   

操作失败！   
原因：MySql不支持创建模式。    

【3.2】CREATE SHCEMA AUTHORIZATION WANG;       

操作失败！   
原因：MySql不支持创建模式。    

【3.3】为用户 创建一个模式TEST，并且在其中定义一个表TAB1。    

	CREATE SHCEMA TEST AUTHORIZATION ZHANG;
	CREATE TABLE TAB1(
		COL1 SMALLINT,
		COL2 INT,
		COL3 CHAR(20),
		COL4 NUMERIC(10,3),
		COL5 DECIMAL(5,2)
	);   

操作失败！   
原因：MySql不支持创建模式。    

【3.4】DROP SCHEMA ZHANG CASCADE;    

操作失败！   
原因：MySql不支持创建模式。    

###基本表的定义、删除与修改

【3.5】建立一个“学生”表Student。    

	CREATE TABLE student(
		Sno CHAR(9) PRIMARY KEY,
		Sname CHAR(20) UNIQUE,
		Ssex CHAR(2),
		Sage SMALLINT,
		Sdept CHAR(20)
	);     
    
脚本文件：   
![3.5](images\3.5.0.png)   

运行结果：   
![3.5](images\3.5.1.png)   

![3.5](images\3.5.2.png)   

【3.6】建立一个“课程”表Course。    
	
	CREATE TABLE course(
		Cno CHAR(4) PRIMARY KEY,
		Cname CHAR(40) NOT NULL,
		Cpno CHAR(4),
		Ccredit SMALLINT,
		FOREIGN KEY(Cpno) REFERENCES Course(Cno)
	);
脚本文件：   
![3.6](images\3.6.0.png)   

【3.7】建立学生选课表SC。   

	CREATE TABLE SC(
		Sno CHAR(9),
		Cno CHAR(4),
		Grade SMALLINT,
		PRIMARY KEY(Sno,Cno),
		FOREIGN KEY(Sno) REFERENCES Student(Sno),
		FOREIGN KEY(Cno) REFERENCES Course(Cno)
	);
脚本文件：   
![3.7](images\3.7.0.png)     

通过desc查询表的结构：    
![3.7](images\3.7.1.png)     

----
**本例开始,由于书本的例程中逻辑有误:加了外键约束后，表不能插入数据，此处调整一下顺序。即插入数据后再增加外键约束。**     

	# 学生-课程数据库 脚本
	# 书本的例程中逻辑有误：加了外键约束后，表不能插入数据，此处调整一下顺序。即插入数据后再增加外键约束。
	
	# Student表
	CREATE TABLE student(
		Sno CHAR(9) PRIMARY KEY,
		Sname CHAR(20) UNIQUE,
		Ssex CHAR(2),
		Sage SMALLINT,
		Sdept CHAR(20)
	);
	
	INSERT INTO student VALUES ('201215121', '李勇', '男', 20, 'CS');
	INSERT INTO student VALUES ('201215122', '刘晨', '女', 19, 'CS');
	INSERT INTO student VALUES ('201215123', '王敏', '女', 18, 'MA');
	INSERT INTO student VALUES ('201215125', '张立', '男', 19, 'IS');
	
	# Course表
	CREATE TABLE course(
		Cno CHAR(4) PRIMARY KEY,
		Cname CHAR(40) NOT NULL,
		Cpno CHAR(4),
		Ccredit SMALLINT
	);
	
	INSERT INTO course VALUES ('1', '数据库', '5', 4);
	INSERT INTO course VALUES ('2', '数学', NULL, 2);
	INSERT INTO course VALUES ('3', '信息系统', '1', 4); 
	INSERT INTO course VALUES ('4', '操作系统', '6', 3); 
	INSERT INTO course VALUES ('5', '数据结构', '7', 4);
	INSERT INTO course VALUES ('6', '数据处理', NULL, 2);
	INSERT INTO course VALUES ('7', 'PASCAL语言', '6', 4);
	
	ALTER TABLE course ADD FOREIGN KEY(Cpno) REFERENCES Course(Cno);
	
	# SC表
	CREATE TABLE SC(
		Sno CHAR(9),
		Cno CHAR(4),
		Grade SMALLINT,
		PRIMARY KEY(Sno,Cno)
	);
	
	INSERT INTO SC VALUES ('201215121', '1', 92);
	INSERT INTO SC VALUES ('201215121', '2', 85);
	INSERT INTO SC VALUES ('201215121', '3', 88);
	INSERT INTO SC VALUES ('201215122', '2', 90);
	INSERT INTO SC VALUES ('201215122', '3', 80);
	
	ALTER TABLE SC ADD FOREIGN KEY(Sno) REFERENCES Student(Sno);
	ALTER TABLE SC ADD	FOREIGN KEY(Cno) REFERENCES Course(Cno);    
         

【3.8】向Student表增加“入学时间”列，其数据类型为日期型。   

运行结果：   
![3.8](images\3.8.1.png)    
     
【3.9】将年龄的数据类型由字符型（假设原来的数据类型是字符型）改为整数。    

![3.9](images\3.9.1.png)    
结果出现错误！    
解决方法：    
改成：   

	alter table student modify Sage int;

运行结果：   
![3.9](images\3.9.2.png)   

【3.10】增加课程名称必须取唯一值的约束条件。   

运行结果：   
![3.10](images\3.10.1.png)    

【3.11】删除Studnet表。     
	
	drop table student CASCADE;     

运行结果：   
![3.11](images\3.11.1.png)   

【3.12】若表上建有视图，选择RESTRICT时表不能删除；选择CASCADE时可以删除表，视图也自动被删除。    

脚本文件：   
![3.12](images\3.12.0.png)     

运行结果：   
![3.12](images\3.12.3.png)     

###索引的建立与删除

【3.13】为学生-课程数据库中的Student、Course和SC三个表建立索引。其中Student表按学号升序建唯一索引，Course表按课程号升序建唯一索引，SC表按学号升序和课程号降序建唯一索引。    

脚本文件:     
![3.13](images\3.13.0.png)   

运行结果：   
![3.13](images\3.13.2.png)      

【3.14】将SC表的SCno索引名改为SCSno。   
  
运行结果：   
![3.14](images\3.14.2.png)   
操作失败！    
原因：语法不正确     
解决方法：见上图     

【3.15】删除Student表的Stusname索引。

运行结果：   
![3.15](images\3.15.1.png)   
操作失败！     
原因：语法不正确    
解决方法：见上图    

----
###3.4数据查询

###单表查询          
【3.16】查询全体学生的学号与姓名。       
![3.16](images\3.16.1.png)      

【3.17】查询全体学生的姓名、学号、所在系。      
![3.17](images\3.17.1.png)       

【3.18】查询全体学生的详细记录。        
![3.18](images\3.18.1.png)      

【3.19】查询全体学生的姓名及其出生年份。     
![3.19](images\3.19.1.png)    

【3.20】查询全体学生的姓名、出生年份和所在的院系，要求用小写字母表示系名。       
![3.20](images\3.20.1.png)    

用户可以通过指定别名来改变查询结果的列标题，这对于含算术表达式、常量、函数名的目标列表达式尤为有用。       
![3.20](images\3.20.2.png)       

----

【3.21】查询选修了课程的学生学号。    
![3.21](images\3.21.2.png)    

该查询结果里包含了许多重复的行。如想去掉结果表中的重复行，必须指定DISTINCT：         
![3.21](images\3.21.2.png)    

【3.22】查询计算机科学系全体学生的名单。    
![3.22](images\3.22.1.png)    

【3.23】查询所有年龄在20岁以下的学生姓名及其年龄。     
![3.23](images\3.23.1.png)    

【3.24】查询考试成绩不及格的学生的学号。      
![3.24](images\3.24.1.png)   

【3.25】查询年龄在20~23岁（包括20岁和23岁）之间的学生的姓名、系别和年龄。        
![3.25](images\3.25.1.png)   

【3.26】查询年龄不在20~23岁之间的学生姓名、系别和年龄。   
![3.26](images\3.26.1.png)   

【3.27】查询计算机科学系（CS）、数学系（MA）和信息系（IS）学生的姓名和性别。    
![3.27](images\3.27.1.png)   

【3.28】查询既不是计算机科学系、数学系，也不是信息系的学生的姓名和性别。      
![3.28](images\3.28.1.png)   

【3.29】查询学号为201215121的学生的详细情况。      
![3.29](images\3.29.1.png)   

【3.30】查询所有姓刘的学生的姓名、学号和性别。       

脚本文件：      
![3.30](images\3.30.0.png)    

运行结果：     
![3.30](images\3.30.1.png)    

【3.31】查询姓“欧阳”且全名为三个汉字的学生的姓名。    

脚本文件：      
![3.31](images\3.31.0.png)    

运行结果：     
![3.31](images\3.31.1.png)    

【3.32】查询名字中第二个字为“阳”的学生的姓名和学号。    

脚本文件：      
![3.32](images\3.32.0.png)    

运行结果：     
![3.32](images\3.32.1.png)  

【3.33】查询所有不姓刘的学生的姓名、学号和性别。     

脚本文件：      
![3.33](images\3.33.0.png)    

运行结果：     
![3.33](images\3.33.1.png)  

【3.34】查询DB_Design课程的课程号和学分。       

由于Course中没有DB_Design，现添加上去：        

![3.34](images\3.34.png)    

脚本文件：    
![3.34](images\3.34.0.png)    
运行结果：    
![3.34](images\3.34.1.png)    
操作失败！     
原因：语法不正确。在MySQL中，缺省的转义符为\，当用ESCAPE'\'时并没有给出新的转义符。这是因为MySQL use C escape syntax in strings，就是说上面的转义字符仅仅是给出了一个'，按题目的意思，正确的应该是ESCAPE'\\'，这样两个\\被解释为一个\。      
  
解决方法：      
![3.34](images\3.34.2.png)     
运行结果：    
![3.34](images\3.34.3.png)   

【3.35】查询以“DB_”开头，且倒数第三个字符为i的课程的详细情况。      

脚本文件：    
![3.35](images\3.35.0.png)    
运行结果：    
![3.35](images\3.35.1.png)      
操作失败！     
原因：语法不正确。在MySQL中，缺省的转义符为\，当用ESCAPE'\'时并没有给出新的转义符。这是因为MySQL use C escape syntax in strings，就是说上面的转义字符仅仅是给出了一个'，按题目的意思，正确的应该是ESCAPE'\\'，这样两个\\被解释为一个\。      
  
解决方法：      
![3.35](images\3.35.2.png)     
运行结果：    
![3.35](images\3.35.3.png)   

【3.36】某些学生选修课程后没有参加考试，所以有选课记录，但没有考试成绩。查询缺少成绩的学生的学号和相应的课程号。    
![3.36](images\3.36.1.png)   

【3.37】查所有有成绩的学生学号和课程号。    
![3.37](images\3.37.1.png)   

【3.38】查询计算机科学系年龄在20岁以下的学生姓名。   
![3.38](images\3.38.1.png)   

---

【3.39】查询选修了3号课程的学生的学号及其成绩，查询结果按分数的降序排列。    
![3.39](images\3.39.1.png)

【3.40】查询全体学生情况，查询结果按所在系的系号升序排列，同一系中的学生按年龄降序排列。     
![3.40](images\3.40.1.png)     

---

【3.41】查询学生总人数。      
![3.41](images\3.41.1.png)

【3.42】查询选修了课程的学生人数。      
![3.42](images\3.42.1.png)

【3.43】计算选修1号课程的学生平均成绩。      
![3.43](images\3.43.1.png)

【3.44】查询选修1号课程的学生最高分数。     
![3.44](images\3.44.1.png)

【3.45】查询学生201215012选修课程的总学分数。       
![3.45](images\3.45.1.png)     

----

【3.46】求各个课程号及相应的选课人数。      
![3.46](images\3.46.1.png)

【3.47】查询选修了三门以上课程的学生学号。      
![3.47](images\3.47.1.png)

【3.48】查询平均成绩大于等于90分的学生学号和平均成绩。       
![3.48](images\3.48.1.png)

----
###连接查询

【3.49】查询每个学生及其选修课程的情况。       
![3.49](images\3.49.1.png)     

【3.50】对例3.49用自然连接完成。       
![3.50](images\3.50.1.png)    

【3.51】查询选修2号课程且成绩在90分以上的所有学生的学号和姓名。      
![3.51](images\3.51.1.png)    

【3.52】查询每一门课的间接先修课（即先修课的先修课）。       
![3.52](images\3.52.1.png)    

【3.53】
SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade
FROM Student LEFT OUTER JOIN SC ON (Student.Sno=SC.Sno);      
![3.53](images\3.53.1.png)    

【3.54】查询每个学生的学号、姓名、选修的课程名及成绩。        
![3.54](images\3.54.1.png)    

###嵌套查询

【3.55】查询与“刘晨”在同一个系学习的学生。     

脚本文件：      
![3.55](images\3.55.0.png)    

运行结果：     
![3.55](images\3.55.1.png)  

【3.56】查询选修了课程名为“信息系统”的学生学号和姓名。   

脚本文件：      
![3.56](images\3.56.0.png)    

运行结果：     
![3.56](images\3.56.1.png)    

【3.57】找出每个学生超过他自己选修课程平均成绩的课程号。      

脚本文件：      
![3.57](images\3.57.0.png)    

运行结果：     
![3.57](images\3.57.1.png)  

【3.58】查询非计算机科学系中比计算机科学系任意一个学生年龄小的学生姓名和年龄。      

脚本文件：      
![3.58](images\3.58.0.png)    

运行结果：     
![3.58](images\3.58.1.png)  

【3.59】查询非计算机科学系中比计算机科学系所有学生年龄都小的学生姓名及年龄。        

脚本文件：      
![3.59](images\3.59.0.png)    

运行结果：     
![3.59](images\3.59.1.png)  

【3.60】查询所有选修了1号课程的学生姓名。         

脚本文件：      
![3.60](images\3.60.0.png)    

运行结果：     
![3.60](images\3.60.1.png)  

【3.61】查询没有选修1号课程的学生姓名。       

脚本文件：      
![3.61](images\3.61.0.png)    

运行结果：     
![3.61](images\3.61.1.png)  

【3.62】查询选修了全部课程的学生姓名。       

脚本文件：      
![3.62](images\3.62.0.png)    

运行结果：     
![3.62](images\3.62.1.png)  

【3.63】查询至少选修了学生201215122选修的全部课程的学生号码。        

脚本文件：      
![3.63](images\3.63.0.png)    

运行结果：     
![3.63](images\3.63.1.png)  

###集合查询

【3.64】查询计算机科学系的学生及年龄不大于19岁的学生。       

脚本文件：      
![3.64](images\3.64.0.png)    

运行结果：     
![3.64](images\3.64.1.png)  

【3.65】查询选修了课程1或者选修了课程2的学生。       

脚本文件：      
![3.65](images\3.65.0.png)    

运行结果：     
![3.65](images\3.65.1.png)  

----
【3.66】查询计算机科学系的学生与年龄不大于19岁的学生的交集。      

脚本文件：      
![3.66](images\3.66.0.png)    

运行结果：     
![3.66](images\3.66.1.png)  
操作失败！     
原因：语法不正确。MySQL不支持INTERSECT。  
  
解决方法：      
这实际上就是查询计算机科学系中年龄不大于19岁的学生。     

	SELECT * FROM Student WHERE Sdept='CS' AND Sage<=19;   
运行结果：    
![3.66](images\3.66.2.png)     

【3.67】查询既选修了课程1又选修了课程2的学生。就是查询选修课程1的学生集合与选修课程2的学生集合的交集。     

脚本文件：      
![3.67](images\3.67.0.png)    

运行结果：     
![3.67](images\3.67.1.png)  
操作失败！     
原因：语法不正确。MySQL不支持INTERSECT。   
  
解决方法：      
本例也可以表示为：    

	SELECT Sno FROM SC WHERE Cno='1' AND Sno IN(SELECT Sno FROM SC WHERE Cno='2');   
运行结果： 
![3.67](images\3.67.2.png)     

【3.68】查询计算机科学系的学生与年龄不大于19岁的学生的差集。    

脚本文件：      
![3.68](images\3.68.0.png)    

运行结果：     
![3.68](images\3.68.1.png)  
操作失败！     
原因：语法不正确。MySQL不支持EXCEPT。        
  
解决方法：      
也就是查询计算机科学系中年龄大于19岁的学生。   

	SELECT * FROM Student WHERE Sdept='CS' AND Sage>19;   
运行结果：   
![3.68](images\3.68.2.png)     

----
###3.5数据更新

###插入数据

【3.69】将一个新学生元组（学号：201215128，姓名：陈东，性别：男，所在系：IS，年龄：18岁）插入到Student中。       

脚本文件：      
![3.69](images\3.69.0.png)    

运行结果：     
![3.69](images\3.69.1.png)  

【3.70】将学生张成民的信息插入到Student表中。    

脚本文件：      
![3.70](images\3.70.0.png)    

运行结果：     
![3.70](images\3.70.1.png)    

【3.71】插入一条选课记录（'201215128','1'）。     

脚本文件：      
![3.71](images\3.71.0.png)    

运行结果：     
![3.71](images\3.71.1.png)    

【3.72】对每一个系，求学生的平均年龄，并把结果存入数据库。    

脚本文件：      
![3.72](images\3.72.0.png)    

运行结果：     
![3.72](images\3.72.1.png)    

###修改数据

**为了更直观地显示数据变化，故将查询语句加入到脚本文件中。**

【3.73】将学生201215121的年龄改为22岁。     

脚本文件：      
![3.73](images\3.73.0.png)    

运行结果：     
![3.73](images\3.73.1.png)    

【3.74】将所有学生的年龄增加1岁。     

脚本文件：      
![3.74](images\3.74.0.png)    

运行结果：     
![3.74](images\3.74.1.png)    

【3.75】将计算机科学系全体学生的成绩置零。    

脚本文件：      
![3.75](images\3.75.0.png)    

运行结果：     
![3.75](images\3.75.1.png)    

###删除数据

【3.76】删除学号为201215128的学生记录。    

脚本文件：      
![3.76](images\3.76.0.png)    

运行结果：     
![3.76](images\3.76.1.png)    

【3.77】删除所有的学生选课记录。    

脚本文件：      
![3.77](images\3.77.0.png)    

运行结果：     
![3.77](images\3.77.1.png)    

【3.78】删除计算机科学系所有学生的选课记录。    

脚本文件：      
![3.78](images\3.78.0.png)    

运行结果：     
![3.78](images\3.78.1.png)    

----

###3.6空值的处理

【3.79】向SC表中插入一个元组，学生号是“201215126”，课程号是“1”，成绩为空。      

脚本文件：      
![3.79](images\3.79.0.png)    

运行结果：     
![3.79](images\3.79.1.png)    

【3.80】将Student表中学生号为“201215200”的学生所属的系改为空值。      

脚本文件：      
![3.80](images\3.80.0.png)    

运行结果：     
![3.80](images\3.80.1.png)    

【3.81】从Student表中找出漏填了数据的学生信息。     

脚本文件：      
![3.81](images\3.81.0.png)    

运行结果：     
![3.81](images\3.81.1.png)    

【3.82】找出选修1号课程的不及格的学生。     

脚本文件：      
![3.82](images\3.82.0.png)    

运行结果：     
![3.82](images\3.82.1.png)    

【3.83】选出选修1号课程的不及格的学生以及缺考的学生。     

脚本文件：      
![3.83](images\3.83.0.png)    

运行结果：     
![3.83](images\3.83.1.png)    

----

###3.7视图

###定义视图

【3.84】建立信息系学生的视图。     

脚本文件：      
![3.84](images\3.84.0.png)    

运行结果：     
![3.84](images\3.84.1.png)    

【3.85】建立信息系学生的视图，并要求进行修改和插入操作时仍需保证该视图只有信息系的学生。     

脚本文件：      
![3.85](images\3.85.0.png)    

运行结果：     
![3.85](images\3.85.1.png)     






【3.86】建立信息系选修了1号课程的学生的视图（包括学号、姓名、成绩）。      

脚本文件：      
![3.86](images\3.86.0.png)    

运行结果：     
![3.86](images\3.86.1.png)    

【3.87】建立信息系选修了1号课程且成绩在90分以上的学生的视图。     

脚本文件：      
![3.87](images\3.87.0.png)    

运行结果：     
![3.87](images\3.87.1.png)    

【3.88】定义一个反映学生出生年份的视图。     

脚本文件：      
![3.88](images\3.88.0.png)    

运行结果：     
![3.88](images\3.88.1.png)    

【3.89】将学生的学号及平均成绩定义为一个视图。      

脚本文件：      
![3.89](images\3.89.0.png)    

运行结果：     
![3.89](images\3.89.1.png)    

【3.90】将Student表中所有女生记录定义为一个视图。    

脚本文件：      
![3.90](images\3.90.0.png)    

运行结果：     
![3.90](images\3.90.1.png)    
操作失败！     
原因：F_Student视图的属性列与Student表的属性列没有一一对应。        
  
解决方法：      
![3.90](images\3.90.2.png)    
  
运行结果：   
![3.90](images\3.90.3.png)     

###删除视图

【3.91】删除视图BT_S和视图IS_S1：     
![3.91](images\3.91.1.png)

###查询视图

【3.92】在信息系学生的视图中找出年龄小于20岁的学生。    
  
脚本文件：      
![3.92](images\3.92.0.png)    
  
运行结果：   
![3.92](images\3.92.1.png) 

【3.93】查询选修了1号课程的信息系学生。     
  
脚本文件：      
![3.93](images\3.93.0.png)    
  
运行结果：   
![3.93](images\3.93.1.png) 

【3.94】在S_G视图（例3.89中定义的视图）中查询平均成绩在90分以上的学生学号和平均成绩。    
  
脚本文件：      
![3.94](images\3.94.0.png)    
  
运行结果：   
![3.94](images\3.94.1.png)    


###更新视图

【3.95】将信息系学生视图IS_Student中学号为“201215122”的学生姓名改为“刘辰”。      
  
脚本文件：      
![3.95](images\3.95.0.png)    
  
运行结果：   
![3.95](images\3.95.1.png)    

【3.96】向信息系学生视图IS_Student中插入一个新的学生记录，其中学号为“201215129”，姓名为“赵新”，年龄为20岁。     
  
脚本文件：      
![3.96](images\3.96.0.png)    
  
运行结果：   
![3.96](images\3.96.1.png)    

【3.97】删除信息系学生视图IS_Student中学号为“201215129”的记录。     
  
脚本文件：      
![3.97](images\3.97.0.png)    
  
运行结果：   
![3.97](images\3.97.1.png)    

##实验总结

书上所说的SQL语句和实际操作的mysql语句还是有一定差别的。例如MySql中没有交操作intersect，差操作except。此外在实验过程中还遇到了如语法错误，外键问题，表重定义问题。



