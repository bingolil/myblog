title: Angular学习-1-新手入门
categories: Angular
date: 2018-11-02
tags: [Angular安装,Angular]
---
## 安装Angular6
前提准备：电脑上已经存在 `node.js`（版本 为8.X或者10.X），因为使用 `Angular` 一般都需要它的 `angular/cli` 脚手架，需要使用 `ndoe.js` 的包管理器 `npm`。
安装的 `Angular` 版本是 `Angular6`，进入电脑的 `cmd` 命令行环境下，运行如下命令
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
在 `cmd` 命令行环境下，运行以下命令
```typescript
ng new myApp
```
然后等待 `angular/cli` 脚手架自动新建一个 `myApp` 的项目，然后 `cd myApp` 目录下，运行以下命令
```typescript
ng server --open //open是参数，即运行成功后自动打开
                 //浏览器，并进入本地4200端口界面
```
在浏览器的本地4200端口出现如下图所示。

![](https://bingolil.github.io/images/csh-angular.png)
## Angular6项目文件
`Angular` 新建项目的目录结构如下所示

![](https://bingolil.github.io/images/angular-file.png)
### angular.json
在 `Angular6` 以前，该文件名为.angular.json，`Angular6` 改成了angular.json，少了一个小数点，这个文件是脚手架的核心配置文件。
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
            "styles": [ //项目引入的css文件
              "src/styles.css"
            ],
            "scripts": [] //项目引入的js文件
          }
```
### package.json
在项目下面存在 `pack.json` 文件，该 `json` 文件存放了项目的信息

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
```typescript
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

这个文件叫做腻子脚本，`Angular` 是默认对 `ie浏览器` 是不支持的，即新建项目后，在 `ie浏览器` 的本地4200端口页面是一片空白，将以下的注释取消，在ie浏览器中可以看到页面。
**将项目的默认注释取消后，其对i e浏览器 的支持也不是特别好，[Angualr官网](https://angular.io/)在ie11中有的页面页打不开**
![](https://bingolil.github.io/images/polyfill.png)

### src/index.html

该文件记录了页面是从开始模块 (`AppModule`) 中声明的哪一个组件开始
```HTML
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
```

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
在 `src/main.ts` 中引入的启动模块就这个模块，所以这个模块也叫做项目的根模块，在这个模块中，默认定义了一个根组件，即 `AppComponent`，这个组件定义的选择器是  `app-root`，这个选择器在 `src/index.html` 中被使用，即这个组件也叫做根组件。
**以上文件为 Angular 项目的主要的重要文件**
## Angular6命令大全
`Angular` 为开发者者提供了诸多帮助，在 `cmd` 环境下，使用以下命令
```
ng --help
```
其命令如下图所示
![](https://bingolil.github.io/images/ng-help.png)
### ng add
`ng add` 是 `Angular6` 中新出现的命令，该命令使得向项目中添加新功能更加的方便。
例如，在项目根目录下运行如下命令添加 **`ng-zorro`**
```
ng add ng-zorro-antd
```
命令运行完成后，使用 `ng serve` 运行项目，浏览器打开本地4200端口，出现下图。

![](https://bingolil.github.io/images/ng-zorro-ant.png)
这代表在这个项目中可以使用[ng-zorro官网](https://ng.ant.design/docs/introduce/zh)的组件

### ng new
该命令是创建一个新的 `Angular` 项目，使用如下命令，可以创建一个新项目
```
ng new myApp //创建一个项目名为myApp的angular项目
ng new myApp --routing //这个命令同上，不同之处这个命令会在项目中
      //自动生成代表项目路由的文件 app-routing.module.ts
```
### ng generate
这个命令是 `Angular` 项目中最主要的命令之一，新建组件，服务，指令，模块都可以在项目的根目录下使用这个命令进行创建。

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
1，`Angular` 以前的服务使用的依赖注入，在 `Angular6` 版本中，服务采用的相依注入
2，创建组件 `component` 和类 `class` 都是 `c` 开头，所以使用最简写法有一定区别，如下所示，创建组件使用的是 `ng g c`，创建类时使用的是 `ng g cl`。
```
ng g c 组件名 //创建组件时使用的是c
ng g cl 类名 //创建类时使用的是cl
```
### ng build
`Angular` 项目采用的语言是 `typescript` 语法，由微软开发，该语法不直接在浏览器中运行，需要将 `ts(typescript)` 转换为 `js` 语法才可以运行，`ng build` 命令就是对 `Angular` 项目进行打包，生成静态的文件。
### 其它命令
`Angular` 还有其它的一些命令

| 命令 | 意义   | 
| --------   | -----  |
| ng serve| 运行项目，存在--open参数时自动打开浏览器 | 
| ng eject| 提取项目的webpack.config.json文件，一旦提取就不能还原|
| ng libary 库名| 用户创建库 | 
| ng help | 查找帮助  |
| ng version  |查看版本  |
| ng update |项目的依赖有重大改变时，自动更新代码  |
| ng project 项目名| 已移除，在本项目下新建一个项目 |
