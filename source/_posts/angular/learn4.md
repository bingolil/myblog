title: Angular教程-4-路由1

categories: Angular

tags: [Angular,Angular路由]

---
`路由` 在 `Angular` 中是一个非常重要的模块，一个好的路由对项目的开发有着巨大的促进作用。

`Angular` 是单页面应用，在用户使用程序时，`Angular` 的 `路由` 能让用户从一个视图导航到另一个视图。
## 基本路由
### 路由配置文件
在 `Angular` 项目中，`Angular` 的路由配置信息一般存放在单独的文件中（如xxxx-routing.module.ts），当然，放到 `Angular` 的模块中也可以。

一般来说，存放路由配置信息的文件的生成方式有两种，一是手动生成，一是通过命令自动生成。
> **手动生成**

在 `Angular` 项目的app文件夹目录下，手动新建一个文件，名为 `app-routing.module.ts`，将路由配置信息放到该文件中。

> **自动生成**

在 `cmd` 环境中，新建项目时，增加一个 `--routing` 的参数，如下所示：

```
ng new 项目名 --routing
```

使用该命令创建项目，会在项目的项目文件夹 `app` 下面生成 `app-routing.moudle.ts` 文件，其内容如下所示。
app-routing.module.ts

```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = []; //存放路由配置信息

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
由于是使用命令行生成的路由配置信息文件，`app-routing.module.ts` 文件中的 `AppRoutingMdule` 模块自动被 `AppModule` 根模块 `import` 了，不需要开发者在 `AppModule` 根模块里面去手动引用路由模块；`app.component.html` 页面中 `router-outlet` 占位符也会自动生成。 

开发者在使用 `懒加载` 时，需要新建一个模块，新建的模块里面也存在路由，开发者也可以使用命令创建模块里面的路由信息配置文件，其命令如下所示：

```
ng g m 模块名 --routing  //最简写法
```

### 路由相关属性
`Angular` 的路由中涉及到许多的属性，如下所示。

> **Rotues：** 存在于路由模块中，存放路由配置信息，即那个 `Url` 对应那个组件以及在那个 `outlet-outlet` 中展示组件，其用法: `const routes: Routes = [ 配置信息 ]`

> **router-outlet：** 该属性是组件UI中的路由占位符，存在于组件UI中，其用法：`<router-outlet></router-outlet>`

> **routerLink：** 在组件UI中负责路由跳转，存在于组件UI中，其用法：`1，<a routerKink="xxx">xxxx</a>`，`2，<a [routerKink]="['xxx']">xxxx</a>`

> **routerLinkActive：** 当前路由被激活时的样式，存在于组件UI中，其用法：`<a routerLinkActive='active' routerKink="['xxx']">xxxx</a>`，即当前路由激活时，给当前 `a标签` 增加 `active` 样式类

> **Router：** 负责在运行时执行的路由对象，可以通过其 `navigate()`  方法和 `navigateByUrl()` 方法进行路由跳转，`Router` 是一个类，在 `ts` 文件中使用时，需要在 `ts` 文件中引用并实例化，其用法：`Router.navigate(['/xxx'])`

> **ActivedRoute：** 代表当前激活的路由对象，保存当前路由的 `URL` 以及路由参数，这是一个类，在 `ts` 中使用时，需要在 `ts` 文件中引用并实例化，其用法：`1，参数订阅 ActivedRoute.params.subscribe((data)=>{})`，`2，参数快照 ActivedRoute..snapshot.queryParams["id"]`

> **useHash：** 其路由使用哈希展现，即多了一个 `#` 号，存在于根模块 `app.component.ts` 中，其用法：`imports: [RouterModule.forRoot(routes,{useHash:true})]`

> **redirectTo：** 路由重定向，一般存在于根路由配置文件 `app-routing.module.ts` 中，其用法：`{path:'',redirectTo:'/home',pathMatch:'full'}`，即当前路由为空时，转到路由为 `home` 的页面

> **pathMatch：** 路由完全匹配，一般存在于根路由配置 `app-routing.module.ts` 文件中，其用法：`{path:'',redirectTo:'/home',pathMatch:'full'}`

### 示例
1，在 `cmd` 环境中，使用命令 `ng new learn-route --rouing` 创建 `learn-route` 项目

2，创建成功后，在 `cmd` 环境中，`cd learn-route` 命令进入 `learn-route`，使用命令 `ng g c home` 创建 `home` 组件，使用命令 `ng g c joke` 创建 `joke` 组件

其 `learn-route` 项目部分代码如下所示。

app.module.ts
```typescript
....//代码块
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { JokeComponent } from './joke/joke.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    JokeComponent
  ],
 ..../代码块
```

app-routing.mdule.ts
````typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
app.component.html
```HTML
....//代码块
<router-outlet></router-outlet>
```

3，修改代码，如下所示。
app-routing.module.ts
```typescript
....//代码块
import { HomeComponent } from './home/home.component';
import { JokeComponent } from './joke/joke.component';

const routes: Routes = [
  {path:'',redirectTo:'home',pathMatch:'full'},
  {path:'home',component:HomeComponent},
  {path:'joke',component:JokeComponent}
];
....//代码块
```

app.component.html
```HTML
<!--The content below is only a placeholder and can be replaced.-->
<nav class="navbar navbar-default" role="navigation">
    <div class="container">
        <div class="container-fluid">
            <div>
                <ul class="nav navbar-nav">
                 <!-- routerLink的两种写法 -->  
                    <li routerLinkActive="active">
                     <a routerLink='/home'>Home</a>
                    </li>
                    <li routerLinkActive="active">
                        <a [routerLink]="['/joke']">Joke</a>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</nav>
<div class="container">
    <router-outlet></router-outlet>
</div>
```

页面效果图如下所示

![home路由](https://bingolil.github.io/images/angular-route-home.png)

![joke路由](https://bingolil.github.io/images/angular-route-joke.png)

## 懒加载
###  优势
> **模块化管理：** 在实际的 `Angular` 项目中，可能存在几十上百的组件，每一根组件会在组件所属的模块里面声明，若项目所有的组件都在根模块 `app.moudle.ts` 里面声明，那会导致根模块代码庞大，并且不易管理，使用 `懒加载` 时，将相关的组件放到一个模块，组件在该模块声明，不用在根模块里面声明


> **用户体验：** `Angular` 开发的项目是单页面应用，在实际的 `Angular` 项目中，由于 `Angular` 项目过于庞大，如果不采用 `懒加载` 模式，用户进入到项目的页面时，浏览器会请求整个项目的文件，这会导致项目的页面打开速度过慢

### 示例
1， 接着上面代码，在 `cmd` 环境下使用命令 `ng g m poduct --routing` 生成需要懒加载的模块，该命令还在自动生成懒加载模块的子路由配置信息文件 `product-routing.module.ts`

2，使用命令 `ng g c product/list` 在 `product` 模块下生成 `lsit组件`

现在项目部分代码如下所示。
product.module.ts
```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ProductRoutingModule } from './product-routing.module';
import { ListComponent } from './list/list.component';

@NgModule({
  imports: [
    CommonModule,
    ProductRoutingModule
  ],
  declarations: [ListComponent]
})
export class ProductModule { }
```

product-routing.module.ts
```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class ProductRoutingModule { }
```

3，修改代码如下所示
app.component.html
```HTML
 <!-- 代码块 -->
    <ul class="nav navbar-nav">
        <li routerLinkActive="active">
            <a routerLink='/home'>Home</a>
        </li>
        <li routerLinkActive="active">
            <a [routerLink]="['/joke']">Joke</a>
       </li>
       <li routerLinkActive="active">
           <!-- 新增链接 -->
            <a [routerLink]="['/prodcut']">Product</a>
        </li>
    </ul>
<!-- 代码块 -->
<div class="container">
    <router-outlet></router-outlet>
</div>
```

app-routing.module.ts
```typescript
....//代码块
const routes: Routes = [
  {path:'',redirectTo:'home',pathMatch:'full'},
  {path:'home',component:HomeComponent},
  {path:'joke',component:JokeComponent},
  //惰性加载路由
  {path:'product',loadChildren:'./product/product.module#ProductModule'}
];
....//代码块
```

product-routing.module.ts
```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { ListComponent } from './list/list.component';

const routes: Routes = [
  {path:'',component:ListComponent}
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class ProductRoutingModule { }
```

其页面效果图如下所示

![home页面请求的文件](https://bingolil.github.io/images/angular-lazy-home.png)

![product模块中请求的文件](https://bingolil.github.io/images/angular-lazy-product.png)

4，对比上面两张图片，在 `home` 路由中请求文件和 `product` 路由中请求的文件有一定的区别，在 `product` 路由中多请求了 `product-product-module.js` 文件。即用户进入项目页面时，不会请求项目所有的文件，这提高了项目的用户体验，同时还可以将和 `product` 模块相关的组件或服务放到项目文件中的 `product` 文件夹下，进行模块化管理。

> **注意：** 若初始化路由重定向路由到 `product` 下，那么 `product-product-module.js` 文件一开始就会被加载，因为 `product` 路由对应的文件中存在 `product-product-module.js` 文件

## 子路由
在实际 `Angular` 项目中，路由不止一层，即路由还会嵌套路由，被嵌套的路由就是子路由，又或者叫二级路由。
### 意义
在 `惰性加载` 的模块 `setting` 中，存在 `product` 和 `joke` 两个页面，分别用 `setting/product` 路由和 `setting/joke` 进行管理，两个页面有一部分内容是相同的，若两个页面使用两个组件完成 `setting` 下的所有内容，那么两个页面相同的部分被实现两次，这显然增加了开发者的负担。这时，可以使用子路由解决，即路由嵌套路由。
### 示例解决
1，使用命令 `ng g m setting --routing` 创建 `setting` 模块

2，使用命令 `ng g c setting/layout`，`ng g c setting/product` 和 `ng g c setting/joke` 创建 `layout` 组件，`product` 组件和 `joke` 组件

3，修改代码如下所示。
app.component.html
```HTML
 <!-- 代码块 -->
    <ul class="nav navbar-nav">
        <li routerLinkActive="active">
            <a routerLink='/home'>Home</a>
        </li>
        <li routerLinkActive="active">
            <a [routerLink]="['/joke']">Joke</a>
       </li>
       <li routerLinkActive="active">
            <a [routerLink]="['/prodcut']">Product</a>
        </li>
         <li routerLinkActive="active">
            <!-- 新增链接 -->
            <a [routerLink]="['/setting']">setting</a>
        </li>
    </ul>
<!-- 代码块 -->
<div class="container">
    <router-outlet></router-outlet>
</div>
```

app-routing.module.ts
```typescript
const routes: Routes = [
  {path:'',redirectTo:'home',pathMatch:'full'},
  {path:'home',component:HomeComponent},
  {path:'joke',component:JokeComponent},
  {path:'product',loadChildren:'./product/product.module#ProductModule'},
  //新增setting路由
  {path:'setting',loadChildren:'./setting/setting.module#SettingModule'}
];
```

product/setting-routing.module.ts
```ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { LayoutComponent } from './layout/layout.component';
import { ProductComponent } from './product/product.component';
import { JokeComponent } from './joke/joke.component';

const routes: Routes = [
  {
  	path:'',component:LayoutComponent,
  	children:[ //子路由
  	  {path:'',redirectTo:'product',pathMatch:'full'},
  	  {path:'product',component:ProductComponent},
  	  {path:'joke',component:JokeComponent}
  	]
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class SettingRoutingModule { }
```

product/laout/layout.component.html
```HTML
<div class="jumbotron">
	<h1>共享部分</h1>
</div>
<ul class="nav nav-tabs">
  <li routerLinkActive="active"><a routerLink="product">product</a></li>
  <li routerLinkActive="active"><a [routerLink]="['joke']">joke</a></li>
</ul>
<!-- 子路由占位符 -->
<router-outlet></router-outlet>
```

其页面效果图如下所示

![setting/product路由](https://bingolil.github.io/images/angular-setting-1.png)


![setting/product路由](https://bingolil.github.io/images/angular-setting-2.png)

其中共享部分可以做成一个组件，该组件在 `setting` 模块中声明，在 `product/laout/layout.component.html` 中引用也可以达到同样的效果。

## 预加载
### 意义
在 `Agnular` 项目中，使用 `懒加载` 时有助于提高用户体验和模块化管理项目。

假如现在有一个新的模块 `prod`，开发者使用懒加载模式加载这个模块，但是开发者可以预见 `prod` 模块会被用户频繁浏览（用户初始进入项目是 `home` 路由，然后大部分用户会浏览 `prod` 模块），在加载时 `prod` 模块时，浏览器会停顿一小段时间（需要加载 `prod` 模块对应的文件），这会对降低用户体验，而预加载就是解决浏览器停顿一小段时间的方法。

### 代码准备

使用命令 `ng g m prod --routing` 新建 `prod` 模块，然后使用命令 `ng g c prod/layout` 在  `prod` 模块下新建 `layout` 组件。最后修改代码如下所示。

app-routing.module.ts
```typescript
....//代码块

const routes: Routes = [
  {path:'',redirectTo:'home',pathMatch:'full'},
  {path:'home',component:HomeComponent},
  {path:'joke',component:JokeComponent},
  {path:'product',loadChildren:'./product/product.module#ProductModule'},
  {path:'setting',loadChildren:'./setting/setting.module#SettingModule'},
  //新增路由
  {path:'prod',loadChildren:'./prod/prod.module#ProdModule'}
];

....//代码块
```

app.component.html
```HTML
 <!-- 代码块 -->
    <ul class="nav navbar-nav">
        <li routerLinkActive="active">
            <a routerLink='/home'>Home</a>
        </li>
        <li routerLinkActive="active">
            <a [routerLink]="['/joke']">Joke</a>
       </li>
       <li routerLinkActive="active">
            <a [routerLink]="['/prodcut']">Product</a>
        </li>
        <li routerLinkActive="active">
            <a [routerLink]="['/setting']">setting</a>
        </li>
        <li routerLinkActive="active">
            <!-- 新增链接 -->
            <a [routerLink]="['/prod']">prod</a>
        </li>
    </ul>
<!-- 代码块 -->
<div class="container">
    <router-outlet></router-outlet>
</div>
```

prod-routing.module.ts
```typescript
....//代码 块
import { LayoutComponent } from './layout/layout.component';
const routes: Routes = [
  {path:'',component:LayoutComponent}
];
....//代码块
```

### 自带预加载策略
`Angular` 对于项目的预加载功能有一个自带的策略。其代码如下。
app-routing.module.ts
```typescript
import { Routes, RouterModule, PreloadAllModules } from '@angular/router';
....//代码块

@NgModule({
  imports: [RouterModule.forRoot(routes,{ preloadingStrategy:  PreloadAllModules })],
  exports: [RouterModule]
})
....//代码块
```

在 `Angular` 项目中，使用 `Angular` 自带的预加载策略时， 只需要修改 `app-routing.module.ts` 根路由文件，在根路由文件中引入 `PreloadAllModules` 模块，并在根文件中如上使用即可。

其效果图如下所示。

![](https://bingolil.github.io/images/angular-allprod.png)

在 `app-routing.module.ts` 文件中，加载模块的模式是 `懒加载` 模式，但是由于使用了 `预加载`，从上图中可以看到在 `home` 路由时，项目加载了对应 `prod` 路由下的 `prod-prod-module.js` 文件，说明 `Angular` 自带的预加载策略使用成功。

在上图中，可以看到由于使用了预加载，加载了所有的懒加载模块，这是 `Angular` 自带加载策略的一个缺陷。

对于 `Angular` 预加载，开发者可以定义符合开发者需求的预加载策略，即自定义预加载

### 自定义预加载
1，使用命令 `ng g s own-prod` 新建一个服务，修改 `own-prod.service.ts` 文件如下所示。

own-prod.service.ts
```typescript
import { Injectable } from '@angular/core';
import { Route, PreloadingStrategy } from '@angular/router';
import { Observable, of } from 'rxjs';
@Injectable({
  providedIn: 'root'
})
export class OwnProdService implements PreloadingStrategy{
    preload(route: Route, load: () => Observable<any>): Observable<any> {
        if (route.data && route.data['preload']) {
            return load();
        } else {
            return of(null);
        }
    }
}
```

在路由中使用上面新建的服务，`app-routing.module.ts` 代码如下所示。

```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule, PreloadAllModules } from '@angular/router';

import { OwnProdService } from './own-prod.service'
import { HomeComponent } from './home/home.component';
import { JokeComponent } from './joke/joke.component';

const routes: Routes = [
  {path:'',redirectTo:'home',pathMatch:'full'},
  {path:'home',component:HomeComponent},
  {path:'joke',component:JokeComponent},
  {path:'product',loadChildren:'./product/product.module#ProductModule'},
  {path:'setting',loadChildren:'./setting/setting.module#SettingModule'},
  //根据路由中data的preload的true或false判断是否预加载
  {path:'prod',loadChildren:'./prod/prod.module#ProdModule',data:{preload:true}}
];

@NgModule({
  imports: [RouterModule.forRoot(routes,{ preloadingStrategy:  OwnProdService })],//使用服务
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

使用了预加载后，在浏览器中效果图如下所示。
![](https://bingolil.github.io/images/angular-route-prod.png)






