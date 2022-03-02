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