---
title: "用webpack搭建简易的React应用"
date: 2019-07-21T00:19:54+08:00
draft: false
categories: ["React"]
---

**Emmm,步骤清单😂**  
 
1. `npm init -y`  
2. `npm i webpack webpack-cli webpack-dev-server --save`  
3. `npm i react react-dom --save`  
4. `npm i babel-core babel-preset-es2015 babel-preset-react --save-dev`  
需要安装的包差不多就这些了，接下来配置webpack.config.js文件  
```
    module.exports = {
        entry: "./index.jsx",
        output: {
            path: __dirname + "/public",
            filename: "bundle.js"
        },
        module: {
            rules: [
                {
                    test: /\.jsx$/,
                    loader: "babel-loader"
                }
            ]
        },
        devServer: {
            contentBase: "./"
        }
    }
    ```  
5. 在package.json文件中添加脚本  
5.1. `"build": "webpack"`打包  
5.2. `"dev": "webpack-dev-server --open --inline"`启动测试服务器  
6. 运行下试试？？？ Emmm，这个代码块有点丑啊😂
