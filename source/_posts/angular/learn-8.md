title:  Angular学习-8-Rxjs
categories: Angular
date: 2019-02-01
tags: [Angular,Rxjs]
---
## 什么是Rxjs
`Rxjs` 是 `javascript` 的响应式扩展。其功能是利用响应式编程的模式来实现 `javascript` 的异步编程

`Rxjs` 相关的对象如下所示

> * `Observable`（可观察对象）：表示一种概念，其支持发布在与订阅者之间传递信息
> * `observer`（观察者）：回调函数的集合，用于处理可观察对象提供的值
> * `Subscription`（订阅）：代表可观察对象的执行，一般用于取消订阅
> * `Subject`（主题）：继承 `Observable` 类，是特殊的 `Observable` 对象
> * `opterator`（操作符）：采用函数式编程风格的纯函数

## Observable（可观察对象）
`Observable` 是 `Rxjs` 的核心概念之一，它是一个可以被观察的对象，其状态改变时，会将其改变推送给观察它的对象（观察者）
### Observables
可观察对象支持在应用中发布者和订阅者之间传递信息，进行事件处理、异步编程和处理多个值，其有很大的优势。

`Observable` 是声明式，即当有观察者订阅它之后，它才会同步或异步的返回 0 或多个值，其代码如下所示

```typescript
import { Observable } from 'rxjs';
....//代码块

var observable=Observable.create((observer)=>{
  setInterval(()=>{
     observer.next('aaa');
  },1000)
})
```

上面实例创建的一个可观察对象，当它被观察者订阅后，它会向观察者秒发送一次 `aaa`

### Observer（观察者）
`observer`（观察者）用于接收可观察对象通知的处理器。该对象定义了一些回调函数来处理可观察对象可能发来的三种通知

| 通知类型 | 说明   |  
| --------   | -----  | 
| next | 必选。 用来处理可观察对象发送的值| 
| error  |  可选。用来处理错误通知   | 
| complete |可选。用来处理执行完毕通知   |

定义一个观察者如下所示
```typescript
bb={
  next:x=>console.log('x的值：'+x), //必写
  error:err=>console.log('当前错误：'+err), //可写，可不写
  complete:()=>console.log('完成')  //可写，可不写
}
```
上面定义了一个观察者，当观察者订阅对象后，每次观察者会将接收的值在控制台输出，其代码如下所示
```typescript
import { Observable } from 'rxjs';
....//代码块

var observable=Observable.create((observer)=>{
  setInterval(()=>{
     observer.next('aaa');
  },1000)
})

const bb={
  next:x=>console.log('x的值：'+x), //必写
  error:err=>console.log('当前错误：'+err), //可写，可不写
  complete:()=>console.log('完成')  //可写，可不写
}

observable.subscribe(dd); //观察者 dd 订阅 observable
```

在浏览器控制台的输出如下图所示

![](https://bingolil.github.io/images/angular-rxjs-observer.png)

### Subscription（订阅）
`Subscription` 的作用一般用来取消订阅，取消订阅的时间节点根据需求设置，其代码如下所示
```typescript
import { of, Subscription } from 'rxjs';
....//代码块

oob:Subscription;
....//代码块

const bb=of(1,2,3); //rxjs 静态方法创建可观察对象

const dd={ //观察者
 next:x=>console.log('输出的值：'+x),
 error:err=>console.log('当前错误：'+err),
 complete:()=>console.log('完成'),
}

this.oob=bb.subscribe(dd); //订阅
....//代码块

this.oob.unsubscribe(); //取消订阅
```

### 创建Observable
`Opterator` 提供了多种操作符来创建 `Observable` 对象，具体如下所示

>**of**：处理数据，字符串或数字

具体例子如下所示
```typescript
import { of } from 'rxjs';
....//代码块

const bb=of(1,2,3); //可观察对象

bb.subscribe((xx)=>{
  console.log(xx);
})
// 输出  1,2,3
```

>**from**：处理数据，将数组一个一个的发送

具体例子如下所示
```typescript
import { from } from 'rxjs';
....//代码块

const bb=from([1,2,3]);; //可观察对象

bb.subscribe((xx)=>{
  console.log(xx);
})
// 输出  1,2,3
```

>**fromEvent**：处理事件

具体例子如下所示
```typescript
import { fromEvent } from 'rxjs';
....//代码块

const node=document.querySelector('input[type=text]');

const input$=fromEvent(node,'input');

input$.subscribe((xx)=>{
  console.log(xx);
})
//输出 input 事件
```

>**empty**：返回一个空的 `Observable` 对象，若订阅该对象，它会马上返回 `complete` 信息

具体例子如下所示
```typescript
import { empty } from 'rxjs';
....//代码块

const cc=empty();
cc.subscribe({
  next:xx=>console.log(xx),
  error:err=>console.log(err),
  complete:()=>console.log("执行完成")
})
//输出  执行完成
```

>**nerver**：返回一个无穷的 `Observable`，订阅它后，什么都不会发生

具体例子如下所示
```typescript
import { never } from 'rxjs';
....//代码块

const dd=never();
dd.subscribe(xx=>{
  console.log(xx);
})
//无输出
```

>**throwError**：抛出一个错误的 `Observable`

具体例子如下所示
```typescript
import { throwError } from 'rxjs';
....//代码块

const ee=throwError('xx')
ee.subscribe({
  next:xx=>console.log(xx),
  error:err=>console.log("错误为："+err),
  complete:()=>console.log("执行完成")
})
//输出  错误为：xx
```

>**interval**：接收一个数值为参数，该数值代表时间间隔。每到一定时间，其会推送一个递增的数字，从 0 开始

具体例子如下所示
```typescript
import { throwError } from 'rxjs';
....//代码块

const ff=interval(1000);
ff.subscribe(xx=>{
  console.log(xx);
})
// 输出  0,1,2,3,4,5......
```

>**timer**：接收两个参数，第一个参数代表第一次推送需要的时间，第二个参数代表推送第一次推送后，推送其它值的间隔时间

具体例子如下所示
```typescript
import { timer } from 'rxjs';
....//代码块

const hh=timer(1000,2000);
hh.subscribe(xx=>{
  console.log(xx);
})
// 输出  1（第一秒推送）,2（第三秒推送）,3（第五秒推送）....
```


## Subject
### Subject 和 Observable
`Subject` 是一种特殊的 `Observable`（可观察对象），它既是 `Observable` 对象，又是 `Observer` 对象；它允许将值多播给多个订阅者，而普通的 `Observable` 是单播的（每个已订阅的观察者都拥有 `Observable` 的独立执行）。

`Subject` 是观察者模式的实现，它继承了 `Obsevalbe`。当有观察者订阅它时，它将订阅者添加到观察者列表中，它每次接收到新值时，它会遍历观察者列表，调用其 `next` 方法，将值推送出去。`Subject` 用法如下
```typescript
import { Subject } from 'rxjs';
....//代码块

_oob=new Subject<any>();
....//代码块

this._oob.subscribe((xx)=>{
  console.log('observerA订阅到的值为：'+xx);
})
this._oob.subscribe((xx)=>{
  console.log('observerB订阅到的值为：'+xx);
})

this._oob.next(1);
this._oob.next(2);
```

上述代码在浏览控制台中输出如下图所示
![](https://bingolil.github.io/images/angular-rxjs-subject.png)

### BehaviorSubject 
有时我们希望 `Subject` 能保存当前的最新状态，而不是单纯的对象推送，即每新增加一个观察者，当前 `Subuject` 能够立即向新增加的观察者推送最新的值，而不是没有响应。具体代码如下所示
```typescript
import { Subject } from 'rxjs';
....//代码块

_oob=new Subject<any>();
....//代码块

this._oob.subscribe((xx)=>{
  console.log('observerA订阅到的值为：'+xx);
})

this._oob.next(1);//发送值
this._oob.next(2);//发送值

setTimeout(()=>{
  this._oob.subscribe((xx)=>{
    console.log('observerB订阅到的值为：'+xx);
  })
},1000)
```

在浏览器控制台输出结果如下图所示

![](https://bingolil.github.io/images/angular-rxjs-be1.png)

从上图中，可以看到 `observerB` 没有订阅到数据，因为 `obseverB` 订阅 `_oob` 后，`_oob` 没有再调用 `next` 推送数据。很多时候我们希望 `Subject` 能保存当前的最新状态，当新增订阅者时，该订阅者可以获取当前 `Subject` 的最新状态。完成该功能，需要使用 `BehaviorSubject` 对象，其代码如下所示
```typescript
import { BehaviorSubject } from 'rxjs';
....//代码块

_oob=new BehaviorSubject<any>(0); //设定初始值
....//代码块

this._oob.subscribe((xx)=>{
  console.log('observerA订阅到的值为：'+xx);
})

this._oob.next(1);//发送值
this._oob.next(2);//发送值

setTimeout(()=>{
  this._oob.subscribe((xx)=>{
    console.log('observerB订阅到的值为：'+xx);
  })
},1000)
```

浏览器控制台中输出如下图所示

![](https://bingolil.github.io/images/angular-rxjs-be2.png)

从上图中可以看到，`observerB` 获取到了 `_obb` 对象的最新状态，并且最开始时，`Subject` 对象推送了一个 `0`

> **注意：**因为新增订阅者需要获取到当前 `Subject` 最新的状态，所以在实例化 `BehaviorSubject` 需要一个初始状态

### ReplaySubject
有时我们希望 `Subject` 新增订阅者后，能向新增的订阅者发送 `Subjcet` 的最新的几次状态，实现该功能，需要使用到 `ReplaySubject` 对象，其代码如下所示
```typescript
import { ReplaySubject } from 'rxjs';
....//代码块

_ss=new ReplaySubject<any>(2); //2 代表的是发送最新的几次状态
....//代码块

this._ss.subscribe((xx)=>{
  console.log("observerX订阅到的值为："+xx)
})

this._ss.next(1);
this._ss.next(2);
this._ss.next(3);

setTimeout(()=>{
  this._ss.subscribe((xx)=>{
    console.log('observerY订阅到的值为：'+xx);
  })
},1000)
```

浏览器控制台中的输出如下图所示

![](https://bingolil.github.io/images/angular-rxjs-rey.png)

从上图中，可以看到 `_ss` 向1秒后新增加的订阅者发送了两次最新的状态

> **`BehaviorSubject(1)` 和 `ReplaySubject(1)` 的区别：**可能有人认为 `BehaviorSubject(1)` 等同于 `ReplaySubject(1)`，其实它们是不一样的，有很大的区别，创建 `BehaviorSubject` 对象时，其参数是对象的初始值，用于表示对象的初始状态；而 `ReplaySubject` 是事件的回放，其参数代表回放的次数

## Opterator
`Rxjs` 提供了大量的操作符来进行数据处理，常见的操作符如下所示

>**map**：从内部的 `Observable` 获取者，操作完成后返回给父级流对象

```tyepscript
import { of } from 'rxjs';
import { map } from 'rxjs/operators'
....//代码块

const aa=of(1,2,3);
aa.pipe(map(x=>{return x*x})).subscribe((xx)=>{
  console.log(xx);
}) //输出为 1，4，9
```

>**debounceTime**：主要用于防抖操作，减少订阅次数

```tyepscript
import { of } from 'rxjs';
import { debounceTime } from 'rxjs/operators'
....//代码块

const aa=of(1,2,3);
aa.pipe(debounceTime(1000)).subscribe((xx)=>{
  console.log(xx);
}) //1秒后输出  3
```

>**filter**：用于过滤数据

```tyepscript
import { of } from 'rxjs';
import { filter } from 'rxjs/operators'
....//代码块

const aa=of(1,2,3,4);
aa.pipe(filter(xx=>xx%2==0)).subscribe((xx)=>{
  console.log(xx);
}) //输出  2，4
```

>**reduce**

```tyepscript
import { of } from 'rxjs';
import { reduce } from 'rxjs/operators'
....//代码块

const aa=of(1,2,3);
aa.pipe(reduce(x,y)=>{return x+y})).subscribe((xx)=>{
  console.log(xx);
}) //输出  6
```