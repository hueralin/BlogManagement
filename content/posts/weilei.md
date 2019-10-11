---
title: "伪类 与 伪元素"
date: 2019-08-21T15:17:26+08:00
draft: false
categories: ['CSS与布局']
---

> “请说一下伪类和伪元素的区别”，之前的面试中遇到过这个问题，当初表示只见过伪类，回答的基本上都是:hover之流，这道题就挂了......

如今，在实习中遇到了相关的需求，看来是时候深入学习一下了。其实，区分伪类和伪元素并不难，它俩最大的区别就在于应用场景。

### 伪类  
#### 顾名思义，它相当于一个类，是用来设置样式的；当你需要让一个元素在某种特殊状态下显示特殊的样式时（如：按钮悬浮特效），就该使用伪类。

**下面我们来实现一下伪类：**

需求：实现消息列表悬浮特效  
```html
<!-- 需求很明确，li元素在悬浮情况下应用样式 -->
<ul>
  <li><a href="#">React</a></li>
  <li><a href="#">Vue</a></li>
  <li><a href="#">Angular</a></li>
  <li><a href="#">jQuery</a></li>
  <li><a href="#">Webpack</a></li>
  <li><a href="#">NPM</a></li>
</ul>
li:hover {
    background: #eee;
}
<!-- 再简单不过了，当然也可以设置奇偶行样式不一 -->
```

其他的伪类：
```css
:active :empty  :enabled    :first  :first-child    :first-child  
:first-of-type  :focus  :hover  :visited    :nth-child  :nth-of-type    :link
```

### 伪元素  
#### 顾名思义，它是一个元素，可以是现有的元素，也可以是新增的元素，这些元素往往有自己的特殊位置（如：段落首字母突出样式）往往被作为附加的标记。  

**下面我们来实现一下伪元素**  

需求：表单必填项前面加一个标识（这也是我这次的需求）  
```html
<label>页面地址</label>
<input type='text' placeholder="请输入页面地址"/>
label::before {
    content: '*';
    color: red;
}
```  
效果：![必填项前加星号](/img/posts/xinghao.jpg)  

伪元素并不多：  
1. ::before  
2. ::after  
3. ::first-letter 段落首字母  
4. ::first-line 段落首行  
5. ::selection 这个很有意思，代表被用户高亮的部分（例如鼠标选中）  

### ::before 和 ::after  
这两个伪元素比较特殊，属于扩增原元素一类（我自己瞎起的名字）  
::before 创建一个伪元素，并作为匹配元素的第一个子元素，::after则相反；这个新增的元素默认为“行内元素”  
语法参照[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before)  
```css
/* CSS3 语法 */
element::before { 样式 }  
/* （单冒号）CSS2 过时语法 (仅用来支持 IE8) */
element:before  { 样式 }  
/* 在每一个p元素前插入内容 */
p::before { content: "Hello world!"; } 
```  
MDN上关于这一节的最后一个例子不错，大家可以尝试一下！