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

## String

转义序列表示一个字符，只算一个字符

### 转换为 string

#### 1. toString()

几乎所有值都有的 `toString()` 方法，返回值的字符串等价物

`null` 和 `undefined` 没有该方法

`toString() `操作数字时可以传入参数（进制数）

#### 2. String()

`String()` 转型函数规则：

- 如果值有 `toString()` ，调用
- 值为 null，返回 `"null"`
- 值为 undefined，返回 `undefined`

## 模板字符串

模板字面量保留换行字符，可以跨行定义字符串

定义模板很有用：

```js
let pageHTML = `
<div>
	<a href="#">
		<span>Jake</span>
	</a>
</div>`;
```

模板字符串通过 `${}` 可以插入一个 JavaScript 表达式

将表达式转换为字符串时会调用 `toString()`

```js
let foo = { toString: () => 'World' };
console.log(`Hello, ${ foo }!`); // Hello, World!
```





## 假值的数据类型

"假值"总共有6个：

false，undefined，null，0，""（空字符串），NaN

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

# 4. 操作符

## typeof 和 instanceof

typeof 用于检测原始值，对 `null` 或对象 返回 `"object"`

instanceof 检测引用值（对象）是什么类型的对象
`variable instanceof constructor`

# 5. 垃圾回收 GC

两种主要策略：**标记清理** 和 **引用计数**

## 1. 标记清理

垃圾回收程序运行过程：

1. 进行标记：标记内存中存储的所有变量
2. 清除上下文中的变量以及被变量引用的变量身上的标记
3. 开始内存清理：清除带标记的所有值，并回收内存

## 2. 引用计数

对每一个值都记录它被引用的次数，垃圾回收程序运行时释放引用数为0的值的内存。

缺点：**循环引用**

- 对象A有一个指针指向对象B，而 对象B也引用了对象A

- ```js
  function problem() {
      let objectA = new Object();
      let objectB = new Object();
      objectA.someOtherObject = objectB;
      objectB.anotherObject = objectA;
  }
  ```

  函数执行完后，objectA和objectB无法被清理，内存无法释放

# 6. 内存管理

因为有 GC 的缘故，无需关心内存管理。不过，将内存占用量保持在一个较小的值可以让页面性能更好。

优化内存占用的最佳手段就是保证在执行代码时只保存必要的数据。

数据不必要时，设置为 `null` ，释放其引用（解除引用）。

1. let 和 const 声明能够提升性能。

   块级作用域，相比 `var` ，会更早的让垃圾回收程序回收该回收的内存

2. 使用隐藏类 和 避免 `delete` 操作

   V8 引擎会利用隐藏类，能够共享相同隐藏类的对象性能会更好，比如

   ```js
   let a = new Person();
   let b = new Person();
   ```

   V8 会让两个类实例共享相同的隐藏类，而动态添加属性会产生不同的隐藏类，`delete` 删除属性也会产生不同隐藏类

   因此：避免动态赋值，通过构造函数一次性声明属性，以及将属性设置为 `null` 而不是删除的做法避免性能问题。

3. **避免内存泄漏**
   - 意外生成全局变量
   - 闭包引用外部变量
4. 静态分配和对象池
   - 不要动态创建矢量对象
   - 使用对象池，防止垃圾回收程序频繁执行

# 7.  内置对象

## Array

### 有哪些常用的方法？

- Array.from()：可以将类数组转化为数组

- Array.of(arguments)：将参数转化为数组

- keys()，values()，entries() 三迭代期方法

- pop()，push() 实现类似栈的行为

- shift()，push() 实现队列行为

- unshift()，pop() 实现反向的队列行为

- sort()，reverse() 实现排序

  sort() 默认会转换成 string 来比较；可以传入一个比较函数作为参数，实现想要的升序降序

  

# 8. 迭代器和生成器

## 8.1 迭代器 Iterator

 实现了 Iterable 接口的结构称为 ”可迭代对象“ 。可以通过迭代器消费

### **如何实现 Iterable 接口（可迭代协议）？**

1. 支持迭代的自我识别能力
2. 具有创建实现了 Iterator 接口的对象的能力

在ES中，利用 Symbole.iterator 作为键，值引用一个迭代器工厂函数（每次调用都可以生成返回一个新迭代器），这样就可以暴露一个 “默认迭代器”

### **迭代器是什么？**

迭代器是一种一次性使用的对象，用于迭代与其相关联的可迭代对象

为什么说是一次性？

无法重复使用。

## 8.2 生成器 Generator

### **什么是生成器？生成器有什么作用？**

生成器是带星号（*）的函数

```js
function * generatorFn(){}
```

在生成器函数体内，可以进行暂停和恢复代码执行

利用生成器，可以方便的生成自定义的可迭代对象

### **如何使用生成器？**

通过生成器对象和 yield 关键字使用

#### 1. 生成器对象

调用生成器产生的，生成器对象实现了 Iterable 接口，因此生成器对象具有 `next()` 方法，可以让生成器开始或恢复执行。

> 生成器对象就是一个迭代器，只不过调用 next() 方法是让生成器函数恢复执行。

**生成器对象的消耗方式是作为可迭代对象使用**。

#### 2. yield 关键字

在生成器函数体内使用，让生成器停止

遇到 yield ，执行停止，函数作用域状态保留。调用生成器对象的 `next()` 方法恢复执行。

##### yield* 可以实现递归

yield* 后面跟一个可迭代对象，如果是生成器对象，择会移交执行权。

```js
function* nTimes(n) {
    if (n > 0) {
    	yield* nTimes(n - 1);
    	yield n - 1;
    }
}
for (const x of nTimes(3)) {
	console.log(x);
}
// 0
// 1
// 2
```

# 9. ES6 增强的对象语法

ECMAScript 6为定义和操作对象新增了很多极其有用的语法糖特性

## 9.1 属性值简写

**如何简写？**

对象字面量只需使用变量名（不再写冒号）就会被自动解释为同名的属性键

```js
let name = "thea";
const lover = {
    name
};
```

##  9.2 可计算属性

**有什么用？如何书写？**

实现在对象字面量中的动态的属性键，在中括号中放入js表达式或变量

```js
const nameKey = 'name';
let person = {
    [nameKey]: 'thea',
};
```



## 9.3 简写方法名

给对象定义方法时，不用写冒号与 function 关键字了

```js
let person = {
    sayName(name) {
    	console.log(`My name is ${name}`);
    }
};

```

## 9.4 对象解构

**ES6新增的对象结构语法有什么用？**

可以在一条语句中使用嵌套数据实现一个或多个赋值操作

对象解构就是使用与对象匹配的结构来实现对象属性赋值。

```js
// 使用对象解构
let person = {
    name: 'Thea',
    age: 18
};
let { name: personName, age: personAge } = person;
console.log(personName); // Thea
```

搭配属性简写法，让变量直接使用属性的名称

```js
let person = {
    name: 'Thea',
    age: 18
};
let { name, age } = person;
console.log(name); // Thea
console.log(age); // 18

```

# 10. 原型链

类和继承，底层就是基于原型的继承

## 1. 函数的原型

每个函数都会创建一个 `prototype` 属性（指向原型对象），原型对象默认有一个 `constructor` 属性，指向与之关联的构造函数

```js
function Person(){}

Person.prototype.constructor = Person;

let p1 = new Person();
p1.__prototype__ = Person.prototype;
```

正常的原型链都会终止于Object的原型对象，Object原型的原型是null

## 2. 对象属性查找机制

访问对象属性时，会按照原型链去查找。

## 3. 原型与 in 操作符

**in 有几种使用方式**

`in` 操作符的两种使用方式：

- 单独使用

  判断属性是否在对象（实例属性或原型属性）上。

- `for-in` 循环

   获取对象可被枚举的属性（包括原型上的）

**Object.keys()** 获取对象可枚举的实例属性。

# 11. 继承

**如何实现继承？**

基于原型链实现：通过原型继承多个引用类型的属性和方法

## 1. 原型链继承

## 2. 经典继承（盗用构造函数）

## 3. 组合继承



