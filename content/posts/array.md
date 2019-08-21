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
**Array.find()** 拿走不送！  
该函数接受两个参数：callback、thisObj  
在回调函数中return符合查找条件的元素，若查找成功则直接返回该元素的值，否则返回undefined，并且不会改变原素组，简直不要太爽！  
如果想要查找某个元素的索引的话，可以使用 Array.findIndex() 函数，找不到的话返回-1。  

### 