---
title: "对Redux的理解"
date: 2019-10-15T15:53:24+08:00
draft: false
categories: ['React']
---

最近根据网课实现了一遍Redux，现在来说下自己的理解，[myRedux](https://github.com/hueralin/myRedux)

## React产生的原因

或者说，我为什么使用Redux。在我编写Redux代码的过程中遇到的最棘手的问题就是共享状态，不仅仅是组件传值，组件传值是发生在父子组件等具有嵌套关系的情况下，而在兄弟组件甚至什么关系都扯不上的组件之间，只能称为共享状态。在Redux未出现前，父子组件传值靠的是props，在层级非常深的情况下，这种方法就显得非常麻烦。另外一种父子组件传值的方法就是Context，Context 提供了一种在组件之间传递状态的方式，而不必显式地通过组件树的逐层传递props。虽然它也可以用于组件传值，但这并不是它的本职工作。Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，即共享数据。而组件传值发生在父子组件，祖先和孙子组件之间，也就是说这种情况下的作用范围比较小（局部的），还可能有交叉的情况，this.context的指向就已经很模糊了，若有好多地方都这么用的话，也是一场灾难。也就是说Context方法使数据变得混乱、分散。（据说最新的Context API非常好用......😂）

## Redux

Redux是一个不依赖于任何前端框架的JS状态管理库，Redux的思想是将应用的所有状态放在一起，集中管理，也就是放在全局。任何组件都不能直接修改状态，必须通过store对象的dispatch方法去发送一个action指令，通知reducer按照指令修改store中的状态。网课上实现的方式用闭包实现的单例模式，将state封装在store内部，向外暴露了三个API，分别是getState、dispatch、subscribe。订阅状态用subscribe、获取状态用getState、修改状态用dispatch。

### createStore

store是一个状态管理仓库，通过createStore方法创建。该方法创建了个闭包，返回一个store对象，里面包含上述三个方法来实现对状态的管理。单纯的使用Redux，就是在入口文件创建一个store，作为全局的状态仓库。
其他组件需要访问状态的话就将其导入进组件，使用API访问。

### action

action即指令，一个含有type属性的对象。type指明的是这个命令的内容或目的，表明这个命令是干嘛的。为什么这么设计呢？我认为是为了保证状态的安全性，因为指令是用户（程序员）发出的，万一这个指令不合法（可能是误操作），可能会对state造成破坏，同时type的设置也让指令更加语义化。

### reducer

reducer是一个纯函数，它接收旧状态和action，返回新状态。reducer是唯一一个有权修改状态的人，dispatch发送完指令后会调用它，让reducer来执行指令，reducer会做一次检查，即使用switch来匹配相应的action，执行对应的操作，若没有则返回旧对象（防止不合法的指令）。最后，当状态修改完毕后，通知所有的订阅者更新视图。

### dispatch

dispatch是一个发送指令的操作，即指令分发器。任何状态的修改必须调用该函数，尽管该函数不能直接修改状态。

### subscribe

subscribe是订阅函数，订阅状态的变化。它的参数是一个回调函数，当状态变化时就会调用它。在网课中的实现，这个回调函数是一个固定的写法。当状态变化时我们做什么？当然是重新渲染页面！于是这个回调函数就是这样的：

```javascript
subscribe(() => {
    this.setState({ number: store.getState().number })
})
```
调用setState触发重新渲染，完成了一次store到state的映射。

### combineReducers

combineReducers是一个合并reducer的函数。reducer接收旧状态和action，返回新状态。reducer函数接收的state有一个默认值，我们可以认为reducer是状态的起点，或者说状态是在这里初始化的。也就是说状态和reducer是相关的，该reducer里面的指令都是对这个状态进行的操作。而我们一般有很多的业务逻辑，会涉及到很多的状态，如果我们都放在一起，那么代码会非常混乱，文件也比较大。因此我们按照业务逻辑划分reducer，让它操作不同的状态。这样分而治之，我们可以给createStore传入不同的reducer来创建不同的store，的确使代码变得简洁大方，业务逻辑清晰。但是我们Redux的初衷是将状态集中起来管理，而我们上述操作违背了这一原则。我们好不容易分开了，为什么要合并回去呢？其实我们最终要的是合并后的单一的store，我们拆分出来只是为了逻辑清晰。combineReducers函数接受一个对象，键为reducer名，值为reducer函数，combineReducers函数返回一个合并后的reducer，这个reducer最终合并出了一个单一的store。具体的操作方法为，初始化一个合并后的state，遍历传入的对象，执行对应的reducer返回各个业务逻辑的state，并以reducer名为键存入合并后的对象中，最后combineReducers返回这个合并后的对象。

## react-redux

react-redux是redux的react版，我们开发React应用一般使用这个，react-redux只是在原有的redux上进行扩展。

当我们在组件中使用Redux时，会发现有许多操作是重复的。例如：store到state的映射，组件挂载前设置订阅函数，组件销毁时取消订阅，react-redux将这些操作通过一个connect函数封装了起来。

而且我们之前使用Redux的时候，是将创建的store在各个需要的地方导入，虽然和我们每个组件都要导入React一样，但我们仍需要将其集中在一个地方（如果可以的话，我也不愿意每次都导入React）。react-redux使用一个Provider组件将store放在最顶层，并向下传递。

### connect

connect函数是一个高阶函数，参数为mapStateToProps函数和mapDispatchToProps函数，返回值也是一个函数。这个返回的函数以一个组件为参数，返回包装后的组件。代码如下：
```javascript
import { store } from './store'
let connect = (mapStateToProps, mapDispatchToProps) => {
    return (Component) => {
        // 创建一个容器组件
        class Container extends React.Component {
            constructor(props) {
                super(props)
                this.state = {}
            }
            componentWillMount() {
                this.unsubscribe = store.subscribe(() => {
                    this.setState( mapStateToProps(store.getState()) )
                })
                // 上面只是订阅了一下，并没有拿到store中的数据，所以一开始this.state没有拿到store中的值
                // 所以需要手动更新下状态
                this.setState( mapStateToProps(store.getState()) )
            }
            componentWillUnmount() {
                // 取消订阅
                this.unsubscribe()
            }
            render() {
                // 将映射后的对象作为UI组件的props传入
                return <Component {...this.state} {...mapDispatchToProps(store.dispatch)} />
            }
        }
        return Container
    }
}
// 使用方法（比如在Counter组件中）
export default connect(mapStateToProps, mapDispatchToProps)(Counter)
```
根据上面的代码可以看到，connect将原组件封装了一层，封装的操作即为上面提到的那三个重复的操作。mapStateToProps 和 mapDispatchToProps是将state和dispatch作为原组件的参数传进原组件，也就不用每次都在组件中导入store了。

### Provider组件

这个组件就是将store放在了应用程序的最顶层，给最顶层的组件包装一层。

```javascript
import React from 'react'
import PropTypes from 'prop-types'

class Provider extends React.Component {
    getChildContext() {
        return { store: this.props.store }
    }
    render() {
        // 渲染子组件
        return ( this.props.children )
    }
}
Provider.childContextTypes = {
    store: PropTypes.object
}

export default Provider
```
可以看到，Provider利用Context API将store放在了context中，它下面的所有组件都可以使用this.context获取到。当然并不是让下面的组件自己调用context API，因为每个需要从store获取数据的组件都被connect封装了，所以应该在connect函数中调用context API，获取store。

既然也是在connect中获取store，那为什么不直接在connect中获取合并后的store呢？

