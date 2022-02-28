新增 `CSS3` 特性兼容性，`IE9+` 支持。
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

`CSS3` 新增了结构伪类选择器，是根据 **文档结构** 选择元素，常用于根据父级选择里面的子元素。

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

伪元素选择器是通过 `CSS` 创建新标签元素，而不使用 `HTML` 标签，从而简化 `HTML` 结构。

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

### 伪元素清除浮动

原理：使用了额外标签法（在浮动元素后面加一个空的块级元素）

**单伪元素**

```css
.clearfix:after {
	content: "020";
	display: block;
	height: 0;
	clear: both;  // 核心代码
	visibility: hidden;
}
```

**双伪元素**

```css
.clearfix:before,
.clearfix:after {
    content: "";
    display: table; // 让块级元素一行显示
}
.clearfix:after {
    clear: both;
}
```

# 2. `CSS3` 盒子模型

`box-sizing: content-box | border-box` 

计算盒子大小方式：

- content-box：默认，盒子大小 **width + padding + border**

- border-box：盒子大小就为 **width**  :star:

# 3. 其他特性 （了解）

## 1. `calc `函数

可以计算 `css` 属性值。例如：

```css
width: calc(100% - 30px);
```

支持 `+，-，*，/`

## 2. 滤镜 filter

`filter` 属性将**模糊**或**颜色偏移**等图形效果应用于元素。

```css
filter: 函数();  //  filter: blur(5px);
```



# 4. `CSS3` 过渡 `transition` :star2:

```css
transition: 要过渡的属性 花费时间 运动曲线 何时开始;
```

1. **属性：**变化的 `css` 属性，所有的则为 `all`
2. **花费时间：** 秒（必填） 如 `.5s`
3. **运动曲线：** 默认 `ease`(可省略)
4. **何时开始：** 单位秒必写，触发的延迟时间，默认 `0s`（可省略）

哪个元素需要过渡，就给这个元素加`transition` 属性

对多个属性过渡，只需要用逗号分隔

```css
transition: width .5s ease 1s,
		   height .5s ease 1s;
// 对所有属性添加
transition: all .5s;
```

# 5. `CSS3` `2D` 转换

转换 `transform` 可以实现元素的位移、旋转、缩放等效果

- 移动：translate
- 旋转：rotate
- 缩放：scale

## 移动 `translate`

```css
transform: translate(x, y);
transform: translateX(n);
transform: translateY(n);
```

### 优点

- **不影响其他元素的位置**

- 百分比相对于元素自身的宽高。
- 对行内元素无效

## 旋转 `rotate`

```css
transform: rotate(度数)
```

- 单位 `deg` ，degree 度数
- 中心点为元素中心点（默认 `transform-origin: 50% 50%`）

### 可以通过 `rotate` 实现三角效果

```css
div::after {
    content: '';
    position: absolute;
    right: 5px;
    top: 5px;
    width: 5px;
    height: 5px;
    border-right: 1px solid #000;
    border-bottom: 1px solid #000;
    transform: rotate(45deg);
}
```

`transform-origin: x y;` 可以修改中心点

x，y可以为像素或者方位名词 `trblc` (top right bottom left center)

## 缩放 `scale`

```css
transform: scale(x, y);
transform: scale(1, 1); // 结果是 1*width, 1*height
transform: scale(2); // x y 都为2
```

缩放不影响其他元素

`transform-origin: x y;` 可以修改中心点

## 综合写法

```css
transform: translate() rotate() scale();
```

**顺序会影响转换的效果**（旋转会改变坐标轴的方向）

综合写法时最好的办法是位移放最前面

# 6. `CSS3` 动画 `animation` 

 可以通过 `@keyframes` 设置多个节点实现动画

对比过渡，动画能实现更多的变化，更多控制，还可以连续自动播放等。

制作动画：

1. 定义动画
2. 使用动画

## `@keyframes` 定义动画

```css
@keyframes 动画名称 {
    0% {
        width: 100px;
    }
    100% {
        width: 200px;
    }
}
div {
    animation-name: 动画名称;
    animation-duration: 持续时间;
}
```

动画序列可以用百分比规定节点，也可以用 `from 和 to，等同 0% 和 100%`

## 简化写法

```css
div {
    /* 可以同时添加多个动画 */
    animation: ani1 1s ease infinity,
        	   ani2 2s ease infinity;  
}
```



# 7. `CSS3` `3D` 转换

使用 `transform` 属性

主要内容：

- `3D` 位移：`translate3d(x, y, z);` :star:
- `3D` 旋转：`rotate3d(x, y, z);` :star:
- 透视：`perspective`
- `3D` 呈现：`transform-style`

## 透视 `perspective`

透视用来产生 `3d` 效果，模拟眼睛的位置，也叫做视距。

**透视写在被观察元素的父盒子上**

```css
div {
    perspective: 500px; /* 不推荐使用 */
}
```

现在应该是用的

```css
div {
    transform: perspective(200px);
}
```

`d`：视距

`z`：z轴

## `3d` 移动

```css
transform: translateZ();
transform: translate3d(x, y, z);
```

## `3d` 旋转

```css
transform: rotateX(deg);
transform: rotateY(deg);
transform: rotateZ(deg);  /* 和 rotate()效果一样 */
transform: rotate3d(x, y, z, deg); /* x, y, z 取 0-1， 矢量角度 */
```

## `3d` 呈现

- 控制子元素是否开启三位立体环境。
- `transform-style: flat;` 不开启，默认
- `transform-style: preserve-3d;` 开启
- 属性要给父级

## `backface-visibility`

控制元素的背面是否显示。` visible | hidden` 

