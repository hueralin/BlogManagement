---
title: "圣杯布局"
date: 2019-10-11T10:01:26+08:00
draft: false
categories: ['CSS与布局']
---

圣杯布局： 

+ 顶部和底部占满屏幕宽度
+ 中间的部分是一个三栏布局
+ 三栏布局两侧固定宽度，中间部分自适应
+ 中间部分的高度是三栏中最高的区域的高度

总体来说，还是一个三栏布局，这也是重点部分。

#### 浮动实现  

```html
<style>
    .top,.bottom {
        height: 100px;
        margin: 5px 0;
        background-color: orange;
    }
    .middle {
        margin:5px 0;
    }
    .left,.right {
        width: 200px;
        height: 400px;
        background-color: skyblue;
    }
    .left {
        float: left;
    }
    .right {
        float: right;
    }
    .content {
        overflow: hidden;   /* 形成BFC，防止文本环绕 */
        padding: 0 5px;
        background-color: #eee;
    }
</style>

<body>
    <div class="container">
        <div class="top">顶部</div>
        <div class="middle">
            <div class="left">左侧</div>
            <div class="right">右侧</div>
            <div class="content">
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
            </div>
        </div>
        <div class="bottom">底部</div>
    </div>
</body>
```

![浮动实现的圣杯布局](/img/posts/shengbei1.png)

### flex实现  

```html
<style>
    .top,.bottom {
        height: 100px;
        margin: 5px 0;
        background-color: orange;
    }
    .middle {
        display: flex;
        margin:5px 0;
    }
    .left {
        width: 200px;
        height: 300px;
        background-color: red;
    }
    .content {
        flex: 1;    /* 实现中间自适应 */
        margin: 0 5px;
        background-color: yellow;
    }
    .right {
        width: 300px;
        background-color: blue;
    }
</style>
<body>
    <div class="container">
        <div class="top">顶部</div>
        <div class="middle">
            <div class="left">左侧</div>
            <div class="content">
                <p>哈哈哈哈哈哈</p>
                <p>哈哈哈哈哈哈</p>
                ........
            </div>
            <div class="right">右侧</div>
        </div>
        <div class="bottom">底部</div>
    </div>
</body>
```

![flex实现的圣杯布局](/img/posts/shengbei2.png)

