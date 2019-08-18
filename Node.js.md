# Node.js

## 一、认识Node.js

### 什么是Node.js

 	1. JavaScript的服务器版本
     - Python的Django
     - Java 的Servlet
     - C++的CGI

2. 比其他语言好在哪
   - 便于前端人员上手
   - 性能高
   - 利于和前端代码相整合

3. 工作中的用途

   - 很少用于主力服务器开发语言
   - 常用于中间层语言
     - 充分利用已有代码，增强主服务代码的独立性
     - 安全性
     - 性能
     - 丰富的接口功能

   - 工具开发

### 环境搭建

 	1. node.js安装
 	2. npm换源（淘宝）

## 二、Node.js模块系统

因为Node.js早于ES6出现，所以并没有采用ES6的模块化系统

### 使用模块

一个文件就是一个模块

~~~Node.js
//引入模块
let a = require('mod');  //错误
let a = require('./mod');  //正确
//哪怕是同级，也要加上./ ，遵循node的语言规范，不然会报错
~~~



模块两大对象--exports和nodule

```Node.js
//模块声明
module.exports == exports; //一个东西
module.exports.a = 12;  //输出a
exports.a = 12; //同上

// 模块整体输出
module.exports = {
	...
};
exports = ...; //无效  这样只能输出单个对象

//注意：exports可以被覆盖
exports.a = 12;
module.exports = {
    ...
}; //a消失了，被覆盖掉

//可以输出任何东西
//1.各种变量
module.exports.a = 12;
module.exports = {
    a: 112,
    b: function () {
        ...
    }
};

//2.模块整体是个函数
module.exports = function () {
    
};

//3.模块整体是个类
module.exports = class{};
```

### node_modules目录

引入自定义模块会出错，必须加上路径，这和node.js寻找模块的顺序有关。

 - 如果只有名字
    - $HOME/.node_modules
    - ./node_modules

- 如果带有路径
  - 按照路径寻找

### package.json

相当于Node.js的工程文件

#### 初始化

> npm init [-y]

#### 文件内容

name： 名称

version：版本--npm自带简单的版本控制系统，用于发布和依赖

description：描述

main：入口文件

scripts：自定义脚本

keywords：关键字，用于发布后搜索

author：作者

license：协议



devDependencies：开发者依赖

dependencies：生产依赖



版本前缀：

^  可兼容的最高版本

x.x  特定版本

x.*  

latest  最新版

#### 用途

快速安装

添加各种脚本，配置信息

发布包

### 其他模块管理系统

cnpm：企业内网npm

> #安装
>
> npm install -g cnpm-registry=https://registry.npm.taobao.org
>
>  
>
> #使用
>
> cnpm i xxx

yarn:  facebook出的包管理器

> react中会用到