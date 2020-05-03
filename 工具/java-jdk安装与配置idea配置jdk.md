## 介绍
1.介绍
简单来讲，JDK是Java语言的软件开发工具包，我们使用JDK所提供的开发工具完成对java程序的开发；JRE是运行Java程序锁必须的环境，使用JRE运行开发好的java应用程序。

　　· JDK(Java Development Kit　　Java开发工具包)：JDK是提供给Java开发人员使用的，其中包含了java的开发工具，也包括JRE。所以安装了JDK,就不用再单独安装JRE了。

　　· JRE(Java Runtime Environment　　Java运行环境)：包括Java虚拟机（JVM：Java Virtual Machine）和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安全JRE即可。
## 下载安装
1.下载
地址：[https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)
![Snipaste_2020-05-03_21-23-03](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-23-03-da6d277afc054e23bd03c9abea8b7787.png)
2.安装
傻瓜式安装

## 环境变量配置  
4.环境变量配置
为了方便java程序的开发，我们需要配置一下环境变量。右击此电脑>属性>高级系统设置>环境变量>单击[新建(W)]添加以下环境变量
![Snipaste_2020-05-03_21-32-17](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-32-17-17f4088ae5bc4ee886779780704d189b.png)
（1）配置JAVA_HOME

　　新建JAVA_HOME

　　变量名        JAVA_HOME

　　变量值        D:\java\jdk  你的jdk的安装地址
![Snipaste_2020-05-03_21-35-04](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-35-04-b7c8c86db6a4414da21d04e2d3783aee.png)

（2）配置CLASSPATH

　　新建CLASSPATH

　　变量名   CLASSPATH

　　变量值   .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
![Snipaste_2020-05-03_21-36-07](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-36-07-f17b9d4cca084d3789b2f22cc2523c4e.png)
  
（3）配置PATH （追加）

　　在PATH变量最后面添加两条变量值                            

　　变量名  Path

　　变量值  %JAVA_HOME%\bin

　　变量值  %JAVA_HOME%\jre\bin
![Snipaste_2020-05-03_21-38-42](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-38-42-1cf01d3024f34f0195ab9751aaaf2456.png)

5.检查是否安装成功
windows+r调出cmd窗口输入java -version
![Snipaste_2020-05-03_21-41-02](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-41-02-be1a133d83bb488d8c2305934b487d23.png)

## idea配置
File-Project Structure-SDKs
D:\java\jdk
![Snipaste_2020-05-03_21-58-10](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-58-10-126adb5c93e24a9ba5e40fec57ad4870.png)