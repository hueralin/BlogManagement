---
title: "Array"
date: 2019-08-19T15:46:37+08:00
draft: false
categories: ['随便学学']
---

# Array  

Emmmm, Array的函数不少，至少我觉的是这样。随着ES的发展，Array多了许多奇奇怪怪的函数。  
虽说奇怪....但用起来超爽的啊！！！！  

### 什么？想要查找数组中的某个元素？  
**Array.find()** 拿走不送！**（ES6）**  
该函数接受两个参数：callback、thisObj  
在回调函数中return符合查找条件的元素，若查找成功则直接返回该元素的**值**，否则返回undefined，并且不会改变原素组，简直不要太爽！  
如果想要查找某个元素的**索引**的话，可以使用 Array.findIndex() 函数，找不到的话返回-1。  

### 增删元素类（改变原数组）  

1. push() 在后面插入  
2. unshift() 在前面插入  
3. pop() 在后面弹出
4. shift() 在前面弹出

### 元素查找类  

1. find(callback) 通过callback的逻辑，查找满足条件的元素，返回“值”  
2. findIndex(callback)  通过callback的逻辑，查找满足条件的元素，返回“索引“
3. includes(item) 查找是否有item，返回布尔值  
4. indexOf(item) 查找是否有item，返回下标  

### 数组遍历类  

***--- forEach() ---***  
```javascript
let a = [1,2,3,4,5]  
a.forEach((item, index, arr) => {
    console.log(item)   // 1,2,3,4,5
})
/*
forEach 遍历的范围在第一次调用 callback 前就会确定,所以后续调用中 添加 和 删除 的item都不会被遍历到
*/
a.forEach((item, index, arr) => {
    arr.push(index*2)    // 增加一些
    console.log(item)   // 1,2,3,4,5 (实际上a已经是1,2,3,4,5,0,2,4,6,8了)
})
a.forEach((item, index, arr) => {
    arr.pop()    // 删除一些
    console.log(item)   // 1,2,3,4,5 (实际上a已经是1,2,3,4,5了)
})
// 如果已经存在的值被改变，则传递给 callback 的值是 forEach 遍历到他们那一刻的值(如果提前修改了下一个item，那么遍历到该item的那一刻的值就是修改后的值)
a.forEach((item, index, arr) => {
    arr[index + 1] *= 2    // 做些修改
    console.log(item)   // 1,4,6,8,20.undefined (实际上a已经是1,4,6,8,10,NaN了)
})
// 如果数组的值是undefined(未初始化)或被delete,也不会被遍历
delete a[5]
delete a[4]
a.forEach(item => {
    console.log(item)   // 1,4,6,8(delete后长度不变，被删除的值变成undefined)
})
// 来看看 for 语法
let b = [6,7,8,9,10]  
for (let i = 0;i<b.length;i++) {
    b.push(0)
    console.log(b[i])   // 6,7,8,9,10,0,0,0,0,0..........此处省去成千上万个0（因为每次都增加item，遍历个没完没了）
}
// forEach 只是遍历，但如果你使用了callback的第三个参数，那么也是会修改原数组的
```  
***--- map() ---***  
```javascript
// map函数返回一个新数组，新数组中的每一项都是每个原数组元素执行callback后 返回的值。  
let a = [1,2,3,4,5]  
a.map((item) => item*2) // [2,4,6,8,10]
/*
MDN：使用 map 方法处理数组时，数组元素的范围是在 callback 方法第一次调用之前就已经确定了。  
在 map 方法执行的过程中：原数组中新增加的元素将不会被 callback 访问到；  
若已经存在的元素被改变或删除了，则它们的传递到 callback 的值是 map 方法遍历到它们的那一时刻的值；  
而被删除的元素将不会被访问到。  
这一点和 forEach 一样，毕竟forEach看起来就只是遍历，而map和其他类似的函数是在遍历基础上进行的扩展。
*/
map用途：格式化原数组，根据原数组做一些拓展（计算）  
// map函数的参数是一个回调函数，那是不是也可以传入一些内置的函数呢？当然可以！  
// MDN上就给出了一个简单的例子：将parseInt函数作为参数传递进去
['1','2','3'].map(parseInt) // [1, NaN, NaN]  
// 可以使用一些内置函数，但是要注意这些内置函数的参数，map的回调函数要求参数有三个item，index，arr。  
// 即使你没有显示的设置后两个参数（仅仅是声明一个局部变量而已），map还是会传递index和arr作为回调函数的参数。  
// parseInt函数接受两个参数：数值，进制。而map将item，index，arr都传递进去（arr会被忽略），那么索引就被当成了进制，所以最后出错了。
```