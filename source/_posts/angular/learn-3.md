title: Angular学习-3-生命周期钩子
categories: Angular
date: 2018-11-23
tags: [Angular,Angular生命周期钩子]
---
在`Angular` 中，每个组件都有一个被 `Angular` 管理的生命周期。

`Angular` 提供了生命周期钩子，把这些关键生命时刻（新建、更新和销毁）暴露出来，赋予开发者在它们发生时采取行动的能力。

除了组件具有生命周期钩子外，指令同样也具有生命周期钩子。

**注意：**指令不能实现带有 `content` 和 `view` 相关的钩子，因为在 `Angular2` 以后，指令不存在UI视图，即和 `content` 以及 `view` 没有关系。
`Angular` 组件的生命周期钩子如下图所示。

![](https://bingolil.github.io/images/angular-gouzi.png)

如图所示，`Angular` 组件的生命周期钩子执行顺序从图中的1到8，但没有一个组件或指令会实现全部的生命周期钩子。
其中青色的钩子可能在生命周期中执行多次，紫色的钩子在生命周期中只能执行一次。
## ngOnChanges
> 适用于组件和指令

`ngOnChanges` 钩子只有存在输入属性（`@Input`）时才能调用，如果不存在该属性，则不能调用该钩子。`ngOnChanges` 钩子在生命周期中可以被多次调用，但其第一次被调用发生 `ngOnInit` 钩子之前。
### 触发条件
一旦检测到该组件(或指令)的**输入属性**（`@Input`）发生了变化，`Angular` 组件就会调用 `ngOnChanges` 钩子 。
**注意：**若输入属性是一个对象，对象的某个属性的值变化时，不会触发这个钩子，只有该对象引用发生变化时，才会触发这个钩子。
### 用处
`ngOnChanges` 钩子在项目中被常用到的地方是组件。其相当于实现了 `angularjs` 的 `$scope.watch()` 功能。
### 示例
1，组件中有且只有1个 `@Input` 输入属性，且不为对象

代码如下所示
parent.component.html
```HTML
<div class="form-group">
  <input type="text" class="form-control" [(ngModel)]="parVar">
</div>
<hr>
<app-child [comVar]="parVar"></app-child>
```

child.component.ts
```typescript
import { Component, OnInit, Input, OnChanges, SimpleChanges } from '@angular/core';

....//代码块
                    //使用钩子，需要继承钩子的接口
export class ChildComponent implements OnInit,OnChanges {
  @Input() comVar:string;
  constructor() { }
  ngOnInit() {
    console.log('A')
  }
  //ngOnChanges钩子变化的信息存储在SimpleChanges对象里面
  ngOnChanges(changes: SimpleChanges) {
    console.log(changes);
  }
}
```

在浏览器中打开本地4200端口地址，按键盘上的 `f12`进入浏览器控制台，可以看见先输出 `SimpleChanges` 对象，该对象有一个 `comVar` 属性，这个`comVar` 属性的值是一个对象，该对象存储了3个属性，即 `comVar` 变量的当前值，前一个值和 `firstChange`（是否第一次改变，`boolean` 类型，`true` 代表第一次改变，`false` 代表不是第一次），然后才输出 `ngOnIint` 钩子中的 `A` 。

当在页面中的 `input框` 输入值的时候，`app-child` 组件的输入值发生了改变，触发了 `ngOnChanges` 钩子，控制台会出 `SimpleChanges` 对象，其属性 `comVar` 的值还是3个，即 `comVar` 的当前值，前一个值和 `firstChange`。

**即 `ngOnChanges` 钩子第一次被调用发生调用 `ngOnInit` 钩子之前，`ngOnChanges` 钩子可能被多次调用**

2，组件中有且只有多个 `@Input` 输入属性，且不为对象
其代码如下所示
parent.component.html
```HTML
<div class="form-group">
  <input type="text" class="form-control" [(ngModel)]="parVar1">
</div>
<div class="form-group">
  <input type="text" class="form-control" [(ngModel)]="parVar2">
</div>
<hr>
<app-child [comVar1]="parVar1" [comVar2]="parVar2"></app-child>
```

child.component.ts
```typescript
import { Component, OnInit, Input, OnChanges, SimpleChanges } from '@angular/core';

..../代码块

export class ChildComponent implements OnInit,OnChanges {
  @Input() comVar1:string;
  @Input() comVar2:string;
  constructor() { }
  ngOnInit() {
  }
  ngOnChanges(changes: SimpleChanges) {
    console.log(changes);
  }
}
```

在浏览器控制台中，可以看到输出的 `SimpleChanges` 对象，该对象有两个属性，分别是 `comVar1` 和 `comVar2` ，这两个属性的值为对象，分别存放着输入属性 `comVar1` 和 `comVar2` 的当前值，前一个值和 `firstChange`。

**注意：** 在 `input` 改变时，控制台输出的 `SimpleChanges` 对象只有当前绑定的值的属性，不会输出其它绑定绑定值的属性，因为其它绑定值的没有发生变化。

## ngOnInit
> 适用于组件和指令

`ngOnInit` 钩子在组件中已经被 `Angular` 默认实现了，`ngOnInit` 在 `Angular` 中被使用的次数最多，其在第一次 `ngOnChanges` 之后被调用。

在 `Angular` 中，`ngOnInit` 钩子主要的作用就是：
1. 在构造函数后马上执行复杂的初始化逻辑；
2. 在 `Angular` 设置完输入属性之后，对该组件进行准备。

## ngDoCheck
> 适用于组件和指令

### 变更检测
变更检测就是 `Angular` 检测视图和数据模型之间绑定的值是否发生了改变，当检测到模型中绑定的值发生改变时，同步到UI视图上。
>* `Angular` 的变更检测是通过 zone.js 库来实现的，保证组件的变化和UI视图一致
>* 组件中的任何异步事件都会触发变更检测
>* 每个组件都有独属于自己的变更检测器，当任何一个变更检测器检测到变化，zone.js 库会根据 `变更检测策略` 来检测组件，以判断组件是否需要更新模板。

### Angular变更检测策略

`Angular` 有两种变更检测策略，分别是 `Default` 策略和 `OnPush` 策略。

>Default策略
    `Default` 策略是 `Angular` 默认的变更检测策略，该策略会在发生变更时，`zone.js` 会检测所有的组件。

>OnPush策略
    `Onpush`策略的组件只有输入属性（@Input）发生改变时，才会检测该组件及其子组件。如果所有的组件都采用 `Default` 策略，当某个组件的变更检测器检测到变化，`zone.js` 会检测整个组件树，但它会跳过使用 `OnPush` 策略的组件。
### ngDoCheck和变更检测
>触发变更检测机制时会调用 `ngDoCheck` 钩子

### 示例
在 `ngOnChanges` 钩子中，若 `@Input` 输入属性是一个对象，修改该对象某个属性的值，不会触发 `ngOnChanges` 钩子，但其会触发 `ngDoCheck` 钩子，开发者可以利用这个钩子做开发者需要做的事情。其代码如下所示
par.component.ts
```typescript
....//代码块
  User={ //定义一个对象
    par1:null,
    par2:null
  }
```

par.component.html
```HTML
<div class="form-group">
  <input type="text" class="form-control" [(ngModel)]="User.parVar1">
</div>
<div class="form-group">
  <input type="text" class="form-control" [(ngModel)]="User.parVar2">
</div>
<hr>
<app-child [comObj]="User"></app-child>
```

child.component.ts
```typescript
import { Component, OnInit, Input, OnChanges, DoCheck } from '@angular/core';

....//代码块

export class ChildComponent implements OnInit,OnChanges,DoCheck {
  @Input() comObj:any;
 
 ....//代码块
 
  ngOnChanges() {
    console.log('触发了ngOnChanges钩子')
  }
  ngDoCheck(){
    console.log('发生了变更检测');
  }
}
```

在浏览器中打开本地4200端口地址，进入浏览器控制台，可以看到在浏览器中输出了 `触发了ngOnChanges钩子` 和 `发生了变更检测` ，不断改变页面中 `input` 框（两个 `input` ，随便哪一个都可以）的值，可以看到在控制台中，不断的输出 `发生了变更检测` ，但控制台中不会再输出 `触发了ngOnChanges钩子`，因为 `User` 输入对象引用没有发生改变，不会 `ngOnChanges` 钩子。

**注意：** 虽然 `Angular` 暴露了 `ngDoCheck` 钩子，但是由于 `ngDocheck` 钩子调用频繁，所以开发者尽量不要在 `ngDoCheck` 钩子中写入复杂的逻辑，否则会降低 `Angular` 项目的性能，影响用户体验。

## ngAfterContentInit
> 适用于组件

在 `Angular` 的8个生命周期钩子中，带有 `view` 和 `content` 的4个钩子只适用于组件。其它的4个钩子适用于指令和组件。
`ngAfterContentInit` 钩子只执行一次，在当把内容投影进组件之后调用这个钩子，在这个钩子里面可以访问被投影进来的组件。
**注意：** 在组件中，若没有发生组件投影，`ngAfterContentInit`  钩子还是会执行。
### 意义
在传统的HTML页面中，标签可以嵌套标签，而在 `Angular` 中，可以把组件看成是标签，一般情况下是不能直接组件嵌套组件。`内容投影shadow` 实现了在 `Angular` 中可以组件嵌套组件。在投影组件时，开发者可能需要在组件投影后马上进行一些操作，可以使用 `ngAfterContentInit` 钩子。
### 示例
app.component.html
```HTML
<app-shadow-wrap> 
  <app-content-child></app-content-child> //被投影的组件
</app-shadow-wrap>
```

在根组件中，页面UI代码如上所示，这时在页面中不会展示和 `app-content-child` 组件有关的UI内容
shadow-wrap.conponent.html
```HTML
<div class="container">
  <h1>内容投影</h1>
  <ng-content select="app-content-child"></ng-content> 
</div>
```

在 `app-shadow-wrap` 组件的UI代码中， `<ng-content select="app-content-child"></ng-content> ` 是占位符，这个占位符存放的就是根组件UI代码中的 `<app-content-child></app-content-child>` 组件，其通过 `<ng-content select="app-content-child"></ng-content> ` 中的 `select` 属性的值来确定这个占位符展示组件投影中的哪一个组件。
**注意：** `ng-content` 占位符中 `select` 属性还可以为类名，标签名和属性等。在该占位符中不应该有任何内容，若存在内容，也会被投影的组件内容覆盖。
如下所示：
```HTML
<ng-content select=".blue"></ng-content>//匹配class名为blue的显示内容 
<ng-content select="header"></ng-content>//匹配header标签的显示内容 
<ng-content select="[name]=red"></ng-content>//匹配name属性值为red的显示内容
```

content-child.component.html
```HTML
<p>这儿是content-child组件</p>
```

content-child.component.ts

```typescript
....//代码块
public comVar(){
  console.log("A");
}
```

到此，最基本的内容投影完成（关于内容投影，后面会讲解），假如开发者需要在 `shadow-wrap` 组件中访问 `content-child` 组件的值或者方法，可以通过调用 `ngAfterConentInit` 钩子实现。

shadow-wrap.component.ts
```typescript
import { Component, OnInit, ContentChild, AfterContentInit } from '@angular/core';
import { ContentChildComponent } from '../content-child/content-child.component';

....//代码块
export class ShadowWrapComponent implements OnInit,AfterContentInit {
  
  @ContentChild(ContentChildComponent) child1:ContentChildComponent;

  ....//代码块

  ngAfterContentInit(){
    this.child1.comVar();
  }
}

```

可以在控制台看到输出了 `A`，即在 `shadow-wrap` 组件中访问了 `content-child` 组件的public值或者方法。
**注意：**如果组件中没有发生组件投影，那么就不需要实现这个生命周期钩子。

### AfterContent 和 AfterView
`AfterContent` 钩子和 `AfterView` 相似。关键的不同点是子组件的类型不同。

>`AfterView 钩子`所关心的是 `ViewChildren`，这些子组件的元素标签会出现在该组件的模板里面。
>`AfterContent 钩子` 所关心的是 `ContentChildren`，这些子组件被 `Angular` 投影进该组件中。

## ngAfterContentChecked
> 适用于组件

代码结构和 `ngAfterContentInit` 相同，若被投影的组件发生了变更检测，需要在 `shadow-wrap` 组件中访问被投影组件 `content-child` 的公共属性或方法，这时开发者可以使用 `ngAfterContentChecked` 钩子查看被被投影组件的公共方法或属性。

## ngAfterViewInit
> 适用于组件

在组件和其所有子组件相应的 `视图` 初始化之后执行 `ngAfterViewInit` 钩子，只执行一次。
**注意：** 在执行 `ngAfterViewInit` 钩子时，说明组件的 `视图` 已经组装完毕，开发者不能在该钩子中修改和 `组件UI视图` 有关的属性。虽然在UI视图中显示修改成功，但浏览器控制台会报错。
### 错误示例1-修改组件属性
 view-init.component.ts
 
```typescript
....//代码块
  look:string='jackyy';
  ngAfterViewInit(){
    this.look='loook at';
  }
```

view-init.component.html
```HTML
<div>{{look}}</div>
```

其控制台台报错如下图所示

![](https://bingolil.github.io/images/angular-ngVInitError.png)

**结论：** 在一个变更检测周期中禁止一个视图被组装好之后再去更新视图。

在带有 `view` 的生命周期钩子（`ngAfterViewInit` 和 `ngAfterViewChecked`）中，禁止更新视图。在上面的 `示例` 中，若属性 `look` 没有出现在 `组件UI视图` 中，即修改 `look` 属性，不会更新 `组件UI视图`，那么浏览器控制台不会报错。
 
## ngAfterViewChecked
> 适用于组件

在 `Angular` 检查完组件中的绑定后调用 `ngAfterViewChecked` 钩子，在该钩子中，和 `ngAfterViewInit` 钩子一样，禁止开发者更新视图。

**注意：** 当父组和子组件都有该钩子时，子组件的该钩子先于父组件的该钩子执行。

每次执行该钩子，组件的 `UI视图` 更新完一次，可以在这个钩子中实时获取组件中某个 `DOM` 元素的信息，比如其在页面中的位置，高度或宽度等。
## ngOnDestory
> 适用于组件和指令

在 `ngOnDestory` 钩子中，代表组件或指令的生命周期来到了销毁之前，在该钩子中，开发者一般都是解绑事件或者取消订阅，或者清除定时器。
> **解绑事件：** 比如开发者在某个组件中使用了 `jquery` 绑定了一个点击页面 `body` 事件，路由进入到其它页面中时，不需要这个点击事件，但由于 `Angular` 路由切换机制，会保留这个点击事件，这就需要在组件中的 `ngOnDestory` 钩子里面解除绑定的点击事件，即该组件被销毁后，不会存在点击 `body` 事件。清除定时器的思路和解绑事件的思路一样。

> **取消订阅：** 在 `Angular` 项目中，组件间的通讯有一部分是 `订阅对象` 完成，为了提升用户体验和项目性能，需要取消订阅。

取消订阅代码如下所示：

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { ActivatedRoute } from '@angular/router'
import { Subscription } from 'rxjs';

....//代码块
  
  subscription:Subscription
  constructor(private routeInfo: ActivatedRoute, private){}
  ngOnInit(){                         //订阅路由参数
    this.subscription = this.routeInfo.queryParams.subscribe((data)=>{
      ....//代码块
    })
  }
  ngOnDestroy() { //取消订阅
    this.subscription.unsubscribe();
  }

```

## 生命周期钩子总结
### 初始化阶段
在组件的生命周期中，组件的初始化由一个构造函数3个生命周期钩子完成
> `构造函数`：初始化对象
> `ngOnChanges`：初始化组件输入属性 (@Input)
> `ngOnInit`：初始化除了输入属性外的所有属性
> `ngDoCheck`：做一次变更检查

### 渲染阶段
组件初始化完成后，开始渲染UI视图，首先渲染的就是被投影进来的内容，如果被投影的内容渲染完毕后，会调用 `ngAfterContentInit` 钩子和 `ngAfterContentChecked` 钩子，被投影的内容渲染完毕后，开始渲染组件的内容，当组件内容也渲染完毕后，会调用 `ngAfterViewInit` 钩子和 `ngAfterViewChecked`钩子。

至此，组件的渲染完毕，组件进入存活阶段，即与用户的交互阶段。

### 存活阶段
在组件的存活阶段，由于用户和组件发生了交互，该阶段主要由4个生命周期钩子完成。

> `ngOnChanges`：发生交互，组件的输入属性改变，会触发该钩子。
> `ngDoCheck`：数据每发生一个变化，会触发一次变更检测，会调用一次该钩子。
> `ngAfterContentChecked`：被投影的内容每发生一次变更检测，会调用一次该钩子。
> `ngAfterViewChecked`：每发生一次视图更新，会调用一次该钩子。

### 销毁阶段
组件进入销毁阶段，就只有一个钩子 `ngOnDestory` 被调用，在该钩子中，一般都是销毁一些引用的资源，比如取消订阅，清除定时器，解除绑定事件等。