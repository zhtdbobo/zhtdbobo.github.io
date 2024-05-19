# 触发器和存储过程


[toc]
# 存储过程procedure
### 创建存储过程 
```
create procedure mypro(in a int,in b int,out sum int) 
begin 
set sum = a+b; 
end;
call mypro(1,2,@s);-- 调用存储过程 
select @s;-- 显示过程输出结果
```
> call用来调用过程，@s 是用来接收过程输出参数的变量，
### 存储过程的参数
- IN 输入参数：表示调用者向过程传入值（可以是字面量或变量）
- OUT 输出参数：表示过程向调用者传出值(可返回多个值只能是变量）
- INOUT输入输出参数：既表示调用者向过程传入值，又表示过程向调用者传出值（值只能是变量）
### 存储过程根据参数可分为四种类别：
- 没有参数
- 只有输入参数
- 只有输出参数
- 输入和输出
### 变量定义
```
declare name varchar(20) default ‘jack’
```
- declare用于声明变量
- default用于声明默认值
### 变量赋值
- 使用 schooldb 数据库
	```
	use schooldb;
	```
- 创建过程
	```
	create procedure mypro1()
	begin
	declare name varchar(20);
	set name = '丘处机';
	select * from studentinfo where studentname = name;
	end;
	```
- 调用过程
	```
	call mypro1();
	```
### If条件语句
- 创建过程
	```
	create procedure mypro2(in num int)
	begin
	if num<0 then -- 条件开始
	select '负数';
	elseif num=0 then
	select '不是正数也不是负数';
	else
	select '正数';
	end if;-- 条件结束
	end;
	```
- 调用过程
	```
	call mypro2(-1);
	```
### Case条件语句
- 创建过程
	```
	create procedure mypro3(in num int)
	begin
	case -- 条件开始
	when num<0 then select '负数';
	when num=0 then select '不是正数也不是负数';
	else select '正数';
	end case; -- 条件结束
	end;
	```
- 调用过程
	```
	call mypro3(1);
	```
### While循环语句
- 创建过程
	```
	create procedure mypro5(out sum int)
	begin
	declare num int default 0;
	set sum = 0;
	while num<10 do -- 循环开始
	set num = num+1;
	set sum = sum+num;
	end while; -- 循环结束
	end;
	```
- 调用过程
	```
	call mypro5(@sum);
	```
- 查询变量值
	```
	select @sum;
	```
### Repeat循环语句（和do…while类似）
- 创建过程
	```
	create procedure mypro6(out sum int)
	begin
	declare num int default 0;
	set sum = 0;
	repeat-- 循环开始
	set num = num+1;
	set sum = sum+num;
	until num>=10
	end repeat; -- 循环结束
	end;
	```
- 调用过程
	```
	call mypro6(@sum);
	```
- 查询变量值
	```
	select @sum;
	```
###	Loop循环语句
> leave相当于break，用来终止循环；
> iterate相当于 continue，用来结束本次循环操作
- 创建过程
	```
	create procedure mypro7(out sum int)
	begin
	declare num int default 0;
	set sum = 0;
	loop_sum:loop-- 循环开始
	set num = num+1;
	set sum = sum+num;
	if num>=10 then
	leave loop_sum;
	end if;
	end loop loop_sum; -- 循环结束
	end;
	```
- 调用过程
	```
	call mypro7(@sum);
	```
- 查询变量值
	```
	select @sum;
	```
# 触发器trigger
###	定义
> 特殊的存储过程。不同的是，触发器无需人工调用，当程序满足定义条件时就会被MySQL自动调用。这些条件可以称为触发事件，包括：INSERT、UPDATE和DELETE操作。
###	创建触发器
```
CREATE TRIGGER trigger_name trigger_time trigger_event
ON table_name FOR EACH ROW 
trigger_body
```
- trigger_time：触发器触发时机，有before和after
- trigger_event：触发器触发事件insert,update,delete
- trigger_body：触发器主体语句
###	After触发器
> 指触发器监视的触发事件执行之后，再激活触发器，激活后所执行的操作无法影响触发器所监视的事件。
> New和old：指insert,delete,update操作执行前（后）所在表状态
> - Insert-只有new合法。新插入的行用new来表示，行中每一列的值用new.列名来表示
> - delete-只有old合法。删除的行用old来表示，行中每一列的值用old.列名来表示
> - update-被修改的行。修改前的数据（用old来表示，old.列名）修改后的数据（用new来表示，new.列名）
- Delete触发器
	创建一个触发器t_d_s，当删除表student中某个学生的信息时，同时将grade表中与该学生有关的数据全部删除
	```
	CREATE TRIGGER trigger_t1
	AFTER DELETE ON student
	FOR EACH ROW
	BEGIN 
	DELETE FROM grade WHERE studentid = old.studentid;
	END
	```
- Update触发器
	创建一触发器t_u_s，实现在更新学生表的学号时，同时更新grade表中的相关记录的studentid值
	```
	CREATE TRIGGER t_u_s
	AFTER UPDATE ON student
	for EACH ROW
	BEGIN
	UPDATE grade SET studentid = new.studentid WHERE studentid = old.studentid;
	END
	```
- Insert触发器
	创建一个触发器t_i_s，当student表插入新学生时，class表中该班级人数加1
	```
	CREATE TRIGGER t_i_s
	AFTER INSERT
	ON student
	for EACH ROW
	BEGIN
		UPDATE class SET studentnum = studentnum + 1 WHERE classid = new.classid;
	END;
	```
### Before触发器
> 指触发器在所监视的触发事件执行之前激活，激活后执行的操作先于监视的事件，这样就有机会进行一些判断，或修改即将发生的操作
before：(insert、update)可以对new进行修改
after不能对new进行修改，三者都不能修改old数据
- Insert触发器:
	建一个触发器t_d_t，插入教师信息时，如果教师工资小于3000，则自动调整成3000
	```
	CREATE TRIGGER tdt 
	BEFORE INSERT 
	ON teacher
	for each ROW
	BEGIN
		if new.salary <3000 THEN SET new.salary = 3000;
	END if;
	END;
	```
- Update触发器
	给grade表建立一个学分列，并创建一个触发器，当修改grade表中数据时，如果修改后的成绩小于60分，则触发器将该成绩对应的课程学分修改为0，否则将学分改成对应课程的学分
	```
	ALTER TABLE grade ADD credit int;
	CREATE TRIGGER trigger_ch 
	BEFORE UPDATE
	ON grade
	FOR EACH ROW
	BEGIN
		IF new.grade<60	THEN SET new.credit = 0;
		ELSE SET new.credit = (
			SELECT credit
			FROM coure
			WHERE courseid = new.courseid
		);
	END if;
	END
	UPDATE grade SET grade = 50 WHERE courseid ="Dp010001" AND studentid = "St0109010003"
	```
###	中断触发器
> 假设软件B1802班级最多只能有4个人，当往b_student表中增加新生信息时，b_class班级表内学生人数会随之增加，当人数大于4人时，由于超过人数限制，会报系统错误，错误提示为“超过人数限制”，并且该触发器所有操作（包括引发触发器的操作）均不能成功
- 创建b_classs表存放班级人数，如果班级人数大于4就提示"超出人数限制"
	```
	CREATE TABLE B_class(
		Cid VARCHAR(20) PRIMARY KEY  COMMENT "班级名称",
		num int COMMENT "人数"
	);
	```
- 插入初始数值，初始数值软件B1802班没有人
	```
	INSERT INTO b_class VALUES("软件B1802",0);
	```
- 创建一个b_student表存放学生信息
	```
	CREATE TABLE b_student(
		studentid VARCHAR(20) PRIMARY KEY COMMENT "学号",
		studentname VARCHAR(20) not null COMMENT "姓名",
		classid VARCHAR(20)  DEFAULT '软件B1802' COMMENT "班级",
		CONSTRAINT FK_ID FOREIGN KEY(classid) REFERENCES B_class(Cid)
		ON DELETE RESTRICT on UPDATE CASCADE
	)DEFAULT CHARSET = utf8;
	```
- 要注意的是，我们在插入学生信息的时候，我们要使用触发器来更新班级表中的人数，不然尽管你在学生信息表插入多条数据，班级表中的人数会一直保持不表，也就不会出现超出人数限制的情况。
	```
	CREATE TRIGGER add_trigger
	AFTER INSERT
	ON b_student
	for EACH ROW
	BEGIN
		UPDATE b_class SET num = num + 1 WHERE Cid = new.classid;
	END;
	```
- 创建抛出自定义异常的触发器，当插入学生人数超过4人时，就会抛出异常。
	```
	CREATE TRIGGER exception BEFORE INSERT ON b_student for each row
	BEGIN；
	DECLARE number int;
	SELECT num INTO number from b_class WHERE cid = new.classid;
		if number = 4  THEN	SIGNAL SQLSTATE '45000'
	SET message_text = '超出人数限制',MYSQL_ERRNO = 1333;
	END if;
	END;
	INSERT INTO b_student(studentid,studentname) VALUES('238','位傲气'),('239','阮氏问'),('240','王陇镇'),('250','周志豪');
	INSERT INTO b_student(studentid,studentname) VALUES('241','刘洋');
	```
