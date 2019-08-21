---
title: "React组件中的函数this绑定"
date: 2019-08-20T11:04:35+08:00
draft: false
categories: ['React']
---

## 实习项目遇到的问题  

### 关于函数this绑定！  

最近在React项目中遇到了关于函数this绑定的问题，因为在普通函数（诸如:function(){xxx}）中，this的绑定是动态的，在被执行时才会确定。若这些普通函数在自身组件内使用一般不会有太大的问题，然而一旦被当作参数（例如事件处理函数）传递到其他组件时，this就会绑定为其他组件的实例，在获取原实例的状态时就会发生错误。  

箭头函数的this绑定和普通函数不一样，它是静态绑定的，也就是说箭头函数的this绑定是在定义时绑定的，即定义箭头函数时所处的作用域的this。所以说如果React组件中的函数是用箭头函数的形式定义的话，就不用担心this绑定的问题。  

假如你的onClick事件处理函数在被触发时需要传参，如果直接 `onClick={this.handleClick(xxx)}` 话，onClick得到的就不是函数，而是函数返回的结果，所以应使用 `onClick={() => this.handleClick(xxx)}` ，即给onClick一个箭头函数作为事件处理函数，那么this就绑定为了当前组件的实例，调用的普通函数也不用做特殊的处理。

我犯了一个最基础也最致命的问题，在渲染中进行了状态的改变！按理说我不应该会犯这种错误，我犯这种错误的原因就是我将箭头函数和普通函数结合使用（模仿的项目中的老代码）：  
```javascript
handleInput(key) {
  return (e) => {
      let inputValue = {}
      inputValue[key] = e.target.value
      this.setState({
      data: Object.assign({}, this.state.data, inputValue)
      })
  }
}
onChange={this.handleInput(xxx)}
```  
看出和之前 `onClick={() => this.handleClick(xxx)}` 的不同了么？  
这里是由普通函数将箭头函数return了出来，所以函数渲染时立即执行handleInput，并将执行后的箭头函数传给onChange，顺便完成this绑定，Emmmm，秀啊！可惜当初我没搞懂这个用意，将handleInput彻底改成了普通函数，导致在渲染时执行了this.setState()，浏览器不停的改变状态和渲染，瞬间报错0～20000+，不得不强制杀死浏览器进程......  

### 总结React组件中的函数this绑定的几种方法  

一、每当作为事件处理函数时，例如：`onClick onChange` 等，都手动绑定一次  

```javascript
// 这种方法的好处是在绑定this的同时能传递参数
onClick={this.handleClick.bind(this, args)}
// 缺点是，每次都得手动调用bind()
```  

二、在构造函数中绑定一次，一劳永逸  
```javascript
// 缺点是不能传递参数
this.handleCLick = this.handleClick.bind(this)
```  

三、React组件的函数直接定义成箭头函数  
```javascript
handleClick = (args) => {xxxxx}
// 缺点也是不能传递参数
```  

> 上述三个做法有个矛盾，就是“一次绑定”和“传递参数”不能兼顾  
对于二、三方法，要实现传递参数，就得使用箭头函数，即  
`onClick = {() => this.handleClick(args)}`  
既然使用了箭头函数，那么二三方法中的绑定就没有必要了，这虽然是个好方法，但是看书写的代码量和第一种方法又有什么区别呢？

四、普通函数返回一个箭头函数  
```javascript
// 这是在实习期做项目时前辈的一个做法，感觉还不错
// 这种方法的原理就是在普通函数中返回一个箭头函数，  
// 在渲染时立即执行该函数，接收箭头函数作为事件处理函数，完成绑定的同时又传递了参数  
handleClick (args) {
  return () => {
    balabalabala...
  }
}
onClick = {this.handleClick(args)}
```
