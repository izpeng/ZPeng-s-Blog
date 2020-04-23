-- -----------------------------------
一、创建建数据库、创建建数据表、查看数据库、查看数据表

-- -----------------------------------
-- 01.查看mysql服务器中所有数据库

	show databases;

-- 02.进入某一数据库（进入数据库后，才能操作库中的表和表记录）

	use test;
-- 查看已进入的库

	select database();

-- 03.查看当前数据库中的所有表

	show tables;
-- 04.删除mydb1库

	-- 语法：drop database 库名;
	drop database  mydb1;
	-- 思考：当删除的表不存在时，如何避免错误产生?
	drop database if exists mydb1;
-- 05.重新创建mydb1库，指定编码为utf8

	-- 语法：create database 库名 charset 编码;
	create database mydb1 charset utf8;

	-- 如果不存在则创建mydb1;
	create database mydb1 if not exists charset utf8; 
-- 06.查看建库时的语句（并验证数据库库使用的编码）

	-- 语法：show create database 库名;
	show create database mydb1;
-- 07.进入mydb1库，删除stu学生表(如果存在)

	-- 语法：drop table 表名;
	use mydb1
	drop table stu;
	drop table if exists stu;
-- 08.创建stu学生表（编号[数值类型]、姓名、性别、出生年月、考试成绩[浮点型]）

	/* 建表的语法：
	create table 表名(
		列名 数据类型,
		列名 数据类型,
		...
	); */
---
	create table if not exists stu(
		id int primary key auto_increment,
		name varchar(20),
		genger varchar(10),
		birthday date,
		score double
	);

-- 09.查看stu学生表结构

	show create table stu;
	desc stu;
-- -----------------------------------
二、新增、修改、删除表记录 **********
-- -----------------------------------
-- 10.往学生表(stu)中插入记录(数据)

	-- 插入记录：insert into 表名(列1，列2，列3...) values(值1，值2，值3...);
	insert into stu(id,name,genger,birthday,score) 	values(null,'赵伟成','女','2018-3-9',33);

	insert into stu values(null,'赵伟成','女','2018-3-9',33);
	insert into stu values(null,'张三','男','2018-3-9',11);
	insert into stu values(null,'卫程','女','2022-1-11',55);
	/* 提示：
	 设置编码：set names gbk;
	 查看MySQL数据库使用的编码：show variables like 'char%';
	 mysql --default-character-set=gbk -uroot -proot */
	 
-- 11.查询stu表所有学生的信息

	select * from stu;
	select id,name from stu;
-- 12.修改stu表中所有学生的成绩，加10分特长分

	-- 修改语法: update 表名 set 列=值,列=值,列=值...;
	update stu set score=score+10;
-- 13.修改stu表中王海涛的成绩，将成绩改为88分。\c取消当前语句的执行

	update stu set score=88
	where name = '卫程';
	
	/* 提示：where子句用于对记录进行筛选过滤，
	   保留符合条件的记录，将不符合条件的记录剔除。*/

-- 14.删除stu表中所有的记录

	-- 删除记录语法: delete from 表名 [where条件] 
	delete from stu;
	
	-- 仅删除符合条件的
	delete from stu where id= 1;
-- -----------------------------------
三、基础查询、where子句查询
-- -----------------------------------
-- 准备数据： 以下练习将使用db10库中的表及表记录，请先进入db10数据库!!!
-- 准备数据： 以下练习将使用db10库中的表及表记录，请先进入db10数据库!!!

-- ******** 基础查询 ********
-- 15.查询emp表中的所有员工，显示姓名，薪资，奖金

	use db10;
	select name,sal,bonus  
	from emp;
-- 16.查询emp表中的所有员工，显示所有列

	select * from emp;
	/* 使用 *（星号）的缺点：把不必要的列也查询出来了，而且效率不如直接指定列名 */

-- 17.查询emp表中的所有部门和职位

	select dept,job 
	from emp;

	/* 思考：如果查询的结果中，存在大量重复的记录，如何剔除重复记录，只保留一条？ */
	
	-- distinct 用于剔除重复的记录
	select distinct dept,job from emp;
-- ******** WHERE子句查询 ********
-- 18.查询emp表中薪资大于3000的所有员工，显示员工姓名、薪资

	select name,sal from emp
	where sal>3000;
-- 19.查询emp表中总薪资(薪资+奖金)大于3500的所有员工，显示员工姓名、总薪资

	select name,sal+ifnull(bonus,0) from emp
	where sal+ifnull(bonus,0)>3500;
	-- ifnull(列, 值)函数: 判断指定的列是否包含null值, 如果有null值, 用第二个值替换null值
select name,sal+ifnull(bonus,0) from emp

	where sal+ifnull(bonus,0)>3500;
	-- 注意查看上面查询结果中的表头，如何将表头中的 sal+bonus 修改为 "总薪资"，as可省略
select name  姓名,sal+ifnull(bonus,0) as 总薪资 from emp

	where sal+ifnull(bonus,0)>3500;

	-- 试一试：where中能使用定义好的别名吗?
	不能

	

-- 20.查询emp表中薪资在3000和4500之间的员工，显示员工姓名和薪资

	select name,sal from emp
	where sal>=3000&&sal<=4500;
	-- 提示: between...and... 在...之间
	select name,sal from emp
	where sal between 3200 and 4500;
-- 21.查询emp表中薪资为 1400、1600、1800的员工，显示员工姓名和薪资
select name,sal

	from emp
	where sal=1400 or sal=1600 or sal=1800;
	-- 或者
	select name,sal
	from emp
	where sal in(1400,1600,1800);
-- 22.查询薪资不为1400、1600、1800的员工

	select name,sal
	from emp
	where not (sal=1400 or sal=1600 or sal=1800);
	-- 或者
	select name,sal
	from emp
	where sal!=1400 and sal!=1600 and sal!=1800;
	-- 或者
	select name,sal
	from emp
	where sal not in(1400,1600,1800);
	
-- 23.查询emp表中薪资大于4000和薪资小于2000的员工，显示员工姓名、薪资。

	select name,sal
	from emp
	where sal>4000 or sal<2000;
-- 24.查询emp表中薪资大于3000并且奖金小于600的员工，显示员工姓名、薪资、奖金。

	select name,sal,bonus
	from emp
	where sal>3000 and bonus<600;
	-- 处理null值
	select name,sal,bonus
	from emp
	where sal>3000 and ifnull(bonus,0)<600;
	-- 处理null值

-- 25.查询没有部门的员工（即部门列为null值）

	select * from emp
	where dept is null;
	-- 思考：如何查询有部门的员工（即部门列不为null值）
	select * from emp
	where dept is not null;
	-- 思考：如何查询有部门的员工（即部门列不为null值）

-- ******** Like模糊查询 ********
-- 26.查询emp表中姓名中以"刘"开头的员工，显示员工姓名。

    select name from emp
    where name like'刘%';

	/* like进行模糊查询，"%" 表示通配，表示0或多个字符。
	"_"表示一个任意的字符 */
	select name from emp
    where name like'刘_';

-- 27.查询emp表中姓名中包含"涛"员工，显示员工姓名。\

	select name from emp
    where name like'%涛%';

-- 28.查询emp表中姓名以"刘"开头并且姓名为2个字的员工，显示员工姓名。

	select name from emp
    where name like'刘_';
-- -----------------------------------
-- 三、分组查询、聚合函数、排序查询
-- -----------------------------------
-- 29.对emp表按照部门对员工进行分组，查看分组后效果

	/* 分组的语法: 
	select 查询的列 from 表名 group by 列名
	根据指定的列进行分组 */
	select * from emp 
	group by dept;
-- 30.对emp表按照职位进行分组, 并统计每个职位的人数, 显示职位和对应人数

	select count(id) 人数,job 职位 
	from emp 
	group by job;
-- 31.对emp表按照部门进行分组, 求每个部门的最高薪资(不包含奖金)，显示部门名称和最高薪资 max() min(),

	select dept 部门,max(sal) 最高薪资
	from emp
	group by dept;
-- 32.统计emp表中薪资大于3000的员工个数（- count(column)统计某列的行数）

	select count(*) from emp
	where sal>3000;
	
-- 33.统计emp表中所有员工的薪资总和(不包含奖金)（- sum(column)对某列的值求和）

	select sum(sal) from emp ;
	
-- 34.统计emp表员工的平均薪资(不包含奖金)（- avg(column)对某列的值求平均值）

	select avg(sal) from emp ;
-- 35.查询emp表中所有在1993和1995年之间出生的员工，显示姓名、出生日期。

	select name,birthday 
	from emp
	where birthday between '1993-01-01' and '1995-12-31';

	-- 或者 year(),month(),day(),
	select name,birthday 
	from emp
	where year(birthday) between 1993 and 1995;
-- 36.查询本月过生日的所有员工

	select * from emp
	where month(curdate())= month(birthday);
	/* 
	curdate() 获取当前日期 年月日
	select curdate();
	curtime() 获取当前时间 时分秒
	select curtime();
	sysdate() 获取当前日期+时间 年月日 时分秒 
	select sysdate();
	select now();
	*/
-- -----------------------------------
-- **************** 排序查询 ***************
-- -----------------------------------

	/* order by 排序的列 asc 升序(从低到高)
	order by 排序的列 desc 降序(从高到低)
	默认就是升序，所以asc可以省略不写 */
-- 37.对emp表中所有员工的薪资进行升序(从低到高)排序，显示员工姓名、薪资。

	select name,sal from emp
	order by sal;
-- 38.对emp表中所有员工奖金进行降序(从高到低)排序，显示员工姓名、奖金。

	select name,bonus from emp
	order by bonus desc;
-- -----------------------------------
-- **************** 分页查询 ***************
-- -----------------------------------

	/* 在mysql中，通过limit进行分页查询：
	limit (页码-1)*每页显示记录数, 每页显示记录数 */
-- 39.查询emp表中的所有记录，分页显示：每页显示3条记录，返回第 1 页。

	select * from emp
	limit 0,3;
	select * from emp
	limit 6,3;
-- 40.查询emp表中的所有记录，分页显示：每页显示3条记录，返回第 2 页。

	select * from emp
	limit 3,3;
	
	select * from emp
	limit 9,3;
-- -----------------------------------
-- 三、外键
-- -----------------------------------
-- 准备数据： 以下练习将使用db20库中的表及表记录，请先进入db20数据库!!!
-- 准备数据： 以下练习将使用db20库中的表及表记录，请先进入db20数据库!!!
-- 41.尝试删除dept表中的某一个部门

	/* 上面的部门删除成功后，员工表里的某些员工就没有了对应的部门,
	这种我们称之为数据的完整性被破坏了,
	为了避免这种情况，可以在删除之前，查看将要删除的部门下是否还有员工存在，如果有就不要删除;
	或者，让数据库帮我们去维护这样的对应关系，也就是当将要被删除的部门下如果还有员工，
	就阻止删除操作，让数据库帮我们维护这样的对应关系，就需要指定外键。*/
	

-- 42.重新创建db20中的dept和emp表，在创建时，指定emp表中的dept_id列为外键，即这一列要严格参考dept表中的id列, 再次尝试删除dept表中的某一个部门，查看是否能删除成功

-- -----------------------------------
-- 四、关联查询、外连接查询
-- -----------------------------------
-- 准备数据： 以下练习将使用db30库中的表及表记录，请先进入db30数据库!!!
-- 准备数据： 以下练习将使用db30库中的表及表记录，请先进入db30数据库!!!

-- 43.查询部门和部门对应的员工信息

	select * from emp,dept
	where dept.id = emp.dept_id;
	--
	select * from dept inner join emp
	on dept.id = emp.dept_id;
-- 44.查询所有部门和部门下的员工，如果部门下没有员工，员工显示为null

	select * from dept left join emp
	on dept.id = emp.dept_id;
	
-- 45.查询部门和所有员工，如果员工没有所属部门，部门显示为null

	select * from dept right join emp
	on dept.id = emp.dept_id;

-- -----------------------------------
-- 五、子查询、多表查询
-- -----------------------------------
-- 准备数据：以下练习将使用db40库中的表及表记录，请先进入db40数据库!!!
-- 准备数据：以下练习将使用db40库中的表及表记录，请先进入db40数据库!!!

-- 46.列出薪资比'王海涛'薪资高的所有员工，显示姓名、薪资

	select name,sal from emp
	where sal>(
	select sal from emp
	where name= '王海涛'
	);
-- 47.列出与'刘沛霞'从事相同职位的所有员工，显示姓名、职位。

	select name,job from emp
	where job = (
	select job from emp
	where name = '刘沛霞'
	);
-- 48.列出薪资比'大数据部'部门(已知部门编号为30)所有员工薪资都高的员工信息，显示员工姓名、薪资和部门名称。

	select emp.name,sal,dept.name 
	from emp,dept
	where emp.dept_id = dept.id and sal>(
	    select max(sal) from emp,dept
	    where dept_id = 30 and emp.dept_id = dept.id
	);

	select emp.name,sal,dept.name 
	from emp left join dept
	on emp.dept_id = dept.id and 
	where sal>(
	    select max(sal) from emp
	    where dept_id = 30 
	);
-- 49.列出在'培优部'任职的员工，假定不知道'培优部'的部门编号， 显示部门名称，员工名称。

	-- 关联查询两张表
	-- 求出在培优部的员工
	select dept.name,emp.name 
	from emp,dept
	where emp.dept_id = dept.id and 
	   dept.name = '培优部';
	
-- 50.(自查询)列出所有员工及其直接上级，显示员工姓名、上级编号，上级姓名

	select e1.name,e2.topid,e2.name 
	from emp as e1,emp e2
	where e1.topid = e2.id;
-- 51.列出最低薪资大于1500的各种职位，显示职位和该职位最低薪资

	select job, min(sal)
	from emp
	group by job
	having min(sal)>1500;
-- 52.列出在每个部门就职的员工数量、平均工资。显示部门编号、员工数量，平均薪资。

	select dept_id,count(*),avg(sal)
	from emp
	group by dept_id;

-- 53.查出至少有一个员工的部门。显示部门编号、部门名称、部门位置、部门人数。

	-- 关联查询两张表(dept, emp)
	-- 替换要显示的列和统计部门人数
	select dept.id,dept.name,dept.loc,count(*)
	from emp,dept
	where emp.dept_id = dept.id 
	group by dept.name;
-- 54.列出受雇日期早于直接上级的所有员工的编号、姓名、部门名称。

	/* 
	列: e1.id, e1.name, d.name
	表:	emp e1: 员工表
		emp e2: 上级表
		dept d: 部门表
	关联条件: e1.topid=e2.id
			e1.dept_id=d.id
			e1.hdate<e2.hdate  */
	select e1.id, e1.name, d.name
	from emp e1,emp e2,dept d
	where e1.topid=e2.id and e1.dept_id=d.id
		and e1.hdate<e2.hdate;
-- 55.列出每个部门薪资最高的员工信息，显示部门编号、员工姓名、薪资

	-- 查询emp表中所有员工的部门编号、姓名、薪资
	-- 查询emp表中每个部门的最高薪资，显示部门编号、最高薪资
	-- 第二次查询的结果作为一张临时表和第一次查询进行关联查询
	
	select emp.dept_id, t1.ms,emp.name
	from emp,(
		select dept_id, max(sal) ms 
		from emp 
		group by dept_id) t1
	where emp.dept_id = t1.dept_id and t1.ms=emp.sal;
	
	





======================================
补充1、笛卡尔积查询：

	笛卡尔积查询：如果同时查询两张表，左边表有m条数据，右边表有n条数据，那么笛卡尔积查询是结果就是 m*n 条记录。这就是笛卡尔积查询。例如： 
	select * from dept,emp;
	上面的查询中包含大量错误的数据, 一般不使用这种查询。
	
	如果只想保留正确的记录，可以通过where条件进行筛选，将符合条件的保留下来，不符合条件的自然就会被剔除，例如：
	select * from dept,emp
	where dept.id=emp.dept_id;

补充2、左外连接和右外连接查询：

	(1) 左外连接查询：是将左边表中所有数据都查询出来, 如果在右边表中没有对应的记录, 右边表显示为null即可。
	(2) 右外连接查询：是将右边表中所有数据都查询出来, 如果在左边表中没有对应的记录, 左边表显示为null即可。

补充3、where和having都用于筛选过滤，但是： 

	(1) where用于在分组之前进行筛选, having用于在分组之后进行筛选
	(2) 并且where中不能使用列别名, having中可以使用别名
	(3) where子句中不能使用列别名(可以使用表别名), 因为where子句比select先执行!!
	
补充4、SQL语句的书写顺序和执行顺序:
  SQL语句的书写顺序：
  
	select...
	from...
	where...
	group by...
	order by...
	...
  SQL语句的执行顺序：
  
	from... -- 确定要查询的是哪张表 (定义表别名)
	where... -- 从整张表的数据中进行筛选过滤
	select... -- 确定要显示哪些列 (定义列别名)
	group by... -- 根据指定的列进行分组
	order by... -- 根据指定的列进行排序
	...
======================================