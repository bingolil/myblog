title: Javascript var,let,const区别
categories: Javascript
date: 2022-05-02
tags: [Javascript,var,let,const区别]
description: 在Javascript中，作用域有全局作用域和函数作用域的概念，在es6（即ES2015）中引入了块级作用域，块级作用域由{ }包括，其中if语句和for语句属于块级作用域。赋值的关键字有var（es6之前），let，const，function，class。本次主要讲解var，let，const的区别
---
## var
### 基础
在函数作用域中使用var声明一个变量，则该变量属于当前函数作用域，如果声明在任何函数外的顶层作用语，则变量属于全局变量，如下所示
```javascript
var value1 = 'tom'; // 全局变量
function changeVal() {
  var value1 = 18; // 局部变量
  console.log(value1);
}
changeVal(); // 输出18
console.log(value1); // 输出tom
```

在函数作用域中省略var声明一个变量时，那么该变量变成全局变量，若全局中存在该变量，则更新全局变量的值，如下所示
```javascript
var value1 = 'tom';
function changeVal() {
  value1 = 'jack';
  value2 = 18;
}
changeVal();
console.log(value1); // jack
console.log(value2); // 18
```
var在块级作用域中，声明的变量是全局变量，即在块级作用域外也可以访问，如下所示
```javascript
for (var i = 0; i < 5; i++) {
  var sum = 0;
  sum += i;
}
console.log(sum); // 4
```
**注意**：var声明的变量存在提升

### 提升
提升是指不管var声明的变量在作用域的那个位置，该变量都属于整个作用域，在当前作用域的各个地方都可以访问到，如下所示
```javascript
console.log(value); // undefined
var value = 'jack';
```

```javascript
var value1 = 'tom';
function changeVal() {
  console.log(value1);
  if (false) {
    var value1 = 'lucy';
  }
}
changeVal(); // 输出undefined
```
在上述代码块中，`changeVal`函数中的if条件不管是真还是假，其输出都是undefined

## let和const
### let
let的声明存在以下特征
> * let声明的变量具有块级作用域的特征
> * let声明的变量在同一作用域下不能重复声明
> * let声明的变量不存在提升，即let声明存在暂时性死区

如下几个例子所示
```javascript
// 1，let声明的变量具有块级作用域的特征
{
  let value = 'tom';
  console.log(value); // tom
}
console.log(value); // 报错：Uncaught ReferenceError: value is not defined
```

```javascript
// 2，let声明的变量在同一作用域下不能重复声明
function setValue() {
  let value = 'tom';
  let value = 17; // 编译报错：Identifier 'value' has already been declared
}
```

```javascript
// 3，let声明的变量不存在提升，即let声明存在暂时性死区
console.log(value); // 报错：Cannot access 'value' before initialization
let value = 'jack';
```

### const
const除了具有上述let的特点外，还有以下特征
> * const用于定义常量，不可更改
> * const定义的常量必须初始化

如下几个例子所示
```javascript
// 1，const用于定义常量，不可更改
const value = 'tom';
value = 'lucy'; // 报错：Assignment to constant variable.
```

```javascript
// 2，const定义的常量必须初始化
const value;
console.log(value); // 报错：Missing initializer in const declaration 
```
## var和let的区别
var和let的区别，其例子如下所示
```javascript
for (var j = 0; j < 5; j++) {
  setTimeout(() => {
    console.log(j); // 5，5，5，5，5
  }, 100);
}
```
上述代码中，由于setTimeout是一个异步且let定义的j是一个全局变量，j的值会覆盖已有的值，所以其输出为 `5，5，5，5，5`。

```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i); // 0，1，2，3，4
  }, 100);
}
```
上述代码中，for循环中采用了let声明循环变量i，所以每一个循环都有自己的作用域，其值不会被覆盖，所以其值输出为 `0，1，2，3，4`.
## 总结
> * var可重复声明变量，let和const在同一作用域不允许重复声明
> * var存在提升现象，let和const不存在
> * var和let定义的变量可修改，const不可以
> * const声明的值时必须初始化，let和var不需要
