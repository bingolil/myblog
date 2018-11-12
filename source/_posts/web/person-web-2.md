title: 个人网页搭建流程-2
categories: 个人网页
tags: [hexo,git,github,node.js]
---
## 搭建本地项目
### hexo初始化项目
进入电脑cmd命令行，在某个目录下使用hexo init命令，生成hexo初始化项目，如下图所示。
![hexo init](https://bingolil.github.io/images/hexo-init.png)
使用hexo s命令，在浏览器中打开localhost:4000地址，可以看到hexo新建的初始化项目页面，如下图所示。
![hexo init](https://bingolil.github.io/images/hexo-index.png)
### hexo主题下载
新建hexo项目自带默认主题landscape，样式不是特别好看，可去[hexo主题](https://hexo.io/themes/)查看自己喜欢的主题风格，一般在主题风格的页面都有到该主题源码的GitHub链接，进入到GitHub下载，复制git库的地址，在本地的git命令行下 克隆hexo主题。本个人网页使用的主题地址是https://github.com/yscoder/hexo-theme-indigo
### 使用indigo主题
将下载好的indigo主题放到本地hexo初始化项目的 '/项目名/themes/' 下，将 '项目名/_config.yml' 文件中的themes属性由landscape改成indigo（如果是其它主题，themes的属性修改成其它主题名)，如下图所示。

![](https://bingolil.github.io/images/theme-name.png)

在该主题的[GitHub页面](https://github.com/yscoder/hexo-theme-indigo/wiki/%E5%AE%89%E8%A3%85)，查看使用该主题的教程，在电脑的cmd环境下跟着教程添加插件。

在cmd命令行本地项目中使用hexo s命令，打开浏览器localhost:4000地址，可以看见hexo的indigo主题被使用
### 修改indigo主题
在本地项目中，修改 'indigo/_config.yml'文件，如下图所示。
去掉menu中的weibo和测试
![](https://bingolil.github.io/images/update-1.png)

关闭打赏功能
![](https://bingolil.github.io/images/update-2.png)

去掉title变化功能
![](https://bingolil.github.io/images/update-3.png)

使用自定义样式
![](https://bingolil.github.io/images/update-4.png)

## 搭建GitHub项目
使用账号登录GitHub后，进入创建[仓库页面](https://github.com/new)，创建仓库，仓库名为 '账号用户名.github.io'（这就是为什么账号的用户名是唯一的），如下图所示。
![GitHub-new](https://bingolil.github.io/images/github-new.png)

打开浏览器，输入地址 'GitHub用户名.github.io'，可以看见如下图页面。

![](https://bingolil.github.io/images/github-csh.png)

进入新搭建的GitHub项目的setting设置页面，有一个Git Pages属性，点击 'choice a theme' 按钮，进入GitHub自带主题页面，可以更换 'GitHub账号.github.io' 页面的主题，如下图所示。

![](https://bingolil.github.io/images/github-themes.png)

## 将本地项目传送的GitHub上

### 本地git和Github的连接
将本地git和GitHub连接需要将本地git的公钥发送的GitHub上。
[官网的教程](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)生成本地公钥。进入git bash命令行下，运行命令 'ssh-keygen'，在电脑的C盘里面生成了 .ssh 文件夹，如下图所示。

![](https://bingolil.github.io/images/git-key.png)

进入到GitHub上的添加 ssh key 的页面，如下图。

![](https://bingolil.github.io/images/github-newssh-1.png)

![](https://bingolil.github.io/images/github-newssh-2.png)

打开 id_rsa.pub 文件（该文件为公钥，id_rsa为私钥），将文件中的内容复制粘贴到上图中，点击 Add SSH key 按钮，这样本地的git和GitHub连接完成。

### 传送本地项目
在hexo官网教材中提供了关于hexo项目的[部署方案](https://hexo.io/zh-cn/docs/deployment)，将新建GitHub项目地址复制，在编辑器中打开本地的hexo项目，打开 '项目名/_config.yml' 文件，将GitHub项目地址粘贴到 '_config.yml' 文件中的 deplay 属性下的 git 属性上，如下图所示。

![](https://bingolil.github.io/images/hexo-config1.png)

在电脑cmd命令到项目地址下，使用命令 'npm install hexo-deployer-git --save' 下载插件，如下图所示。

![](https://bingolil.github.io/images/hexo-deploy.png)

然后运行 'hexo g -d' 或 'hexo d -g' 命令，可以看到，已经将本地git项目传送到新建的GitHub项目上，如下图所示。

![](https://bingolil.github.io/images/hexo-g-d.png)

打开浏览器，在地址栏输入 'GitHub用户名.github.io' 可以看到，本地的项目已经在GitHub上了。

