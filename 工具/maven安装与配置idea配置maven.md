## maven介绍
Apache Maven是一个软件项目管理和理解工具。基于项目对象模型（POM）的概念，Maven可以从中央信息管理项目的构建，报告和文档。
Maven是一个项目管理和整合的工具。Maven为开发者提供了一套完整的构建生命周期框架。开发团队基本不用花多少时间就能自动完成工程的基础构建配置，因为Maven使用了一个标准的目录结构和一个默认的构建生命周期。在创建报告、检查、构建和测试自动配置时，Maven可以让开发者的工作变得更简单。
官网：[https://maven.apache.org/](https://maven.apache.org/)

## 下载与安装
1.下载：[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
![Snipaste_2020-05-03_22-08-30](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-08-30-e054caf2efd14a5bbd157f7678a25a87.png)
2.安装
next
我的路径：D:\java\maven\apache-maven-3.6.2
##  环境变量配置
1.右击此电脑>属性>高级系统设置>环境变量>单击[新建(W)]添加以下环境变量
![Snipaste_2020-05-03_21-32-17](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-32-17-17f4088ae5bc4ee886779780704d189b.png)
2.新建环境变量
M2_HOME
D:\java\maven\apache-maven-3.6.2
![Snipaste_2020-05-03_22-13-28](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-13-28-1e1f407b86134790b3734495ec5d7777.png)
3.更新 PATH 变量，添加 Maven bin 文件夹到 PATH 的最后
Path
D:\java\maven\apache-maven-3.6.2\bin
![Snipaste_2020-05-03_22-14-36](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-14-36-7b23a71b9d4c4893a54520da2392990c.png)

## 验证
windows+r调出cmd窗口输入  mvn -v
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-8dfa789d6490436fb27485bb5c582eac.png)
成功

## settings文件修改
（1）本地仓库（可选）
Default: ${user.home}/.m2/repository
<localRepository>D:\java\repository\rep1</localRepository>
![Snipaste_2020-05-03_22-26-19](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-26-19-d83c003d3b7f4991ae296213c83ec4dd.png)
（2）配置阿里云镜像仓库
```language
<mirror>
            <id>alimaven-central</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
        
  </mirrors>
```
![Snipaste_2020-05-03_22-27-44](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-27-44-77c6ea7ac3a04d64a611e7dd1c373cf6.png)
（3）配置jdk版本
```language
<profile>
            <id>jdk18</id>
            <activation>
                <jdk>1.8</jdk>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <maven.compiler.source>1.8</maven.compiler.source>
                <maven.compiler.target>1.8</maven.compiler.target>
                <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
            </properties>
        </profile>
```
![Snipaste_2020-05-03_22-27-53](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-27-53-ad6d6ea2d6c047d29dfeee9837523a27.png)
## idea配置maven
File-settings-Build，Excution，Deployment-BuildTools-Maven
![Snipaste_2020-05-03_22-22-29](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_22-22-29-0f8e492bb6fb41aa80faeb535f21c6f2.png)