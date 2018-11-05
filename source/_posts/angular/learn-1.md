title: Angular初级教程-1
categories: Augular
tags: [angular安装,angular]
---
## 安装Angular6
前提准备：电脑上已经存在node.js（版本 为8.X或者10.X），因为使用angular一般都需要它的angular/cli脚手架，需要使用ndoe.js的包管理器npm。
安装的angular版本是angular6，进入电脑的cmd命令行环境下，运行如下命令
```typescript
npm install -g @angular/cli
```
安装完毕后，在该环境下运行如下命令
```typescript
ng -version
```
出现如下图所示，代表安装成功

![](https://bingolil.github.io/images/angular-verison.png)

## 新建项目
在cmd命令行环境下，运行以下命令
```typescript
ng new myApp
```
然后等待angular/cli脚手架自动新建一个myApp的项目，然后cd myApp目录下，运行以下命令
```typescript
ng server --open //open是参数，即运行成功后自动打开
                 //浏览器，并进入本地4200端口界面
```
在浏览器的本地4200端口出现如下图所示。

![](https://bingolil.github.io/images/csh-angular.png)
## Angular6项目文件
angular新建项目的目录结构如下所示

![](https://bingolil.github.io/images/angular-file.png)
### angular.json
在angular6以前，该文件名为.angular.json，angular6改成了angular.json，少了一个小数点，这个文件是脚手架的核心配置文件。
```typescript
"projects": {
    "myApp": {
      "root": "", //项目的根
      "sourceRoot": "src", //项目的源码地址，即项目源码都在src文件夹下
      "projectType": "application",
      "prefix": "app", //前缀，新建组件时，其选择器为 'app-组件名'
      "schematics": {},
      "architect": {
        "build": { //项目打包部分
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/myApp", //打包后的项目地址
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts", //项目的腻子文件
            "tsConfig": "src/tsconfig.app.json",
            "assets": [ //项目在资源地址
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [ //项目的引入的css文件
              "src/styles.css"
            ],
            "scripts": [] //项目引入的js文件
          }
```
### package.json
在项目下面存在pack.json文件，该json文件存放了项目的信息

```typescript
"name": "my-app", //项目名称
  "version": "0.0.0", //项目版本
  "scripts": {
    "ng": "ng",
    "start": "ng serve", //项目运行命令
    "build": "ng build", //项目打包命令
    "test": "ng test", //项目测试命令
    "lint": "ng lint",
    "e2e": "ng e2e" //项目端到端测试命令
```

### src/main.ts
这个文件记录了项目从哪个模块开始运行
```
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
```

### src/polyfill.ts

这个文件叫做腻子脚本，angular是默认对ie浏览器是不支持的，即新建项目后，在ie浏览器的本地4200端口页面是一片空白，将以下的注释取消，在ie浏览器中可以看到页面。
**将项目的默认注释取消后，其对ie浏览器的支持也不是特别好，[Angualr官网](https://angular.io/)在ie11中有的页面页打不开**
![](https://bingolil.github.io/images/polyfill.png)

### src/index.html

该文件记录了页面是从开始模块(AppModule)中声明的哪一个组件开始

    <html lang="en">
        <head>
            <meta charset="utf-8">
            <title>MyApp</title>
            <base href="/">
            <meta name="viewport" content="width=device-width, initial-scale=1">
             <link rel="icon" type="image/x-icon" href="favicon.ico">
        </head>
        <body>
            <app-root></app-root> //从AppModule的app-root组件开始展示页面
        </body>
    </html>

### src/app/app.module.ts
这个文件是项目默认的根模块

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent] //bootstrap代表从哪个组件开始编译
})
export class AppModule { }
```
在src/main.ts中引入的启动模块就这个模块，所以这个模块也叫做项目的根模块，在这个模块中，默认定义了一个根组件，即AppComponent，这个组件定义的选择器是 'app-root'，这个选择器在src/index.html中被使用，即这个组件也叫做根组件。
**以上文件为angular项目的主要的重要文件**
## Angular6命令大全
angular为使用者提供了诸多帮助，在cmd环境下，使用以下命令
```
ng --help
```
其命令如下图所示
![](https://bingolil.github.io/images/ng-help.png)
### ng add
ng add是angular6中新出现的命令，该命令使得向项目中添加新功能更加的方便。
例如，在项目根目录下运行如下命令添加**ng-zorro**
```
ng add ng-zorro-antd
```
命令运行完成后，使用ng serve运行项目，浏览器打开本地4200端口，出现下图。

![](https://bingolil.github.io/images/ng-zorro-ant.png)
这代表在这个项目中可以使用[ng-zorro官网](https://ng.ant.design/docs/introduce/zh)的组件

### ng new
该命令是创建一个新的angular项目，使用如下命令，可以创建一个新项目
```
ng new myApp //创建一个项目名为myApp的angular项目
ng new myApp --routing //这个命令同上，不同之处这个命令会在项目中
      //自动生成代表项目路由的文件 app-routing.module.ts
```
### ng generate
这个命令是angular项目中最主要的命令之一，新建组件，服务，指令，模块都可以在项目的根目录下使用这个命令进行创建。

| 默认创建名为home      | 完整写法   |    最简写法  |
| --------   | -----  | ----  |
| 组件| ng generate componetent home |  ng g c home    |
| 服务| ng generate service home| ng g s home |
| 指令| ng generate directive home |  ng g d home  |
| 管道 | ng generate pipe home |  ng g p home  |
| 类  | ng generate class home |  ng g cl home  |
| 接口 | ng generate interface home |  ng g i home  |
| 模块| ng generate module home |  ng g m home  |

**注意：**
1，angular以前的服务使用的依赖注入，在angular6版本中，服务采用的相依注入
2，创建组件component和类class都是c开头，所以使用最简写法有一定区别，如下所示，创建组件使用的是ng g c，创建类时使用的是ng g cl。
```
ng g c 组件名 //创建组件时使用的是c
ng g cl 类名 //创建类时使用的是cl
```
### ng build
angular项目采用的语言是typescript语法，由微软开发，该语法不直接在浏览器中运行，需要将ts(typescript)转换为js语法才可以运行，ng build命令就是对angular项目进行打包，生成静态的文件。
### 其它命令
angular还有其它的一些命令

| 命令 | 意义   | 
| --------   | -----  |
| ng serve| 运行项目，存在--open参数时自动打开浏览器 | 
| ng eject| 提取项目的webpack.config.json文件，一旦提取就不能还原|
| ng libary 库名| 用户创建库 | 
| ng help | 查找帮助  |
| ng version  |查看版本  |
| ng update |项目的依赖有重大改变时，自动更新代码  |
| ng project 项目名| 已移除，在本项目下新建一个项目 |
