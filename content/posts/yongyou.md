---
title: "用友笔试题"
date: 2019-09-04T15:07:51+08:00
draft: false,
categories: ['笔试题']
---

> 21题  
css 中经常有类似 background-image 这种通过 - 连接的字符，通过 javascript 设置样式的时候需要将这种样式转换成 backgroundImage 驼峰格式，请完成此转换功能。  
输入：-webkit-background-image  
输出：webkitBackgroundImage  

```javascript
解法一、  
思路：将输入按照‘-’分开，得到一个数组，例如：['', 'webkit', 'background', 'image']  
然后从第三个元素开始，将首字母变大写（注意字符串是不可变的）
function func(pre){
    let splitArr = pre.split('-')
    for(let i=2;i<splitArr.length;i++){
        splitArr[i] = splitArr[i][0].toUpperCase() + splitArr[i].slice(1)
    }
    return splitArr.join('')
}
```

```javascript
解法二、  
思路：
```  

> 22题  
请实现一个简单的事件机制，能够实现对事件的触发和监听。
如：EventEmitter.on(); EventEmitter.trigger();  

```javascript
思路：封装一个对象，该对象有两个方法：on、trigger。on方法接收两个参数，事件名和回调函数，如果一个事件可以绑定过个回调，可以考虑使用数组。trigger方法接受一个参数，即事件名，拿到指定事件，将该事件下的所有回调函数执行一遍。
这种写法只是实现了时间的触发和更新，并没有将事件绑定到某个元素上。
function EventEmitter () {
    let eventObj = {}

    function on (eventName, callback) {
        if (!eventObj[eventName]) {
            eventObj[eventName] = []
        }
        eventObj[eventName].push(callback)
    }

    function trigger (eventName) {
        if (!eventObj[eventName]) {
            return
        }
        for(let i=0;i<eventObj[eventName].length;i++){
            eventObj[eventName][i]()
        }
    }

    return {
        on: on,
        trigger: trigger
    }
}
```
