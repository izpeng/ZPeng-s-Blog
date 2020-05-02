>作为程序员拥有一个属于自己的个人技术博客，是很有好处的，经常写写文章，面试时亮出博客地址也会让面试官对你的好感度倍增。我已经有halo博客了，为什么来搞这个呢？完全是好奇而已。hexo+github部署自己的静态博客最大的好处是自己不用花钱去买服务器了。
## 介绍
官网：[https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)
官方文档：[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)
主题:[https://hexo.io/themes/](https://hexo.io/themes/)
hexo：Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
>超快速度
Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染

>支持 Markdown
Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。

>一键部署
只需一条指令即可部署到 GitHub Pages, Heroku 或其他平台。

>插件和可扩展性
强大的 API 带来无限的可能，与数种模板引擎（EJS，Pug，Nunjucks）和工具（Babel，PostCSS，Less/Sass）轻易集成

## 步骤
>前提
已安装Node.js (Node.js 版本需不低于 8.10，建议使用 Node.js 10.0 及以上版本)（node -v）（npm -v）
已安装Git（$ git --version）

**安装：**
1. 新建文件夹（例如hexoblog）
2. 右键：git bush here
3. 切换阿里的npm镜像：$ npm install -g cnpm --registry=https://registry.npm.taobao.org
4. $ cnpm install hexo-cli -g
5. $ npm install hexo  
6. $ hexo init hexo（以hexo文件夹名为例）
7. $ cd hexo
8. $ cnpm install
9. $ hexo server
10. 访问： http://localhost:4000

**部署到github：**
1. github 创建仓库 XXX.github.io
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-92580a30751440a8af455c74aaaf70ea.png)
2. 配置_config.yml文件
修改_config.yml文件，添加你创建的GitHub仓库地址（以我的为例）
```
deploy:
  type: git
  repo: https://github.com/zpengit/zpengit.github.io
  # example, https://github.com/hexojs/hexojs.github.io
  branch: master

```


3. hexo-deployer-git.
4. hexo clean && hexo deploy 。

5. 查看 zpengit.github.io 上的网页是否部署成功。(我这是更换了主题的样子)
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-df81cef6c54a48adb7dd09255e1834f4.png)

**修改主题**
主题网址：[https://hexo.io/themes/](https://hexo.io/themes/)
我以主题https://github.com/iissnan/hexo-theme-next为例
1. mkdir themes/next
2. 下载git clone https://github.com/iissnan/hexo-theme-next themes/next
3. 配置_config.yml
修改_config.yml文件中的theme属性
theme: next
4.打包上传看看效果：
hexo clean
hexo generate
hexo deploy
5.访问https://zpengit.github.io/
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-694d6fea956e4edaac4b4c783ffcc8ca.png)

**配置域名**
1. 将域名指向GitHub的服务器地址
2. 进入存放博客的GitHub仓库，点击settings，设置Custom domain，输入域名xxx.xx。
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-5fb60de8ce2547f8918bae6d0904e59f.png)
3. 然后在本地博客文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。
4. 最后重新编译上传文件，访问：xxx.xx即可。
hexo clean
hexo generate
hexo deploy
