---
title: "后台管理系统"
date: 2019-08-07T10:49:37+08:00
draft: false,
categories: ['实习']
---

### 这是实习期一个后台管理的项目  
目前正在熟悉该项目，边熟悉，边做一些记录  

先来看一下大体的框架：  
**顶层容器**  
在container目录下的index.js文件中，有一个IndexContainer组件，该组件中的render函数是这么写的  
```javascript
render () {
    let { permission } = this.props
    return (
        <div>
        <Header params={this.props.params} username={permission.username} />
        <div className='main-content'>
            <SideNav routes={this.props.routes} permissions={permission.permission} />
            <div>
            {this.props.children}
            </div>
        </div>
        </div>
    )
}
```  
可以看出该系统采用了非常常见的布局，即‘顶部导航+左侧导航+右侧内容’  

**路由配置**  
因为路由比较多，所有路由配置分为两个路由文件：  
*外层路由文件*  
```javascript
// 这里引入了内层路由文件  
import routers from './routers'
module.exports = (
  <Router history={browserHistory}>
    <Route path='/' />
    <Route path='/xxx' component={xxx} />
    <Route path='/xx/xx' component={xx} />
    ......
    <Route path='' component={Container} onEnter={handleEnterSystem}>
      {/* 嵌套路由，内层路由文件里的Route渲染的组件都是Container容器下的子组件，在this.props.children处渲染*/}
      { routers }
    </Route>
    <Redirect from='*' to='/error/404.html' />
  </Router>
)
```  
*内层路由组件*  
```javascript
const Router = (
  <Route path='/xxx'>
    <IndexRoute component={Page} />
    <Route path='yyy/product' component={ProductList} />
    <Route path='yyy/product/:id' component={Product} />
    <Route path='yyy/users/:id' component={Users} />
    ......
  </Route>
)
module.exports = Router
```  

