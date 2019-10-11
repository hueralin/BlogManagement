---
title: "清除浮动"
date: 2019-10-09T15:16:09+08:00
draft: false
categories: ['CSS与布局']
---

> 清除浮动是个老生常谈的问题了，面对这个问题我们往往能说出一两个解决方法，但你是否知道其背后的原理呢？《CSS世界》这本书给出了很好的解答，现推荐下张鑫旭近10年前的博客。  
[CSS float浮动的深入研究、详解及拓展(一)](https://www.zhangxinxu.com/wordpress/2010/01/css-float%e6%b5%ae%e5%8a%a8%e7%9a%84%e6%b7%b1%e5%85%a5%e7%a0%94%e7%a9%b6%e3%80%81%e8%af%a6%e8%a7%a3%e5%8f%8a%e6%8b%93%e5%b1%95%e4%b8%80/)  
[CSS float浮动的深入研究、详解及拓展(二)](https://www.zhangxinxu.com/wordpress/2010/01/css-float%e6%b5%ae%e5%8a%a8%e7%9a%84%e6%b7%b1%e5%85%a5%e7%a0%94%e7%a9%b6%e3%80%81%e8%af%a6%e8%a7%a3%e5%8f%8a%e6%8b%93%e5%b1%95%e4%ba%8c/)  
<a href='#4'>别废话!切入正题!</a>

### <a href='#1' name='1'>清除浮动的原因</a>  

浮动布局时不小心高度塌陷了呗~

### <a href='#2' name='2'>浮动的真正目的</a>  

浮动的出现仅仅是为了让**文字围绕图片**而已，就像WORD中的“文字环绕”一样。

### <a href='#3' name='3'>浮动的原理</a>

在张鑫旭的两篇文章中将浮动的本质定义为“包裹与破坏”。  

**<a href='#31' name='31'>1. 浮动的包裹性</a>**  

浮动就是个带有方位的display: inline-block; inline-block就是让一个块级盒子变成一个内联盒子，不让其独占一行，而是将其内容紧紧地包裹起来。而在块级盒子上应用了浮动，你会发现其确实变成了一个内联盒子。

**<a href='#32' name='32'>2. 浮动的破坏性</a>**  

结合“浮动的本职工作是实现文字环绕图片”来说明其**“破坏性”**。

文字之所以会环绕含有float属性的图片时因为**浮动破坏了正常的line boxes**  

**<a href='#33' name='33'>line box模型</a>**:  

```html
<p>float的本职工作是实现<em>文字环绕图片</em></p>

<!-- 
    上面代码包含四个box
    1. inline box：‘float的本职工作是实现’是匿名的inline box，‘<em>文字环绕图片</em>’是inline box。
    2. line box：一个个的inline box排成一行就是一个line box，多行则有多个line box。
    3. content area：在我理解就是内容区域包含上面所有的一种box。
    4. <p>标签所在的containing box，此box包含了其他的boxes。
    以上3，4解释不够严谨或有错误，可不做参考，日后修正。
-->
```

默认无浮动的图文混排是这个样子的（借用下图片）：

![](https://image.zhangxinxu.com/image/blog/201001/2010-01-20_230801.png)  

图片本身就是个inline box，和两侧的inline box共同组成了line box，line box的高度由最高的inline box决定，第二行文本属于另一个line box，且一张图片只能与一个line box对其。

给图片加了浮动之后：

![](http://image.zhangxinxu.com/image/blog/201001/2010-01-20_234149.png)  

图片不再和其他的inline box在一块了，可见浮动使图片丧失了inline box特性。而**丧失了inline box特性的后果就是高度塌陷**。为什么呢？  

在CSS中，所有的高度都是有两个CSS模型产生的。box模型，line box模型。

**box模型**：height + padding + margin  

**line box模型**：line-height

HEIGHT（line box） = HEIGHT（inline box）* n  

HEIGHT（containing box） = HEIGHT（line box）* n  即div或p标签的高度。  

而一旦没了inline box特性，这个高度势必大打折扣。要么完全塌陷，要么由其他inline box的高度顶替。

个人认为，line boxes掌管的是box内容的高度。而一个元素真正的高度由box代表的外层高度和line box代表的内层高度组成。

我们要实现文字环绕就必须使图片和多行文本对齐，而如果不能打破inline box特性，就只能和一行文本对齐。所以浮动一定要打破inline box特性。

浮动的元素并没有脱离文档流，所以其他文本（inline box）并不会与之发生重合。并且，丧失inline box特性仅仅让其**不在容器中占据高度**，其自身高度还是有的，而且在容器中还占据着宽度，这就实现了文本绕它而行，给围了起来。

接下来说包裹性，实际上浮动只是将元素变成了类似inline-box的样式，只保留了其包裹性，而inline box的特性被抛弃了。  

下面我们来看一下张鑫旭大神给个例子：

1. 单个无浮动的li  
![](http://image.zhangxinxu.com/image/blog/201001/2010-01-21_202917.png)

2. 单个左浮动的li  
![](https://image.zhangxinxu.com/image/blog/201001/2010-01-21_205119.png)  
呈现了包裹性。

3. 多个左浮动的li  
![](https://image.zhangxinxu.com/image/blog/201001/2010-01-21_214516.png)  
呈现了包裹性，于是排列成一行。

4. 一个左浮动一个无浮动  
![](https://image.zhangxinxu.com/image/blog/201001/2010-01-21_230302.png)  
第一个li因为浮动导致其“上浮”，为什么会“上浮”？因为它失去了inline box的特性，没了高度，第二个li顶了上来，呈现了第一个li上浮的假象。即便“上浮”，但并没有覆盖掉下面的li内容，因为浮动并没有使其内容脱离文档流，第一个li中的图片仍占据着空间，导致第二个li的图片紧贴在第一张图片的后面（第二个li没有浮动，其宽度是100%）。

原理部分就讲那么多，其实原文牵扯了不少内容，我总结引用的也不是很好。下面来终于到了重点内容！  

### <a href='#4' name='4'>如何清除浮动，解决高度塌陷</a>  

高度为什么会塌陷？  

浮动使元素变成了inline-box，然后破坏其inline box特性，导致line box没了高度，进而使contain box（div、p等的内容高度）没了高度，于是塌陷。  

**1.最简洁的方法**  

```html
<!-- 将下面代码放到父元素中作为最后一个子标签 -->
<div class='clear'>.clear { clear: both; }</div>
<!-- 我觉得挺好的，不过别人都说增加了无意义的标签，Emmm -->
<!-- 张鑫旭大大的原话 -->
<!-- 我从来不用，因为我看到的是个巨大的浪费，浪费了一个标签，
而且只能使用一次，我个人是无法容忍的，所以这个方法不推荐。
而且有时候一不留神中间多了个空格会产生一段空白高度的。 -->
```

**2.overflow + zoom方法**  

```css
.parent{
    overflow:hidden; 
    zoom:1; /* 值可为 normal, 倍数, 百分比 */
}
/* zoom是设置或检索对象的缩放比例的,除了火狐其他浏览器都兼容. */
/* 不过overflow:hidden容易裁剪 */
```

**3.after + zoom方法**  

```css
.parent{zoom:1;}
.parent:after{display:block; content:'clear'; clear:both; line-height:0; visibility:hidden;}
/* 张鑫旭大大鼎力推荐,after里可以只写display,content,clear. */
```

**4.破罐破摔法**

手动给父元素加高度......

> zoom这个属性是**ie**专有属性，除了设置或者检索对象的缩放比例之外，它还有可以触发ie的haslayout属性，清除浮动，清除margin重叠等作用。 不过值得注意的一点就是火狐浏览器不支持zoom属性，但是在webkit内核浏览器中zoom这个属性也是可以被支持的。

