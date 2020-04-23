# Dubbo
## 1.1 SOA思想
### 1.1.1 SOA介绍
**SOA** （面向服务的架构）<br/>
  面向服务的架构（SOA）是一个组件模型，它将应用程序的不同功能单元（称为服务）进行拆分，并通过这些服务之间定义良好的接口和协议联系起来。接口是采用中立的方式进行定义的，它应该独立于实现服务的硬件平台、操作系统和编程语言。这使得构件在各种各样的系统中的服务可以以一种统一和通用的方式进行交互。

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-ae856a7313ea47bd8f74c64d9fa6222a.png)

## 1.2 RPC协议
### 1.2.1 RPC介绍
  RPC是远程过程调用（Remote Procedure Call）的缩写形式。SAP系统RPC调用的原理其实很简单，有一些类似于三层构架的C/S系统，第三方的客户程序通过接口调用SAP内部的标准或自定义函数，获得函数返回的数据进行处理后显示或打印。<br/>
  RPC:实现系统之间的通信,用户不需要了解底层原理.
### 1.1.2 RPC与HTTP区别
网络7层协议如图所示.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-f5538a7b128f47afba3045baa6e87bf5.png)

层级关系与对应的协议.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-c655388dc2494930aa4790fbba69eee7.png)

区别:<br/>
  1.RPC是传输层协议(4层).而HTTP协议是应用层协议(7层).<br/>
  2.RPC协议可以直接调用中立接口,HTTP协议不可以.<br/>
  3.RPC通信协议是长链接,HTTP协议一般采用短连接需要3次握手(可以配置长链接添加请求头Keep-Alive: timeout=20).(长连接，指在一个连接上可以连续发送多个数据包，在连接保持期间，如果没有数据包发送，需要双方发链路检测包。)<br/>
  4.RPC协议传递数据是加密压缩传输.HTTP协议需要传递大量的请求头信息.<br/>
  5.RPC协议一般都有注册中心.有丰富的监控机制.<br/>

## 1.3 Dubbo
### 1.3.1 Dubbo介绍
   Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。
   Dubbo(读音[ˈdʌbəʊ])是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和 [1]  Spring框架无缝集成。
[http://dubbo.apache.org/en-us/](http://dubbo.apache.org/en-us/ "http://dubbo.apache.org/en-us/")
### 1.3.2 Dubbo调用原理

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-6c7a6b6179be4349b18d174583961183.png)

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-88b29022ac3148cf8390805f527708e0.png)

## 1.4 Dubbo负载均衡算法
### 1.4.1 Nginx负载均衡机制
说明:nginx是服务端负载均衡,所有的请求都会由nginx实现负载.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-925dac6ef8b3409db365d2480b0cb6a6.png)

### 1.4.2 Dubbo的负载均衡模式
dubbo采用客户端负载均衡机制,并且是正向代理.

---
![image.png](http://www.zhangpeng.fun/upload/2019/12/image-0573b98f88c2453f8ece46a0e497d2f0.png)

1.6.3	随机机制
说明:dubbo默认采用的负载均衡算法为随机.
```java
	@Reference(timeout=3000,check=false,
			loadbalance = "random")
	private UserService userService;
```

1.6.4	轮循算法
```java
@Reference(timeout=3000,check=false,
			loadbalance = "roundrobin")
	private UserService userService;
```
1.6.5	IPHASH算法
说明:根据IP绑定服务器.
```java
	@Reference(timeout=3000,check=false,
			loadbalance = "consistenthash")
	private UserService userService;
```
1.6.6	最小访问量机制
说明:根据当前服务器的最小访问量实现负载均衡.(压力小先访问)
```java
	@Reference(timeout=3000,check=false,
			loadbalance = "leastactive")
	private UserService userService;
```