# 1. JavaScript 实现

能够与网页交互的脚本语言，包含仨：

- ECMAScript：是蓝图与标准，由 ECMA-262 定义，提供了核心功能
- 文档对象模型（DOM）：提供与网页内容交互的方法和接口。
- 浏览器对象模型（BOM）：提供与浏览器交互的方法和接口。

![JavaScript实现](JavaScript.assets/JavaScript实现.jpg)

# 2. 变量声明

## var 

- 定义变量时省略 var 操作符会创建为全局变量
- var 具有声明提升
- 全局作用域中使用会使得变量成为 `window` 对象的属性

## let
- 不允许重复声明，会报 `SyntaxError` 语法错误
- 暂时性死区，在 let 声明变量之前使用变量会报 `ReferenceError`
- 全局作用域中`let` 声明的变量不会成为 `window` 对象的属性
- 使用 `let` 声明迭代变量，适用于 `for、for-in、for-of` 循环

## const

- 对于 `for-in、for-of` 很实用

# 3. 数据类型

6种原始数据类型：Undefined，Null，Boolean，Number，String，Symbol

复杂数据类型：Object

## 使用 typeof 返回

| 返回值      | 说明                    |
| ----------- | ----------------------- |
| "undefined" |                         |
| "boolean"   |                         |
| "string"    |                         |
| "number"    |                         |
| "symbol"    |                         |
| "object"    | 值为 object 或 **null** |
| "function"  | 函数                    |

null 被认为是一个空对象的引用

函数也是对象，但它有特殊的属性，所以特殊对待一下

## Undefined

使用 `var` 或 `let`声明了变量但没有初始化，相当于给变量赋予了 `undefined` 值

```js
let message;
console.log(message == undefined); // true
// 显示赋值 undefined 效果一样
let message = undefined;
console.log(message == undefined); // true
```

typeof 操作符对一个没声明的变量使用时，会返回 `undefined`

```js
let message; // 这个变量被声明了，只是值为undefined
// 确保没有声明过这个变量
// let age
console.log(message); // "undefined"
console.log(age); // 报错

console.log(typeof message); // "undefined"
console.log(typeof age); // "undefined"
```



## Null

`undefined` 值是由 `null` 值派生而来的，因此ECMA-262将它们定义为表面上相等

```js
console.log(null == undefined); // true
```

## Boolean

布尔值不同于数值，true 不等于 1，false 不等于 0

```js
console.log(true === 1) // false
console.log(false === 0) // false
// 隐式转换会将 1 转换为 true，0 转换为 false
console.log(true == 1) // true
console.log(false == 0) //false
```

将一个其他类型的值转换为布尔值，调用特定的 Boolean() 函数

### **其他数据类型转换为布尔值规则**

| 数据类型  | false        | true          |
| --------- | ------------ | ------------- |
| String    | ""(空字符串) |               |
| Number    | 0，NaN       |               |
| Object    | null         |               |
| undefined | undefined    | N/A（不存在） |

**像 `if` 等流控制语句会自动执行其他类型值到布尔值的转换**

## Number

### 1. 不同进制表示

| 进制 | 前缀 |
| ---- | ---- |
| 八   | 0o   |
| 十六 | 0x   |

很难测试特定的浮点值，0.1加0.2得到的不是0.3

> 因为使用了IEEE 754数值，这种错误并非ECMAScript所独有

### 2. +- infinity

`Number.NEGATIVE_INFINITY、`
`Number.POSITIVE_INFINITY、`
`isFinity()`

```js
console.log(5/0); // Infinity
console.log(5/-0); // -Infinity
```

### 3. NaN

```js
console.log(0/0); // NaN
console.log(-0/+0); // NaN
// NaN 不等于任何值
console.log(NaN == NaN) // false
```

#### isNaN()

如果传入其他类型，会尝试转换参数为数值，不能转换的都会导致函数返回 `true`

### 4. 将非数值转换为数值

`Number()、parseInt()、parseFloat()`

- Number() 适合任意类型

- parseInt()、parseFoat() 主用于字符串

#### Number() 函数

Number() 函数转换规则：

- boolean：true => 1，flase => 0

- null：0

- undefined：NaN

- string：

  ```js
  // 数值字符串转换为数字，保留加减号
  Number("123") // 123
  Number("0123") // 123
  Number("1.1") // 1.1
  Number("-123") // -123
  
  // 八进制或十六进制 转换为十进制数值
  Number("0o17") // 15
  Number("0xf") // 15
  
  // 空字符串返回 0
  Number("") // 0
  
  // 非数值字符串返回 NaN
  Number("123abc") // NaN
  ```

- object：

  调用 `valueOf()` 方法，按照上述几种规则转换，如果是 NaN，再调用 `toString()` 方法，按照字符串规则转换。

#### parseInt() 函数

专注于字符串是否包含数值，从第一个非空格字符开始检查，第一个字符不为 `数字字符，+、-` ，立刻返回 `NaN`

否则检测到非数值字符或字符串结尾停止。

```js
parseInt(" 13Love") // 13
// 与Number("")返回 0 不同，parseInt("") 返回 NaN
parseInt("") // NaN
// 支持16进制0x开头
parseInt("0xd") // 13
```

**第二个参数可以显示传递解析数值的进制**

```js
parseInt("d", 16) // 13

// 返回 0 和 NaN 的不同
parseInt("08", 8) // 0
parseInt("8", 8) // NaN
```

#### parseFloat() 函数

parseFloat() 只解析十进制值，没有第二个参数，会保留第一个小数点，忽略第二个小数点'

**支持科学计数法**

```js
let num1 = parseFloat("1234blue"); // 1234，按整数解析
let num2 = parseFloat("0xA"); // 0
let num3 = parseFloat("22.5"); // 22.5
let num4 = parseFloat("22.34.5"); // 22.34
let num5 = parseFloat("0908.5"); // 908.5
let num6 = parseFloat("3.125e7"); // 支持科学计数法 31250000
```

## 假值的数据类型

undefined , null 是假值

```js
let message = null;
let age;
if (message) {
	// 这个块不会执行
}
if (!message) {
	// 这个块会执行
}
if (age) {
	// 这个块不会执行
}
if (!age) {
    // 这个块会执行
}
```





