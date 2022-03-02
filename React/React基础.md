# 1. JSX

JavaScript 扩展语法

```jsx
const element = <h1>Hello, world!</h1>;
```

UI 和数据双向绑定

## 表达式

```react
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

大括号内放置 JavaScript 表达式

## JSX 也是一个表达式

JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

```react
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```react
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

实际上它创建了一个这样的对象：

```react
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

这些对象被称为 “React 元素”

## JSX 特定属性

使用引号指定属性值

```jsx
const element = <div tabIndex="0"></div>;
```

使用大括号插入 JavaScript 表达式

```jsx
const element = <img src={user.avatarUrl}></img>;
```

JSX 语法上接近 JavaScript，**使用 `camelCase` 定义属性名称**。

如`class` 变成了 `className`，而 `tabindex` 则变为 `tabIndex`

# 2. 元素渲染

与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。React DOM 会负责更新 DOM 来与 React 元素保持一致。

仅使用 React 构建的应用通常只有单一的根 DOM 节点，该节点内的所有内容都将由 React DOM 管理

将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 `ReactDOM.render()`：

```react
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

React 元素是不可变对象

更新 UI 唯一的方式是创建一个全新的元素，并将其传入 ReactDOM.render()

## React 只更新它需要更新的部分

React DOM 会将元素和它的子元素与它们之前的状态进行比较，只会进行必要的更新（diff算法）

# 3. 组件 & Props

组件能将 UI 拆分为独立可复用的代码片段

概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并**返回用于描述页面展示内容的 React 元素**。

## 函数组件与 class 组件

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

函数组件：本质上是 JavaScript 函数，接受唯一带有数据的 “props”（代表属性）对象并返回一个 React 元素

使用 ES6 的 class 来定义组件即是 class 组件

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Props 的只读性

组件不能修改自身的 props

### 纯函数

不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

