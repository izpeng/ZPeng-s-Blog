之前一直使用eclipse，决定不用了，以后用idea。。
>IDEA 全称 IntelliJ IDEA，是java编程语言开发的集成环境。IntelliJ在业界被公认为最好的java开发工具，尤其在智能代码助手、代码自动提示、重构、JavaEE支持、各类版本工具(git、svn等)、JUnit、CVS整合、代码分析、 创新的GUI设计等方面的功能可以说是超常的。IDEA是JetBrains公司的产品，这家公司总部位于捷克共和国的首都布拉格，开发人员以严谨著称的东欧程序员为主。它的旗舰版本还支持HTML，CSS，PHP，MySQL，Python等。免费版只支持Java等少数语言。
## 安装
1.下载
官网：[https://www.jetbrains.com/](https://www.jetbrains.com/)
下载：[https://www.jetbrains.com/idea/download/#section=windows](https://www.jetbrains.com/idea/download/#section=windows)
![Snipaste_2020-05-03_21-11-53](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-11-53-c17a04424522428ba82a33f534456267.jpg)
开源版和旗舰版的区别
开源版（Community）完全免费，旗舰版（Ultimate）需要收费，但是注册账号后可以试用30天
两者的安装包是不同的，同一台机器可以同时安装开源版和旗舰版
旗舰版比开源版多了很多功能，例如支持HTML/CSS/JavaScript/TypeScript的代码智能提醒、提供常用框架（Spring，Play，Grails）的实用插件、提供数据库工具、自动检查代码重复等。
旗舰版的收费目前是订阅模式收费策略，支持按年订阅或者按月订阅。

2.安装
无特殊，选择安装位置安装就好
我的安装路径：D:\software2\idea\IntelliJ IDEA 2019.3.3
![Snipaste_2020-05-03_21-06-24](https://www.zhangpeng.fun/upload/2020/05/Snipaste_2020-05-03_21-06-24-cc857dd4e3504ba3b3a2b02ca1b12fa2.jpg)


##破解
1.先适用30天进入idea，新建个项目
2.网上下载破解补丁jetbrains-agent.jar
3.放在idea安装目录的bin目录下
D:\software2\idea\IntelliJ IDEA 2019.3.3\bin
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-ee5d202b11444d1aa1275172e2fb6078.png)
4.idea-help-Edit Custom VM Options
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-ce0570929d094c028d9cf3f6a762d69b.png)

5.末尾加上一行（填写自己的路径）：  
-javaagent:D:\software2\idea\IntelliJ IDEA 2019.3.3\bin\jetbrains-agent.jar
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-cf055a5139dd4717ad62be3dd816da20.png)

6.关掉ieda重启

7.填入激活码，激活
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-082ae1ce53dd463c98a8a0dfe7c4bce1.png)

8.本文不包含任何资源，请自行百度。