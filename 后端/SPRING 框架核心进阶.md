#  1.SPRING 框架核心进阶
##  1.1.SPRING 框架架构分析
###  1.1.1.Spring 框架应用架构
___
Spring 官网资源:spring.io/projects
Spring 是一个”资源整合”框架,通过spring可将很多资源(例如连接池,
mybatis,...)等整合在一起,对外提供相关服务(例如,秒杀服务,支付服务,...)。

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-d92bfc62abae4b1abbc7a002da599370.png)
---

说明:spring 框架中一切资源的整合都源于IOC模块，IOC要实现对象生命周期的管理，对象依赖关系的管理。

###  1.1.2. Spring 框架产品架构
___
产品架构主要从这个框架对外提供的服务（功能）进行理解.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-9dbc108b167f467ca8b895cd29288f64.png)
---

###  1.1.3.Spring 框架技术架构
IOC API基础架构(Spring 工厂对象分析)

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-96662d4783ed4306993deb401bac9c8b.png)
IOC(控制反转) 容器初始化过程分析.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-92dc43a7d1364ae694bd834ebf80e959.png)

##  1.2.SPRING框架快速实践 (注解方式-脱离文档)
###  1.2.1.Spring项目创建及配置
1.创建maven项目(jar包项目)
1)项目名称 CGB-SPRING-01
2)组id:com.cy
3)打包方式:jar

2.添加项目依赖(spring-context)

添加spring依赖

	   <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>5.1.9.RELEASE</version>
	   </dependency>

添加junit依赖

	<dependency>
	  <groupId>junit</groupId>
	  <artifactId>junit</artifactId>
	  <version>4.12</version>
	</dependency>

###   1.2.2.Spring基本测试环境创建
添加spring配置类(SpringConfig)

	package com.cy.spring.config;
	import org.springframework.context.annotation.ComponentScan;
	/**
	 * @ComponentScan 用于告诉spring容器从
	   * 从指定包进行bean的扫描
	 */
	@ComponentScan("com.cy.spring.beans")
	public class SpringConfig {//spring-configs.xml

	}

定义测试基类 

	package com.spring;
	import org.junit.After;
	import org.junit.Before;
	import org.junit.Test;
	import org.springframework.context.annotation.AnnotationConfigApplicationContext;
	public class TestBase {
		 protected AnnotationConfigApplicationContext ctx;
		 @Before
		 public void init() {
		  ctx=new AnnotationConfigApplicationContext(
					 SpringConfig.class);
		 }
		 @After
		 public void close() {
			 ctx.close();
		 }
		 @Test
		 public void testCtx() {
			 System.out.println(ctx);
		 }
	}

###   1.2.3.Spring项目基本业务实现
业务描述,创建一个DefaultCache对象然后将此对象交给Spring容器管理.
创建DefaultCache类,并明确此类交给spring管理.

	package com.cy.spring.beans;
	import javax.annotation.PostConstruct;
	import javax.annotation.PreDestroy;
	import org.springframework.context.annotation.Lazy;
	import org.springframework.context.annotation.Scope;
	import org.springframework.stereotype.Component;
	/**
	@Component 注解用于告诉spring容器
	请将这个类交给spring管理.
	@Lazy 用于告诉spring容器此对象要延迟加载
	@Scope 用于告诉spring容器此bean的作用域
	1)singleton (单例作用域-默认,会存储到池中)
	2)prototype (多例作用域,每次获取都创建新对象)
	 */
	@Lazy
	@Scope("singleton") 
	@Component //@Controller,@Service,...
	public class DefaultCache {
		 public DefaultCache() {
			 System.out.println("DefaultCache()");
		 }
		 @PostConstruct //告诉spring 此对象初始化时执行init方法
		 public void init() {
			 System.out.println("init()");
		 }
		 @PreDestroy//告诉spring 此对象销毁时执行close方法
		 public void close() {
			 System.out.println("close()");
		 }
	}

定义测试类,从spring容器中获取bean对象

	package com.cy.test;
	import org.junit.Assert;
	import org.junit.Test;
	import com.cy.spring.beans.DefaultCache;
	public class TestCache extends TestBase {
		@Test
		public void testDefaultCahce() {
			DefaultCache cache01=
			ctx.getBean("defaultCache",DefaultCache.class);
			Assert.assertNotEquals(null, cache01);
			DefaultCache cache02=
			ctx.getBean("defaultCache",DefaultCache.class);
			Assert.assertNotEquals(null, cache02);
			System.out.println(cache01==cache02);
		}
	}

###  1.2.4.Spring项目课堂练习分析及实现

1.整合第三方连接池DRUID.
1)步骤分析:
step01:添加依赖(数据库驱动依赖,连接池依赖)
step02:对druid连接池进行配置
step03:对连接池进行单元测试.
2)关键代码实现

添加mysql依赖

	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.17</version>
	 </dependency>

添加Druid依赖

	  <dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>druid</artifactId>
		<version>1.1.19</version>
	  </dependency>

定义数据源配置类

	package com.cy.spring.beans;
	import javax.sql.DataSource;
	@Configuration
	public class DataSourceConfig {
		  @Bean(value="druid",initMethod="init",destroyMethod="close")
		  public DataSource newDruid() {
			 // System.out.println("newDruid()");
			  DruidDataSource ds=new DruidDataSource();
			  //ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
			  ds.setUrl("jdbc:mysql:///test?serverTimezone=GMT");
			  ds.setUsername("root");
			  ds.setPassword("root");
			  ds.setMaxWait(10000);
			  //....
			  return ds;
		  }	  
	}

创建单元测试类

	package com.cy.test;
	import javax.sql.DataSource;
	import org.junit.Assert;
	import org.junit.Test;
	import com.zaxxer.hikari.HikariDataSource;
	public class TestDataSource extends TestBase{
		@Test
		public void testDruidDataSource()throws Exception {
			DataSource ds=
			ctx.getBean("druid",DataSource.class);
			Assert.assertNotEquals(null, ds);
			System.out.println(ds.getConnection());
		}
	}

#  2.整合第三方连接池HiKariCP连接池(扩展).


##  1.3.SPRING IOC 应用原理进阶分析
###  1.3.1.Spring IOC设计思想分析
IOC 是一种设计思想，称之为控制反转。基于这种思想实现对象创建，对象的科学管理以及应用时的解耦(借助DI机制实现)。Spring框架核心就是基于这种机制进行了完美实现。
说明：
1)控制反转探讨的是什么？谁控制谁的问题（spring控制对象的创建管理）
2)生活中的IOC的实现？(例如股票操盘手,父母包办婚姻）
###  1.3.2.Spring Bean 工厂的初始化

基于xml方式实现Spring中Bean工厂的初始化.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-630cb3fbad434155b03166a0ec10b184.png)
说明:Spring 中的Bean工厂会基于Bean对象描述,创建bean的实例,并可以有选择性的对实例对象进行管理(例如单例作用域的对象).
###  1.3.3.Spring 中的两大map对象分析
1.如何理解Spring中的两大map对象(存储对象的两个容器)？
1）一个map用于存储bean的配置信息(工厂的原材料)
2）一个map用于存储bean的实例信息(工厂中的成品对象)

基于xml配置文件实现:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-c49b3bb717964a3189411cc5c0387a3f.png)
---

基于注解配置实现:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-1e18b43a520043d681eea9e4a79a5f7e.png)
---

###  1.3.4.Spring 中两大bean对象分析(了解)
Bean对象创建
1)未实现FactoryBean接口(直接构造方法);
2)实现FactoryBean接口（调用FactoryBean对象的getObject方法）

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-5bdbf2e837434d1eabb4a2ddc7c838e2.png)
---

说明：一般在创建一些相对复杂的工厂对象时，通常会写一个工厂bean对象，
然后基于工厂bean对象创建具体的工厂对象，例如SqlSessionFactoryBean,
ShiroFilterFactoryBean，ProxyFactoryBean等。

###  1.3.5.Spring 中两大bean对象描述方式
Bean 对象的描述
1)xml方式 (例如<bean id=”factory” class=”com.beans.Factory”>)
2)annotation方式（@Service,@Controller，@Configuration，@Bean，..）

Spring 中用于描述这是一个Bean对象的相关注解如下:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-ec7bdf52e7324e209438f176a87a0527.png)
说明:无论使用如上图中的哪个注解对Bean进行描述,对Spring而言都认为是一样的Bean.

## 1.1.Spring 依赖注入实践增强

###  1.1.1.Spring 中Bean对象依赖注入分析(重点)
依赖注入(DI)是一种设计思想,简单点就是对象通过set或构造方法直接为对象属性赋值,在Spring框架中为这种注入基于反射技术提供一种自动实现方式.例如:

IOC 依赖注入在项目中的应用实现：

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-8effc442fbba466fbdf7b27016a7bab9.png)
---

实际项目中为了解耦和,对象之间通常会通过接口进行通讯,也就是
说对象要耦合与接口,例如2

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-aa073ba90cc446a3ab1018da3c585284.png)

个人认为：IOC的核心是对象生命周期管理(资源管理)以及
依赖注入（资源协同）；


###  1.1.2.Spring 中Bean对象依赖注入实践
在1.2小节基础上,基于如下设计,进行代码实践分析及实现。

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-31a98667ce2d4cd89c0896f59d297459.png)

核心步骤分析：
Step01:创建Cache接口以及对应的实现DefaultCache
Step02:创建SearchService接口以及对应的实现类。
Step03:将DefaultCache对象注入给DefaultSearchService。
Step03:创建SynchronizedCache测试@Autowired注解。

关键代码分析及实现：

Cache接口定义

	public interface Cache {

	}
DefaultCache类定义

	@Lazy
	@Component
	public class DefaultCache implements Cache{
		public DefaultCache() {
			System.out.println("DefaultCache()");
		}
	}
SynchronizedCache定义

	@Component
	public class SynchronizedCache implements Cache {

	}

SearchService接口定义

	public interface SearchService {

	}

SearchService实现类定义


	@Service
	public class DefaultSearchService implements SearchService {
		/**@Autowired 可以修饰属性，set方法，构造方法等
		 * 默认按照属性类型，方法参数类型为对象属性注入值,
		 * 假如相同类型的对象有多个，还会按属性名或方法参
		 * 数名等进行查找。
		 * @Qualifier 配合@Autowired，用于指定要注入的对
		 * 象的名字。
		 */
		@Autowired
		@Qualifier("defaultCache")
		private Cache cache;
		@Override
		public  String toString() {
			return "DefaultSearchService [cache=" + cache + "]";
		}
	}

测试实现

	public class TestSearchService extends TestBase {
		@Test
		public void testSearchService() {
			DefaultSearchService ds=
			ctx.getBean("defaultSearchService",
					DefaultSearchService.class);
			System.out.println(ds);
		}
	}

#  少年易老学难成，一寸光阴不可轻。