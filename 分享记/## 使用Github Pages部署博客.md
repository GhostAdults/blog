[toc]
## Github Pages搭建博客(Theme：Aurora)
### 前言 ###
 {{guilian}} 最近在闲逛摸鱼的时候嗯...我居然发现可以在Github上搭建自己的博客<s>(其实早就可以)</s> 一般情况下搭建个人博客需要购服务器购入域名，然后自己捣鼓...这刚开始就这么多事情而且咱还会遇到各种各样的麻烦~咱们是只想安静的写写文章~怎么就这么麻烦呢？ {{weixiao}}  所以就有了这篇文档~~~
### 说明 ###
- 搭建博客网站有各种各样的方法，做法也有很多。这个方法仅适合对前端后端都不太懂的小伙伴~对其他搭建方式而言，不需要太多的服务器基础。
- 基于Git hub 开源的API 安全性和稳定性较好
- 脱离服务器与数据库<s>(太友好了！)</s>更关注于写作内容本身，不需要过多的操作
- 使用Github Pages需要两个博客引擎驱动：[jekyllrb](https://jekyllrb.com/)或者[hexo](https://hexo.io/)
- 这个方法很简单！可以借这个文档开始学习如何自己搭建一个博客~

### Tips ###
本文主要教大家如何使用GitHub pages 搭建博客。所以这次食用的是一位大佬的博客主题<br>
Theme：[Aurora](https://github.com/chanshiyucx/aurora)<br>
Author：[蝉时雨](https://github.com/chanshiyucx)<br>
有兴趣可以去作者的仓库看看，这个项目是开源的！<br>
演示：
[![作者的博客主页](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn@master/img/chanshiyu.gif "chanshiyu.com")](https://chanshiyu.com)
> *网上针对`Github Pages`搭建博客的文章太多了<s>(一大堆)</s>，故基础性的东东不细说。这里针对一些比较容易出错的地方 进行一下讲解(博主已经提前踩过坑了讷)*

### 技术栈 ###
[`Vue`](https://vuejs.org/v2/guide/) + [`Github Issues`](https://developer.github.com/v3/issues/) + [`Github API`](https://developer.github.com/v3/) + [`Gitalk`](https://github.com/gitalk) + [`LeanCloud`](https://console.leancloud.app/)
### 前置需求说明 ###
此博客(Aurora)采用的前端技术基于 [Vue](https://vuejs.org/v2/guide/)，文章存储采用 [Github Issues](https://developer.github.com/v3/issues/)及[Github API](https://developer.github.com/v3/)，数据库采用[LeanCloud](https://console.leancloud.app/)(第三方数据库，用来存储访问热度)。在食用之前，首先需要装一下环境：<br>
<font size=1px>怎么又要装环境~!好烦！.jpg 不过大家放心这个不复杂，知道了步骤好。</font>
### 安装运行环境 ###
**Nodejs和Git的安装**<br>
这两个前者是JavaScript所需要的服务端运行环境其次就是Git大家应该经常用都懂的~<br>
▼ 先去[这里](https://nodejs.org/en/)获取 Nodejs 然后安装看下图<br>

[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Nodejs.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Nodejs.png)<br>
{{tiaokan}}看到Next的按钮了吗？对着它(如果需要改安装路径的中间可以停一下）疯狂的往按下就完事了~最后finish,然后 **Win+R** 输入 **CMD** 使用指令 `node -v` 检查一下版本，显示版本号就说明你已经安装完成了~<br>

▼ 再去[这里](https://git-scm.com/downloads)获取Git 然后安装下图<br>

[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitl.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitl.png)<br>
{{tiaokan}}和Nodejs方法基本一致，勾选同意 然后Install往按下就好了<br>

▼ 由于 Aurora 使用 vue 开发，所以需要安装 [vue-cli](https://cli.vuejs.org/zh/guide/installation.html)。

```
$ npm install -g @vue/cli-service-global
```
*PS:因为Vue的服务器访问比较慢，可以通过淘宝镜像的[cnpm](https://developer.aliyun.com/mirror/NPM?from=tnpm)装，使用以下指令：*

```
$ npm  install -g cnpm --registry=https://registry.npm.taobao.org

$ cnpm install cnpm -g        //cnpm代替npm

$ cnpm install vue-cli -g    //全局安装vue-cli
```
使用指令 `vue -V` 检查一下版本~

### Git的环境配置 ### 
▼ 安装git后，通过命令行打开终端 或者 Terminal，输入一下指令。
```
$ git config --global user.name "YOUR NAME"
$ git config --global user.email "YOUR EMAIL ADDRESS"
```
▼ 生成 [SSH](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5) 公钥和私钥，本地一份，Github 一份，这样 Github 可以在你使用仓库的时候，进行校验确定你的身份。继续在终端里面输入下面的命令。
```
ssh-keygen -t rsa -C "Github上绑定邮箱"
```
按三次`Enter` 成功后在~/.ssh/目录会生成两个文件，id_rsa和id_rsa.pub ，如下
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/sshid.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/sshid.png)<br>
▼ 将SSH key添加到Github账户中<br>
打开 `id_rsa.pub` 将里面的的内容全部复制出来 然后打开GitHub官网。
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/GithubScreen.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/GithubScreen.png)<br>
把key贴入下面这框里然后 `Add SSH key`。
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/GithubScreen2.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/GithubScreen2.png)<br>

### 测试本地Vue项目 ###
▼ 拉取项目
```
# 先把主题clone 下来
$ git clone git@github.com:chanshiyucx/aurora.git
# 移动到主题目录
$ cd  aurora
# 安装依赖
$ npm install
# 安装依赖
$ npm start
```
 依赖包安装完毕，便可执行 `npm start` 启动server拉起项目，如下<br>
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/npmcmd.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/npmcmd.png)<br>

 输入本地Local: http://localhost:8080/, 就可以打开博客页，现在看到的是作者蝉时雨的博客<br>
<font color="#BF3922">到此为止，你已经在本地部署好了，那么，下一步就开始部署到Github上吧~</font><br>

### Github Pages部署 ###
▼ 创建自己github的Pages
首先，打开GitHub 然后创建一个库。
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/gitscreen.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/gitscreen.png)<br>
然后打开Settings在 `GitHub Pages` 处 `Choose a Theme` 选一个主题，然后Github会自动把这个主题部署到你的库中。网址栏输入你的库名就可以访问啦~<br>

*现在的主题就是你刚才选择的主题。当然这主题是可以自定义的。下面就以蝉時雨的主题为主来看怎么部署到GitHub上去*<br>

### Github Issues创建 ###
食用这个主题你还需要进行一些配置<br>
首先，你需要创建三个库：`*.github.io` (你的博客域名) `blog` (存储文章) `comment`（存储评论）。创建方法就不多说了。<br>

**▼ 注册专门访问库的token**<br>
创建好库之后，你需要一个token令牌来获取你仓库的访问权限，打开Seetings

[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen3.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen3.png)
Note名是 `blog` 框内的repo必须勾选，其它的看需求。之后按 `Generate token` 就创建好了。<br>
**▼ 将注册好的token令牌配置到自己的项目中**<br>
修改config.js配置文件
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/intellij.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/intellij.png)
### Github Issues配置 ###
Issues主要作为文章，书单，关于，友链，灵感存放的位置。以一个Issues为主就是一篇文章<br>
**▼ 设置Open的Issues(文章的存放位置)**
首先打开你的 `blog` 库<br>
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer.jpg "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer.jpg)

**▼ 设置Closed的Issues(书单，关于，友链，灵感的存放位置)**<br>
打开`Issues`最下方`Closed issue`
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer2.jpg "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer2.jpg)

**▼ 设置Issues分类**
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer3.jpg "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/dsBuffer3.jpg)

**▼ 文章Issues格式规定**<br>
文章模板没有太多的格式约束，只需要在文章正文顶部加上封面配图即可，配图采用的是 markdown 的注释语法，所以并不会在正文里渲染，以后即使你更换博客主题，也不会影响内容的展示。
```
[ pixiv: 45213461 ]: #'https://raw.githubusercontent.com/GhostAdults/yoi/master/1.jpg’
```
**▼ 书单，友链，关于Issues格式规定**<br>
**书单：书单页面使用##做分割，内容示例如下：**
```
## ES6 标准入门

author: 阮一峰
published: 2017-09-01
progress: 正在阅读...
rating: 5,
postTitle: ES6 标准入门
postLink: https://ghostadults.github.io/#/post/13
cover: https://raw.githubusercontent.com/GhostAdults/yoi/master/ES6-标准入门.jpg
link: //www.duokan.com/book/169714
description: 柏林已经来了命令，阿尔萨斯和洛林的学校只许教 ES6 了...他转身朝着黑板，拿起一支粉笔，使出全身的力量，写了两个大字：“ES6 万岁！”（《最后一课》）。
```
**友链：友链页面使用##做分割，内容示例如下：**
```
## 阁子

link: //newdee.cf/
cover: //i.loli.net/2018/12/15/5c14f329b2c88.png
avatar: //i.loli.net/2018/12/15/5c14f3299c639.jpg
```
**关于：关于和灵感页面同样##做分割，内容示例如下：**
```
## test

内容随便
```
### Gitalk评论系统 ###
**▼ 新建一个监控App**<br>
惯例打开Settings 然后选择这个
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen4.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen4.png)
然后右上角新建一个App 第一个框的名是comment(也就是刚才需要创建的仓库其中一个)
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen5.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen5.png)

**▼ 将注册好的Client ID 和 Secret 配置到本地项目**
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen6.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen6.png)
修改config.js配置文件
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen7.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen7.png)
**▼ 给需要评论的文章建立一个Isuues**<br>
打开`comment`仓库
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen8.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Gitscreen8.png)
把咱们的 `文章页面链接` 给一条Open状态的Isuue当作标签。同时别忘了添加 `Gitalk` 标签
### LeanCloud 数据库 ###
借由第三方管理数据平台，存储咱们的文章热度还有浏览次数<br>

**▼创建一个开发版的应用(毕竟咱暂时还不需要商业版的)**<br>
首先打开[LeanCloud](https://console.leancloud.app/)官网，注册一个账号(必须是国际版)<br>
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean1.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean1.png)
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean2.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean2.png)

**▼创建三个class(Comment,Counter,Visitor)**
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean3.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean3.png)

**▼保存AppID和Appkey**
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean4.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean4.png)

**▼修改config.js文件**
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean5.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/Lean5.png)

### 本地项目部署到Pages ###
好啦！到此为止咱已经把相关的配置全都设置好了！下一步，咱就可以把项目部署到GitHub上了。<br>

**▼将本地项目部署到GitHub远程仓库**<br>
蝉时雨的主题自带了一键部署的功能 `deploy.sh`
在运行之前需要修改一下内容：<br>
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/deployt.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/deployt.png)
然后 `Shift+右键`管理员打开Powershell窗口执行 `deploy.sh` 等待上传完成<br>
*如果中间执行出现错误，可能是Git环境没有配置好可以[返回](#toc-head-8)前面再去看一下下*<br>

部署完成你的仓库应该就是这个样子的~
[![预览](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/ghostadults1.png "")](https://cdn.jsdelivr.net/gh/GhostAdults/blog-cdn/img/ghostadults1.png)

<font size=4px color="#B28FCE">接下来，你就可以在网址栏输入你的域名去看看你的博客啦~~</font><br>
下面附上我的Github pages博客：https://ghostadults.github.io/


### 后记 ###
基本上就差不多了。需要修改的地方不多，主要记住Github Issues 的配置<br>
不熟悉的小伙伴多试试就明白了~<br>
还是感谢蝉時雨大佬提供的开源项目！
如果大家比较喜欢这个主题的话， {{weixiao}} 可以去大佬仓库看看<br>
<font color="#B28FCE">蝉時雨</font>的博客：{{guilian}} [传送](https://chanshiyu.com)<br>
<font color="#B28FCE">蝉時雨</font>的仓库：{{guilian}} [传送](https://github.com/chanshiyucx/aurora)<br>

那么这样应该就可以了，如果有问题欢迎疯狂在下面评论我~<br>
非常歓迎です。