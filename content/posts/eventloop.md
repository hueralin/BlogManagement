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

由于JS是单线程的，一定时间内只能执行一个函数，所以这些函数就得按照一定的顺序排好队，但它并不是一个队列，因为函数调用会涉及到作用域链的问题，函数调用之间有着一定的嵌套关系，所以栈结构比较合适。那么由函数调用组成的栈，也被称为 “执行栈” ，函数被调用时压入栈，执行完毕弹出栈。结合 [JS执行环境](http://localhost:1313/2019/jsec/) 中所讲的内容，函数调用栈中的每一个帧都是一个执行环境，执行环境里面包含着变量对象（函数被调用时才会有）。   

### 任务源  

这个高大上的名词可能没见过，但是它本身却很常见。顾名思义，任务源就是分发任务的源头，绝大部分是异步任务。分发异步任务的源头有哪些呢？setTimeout ！setInterval ! promise !等等都是任务源，这些函数在被执行的时候都是立即执行的，但是它们所分发的任务需要在特定的情况下才会被执行，而这些特定的环境则需要事件循环机制来处理。任务源所分发的任务才会被放进任务队列中。

### 任务队列  

MDN 上叫“消息队列”，好像是从前的说法，讲的有些笼统。新标准下给了它们新的名字：任务队列，给异步任务做了更细致的划分。任务队列分为：“宏任务”和“微任务”。
宏任务包括：script(整体代码), setTimeout, setInterval, setImmediate, I/O, UI渲染。微任务包括：process.nextTick，Promise, Object.observe(已废弃), MutationObserver(html5新特性)。

---

推荐两篇博客：  
[深入浅出JS事件循环机制（上）](https://zhuanlan.zhihu.com/p/26229293)  
[深入浅出JS事件循环机制（下）](https://zhuanlan.zhihu.com/p/26238030)  

上面两篇博客涉及到了[WebpAPIs](https://developer.mozilla.org/zh-CN/docs/Web/API)，文中给出了三种常见的WebAPIs，DOM相关、网络相关、定时器相关。  

 下面用一个简单的例子来说明：  
 ![代码执行前](/img/posts/eventloop01.png)  

 现在开始执行JS代码，main()入栈，执行到console.log()，log函数入栈，执行log函数，控制台输出'Hello world!' 
 ![main函数入栈，log函数入栈](/img/posts/eventloop02.png)  

 log函数出栈，setTimeout函数入栈，执行栈发现setTimeout函数是WebAPIs，于是将setTimeout所分发的任务交由浏览器内核对应的timer模块处理，然后将setTimeout函数出栈。  
 ![setTimeout函数入栈](/img/posts/eventloop03.png)  

 setTimeout函数出栈，console.log('end')入栈，执行完毕后控制台输出'end'，然后console.log()函数出栈，若timer模块计时结束就将回调函数放进任务队列。  
 ![console.log('end')入栈](/img/posts/eventloop04.png)  

 然后执行栈中还剩下main()，main函数出栈，此时执行栈为空。开始检查任务队列，队列中有任务，就拿到执行栈中去执行。再依次出栈。    
 ![匿名回调函数入栈，console.log入栈](/img/posts/eventloop05.png)  

 这个例子中只使用了setTimeout，是WebAPIs中的一个。其他的WebAPIs例如ajax请求，DOM等都由浏览器内核中不同的模块去处理。结合前面讲的任务源，来自不同任务源的任务会被放进不同的任务队列。即setTimeout分发的任务会进入setTimeout任务队列（因为可能会有多个setTimeout被调用），诸如setTimeout、setInterval等又同属于“宏任务”，而像promise等则属于“微任务”。既然有“宏任务”和“微任务”之分，那么当执行任务队列中的任务时先去哪个呢？下面来讲一下时间循环的具体流程。  

### 事件循环的流程  

1. 从script整体代码开始，执行同步任务。  
2. 当碰到异步任务时，会将异步任务交由对应的浏览器内核模块去执行，执行完毕后将其回调函数（即事件处理程序）放到对应的任务队列中，等待被执行。  
3. 第二步并不会阻塞下面同步代码的执行。  
4. 当执行栈为空时，就会优先去检查“微任务”，“微任务”中有“process.nexttick任务队列”、“promise队列”等，前者优先级大于后者，拿出队首任务放到执行栈中执行，执行完出栈，继续拿下一个“微任务”，直到所有的“微任务队列”清空。**此时，一轮循环结束！**  
5. 下一轮循环从“宏任务”开始，“宏任务”包括“setTimeout任务队列”、“setInterval任务队列”等，拿出队首的宏任务放进执行栈中执行，执行完出栈。**注意！当一个“宏任务”又创建了一个“微任务”的话，则会将该“宏任务”所在的“宏任务队列”清空后，再转去处理微任务，且微任务被清空后，才会执行下一个“宏任务队列”，也就是说，“微任务”可插队！。**  
6. 事件循环机制会一直检测执行栈、宏任务队列、微任务队列，不断循环执行。  

### 例题  

```javascript
(function test() {
    setTimeout(function() {console.log(4)}, 0);
    new Promise(function executor(resolve) {
        console.log(1);
        for( var i=0 ; i<10000 ; i++ ) {
            i == 9999 && resolve();
        }
        console.log(2);
    }).then(function() {
        console.log(5);
    });
    console.log(3);
})()
```
还有更复杂的例题[这波能反杀](https://www.jianshu.com/p/12b9f73c5a4f)  

### Q&A  

**Q：** setTimeout又分发了一个setTimeout，会在当前循环中执行么？  
**A：** 不会，宏任务不会插队，新分发的宏任务会等到下一次循环。  
```javascript
console.log('start')
setTimeout(() => {
	console.log('out')
  	setTimeout(() => {
    	console.log('in')
    }, 0)       // 最早1s后放进setTimeout队列
}, 1000)        // 最早1s后放进setTimeout队列

new Promise((resolve) => {
	console.log('promise')
  	for(var i = 0;i<100;i++){
    	i == 99 && resolve()
    }
}).then(() => {
	console.log('then')
})
setInterval(() => {
	console.log(new Date().toString())
}, 1000)       // 最早1s后放进setInterval队列
console.log('end')

/*
start
promise
end
then
out
Tue Sep 03 2019 16:14:57 GMT+0800 (中国标准时间)
in
*/
// 可以看到，setTimeout任务队列清空后执行的是setInterval任务队列，即使中途插入了新的setTimeout也没有被执行，而是老老实实的等下个循环。
```

在其他博文还看到另一个说法，就是在任务进入执行栈前会判断是同步任务还是异步任务，是异步任务的话，会有一个事件注册表的东西，来给相应的事件注册回调函数。

回更!
