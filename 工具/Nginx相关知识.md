
## 1.nginx 介绍
Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。
其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。2011年6月1日，nginx 1.0.4发布。
Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。
## 2.安装nginx
安装nginx

```
sudo yum install epel-release
sudo yum install nginx
```

让nginx随系统启动而启动

```
sudo systemctl enable nginx
```
/etc/nginx
## 3.nginx 负载均衡
Nginx通过反向代理可以实现服务的负载均衡，避免了服务器单节点故障，把请求按照一定的策略转发到不同的服务器上，达到负载的效果。
1、轮询
将请求按顺序轮流地分配到后端服务器上，它均衡地对待后端的每一台服务器，而不关心服务器实际的连接数和当前的系统负载。

```
http {

    upstream myapp1 {

        server 192.168.1.103:8080;

        server 192.168.1.104:8080;

    }

    ……略

   server {

        listen 80;

        server_name  localhost;

 

        ……略

        location /webautotest/ {

            proxy_buffering off;

            proxy_pass http://myapp1;

        }

    }

}
```

2、加权轮询
不同的后端服务器可能机器的配置和当前系统的负载并不相同，因此它们的抗压能力也不相同。

给配置高、负载低的机器配置更高的权重，让其处理更多的请；而配置低、负载高的机器，给其分配较低的权重，降低其系统负载，加权轮询能很好地处理这一问题，并将请求顺序且按照权重分配到后端。
```
 upstream myapp1 {

        server srv1.example.com weight=3;

        server srv2.example.com;

        server srv3.example.com;

    }
```

3、ip_hash（源地址哈希法）
根据获取客户端的IP地址，通过哈希函数计算得到一个数值，用该数值对服务器列表的大小进行取模运算，得到的结果便是客户端要访问服务器的序号。

采用源地址哈希法进行负载均衡，同一IP地址的客户端，当后端服务器列表不变时，它每次都会映射到同一台后端服务器进行访问。
```
upstream myapp1 {

    ip_hash;

    server srv1.example.com;

    server srv2.example.com;

    server srv3.example.com;

}
```

4、least_conn（最小连接数法）
由于后端服务器的配置不尽相同，对于请求的处理有快有慢，最小连接数法根据后端服务器当前的连接情况，动态地选取其中当前积压连接数最少的一台服务器来处理当前的请求，尽可能地提高后端服务的利用效率，将负责合理地分流到每一台服务器。

```
upstream myapp1 {

        least_conn;

        server srv1.example.com;

        server srv2.example.com;

        server srv3.example.com;

}

```
5、随机

## 4.nginx 反向代理
```
vim Nginx.conf
```

```
server {
	# 监听80端口，一般不用改
    listen 80;
	# 将这里的example换成自己的域名
    server_name example.com www.example.com;

    location / {
        proxy_set_header HOST $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		# 这里的http后面的url就是你访问你上方的域名代理的地址
        proxy_pass http://127.0.0.1:8090/;
    }
}
```
重启
```
nginx -s reload 
```
## 5.配置SSL证书
1.下载证书上传
在阿里云购买免费版本的SSL证书后，下载SSL证书到本地，选择Nginx版本。
下载的证书共有两个文件，分别是：

xxxxxxxx.pem
xxxxxxxx.key

在服务上/root路径下新建ssl文件夹，并将下载到本地的证书文件上传至该文件夹。

可使用rz命令或者Xftp来上传,也可以使用filezilla上传。
2.配置
在/etc/nginx/conf.d/路径下新建文件nginx.conf
```
vim /etc/nginx/conf.d/nginx.conf
```
输入
```
server
{
listen 443 ssl;
server_name yuzho.cn www.yuzho.cn;
ssl_certificate /root/ssl/xxxxxxxx.pem;
ssl_certificate_key /root/ssl/xxxxxxxx.key;
ssl_session_timeout 5m;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
location / {
proxy_buffer_size 128k;
proxy_buffers 32 32k;
proxy_busy_buffers_size 128k;
proxy_pass http://127.0.0.1:8090;
}
}
server
{
listen 80;
server_name yuzho.cn www.yuzho.cn;
rewrite ^(.*)$ https://host1 permanent;
}
```
将其中的 yuzho.cn, www.yuzho.cn换成你自己的域名；/root/ssl/xxxxxxxx.pem，/root/ssl/xxxxxxxx.key换成你自己在上一步中所上传的证书的绝对路径；将8090换成你所需要代理的端口。
3.重新加载nginx
使用nginx -t命令来检查语法错误，使用nginx -s reload命令来重新加载nginx。
如nginx -t正确，而nginx -s reload命令出现错误提示：

```
nginx: [error] invalid PID number "" in "/run/nginx.pid"
```
执行
```
nginx -c /etc/nginx/nginx.conf
sudo nginx -s reload
```
引用 [https://www.cnblogs.com/liulun/p/11518252.html](https://www.cnblogs.com/liulun/p/11518252.html)

## 6.

Nginx中文文档 ：      [https://www.nginx.cn/doc/](https://www.nginx.cn/doc/)
官网 ： [https://www.nginx.com/](https://www.nginx.com/)