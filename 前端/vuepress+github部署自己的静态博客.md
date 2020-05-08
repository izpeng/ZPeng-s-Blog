
没错，我又弄了一个静态博客，哈哈,我也不知道为什么，当作学习了吧!
## Vue.js
还要从昨天想要学习vue开始，
>Vue.js是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合。另一方面，Vue 完全有能力驱动采用单文件组件和Vue生态系统支持的库开发的复杂单页应用。

官网：[https://cn.vuejs.org/](https://cn.vuejs.org/)
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-92a23e88c1ec4966ae0a2769ec2f03ac.png)

vue没学倒是挺喜欢官网的界面的，想着是怎么做的，就找到了vuepress。

## VuePress
官网：[https://www.vuepress.cn/](https://www.vuepress.cn/)
简洁至上
以 Markdown 为中心的项目结构，以最少的配置帮助你专注于写作。
Vue 驱动
享受 Vue + webpack 的开发体验，可以在 Markdown 中使用 Vue 组件，又可以使用 Vue 来开发自定义主题。
高性能
VuePress 会为每个页面预渲染生成静态的 HTML，同时，每个页面被加载的时候，将作为 SPA 运行。
![image.png](https://www.zhangpeng.fun/upload/2020/05/image-3694e7a68e494d70b127c4f8966eda35.png)
其实就是用来写Markdown的
[http://www.zhangpeng.plus/vuepress/](http://www.zhangpeng.plus/vuepress/)

## vuepress搭博客
但是我找了个模板就开始搞博客哈哈
>请确保你的 Node.js 版本 >= 8。
vuepress
1. 全局安装
```language
# 安装
yarn global add vuepress # 或者：npm install -g vuepress

# 创建项目目录
mkdir vuepress-starter && cd vuepress-starter

# 新建一个 markdown 文件
echo '# Hello VuePress!' > README.md

# 开始写作
vuepress dev .

# 构建静态文件
vuepress build .
```

2. 现有项目
```language
# 将 VuePress 作为一个本地依赖安装
yarn add -D vuepress # 或者：npm install -D vuepress

# 新建一个 docs 文件夹
mkdir docs

# 新建一个 markdown 文件
echo '# Hello VuePress!' > docs/README.md

# 开始写作
npx vuepress dev docs
```
3. 详细教程：[https://www.vuepress.cn/](https://www.vuepress.cn/)

## 部署github
1. 部署github，创建一个如下的 deploy.sh 文件
```language
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run docs:build

# 进入生成的文件夹
cd docs/.vuepress/dist

# 如果是发布到自定义域名
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# 如果发布到 https://<USERNAME>.github.io/<REPO>
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages

cd -
```
2. 小问题

>在 docs/.vuepress/config.js 中设置正确的 base。
如果你打算发布到 https://<USERNAME.github.io/，则可以省略这一步，因为 base 默认即是 "/"。
如果你打算发布到 https://<USERNAME.github.io/<REPO/（也就是说你的仓库在 https://github.com/<USERNAME/<REPO），则将 base 设置为 "/<REPO/"。

3. 每个账号可以有几个github pages

解决了我一个难题，之前我甚至注册了个新的邮箱去放我的hexo博客

>个人主页必须要和用户的GitHub帐号同名，所以每个用户有且只能有一个repo作为个人主页，且必须是<username>/<username>.github.io的形式；而项目主页的命名则没有这种限制，且数量有任意多个。

>不考虑绑定的自定义域名的前提下，个人主页的GitHub二级域名为<username.github.io;项目主页的GitHub二级域名为<username.github.io/<projectname,没有<projectname.<username.github.io这种方式

>个人主页的展示内容以master分支里的文件为准；而项目主页的展示内容以gh-pages分支内的文件为准

[https://help.github.com/en/github/working-with-github-pages/about-github-pages](https://help.github.com/en/github/working-with-github-pages/about-github-pages)

## 结果展示
文档：http://www.zhangpeng.plus/vuepress/
博客：[https://zhangpengpeng69.github.io/vuepressblog/](https://zhangpengpeng69.github.io/vuepressblog/)





