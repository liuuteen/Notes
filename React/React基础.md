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

React DOM 会将元素和它的子元素与它们之前的状态进行比较，只会进行必要的更新（虚拟DOM 和 diff算法）

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

# 4. State & 生命周期

state 是私有的，并且完全受控于当前组件

**class 组件才能使用 state**（暂时不考虑 hooks）

组件第一次被渲染到 DOM 中的时候，就为其**设置一个计时器**。这在 React 中被称为“挂载（mount）”。

当 DOM 中组件被删除的时候，应该**清除计时器**。这在 React 中被称为“卸载（unmount）”。

## 生命周期方法

为 class 组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
  }

  componentDidMount() {
  }

  componentWillUnmount() {
  }

  render() {}
}
```

## 使用 State

应该使用 `setState()` 修改 `state` ; 并且初始化 this.state 应该在构造函数内

出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用；因为 `this.props` 和 `this.state` 可能会是异步更新的。不能依赖值来更新下一个 `state` 。

```react
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// 可以传入回调函数来解决问题
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

### State 的更新会被合并

合并是浅合并

## 数据是向下流动的（单向数据流）

组件无法无法知道其他组件是有状态的还是无状态的

组件可以选择把它的 state 作为 props 向下传递到它的子组件中

从 state 派生的数据或 UI 只能影响后代组件

# 5. 事件处理

1. 与 DOM 事件处理语法不同：

- 事件命名小驼峰 onClick
- 传入函数

2. 阻止默认行为

   不能通过返回 `false` ，只能显示调用 `preventDefault`

建议在构造器中绑定或使用 class fields 语法绑定事件处理函数的 `this`

# 6. 条件渲染

使用 JavaScript 运算符 `if` 或者`条件运算符`去创建 react 元素

可以使用变量来储存 react 元素

## 内联条件渲染

### 与运算符 &&

通过花括号包裹代码，你可以在 `JSX `中嵌入任何表达式

在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`

### 三元运算符

```react
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

如果条件变得过于复杂，那你应该考虑如何提取组件

## 阻止组件渲染

可以让 `render` 方法直接返回 `null`; 组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期

# 7. 列表 & Key

## Key 的作用

key 帮助 React 识别哪些元素改变了，比如被添加或删除

一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串

当元素没有确定 id 的时候，万不得已你可以使用元素索引 index 作为 key

如果列表项目的顺序可能会变化，使用索引来用作 key 值会导致性能变差，还可能引起组件状态的问题

## 用 key 提取组件

元素的 key 只有放在就近的数组上下文中

```react
  const listItems = numbers.map((number) =>
    // 正确！key 应该在数组的上下文中被指定
    <ListItem key={number.toString()} value={number} />
  );
```

key 会传递信息给 React ，但不会传递给你的组件。如果组件中需要使用 `key` 属性的值，需要使用其他属性名显式传递这个值

```react
const content = posts.map((post) =>
  <Post
    key={post.id}    
    id={post.id}
    title={post.title}
  />
);
```

`Post` 组件可以读出 `props.id`，但是不能读出 `props.key`。

# 8. 表单

## 8.1 受控组件

渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

对于受控组件来说，输入的值始终由 React 的 state 驱动

