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
将下载好的indigo主题放到本地hexo初始化项目的 '/项目名/themes/' 下，将 '项目名/_config.yml' 文件中的themes属性由landscape改成indigo（如果是其它主题，themes的属性修改成其它主题名），
在https://github.com/yscoder/hexo-theme-indigo/wiki/%E5%AE%89%E8%A3%85中页面找那个查看使用hexo的indigo主题需要注意的事项，在cmd命令行到本地项目，跟着https://github.com/yscoder/hexo-theme-indigo/wiki/%E5%AE%89%E8%A3%85页面的教程添加插件。
在cmd命令行本地项目中使用hexo s命令，打开浏览器localhost:4000地址，可以看见hexo的indigo主题被使用
### 修改indigo主题
在本地项目中，修改 'indigo/_config.yml'文件，如下图所示。
去掉menu中的weibo和测试
![](https://bingolil.github.io/images/update-1.png)
关闭打赏功能
![](https://bingolil.github.io/images/update-2.png)
去掉title变化功能
![](https://bingolil.github.io/images/update-3.png)
