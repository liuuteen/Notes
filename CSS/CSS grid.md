# 一、 概述

网格布局（`Grid`）是最强大的 `CSS` 布局方案。

它将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。以前，只能通过复杂的 CSS 框架达到的效果，现在浏览器内置了。

![img](CSS grid.assets/1_bg2019032501.png)

Grid 布局与 Flex 布局有一定的相似性，都可以指定容器内部多个项目的位置。但是，它们也存在重大区别。

Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是**一维布局**。Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。Grid 布局远比 Flex 布局强大

# 二、 基本概念

## 2.1 容器和项目

采用网格布局的区域，称为"容器"（container）。容器内部采用网格定位的子元素，称为"项目"（item）。

```html
<div>
  <div><p>1</p></div>
  <div><p>2</p></div>
  <div><p>3</p></div>
</div>
```

最外层 `<div>` 是容器，内层三个 `<div>` 元素是项目。

项目只能是容器的顶层子元素；Grid 布局只对项目生效

## 2.2 行和列

容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）。

![img](CSS grid.assets/1_bg2019032502.png)

## 2.3 单元格

行和列的交叉区域，称为"单元格"（cell）

正常情况下，`n`行和`m`列会产生`n x m`个单元格。比如，3行3列会产生9个单元格。

## 2.4 网格线

划分网格的线，称为"网格线"（grid line）。水平网格线划分出行，垂直网格线划分出列。

正常情况下，`n`行有`n + 1`根水平网格线，`m`列有`m + 1`根垂直网格线，比如三行就有四根水平网格线。

![img](CSS grid.assets/1_bg2019032503.png)

# 三、容器属性

Grid 布局的属性分成两类：

1. 定义在容器上面，称为容器（container）属性
2. 定义在项目上面，称为项目（item）属性

## 3.1 display 属性

`display: grid`指定一个容器采用网格布局。

```css
div {
  display: grid;
}
```

![img](CSS grid.assets/bg2019032504.png)

默认情况下，容器元素都是块级元素，但也可以设成行内元素。

```css
div {
  display: inline-grid;
}
```

![img](CSS grid.assets/bg2019032505.png)

> 注意，设为网格布局以后，容器子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

## 3.2 grid-template-columns / grid-template-rows

容器需要划分行和列。

`grid-template-columns` 属性定义每一列的列宽
`grid-template-rows`      属性定义每一行的行高

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```

指定了一个三行三列的网格，列宽和行高都是`100px`；

除了使用绝对单位 `100px`，还能使用百分比 `40%` 和 `3em`;

### （1）repeat()

`repeat()`函数，可以简化重复的值

```css
.container {
    display: grid;
    grid-template-columns: repeat(5, 20%);
    grid-template-rows: 20% 20% 20% 20% 20%;
}
```

`repeat()`还可以重复某种模式

```css
grid-template-columns: repeat(2, 10px 20px 30px);  /* 10px 20px 30px 10px 20px 30px*/
```

### （2）auto-fill 关键字

如果单元格大小固定，容器大小不确定，可以使用`auto-fill`关键字表示自动填充列 / 行数。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

每列宽度`100px`，自动填充，直到容器不能放置更多的列

### （3）fr 关键字

`fr `关键字（fraction 的缩写，意为"片段"）

如果两列的宽度分别为 `1fr` 和 `3fr` ; 那么第一列占据 总宽度的 `1/4`，第二列占据总宽度的 `3/4`

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
}
```

​	![image-20220308162020631](CSS grid.assets/image-20220308162020631.png)

当有列被设置为像素 `px` ，百分比 `%`，或 `em`，其他用 `fr` 关键字设置的列就会分摊剩下的空间。

### 简写属性 grid-template

结合了 `grid-template-rows` 和 `grid-template-columns`

```css
#garden {
    display: grid;
    grid-template: 60% 40% / 200px;
}
```

上面代码表示 两行（`60% 40%`）一列（`200px`）。

## 3.3 grid-row-gap 属性， grid-column-gap 属性， grid-gap 属性

`grid-row-gap`属性设置行与行的间隔（行间距），`grid-column-gap`属性设置列与列的间隔（列间距）。

```css
grid-gap: <grid-row-gap> <grid-column-gap>;
```

如果`grid-gap`省略了第二个值，浏览器认为第二个值等于第一个值

根据最新标准，三个属性名的`grid-`前缀已经删除，`grid-column-gap`和`grid-row-gap`写成`column-gap`和`row-gap`，`grid-gap`写成`gap`。

## 3.4 grid-auto-flow 属性

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"

`grid-auto-flow`属性决定，默认值是`row`。也可以将它设成`column`，变成"先列后行"

还可以设成`row dense`和`column dense` ;决定剩下的项目如何排列。

## 3.5
justify-items 属性，
align-items 属性，
place-items 属性

`justify-items`属性设置单元格内容的水平位置（左中右），`align-items`属性设置单元格内容的垂直位置（上中下）。

- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）。

```css
place-items: <align-items> <justify-items>;
```

## 3.6 justify-content 属性， align-content 属性， place-content 属性

`justify-content`属性是**整个内容区域**在容器里面的水平位置（左中右），`align-content`属性是整个内容区域的垂直位置（上中下）。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```



```css
place-content: <align-content> <justify-content>
```

# 四、项目属性

## 4.1 grid-column-start 属性， grid-column-end 属性， grid-row-start 属性， grid-row-end 属性

- `grid-column-start`属性：左边框所在的垂直网格线
- `grid-column-end`属性：右边框所在的垂直网格线
- `grid-row-start`属性：上边框所在的水平网格线
- `grid-row-end`属性：下边框所在的水平网格线

这四个属性的值还可以使用`span`关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。

如果产生了项目的重叠，则使用`z-index`属性指定项目的重叠顺序

## 4.2 grid-column 属性，
grid-row 属性

```css
.item {
  grid-column: <start-line> / <end-line>;
  grid-row: <start-line> / <end-line>;
}
```

这两个属性之中，也可以使用`span`关键字，表示跨越多少个网格。

## 4.3 grid-area 属性

```css
.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

## 4.4 justify-self 属性，
align-self 属性，
place-self 属性

`justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

`align-self`属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。

```css
place-self: <align-self> <justify-self>;
```

# 总结

## 项目

```css
grid-column-start: 1;  /* 网格序号 也可以 -1 */
grid-column-end: 5;
grid-row-start: -1;
grid-row-end: -5;

grid-column: 1 / 5; /* grid-column-start / grid-column-end */
grid-row: -1 / -5;

grid-area: -1 / 1 / -5 / 5;
/* grid-row-start / grid-column-start / grid-row-end / grid-row-start  */
/* 以上属性都可以 span 代表网格跨度 */
grid-column: 1 /span 4;
```

项目可以重叠，重叠可以通过 `z-index` 修改层叠顺序

项目 `order`  可以调整项目的序号，数字越大越靠后；可以负数

## 容器

`grid-template-columns、grid-template-rows`  划分行列

```css
grid-template-columns: 20% 20% 50px 3em;
```

取值 绝对值 `50px` 、百分比 `50%` （相对于容器宽高）、相对值 `3em` （相对于自身文本大小）

可以用 `repeat(5, 20%)` 函数来简写。

```css
grid-template-columns: repeat(2, 50%);
```

### **fr 关键字**

分摊空间

```css
grid-template-columns: 1fr 3fr; /* 两列 第一列占 1/4 第二列占 3/4；
```

如果设置行高时有用 `px` `%` , `em`；那么 fr 分摊剩下空间。

```css
grid-template-columns: 50px 1fr 3fr;
/* 三列 第一列 50px， 第二列 (width - 50px) * (1/4) , 第三列 (width-50px) * (3/4)
```

### grid-template 简写

```css
grid-template: 60% 40% / 200px; /* 两行 一列 */
/* grid-template-rows / grid-template-columns */
```



