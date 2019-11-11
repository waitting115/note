# Node.js

## 2019.10.26

1. 下载node，安装
2. 用vsCode编辑，新建hello-nodejs文件
3. 下载terminal插件（一个终端，可以边写代码边运行）
4. 安装express包：在终端写：npm install express，就好了，然后会出现一个node_modules文件，就成功了
5. 安装webpack包：npm install -g webpack，就好了，-g是表示全局安装，webpack以后就可以当作一个命令来用

## webpack问题解决方法

问题：

`PS C:\wamp\www\hello-nodejs> webpack`
`webpack : 无法加载文件 C:\Users\Wei\AppData\Roaming\npm\webpack.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。`
`所在位置 行:1 字符: 1`

- `webpack`

- ```
  + CategoryInfo          : SecurityError: (:) []，PSSecurityException
  + FullyQualifiedErrorId : UnauthorizedAccess
  ```

解决：1.管理员身份打开powerShell；

​	   2.输入set-ExcutionPolicy RemoteSigned

​	   3.选择Y或A就好了

## 安装淘宝镜像

淘宝npm:<https://npm.taobao.org/>

终端输入：npm install -g cnpm --registry=https://registry.npm.taobao.org

然后以后就可以用cnpm来代替npm，就可以在国内网站下载各种包，解决了下载慢的问题

## 管理包（npm init)

运行npm init命令后，一直回车，最后会自动创建一个文件package.json，安装的包的名字都在里面

package.json：dependencies:[]里面记录着你所有安装的包的信息

如果用--save-dev命令安装的包，就会记录在devDependencies:[]中，开发环境所用的包

script中是一些脚本，如可以直接用npm run start命令来启动后端服务（前提是script脚本中的start部分没有错误）

## 下载所有所需包

如果新接手一个node，然后没有包文件，只有package.json中的包记录，如何将所需的包全部下载下来？

直接输入命令 npm（cnpm） install 就可以了

## nodemon

作用是：监控文件的改变，自动重启服务器

如何使用：1. 输入命令：cnpm install -g nodemon

​		  2.用nodemon代替node命令就可以了

 		 3.然后就会发现它一直在监听你的代码改变情况

但仅适用于开发环境，生产环境不会总是改代码，所以没必要总是重启服务器

​		4.然后将package.json文件中的script中的start中的node改为nodemon，以后npm run start就可以直接用nodemon开启服务器了

## 安装mysql时找不到mysql

错误：

PS C:\Users\Wei\WeChatProjects\TheSeasideIdle\wxnode> node database
internal/modules/cjs/loader.js:797
    throw err;
    ^

Error: Cannot find module 'mysql'
Require stack:
?[90m    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:794:15)?[39m
?[90m    at Function.Module._load (internal/modules/cjs/loader.js:687:27)?[39m
?[90m    at Module.require (internal/modules/cjs/loader.js:849:19)?[39m
?[90m    at require (internal/modules/cjs/helpers.js:74:18)?[39m
?[90m    at Module._compile (internal/modules/cjs/loader.js:956:30)?[39m
?[90m    at Module.load (internal/modules/cjs/loader.js:812:32)?[39m
?[90m    at Function.Module._load (internal/modules/cjs/loader.js:724:14)?[39m
?[90m    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)?[39m {
  code: ?[32m'MODULE_NOT_FOUND'?[39m,
  requireStack: [
    ?[32m'C:\\Users\\Wei\\WeChatProjects\\TheSeasideIdle\\wxnode\\database.js'?[39m
  ]
}

解决：

安装mysql的时候不要加-g，不要安装到全局，那样会导致s文件引入的mysql目录的路径与mysql模块真正安装的路径不一致，所以要安装到此目录下。



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





