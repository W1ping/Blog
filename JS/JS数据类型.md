# JavaScript 的数据类型

JavaScript 语言共有 8 种数据类型，其中包括 7 种原始数据类型和 1 种对象数据类型：

1.  数字（Number）
2.  字符串（String）
3.  布尔值（Boolean）
4.  空值（Null）
5.  未定义（Undefined）
6.  符号（Symbol）
7.  大整数（BigInt）
8.  对象（Object）

下面对每种数据类型进行详细介绍，并给出代码示例和区别点的总结。

### 数字（Number）

数字类型用于表示数值，包括整数和浮点数。例如：

```js
let num = 10; // 整数
let pi = 3.14; // 浮点数
```

区别点：

*   JavaScript 中数字类型不分整数和浮点数类型，而是统一使用 Number 类型来表示；
*   JavaScript 中的数字实际上是 64 位浮点数，在处理整数时可能会出现精度丢失的问题，比如经典的 0.1 + 0.2 ≠ 0.3；
*   JavaScript 中可以使用 `NaN` 表示非数字，它与任何值都不相等，包括自身，用过 isNaN 来判断是否是 NaN。

### 字符串（String）

字符串类型用于表示一段文本，可以使用单引号、双引号或反引号括起来。例如：

```js
let str1 = 'hello'; // 使用单引号
let str2 = "world"; // 使用双引号
let str3 = `hello, ${name}`; // 使用反引号和模板字符串
```

区别点：

*   单引号、双引号和反引号都可以用于表示字符串，但需要注意在字符串中需要转义的字符；
*   反引号可以使用模板字符串来方便地拼接变量和表达式；
*   JavaScript 中的字符串是不可变的。

### 布尔值（Boolean）

布尔值类型用于表示真或假，只有两个值：`true` 和 `false`。例如：

```js
let isDone = true;
let isFinished = false;
```

区别点：

*   布尔值仅包含 `true` 和 `false` 两个值；
*   布尔值常用于条件判断和逻辑运算中。

### 空值（Null）

空值类型用于表示一个不存在的对象或空值。例如：

```js
let obj = null;
```

区别点：

*   空值表示一个空对象，与未定义不同；
*   在 JavaScript 中，`null` 被视为一个对象，但实际上它是一个表示“空”的特殊值。

### 未定义（Undefined）

未定义类型用于表示一个未赋值的变量或不存在的属性。例如：

```js
let num; // 声明但未赋值，值为 undefined
let obj = {}; // 定义了一个空对象，但没有设置属性 name，obj.name 的值为 undefined
```

区别点：

*   未定义表示一个值未被赋值，与空值不同。

### 符号（Symbol）

符号类型用于表示独一无二的值，ES6 引入了这个新类型。例如：

```js
let key1 = Symbol('key');
let key2 = Symbol('key');
```

区别点：

*   符号是一种原始数据类型，用于表示一些独一无二、不可变且无法重复的值；
*   符号在对象中常用作属性名，以保证属性名不会冲突；
*   同一个符号值不相等，不同符号值不相等。

### 大整数（BigInt）

大整数类型用于表示任意精度的整数，ES2020 引入了这个新类型。例如：

```js
let bigNum = 1234567890123456789012345678901234567890n;
```

区别点：

*   JavaScript 中的 Number 类型只能表示有限范围的整数和浮点数，而 BigInt 类型可以表示任意精度的整数；
*   BigInt 类型的数字必须带有后缀 `n`，例如 `10n` 表示 BigInt 类型的整数 10。

### 对象（Object）

对象类型表示一个包含属性和方法的集合，可以使用大括号 `{}` 来创建。JavaScript 中还有一些特殊的对象类型，如数组、函数、日期时间、正则表达式、Map 和 Set 等。例如：

```js
let obj1 = {}; // 空对象
let obj2 = {
  name: '半生瓜',
  age: 20,
}; // 拥有两个属性的对象
```

区别点：

*   对象类型表示一组属性和方法的集合；
*   对象可以通过点号 `.` 或中括号 `[]` 来访问属性和方法。


## 总结

下面是一个表格，用于总结以上 8 种数据类型的特点：

| 数据类型 | 值类型       | 可变性 | 特殊值            | 创建方式                        |
| -------- | --------- | --------- | -------------- | ------------------------ |
| 数字   | Number    | 可变  | NaN            | 直接赋值或使用 Number() 函数  |
| 字符串  | String    | 不可变 | 空字符串 | 直接赋值或使用 String() 函数         |
| 布尔值  | Boolean   | 不可变 | true、false     | 直接赋值或使用 Boolean() 函数        |
| 空值   | Null      | 不可变 | null           | 通过将变量赋值为 null 创建     |
| 未定义  | Undefined | 不可变 | undefined      | 通过声明变量但不进行赋值，或通过 void 运算符创建 |
| 符号   | Symbol    | 不可变 | 无            | 使用全局函数 Symbol() 创建          |
| 大整数  | BigInt    | 可变  | 无            | 直接使用后缀 n 来创建                |
| 对象   | Object    | 可变  | 无            | 使用大括号或者构造函数创建               |


不可变性: 简单来说，当你尝试通过改变字符串中的某个特定字符来修改一个字符串时，实际上你并没有更改原有的字符串，而是创建了一个新的字符串并替换了旧的字符串。在 JavaScript 中，String 等类型被设计为不可改变的，以提高字符串操作的效率和性能。

以上是本文对 JavaScript 数据类型的介绍，包括每种类型的定义、示例和区别点的总结。希望这篇文章能够帮助大家更好地理解这些基础知识。
