---
title: "CSS Grid 布局"
date: 2019-10-11T10:49:22+08:00
draft: false
categories: ['CSS与布局']
---

![Grid兼容性](/img/posts/grid.png)

[推荐教程：阮一峰](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

**妈耶~好长~**

## 术语  

**容器**和**项目**

容器就是外面的壳儿~，项目就是它的孩儿（第一层子元素），孙子不算哦~~~

**网格线**

m行n列，就有m+1个水平网格线和n+1个垂直网格线

## CSS属性

CSS属性有两种：容器属性 和 项目属性

## 容器属性

### display

1. grid：指定一个容器采用网格布局，当然这个容器时块级元素。
2. inline-grid：将块级元素变为行内的元素，采取网格布局。

“注意，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效。”

### grid-template-rows/columns

划分行高和列宽  

```css
/* 3行3列 */
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
/* 也可以使用百分比 */
/*
    如果行列很多的话，这样会比较麻烦，可以使用repeat()
    repeat(n, item) 重复item n次
*/
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```
#### repeat关键字  
```css
/* 当然，item也可以是某种模式，比如： */
grid-template-columns: repeat(2, 100px 20px 80px);
/* 这样的话会出现6列，按上面的次序重复排列，但是如果超出容器的宽度呢，
实际上可以给容器设置宽度，但是对内部的项目无效，即使容器的宽度被设为10，
项目也会按照6列排列 */
```
#### auto-fill关键字  
```css
/* n 也不止是次数，也可以是 auto-fill 关键字 ，该关键字表明会自动排列，
即容器只要有空间就会一直排下去，直到换行。*/
grid-template-columns: repeat(auto-fill, 200px)
```

#### fr 关键字  
```css
/* fr关键字表示比例关系，如下，第二列宽度是第一列的2倍 */
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
}
/* 当然也可以和长度单位结合使用，如下，第二列和第三列会自动按比例分配剩下的空间。*/
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

#### minmax()

minmax(最小值，最大值)函数产生一个长度范围。值可为像素，可为fr，可为百分比。

#### auto关键字  

表示由浏览器自己决定长度，基本上等于单元格的最大宽度，除非单元格设置了min-width，且大于单元格的最大宽度。

#### 网格线的名称  

grid-template-columns 和 grid-template-rows还可以设置每条网格线的名称：

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

网格布局允许同一根线有多个名字，比如[fifth-line row-5]。






### grid-row/column-gap 行/列间距  

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

### grid-gap 上面两个属性的结合

```css
.container {
  grid-gap: 20px 20px;  /* row/column */
}
/* 如果grid-gap省略了第二个值，浏览器认为第二个值等于第一个值 */
```

### grid-template-areas

指定区域，一个区域由单个或多个单元格组成。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
/* 上面代码划分了9个单元格，对应了 a - i 9个区域 */
/* 当然也可以合并多个单元格，例如： */
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';
/* 上面代码将九个单元格划分成了a、b、c三个区域。 */
grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";
/* 不需要的区域可以使用 . 来代替 */
grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';
```

### grid-auto-flow

容器项目的排列顺序，默认值是 row，即先行后列，也可以设置为column，即先列后行。
```css
grid-auto-flow: column/row;
```

这个属性还有第二个值，即dense，决定某些项目指定位置以后，剩下的项目怎么自动放置，例如：  
![]()

### justify-items && align-items

一个控制单元格内容的水平位置，一个控制单元格内容的垂直位置。

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
/* stretch：拉伸，占满单元格的整个宽度（默认值） */
```

place-items 属性是上面两个的复合形式：
```css
place-items: <align-items> <justify-items>; /* 如果省略第二个值，则浏览器认为与第一个值相等 */

### justify-content && align-content

一个控制整个内容区域在容器内的水平位置，一个控制整个内容区域在容器内的垂直位置。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
/* stretch：拉伸，占满单元格的整个宽度（默认值） */
/* space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔 */
```

place-content 属性是上面两个的复合形式：
```css
place-content: <align-content> <justify-content>; /* 如果省略第二个值，则浏览器认为与第一个值相等 */
```

### grid-auto-columns && grid-auto-rows

当项目出现在所设置的行或列之外时，比如3X3，结果项目比较多，出现在了第四行，但没有给它设置行高，所以浏览器会自动设置多余的单元格，以便放置项目。这两个属性就是来设置多余的网格的行高和列宽的。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-rows: 50px; 
}
```

## 项目属性  

### （grid-column-start && grid-column-end）&&（grid-row-start && grid-row-end）

指定项目的位置，就是指定项目的四个边，即四个网格线。

```css
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
/* 这个项目在水平方向上以第二个网格线开始，在第四个网格线结束，即占据了两个单元格。 */
/* 它们的值除了数字，还可以是之前设置的网格线名 */
.item-1 {
  grid-column-start: span 2;
}
/* 也可以这么用，使用span关键字，代表跨越，即项目的左右（上下）网格线之间跨越几个单元格 */
```

### grid-column && grid-row

是上面属性的合并写法：

```css
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 3;
}
/* 等同于 */
.item-1 {
  grid-column: 1 / span 2;
  grid-row: 1 / span 2;
}
```

### grid-area

指定项目的放置区域：

```css
.item-1 {
  grid-area: e; /* 放置在e区域 */
}
/* 也可以 */
.item {
  /* grid-area: <row-start> / <column-start> / <row-end> / <column-end>; */
  grid-area: 1/1/3/3;   /* 占满4个单元格 */
}
```

### justify-self && align-self && place-self

设置项目在单元格内的位置

```css
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```






