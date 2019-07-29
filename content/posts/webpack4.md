---
title: "Webpack4 配置文件解析"
date: 2019-07-29T18:54:31+08:00
draft: false,
categories: ["Webpack"]
---

``` javascript
let path = require('path')
// 创建HTML模板的插件
let HtmlWebpackPlugin = require('html-webpack-plugin')
// 抽离CSS样式为单独文件的插件（原来都是将CSS样式全部放进了style里面）
let MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 优化CSS的插件（压缩CSS文件）
let OptimizeCss = require('optimize-css-assets-webpack-plugin')
// 用了上面优化CSS的插件，就得使用下面这个插件来优化JS（压缩JS文件）
let UglifyJsWebpackPlugin = require('uglifyjs-webpack-plugin')

module.exports = {

    // 优化CSS和JS
    // 优化项
    optimization: {
        // 压缩
        minimizer: [
            new UglifyJsWebpackPlugin({
                cache: true,    // 缓存
                parallel: true, // 并发压缩
                sourceMap: true // 
            }),
            new OptimizeCss()
        ]
    },

    // 模式：开发和生产模式两种
    mode: 'development',
    entry: './src/index.js',
    output: {
        // path 必须是绝对路径
        // path.resolve()可以将相对路径解析为绝对路径
        // __dirname 是当前目录
        path: path.resolve(__dirname, 'build'),
        // 加上hash，每次都生成一个新的文件(不重复 ，防止出现缓存或覆盖的问题)（8位hash值）
        filename: 'bundle.[hash:8].js'

    },
    devServer: {
        port: 3000,
        // 显示进度条
        progress: true,
        //  以build目录为静态服务的根目录(所有被服务的内容都来自这里)
        contentBase: './build',
        // 启动时自动打开浏览器
        open: true,
        // 使用gzip压缩所有被服务的文件
        compress: true
    },
    plugins: [
        // 这个插件可以按照模板生成一个index.html文件，并插入打包后的bundle.js文件
        new HtmlWebpackPlugin({
            // 指定模板文件
            template: './src/index.html',
            // 打包后的文件名
            filename: 'index.html',
            // 上线时的压缩操作
            minify: {
                // 去掉属性的引号
                removeAttributeQuotes: true,
                // （压缩）变成单行
                collapseWhitespace: true,
            },
            // 在引用文件名尾部加一个hash串
            hash: true
        }),
        new MiniCssExtractPlugin({
            // 抽离出来的CSS文件的名字
            filename: 'main.css'
        })
    ],
    module: {   // 模块
        rules: [    // 规则
            {
                // css-loader是来处理 css文件中@import语法的
                // style-loader 是来将css文件插入到html中的style标签里去的
                // 使用单个loader只需要一个字符串表示，
                // 使用多个loader需要用数组
                // loader的顺序，从右向左执行

                // loader还可以用对象表示法（ 即数组元素为对象而不是字符串），该方法下可以传个option
                test: /\.css$/,
                use: [
                    // {
                    //     loader: 'style-loader',
                    //     options: {
                    //         // 将CSS文件插入到模板HTML自定义样式的上方
                    //         insertAt: 'top'
                    //     }
                    // }
                    // 使用MiniCssExtractPlugin插件的loader，通过link引用CSS文件
                    MiniCssExtractPlugin.loader
                    , 'css-loader'
                    , {
                        loader: 'postcss-loader',
                        options: {
                            plugins: [
                                require('autoprefixer')
                            ]
                        }
                    }]     // 自动加前缀，兼容浏览器
            },
            {
                test: /\.less$/,
                use: [
                    MiniCssExtractPlugin.loader
                    , 'css-loader'
                    ,  {
                        loader: 'postcss-loader',
                        options: {
                            plugins: [
                                require('autoprefixer')
                            ]
                        }
                    } // 加前缀
                    ,  'less-loader'    // 处理less文件   
                ]      
            }
        ]
    }
}
```
