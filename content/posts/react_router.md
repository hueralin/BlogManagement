---
title: "React_router配置"
date: 2019-08-05T14:24:46+08:00
draft: false,
categories: ['React']
---

最近在学习React-router遇到了一些问题，在此记录一下：  
目前成功的配置为：  
**app.jsx**
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { HashRouter as Router, Route } from 'react-router-dom'
import Content from './components/Content.jsx'
import About from './components/About.jsx'
import Posts from './components/Posts.jsx'
import Post from './components/Post.jsx'

ReactDOM.render(
    <Router>
        <Content>
            <Route path='/about' component={About}></Route>
            <Route path='/posts' component={Posts}></Route>
            <Route path='/posts/:id' component={Post}></Route>
        </Content>
    </Router>,
    document.getElementById('app')
)
``` 
**Content.jsx**  
```javascript
import React from 'react'
import { Link } from 'react-router-dom'

class Content extends React.Component {
    render () {
        return <div>
            <ul>
                <li><Link to='/'>主页</Link></li>
                <li><Link to='/posts'>博文</Link></li>
                <li><Link to='/about'>关于</Link></li>
            </ul>
            {this.props.children}
        </div>
    }
}

export default Content
```  
可以看到，这里是用的是 react-router-dom（v5.0.1），而不是react-router  

### react-router-dom的三个基本组件  
1. Router Component  
2. Route matching Component  
3. Navigation Component  
这三个组件都必须从 react-router-dom中导入  

### Router Component  
1. `<BrowserRouter>`：用于使用服务器来处理请求的情况  
2. `<HashRouter>`：用于静态文件服务  

> **HashRouter**  
哈希路由用来兼容传统浏览器  
HashRouter几个常见的参数：  
1. `basename` 用来指定统一的前缀  
```javascript
<HashRouter basename='/huer'>
    <Link to='/about'>狐耳</Link>
</HashRouter>
// 即'#/huer/login'
```

### Route Matching Component  
路由匹配组件主要是用来根据不同的path来显示不同的component的，如果一个路由匹配组件没有设置path路径，那么它总会被匹配到。当一个Route的path匹配到当前的location's pathname，就会渲染component属性所对应的内容，若没有匹配到，就会render null。  

```javascript
// when location = { pathname: '/about' }
<Route path='/about' component={About}/> // renders <About/>
<Route path='/contact' component={Contact}/> // renders null
<Route component={Always}/> // renders <Always/>
```
1. `Route`：`<Route path='/user' component={User}>`    
2. `Switch`：`<Switch>一组<Route></Switch>` 

一个Switch组件包含一组Route组件，但并不是必须的。Switch组件会迭代它的子Route组件，并且只渲染第一个匹配到的Route，这在许多Route组件匹配到同一个pathname时非常有用。在没有匹配到合适的pathname时，它可以在最下面设置一个无path的Route组件，表示匹配不到时的组件，例如404页面。  
```javascript
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/contact" component={Contact} />
  {/* when none of the above match, <NoMatch> will be rendered */}
  <Route component={NoMatch} />
</Switch>

// 当前位置：/about ，那么About User NoMatch都会被渲染
<Route path="/about" component={About}/>
<Route path="/:user" component={User}/>
<Route component={NoMatch}/>
// 若使用Switch包裹起来，则匹配到第一个何时的就结束了，不会继续向下匹配
```  

### Navigation  
导航链接组件，有三种：  
`<Link>`  
```javascript
// to: string  
<Link to="/courses?sort=name" />  
// to: object  
<Link
  to={{
    pathname: "/courses",           // 路径名
    search: "?sort=name",           // 查询字符串
    hash: "#the-hash",              // 锚点
    state: { fromDashboard: true }  // 保留到该位置的状态
  }}
/>
// to: func  
<Link to={location => ({ ...location, pathname: "/courses" })} />  
<Link to={location => `${location.pathname}?sort=name`} />  
// 其实返回对象或字符串，即上面两种用法  
// replce  当值为true时，替换历史记录栈而不是新增一个记录
<Link to='/login' replace></Link>  
// exact代表当前路由path的路径采用精确匹配,不然像下面的代码（若没有exact），匹配/时也会渲染about
// 嵌套路由不要加exact属性
<Link exact to='/'></Link>
<Link to='/about'></Link>
// 其他的属性，例如title,id,className等也可以加上
```
`<NavLink>`  
```javascript
// activeClassName  被选中时应用该样式  
<NavLink to="/faq" activeClassName="selected">FAQs</NavLink>
// activeStyle 同理，接受一个对象  
// isActive：func 匹配时执行的回调函数
```
`<Redirect>`  
```javascript
// 重定向本来就是定向到其他URL，本质上就是replace，现有 ‘push’ 属性，代表想历史记录栈中加入一条历史记录，而不是替换。  
<Redirect push to='/about'/>
```  

> **match**  
match对象包含一些路由匹配的信息，match对象从this.props.match中获取  
它包含以下四个属性：  
1. params：  
2. isExact：是否精确匹配  
3. path：即写在Route中的path匹配模式  
4. url：实际的path  
