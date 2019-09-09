---
title: "字节跳动面试题"
date: 2019-09-04T16:18:24+08:00
draft: false,
categories: ['笔试/面试题']
---

### 实现sleep函数（将程序挂起一段时间，阻塞运行）  
```javascript
我能想到的方法就是ES6的 async/await
function my_sleep (time) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve()
        }, time)
    })
}
async function main() {
    console.log('start')
    await my_sleep(5000)
    console.log('end')
}
main()
网上还有一种解法，利用循环+Date()，不断的循环，检测当前时间是否超出了间隔时间  
原理就是一直在执行同步任务，阻塞下面同步任务的执行
function my_sleep (time) {
    let start = new Date().getTime()    // 一串长数字
    let end = start + time  // time是毫秒
    while(new Date().getTime() <= end){}
}
console.log('start')
my_sleep(5000)
console.log('end')
```

