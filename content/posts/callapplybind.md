---
title: "call, apply, bind"
date: 2019-10-06T13:27:32+08:00
draft: false
categories: ['随便学学']
tags: ['JavaScript']
---

> call apply bind 这三个函数的作用都是将一个函数的this指向另一个对象，使得该对象可以调用这个它自身没有的函数。  

### Func.call(thisObj, [arg1,arg2,....])  

**thisObj** 作为Func内部的this，它的取值有四种情况：  
1. null/undefined/不传 Func的this ----> window  
2. 函数名 Func的this ----> 该函数的引用  
3. 数值/字符串/布尔值 Func的this ----> Number/String/Boolean  
4. 对象 Func的this ----> 对象  

```javascript
function Func(){
    console.log(this)
}
function Test(){console.log('Test')}
var obj = { name: 'huer' }
Func.call() // window
Func.call(null) // window
Func.call(undefined) // window
Func.call(Test) // Test(){console.log('Test')}
Func.call(666) // Number {666}
Func.call('666') // String {"666"}
Func.call(true) // Boolean {true}
Func.call(obj)  // {name: "huer"}
```

这三个函数除了由上面的功能外，还有一个功能。还能给对象添加新属性，例如在继承中：

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age; 
}
let dog = {}
Person.call(dog, 'wangcai', 5);
console.log(dog);   // {name: "wangcai", age: 5}
// dog调用了Person构造函数，于是Person函数中的this指向dog，然后给dog添加了两个属性。
```

### Func.apply(thisObj, [args[]])  

功能和call一样，只不过第二个参数是一个参数数组（或伪数组，如arguments），而不是参数列表。

### Func.bind(thisObj, [arg1, arg2...])  

功能和上面两个一样，只不过该函数并不会立即执行Func，而是返回一个绑定后的函数。它常用于事件绑定中，因为事件绑定要求传入函数引用而不是函数的执行结果，bind的其他参数和call一样，是一个参数列表。

bind函数返回一个原函数的拷贝，并拥有指定的this值和初始参数。

bind()方法创建一个新的函数，在bind()被调用时，这个新函数的this被bind的第一个参数指定，其余的参数将作为新函数的参数供调用时使用。

**bind函数用法1：创建一个绑定函数。**

```javascript
var x = 9;
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();   
// 返回9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

**bind函数用法2：创建一个偏函数**  

```javascript
// 偏函数，即拥有预设参数的函数
// 只要将这些参数（如果有的话）作为bind()的参数写在this后面。
// 当绑定函数被调用时，这些参数会被插入到目标函数的参数列表的开始位置，传递给绑定函数的参数会跟在它们后面。

function sum(a, b) {
    return a + b;
}
function toArr() {
    return Array.prototype.slice.call(arguments);
}

let FuncA = sum.bind(null, 666);    // 将666作为第一个参数的预设
console.log(FuncA(10)); // 676
console.log(FuncA(10, 20)); // 676, 第二个参数实际上被作为第三个参数而忽略掉

let FuncB = toArr.bind(null, 6, 7, 8);  // 将6，7，8作为预设参数
console.log(FuncB());   // [6, 7, 8]
console.log(FuncB(9, 10));   // [6, 7, 8, 9, 10]
```

### bind函数用法3: 配合setTimeout

```javascript
// setTimeout实际上是window.setTimeout
// 下面计时器的执行过程是，3秒后，将匿名函数放进事件队列，事件循环到它时执行
// 这个执行和 function A(){xxx} A()一样，即在全局环境下执行匿名函数，里面的this指向window
setTimeout(function() {
    console.log('this is: ', this)
}, 3000)

// 比如有个绘制函数
let canvas = {
    render: function() {
        this.update();
        this.draw();
    }
    update: function(){xxx}
    draw: function(){xxx}
}

window.setInterval(canvas.render, 1000 / 60)
// 这样的话，render函数里的this很容易指向window，导致报错！

// 所以我们可以显式地将this绑定到canvas
window.setInterval(canvas.render.bind(canvas), 1000);
```

