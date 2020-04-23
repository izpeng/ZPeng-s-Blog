# 2.SSM框架综合案例实现(注解方式)
## 2.1.业务描述
通过spring整合mybatis完成商品数据库中数据的查询,并在客户端以json格式对数据进行输出?
思路:
1)创建maven web项目(打包方式war):CGB-SSM-01
2)配置项目?(运行时环境,编码,编译版本)
3)添加依赖?(mysql,druid,mybatis,junit,spring-webmvc,..)
4)整合资源?(连接池,mybatis,spring mvc..)

## 2.2.初始化化数据环境
数据库脚本分析:

	drop database if exists dbgoods;
	create database dbgoods default character set utf8;
	use dbgoods;
	create table tb_goods(
	   id bigint auto_increment,
	   name varchar(200) not null,
	   remark text,
	   createdTime datetime,
	   primary key (id)
	)engine=InnoDB;

	insert into tb_goods values (null,'A','A...',now());
	insert into tb_goods values (null,'B','B...',now());
	insert into tb_goods values (null,'C','C...',now());

说明:可以将如上脚本写到db-goods.sql文件中,然后去执行完成完成初始化.
## 2.3.项目创建及配置
1.创建maven web项目:CGB-SSM-01
2.在pom.xml添加war包插件,修改其配置(这样项目可以不写web.xml)

	 <build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-war-plugin</artifactId>
				<version>2.6</version>
				<configuration>
				  <failOnMissingWebXml>false</failOnMissingWebXml>
				</configuration>
			</plugin>
		</plugins>
	  </build>

3.配置项目(设置运行时环境-tomcat,编译版本JDK1.8)
4.添加项目依赖(参考如下图中的依赖)
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-e0e83512c916471aaa3961fc4299d37b.png)


## 2.4.项目资源整合实现
### 2.4.1.配置架构分析及实现

配置架构分析:

![image.png](http://www.zhangpeng.fun/upload/2019/12/image-b79885d125f24239b23067babe1e3333.png)

其中:
1)SpringRepositoryConfig 负责数据层配置
2)SpringServiceConfig负责业务层配置
3)SpringWebConfig负责请求处理层配置(Spring MVC中的V,C)
4)WebInitializer 负责启动初始化.

基于配置架构,创建配置类:

数据层配置类

	package com.cy.pj.common.config;
	@Configuration
	@MapperScan("com.cy.pj.goods.dao")//扫描dao
	public class SpringRepositoryConfig {
	}

业务层配置类:
package com.cy.pj.common.config;
@Configuration
@ComponentScan("com.cy.pj.goods.service")
public class SpringServiceConfig {
	//.....
}


控制层配置类

	package com.cy.pj.common.config;
	@ComponentScan("com.cy.pj.goods.controller")
	@EnableWebMvc
	@Configuration 
	public class SpringWebConfig implements WebMvcConfigurer{

	}

WebInitializer 启动类配置:

	package com.cy.pj.common.config;
	//-->web.xml
	public class WebInitializer extends 
	AbstractAnnotationConfigDispatcherServletInitializer {
		//Service,Repository
		@Override
		protected Class<?>[] getRootConfigClasses() {
			System.out.println("getRootConfigClasses()");
			return new Class[] {SpringRepositoryConfig.class,
	SpringServiceConfig.class};
		}
		//View,Controller
		@Override
		protected Class<?>[] getServletConfigClasses() {
			System.out.println("getServletConfigClasses()");
			return new Class[] {SpringWebConfig.class};
		}
		@Override
		protected String[] getServletMappings() {
			System.out.println("getServletMappings()");
			return new String[] {"/"};
		}

	}

整合过程分析:(顺序从右到左)
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-2c8c25e8740f4e56b2b9d96c7e01008a.png)
### 2.4.2.整合连接池对象
在SpringRepositoryConfig 类中添加dataSource()方法,在方法中创建
DruidDataSource对象.并将此对象交给spring管理.

	@Bean(value="druid",initMethod="init",destroyMethod="close")
	public DruidDataSource dataSource() {
		DruidDataSource ds=new DruidDataSource();
		ds.setUrl("jdbc:mysql:///dbgoods?serverTimezone=GMT%2B8");
		ds.setUsername("root");
		ds.setPassword("root");
		return ds;
	}


说明:@Bean注解要配合@Configuration注解使用,@Configuration用于描述类.
### 2.4.3.整合mybatis框架
在SpringRepositoryConfig类中添加创建SqlSessionFactory对象创建
的方法:

	@Bean("sqlSessionFactory")
	public SqlSessionFactory newSqlSessionFactory(
			DataSource dataSource) 
			throws Exception {
	  //构建SqlSessionFactoryBean对象
	  SqlSessionFactoryBean factoryBean =
	  new SqlSessionFactoryBean();
	  factoryBean.setDataSource(dataSource);
	  //调用FactoryBean的getObject方法创建SqlSessionFactory
	  //底层会使用SqlSessionFactoryBuilder创建
	  return factoryBean.getObject();
	}

参考:www.mybatis.org/spring

### 2.4.4.整合Spring MVC 模块

在Spring MVC配置类中添加视图解析配置,默认servlet处理等配置.

	package com.cy.pj.common.config;

	@Configuration 
	@ComponentScan("com.cy.pj.goods.controller")
	@EnableWebMvc //<mvc:annotation-driven/>
	public class SpringWebConfig implements WebMvcConfigurer{
		//<mvc:default-servlet-handler/>
		@Override
		public void configureDefaultServletHandling(
				DefaultServletHandlerConfigurer configurer) {
			configurer.enable();
		}
		@Override
		public void configureViewResolvers(
			ViewResolverRegistry registry) {
			registry.jsp("/WEB-INF/pages/",".html");
		}
	}

## 2.5.业务设计分析及实现
### 2.5.1.业务架构
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-672453807116433ca5a8bab6a294760f.png)
### 2.5.2.POJO实现
定义pojo对象Goods用于封装数据:

	package com.cy.pj.goods.pojo;
	import java.io.Serializable;
	import java.util.Date;
	public class Goods implements Serializable{
		private static final long serialVersionUID = 690138036951052829L;
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

		@Override
		public String toString() {
			return "Goods [id=" + id + ", name=" + name + ", remark=" + remark + ", createdTime=" + createdTime + "]";
		}
	}


### 2.5.3.Dao实现
定义GoodsDao接口,代码如下:

	package com.cy.pj.goods.dao;
	import org.apache.ibatis.annotations.Select;
	public interface GoodsDao {

	@Select("select * from tb_goods")
		List<Goods> findGoods();
	}

在SpringRepositoryConfig类上添加@MapperScan注解,实现对指定包下Dao接口的扫描:

	@Configuration
	@MapperScan("com.cy.pj.goods.dao")//扫描dao
	public class SpringRepositoryConfig {
	  //....
	}

编写单元测试类TestGoodsDao,然后getRowCount方法进行测试

	public class TestGoodsDao extends TestBase{
		 @Test
		 public void testFindGoods() {
			   GoodsDao dao=ctx.getBean("goodsDao",GoodsDao.class);
			   List<Goods> list=dao.findGoods();
			   for(Goods g:list) {
				System.out.println(g);
			   }
		 }
	}

### 2.5.4.Service实现

创建GoodsService接口,实现具体业务:

	package com.cy.pj.goods.service;
	import java.util.List;
	import com.cy.pj.goods.pojo.Goods;
	public interface GoodsService {
		 List<Goods> findGoods();
	}

创建接口实现类:

	package com.cy.pj.goods.service.impl;
	import java.util.List;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Service;
	import com.cy.pj.goods.dao.GoodsDao;
	import com.cy.pj.goods.pojo.Goods;
	import com.cy.pj.goods.service.GoodsService;
	@Service
	public class GoodsServiceImpl implements GoodsService {
		@Autowired
		private GoodsDao goodsDao;
		@Override
		public List<Goods> findGoods() {
			//...
			List<Goods> list=goodsDao.findGoods();
			//...
			return list;
		}
	}

编写Service配置类(需要在容器启动时加载),对service对象进行扫描

	package com.cy.pj.common.config;
	import org.springframework.context.annotation.ComponentScan;
	import org.springframework.context.annotation.Configuration;
	@Configuration
	@ComponentScan("com.cy.pj.goods.service")
	public class SpringServiceConfig {

		//.....
	}

编写测试类执行单元测试:

	public class TestGoodsService extends TestBase{
		@Test
		public void testFindGoods() {
			GoodsService gs=
			ctx.getBean("goodsServiceImpl", GoodsService.class);
			List<Goods> list=gs.findGoods();
			for(Goods g:list) {
				System.out.println(g);
			}
		}
	}

2.5.5.Controller实现
编写controller,用于处理客户端的请求:

	package com.cy.pj.goods.controller;

	import java.util.List;

	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.ResponseBody;

	import com.cy.pj.goods.pojo.Goods;
	import com.cy.pj.goods.service.GoodsService;

	@Controller
	@RequestMapping("/goods/")
	public class GoodsController {
		@Autowired
		private GoodsService goodsService;
		@RequestMapping("doFindGoods")
		@ResponseBody
		public List<Goods> doFindGoods(){
			return goodsService.findGoods();
		}//json 串:spring mvc 启动API将对象转换为JSON串
	}


2.6.项目部署及运行分析
项目部署到tomcat,然后启动进行访问测试.

http://localhost/CGB-SSM-01/goods/doFindGoods

# 少年易老学难成，一寸光阴不可轻。