title:  Angular学习-6-表单1
categories: Angular
date: 2019-01-07
tags: [Angular,Angular表单]
---
在 `Angular` 中，提供了两种表单来处理用户的输入，`模板驱动表单` 和 `响应式表单`。
## 建立表单模型
### 响应式表单

> **前提条件**：在需要使用响应式表单的组件所在的模块中引入 `ReactiveFormsModule`

最基本的响应式表单

Html
```HTML
<input type="text" class="form-control"  [formControl]="nameControl">
```
Ts
```typescript
import { FormControl } from '@angular/forms';
....//代码块

nameControl = new FormControl('jack'); //jack是初始值，也可以为空字符串('')
```

此处为单个控件，使用响应式表单时需要引入 `FormControl`。

**注：** `Html` 模板中的 `class="form-control"` 代码只和样式有关，和响应式表单无关。


### 模板驱动表单
>**前提**：在需要使用响应式表单的组件的对应的模块中引入 `FormsModule`

最基本的模板驱动表单

Html
```HTML
<input type="text" class="form-control" name="favorite"  [(ngModel)]="color">
```
Ts
```typescript
....//代码块
color:string;
```

## 响应式表单
响应式表单适用于构建大型表单，且其跨代码的重用性强
### 控件分组
响应式表单只是展示一个控件，会使用到 `FormControl`。而响应式表单在项目中往往适用于大型表单，即表单中不会只存在一个表单控件。而存在多个控件时，需要对表单控件进行分组。在响应式表单中，对控件进行分组，需要引入并实例化 `FormGroup`，其代码如下所示。

Ts
```typescript
import { FormControl, FormGroup } from '@angular/forms';
....//代码块
Charter=new FormGroup({
  name:new FormControl(''),
  age:new FormControl()
})
```

Html
```HTML
<form [formGroup]="Charter" (ngSubmit)="consoleSubmit()">
  名称：<input type="text" name="name1"  formControlName="name"><br>
  年龄<input type="number" name="age"  formControlName="age">
  <div class="mt-2">
    <button type="submit" class="btn btn-sm">提交表单</button>
   </div>
</form>
```
`FormControl` 能让开发者管理单个控件，当存在控件分组时，`FromGroup` 实例能跟踪一组 `FormControl` 的状态。

在 `Ts` 代码中实例化 `FromGroup` 后，在 `Html` 中，将 `Charter` 这个 `FormGroup` 通过 `FormGroup` 指令绑定到 `form` 元素上。由 `formControlName` 指令提供的 `formControlName` 属性将每个输入框和 `FormGroup` 中的定义 `FormControl` 关联起来。

### 嵌套表单组
在响应式表单中，建立更加复杂的表单时，对表单信息进行分组，将相关联的消息放到一个组，将该组又放到一个更大的组中，即嵌套表单组。使用嵌套分组能让开发者更加易于实现和管理表单。

在填写个人信息表单时，地址是一个很常见的信息。在响应式表单中，地址是对表单进行分组的一个绝佳范例。其代码如下所示。

Ts
```typescript
...//代码块
Charter=new FormGroup({
  name:new FormControl(''),
  age:new FormControl()，
  address:new FormGroup({
    province:new FormControl(''),
    city:new FormControl(''),
    county:new FormControl(''),
    zip:new FormControl('')
  })
})
...//代码块
```
Html
```HTML
<form [formGroup]="Charter" (ngSubmit)="consoleSubmit()">
  名称：<input type="text" name="name" formControlName="name"><br>
  年龄：<input type="number" name="age" formControlName="age">
  <div formGroupName="address" class="mt-2">
    <div>
      省：<input type="text" name="province" formControlName="province">
      市：<input type="text" name="city" formControlName="city">
      县：<input type="text" name="county" formControlName="county">
    </div>
    具体地址：<input type="text" name="zip" formControlName="zip">
  </div>
  <button type="submit" class="btn btn-success">提交表单</button>
</form>
```
在上面代码中，`address` 为一个 `FormGroup`，在 `Html` 中通过 `FormGroupName` 指令将 `address` 这个 `FormGroup` 与视图关联起来。

### FormBuilder 生成表单控件
在上面的 `Ts` 代码中实例化表单控件时，只有几个属性，一会是 `new FormGroup()`，一会是 `new FormControl()`，代码过于笨重，而 `Angular` 提供了 `FormBuilder` 服务来快捷生成表单。其代码如下所示

Ts
```typescript
import { FormBuilder } from '@angular/forms';
....//代码块

Charter=this.fb.group({
  name:[''],
  age:[null],
  address:this.fb.group({
    province:[''],
    city:[''],
    county:[''],
    zip:['']
  })
})

constructor(private fb:FormBuilder) { }
```
使用 `FormBuilder` 创建表单和使用 `new FormGroup()` 和 `ngw FormControl()` 创建表单结果是一样的，但其代码跟加的简洁。

> `FormBuilder` 服务有三个方法：`control()`、`group()` 和 `array()`

### FormArray 管理动态表单
`FormArry` 是 `FormGroup` 的另一种选择，当开发者事先不知道子控件的具体数量是多少， `FormArray` 是一种很好的选择，它可以帮助开发者很好的管理匿名控件。

应用场景：当表单中存在组件变化时，如在 `Charter` (人物)表单中，添加人物的 `enable` （能力）属性，但是人物的 `enable` 能力不可能只有一个，存在多个，比如驾驶，观察，组织等，不确定有多少。而  `FormArray` 就是应用于这种场景，其代码如下所示

Ts
```typescript
import { FormArray, FormBuilder, Validators } from '@angular/forms';
....//代码块

Charter=this.fb.group({
  name:[''],
  age:[null],
  address:this.fb.group({
    province:[''],
    city:[''],
    county:['']
  }),
  // enable:this.fb.array([this.fb.control('')])  //加入初始值
  enable:this.fb.array([])
})

get enable(){
  return this.Charter.get('enable') as FormArray;
}

constructor(private fb:FormBuilder) { }

addNewEnable(){//添加新input以添加新能力，同时可以加入验证器
  this.enable.push(this.fb.control('',Validators.required))
}

reduceEnable(i){//移除能力
  this.enable.removeAt(i);
}
```

Html
```HTML
<form [formGroup]="Charter" (ngSubmit)="consoleForm()">
  姓名：<input type="text" name="name" formControlName="name">
  年龄：<input type="number" name="age" formControlName="age">
  <div class="d-flex mt-2" formGroupName="address">
    <span>地址：</span>
    <div class="clearfix">
      省<input type="text" name="province" formControlName="province"><br>
      市<input type="text" name="city" formControlName="city"><br>
      县<input type="text" name="county" formControlName="county">
    </div>
  </div>
  <div class="mt-2" formArrayName="enable">
    <span>能力：</span>
    <div *ngFor="let en of enable.controls;let i=index">
      <p class="d-flex">
        <input type="text" [formControlName]="i">
        <button type="button" (click)="reduceEnable(i)">移除</button>
      </p>
    </div>
    <button type="button" (click)="addNewEnable()">添加新能力</button>
  </div>
  <p class="small text-muted">表单的值：{{Charter.value|json}}</p>
  <button [disabled]="!Charter.valid" type="submit">提交表单</button>
</form>
```

其在浏览器页面中的效果（`Html` 代码中已移除样式类）如下所示

![FormArray管理动态表单](https://bingolil.github.io/images/angular-formarray.png)

>  **注意**：在 `Html` 中，关于响应式表单的指令一律小写开头，如 `formGroup`，`formControlName`等，不能写成 `FormGroup`，`FormControlName`等，若大写开头会报错或绑定不成功

在上面的 `Ts` 代码中，使用了`FormArray` 的 `push` 和 `removeAt()` 方法，`FormArray` 的常用方法如下表所示

| 方法        | 参数   |  描述  |
| --------   | ----- |---- |
| at()     | (index:number) |  获取指定位置的 FormControl |
| push() | (control:AbstractControl)|在数组末尾添加一个 FromControl   |
| insert()  | (index:number,control:AbstractControl) |在指定位置添加一个 FromControl|
| removeAt()  | (index:number) |移除指定位置的 FromControl|
| setControl()  | (index:number,control:AbstractControl) |替换指定位置的 FromControl|
|setValue | (value: any[]) |  设置 FormArray 中各个控件的值，数据结构不匹配时会报错（建议到 [`官网`](https://www.angular.cn/api/forms/FormArray#setvalue) 查看） |
|patchValue | (value: any[]) |  设置 FormArray 中各个控件的值，数据结构不匹配时不会会报错（建议到 [`官网`](https://www.angular.cn/api/forms/FormArray#patchvalue) 查看）|
| resset()  | (value:any[]) |重置 FormArray 的值，并设置各个控件标记为 untouched 和 pristine 都为true|
| getRawValue()  | () |这个 FormArray 的聚合值，包括已禁用的控件|

## 响应式表单验证
表单验证是表单中最重要的部分之一，对于一个表单，有一个完善的表单验证能有效的降低后台开发者的工作时间。并且在提交表单时，减少项目的报错（提交的表单数据不符合需求），提高用户体验。

### `Angular` 提供的验证器函数

对于响应式表单，`Angular` 提供了一组常用的验证器函数，这些函数接收一个表单控件，验证并根据验证的结果返回一个空值（`null`，其代表验证通过）或错误对象。当然，`HTML5` 存在一些内置的属性，用来进行原生的验证，如 `required`，`minLength`，`maxLength`，`pattern` 等，其用法如下所示

Html
```HTML
<input type="text" name="name" formControlName="name" required>
```

使用 `Angular` 提供的验证器函数时，需要在组件中导入 `Validators` 类，其基本用法如下所示。

Ts
```typescript
import { FormBuilder, Validators } from '@angular/forms';
....//代码块
Charter=this.fb.group({
  name:['',Validators.required], //使用验证
  age:[null],
  address:this.fb.group({
    province:[''],
    city:[''],
    county:[''],
    zip:['']
  })
})
```

使用 `Angular` 自带的 `Validators` 验证器，其提供的验证方案如下所示。

> `Validators.min()`，要求控件大于等于某个值，一般用于验证数字

用法：`const age=new FormControl(18,Validators.min(2))`

> `Validators.max()`，要求控件小于等于某个值，一般用于验证数字

用法：`const age=new FormControl(0,Validators.max(2))`

> `Validators.required`，要求控件非空，一般用于验证 `input` 输入或 `select` 框

用法：`const name=new FormControl('',Validators.required`)

> `Validators.requiredTrue`，要求控件值为真，一般用于验证 `checkbox`

用法：`const control=new FormControl('',Validators.requiredTrue)`

> `Validators.email`，要求控件的值为 `email` 格式，一般用于验证输入为邮箱格式

用法：`const owebEmail=new FormControl('',Validators.email)`

> `Validators.minLength()`，要求控件值的字符长度大于等于某个指定的长度，一般用于验证 `input` 的 `text` 输入类型

用法：`const name=new FormControl('',Validators.minLength(8))`

> `Validators.maxLength()`，要求控件值的字符长度小于等于某个指定的长度，一般用于验证 `input` 的 `text` 输入类型

用法：`const name=new FormControl('',Validators.maxLength(8))`

> `Validators.pattern()`，要求控件的值符合某种规则（正则表达式），一般用于验证 `input` 的 `text` 输入类型，判断其输入是否符合某种正则表达式

用法：`const name=new FormControl('',Validators.pattern('[a-zA-Z]*'))`，验证 `name` 控件的值只包含字母或为空格

> `Validators.nullValidator`，该验证器什么也不做

用法：`const name=new FormControl('',Validators.nullValidator)`

> `Validators.compose()`，将多个验证器合并成一个

用法：`const name=new FormControl('',Validators.compose([Validators.required,Validators.pattern('[a-zA-Z]*')]))`，验证 `name` 控件必须要有值且只包含字母

存在多个验证器时的另一种写法：`const name=new FormControl('',[Validators.required,Validators.pattern('[a-zA-Z]*')])`，直接去掉 `Validators.compose()`

> `Validators.asyncCompose()`，将多个异步验证器合并成一个

### 自定义验证器函数

虽然 `Angular` 提供丰富的内置验证器函数，但是不能保证这些内置验证器函数能用于所有的表单，这时，就需要开发者创建自定义验证器。

假如需要定义一个 `input` 的 `text` 框，禁止在框中出现 `jack` 的字符。其验证器代码如下所示

forbidden.directive.ts
```typescript
export function forbiddenValitors(nameRe:RegExp):ValidatorFn{
  return (control:AbstractControl):{[key:string]:any} | null =>{
    const forbidden=nameRe.test(control.value);
    return forbidden?{'forbiddenName':{value:control.value}}:null;
  }
}
```

在响应式表单中使用自定义验证器

Ts
```typescript
import { forbiddenValitors } from '../forbidden.directive'
....//代码块

Charter=this.fb.group({
  name:['',[forbiddenValitors(/jack/i)]],
  age:[null],
  address:this.fb.group({
    province:[''],
    city:[''],
    county:['']
  })
 }）
```

使用了自定义验证器后，在表单的 `name` 控制器中输入 `jack` 时，表单为 `invalid` 状态。

### 验证时错误提示
在表单中，用户的输入不符合要求时，表单一般都会提示错误（提高用户体验和告诉用户错在什么地方）。响应式表单中也可以实现该功能。代码如下所示

Ts
```typescript
....//代码块

Charter=this.fb.group({ //加入自定义验证器
  name:['',[Validators.required,Validators.minLength(4),forbiddenValitor(/jack/i)]],
  ....//代码块

get name(){ //必须，页面中需要 name
  return this.Charter.get('name')
}
```

Html
```HTML
 姓名：<input type="text" name="name" formControlName="name">
<p *ngIf="name.invalid && (name.dirty && name.touched)">
  <span *ngIf="name.errors.required">姓名是必填项</span>
  <span *ngIf="name.errors.minlength">姓名字符必须大于4</span>
  <span  *ngIf="name.errors.forbiddenName">姓名不能存在jack字符</span>
<!-- 提示：此处的 forbiddenName 需要和自定义验证器中 forbiddenName 字符一致 -->
</p>
```

### 异步验证
