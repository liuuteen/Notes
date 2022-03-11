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

`Context` 可以隔层级传递数据。提供方便的组件之间共享数据的方式

```react
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。
const ThemeContext = React.createContext('light');
class App extends React.Component {
    render() {
        // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。    
        // 无论多深，任何组件都能读取这个值。
        // 在这个例子中，我们将 “dark” 作为当前的值传递下去。    return (
        <ThemeContext.Provider value="dark">        
            <Toolbar />
        </ThemeContext.Provider>
        );
    }
}

// 中间的组件再也不必指明往下传递 theme 了。
function Toolbar() {  
    return (
        <div>
            <ThemedButton />
        </div>
    );
}

class ThemedButton extends React.Component {
    // 指定 contextType 读取当前的 theme context。  
    // React 会往上找到最近的 theme Provider，然后使用它的值。  
    // 在这个例子中，当前的 theme 值为 “dark”。  
    static contextType = ThemeContext;
    render() {
        return <Button theme={this.context} />;
    }
}
```

1. *`React.createContext`：*创建一个装上下文的容器（组件），`defaultValue`可以设置需要共享的默认数据
2. *`Context.Provider`：*提供者，用于提供共享数据的地方，`value`属性设置什么数据就共享什么数据
3. *`Context.Consumer`：*消费者，专门消费`Provider`提供的共享数据，`Consumer`需要嵌套在`Provider`下面，才能通过回调的方式拿到共享的数据。<u>只有函数组件会用到。</u>
4. *`Context.Consumer`：*消费者，专门消费`Provider`提供的共享数据，`Consumer`需要嵌套在`Provider`下面，才能通过回调的方式拿到共享的数据。<u>只有函数组件会用到。</u>
5. *`Context.displayName`：*context对象接收一个名为`displayName`的属性，类型为字符串。`React DevTools`使用该字符串来确定context要显示的内容(暂时还没用到)

## 谨慎使用

会使得组件的复用性变差；

想要**避免层层传递一些属性**，组件组合或许不错。