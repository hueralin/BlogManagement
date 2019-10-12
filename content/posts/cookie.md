---
title: "用Node.js讲解Cookie"
date: 2019-10-12T09:50:42+08:00
draft: false
categories: ['计算机网络']
tags: ['HTTP']
---

## Cookie

靠死记硬背是学不了东西的...

### 搭建简易的服务器环境

```javascript
let express = require('express')
let app = express()
app.get('/', (req, res) => {
    // 在响应中添加cookie
    res.cookie('background', 'skyblue')
    res.cookie('color', 'green')
    res.send('<h1>Hello World!</h1>')
})
app.listen(3000)
```

### 发起请求并分析字段

我们请求下试试看：

![访问localhost:3000](/img/posts/cookie1.png)

一开始，我们假设当前域名下是没有cookie的，第一次请求的时候也不会带上cookie。第一次响应时，服务器设置了两个cookie，如上图的set-cookie字段。当我们再次请求时，浏览器会自动带上cookie，如下图：

![刷新页面](/img/posts/cookie2.png)

**注意：我们设置cookie前，响应报文没有set-cookie字段，请求报文也不会有cookie字段。**

因为我们设置了两个cookie，见上图，cookie字段里不同的cookie之间是通过；和 空格 分割的。

以上就是cookie的大体工作流程。下面我们来介绍下cookie。

### Cookie

我们知道HTTP协议是无状态的协议，即TCP连接断开后，当浏览器再次请求访问时，服务器根本不认识这个客户端。但是我们在某些情况下却又希望服务器能记住客户端的状态，这时cookie出现了。

> Cookie 的工作机制是用户识别及状态管理。Web 网站为了管理用户的状态会通过 Web 浏览器，把一些数据临时写入用户的计算机内。接着当用户访问该Web网站时，可通过通信方式取回之前发放的 Cookie。-————《图解HTTP》

#### Cookie的属性

Cookie的属性有六种：  
1. NAME=VALUE 
2. expires=日期 
3. path=路径 
4. domain=域名 
5. secure 
6. httpOnly

#### expires

指定浏览器可发送 Cookie 的有效期，若省略的话，则有效期为该会话结束，即关闭浏览器。

在HTTP/1.1中有个max-age属性，该属性值设置的是相对时间，以秒为单位，优先级比expires高。

#### path

限制指定 Cookie 的发送范围的文件目录

#### domain

指定cookie发送的域名，即发送请求时如果当前URL的域名是该domain或者是该域名的子域，就可以携带该cookie。子域可以访问父域的cookie，即从子域发起请求时，cookie字段会带上父域的cookie字段，反之不行。

#### secure

用于限制Web页面仅在HTTPS安全连接时，才可以发送 Cookie。

```javascript
app.get('/', (req, res) => {
    // 在响应中添加cookie
    res.cookie('background', 'skyblue', {
        secure: true
    })
    res.send('<h1>Hello World!</h1>')
})
```

![带secure的cookie](/img/posts/cookie.png)

可见，服务端设置了secure后，浏览器发现自己的协议并不是HTTPS，所以再次刷新时没有携带cookie。

#### httpOnly

Cookie 的 HttpOnly 属性是 Cookie 的扩展功能，它使 JavaScript 脚本 无法获得 Cookie。其主要目的为防止跨站脚本攻击（Cross-site scripting，XSS）对 Cookie 的信息窃取。

### Cookie的生存周期

前面说到expires属性可以设置cookie的过期时间，但它毕竟是个可选项，也可以不设置。那么若不设置过期时间，cookie会保存多久呢？如果我们在服务器端不设置expires等过期时间，那么此时的cookie被称为“session cookie”，即其生命周期为会话期，当浏览器窗口关闭时就会消失，session cookie保存在内存中。比如下面：

![没设置过期时间,则为session cookie](/img/posts/cookie4.png)

而设置了过期时间的cookie则放在硬盘中，存储在硬盘上的cookie可以在不同的浏览器进程间共享。

### Cookie的缺陷

1. Cookie会被附加在每个HTTP请求中，所以无形中增加了流量。  
2. 由于在HTTP请求中的Cookie是明文传递的，所以安全性成问题，除非用HTTPS。  
3. Cookie的大小限制在4KB左右，对于复杂的存储需求来说是不够用的。  
4. 识别不精确，如果在同一台机器上使用多个浏览器，每个浏览器在不同的存储位置保存 Cookie，因此，Cookie 并不能定位到一个具体的人，而是用户,计算机和浏览器的组合。  
5. 如果用户在获取了一个 Cookie 之后,点击了浏览器的"回退"按键,则浏览器的状态和获取Cookie 的状态就出现了不一致（因为父域不能访问子域的cookie，包括path）.例如, 如果网站基于 Cookie 技术实现了购物车的应用,当用户添加了物品后点击了"回退"按键, 购物车的物品状态可能并没有发生变化.（当然，购物车不会这么实现。）

### Cookie安全性

1. 偷cookie，利用别人的合法的cookie，入侵账户  
2. 修改cookie，Emmmm

[Cookie与Session的区别](https://juejin.im/entry/5766c29d6be3ff006a31b84e)
