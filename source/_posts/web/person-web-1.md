title: 个人网页搭建流程-1
categories: 个人网页
tags: [hexo,git,github,node.js]
---
## 需求与准备
### 需求
本人想做一个个人网页,类似于博客，数据量不大，不涉及到数据库和后台，可以做静态网页，由于个人经济所限，不会花费资金购买域名和服务器
### 准备
根据需求，个人网页使用的域名是 `GitHub` 为每个用户提供的唯一域名(用户名.xxxx.github.io),由于个人网页类似于博客，采用hexo框架，选用 `hexo` 的主题是 `indigo`。
需要的准备有：`node.js`，`hexo框架`，`git`，`GitHub的账号`
## 安装
### 安装node.js
进入英文版本的[node.js官网](https://nodejs.org/en/download/)，如果打开速度太慢，可以进入中文版本的[node.js官网](http://nodejs.cn/download/)，根据个人电脑选择下载 `node.js` 的安装包。

![](https://bingolil.github.io/images/node-down.png)

点击下载后的 `node.js` 的安装包。

![](https://bingolil.github.io/images/node-msi.png)

然后一直跟着窗口点击，最后在电脑的 `cmd` 目录行里面验证 `ndoe.js` 是否安装成功，`npm` 是 `node.js` 自带的包管理工具。

![](https://bingolil.github.io/images/node-validate.png)
### 安装git

进入[git官网](https://git-scm.com/downloads)下载 `git` 的安装包。

![](https://bingolil.github.io/images/git-down.png)

下载安装包后，点击安装包进行安装，一直跟着安装程序的提示点击下去。安装完成后，在window10自带搜索处输入 `git`。出现下图。

![](https://bingolil.github.io/images/git-validate.png)

点击图中 Git Bash，出现下图，说明git安装成功。

![](https://bingolil.github.io/images/git-cmd.png)

 安装 `git` 成功后为 `git` 设置用户名和邮箱，如下图。

![](https://bingolil.github.io/images/git-config.png)

### 安装hexo
安装 `hexo` 可以查看[hexo官网](https://hexo.io/zh-cn/docs/)安装教程。在电脑的 `cmd` 命令行环境下，运行 `npm install -g hexo-cli` 命令，电脑会自动全局安装 `hexo框架`。安装完成后，运行 `hexo -version` 命令，出现下图，证明 `hexo` 安装成功。

![](https://bingolil.github.io/images/hexo-version.png)

### GitHub账号
进入[GitHub官网](https://github.com)首页，点击导航栏中的 `Sign up` 进入到[注册页面](https://github.com/join?source=header-home)（如下图），在页面中填写用户名，密码和邮箱。

![](https://bingolil.github.io/images/github-join.png)

注意：`GitHub账号` 的用户名是唯一的
