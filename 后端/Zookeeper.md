# Zookeeper 安装
## 1.1Zookeeper
### 1.1.1Zookeeper介绍
ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。
ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。
ZooKeeper包含一个简单的原语集,提供Java和C的接口。
ZooKeeper代码版本中，提供了分布式独享锁、选举、队列的接口，代码在zookeeper-3.4.3\src\recipes。其中分布锁和队列有Java和C两个版本，选举只有Java版本。
总结:Zookeeper负责服务的协调调度.当客户端发起请求时,返回正确的服务器地址.
### 1.1.2Zookeeper下载
网址: http://zookeeper.apache.org/releases.html.
如图-2所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-8216270faedd430d85ed24fe29898f31.png)
---
图-2
下载路径,点击download.
如图-3所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-eedd0e510d7943ee829628375061624f.png)
---
图-3
下载Zookeeper地址.
http://mirrors.hust.edu.cn/apache/zookeeper/
如图-4所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-41f07c949eaf4d71b10d3d1df73fa693.png)
---
图-4
## 1.2Zookeeper安装
### 1.2.1安装JDK
将JDK1.8文件上传到Linux操作系统中/src/usr/local/java/文件下.
如图-5所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-7074267a86504a7bb7d608e53530c7b6.png)
---
图-5
1.解压文件
tar -xvf jdk-8u51-linux-x64.tar.gz
2.配置环境变量
编辑环境变量配置文件
vim /etc/profile
如图-6所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-860af512cce74a00983685715f187959.png)
---
图-6
使JDK生效,之后检查jdk安装是否成功
source /etc/profile
如图 -7所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-781c797140fe4380881e91619d8fcf5c.png)
---
图-7
### 1.2.2上传安装文件
说明:上传zookeeper安装文件.之后解压.
如图-8所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-971e330e1d7341ea872052081c42fc9b.png)
---
图-8
解压目录:
tar -xvf zookeeper-3.4.8.tar.gz
### 1.2.3修改配置文件
在zk根目录下创建文件夹data/log
mkdir data log
如图-9所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-a57e315772904f6fa0e053f88211be4f.png)
---
图-9
复制配置文件并且修改名称
cp zoo_sample.cfg zoo.cfg
如图-10所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-9185194983bf4296b3331ee19c6b55a2.png)
---
图-10
### 1.2.4启动zk
zk启动关闭命令如下.
sh zkServer.sh start     或者  ./zkServer.sh start
sh zkServer.sh stop
sh zkServer.sh status
如图-11所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-47e0942ad88443fab74a7328a65bdbb0.png)
---
图-11
## 1.3Zookeeper集群安装
### 1.3.1准备文件夹
在zookeeper根目录中创建新的文件夹zkCluster.
如图-12所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-6f229215118a4742b272dcd82284412a.png)
---
图-12
如图-13所示

创建zk1/zk2/zk3文件夹.
---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-a231d6cf8f624ea6afe754f027cc4026.png)
---
图-13
在每个文件夹里创建data/log文件夹.
mkdir {zk1,zk2,zk3}/{data,log}
如图-14所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-22fca11a61de4db5944f17c7ceca09f7.png)
---
图-14
### 1.3.2添加myid文件
分别在zk1/zk2/zk3中的data文件夹中创建新的文件myid.其中的内容依次为1/2/3,与zk节点号对应.
如图-15所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-6efe08a4dac84be38670a491f08851a1.png)
---
图-15
编辑myid文件,定义编号.
图-16所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-cba24531f3b04988ba41302de0e8e8da.png)
---
图-16
### 1.3.3编辑配置文件
将zoo_sample.cfg 复制为zoo1.cfg之后修改配置文件.
如图-17所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-f060179ff53d4e2a8c73a3304f50aa42.png)
---
图-17
### 1.3.4修改zoo1.cfg
如图-18所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-7ec28ae2215a4f45a49c784263cb5023.png)
---
图-18
配置完成后将zoo1.cfg复制2份.之后需要修改对应的文件夹目录.和不同的端口即可.
### 1.3.5ZK集群测试
通过下面的命令启动zk集群.
sh zkServer.sh start   zoo1.cfg
sh zkServer.sh stop    zoo1.cfg
sh zkServer.sh status  zoo1.cfg
检查主从关系,从机情况说明.
如图-19所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-de4854b240504612b6dce11f46ed2645.png)
---
图-19
检查主从关系,主机情况说明.
如图-20所示

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-748f94c832c343c08ac3cdeff31ea15f.png)
---
图-20
### 1.3.6关于zookeeper集群说明
Zookeeper集群中leader负责监控集群状态,follower主要负责客户端链接获取服务列表信息.同时参与投票.
