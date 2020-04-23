## lombok插件 简化代码
[lombok](https://projectlombok.org/)
&emsp;&emsp;Project Lombok
&emsp;&emsp;Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.
&emsp;&emsp;Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注释，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。
&emsp;&emsp;Lombok通过增加一些“处理程序”，可以让java变得简洁、快速。
&emsp;&emsp;Lombok能以简单的注解形式来简化java代码，提高开发人员的开发效率。例如开发中经常需要写的javabean，都需要花时间去添加相应的getter/setter，也许还要去写构造器、equals等方法，而且需要维护，当属性多时会出现大量的getter/setter方法，这些显得很冗长也没有太多技术含量，一旦修改属性，就容易出现忘记修改对应方法的失误。

&emsp;&emsp;Lombok能通过注解的方式，在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法。出现的神奇就是在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法。这样就省去了手动重建这些代码的麻烦，使代码看起来更简洁些。

### 1.引入jar包
引入插件lombok 自动的set/get/构造方法插件  


```java
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```
### 2.找到lombokjar包文件位置 以java方式运行
![image.png](http://www.zhangpeng.fun/upload/2020/3/image-2bd1b12e0041491ab1c25d89b3ceab8d.png)
### 3.命令:
![image.png](http://www.zhangpeng.fun/upload/2020/3/image-25ea6cb0fe704751bf0efd0e5f425ac5.png)
之后点击install/update即可
在eclipse安装的根目录下会生成lombok.jar包文件,表示插件安装成功.
![image.png](http://www.zhangpeng.fun/upload/2020/3/image-0be5f4eabff74481a5fa694174e65640.png)
### 4.配置
在文件安装的根目录找到SpringToolSuite4.ini中
手动在ini文件中最后一行添加
-vmargs -javaagent:lombok.jar
 
安装完成后重启Eclipse即可

### 5.常用注解
 Lombok主要常用的注解有：@Data,@getter,@setter,@NoArgsConstructor,@AllArgsConstructor,@ToString,@EqualsAndHashCode,@Slf4j,@Log4j。我们一个一个来看：
 
@Data注解：在JavaBean或类JavaBean中使用，这个注解包含范围最广，它包含getter、setter、NoArgsConstructor注解，即当使用当前注解时，会自动生成包含的所有方法；
 
@getter注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的getter方法；
 
@setter注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的setter方法；
 
@NoArgsConstructor注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的无参构造方法；
 
@AllArgsConstructor注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的有参构造方法；
 
@ToString注解：在JavaBean或类JavaBean中使用，使用此注解会自动重写对应的toStirng方法；
 
@EqualsAndHashCode注解：在JavaBean或类JavaBean中使用，使用此注解会自动重写对应的equals方法和hashCode方法；
 
@Slf4j：在需要打印日志的类中使用，当项目中使用了slf4j打印日志框架时使用该注解，会简化日志的打印流程，只需调用info方法即可；
 
@Log4j：在需要打印日志的类中使用，当项目中使用了log4j打印日志框架时使用该注解，会简化日志的打印流程，只需调用info方法即可；
 
在使用以上注解需要处理参数时，处理方法如下（以@ToString注解为例，其他注解同@ToString注解）：
 
@ToString(exclude="column")
 
意义：排除column列所对应的元素，即在生成toString方法时不包含column参数；
 
@ToString(exclude={"column1","column2"})
 
意义：排除多个column列所对应的元素，其中间用英文状态下的逗号进行分割，即在生成toString方法时不包含多个column参数；
 
@ToString(of="column")
 
意义：只生成包含column列所对应的元素的参数的toString方法，即在生成toString方法时只包含column参数；；
 
@ToString(of={"column1","column2"})
意义：只生成包含多个column列所对应的元素的参数的toString方法，其中间用英文状态下的逗号进行分割，即在生成toString方法时只包含多个column参数；
@Accessors(chain=true)
链式加载

### 可能问题
![image.png](http://www.zhangpeng.fun/upload/2020/3/image-e3803a1ea7cd48f79d857b2a00fbe985.png)
sts安装lombok后重启时，如报以上错误，是没有找到jdk的安装路径
解决：
	在SpringToolSuite4.ini中加入如下代码：
![image.png](http://www.zhangpeng.fun/upload/2020/3/image-7000f78eee3a4955ba436dcf5597926a.png)	