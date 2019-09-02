---
title: "JS 事件循环"
date: 2019-09-02T18:56:56+08:00
draft: false
categories: ['随便学学']
---

[MDN 事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)  
[详细的不知道该给这个链接命什么名好!](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops)  

Ahhhhhh, 这JS学的，不断刷新世界观~  

事件循环可谓是JS中的一大重点，如果说之前的[JS执行环境](http://localhost:1313/2019/jsec/)从微观角度上说明了函数内部的执行原理，那么事件循环则从宏观角度上说明了整个JS代码的执行情况。  

事件循环中的关键概念：执行栈、任务源、任务队列  

### 执行栈  

由函数调用组成的栈，也被称为 “执行栈” ，函数被调用时压入栈，执行完毕弹出栈。结合 [JS执行环境](http://localhost:1313/2019/jsec/) 中所讲的内容，函数调用栈中的每一个帧都是一个执行环境，执行环境里面包含着变量对象（函数被调用时才会有）。   

### 任务源  

这个高大上的名词可能没见过，但是它本身却很常见。顾名思义，任务源就是分发任务的源头，绝大部分是异步任务。分发异步任务的源头有哪些呢？setTimeout ！setInterval ! promise !等等都是任务源，这些函数在被执行的时候都是立即执行的，但是它们所分发的任务需要在特定的情况下才会被执行，而这些特定的环境则需要事件循环机制来处理。任务源所分发的任务才会被放进任务队列中。

### 任务队列  

MDN 上叫“消息队列”，好像是从前的说法，讲的有些笼统。新标准下给了它们新的名字：任务队列，给异步任务做了更细致的划分。任务队列分为：“宏任务”和“微任务”。
宏任务包括：script(整体代码), setTimeout, setInterval, setImmediate, I/O, UI渲染。微任务包括：process.nextTick, Promise, Object.observe(已废弃), MutationObserver(html5新特性)。

### 事件循环执行顺序  

1. 先执行宏任务，即先从script（整体代码）开始，遇到宏任务源，将宏任务放进宏任务队列（微任务类似）  
2. 等执行栈清空后，再执行微任务  
3. 循环...
