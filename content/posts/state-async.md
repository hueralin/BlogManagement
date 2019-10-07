---
title: "setState"
date: 2019-10-04T23:57:30+08:00
draft: false
---

### State机制  

setState函数是React管理组件状态的一种方法，修改组件的状态时必须使用该方法，而不能直接在this.state上赋值。但是，如果我们在调用完setState之后直接去访问State时，拿到的是修改前的值。也就是说，setState并不保证“同步执行”。

如果在一次操作中多次调用setState，那么React会将这几次操作暂存起来，然后统一集中合并，根据合并后的结果去计算新的虚拟DOM，最后执行渲染。　　

```javascript
// 初始 count = 0
this.setState({ count: this.state.count + 1 })
console.log(this.state.count)       // 0
```

那么如果和解决上面的问题呢？-----回调函数

setState函数的参数有两个，第一个参数是对象，第二个参数是回调函数。回调函数在State合并更新完后才会被调用，所以可以拿到更新后的值。

```javascript
this.setState({
    count: this.state.count + 1
}, () => {
    console.log(this.state.count)   // count = 1
})
```

下面来看另一段代码：
```javascript
this.setState({ count: this.state.count + 1 })
this.setState({ count: this.state.count + 1 })
```
该会怎么执行呢？结果是2么？不对！结果是1。  
上面提到过，多次调用setState会被合并为一次操作。上面的代码类似于下面这段：
```javascript
Object.assign(state, {count: state.count+1}, {count: state.count+1})
```
都是操作的count，合并时只执行最后一个。  

那么如何做到增加两次呢？其实setState函数的第一个参数也可以为函数：
```javascript
this.setState((preState, props) => {count: preState.count + 1})
this.setState((preState, props) => {count: preState.count + 1})
console.log(this.state.count)   // 仍然是10，但页面渲染的是2

// preState为之前的State，props为State更新执行瞬间的props
```

这样，每次调用都是在上一次state的基础上修改。

上面代码相当于:
```javascript
[{increment: 1}, {increment: 1}, {increment: 1}].reduce((preState, props) => {
    count: preState.count + props.increment
})
// 累加器
```

其实，React的内部会维护一个State的更新队列，会将当前所有的State修改操作都推入队列中，然后集中合并处理，再调用回调函数，最后计算新的虚拟DOM，执行渲染。

### setState真的是异步的么？非也！  

React为了实现跨平台，保证兼容性，内部封装了自己的事件机制，来代替原生事件。React内部会维护一个isBatchingUpdate的标识，来决定是同步修改还是异步修改。在这些合成事件和生命周期函数里执行setState的时候，React将isBatchingUpdate设置为true，即批处理修改，也就是说异步修改。而在原生事和setTimeout函数里面使用setState的时候则是同步修改。  

[参考](https://github.com/sisterAn/blog/issues/26)

