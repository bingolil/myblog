title:  Angular学习-7-表单2
categories: Angular
date: 2019-01-21
tags: [Angular,Angular表单]
description: Angular 提供了两种不同的方法来通过表单处理用户输入：响应式表单和模板驱动表单。 两者都从视图中捕获用户输入事件、验证用户输入、创建表单模型、修改数据模型，并提供跟踪这些更改的途径。
---
## 模板驱动表单
模板驱动表单专注于简单的场景（也可以应用于大型表单）

### [(ngModel)]

在模板驱动表单中，通过 `[(ngModel)]` 将数据模型和视图相关联起来，其为双向绑定（值可以从模型流到视图，也可以从视图流到模型）。其代码如下所示

Ts
```typescript
....//代码块

//定义绑定的变量
public charter={
  name:'',  //姓名
  age:18,   //年龄默认 18
  power:'', //超能力
  desc:''   //描述
}

private conCha(){
  console.log(this.charter)
}
```

Html
```HTML
....//代码块
<form>
  姓名：<input type="text" name="name" [(ngModel)]="charter.name"><hr>
  年龄：><input type="number" name="age" [(ngModel)]="charter.age"><hr>
  超能力：<input type="text" name="power" [(ngModel)]="charter.power"><hr>
  描述：<input type="text" name="desc" [(ngModel)]="charter.desc"><hr>
  <button type="button" (click)="conCha()">控制台输出信息</button>
</form>
```

> 在标签上使用 `[(ngModel)]` 时，需要在该标签加上 `name` 属性（其值在页面中唯一），建议 `name` 属性的值和 `[(ngModel)]` 绑定的值相同。在响应式表单中可以不用加 `name` 属性

运行代码（上述 `Html` 代码已移除样式）后，在页面中改变各个输入框的，然后点击 `控制台输出信息` 按钮，可以看到，在控制台中输出组件的 `charter` 值，其值的属性值为页面中对应输入框输入的值。 

### ngForm 和 ngNoForm
`ngForm` 是一个指令，若开发者导入了 `FormsModule` 模块，该指令会在该模块下所有的 `form` 标签上生效（默认添加，`ngForm` 指令不会被自动添加到响应式表单中）。`ngForm` 指令创建一个顶级的 `FormGroup`，并把这个 `FormGroup` 绑定到一个表单上，以跟踪表单的状态和聚和值。

> 当开发者不希望 `Angular` 接管表单时，可以在 `form` 标签上添加 `ngNoForm` 指令；当标签为 `div` 时，希望 `div` 接管表单，可以在 `div` 上添加 `ngForm` 指令

其用法如下所示

Ts
```typescript
....//代码块

conNgForm(f){
  console.log(f);
}
```

Html
```HTML
<form #cha="ngForm"><!-- cha 为ngForm的引用-->
  <!-- 代码块，绑定姓名年龄等 -->
  <button type="button" (click)="conNgForm(cha)">输出ngForm</button>
</form>
```

运行该代码时，点击页面中的 `输出ngForm` 按钮，可以在控制台看到 `ngForm` 指令的值，其结果如下图所示

![ngForm的值](https://bingolil.github.io/images/angular-ngForm.png)

### FormArry 的替代方案
当子控件的数量不确定时，在响应式表单中，其解决方案是由 `FormArry` 管理来子控件。在模板驱动表单中，其解决方案代码如下所示

Ts
```typescript
....//代码块

public charter={
  name:'',  //姓名
  age:18,   //年龄默认 18
  power:'', //超能力
  desc:'',  //描述
  favs:[]   //爱好（在已有代码中新增的属性）
}
//添加新 input text 框以输入新爱好
private addF(){
  this.charter.favs.push('');
}
//防止失去焦点
indexTracker(index: number, value: any) {
  return index;
}
//移除当前的爱好
private removeF(i){
  this.charter.favs.splice(i,1);
}
....//代码块
```

Html
```HTML
<form #cha="ngForm">
  <!--代码块，绑定姓名年龄等-->
  爱好：
  <p *ngFor="let item of charter.favs;let i=index;trackBy:indexTracker;">
    <input type="text" name="fav{{i}}" [(ngModel)]="charter.favs[i]">
    <button type="button" (click)="removeF(i)">移除</button>
  </p>
      <!--type一定要有-->
  <button type="button" (click)="addF()">添加爱好</button>
  <p>表单的值：{{charter | json}}</p>
</form>
<!--代码块-->
```

在浏览器中运行代码后，其页面效果如下所示

![](https://bingolil.github.io/images/angular-formarray2.png)

> **注意1：**在模板驱动表单中，使用上述方法绑定数组时，虽然循环表达式 `*ngFor="let item of charter.favs;let i=index;trackBy:indexTracker;"` 中存在item，但其在双向绑定中并没有被使用，在 `[(ngModel)]` 的双向绑定中使用的是 `charter.favs[i]` 

> **注意2：**在 `ngFor` 的循环中使用 `trackBy`，其值 `indexTracker` 在 `Ts` 代码中被定义。它的作用是防止表单中绑定数组的 `input` 框在输入时失去焦点，即在页面中没有 `indexTracker` 时，表单中的爱好 `input` 框的值改变时，当前的表单会失去焦点

> **注意3：**在 `Html` 代码中，按钮 `button` 需要加上 `type` 类型，如 `button`，`submit`等，若不加上，该按钮会被表单默认为 `submit` 类型。即当 `input` 框获取焦点后（不管有没有数据），按键盘上的回车键会触发表单中 `submit` 提交事件


## 模板表单验证

### Angular 提供的验证器
在使用模板驱动表单时，`Angular` 提供了许多的验证器，其验证器直接在组件 `UI` 中使用，其代码如下所示

Html
```HTML
<form #cha="ngForm">
姓名：<input type="text" name="name" [(ngModel)]="charter.name" required>
<!--代码块，绑定其它的属性-->
<p>表单的状态：{{cha.valid}}</p>
</form>
```
当姓名 `input` 框没有值时时，页面中表单的状态为 `false`，一旦有值时，表单的状态为 `true`。

> 当 `input` 为 `number` 类型时，`min="10"` 的验证绑定不到表单中，在该框中直接输入一个比 `10` 小的数，表单的状态不改变

在模板驱动表单中，`Angular` 能用的验证如下所示

> `required`：表示该控件必须要有值，一般用于 `input` 的 `number`类型、`text`类型、`select`类型等，用法如上所示

> `minLength`：表示当前输入的字符最小长度，一般用于 `input` 的 `text` 类型

用法：`<input type="text" name="name" [(ngModel)]="charter.name" minlength="2"`，表示 `name` 必须不小于两个字符

> `maxLength`：表示当前输入的字符最大长度，一般用于 `input` 的 `text` 类型

用法：`<input type="text" name="name" [(ngModel)]="charter.name" minlength="10"`，表示 `name` 必须不大于10个字符

> `pattern`：验证当前输入是否符合某种正则表达式，一般用于 `input` 的 `text` 类型

用法：`<input type="text" name="power" [(ngModel)]="charter.power" pattern="[A-z]*">`，表示当前的 `input` 输入框的输入必须为英文字母（不管大小写）

> `email`：表示当前输入的格式必须符合邮箱的格式，一般用于 `input` 的 `email` 类型

用法：`<input type="email" email name="email" [(ngModel)]="charter.email">`，表示当前 `input` 输入框的输入必须为邮箱格式


> **注意：** 在模板驱动表单中，当 `input` 为 `number` 类型时，没有验证其大小的验证器（在响应式表单中为 `Validators.min()` 和 `Validators.max()`）

### 自定义验证器
在模板驱动表单中，自定义验证器的实质是一个自定义指令，自定义一个其代码如下所示

指令Ts
```typescript
import { Directive, forwardRef, Input } from '@angular/core';
import { NG_VALIDATORS, Validator, AbstractControl } from '@angular/forms'

@Directive({ //装饰器定义指令
  selector: '[appZdy]',
  providers:[
    {provide:NG_VALIDATORS,useExisting:forwardRef(()=>ZdyValidator),
    multi:true
    } 
  ] 
})
export class ZdyValidator implements Validator{
  @Input() appZdy:string; //输入

  validate(c:AbstractControl):{[key:string]:any;}{
    let value:string=c.value || ''; //c代表当前控件
    if(value.startsWith(this.appZdy)){
      return {mobile:{
        msg:`手机号不能为${this.appZdy}开头`,
        actualValue:value}
      }
    }
    return null;
  }
 }
```

在模板驱动表单中使用自定义验证器
```HTML
<!--代码块-->
<span>手机：</span>
<input type="text" required appZdy="133" name="phone" [(ngModel)]="charter.phone">
<!--代码块-->
```

在浏览器中，运行该代码时，若在手机 `input` 框输入133为开头的字符串，该控件的状态会为 `INVALID`

### 验证时错误提示
在表单中，若用户输入不符合要求，需要提醒用户其出错在什么地方。在模板驱动表单中，提供了 `ngModel` 指令来实现该效果，其代码如下所示

Html
```HTML
<!--代码块-->
<span>手机：</span>
<input type="text" required appZdy="133" name="phone" [(ngModel)]="charter.phone" #phone="ngModel">
<div *ngIf="phone.invalid && (phone.dirty || phone.touched)">
  <p>手机号必填</p>
  <p *ngIf="phone.errors.mobile">{{phone.errors.mobile.msg}}</p>
</div>
<!--代码块-->
```

在上面的 `Html` 代码中，`input` 标签中的 `#phone="ngModel"` 代表当前控件赋值给 `phone` 变量。即在页面中，`phone` 变量包含这个控件的一切，可以访问该控件的 `value` 或 `status`。

> **dirty 和 touched：**在初始化页面后，开发者一般不希望将错误提示在用户还没有输入时就展示出来，这会降低用户体验，而 `dirty` 和 `touched` 可以解决该该问题。改变控件的值时，控件的 `dirty`（脏）状态发生改变，当控件失去焦点时，会改变控件的 `touched`（碰过）状态。

### 异步验证
在模板驱动表单中，异步验证的验证器是一个自定义指令，验证用户名唯一的功能代码如下所示

asyc-name.directive.ts
```typescript
import { Directive, forwardRef } from '@angular/core';
import { AsyncValidator, NG_ASYNC_VALIDATORS, AbstractControl, ValidationErrors } from '@angular/forms'
import { map, catchError } from 'rxjs/operators';
import { Observable } from 'rxjs';
import { AsycService } from '../asyc.service';

@Directive({
  selector: '[appAsycName]',
  providers:[
    {provide:NG_ASYNC_VALIDATORS,useExisting:forwardRef(()=>AsycNameValidator),
    multi:true
    }
  ]
})
export class AsycNameValidator implements AsyncValidator {

  constructor(private asycService:AsycService) { }

  validate(ctrl:AbstractControl):Promise<ValidationErrors | null> | Observable<ValidationErrors | null>{
    return this.asycService._can_use(ctrl.value).pipe(
      map(xx=>(xx?{can_use:false}:null)),
      catchError(()=>null)
    )
  }
}

```

在模板驱动表单中使用该异步验证器

Html

```HTML
<!--代码块-->
姓名：<input type="text" name="name" [(ngModel)]="charter.name" [ngModelOptions]="{ updateOn: 'blur' }" appAsycName>
<!--代码块-->
```

## 性能影响
在默认情况下，表单的值一旦发生改变，`Angular` 会执行所有的验证器。对于同步验证器，其对项目没有明显的影响，不过，异步验证一般都会发送 `http` 请求来对控件进行验证，每次按键时，触发验证，若按键的速度过快，程序会在短时间来发送大量的 `http` 请求，这会对项目的性能造成明显的影响，降低用户体验，应该避免这种情况出现。

在异步验证用户名时，在 `input` 框中，可以将 `updateOn` 的值从 `change` 改为 `blur` 来推迟异步验证的时机，即当控件失去焦点时才进行异步验证。其代码如下所示

**模板驱动表单**
Html
```HTML
<input type="text" name="name" [(ngModel)]="charter.name" ngModelOptions="{updateOn:'blur'}">
```

**响应式表单**

Ts
```typescript
new FormControl('',{updateOn:'blur'})
```






