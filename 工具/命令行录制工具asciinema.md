## 介绍
>Record and share your terminal sessions, the right way.

>**简单录制**
在终端上记录您的工作。要开始只是运行asciinema rec，完成使用Ctrl-D或输入exit即可

>**复制粘贴**
每当您看到要在自己的终端中尝试的命令时，只需暂停播放器并复制粘贴所需的内容即可。毕竟只是文本！

>**嵌入**
轻松地在博客文章，项目文档页面或会议演讲幻灯片中嵌入asciicast播放器。

官网例子：
<script id="asciicast-239367" src="https://asciinema.org/a/239367.js" async></script>


官网：[https://asciinema.org/](https://asciinema.org/)
官方教程：[https://asciinema.org/docs/how-it-works](https://asciinema.org/docs/how-it-works)
## 步骤
>前提：
1.了解服务器相关知识
2.注册网站账号

1.在linux上安装asciinema
我的服务器系统为centos7，下面以这个为例，不同系统请参考官方详细安装文档。
#安装epel发行包
yum -y install epel-release
yum -y install asciinema

#用命令测试版本
asciinema --version

#安装expect命令 用于交互
yum -y install expect
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-037157144b674b64bf12dffbcb3ea4cd.png)
2. 录制

执行录制
asciinema rec test   （test为文件名，可自定义）

这里我先胡乱输入一些东西。
ctrl+d退出录屏，回车则上传录制内容到网站，ctrl+c可以保存到本地（后续也可以通过asciinema upload来上传）

执行播放
asciinema play test

上传asciinema upload test
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-f80895e02e3942c4bcd74576ebef35a1.png)

3. 打开网址或者进入网站就会发现已经有这个东西了
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-73db2980e9b24cab91abc27e3e428269.png)

4. 使用方法
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-4e9cc7e47d24405781f2f85c3e6774cf.png)

```
Link
https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj

Append ?t=30 to start the playback at 30s, ?t=3:20 to start the playback at 3m 20s.

Embed image link
Use snippets below to display a screenshot linking to this recording.
Useful in places where scripts are not allowed (e.g. in a project's README file).

HTML:
<a href="https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj" target="_blank"><img src="https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj.svg" /></a>

Markdown:
[![asciicast](https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj.svg)](https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj)

Embed the player
If you're embedding on your own page or on a site which permits script tags, you can use the full player widget:

<script id="asciicast-E9zOTdPU5eb1CoC9F8EXQdIVj" src="https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj.js" async></script>

Paste the above script tag where you want the player to be displayed on your page.
```

5. 效果
<script id="asciicast-E9zOTdPU5eb1CoC9F8EXQdIVj" src="https://asciinema.org/a/E9zOTdPU5eb1CoC9F8EXQdIVj.js" async></script>

6.结束
