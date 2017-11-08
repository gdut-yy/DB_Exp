#实验报告——SQL语言使用
----

##一、实验目的

##二、实验平台

###1.数据库管理系统    
**MySQL 5.7.19**
![](images\20171108110436.png)

###2.可视化管理工具

###3.操作系统
**Windows 10**

##三、实验准备

##四、实验内容

###模式的定义与删除
【3.1】为用户 定义一个学生-课程模式S-T。   

![3.1](images\3_1.png)   

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
![3.12](images\3.12.1.png)     

###索引的建立与删除

【3.13】为学生-课程数据库中的Student、Course和SC三个表建立索引。其中Student表按学号升序建唯一索引，Course表按课程号升序建唯一索引，SC表按学号升序和课程号降序建唯一索引。    

运行结果：   
![3.13](images\3.13.1.png)      

【3.14】将SC表的SCno索引名改为SCSno。   

运行结果：   
![3.14](images\3.14.1.png)  

【3.15】删除Student表的Stusname索引。

运行结果：   
![3.15](images\3.15.1.png)   
操作失败！
原因：语法不正确
解决方法：见上图
   