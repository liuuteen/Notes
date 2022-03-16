# 1. 代码分割

## 1.1 打包

打包是一个将文件引入并合并到一个单独文件的过程，最终形成一个 “bundle”。 接着在页面上引入该 bundle，整个应用即可一次性加载。

 代码分割是由诸如 [Webpack](https://webpack.docschina.org/guides/code-splitting/)，[Rollup](https://rollupjs.org/guide/en/#code-splitting) 和 Browserify（[factor-bundle](https://github.com/browserify/factor-bundle)）这类打包器支持的一项技术，能够创建多个包并在运行时动态加载

能够**“懒加载”当前用户所需要的内容，提高应用的性能，避免加载用户永远不需要的代码，在初始加载时减少加载的代码量**

## 1.2 import()

动态 `import()` 语法

```react
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

当 Webpack 解析到该语法时，会自动进行代码分割

# 2. Context

`Context` 可以**往下隔层级传递数据**，不必每一层都写一遍 props。提供方便的组件之间共享数据的方式

1. *`React.createContext`：*创建一个装上下文的容器（组件），`defaultValue`可以设置需要共享的默认数据
2. *`Context.Provider`：*提供者，用于提供共享数据的地方，`value`属性设置什么数据就共享什么数据
3. *`Context.Consumer`：*消费者，专门消费`Provider`提供的共享数据，`Consumer`需要嵌套在`Provider`下面，才能通过回调的方式拿到共享的数据。<u>只有函数组件会用到。</u>
4. *`Class.contextType`：*记住是用于指定`contextType`等于当前的`context`，必须指定，才能通过`this.context`来访问到共享的数据。<u>只有类组件会用到。</u>
5. *`Context.displayName`：*context对象接收一个名为`displayName`的属性，类型为字符串。`React DevTools`使用该字符串来确定context要显示的内容(暂时还没用到)

上层组件提供数据，下层组件消费/读取数据；来一个 `demo`

> 此demo是通过工具栏中的按钮来切换主题色

1. 首先，创建一个`theme-context.js`文件，用于**创建一个装上下文的容器**

```jsx
// theme-context.js
import React from 'react';
export const themes = {
    light: {
        foreground: '#000',
        background: '#eee'
    },
    dark: {
        foreground: '#fff',
        background: '#222'
    }
}
// Step1： 创建一个装上下文的容器
export const ThemeContext = React.createContext(
    themes.dark // defaultValue 默认值
)
```

2. 在顶层组件设置需要共享的数据

```jsx
// App.js
import React, { Component } from 'react';
import { themes, ThemeContext } from './theme-context.js';
import ToolBar from './component/Toolbar'
export default class App extends Component {
    constructor(props) {
        super(props)
        this.state = {
            theme: themes.dark
        }
    }
    
    toggleTheme = () => {
        this.setState(state => ({
            theme: state.theme === themes.dark ? themes.light : themes.dark
        }))
    }
    
    render() {
        return (
            <div>
                <ThemeContext.Provider value={this.state.theme}> {/* Step2: 在顶层组件设置需要共享的数据, 到时候在ToolBar里面会用到*/}
                    <ToolBar changeTheme={this.toggleTheme}/>
                </ThemeContext.Provider>
            </div>
        )
    }
}
```

3. 子组件使用（消费）context（类组件和函数式组件对应两种方式）

```jsx
// ToolBar.js
import React, { Component } from 'react';
import { ThemeContext } from './theme-context.js'

1. 如果子组件是类组件,需要指定contextType等于当前的context（也是两种方式）
export default class ToolBar extendx Component {
    // static contextType = ThemeContext // 方式1：在class组件中声明静态属性static
    render() {
        let theme = this.context; 
        return (
            <div style={{ border: '1px solid #000', background: theme.background }}>
                <h2 style={{ color: theme.foreground }}>ToolBar</h2>
                <ThemeButton onClick={this.props.changeTheme}>
                    Change Theme
                </ThemeButton>
            </div>
        )
    }
}
ToolBar.contextType = ThemeContext; // 方式2： 在class组件外面指定
-------------------------------------------------------------------------------------
2. 如果子组件是函数式组件，需要用Consumer组件来包裹，通过value拿到数据
export default function ToolBar(props) {
    return (
        <ThemeContext.Consumer>
            {theme => (
                <div style={{ border: '1px solid #000', background: theme.background }}>
                    <h2 style={{ color: theme.foreground }}>ToolBar</h2>
                    <ThemeButton onClick={props.changeTheme}>
                        Change Theme
                    </ThemeButton>
                </div>
            )}
        </ThemeContext.Consumer>
    )
}

// ThemeButton.js中要使用共享数据也是同理，这里贴出ThemeButton.js的代码
// ThemeButton.js
import React, { Component } from 'react';
import { ThemeContext } from './theme-context.js';

export default function ThemeButton(props) {
    return (
        <ThemeContext.Consumer>
            {theme => (
                <button 
                    {...props}
                    style={{background: theme.background, color: theme.foreground}}
                    ></button>
            )}
        </ThemeContext.Consumer>
    )
}
```

## 谨慎使用

会使得组件的复用性变差；

想要**避免层层传递一些属性**，组件组合或许不错。

# 3. 错误边界（Error Boundaries）

React 16+ 引入的概念，部分 UI 不应该导致整个应用崩溃

错误边界是一种 React 组件，**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

> 错误边界无法捕获以下场景中产生的错误：
>
> - 事件处理
> - 异步代码
> - 服务器端渲染

**如何生成错误边界**

给 class 组件定义 `static getDerivedStateFromError()` 或 `componentDidCatch()` 任意一个（或两个）生命周期方法，既为错误边界组件。

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
      super(props);
      this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {    
      // 更新 state 使下一次渲染能够显示降级后的 UI    
      return { hasError: true };
  }
  componentDidCatch(error, errorInfo) {
      // 你同样可以将错误日志上报给服务器
      logErrorToMyService(error, errorInfo);
  }
  render() {
    if (this.state.hasError) {      
        // 你可以自定义降级后的 UI 并渲染      
        return <h1>Something went wrong.</h1>;
    }
    return this.props.children; 
  }
}
```

```jsx
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

# 4. Refs 转发

