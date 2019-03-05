title: webpack学习-1-新手入门
categories: webpack
date: 2019-03-05
tags: [webpack,webpack4]
---

## webpack介绍

### 描述

`webpack` 是模块打包工具，在开发中，各种文件（`js`，`css`，图片，`json` 等）可以看成是一种独特的模块资源，通过 `webpack`，可以将各种资源打包压缩到指定的文件中，还可以将在浏览器上不能直接运行的资源（`scss`，`less`，`typescript` 等）转换成合适的格式供浏览器使用
### webpack应用场景
1. 开发存在模块化（将复杂的程序细化为更小的文件）
2. 类似于 `JavaScript` 的拓展语言 `typescript`（浏览器不能直接运行）
3. `less` 和 `scss` 等文件的处理
....

### webpack和gulp以及grunt
`webpack` 和其它两个工具有一定的相似性，但其实它们没有可比性，`webpack` 是一种模块化的解决方案，而其它两个工具是优化开发流程的工具。但是大多数情况向 `webpack` 可以代替 `gulp` 和 `grunt`

## 开始使用
### 安装
在 `cmd` 环境下，使用 `node.js` 的包管理器 `npm` 运行以下两个命令，全局安装 `webpack`

```
npm install -g webpack //全局安装webpack 此处为webpack4
```

```
npm install -g webpack-cli //webpack4 并没有和 webpack3 一样包含 webapck-cli，
                          //所以需要全局安装 webpack-cli，在webpack3中不用
```
### 基本的webpack使用
1，在 `cmd` 环境中，进入某个文件目录（项目位置），使用命令 `md learn-webapck` 新建 `learn-webapck` 文件夹，`cd learn-webpack` 进入该文件夹下
2，使用命令 `npm init -y` 创建默认的 `package.json` 文件，使用命令 `npm install --save-dev webpack` 下载 `webpack` 到本地（非全局），使用命令 `npm install --save-dev webapck-cli` 下载 `webpack-cli` 到本地（项目中，非全局）
3，在 `learn-webpack` 文件下新建 `app文件夹` 和 `public文件夹`，在 `public文件夹` 下创建 `index.html` 文件，在 `app文件夹` 下创建 `main.js` 文件。此时，项目的目录结构如下所示

![](https://bingolil.github.io/images/learn-webpack-1.png)

4，其中各个文件的内容如下所示
index.html
```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>webpack学习</title>
  <link rel="stylesheet" href="">
</head>
<body>
  <div>学习webpack</div>
  <script src="bundle.js"></script>
</body>
</html>
```

main.js
```javascript
function getData(){
  console.log("获取到数据")
}
getData();
```

5，在 `cmd` 环境中，直接运行 `webpack app/main.js public/bundle.js` 命令会报错误，如图所示

![](https://bingolil.github.io/images/learn-webpack-2.png)

在上图中，出现了一个警告和一个错误，这是因为在 `webpack4` 中， `webpack app/main.js public/bundle.js` 命令不能这样使用了，需要在命令中说明执行该命令的环境和出入口要明写。修改命令如下所示
```javascript
webpack app/main.js --output-filename public/bundle.js --output-path. --mode development
```
> **注意：**上面命令中 . 是必选要的，将 `--mode development` 和命令的前部分隔开

在 `cmd` 环境下运行如上命令，打包成功如下图所示

![](https://bingolil.github.io/images/learn-webpack-3.png)

在项目中的 `public` 目录下，新出现了一个 `bundle.js` 文件

在浏览器中打开 `index.html` 文件，其结果如下图所示

![](https://bingolil.github.io/images/learn-webpack-4.png)

从上图中可以知道，在 `app/main.js` 文件中写入的 `getData` 方法在浏览器中被执行了。

### webpack.config.js
在上面的叙述中，可以知道，在 `cmd `环境下使用 `webpack` 时会写一大段命令，这无疑是非常不友好的，`webpack` 允许我将 `webpack` 打包配置写到一个名为 `webpack.config.js` 文件里面

新建 `webpack.config.js` 文件，并修改该文件如下所示

```javascript
const path=require('path');
module.exports={
  entry:__dirname + '/app/main.js',    //entry入口
  output:{
    path:__dirname + '/public',      //出口文件夹
      filename:'bundle.js'          //名称
  }
}
```

修改 `package.json` 文件如下所示

```json
...//部分内容
"scripts": {
  "build": "webpack --mode production",
  "dev": "webpack --mode development",
  "test": "echo \"Error: no test specified\" && exit 1"
},
...//部分内容
```

在 `cmd` 环境下，运行 `npm run build`（代表生产环境）和运行 `npm run dev`（代表开发环境）都可以进行 `webpack` 打包

## webpack 的强大之处


### 生成Source Maps（使调试更容易）
开发总是离不开调试，良好的调试可以有效的提高开发效率。有的时候，通过打包后的文件不容易找到代码出错所对应的地方，而 `source maps` 可以解决这个问题

通过简单的配置，打包的时候生成一个 `source maps` 文件，这为我们提供了一种源文件和编译文件的对应方法，使得编译后的文件可读性更高，更容易调试

在 `webpack.config.js` 文件中加入 `devtool` 属性，该属性具有几个常见配置选项，如下所示

<style>
  table th:first-of-type {
    width: 220px;
  }
</style>

| 模式 | 描述   | 
| --------| -----  | 
| source-map |在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度 |  
| eval-source-map|使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项|
| cheap-source-map |在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便| 
| cheap-module-source-map| 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点| 


`source-maps` 并不少只有上面的这几种模式，其使用推荐如下所示
> 开发环境推荐：cheap-module-eval-source-map
> 生产环境推荐：cheap-module-source-map

在 `webpack.config.js` 中加入 `devtool` 属性，其代码如下所示

```javascript
const path=require('path');
module.exports={
devtool: 'eval-source-map', //新增属性
  entry:__dirname + '/app/main.js',
  output:{
    path:__dirname + '/public',
    filename:'bundle.js'     
  }
}
```

### 搭建本地服务器
`webpack` 提供了一个可选的本地开发服务器，该服务基于 `node.js` 构建。使用该服务器可以让浏览器监听代码修改并在浏览器中实时刷新

运行 `npm install --save-dev webapck-dev-server` 命令下载 `devserver` 插件，以下是一些它的常见配置选项

| devserver的配置选项  | 功能描述   | 
| --------   | ----- | 
| contentBase|默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录） | 
| post    | 设置默认监听端口，如果省略，默认为”8080“  | 
| inline |  设置为true，当源文件改变时会自动刷新页面| 
| historyApiFallback |在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html| 

修改项目中 `package.json` 文件和 `webpack.config.js` 文件如下所示

package.json
```
//部分内容，该
"scripts": {
  "build": "webpack --mode production",
  "dev": "webpack --mode development",
  "serve": "webpack-dev-server --open",
  "test": "echo \"Error: no test specified\" && exit 1"
}
//部分内容
```

webpack.config.js
```typescript
const path=require('path');
module.exports={
  devtool: 'eval-source-map',
  entry:__dirname + '/app/main.js', //entry入口
  output:{
    path:__dirname + '/public', //出口文件夹
	filename:'bundle.js'        //名称
  },
  devServer:{
	contentBase:__dirname+"/public", //本地服务器加载页面所在的目录
	historyApiFallback:true, //不跳转
	inline:true,  //实时刷新
	port:3200
  }
}
```

在 `cmd` 环境下，运行命令 `npm run serve`，浏览器自动打开本地的 3200 窗口，在编辑中修改项目中源文件 `main.js`，浏览器会自动刷新（在此处修改 `index.html` 浏览器是不会实时刷新的，因为 `webpack` 没有对 `index.html` 进行打包）