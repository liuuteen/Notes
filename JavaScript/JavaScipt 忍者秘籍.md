# 1. 了解 JavaScript 

JavaScript 中的一些概念不同于其他语言：

- 函数也是对象
- 函数的闭包 —— 简单来讲就是函数主动维护了再函数内使用的外部的变量。
- 作用域 —— 全局、函数、块级作用域
- 基于原型的面向对象

JavaScript 还有很多特性：

- Generators ，一次请求，生成多值，不同请求之间能够挂起执行。
- Promise，更好控制异步代码
- 代理，控制对特定对象的访问

- 高级 Array 的方法
- Map，Set
- 正则表达式
- 模块化

## 1.1 浏览器

![image-20210816111546901](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210816111546901.png)

- DOM
- 事件
- 浏览器API

## 1.2 最佳实践

实践需要的特质：

- 调试技巧

  使用浏览器的开发者工具

- 测试

  利用断言函数

- 性能分析

  在方法前后使用计时器打印时间日志，测试性能

# 2. 页面构建

## 2.1 生命周期

![image-20210816160130648](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210816160130648.png)

浏览器接收到响应时，开始生命周期。

客户端  Web 应用时图形用户界面（GUI）应用，生命周期与其他 GUI 应用相似（桌面应用或移动应用）

步骤：

1. 页面构建 —— 创建 UI
2. 事件处理 —— 进入循环等待事件发生，发生后调用事件处理器。

应用的声明周期会在用户关掉或离开页面时结束。

## 2.2 页面构建阶段

1. 解析 HTML 代码并构建文档对象模型
2. 执行 JavaScript 代码

步骤 1 在浏览器处理 HTML 节点的过程中执行，步骤 2 在 HTML 解析到一种特殊节点 —— script 节点（包含或引用 JavaScript 代码）时执行。这两个步骤会交替执行多次。

![image-20210816160909156](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210816160909156.png)

### 2.2.1 HTML 解析和 DOM 构建

根据 HTML 代码构建 DOM。遇到脚本元素执行脚本代码

:heavy_check_mark: 浏览器会修复在 HTML 代码中发现的问题来创建一个合法的 DOM。

### 2.2.2 执行 JavaScript 代码

脚本元素中的 JavaScript 代码由浏览器的 JavaScript 引擎执行。(Firefox 是 Spidermonkey，Chrome 和 Opera 是 V8)

浏览器通过全局对象提供了一个 API ，让 JavaScript 引擎可以与纸交互并改变页面内容。

#### **Window 对象**

浏览器暴漏的主要全局对象，通过 `window` 对象可以获取所有其他全局对象、全局变量（包含用户定义对象）和浏览器 API 的访问途径。

全局 `window` 对象最重要的属性是 **`document`**

#### 全局代码 / 函数代码

包含在函数内的代码是函数代码

所有函数以外的代码是全局代码

## 2.3 事件处理

为了提供交互能力，在页面构建阶段执行 JavaScript 代码时会注册事件监听器（或处理器）。当事件发生时，浏览器调用执行。

浏览器使用事件队列来跟踪已经发生但尚未处理的事件。

**事件是异步**

事件类型：

- 浏览器事件，例如页面加载完成或无法加载
- 网络事件，例如来自服务器的响应（Ajax 事件和服务器端事件）
- 用户事件，例如鼠标单击、鼠标移动和键盘事件；
- 计时器事件，当 timeout 时间到期或又触发了一次时间间隔(setTimeout, setInterval)

### 2.3.1 注册事件处理器

- 把函数赋给某个特殊属性
- 通过内置 `addEventListener` 方法

不推荐把函数赋值给特殊属性的方式，这种方式对于某个事件只能注册一个事件处理器。容易被下一个事件处理器改写。

`addEventListener` 可以给同一个事件注册多个事件处理器。(例如 click 事件)

### 2.3.2 处理事件

单线程执行模型，同一时刻只能处理一个事件。

# 3. 函数 Function

JavaScript 函数式编程：函数是程序执行过程中的主要模块单元。

## 3.1 函数是第一类对象

- 通过字面量创建

  ```js
  function ninjaFunction(){}
  ```

- 赋值给变量，数组项或其他对象的属性

- 作为函数的参数传递

- 作为函数返回值

- 能保存动态创建和分配的属性

  ```js
  var ninjaFunction = function(){};
  ninjaFunction.name = "Hanzo"; 
  ```

函数也是对象，but **函数特殊的地方在于可以调用(call)**，用以执行某项动作。

## 3.2 回调函数

定义一个随后被调用的函数，这个函数（在事件处理阶段被浏览器）或（被其他代码调用），这就是回调函数。

```js
function useless(ninjaCallback) { 
	return ninjaCallback(); 
}
```

JavaScript最重要的特性之一是表达式代码可以出现的任何地方，都能够创建函数

数组排序 sort 方法需要一个回调函数。

## 3.3 函数添加属性好处

- 集合中存储函数可以管理相关的函数
- 记忆能力，函数记住上一次的值，提高后续调用性能

### 3.3.1 存储函数

```js
var store = {
	nextId: 1, 
	cache: {}, 
	add: function(fn) { 
    	if (!fn.id) { 
        	fn.id = this.nextId++; 
        	this.cache[fn.id] = fn; 
        	return true; 
    	} 
	}
};
function ninja(){}
assert(store.add(ninja), "Function was safely added."); 
assert(!store.add(ninja), "But it was only added once."); 
```

### 3.3.2 自记忆函数

```js
function isPrime(value){
	if (!isPrime.answers) {
		isPrime.answers = {}; 
    }
    if (isPrime.answers[value] !== undefined) {
    	return isPrime.answers[value];
    }
    
    var prime = value !== 1; // 1 is not a prime
    
    for(var i = 2; i < value; i++) {
        if(value % i === 0) {
            prime = false;
            break;
        }
    }
    return isPrime.answers[value] = prime;
} 
```

## 3.4 定义函数

- 函数声明 和 函数表达式
- 箭头函数
- 函数构造器(Function constructors)
- 生成器函数

### 3.4.1 函数声明和函数表达式

**函数声明**

强制性的 `function` 开头，强制性的函数名，函数声明必须独立

**函数表达式**

```js
var a = function() {};
myFunction(function(){});
```

总是其他语句一部分的函数叫做函数表达式。

### 3.4.2 立即函数 IIFE

```js
(function(){})(3);
```

immediately invoked function expression (IIFE), 立即调用函数表达式

给函数表达式加括号是为了让 JavaScript 解析器能够认识这是一个表达式
否则，以 `function` 开头会被解析器认为是函数定义，没有名字就会报错。

还可以使用一元运算符来表示 立即调用函数

```js
+function(){}();
-function(){}();
!function(){}();
~function(){}();
```

### 3.4.3 箭头函数 =>

从某方面说，箭头函数是函数表达式的简化版。

## 3.5 函数的实参和形参

- 形参是定义函数时列举的变量
- 实参是调用函数时传递给函数的值

### 3.5.1 剩余参数 rest parameters

```js
function multiMax(first, ...remainingNumbers){
    // ...
}
```

剩余参数以省略号作为前缀

### 3.5.2 默认参数

为函数形参赋值

```js
function performAction(ninja, action = "skulking"){};
```

**引用前一个默认参数**

```js
function performAction(ninja, action = "skulking", message = ninja + " " + action){};
```

# 4. 函数的调用

## 4.1 隐式参数

函数调用时会传递两个隐式的参数： `arguments` 和 `this`

### 4.1.1 arguments

`arguments` 是传递给函数的所有参数集合。可以实现原生 JavaScript 不支持的**函数重载特性**，还可以实现参数可变的可变函数。

`arguments` 是类数组，有 length 属性，可以通过下标索引访问元素。但不能使用数组方法

> 剩余参数  function sum(...restParams)，是数组，推荐使用。

```js
function sum() {
    var sum = 0;
    for(var i = 0; i < arguments.length; i++){
        sum += arguments[i];
    }
    return sum;
}
```

**arguments 对象是函数参数的别名** （严格模式下不生效）

修改 arguments 对象的值，同时会影响对应的函数参数; 反之亦然。

```js
test('a');
function test(str){
    arguments[0] = 'b';
    console.log(str); // b
    str = 'c';
    console.log(arguments[0]); // c
}
```

### 4.1.2 this ： 函数上下文

代表函数调用相关联的对象。

函数调用方式不同 = this 值不同

## 4.2 函数调用

4种方式：

- 作为函数直接被调用 — skulk()
- 作为方法，关联在对象上，实现面向对象编程 — ninja.skulk()
- 作为构造函数，实例化一个新的对象 — new Ninja()
- 通过函数的 apply 或者 call 方法 — skulk.apply(ninja) 或 skulk.call(ninja)

### 4.2.1 作为函数被调用

使用 `()` 操作符调用函数，且函数表达式不是作为一个对象的属性时，称为作为函数调用。

`this` 函数上下文有两种情况：

1. windows对象 ，非严格模式
2. undefined，严格模式

### 4.2.2 作为方法调用

函数被赋值给对象的属性，并且通过对象属性引用的方式调用函数。

对象会成为函数的上下文（this）。

### 4.2.3 作为构造函数调用

函数调用之前使用关键字 `new`

```js
function Person(){
    return {name: 'erke'}
}
new Person();
```

构造函数是用来创建和初始化对象实例的函数。

> 函数构造器可以用来构造新的函数
>
> new Function('a', 'b', 'return a+b');

通过 `new` 调用函数触发：

1. 创建一个新的空对象

2. 该对象作为 `this` 参数传递给构造函数，从而成为构造函数的函数上下文

3. 新构造的对象作为 `new` 运算符的返回值（手动return时会有例外情况）

   > 构造函数显式返回一个对象时，则该对象被返回，传入构造函数的 this 被丢弃
   >
   > 如果显式返回非对象，则忽略返回值，返回新创建的对象

**注意**

函数和方法命名通常以描述行为的动词开头，且开头字母小写，walk，run

构造函数以描述所构造对象的名词命名，以大写字母开头 Cat、Dog

### 4.2.4 apply 和 call 方法调用

改变函数上下文（this引用的对象），显式指定它

解决事件处理函数中的 `this` 指向问题（默认指向事件触发的目标元素）。最方便的就是使用箭头函数解决

两个方法传递参数：

apply：作为函数上下文的对象和一个作为函数调用参数的值的**数组**

call：作为函数上下文的对象和一个参数列表

```js
juggle.apply(ninja1, [1, 2, 3, 4])
juggle.call(ninja2, 5, 6, 7, 8)
```

## 4.3 解决函数上下文的问题

箭头函数和 `bind` 方法可解决回调函数中（如事件处理器）函数上下文与预期不符的问题。

### 4.3.1 箭头函数函数上下文

箭头函数的 `this` 与声明所在的上下文相同

### 4.3.2 bind

函数可以通过 `bind` 方法创建新函数。

通过 `bind` 方法创建的新函数与原始函数的函数体相同，新函数的上下文被绑定到指定的对象上（不论新函数以何种方式被调用）

# 5. 闭包与作用域

## 5.1 理解闭包

外部函数中声明内部函数时，创建了闭包，只要该内部函数存在，它就会存在。

闭包包含了被定义时的作用域内得变量和函数。

> 全局作用域其实就是一个闭包

## 5.2 闭包有什么用

### 1. 封装私有变量

原生 JavaScript 不支持私有变量

```js
// 闭包模拟私有变量
function Ninja() {
    var feints = 0;
    this.getFients = function() {
        return feints;
    };
    this.feint = function() {
        feints++;
    };
}

var ninja1 = new Ninja();
ninja1.feint();

// 无法直接访问 fients
ninja.fients; // undefined
ninja.getFients;
```

![image-20210825140438977](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210825140438977.png)

### 2. 回调函数

处理回调函数是另一种常见的使用闭包的情景。

```js
function animateIt(elementId) {
    var elem = document.getElementById(elementId);
    var tick = 0;
    var timer = setInterval(function() {
        if (tick < 100) {
           elem.style.left = elem.style.top = tick + "px";
           tick++;
        }
        else {
            clearInterval(timer);
        }
    }, 10);
}
```

![image-20210825141509533](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210825141509533.png)

**闭包是一个真实的状态封装，只要闭包存在，就可以对变量进行修改**

## 5.3 执行上下文 (excution context)

有两种执行上下文：全局执行上下文和函数执行上下文

全局执行上下文只有一个， JavaScript 程序开始执行时创建，不会消失（直到应用结束）

每次调用函数都生成一个函数执行上下文

**调用栈**：执行上下文栈

![image-20210825150824851](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210825150824851.png)

## 5.4 作用域（词法环境）

作用域主要基于代码嵌套，（全局代码包含函数，函数包含内部函数，内部函数包含for循环）

![image-20210825160344366](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210825160344366.png)

每个执行上下文都有一个与之关联的作用域。

**创建函数，就会创建与之相关联的上级作用域**，存储在 [[Enviroment]]的内部（意味着无法直接访问或操作）属性上。

![image-20210825174719828](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210825174719828.png)

外部环境与新建的词法环境，JavaScript 引擎将调用函数的内置 [[Enviroment]] 属性与创建函数时的环境进行关联。

## 5.5 变量类型

**const** 对 JavaScript 引擎性能优化有帮助。

### 关键字和作用域

var：变量在最近的函数内部或全局作用域定义（忽略块级作用域）

let 和 const 定义的变量具有块级作用域。`let` 和 `const` 在最近的作用域定义变量（块级，循环，函数，全局环境）

![image-20210827095356881](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210827095356881.png)

![image-20210827095447781](https://gitee.com/liutinghome/image-fridge/raw/master/JavaScript-ninja/image-20210827095447781.png)

### 作用域中注册标识符

JavaScript 在函数定义之前就可以调用函数， 但是 why?

**注册标识符的过程**

JavaScript 代码的执行实际上是分两个阶段执行的：

一旦创建了新的作用域，就会执行第一个阶段，不执行代码，但是 JavaScript 引擎会访问并注册在当前作用域中所声明的变量和函数。第一阶段完成后，执行第二阶段，具体如何执行取决于变量的类型（let、var、const和函数声明）以及环境类型（全局环境、函数环境或块级作用域）

具体的过程：

1. 如果是创建一个函数环境，那么创建隐式参数`arguments`，以及以及所有形式的函数参数及其参数值。
   如果是非函数环境，跳过此步骤。
2. 如果我们正在创建一个全局或函数环境，当前代码将被扫描(而不是进入其他函数体)以获取函数声明(但不包含函数表达式或箭头函数!)。对于所找到的函数声明，都会创建一个函数，并绑定到当前环境与函数名相同的标识符上。如果该标识符已经存在，那么该标识符的值会被重写。如果是块级作用域，跳过此步骤。
3. 扫描当前代码进行变量声明，在函数或全局环境中，找到所有定义在其他函数外的用`var` 关键字声明的变量。并找到所有在其他函数或代码块之外通过 `let` 或 `const` 定义的变量。在块级环境中，仅查找通过 `let` 或 `const` 定义的变量。对于所查找到的变量，若该标识符不存在，注册并将其初始化为 `undefined` 。若该标识符已经存在，将保留其值。



# 6. Generator 和 Promise

## 6.1 Generator 函数

定义：`function* Generator() { yield expression }`

生成器调用返回迭代器 `iterator` ，迭代器控制生成器执行。

`iterator.next() // { value: ??, done: true/false }`

可以通过 `for-of` 消费迭代器获取值序列

```js
for(value of iterator){
    console.log(value);
}
// for-of 对迭代器迭代语法糖
```

**移交执行权给下一个Gnerator**(generator函数中调用generator函数)

```js
function* GeneratorA(){
    yield* GeneratorB();
}
```

生成器迭代可以很好的实现递归的代码（如遍历DOM）

**给生成器传递值**

1. 调用生成器函数时传参
2. 调用迭代器 `next`方法时传参

**抛异常**

内部添加`try...catch`，外部调用迭代器 `throw` 方法

## 6.2 Promise

内置构造函数 `Promise`

`new Promise((resolve, reject) => {})`

Promise 是异步的，可防止程序执行被阻塞。

还能更好的进行错误处理，以及解决“回调地狱”问题，也能很好的执行并行任务

**Promise状态**

Promise声明周期中有三种状态

1. pending（等待）
2. fulfilled（完成），调用 resolve 
3. rjected（拒绝），调用 reject 或一个未处理的异常在 promise 调用过程中发生了。

状态变化后无法再改变。

**错误异常处理**

1. `then` 方法提供第二个错误回调函数参数
2. 使用内置 `catch` 方法

触发方式：未处理的异常或显式调用 `reject()` 拒绝 `promise`

**Promise可链式调用**

promise 和 generator 结合

async 是 generator 得语法糖

# 7. 面向对象与原型

原型类似于面向对象语言中的类（class）

**继承**是代码复用（一个对象的属性，另外一个对象可用）的一种方式，通过原型可以实现继承

**查找**对象**属性**会在整个原型链上

**原型链**：每个对象都可以有一个原型，原型也可以有一个原型...形成链



JavaScript 通过 `new` 操作构造函数初始化新对象

构造函数的原型对象会被自动设置为通过该函数创建的对象的原型

- 函数有原型对象
- 函数原型有 constructor 属性，指向函数本身
- 函数的原型设置为新创建的对象的原型

**原型属性和实例属性**

---

私有对象，只能通过构造函数内指定方法模拟（使用闭包）

```js
function Person(){
    let age;
    this.getAge = function () {
        return age;
    }
}
const p1 = new Person();
```

## 7.1 实现继承

实现一个完整原型链最佳技术方案是 一个对象的原型是另一个对象的实例

`SubClass.prototype = new SuperClass()`

这样设置会导致丢失 `constructor` 属性

```js
// 添加回 constructor 属性
Object.defineProperty(SubClass.protptype, "constructor", {
    enumerable: false,
    value: Ninja,
    writable: true,
})
```

## 7.2 对象属性描述（property descriptor）

对象是通过属性描述来表示该对象的一些性质的。

```js
Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});
```

### [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#参数)

- `obj`

  要定义属性的对象。

- `prop`

  要定义或修改的属性的名称或 [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 。

- `descriptor`

  要定义或修改的属性描述符。

描述符关键字：

- configuarble —— true：可以删除属性，可以重新修改属性描述；false：不允许修改。
- enumerable —— 是否会在 for-in 循环对象属性时出现
- value 
- writable —— 是否可以通过**赋值语句**修改属性值
- get —— 定义 getter 函数，访问属性时隐式调用，不能与 value 与 writable 同时使用
- set —— 定义 setter 函数，当对属性赋值时发生调用，也不能与 value 与 writable 同时使用

## 7.3 instanceof 操作符

检查操作符右边的函数的原型是否存在于操作符左边的对象的原型链上。



## 7.4 class 实现继承的原理

class 关键字底层的实现仍然时基于原型继承

```js
class Ninja {
    // 构造函数
    constructor(name){
        this.name = name;
    }
    
    // 定义原型的方法
    swingSword() {
        return true;
    }
    
    // 定义静态方法
    static compare(ninja1, ninja2) {
        return ninja1.level - ninja2.level;
    }
}
var ninja = new Ninja("Alen");
```

```js
// 底层仍然是基于原型的继承

function Ninja(name){
    this.name = name;
}
Ninja.prototype.swingSword = function(){
    return true;
};
// 静态方法
Ninja.compare = function(ninja1, ninja2) {...}
```

**class 实现继承**

```js
class Person{
    constructor(name){
        this.name = name;
    }
    dance(){
        return true;
    }
}

class Ninja extends Person{
    constructor(name, weapon){
        super(name); // super 调用基类 的构造函数
        this.weapon = weapon;
    }
    
    wieldWeapon(){
        return true;
    }
}
```

# 8. 控制对象的访问

## 8.1 getter 与 setter

直接访问或修改对象属性会发生什么情况？

1. 错误赋值
2. 无法记录属性的变化
3. 当属性值变更时，无法更新界面 UI

通过 `getter` 和 `setter` 方法，可以解决这些问题：

- 数据校验
- 日志记录
- 计算属性值（fullName ''姓 +名')'

### 自实现

通过 `getter` 与 `setter` 方法访问对象私有属性

```js
function Ninja () {
    let skillLevel;
    
    this.getSkillLevel = () => skillLevel;
    
    this.setSkillLevel = value => {
        console.log('正在修改属性 skillLevel 为', value); // 插入日志记录
        skillLevel = value;
    }
}
```

扩展方法可以实现插入日志、数据验证、界面修改等...

这种方式下，为了获取属性，必须**显式**调用对应的相关方法（别扭）
`skillLevel` <==> `getSkillLevel / setSkillLevel`

### JavaScript 支持的正牌 getter / setter

JavaScript 支持两种方式定义 `getter` 和 `setter`：

- 对象字面量 或 ES6 的 `class` 中定义 （无法用于私有变量）
- 使用内置 `Objectt.defineProperty` 方法（可以用于私有变量，但更复杂）

#### 对象字面量

使用 `get` 、`set` 关键字

```js
const ninjaCollection = {
	ninjas: ["Yoshi", "Kuma", "Hattori"],
	get firstNinja(){
		report("Getteing firstNinja");
        return this.ninjas[0];
	},
    set firstNinja(value){
        report("Setting firstNinja");
        this.ninjas[0] = value;
    }
}

// 访问 getter 属性
ninjaCollection.firstNinja;

// 直接用对象访问
ninjaCollection.ninjas[0];
```

`getter` 属性与标准对象属性的访问方式一致

一个属性拥有 getter 和 setter 方法：
		访问属性 = 隐式调用 getter 方法
		给属性赋值 = 隐式调用 setter 方法

#### ES6 class 中定义

```js
class NinjaCollection {
    constructor () {
        this.ninjas = ["Yoshi", "Kuma", "Hattori"]
    }
	get firstNinja(){
		report("Getteing firstNinja");
        return this.ninjas[0];
	}
    set firstNinja(value){
        report("Setting firstNinja");
        this.ninjas[0] = value;
    }
}
```

> 不需要对一个属性同时指定 getter 和 setter。通常只提供 getter。
>
> 提供了 getter，如果没有 setter，在非严格模式下，赋值不起作用，JavaScript 引擎会忽略。严格模式下，JavaScript 引擎会报错。

#### Object.defineProperty

实现私有变量的 getter / setter

```js
function Ninja() {
    let _skillLevel = 0;
    Object.defineProperty(this, 'skillLevel', {
        get: () => {
            report("The get method is called");
            return _skillLevel;
        },
        set: value => {
            report("The set method is called");
            _skillLevel = value;
        }
    })
}

const ninja = new Ninja();
```

## 8.2 代理 Proxy

`setter / getter ` 对每个属性都得单独设置

而代理 `proxy` 则可以控制对象的全部交互（属性，方法调用....）

```js
// 通过内置 Proxy 构造器创建代理
const emperor = { name: "Komei" }; // 目标对象
const handler = {
    get: (target, key) => {
        return key in target ? target[key] : "没有这个属性"
    },
    set: (target, key, value) => {
        target[key] = value;
    }
}
const representative = new Proxy(emperor, handler); // 生成代理对象
```

第二个参数 handler 定义对象执行特定行为时触发的函数。详细可看 [[handler对象的方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy#handler_%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%96%B9%E6%B3%95)]

:warning: 代理对象和目标对象都能访问属性。

代理实际使用案例：

- 记录日志

- 性能测量

- 自动填充属性（a.b.c.d.e.f）

  :chestnut: 如果没有b则创建b，防止null异常

- 实现支持负索引的数组

### 支持负索引的数组

```js
function createNegativeArryProxy(array){
    if(!Array.isArray(array)){
        throw new TypeError('Expected an array');
    }
    
    return new Proxy(array, {
        get: (target, index) => {
            index = +index; // 利用隐式转换将属性名转换为数值
            return target[index < 0 ? target.length + index : index];
        },
        set: (target, index, val) => {
            index = +index;
            return target[index < 0 ? target.length + index : index] = val;
        }
    });
}
```

### 代理含有副作用

代理可以实现很好的功能，但也因为引入了额外的处理，所以消耗性能。

谨慎使用代理（ vue 框架好像使用了代理）。

---

# 9. 集合 Collections

## 9.1 Array

创建：

- Array 构造函数 （在原型上，可能被重写，不推荐）
- **数组字面量 `[]` （推荐）**

访问数组长度范围外的索引，返回 `undefined` 

可以修改 `length` 属性：

- 改大，新元素为 `undefined` 
- 改小，数组被裁减

###  添加、删除元素

- push：尾添
- pop：尾删，返回被删除元素
- unshift：头添
- shift：头删，返回被删除元素

> 性能：shift 和 unshift 修改第一个元素，之后每一个元素的索引都需要调整，性能差，一般不建议使用。

### splice 任意位置添加、删除元素

:question:`delete` 操作符无效：数组中被删除的位置变成了空元素，数量不变化

:star: 数组的 `splice` 方法：

```js
splice(startIndex, removeNumber, ...arrays);
起始索引、移除的元素个数、要插入的元素
```

### 数组常用操作

- 映射数组 `forEach`
- 基于现有的数组元素映射创建新数组 `map`
- 验证数组是否匹配指定的条件 `some、every`
- 查找特定数组元素 `find、filter` 、查特定索引 `indexOf、lastIndexOf、findIndex`
- 数组排序 `sort`
- 聚合数组，根据数组元素计算`reduce`

**数组遍历**

1. 可以使用 for 循环
2. Array.prototype.forEach

```js
const ninjas = ["Yagyu", "Kuma", "Hattori"]
ninjas.forEach(ninja => console.log(ninja))
// 不用考虑起始索引，结束条件，计步器
```

**映射数组**

```js
const beautys = [
    {name: "Hepburn", age: '18'},
    {name: "Scarlett", age: '19'}
]

const names = beautys.map(beauty => beauty.name)
```

**测试数组元素**

JavaScript 内置数组的 every 和 some 方法

```js
const ninjas = [
    {name: "yagyu", weapon: "shuriiken"},
    {name: "Yoshi"},
    {name: "Kuma", weapon: "wakizashi"}
]

const allNinjaAreArmed = ninjas.every(ninja => "weapon" in ninja); // false

const someNinjasAreAremed = ninjas.some(ninja => "weapon" in ninja); // true
```

every 和 some 会对每个数组元素执行回调函数：

- `every` 当所有回调结果都为 true 时为 `true`，如果回调函数返回 `false` ，后续元素不再检查
- `some` 当有一项回调结果返回 true 就为` true`，有一项回调结果返回`true` ，后续元素不再检查

**数组查找**

在数组中查找满足条件的一个元素，用 `Array.prototype.find()` 方法

查找满足条件的多个元素（过滤），用 `Array.prototype.filter()` 方法

```js
const ninjas = [
    {name: "yagyu", weapon: "shuriiken"},
    {name: "Yoshi"},
    {name: "Kuma", weapon: "wakizashi"}
]

const ninjaWithWakizashi = ninjas.find(ninja => {
    return ninja.weapon === "wakizashi"
})
// {name: "Kuma", weapon: "wakizashi"} object

const armedNinjas = ninjas.filter(ninja => "weapon" in ninja)
// [{name: "yagyu", weapon: "shuriiken"}, {name: "Kuma", weapon: "wakizashi"}] array
```

find 返回回调结果为 `true` 的那个**元素**

filter 返回由满足条件的元素组成的数组

```js
// 查找数组索引
const ninjas = ["Yaggyu", "Yoshi", "Kuma", "Yoshi"];

ninjas.indexOf("Yoshi") // 1
ninjas.lastIndexOf("Yoshi") // 3

const yoshiIndex = ninjas.findIndex(ninja => ninja === "Yoshi") // 1
// findeIndex 的条件更宽泛，可以判断大于、小于等等条件
```

**数组排序**

JavaScript 引擎实现了排序算法。我们需要提供回调函数，告诉排序算法相邻的两个数组元素的关系。

```js
array.sort((a, b) => a - b)
```

- 如果回调函数的返回值小于 0，元素 a 应该出现在元素 b 之前。
- 如果回调函数的返回值等于 0，元素 a 和 元素 b 保持位置不变。
- 如果回调函数的返回值大于 0 ，元素 a 应该出现在元素 b 之后。

**合计数组元素**

以前计算数组内的元素之和需要这样

```js
const number = [1, 2, 3, 4]
const sum = 0

numbers.forEach(number => {
    sum += number
})
```

使用 `reduce`

```js
const number = [1, 2, 3, 4]

const sum = numbers.reduce((aggregated, number) => aggretated + number, 0)
```

reduce 方法接收初始值，对数组每个元素执行回调函数。回调函数接收上一次回调结果以及当前的数组元素作为参数。

如果没有提供初始值，则将使用数组中的第一个元素作为初始值，并且 reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引

## Map

使用 Object 作为字典的缺陷：

- 带有原型继承属性
- key 仅仅支持字符串

`Map` 有效避免了这两个问题。

```js
const ninjaIslandMap = new Map();

const ninja1 = { name: "Yoshi"}
ninjaIslandMap.set(ninja1, { homeIsland: "Honshu"})

ninjaIslandMap.get(ninja1).homeIsland // "Honshu"

ninjaIslandMap.size // 1

ninjaIslandMap.has(ninja1) // true

ninjaIslandMap.delete(ninja1) // 从map中删除key
ninjaIslandMap.clear() // 清空map
```

`map` 是 键值对的集合， key 可以是任意类型的值，甚至可以是对象

可以使用 `for...of` 遍历 `map` 

```js
const directory = new Map();

directory.set("Yoshi", "18")	
directory.set("Kuma", "20")

for(let item of directory){
    // 迭代中每个元素是两个值的数组 [key, value]
    console.log("key:" + item[0])
    console.log("value:" + item[1])
}

for(let key of directory.keys()){
    console.log("key:" + key)
    console.log("value:" + directory.get(key))
}

for(let value of directory.values()){
    console.log("value:" + value)
}
```

## Set

`Set` 与 `Array` 相似，但要求集合中的**元素是唯一**的。

```js
const ninjas = new Set(["Kuma", "Hattori", "Yagyu", "Hattori"]);

ninjas.size // 3
ninjas.add("Yoshi")

ninjas.has("Yoshi") // true

ninjas.delete("Yoshi")
ninjas.clear()

// for... of 遍历
for(let ninja of ninjas){
    console.log(ninja)
}
```

:warning: `Set` 没有索引，遍历 iterator 的 `key` 和 `value` 是一样的

### Set 并集

```js
const ninjas = ["Kuma", "Hattori", "Yagyu"]
const samurai = ["Hattori", "Oda", "Tomoe"]

const warriors = new Set([...ninjas, ...samurai])
```

### Set 交集

```js
const ninjas = new Set(["Kuma", "Hattori", "Yagyu"])
const samurai = new Set(["Hattori", "Oda", "Tomoe"])

const ninjaSamurais = new Set([...ninjas].filter(ninja => samurai.has(ninja)))
// ['Hattori']
```

### Set 差集

```js
const pureNinjas = new Set([...ninjas].filter(ninja => !samurai.has(ninja)))
```

# 11. 模块化



