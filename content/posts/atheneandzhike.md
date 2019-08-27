---
title: "技术分享会"
date: 2019-08-21T18:08:27+08:00
draft: false 
categories: ['实习']
---

1. 智课网
2. Athene  


（题库、订单、smart等服务）  
1. ---> API ---> 智课网  <--读取--> DB  
2. ---> 管理 ---> Athene  <--配置、写入--> DB  
3. DB（Athene & beikao）  

### 智课网  
智课展示资讯、商品、练习等，对其他服务提供接口  

> **资讯**  
archetype  
1. id(当前类目ID), reid（上一级类目ID）, topid（顶层类目ID）  

> **广告**  
advertisement  
1. position_id（一级类目）  
2. type_id (二级类目)  
3. PC or WP  
4. archive_id  

> **订单**  
1. type：1（付款成功）2（发起退款）3（取消订单）4（等待付款）  

> **退款**  


### Athene  
负责智课网所有项目的配置，不对外部服务提供任何接口  
项目1 [athene-frontend(React+antd)]  
项目2 [admin-backend(express+node)]  
数据库 [DB(athene & beikao)]

刷新页面，看getSession请求，里面包含用户的权限  

start - 获取权限 - 展示菜单 - 浏览页面 - 编辑、提交数据 - 检验登录、权限 - 数据入库or拒绝  

