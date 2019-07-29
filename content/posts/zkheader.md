---
title: "实习项目的Header结构解析"
date: 2019-07-28T00:07:10+08:00
draft: false,
categories: ["实习"]
---

最近在做公司的项目，更改移动端Header的现有布局。  
Header是React组件，点击按钮出现侧边栏导航。  

**下面简单来介绍一下Header的结构：**  
Header接受了3个props，分别是“userInfo”、“changeNavState”、“style”。  
userInfo是用户信息，changeNavState是用来更改侧边导航状态的函数，style是一个类样式。  

以上几个props都交由PropTypes来验证参数的合法性。  
该Header组件内仅维护了一个状态，那就是 subNav，即侧边导航栏的显示与否，changeNavState函数改变的就是它。 

**在render渲染函数中：**  
`const { userInfo, style } = this.props`  
根据是否有userInfo.avatar属性来初始化headUrl变量（有则赋值为用户头像URL，无则赋值为默认头像URL）  

**接下来return一个JSX，**  
首先判断subNav是否为真，真则显示侧边导航。  
然后是左右布局，左边是点击显示侧边导航的下拉按钮和点击能返回主页的LOGO，右边是用户头像。  
左边的导航按钮添加点击事件（调用changeNavState函数），通过控制subNav状态来显示和隐藏侧边导航。  
右边判断userInfo.id属性，有则根据headeUrl显示用户头像，否则显示默认头像。默认头像图标添加点击跳转登录事件。 

其中userInfo属性是由connect函数连接的， userInfo来自于Redux的State。

