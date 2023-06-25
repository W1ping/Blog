# null 和 undefined 的区别

## 介绍

在 JavaScript 中，null 和 undefined 都表示“没有值”，但它们之间有一些不同的细微差别。

## 区别

### 数据类型

*   null 是一个表示空对象引用的特殊关键字，而 undefined 表示未定义的值。
*   在 JavaScript 中，typeof null 返回 "object"，而 typeof undefined 返回 "undefined"。

### 赋值方式

*   undefined 是一个默认值，表示声明了变量但未赋值或函数没有返回值时的默认返回值。null 是一个赋值给变量的值，表示该变量目前没有指向任何对象。
*   注意：如果想将值设置为 null，请使用显式赋值，而不是直接不声明变量或者使用 undefined。

### 行为

*   如果给变量赋值为 null 或者 undefined，它们都会让变量的值变为“没有值”。
*   在条件语句中，它们均被视为“假”（falsy）值。也就是说，在 if 语句、逻辑操作符和三元运算符等条件判断语句中，它们都被当作 false 来处理。
*   在运行时，在读取未定义的变量或属性时，js 会抛出 Error，而使用 null 则不会抛出 Error。

### 对象属性

*   当访问一个未定义的对象属性时，返回 undefined。
*   当访问一个 null 对象的属性时，抛出 TypeError 错误。

## 联系

*   在条件语句中，它们均被视为“假”（falsy）值。也就是说，在 if 语句、逻辑操作符和三元运算符等条件判断语句中，它们都被当作 false 来处理。
*   如果给变量赋值为 null 或者 undefined，它们都会让变量的值变为“没有值”。

## 代码示例

```javascript
// undefined 示例
let x; // 声明一个变量，但未赋值
console.log(x); // 输出 undefined
console.log(typeof x); // 输出 "undefined"

function doSomething() {
  return; // 没有返回值
}

const result = doSomething();
console.log(result); // 输出 undefined

// null 示例
let y = null; // 显式将其设置为 null
console.log(y); // 输出 null
console.log(typeof y); // 输出 "object"
```

## 神奇的 null

typeof null 返回 "object" 是 JavaScript 语言的历史原因所致。当 JavaScript 在最初设计和实现时，null 被认为是一个空对象指针，其类型标记被设置为 "object"，这个错误一直保留到了现在。虽然这是一种历史遗留问题，但是这个行为已经成为了 JavaScript 的标准规范，所以 typeof null 返回 "object" 已经像一个特性一样被固定下来，并难以改变。因此，对于判断 null 的类型，我们需要额外判断一下，而不是仅仅使用 typeof 。

使用严格相等运算符（===）可以把 null 与 undefined 区分开：

```javascript
    let x = null;
    console.log(x === null); // 输出 true

    let y;
    console.log(y === undefined); // 输出 true
```
## 表格总结

| 区别点        | null     | undefined    |
| ---------- | -------- | ------------ |
| 数据类型       | 空对象引用    | 未定义的值        |
| 赋值方式       | 赋值给变量    | 默认值          |
| 值          | 指向空对象    | 未定义          |
| 对象属性访问     | 不会抛出异常   | 抛出 TypeError |
| typeof 运算符 | "object" | "undefined"  |
| 在条件语句中的值   | false    | false        |

## 总结

null 和 undefined 在 JavaScript 中都表示“没有值”，但是它们的数据类型、赋值方式、行为等方面存在着一些细微差别。null 通常用来表示变量没有值或者不存在，而 undefined 出现在一些边界情况下，例如未定义的变量、不存在的对象属性、忘记返回值的函数等等。

