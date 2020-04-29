用github部署静态网页其实很简单还不用动服务器，记录一次静态网页的部署。
## 介绍
在网站设计中，纯粹HTML（标准通用标记语言下的一个应用）格式的网页通常被称为“静态网页”，静态网页是标准的HTML文件，它的文件扩展名是.htm、.html，可以包含文本、图像、声音、FLASH动画、客户端脚本和ActiveX控件及JAVA小程序等。静态网页是网站建设的基础，早期的网站一般都是由静态网页制作的。静态网页是相对于动态网页而言，是指没有后台数据库、不含程序和不可交互的网页。静态网页相对更新起来比较麻烦，适用于一般更新较少的展示型网站。容易误解的是静态页面都是htm这类页面，实际上静态也不是完全静态，他也可以出现各种动态的效果，如GIF格式的动画、FLASH、滚动字幕等。

## 步骤
>前提：
了解html、css、js等前端知识。(可以自己编写或修改页面)
会使用github（代码仓库）
了解git基本语法（上传代码到github）

1. 从网上下载了一份简历模板（当然也可以自己编写）。
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-87a51cc72224409698800143465ac714.png)
2. github新建仓库
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-0b38993db9f343a799bf38973188206c.png)
3. 用git上传
git init
git add .
git commit -m 'jianli'
git push 地址 master
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-44721c1cfb274534b2a0e94a9ba7d5b4.png)
4. 进入仓库，找到settings-GitHub Pages-none
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-6e81bf0c5a1f486fa22d689fed5225bf.png)
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-ea1492ed6bf84126a4666449bcd9bbaa.png)

5. 点击后找到网址
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-44e2afd932b0435d8bcb998b11fbff20.png)

6. 访问
记得加上自己的文件路径
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-dbef78dac157480d9836e5d0d3d78b6f.png)

7. 成功哈哈。
## 域名绑定
>前提：
有阿里云账号（我用的阿里云）
拥有一个自己的域名且已备案

1.打开cmd窗口ping自己的github仓库网址如 例如：ping zhangpengpeng69.github.io
得到 ip地址
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-953c4f242ef941a4af0174fe388d6910.png)

2.阿里云域名解析
将自己的域名指向刚刚的ip
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-0d54e92a0f2a4f4a93141b2c3e8df433.png)

3.仓库新建文件CNAME
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-bc2369b7a69d4dc68e8d8f87165635dd.png)

4.写入域名保存
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-2840890a9d5744728cb053f9f07e76e3.png)

或者：在此处添加域名，会自动生成一个CNAME文件
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-a22c0299269f4531b9aacd8704228d65.png)

5.访问即可 
http://www.zhangpeng.plus/jianli1/index.html
我又修改了下我的仓库目录结构，所以多了个jianli1
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-80faf2a1580848369856bd0593d4adae.png)
成功！