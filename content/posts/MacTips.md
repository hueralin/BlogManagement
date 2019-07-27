---
title: "Mac使用技巧"
date: 2019-07-27T17:21:45+08:00
draft: false,
categories: ["Mac"]
---

**VSCODE使用命令行打开目录**  
>相关知识：环境变量的设置  

环境变量的分类：系统级和用户级  
系统级：对所有用户生效的永久性变量（/etc/profile）  
用户级：对单一用户生效的永久性变量（～/.bash_profile）  

1. 查看单个环境变量`echo $PATH`  
2. 查看所有环境变量`env`  
3. 修改环境变量`PATH="xxxx"`
4. 查看所有本地定义的shell变量`set`  
5. 删除变量`unset VNAME`  
6. 设置只读变量`readonly VNAME`

使用export指令添加环境变量：  
`vim ~/.bash_profile`  
`输入 export JAVA_HOME=/usr/java/jdk`  
`输入 export PATH=$JAVA_HOME/bin:$PATH`  
添加完成后新的环境变量不会立即生效，调用source ~/.bash_profile 该文件才会生效  

使用export命令添加临时环境变量：  
`命令行下输入 export MALIN='malin'`

使用code命令用VSCODE打开目录：  
1. 在VSCODE中 command+shift+P -> 输入shell command -> 点击提示Shell Command: Install ‘code’ command in PATH运行  
2. 在终端中使用 code 目录路径 打开目录（vscode） 
