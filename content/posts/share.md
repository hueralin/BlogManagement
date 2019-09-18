---
title: "技术分享之Redux思想"
date: 2019-09-18T13:40:10+08:00
draft: false
categories: ['随便学学']
---

# Redux  

### 普通的状态管理  

```javascript
// 状态
let state = {
    // 计数
    count: 0
}···
// 修改状态
state.count = 1
// 获取状态
console.log(state.count)
```  
缺点：状态改变，依赖状态的地方得不到通知  

### 带有发布订阅的状态管理  

```javascript
// 状态
let state = {
    // 计数
    count: 0
}
// 集中管理订阅
let listeners = []
// 订阅函数
function subscribe(callback) {
    listeners.push(callback)
}
// 修改状态的函数
function changState(newVal) {
    state.count = newVal
    for(let i=0;i<listeners.length;i++){
        listeners[i]()
    }
}
// main
subscribe(() => {
    console.log('count', state.count)
})
changState(1)  // 1
changState(2)  // 2
changState(3)  // 3
```

缺点：只对count（即单个状态）有效，应将公共操作封装起来。  

### 封装后的版本  

```javascript
//  创建store的函数
function createStore(initState) {
    let state = initState || {}
    let listeners = []
    // 订阅函数
    function subscribe(callback) {
        listeners.push(callback)
    }
    // 修改状态的函数
    function changeState(newState) {
        state = newState
        for(let i=0;i<listeners.length;i++){
            listeners[i]()
        }
    }
    //  获取状态
    function getState() {
        return state
    }
    // 返回一个store对象
    return {
        subscribe,
        changeState,
        getState
    }
}
// main
let initState = {
    count: 0,
    user: {
        name: 'huer',
        age: 20,
        sex: 'male'
    }
}
let store = createStore(initState)
store.subscribe(() => {
    let count = store.getState().count
    console.log(`count: ${count}`)
})
store.subscribe(() => {
    let user = store.getState().user
    console.log(`name: ${user.name}`)
})
store.changeState(Object.assign({}, store.getState(), {count: 1}))
store.changeState(Object.assign({}, store.getState(), {user: 'malin'}))
```

缺点：state可以被随便改，没有约束。  

### 带计划的状态管理  

```javascript
//  以计数器为例
/*
    {
        type: 'ADD',
        count: 2
    }
*/
// 计划函数 plan
function plan(state = {}, action) {
    switch(action.type) {
        case 'ADD': return Object.assign({}, state, {count: state.count + 1});break;
        case 'SUB': return Object.assign({}, state, {count: state.count - 1});break;
        default: return state;
    }
}
//  改进封装后的changeState函数
function changeState(action) {
    // 按计划修改状态
    state = plan(state, action)
    for(let i=0;i<listeners.length;i++){
        listeners[i]()
    }
}
// main
// 自增
store.changeState({
  type: 'ADD'
})
// 自减
store.changeState({
  type: 'SUB'
})
```

通过Action，使得状态变化的结果可预测。

changeState --> dispatch  
plan ---> Reducer  

Store的角色是整个应用的数据存储中心，集中大部分页面需要的状态数据。  

Redux工作流程：用户通过界面组件 触发Action，携带Store中的旧State与Action 流向Reducer, Reducer返回新的state，并更新界面。

[Rimage](https://github.com/hueralin/Rimage)

[Zmage](https://zmage.caldis.me/)
