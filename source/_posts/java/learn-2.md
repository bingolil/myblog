title: Java入门-2-数据类型
categories: Java
date: 2019-08-11
tags: [Java]
---
## 二进制基础
二进制是计算机唯一识别的语言，是学习计算机技术必须要了解的知识
### 进制转换
二进制的相关概念

|进制|基本数字 |进位规则|
|-----| -----  |----  |
|二进制  |2个：0 ~ 1   |二进一  |
| 十进制 | 10个：0 ~ 9|十进一  |


> 二进制转十进制：采用科学计数法，按权展开

设置一个二进制数为 10101011，将其转化为10进制数如下所示

![](https://bingolil.github.io/images/2to10.png)

> 十进制转二进制：采用短除 2

设置一个十进制数为 87，将 87 转化位二进制数如下所示

![](https://bingolil.github.io/images/10to2.png)

将余数从下往上组合起来就是 87 的二进制数：1010111

常用进制的换算

|十进制| 二进制|八进制|十六进制|十进制| 二进制|八进制|十六进制| 
|-----| -----  |----  |----  |-----| -----  |----  |---- |
| 0 | 0000 | 0 | 0 | 8 | 1000 | 11 | 8 |
| 1 | 0001 | 1 | 1 | 9 | 1001 | 7 | 9 |
| 2 | 0010 | 2 | 2 | 10 | 1010 | 7 | A |
| 3 | 0011 | 3 | 3 | 11 | 1011 | 7 | B |
| 4 | 0100 | 4 | 4 | 12 | 1100 | 14 | C |
| 5 | 0101 | 5 | 5 | 13 | 1101 | 15 | D |
| 6 | 0110 | 6 | 6 | 14 | 1110 | 16 | E |
| 7 | 0111 | 7 | 7 | 15 | 1111 | 17 | F |



### 位运算
二进制位运算

| 运算符 | 运算   | 实例   | 
| --------   | -----  |---|
| '&' | 与运算 | 5 & 7 = 5 |
| 'l' | 或运算 | 5 l 7 = 7 |
| '^' | 异或运算 | 5 ^ 7 = 2 |


> 与运算 &

与运算是二进制时，对应的两位数全为1，结果才为1，如下所示

5 = 0 1 0 1
7 = 0 1 1 1
5 & 7 = 0 1 0 1 = 5

> 或运算 |

或运算是二进制时，对应的两位数至少有一个为 1 时，结果为 1，如下所示

5 = 0 1 0 1
7 = 0 1 1 1
5 | 7 = 0 1 1 1 = 7

> 异或运算 ^

异或运算是二进制时，对应的两位数不同时为1，相同时为 0，如下所示

5 = 0 1 0 1
7 = 0 1 1 1
5 ^ 7 = 0 0 1 0 = 2

### JDK 内置进制转换

| 描述 | 方法   |
| --------   | -----  |
| 十进制转十六进制 | Integer.toHexString(int i) |
| 十进制转八进制 | Integer.toOctalString(int i) |
| 十进制转二进制 | Integer.toBinaryString(int i) |
| 十六进制转十进制 | Integer.valueOf('FF5F',16).toString() |
| 十六进制转八进制 | Integer.valueOf('543',8).toString() |
| 十六进制转二进制 | Integer.valueOf('100010',2).toString() |


## 数据类型
`Java` 中的数据类型分为两类：

* 值类型（又被称为基础数据类型，内置类型）
* 引用类型（除值类型以外都是引用类型，包括 `string`，自定义类等）

### 值类型

| 数据类型 | 表示   |字节|包装类|备注|
| --------   | -----  |-----|-----|--|
| 整数型 | byte | 8bit| Byte ||
| 整数型 | short | 16bit| Short| |
| 整数型 | int | 32bit| Interger| |
| 整数型 | long | 64bit| Long |赋值时一般在数字后夹l或L|
| 浮点型 | float | 32bit| Float |直接赋值时必须在数字后加f或F|
| 浮点型 | double | 64bit| Double |直接赋值时必须在数字后加d或D|
| 字符型 | char | 16bit| Character |存储unicode码，单引号赋值|
| 布尔型 | boolean | 1bit| Boolean |只有true和false两个值|

### 值类型和引用类型的区别

1. 概念上的区别：基本类型变量名指向的是具体的数值；引用类型变量名指向的是存储数据对象的内存地址

2. 内存上的区别：基本类型变量名声明之后，`Java` 会立即给它分配内存空间；引用类型以特殊的方式指向对象实体，声明时只是存储了一个内存地址

3. 使用方面的区别：基本类型使用时需要具体赋值，判断时使用符号 `==`；引用类型使用时可以赋 `null`，判断时使用 `equals`

**代码演示**

```java
    int[] arr1 = new int[]{1, 2, 3, 4};
    System.out.println(arr1[3]); // 输出4
    int[] arr2 = arr1;
    arr2[3] = 3;
    System.out.println(arr1[3]); // 输出3
```

在上面的代码中，执行到 `new` 关键字时，会在堆内存中分配内存空间，将内存空间的地址赋值给 `arr1`；然后执行到 `int[] arr2 = arr1`，是将 `arr1` 的代表的内存地址赋值给 `arr2`，即 `arr1` 和 `arr2` 都指向了同一块内存地址；执行到 `arr2[3] = 3` 时，修改内存中的值，最后打印的 `arr1[3]` 则为 `3`，不是初始的  `4`。

## 基础数据类型转换
`Java` 允许编程者对已定义数据类型的数据进行类型转换，类型转换分为自动转换和强制转换。

### 自动转换
自动转换是程序在执行过程中 "悄然" 进行的转换，不需要编程者提前进行声明，一般是从位数低的类型向位数高的类型进行转换。

基础数据类型进行运算时，会将不同的数据类型转换为统一类型数据，然后再进行运算，数据类型的转换规则如下所示


| 数据类型1 | 数据类型2   |运算结果数据类型|
| --------   | -----  |-----|
| byte，short，char | int | int|
| byte，short，char，int | long | long|
| byte，short，char，int，long | float | float|
| byte，short，char，int，long，float| double | double|

>**总结：** 小数据转换为大数据，数据类型要兼容，整型和浮点型进行计算后，结果会转换为浮点型

**代码演示**
```java
    float x = 1.1f;
    int y = 2;
    System.out.println(x + y); // 输出为 3.1 float型数据
    
    long z = 20;
    System.out.println(z / x); // 输出为 18.181818 float型数据
```
在上面的代码中，先定义了浮点型 `x` 和整型 `y`，执行到 `x+y` 时，会先将 `y` 由 `int` 型自动转换为 `float` 型数据，然后进行运算，得到结果 `3.1` 。在代码的最后面，`long` 类型的数据除以 `int` 类型数据，尽管 `long` 类型精度大于 `float` 类型，但得到的结果是 `float` 型数据。

### 强制转换
强制转换必须在代码中声明，转换的顺序不受限制，使用的方式是在需要被转换的数据前面加一个 `()`，`()` 的里面是需要转换的数据类型。有的数据类型强制转换后会失去精度，有的会提高精度。

**代码演示**
```java
    float x = 12.3f;
    System.out.println((int) x); // 输出int型数据 12，失去精度

    int y = 22;
    System.out.println((float) y); // 输出float型数据 22.0，提高精度
```

## 装箱和拆箱
在本文的 [值类型](#值类型) 处展示了 `Java` 为每一种基本数据类型提供的包装类，基本数据类型算不上是一个对象，为了靠拢纯面向对象，`Java` 提供了基本数据类型的包装类型。

**应用场景**

* 调用一个含类型为 `Object` 的参数，`Object` 支持任意类型（所有类的父类）以便通用，若需要传入一个值类型时，这时需要对这个值类型进行装箱
* 一个非泛型容器，同样是为了通用，将元素类型定义为 `Object`，于是，将数值放入容器时，需要装箱
* 当运行 `==` 时，若一个操作数为包装类，一个为值类型，这时会触发自动拆箱

### 装箱
装箱就是将基本数据类型转化为包装类型

* 装箱：利用包装类的构造方法进行装箱，如 `Integer a = new Integer(1);`
* 自动装箱：又叫隐式装箱，直接给 `Integer` 赋值，即 `Integer a = 1;`，在编译的时候，会调用 `Integer.valueOf()` 的方法进行装箱

自动装箱可能比装箱又更好的性能，主要体现在自动装箱的缓存上
**代码演示**
```java
    Integer a = 100; // 自动装箱
    Integer b = 100; //  自动装箱
    Integer c = 200; //  自动装箱
    Integer d = 200; //  自动装箱
    System.out.println(a == b); // 输出 true
    System.out.println(c == d); // 输出 flase
```
在上面的代码中，`a==b` 的结果为 `true`，`c==d` 的结果为 `false`，这是因为自动装箱在编译代码的时候，调用 `Integer.valueOf()` 方法，查看该方法的源码，可以知道，当 `i` 的值在 `[-128，127]` 之间是，返回的 `Integer` 中的缓存引用，没有在这个范围时，会重新创建一个 `Integer` 实例，即代码中 `a` 和 `b` 指向了同一个对象，`a == b` 返回 `true`，而 `c` 和 `d` 不在范围内，即 `c` 和 `d` 不是指向同一个对象，`c == d` 返回 `flase`。

valueOf()
```java
    @HotSpotIntrinsicCandidate
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```
**注意：** `Byte`，`Short`，`Long`，`Character` 的 `valueOf` 实现机制类似，其中，`Byte` 类型比较一直返回 `true`，因为它的范围就是 `[-128，127]`；`Short`，`Long`，`Integer` 的 `i` 的范围是 `[-128，127]`；`Character` 的  `i` 的范围是小于 `127`，因为 `Character` 的最小值为 `0`；`Float` 和 `Double` 的 `valueOf` 永远创建新的对象，因为小数无法放到缓存中；`Boolean` 中只有两个对象，`true` 和 `false`，只要 `boolean` 的值xia相同，`Boolean` 就相同。

### 拆箱
拆箱就是将包装类型转化为基本数据类型

* 拆箱：使用包装类提供的方法，如 `intValue()` 转化为值类型
* 自动拆箱：包装类和值类型进行运算的时候，包装类对自动调用 `intValue` 方法，转换为值类型

**代码演示**
```java
    Integer a = 100;
    int c = a.intValue(); // 手动拆箱
    int d = c; // 自动拆箱
```

其中，包装类对应拆箱方法如下所示

| 包装类 | 拆箱方法   |
| --------   | -----  |
| Byte   | byteValue()  |
| Short   | shortValue()  |
| Integer   | intValue()  |
| Long   | longValue()  |
| Float   | floatValue()  |
| Double   | doubleValue()  |
| Character   | chartValue()  |
| Boolean  |booleanValue()  |




