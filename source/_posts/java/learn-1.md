title: Java入门-1-开发环境搭建
categories: Java
date: 2019-07-06
tags: [Java]
---

## 安装JDK
### 下载JDK
进入 [Oracle 官网下载页面](https://www.oracle.com/technetwork/java/javase/downloads/index.html) 下载 `JDK` 文件，下载的  `JDK` 版本是 `JDK12`（目前最新）。如图1所示

![图1](https://bingolil.github.io/images/java-jdk-12.png)

点击图中的下载按钮，进入 `JDK12` 的下载文件页面，根据需要安装 `JDK` 电脑的配置，选择不同的文件进行下载。如图2所示

![图2](https://bingolil.github.io/images/java-jdk-down.png)

点击图2中的版本名称，下载  `JDK` 

### JDK 安装
将 `JDK12` 的 `exe` 文件下载到本地后，如图3所示

![图3](https://bingolil.github.io/images/java-jdk-exe.png)

然后跟着提示走，点击下一步，下一步，直到 `JDK` 安装完成（其中，安装 `JDK` 的地址选择安装到 D 盘的 Java 文件夹下）

安装完成后，进入 D 盘的 Java 文件夹下查看，可以看见多一个 `jdk-12.0.1` 的文件夹，如图4所示，自此 JDK 安装成功

![图4](https://bingolil.github.io/images/java-jdk-path.png)

## 配置环境变量
到目前，`JDK` 已经安装成功，但是还不能使用，需要手动的去配置环境变量，进入配置环境变量的方法如图5所示

![图5](https://bingolil.github.io/images/windows-config-var.png)
### JAVA_HOME
`JAVA_HOME` 的用处：用于指定 `JDK` 安装在那个路径

配置 `JAVA_HOME` 环境变量 ，其值为 `D:\Java\jdk-12.0.1`（本地的 `JDK` 安装路径），其结果如下图所示

![JAVA JAVA_PATH 环境变量](https://bingolil.github.io/images/java_home.png)

### PATH
`PATH` 的用处：指定 `JDK` 命令文件的位置，即安装 `JDK` 目录下的 bin 目录

配置 `PATH` 环境变量，其值为 `D:\Java\jdk-12.0.1\bin` ，其过程和结果如下所示

![JAVA PATH 环境变量](https://bingolil.github.io/images/java-path.png)

### CLASSPATH
`CLASSPATH` 的用处：用来指定开发者需要使用到的配置类库文件的位置，即安装 `JDK` 目录下的 lib 目录

配置 `CLASSPATH` 变量，其值为 `D:\Java\jdk-12.0.1\lib` ,其过程和结果如下所示

![JAVA PATH 环境变量](https://bingolil.github.io/images/java_classpath.png)

## 开发工具 IntelliJ IDEA
JDK 安装完成，环境变量配置完成。现在就可以进行 java 开发了，但是一款好的开发者工具能够帮助开发快速开发项目，java 的开发工具最流行一共有3款（大体上2款），分别是 `eclipse`，`myEclipse`，`IntelliJ IDEA`，此处安装的 JAVA 开发者工具是 `IntelliJ IDEA`

### 下载 IntelliJ IDEA
进入 [`IntelliJ IDEA` 官网下载页面](http://www.jetbrains.com/idea/download/#section=windows)，如下图所示

![IntelliJ IDEA 下载](https://bingolil.github.io/images/idea-web.png)

`IntelliJ IDEA` 官网对应该开发工具有两个版本，分为社区版和旗舰版。其中，社区版是开源并免费的，而旗舰版的闭源收费的，自然，旗舰版的功能比社区版的功能强大。结合自身的情况选择一款下载（对于初学者，社区版可以满足开发需要）

### 安装 IntelliJ IDEA

`IntelliJ IDEA` `exe` 下载到本地，如下图所示，点击该文件，进行安装，跟着提示，下一步下一步的点下去即可安装成功

![IntelliJ IDEA 安装包](https://bingolil.github.io/images/idea-exe.png)