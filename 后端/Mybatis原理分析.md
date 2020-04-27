# Mybatis 原理分析
## 1.MYBATIS 框架核心基础
### 1.1.MYBATIS 架构分析
#### 1.1.1.Mybatis 应用架构(重点)
  MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github。<br/>
  iBATIS一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAOs）<br/>
  当前，最新版本是MyBatis 3.5.3 ，其发布时间是2019年10月20日。<br/>
  谈谈对mybatis的应用架构的理解？<br/>
  MyBatis 是一个优秀的持久层框架，实现了对JDBC操作（标准API）的封装，主要用于传统简化JDBC操作中的一些相对繁琐的步骤，例如参数的映射，结果集的映射（数据库中记录存储到内存中的对象中）等。

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-560a50a7839f4872849ab1ccbfad6469.png)

为何使用mybatis实现数据持久层应用？<br/>
  1)第一稳定，灵活（动态SQL），功能强大(池，日志，缓存)<br/>
  2)学习以及使用成本低<br/>
  3)解耦，SQL的可维护性，可复用性比较高。<br/>
说明:mybatis官网参考<br/>
  1)mybatis.org/mybatis-3<br/>
  2)github.com/mybatis<br/>
  http://www.mybatis.org/spring/<br/>
#### 1.1.2.Mybatis 产品架构

  所有框架都要解决一些共性问题（持久化），都是一种半成品，mybatis也不例外，它作为一种框架，它要解决相关问题，如何解决问题？采用怎样的架构解决问题，这是我们要学习的一个点。<br/>

  ---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-ce73a35989d24609bea1e690e4fefbe8.png)![](http://182.92.171.224/upload/20191123_11104417.png)
思考:mybatis作为一个持久层框架,应该解决哪些功能性问题?<br/>
  1)会话功能 (SqlSession)<br/>
  2)会话语言 (SQL,动态SQL)<br/>
  3)会话协议 (TCP)<br/>
  4)用户体验 (连接池,缓存,日志)<br/>
#### 1.1.3.MyBatis 技术架构
___
Mybatis 框架"构成"分析,如下图所示:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-a20f8c95d3b9434ebc3b31b227e67ad1.png)


资源配置技术架构,如下图所示:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-06be9ff35c7c4e91b120bcd7037e6724.png)

会话工厂对象创建(说说SqlSessionFactory对象的创建过程),如下图所示:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-bcc803a719c34a9ba811d459227e8a5b.png)

会话对象创建？(说说sqlsession对象创建过程),如下图所示:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-b694dc22593a49fba51c7e8e797825fc.png)

会话对象应用方式,如下图所示:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-a42b6b7b51a644f5b2e4d05d2ab4e37d.png)
### 1.2.MYBATIS 环境配置实践
#### 1.2.1.初始化数据环境
本次数据初始化直接在mysql的命令行执行:<br/>
1.启动系统命令行控制台<br/>
2.登陆数据库:mysql -u root -p<br/>
3.设置客户端编码：set names utf8<br/>
4.导入数据：source d:/xxx.sql<br/>
说明：查询时，假如有中文要显示，可先执行set names gbk.<br/>
#### 1.2.2.创建并配置项目
1.打开IDE并进行配置(新的工作区)<br/>
1)配置工作区编码(utf-8)<br/><br/>
2)配置maven环境(本地库,私服配置)<br/>

2.创建maven项目并添加依赖<br/><br/>
项目名称:CGB-MYBATIS-01<br/>
打包方式:jar包<br/>
组id:com.cy<br/>

MySQL驱动依赖(假如驱动版本与当前数据库不一致可能会有问题)


     	<dependency>
      		<groupId>mysql</groupId>
    	    <artifactId>mysql-connector-java</artifactId>
      		<version>8.0.17</version>
      	</dependency>

Mybatis 框架依赖（参考官方 mybatis.org/mybatis-3）


      	<dependency>
      		<groupId>org.mybatis</groupId>
      		<artifactId>mybatis</artifactId>
      		<version>3.5.2</version>
      	</dependency>

Junit单元测试依赖


    <dependency>
      		<groupId>junit</groupId>
      		<artifactId>junit</artifactId>
      		<version>4.12</version>
      	</dependency>

说明:学了spring boot以后还有一种测试方式<br/>
3.配置项目<br/>
在src/main/resources目录下创建mybatis-configs.xml配置文件如下：


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE configuration
      PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <!-- mybatis 核心配置 -->
    <configuration>
      <!-- 配置初始化环境(连接) -->
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/>
          <!-- 使用mybatis自带连接池 -->
          <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url"  value="jdbc:mysql:///dbgoods?serverTimezone=GMT"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
          </dataSource>
        </environment>
      </environments>
    </configuration>

其模板：参考官方配置实现（Getting start ）
#### 1.2.3.项目环境单元测试
在测试包中，创建测试类，测试是否可以获取与数据库的连接<br/>
```java
public class TestBase {
	/**
	 * 借助此对象创建SqlSession(通过此对象
	 * 实现与数据库之间的会话)
	 */
	protected SqlSessionFactory factory;
    /**
     * 此方会在@Test注解修饰的方法之前执行,
     * 通常用于做一些初始化操作(方法名自己定义)
     */
	@Before
	public void init()throws IOException{
		InputStream in=
Resources.getResourceAsStream("mybatis-configs.xml");
		factory=new SqlSessionFactoryBuilder().build(in);
		//系统底层建造者模式构建工厂对象(此对象构建过程相对复杂)
		System.out.println(factory);
	}
	@Test
	public void testSqlSessionConnection(){
		SqlSession session=factory.openSession();
		Connection conn=session.getConnection();
		System.out.println(conn);
	}
}
```

API 创建过程分析:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-dc8bb262568b4dea95913f0c517f4486.png)
### 1.3.MYBATIS业务应用快速实践
参考官网：www.mybatis.org/mybatis-3 实现对商品模块数据的CRUD操作。
#### 1.3.1.基于业务实现

添加业务实现(以添加商品信息为例进行实现)<br/>
第一步：编写pojo对象(com.cy.pj.goods.pojo.Goods),<br/>
借助此对象存储内存中的数据,最后将内存中的数据持久化到数据库.<br/>

```java
package com.cy.pj.goods.pojo;
import java.io.Serializable;
import java.util.Date;
/**
 * POJO:借助此对象在内存中封装商品信息
 */
public class Goods implements Serializable{
	private static final long serialVersionUID = 6239917530570596544L;
	private Long id;
	private String name;
	private String remark;
	private Date createdTime;
	public Long getId() {
		return id;
	}
	public void setId(Long id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getRemark() {
		return remark;
	}
	public void setRemark(String remark) {
		this.remark = remark;
	}
	public Date getCreatedTime() {
		return createdTime;
	}
	public void setCreatedTime(Date createdTime) {
		this.createdTime = createdTime;
	}

}
```
第二步:编写映射文件(mapper/GoodsMapper.xml),实现内存中对象(一般是pojo对象)与数据库表的映射(例如属性与字段进行映射)


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.cy.pj.goods.dao.GoodsDao">
        <insert id="insertObject"
                parameterType="com.cy.pj.goods.pojo.Goods">
            insert into tb_goods
            (id,name,remark,createdTime)
            values
            (#{id},#{name},#{remark},now())
        </insert>
    </mapper>
第三步:注册映射文件(mybatis-configs.xml)，在配置文件中添加如下语句：


    <mappers>
         <mapper resource="mapper/GoodsMapper.xml"/>
      </mappers>\
第四步:定义单元测试类(TestGoodsDao)及方法进行单元测试
```java
package com.test;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import com.cy.pj.goods.pojo.Goods;
public class TestGoodsDao01 extends TestBaseWithXml {
	@Test
	public void testInsertObject() {
		//1.创建Goods对象
		Goods g=new Goods();
		g.setId(14L);
		g.setName("IO");
		g.setRemark("IO...");
		//2.将Goods对象写入到数据库
		//2.1获取SqlSession对象
		SqlSession session=
		sqlSessionFactory.openSession();
		//2.2将对象持久化
		String statement="com.cy.pj.goods.dao.GoodsDao.insertObject";
		int rows=session.insert(statement, g);
		System.out.println("insert.rows=+"+rows);
		//2.3提交事务
		session.commit();
		//2.4释放资源
		session.close();
	}
	
```
修改业务实现(以修改商品信息为例进行实现)<br/>
第一步：修改映射文件(mapper/GoodsMapper.xml),添加update元素实现信息修改操作语句的定义.


        <update id="updateObject">
            update tb_goods
            set name=#{name},
                remark=#{remark}
            where id=#{id}
        </update>
第二步:修改单元测试类(TestGoodsDao01),添加修改测试方法.
```java
@Test
	public void testUpdateObject() {
		//1.创建Goods对象
		Goods g=new Goods();
		g.setId(14L);
		g.setName("Thread");
		g.setRemark("Thread...");
		//2.将Goods对象写入到数据库
		//2.1获取SqlSession对象
		SqlSession session=
				sqlSessionFactory.openSession();
		//2.2将对象持久化
		String statement="com.cy.pj.goods.dao.GoodsDao.updateObject";
		int rows=session.update(statement, g);
		System.out.println("update.rows=+"+rows);
		//2.3提交事务
		session.commit();
		//2.4释放资源
		session.close();
	}
}
```
1)删除业务实现(以删除作者信息为例进行实现)<br/>
step01:修改映射文件(mapper/GoodsMapper.xml),添加delete元素,实现基于id进行删除操作语句的定义.


     <delete id="deleteById">
           delete from tb_goods
           where id=#{id}
      </delete>
step02:修改单元测试类(TestGoodsDao01),添加删除测试方法
```java
@Test
	public void testDeleteById() {
		//1.获取session对象
		SqlSession session = 
		sqlSessionFactory.openSession();
		//2.执行删除操作
		String statement=
		"com.cy.pj.goods.dao.GoodsDao.deleteById";
		try {
		int rows=session.delete(statement, 10);
		System.out.println("delete.rows="+rows);
		//3.提交事务
		session.commit();
		//4.释放资源
		}finally {
		session.close();
		}
	}
```

2)查询业务实现(以查询某个商品信息为例进行实现)<br/>
step01:修改映射文件(mapper/GoodsMapper.xml),添加select元素,实现基于id查询作者信息语句的定义.


       <select id="selectById"
                resultType="com.cy.pj.goods.pojo.Goods">
           select id,name,remark,createdTime
           from tb_goods
           where id=#{id}
        </select>
step02:修改单元测试类(TestGoodsDao01),添加基于id执行查询的操作
```java
@Test
	public void testSelectById() {
		//1.获取session对象
		SqlSession session = 
				sqlSessionFactory.openSession();
		//2.执行删除操作
		String statement=
				"com.cy.pj.goods.dao.GoodsDao.selectById";
		try {
			Goods g=session.selectOne(statement,11);
			System.out.println(g);
			//3.提交事务
			session.commit();
			//4.释放资源
		}finally {
			session.close();
		}
	}
```
3)分页查询实现(以查询多个商品信息为例进行实现)<br/>
step01:修改映射文件(mapper/GoodsMapper.xml),添加select元素,实现基于分页条件查询作者信息语句的定义.


    <select id="findPageObjects"
                resultType="com.cy.pj.goods.pojo.Goods">
             select *
             from tb_goods
             limit #{startIndex},#{pageSize}
        </select>
step02:修改单元测试类(TestGoodsDao01),添加基于分页条件执行查询.
```java
@Test
public void testFindPageObjects() { 
		//1.获取session对象
		SqlSession session = 
		sqlSessionFactory.openSession();
		//2.执行删除操作
		String statement=
		"com.cy.pj.goods.dao.GoodsDao.findPageObjects";
		try {
		Map<String,Object> map=new HashMap<String,Object>();
		map.put("startIndex", 0);
		map.put("pageSize", 3);
		List<Goods> list=
		session.selectList(statement,map); 
		System.out.println(list);
		//3.提交事务
		session.commit();//connection.commint
		//4.释放资源
		}finally {
		session.close();
		}
	}
```
4)删除多条数据(以删除商品信息为例进行删除,借助动态sql)<br/>
step01:修改映射文件(mapper/GoodsMapper.xml),添加delete元素,实现基于多个ID删除作者信息的语句的定义.


       <delete id="deleteObjects">
           delete from tb_goods
           <where> 
             <!-- foreach元素用于迭代一个数组或集合 -->
             <foreach collection="array"
                    item="id">
                  or id=#{id}
             </foreach>
           </where>
        </delete>
step02:修改单元测试类(TestGoodsDao01),添加基于多个ID执行删除的操作.
```java
@Test
public void testDeleteObjects() { 
		//1.获取session对象
		SqlSession session = 
		sqlSessionFactory.openSession();
		//2.执行删除操作
		String statement=
		"com.cy.pj.goods.dao.GoodsDao.deleteObjects";
		try {
		int rows=session.delete(statement, new Object[] {13,3});
		System.out.println("delete.rows="+rows);
		//3.提交事务
		session.commit();//connection.commint
		//4.释放资源
		}finally {
		session.close();
		}}
```

#### 1.3.2.基本业务进阶实现
基于接口方式实现数据访问操作<br/>
1)分页查询实现(以查询多个作者信息为例进行实现)<br/>
第一步: 创建一个接口GoodsDao 定一个分页查询方法
```java
package com.cy.pj.goods.dao;
import java.util.List;
import org.apache.ibatis.annotations.Param;
import com.cy.pj.goods.pojo.Goods;
public interface GoodsDao {
	 /**
	    * 执行数据的删除操作
	  * @param ids
	  * @return
	  */
	 int deleteObjects(int... ids);
}
```
第二步: 创建一个单元测试类TestGoodsDao02对分页查询进行单元测试
```java
public class TestGoodsDao02 extends TestBaseWithXml {
	@Test
	public void testDeleteObjects() { 
		//1.获取session对象
		SqlSession session = 
		sqlSessionFactory.openSession();
		try {
		//2.执行删除操作
		GoodsDao gDao=
		session.getMapper(GoodsDao.class);
		int rows=gDao.deleteObjects(11,12);
		System.out.println("delete.rows="+rows);
		//3.提交事务
		//session.commit();//connection.commint
		//4.释放资源
		}finally {
		session.close();
		}
	}
}
```
2)基于多个id进行删除 (以删除作者信息为例进行实现)<br/>
第一步：修改GoodsDao接口,添加deleteObjects方法


    	 int deleteObjects(int... ids);
第二步: 修改单元测试类,添加testDeleteObjects方法实现单元测试.
```java
@Test
	public void testDeleteObjects() { 
		//1.获取session对象
		SqlSession session = 
		sqlSessionFactory.openSession();
		try {
		//2.执行删除操作
		GoodsDao gDao=
		session.getMapper(GoodsDao.class);
		int rows=gDao.deleteObjects(11,12);
		System.out.println("delete.rows="+rows);
		//3.提交事务
		//session.commit();//connection.commint
		//4.释放资源
		}finally {
		session.close();
		}
	}
```
基于注解(Annotation)方式做业务实现?<br/>
第一步：在GoodsDao接口中添加统计记录行数的方法：getRowCount()<br/>


    int getRowCount();<br/>
第二步：在GoodsDao接口的统计记录方法上添加@Select注解，定义查询语句。<br/>


    @Select("select count(*) from tb_goods")
    int getRowCount();
第三步：在TestGoodsDao02测试类中添加测试方法。
```java
@Test
	public void testGetRowCount() {
		SqlSession session=
		sqlSessionFactory.openSession();
		try {
		GoodsDao dao=
		session.getMapper(GoodsDao.class);
		int rowCount=dao.getRowCount();
		System.out.println(rowCount);
		session.commit();
		}finally {
		session.close();
		}
	}
```
#### 1.3.3.会话工厂对象创建增强(了解)
___
基于非XMl方式构建SqlSessionFactory对象?
```java
package com.test;
import com.cy.pj.goods.dao.GoodsDao;
/**
 * Building SqlSessionFactory without XML
 */
public class TestBaseWithJava {
    protected SqlSessionFactory sqlSessionFactory;
	@Before
	public void init() {
		//1.构建数据源对象
		PooledDataSource dataSource=new PooledDataSource();
		dataSource.setDriver("com.mysql.cj.jdbc.Driver");
	dataSource.setUrl("jdbc:mysql:///dbgoods?serverTimezone=GMT&characterEncoding=utf8");
		dataSource.setUsername("root");
		dataSource.setPassword("root");
		//2.创建事务管理工厂
		JdbcTransactionFactory transactionFactory=
		new JdbcTransactionFactory();
		//3.创建一个环境对象
		Environment env= new Environment("development",
		transactionFactory, dataSource);
		//4.创建配置对象
		Configuration config=
		new Configuration(env);
		config.addMapper(GoodsDao.class);
		//5.创建sqlSessionFactory对象
		sqlSessionFactory=
        new SqlSessionFactoryBuilder()
		.build(config);
	}
	@Test
	public void testConnection(){
		Connection conn=
		sqlSessionFactory.openSession()
		.getConnection();
		System.out.println(conn);
	}
}
```

测试实现
```java
package com.test;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import com.cy.pj.goods.dao.GoodsDao;
/**
 * 	基于接口方式进行测试实现
 */
public class TestGoodsDao03 
     extends TestBaseWithJava {
	@Test
	public void testGetRowCount() {
		SqlSession session=
		sqlSessionFactory.openSession();
		try {
		GoodsDao dao=
		session.getMapper(GoodsDao.class);
		int rowCount=dao.getRowCount();
		System.out.println(rowCount);
		session.commit();
		}finally {
		session.close();
		}
	}
}
```
### 1.4.MYBATIS 应用原理进阶分析
#### 1.4.1.会话工厂创建分析(了解)
___
会话工厂(SqlSessionFactory)创建过程分析:基于xml方式

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-454b49d31ee043ceb7e67d1cc5848800.png)
1.通过IO读取配置文件<br/>
2.解析IO数据并进行封装(所有信息都会存储到Configuration对象)<br/>
3.基于配置对象创建SqlSessionFactory对象<br/>

会话工厂(SqlSessionFactory)创建过程分析:不基基于xml方式

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-b3af8a61276444e7a1313cb634862e86.png)
### 1.4.2.会话对象应用分析 (了解)
___
说说mybatis对jdbc的封装过程（了解）？（SqlSession应用增强分析）

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-9eeb8dbe5a1e42fea794fcc251a9b054.png)
封装了连接获取过程？（Executor->JDBCTransactionConnection）<br/>
2)封装了Statement对象创建过程？(connection)<br/>
3)封装了sql的发送过程，参数的处理过程，结果的映射过程。<br/>
### 1.4.3.基于Mapper接口会话(了解)
例如：XxxMapper xm=session.getMapper(XxxDao.class);<br/>
当获取到mapper接口对应的实现类对象以后可以基于实现类底层执行sql操作。<br/>

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-2bf3be2cf81f4e3b9866452880e67a14.png)
FAQ?<br/>
1)不同通过getMapper方式获取指定接口实现类是否可以基于mybatis实现数据访问操作?(可以)<br/>
2)基于Mapper接口访问数据库库的优势,劣势?(解耦,可读性好但是性能相对较差,相对于直接通过sqlsession的selectList,update方法而言).<br/>

#### 1.4.4.缓存应用实现过程分析(掌握)
Mybatis 框架提供一种缓存机制，通过缓存的应用来提高查询性能,但可能会有一定的不一致(脏读)问题。Mybatis中的缓存提供一级缓存和二级缓存的实现,默认都是开启状态（可参考官网）。<br/>
1.Mybatis 一级缓存<br/>
Mybatis中的一级缓存有时又称为SqlSession级缓存，SqlSession关闭时一级缓存失效。在同一个SqlSession内部多次执行同一个查询，后续的查询会从此缓存取数据.一级缓存结构分析如下:<br/>
一级缓存基本架构应用分析:<br/>

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-994fdc20ec8f406b90944ffe8583ca93.png)
一级缓存代码架构分析:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-6e8c3b268e9b4227948e747aa9e19071.png)
说明：一级缓存的实现可查看BaseExecutor类中的localCache属性。

2.MyBatis二级缓存<br/>

Mybatis中的二级缓存有时又称跨session缓存，可在多个SqlSession间共享数据,假如要使用二级缓存，可在对应的mapper文件中借助cache元素进行配置(可参考官方映射文件配置),二级缓存架构分析如下:<br/>

二级缓存基本应用架构:<br/>

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-320d1aa0f9a14514867317e697c88ae1.png)
二级缓存代码结构分析:<br/>

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-15ee6b13b3c24b4e8fbb1b42b07b7651.png)
说明：<br/>
1)二级缓存的实现可查看CachingExecutor类中的query方法.<br/>
2)二级缓存底层采用装饰模式进行设计,例如:<br/>
new SynchronizedCache (new LoggingCache(…));<br/>
3)二级缓存中可以存储序列化对象(readWrite属性值为false时)<br/>
4)使用二级缓存可在映射文件中借助<Cache/>标签进行声明,也可在接口中借助注解@CacheNamespace对接口进行描述.<br/>
5)当映射配置有注解方式,也有xml方式时可通过缓存引用进行配置,例如.
@CacheNamespaceRef(name="com.cy.pj.googs.dao.GoodsDao ")<br/>