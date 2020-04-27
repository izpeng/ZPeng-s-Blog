#  2.SPRING 框架MVC模块进阶
## 2.1.Spring MVC 设计思想
###  2.1.1.Spring MVC 设计思想分析
MVC是一种分层架构设计思想，目的是基于对象职责上的不同，进行分层设计，实现各司其职，各尽所能，以提高代码的可维护性，可扩展性。
1)生活中的MVC:正规饭店(菜单,服务员,厨师)
2)程序中的MVC:(html,jsp)/servlet/(service,dao))

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-7a786784d3ca41f4946261f296b671bd.png)
---

SPRING MVC是Spring 框架中的一个WEB模块, 是基于MVC设思想的一种完美实现。

### 2.1.2.Spring MVC 核心对象及流程分析
Spring MVC 处理流程及其核心组件对象构成，如下图所示：

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-349c5b35210b4a4b9bb0991a02d335bc.png)
---

FAQ：说说spring MVC中的核心组件？
1)DipatcherServlet:前端控制器(web服务器启动时加载)
2)HandlerMapping:注册中心(负责存储url到后端控制器的映射)
3)HandlerInterceptor:拦截器(请求到handler之间的拦截)
4)Handler:后端处理器(又称之为Controller)
5)ViewResolver:视图解析器(负责是视图页面进行解析)

## 2.2.Spring MVC 快速实践

### 2.2.1.xml方式配置实现(脱离文档)
1.创建maven项目

1)项目名称: CGB-SPRINGMVC-01
2)组id: com.cy
3)打包方式: war包方式

2.配置并初始化项目环境

1)生成web.xml(配置spring mvc 前端控制器)
2)设置项目的运行时环境(选择tomcat,提供servlet支持)
3)设置项目编码方式 utf-8
4)设置统一编译环境 JDK8
5)添加项目依赖:spring-webmvc
6)添加spring mvc配置文件并进行配置:spring-configs.xml
7)web.xml中配置spring mvc前端控制器(DispatcherServlet)
8)部署项目,启动tomcat测试 (假如tomcat正常启动,则没问题)

说明:如上配置可参考后面内容中的关键代码分享.


3.Spring MVC基础业务实现 

1)定义Controller类
a)包名:com.cy.pj.search.controller
b)类名:SearchController

代码如下:

	@Controller
	@RequestMapping("/search/")
	public class SearchController {
	}

其中:@RequestMapping修饰类时用于定义请求路径

2)添加Controller方法
方法1:此方法用于向客户端返回一个页面

	@RequestMapping("doSearchUI")
	public String doSearchUI() {
	return "search";
	}

其中:对于返回值search需要在/WEB-INF/pages/目录下有对应的
search.html页面.

方法2:此方法用于向客户端返回一个json格式的字符串
```java
@RequestMapping("doSearch")
	@ResponseBody
	public Object doSearch(String key) {
		Map<String,Object> map=new HashMap<String,Object>();
		map.put("state", 1);
		map.put("message","hello everyone");
		return map;//{"state":1,"message":"hello everyone"}
	}
```

其中:
a)@ResponseBody修饰方法时,Spring MVC可以将方法的返回值以指定格式进行进行输出,例如JSON格式字符串.
b)doSearch方法进行测试时,需要在项目中添加如下依赖:

	<dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.9.3</version>
	</dependency>


3)部署并启动服务测试.


4.关键代码分享:

spring-configs.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:mvc="http://www.springframework.org/schema/mvc"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="
			http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/context
			http://www.springframework.org/schema/context/spring-context.xsd
			http://www.springframework.org/schema/mvc
			http://www.springframework.org/schema/mvc/spring-mvc.xsd">

		<context:component-scan base-package="com.cy.controller"/>
		<!-- Enable MVC Configuration:启用默认bean对象(注解驱动)  -->
		<mvc:annotation-driven/>
		<!-- 启用 DefaultServletHttpRequestHandler对静态进行处理 -->
		<mvc:default-servlet-handler/>
		<!-- 配置视图解析器(ViewResolver)对象 -->
		<bean id="viewResolver"  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		   <!-- Set DI (默认找对象中的set方法)-->
		   <property name="Prefix" value="/WEB-INF/pages/"/>
		   <property name="Suffix" value=".html"/>
		</bean>
	</beans>

Web.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">
	  <display-name>CGB-SPRINGMVC-01</display-name>
	  <!-- 配置前端控制器 -->
	  <servlet>
		<servlet-name>frontController</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring-configs.xml</param-value>
		</init-param>
		<!-- tomcat 启动时则初始化servlet,数字越小优先级越高 -->
		<load-on-startup>1</load-on-startup>
	  </servlet>
	  <servlet-mapping>
		 <servlet-name>frontController</servlet-name>
		 <url-pattern>/</url-pattern>
	  </servlet-mapping>
	</web-app>

SearchController

	package com.cy.pj.search.controller;
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.ResponseBody;
	@Controller 
	@RequestMapping("/search/") 
	public class SearchController {
		@RequestMapping("doSearchUI")
		public String doSearchUI() {
			return "search";
		}///WEB-INF/pages/search.html

		@RequestMapping("doSearch")
		@ResponseBody
		public Object doSearch(String key) {
			Map<String,Object> map=new HashMap<String,Object>();
			map.put("state", 1);
			map.put("message","hello everyone");
			return map;//{"state":1,"message":"hello everyone"}
		}
	}

###  2.2.2.注解方式配置实现（脱离文档）
Tomcat 启动加载方式:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-20db542cf104453c8a8451787821bda0.png)
---

1.创建maven项目

1)项目名称: CGB-SPRINGMVC-02
2)组id: com.cy
3)打包方式: war包方式

2.配置并初始化项目环境(重点对xml方式进行重构)

1)配置maven war包插件(忽略web.xml)
2)设置项目的运行时环境(选择tomcat)
3)设置项目编码方式 utf-8
4)设置统一编译环境 JDK8
5)添加项目依赖:spring-webmvc
6)添加spring mvc配置类:SpringWebConfig类
7)创建WebInitializer类配置spring mvc(例如前端控制器)。
8)部署项目,启动tomcat测试 (假如tomcat正常启动,则没问题)

3.Spring MVC基础业务实现 (参考xml方式业务代码实现)


4.Spring mvc 注解方式应用分析:

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-de958697c6d948dda4e45ae287c18496.png)
---

其中META-INF目录可在sping-web.jar中进行查看.

关键代码分析:

SpringWebConfig类

	@Configuration 
	@ComponentScan("com.cy.pj.search.controller")
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

WebInitializer类

	public class WebInitializer extends 
	AbstractAnnotationConfigDispatcherServletInitializer {

		//Service,Repository
		@Override
		protected Class<?>[] getRootConfigClasses() {
			System.out.println("getRootConfigClasses()");
			return null;
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


##  2.3.Spring MVC 请求响应处理增强分析
所有MVC框架的重点都在请求和响应数据的处理上。

###  2.3.1.请求处理增强分析及实现(重点)
Spring MVC请求处理主要从如下几个方面进行考虑：
1)请求路径(普通方式，rest方式) ：404
2)请求方式(Get请求，Post请求,…):405
3)请求参数(直接量，POJO对象，MAP对象):400

代码分析如下：

	@Controller
	@RequestMapping("/request/")
	public class RequestHandleController {
		 //===请求url定义 
		  //普通URL定义实现(多个url可以对应同一个资源)
		  @RequestMapping(value={"doHandleUrl","doWelcomeUI"})
		  public String doHandleUrl() {
			 return "welcome"; 
		  }
		  //REST风格的URL，其url格式为{a}/{b}/{c}
		  //假如希望方法参数获取url中{}表达式内部的值
		  //可以使用@PathVariable对参数进行修饰
		  @RequestMapping("{module}/{page}")
		  public String doMoudleUrl(
				  @PathVariable String module,
				  @PathVariable String page) {
			 return module+"/"+page;
		  }
		  //===请求方式定义
		  //@RequestMapping(value="type",method=RequestMethod.GET)
		  @GetMapping("type")
		  //@PostMapping("type")
		  @ResponseBody
		  public String doRequestType() {
			  return "request type";
		  }
		  //=====请求参数处理======
		  @GetMapping("param")
		  @ResponseBody
		  public String doRequestParam(
				 RequestWrapper rw,//pojo
				 @RequestParam(required=false) String msg,
				 @DateTimeFormat(pattern="yyyy/MM/dd")Date begin) {
			  return "request parameter handle msg="+msg+",begin="+begin+",rw="+rw.toString();
		  }
	}


###  2.3.2.响应处理增强分析实现（重点）
Spring MVC响应处理主要从如下几个方面进行考虑：
1）响应方式(转发forward，重定向redirect)
2）响应数据封装(ModelAndView,Model,Map,Pojo)
3）响应数据转换（将对象序列化为JSON格式字符串）

代码分析如下：


	@Controller
	@RequestMapping("/resp/")
	public class ResponseHandleController {
		 @RequestMapping("doResponseUI")
		 public String doResponseUI() {
			 return "response";
		 }
		 @RequestMapping("type")
		 public String doResponseType() {
			 //转发(客户端请求一次)
			 return "forward:doResponseUI";//address
			 //重定向(客户端两次请求)
			 //return "redirect:doResponseUI";//address
		 }

		 @RequestMapping("doDataConvert")
		 @ResponseBody
		 public String doDataConvert()throws Exception {
			 Map<String,Object> map=new HashMap<String,Object>();
			 map.put("id", 100);
			 map.put("msg", "hello jackson");
			 //借助jackson中的API将对象转换json格式的字符串
			 ObjectMapper om=new ObjectMapper();
			 return om.writeValueAsString(map);
		 }
	}
# 少年易老学难成，一寸光阴不可轻。