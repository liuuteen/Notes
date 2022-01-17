新增 CSS3 特性兼容性，`IE9+` 支持。
移动端的支持程度更好

# 1. 新增选择器

## 1. 属性

在类和id选择器的基础上多了一种选择元素的方式。

```css
E[attr]
E[attr=val]
E[attr^=val] // attr 属性且值以 val 开头的 E 元素 *
E[attr$=val] // attr 属性且值以 val 结尾的 E 元素
E[attr*=val] // attr 属性的值含有 val 的 E 元素
```

类、伪类、属性选择器的权重都为 **10**

## 2. 结构伪类

伪类选择器一开始是 **状态伪类选择器**，例如 `:hover` 鼠标悬停。

CSS3 新增了结构伪类选择器，是根据 **文档结构** 选择元素，常用于根据父级选择里面的子元素。

```css
:first-child
:last-child
:nth-child(n)
//
:first-of-type
:last-of-type
:nth-of-type(n)
```

```css
ul > :first-child{} // ul 下的第一个子元素
ul > li:first-child{} // ul 下的的第一个子元素 li
```

其他可查：[伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)

`:nth-child(n)`，`:nth-of-type(n)` 中的 **n** 取值：

- 数字
- 关键字： `odd` 奇数，`even` 偶数
- 公式： `an + b`，**n** 为 0 ~ ∞ ，公式结果第0个元素或超出了元素的个数都会忽略。变量只能用 `n`

### nth-child 和 nth-of-type 区别

只有和其他选择器交集使用时，才能说明区别。

`div:nth-child(1)` 和 `div:nth-of-type(1)`

```css
div:nth-child(1) 
所有的兄弟元素集合进行从1开始排序，选择每个集合的第一个元素，再匹配是 div 标签的元素
div:nth-of-type(1)
所有的 div 兄弟元素集合从1开始排序，选择每个集合的第一个元素
```

:nth-child 先后再前

:nth-of-type 先前再后

## 3. 伪元素

为元素选择器是通过 CSS 创建新标签元素，而不使用 `HTML` 标签，从而简化 `HTML` 结构。

| 选择符     | 简介                     |
| ---------- | ------------------------ |
| `::before` | 在元素内部的前面插入内容 |
| `::after`  | 在元素内部的后面插入内容 |

- 创建的元素属于行内元素
- 文档树中不存在，所以叫 **伪元素**
- 必须要有 `content` 属性
- 权重为 1

### 伪元素字体图标

![image-20220117222426402](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220117222426402.png)

### 实现遮罩层

```css
.shiping::before {
    content: '';
    display: none;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .4) url(img/xx.png) no-repeat center;
}

.shiping:hover::before {
    display: block;
}
```

