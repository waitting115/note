# webpack   ——笔记

## 认识webpack

#### 能完成所有的常用功能

 - 压缩
 - 打包
 - 多种文件的编译
 - 脚手架
 - 生成

#### 安装

> npm i webpack-cli -g

## 常用功能

#### 基本使用

##### index.js

```javascript
//jquery放在本地
import $ from './libs/jquery';
//jquery放在node_modules
import $ from 'jquery';

$(function () {
    $(body).css('background', '#ccc');
})
```

##### webpack.config.js

```js
const path = require('path');

module.exports = {
  mode: 'development' ,         //模式，一共三种，决定了webpack是如何工作的
  entry:  {
    index: './src/index.js',
    news: './src/news.js'
  },         //从那个文件开始编译，编译入口，可以是单入口（更适用于SPA页面），也可以是多入口（更适用于MPA页面），多入口用json形式
  output: {
    path: path.resolve(__dirname, 'dest'),//指定输出目录,必须用绝对路径，相对路径容易出问题，建议使用path.resolve()
    filename: './[name].bundle.min.js',//指定输出目标文件名,[name]项与entry中的名字相对应
  }, //必须用json类型来输出到指定文件，编译出口
};//就是对外输出一个大的json


//mode可以决定webpack的优化级别
// mode：‘development’，开发模式,帮助你输出调试信息，然后顺便设置process.env.NODE_ENV值（node里的一个模块,代表环境变量）.process.env.NODE_ENV值就等于mode 的值，在html页可以打出来看
        // 'none',不优化
        // 'production'  生产环境，是最高级别的优化，启用压缩，自动忽略错误

```

##### index.html

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
    <script src="dest/bundle.min.js" charset="utf-8"></script>
  </head>
  <body>

  </body>
</html>

```

##### package.json

```json
//大部分自动生成
{
  "name": "demo2",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

##### path.js

```js
const path = require('path');//path用来处理各种各样的路径的

console.log(path.resolve(__dirname, 'dest'));  
//解析，可以把各种各样的路径串起来,__dirname总是指向被执行js文件的绝对路径，而./会返回你执行node命令的路径

```

等文件

#### 编译

在cmd中找到文件当前路径然后 node xxx.js

​	这样就会将某个或者某些js文件进行编译打包

#### 运行

在cmd中找到文件当前路径然后  webpack 回车

## loader

让webpack能处理js和json以外的数据

### 基本用法

#### ​	webpack.config.js

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/js/index',
  output: {
    path: path.resolve(__dirname, 'dest'),
    filename: './bundle.min.js'
  },
  module: {  //模块
    rules: [   //规则
      {
        test: /\.css$/i,  //一个用于检测的regexp
        use: [
          'style-loader',
          'css-loader'
        ]   //处理方式,从后向前处理文件
      }
    ]
  }
}


// css-loader: 用于读取和解析css文件，将css文件转化为js字符串
// style-loader：把样式输出到一个style标签中

```

#### index.js

~~~js
import '../css/index'
~~~

#### index.css

~~~css
.box {
  width: 100px;
  height: 100px;
  background: #f00;
  transform: rotate(0deg);
  transition: 1s all ease;
}
.box:hover{
  transform: rotate(90deg);
}

~~~



## loader(2)

### postcss-loader 和 autoprefixer

#### webpack.config.js

```js
const path = require('path');

module.exports = {
    mode, entry,output,
    module: {
        rules: [
            {
                test:/\.css$/i,
                use:[
                    'style-loader',
                    'css-loader',
                    {  //也可以是json形式
                        loader: 'postcss-loader',  //loader名
                        options: {   //选项   也可以单独一个文件
                            plugins: [  //插件
                                require('autoprefixer')
                            ]
                        }
                    }
                ]  //处理顺序，由后向前
            }
        ]
    }
}

// css-loader: 用于读取和解析css文件，将css文件转化为js字符串（便于webpack认识）
// style-loader：把样式输出到一个style标签中
//postcss-loader : 用于给css文件中的某些属性加浏览器前缀，提高兼容性
// autoprefixer： 用于判断css哪些属性需要加浏览器前缀，（比如根据大于5%用户的浏览器就做兼容，就要加相应的前缀（放在postcss.config.js文件中）
//  npx 命令和npm差不多，它用来执行一个包
// --info 用来看某个包的具体信息  比如命令  npx autoprefixer --info 就是来看autoprefixer包的具体信息
// .browserlistrc 文件用来配置自定义浏览器支持程度（配置autoprefixer）
  // last 1 version   支持每个浏览器的最后一个版本（最新版本）
  // last 5 version   支持每个浏览器的最后五个版本
  // >1%  支持大于1%用户的浏览器
  // .browserlistrc也可以不单独放一个文件里，可以放到package.json里面，新建一个"browserslist": []
```



#### postcss.config.js

```js
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}

//也可以不单独用一个文件
```

#### package.json(部分)

~~~json
"browserslist": 	[
    "last 1 version",
    "> 1%"
]
~~~

### file-loader

安装

> cnpm i file-loader -d

#### webpack.config.js(部分)

~~~js
{
         test: /\.(png|jpg|gif)$/i,
         use: [
           {
             loader: 'file-loader',
             options: {
               outputPath: 'imgs/',   //相对于output.path
               publicPath: 'dest/imgs'  //输出到css的路径
             }
           }
         ]
       },
~~~

### url-loader(部分)

安装

> cnpm i url-loader -d

#### webpack.config.js

~~~js
	//图片
	{
        test: /\.(jpg|png|gif)$/i,
        use: [
          {
            loader: 'url-loader',  //url-loader 可以涵盖掉file-loader
            options: {
              outputPath: 'imgs/',
              publicPath: 'dest/imgs/',
              limit: 8*1024  //限制  如果文件小于8kb，就用base64的形式放在css里面，就不用再从服务器中请求一次，可以提高性能，但仅适用于小文件，比如小图标小logo等，一般2k 4k 8k等，因为如果数值大了css文件就会变大，这也会是个麻烦，这里只是做试验看效果，所以设置了大一点
            }
          }
        ]
      },
      //字体
      {
        test: /\.(eot|svg|ttf|woff|woff2)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              outputPath: 'fonts/',
              publicPath: 'dest/fonts/',
              limit: 4*1024
            }
          }
        ]
      }


// cnpm un file-loader -d 命令就是将file-loader删掉  un 删除命令
// url-loader和file-loader 是相互依存的关系，即使没有用到file-loader，也要安装这个loader，不能删掉
// 事实上url-loader是看limit值来判断是否调用file-loader的，如果文件大小超过了limit，url-loader就会调用file-loader来处理这个大文件
~~~



### less-loader

> npm i less-loader less -d

less-loader其实本身就是一个桥梁，它并不具备less的功能，所以less也要一起安装

#### webpack.config.js

~~~js
...
{
        test: /\.less$/i,
        use: [
          'style-loader',
          'css-loader', //需要这两个loader帮忙
          'less-loader'
        ]
      }
      ...
~~~

#### 1.less

~~~less
@bg_color: #ccc;
body{background: @bg_color};
~~~

#### index.js

~~~js
import '../less/1.less'
~~~



### babel-loader

> npm i babel-loader @babel/core @babel/preset-env -d

@babel/core是babel的核心

@babel/preset.env是babel的预设

#### webpack.config.js

```js
moudle.exports = {
    mode,entry,output,
    module: {
        rules: [
            {
                //js  ||  jsx
                test: /\.jsx?$/i,
                
                //排除
                exclude: /node_modules|bower_components/,
                //include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
                
                //babel-loader
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                        //预设
                    }
                }
            }
        ]
    }
}
```

### 开启sourcemap

是一个开发工具（加上之后编译时候会稍微慢一些） 作用是加上之后在浏览器控制台出错时点击错误会显示index.js里面的代码，而不是babel编译后的代码，，便于调试

~~~js
module.exports = {
    mode, entry, output,
    module,
    devtool: 'source-map'
}
~~~

## wbpack-dev-server

### 使用及其技巧

在用dev-server的时候，要记住所编译的文件都是被保存到dev-server的内存中的，而不是输出出去。所以会出现找不到文件的问题，也就是文件路径的问题。这也就是为什么dev-server不能服务一个大型网站，而只用于测试阶段，因为大型网站文件非常多，如果都放在服务器的内存中，网站可能会爆炸，您最好上个保险。

在开发阶段，也就是mode值为development时，用dev-server，所存文件都是在它本身内存中的，很多时候并不需要publicPath来用路径找本地文件。

而在真正应用的时候，也就是mode值为production时，并不用dev-server，所存文件都是在服务器的本地路径的，此时需要publicPath来帮忙。

然后，如果要兼容两种情况，就要将两种情况的代码都单独写一份，或者将webpack.config.js文件换一种写法，让配置动态起来。而且写两个命令：

> "start": "webpack-dev-server  --env.development  --open"
>
> "build": "webpack  --env.production"

start代表用dev-server构建，build代表真正应用的时候。

---open命令使cnpm执行完后自动打开浏览器

webpack.cofig.js

~~~js
const path = require('path');

module.exports = function (env,argv) {//env=环境配置（mode值）默认是development，在pack.json中用到,argv=存了所有的webpack中要用到的选项。
  env = env || {development: true};
  if(env.production) {
    return {//返回真正应用时环境配置

    }
  } else {
    return { //返回开发阶段环境配置

    }
  }
// 但是将两套配置全都放到一个文件中更难以维护，所以也可以将两套配置分别单独放到一个文件中
};
~~~

另一种情况：

./config/webpack.development.js

~~~js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  output: {
    //development模式下并不需要path，因为它的文件都是放到server中的，而不是放到本地目录
    filename: 'bundle.js'
  },
  //如法炮制
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../index.html')
    })
  ]
}

~~~

./config/webpack.production.js

~~~js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'production',
  output: {
    path: path.resolve(__dirname, '../build'),
    filename: 'boundle.min.js'//因为在production模式下webpack是将输出的文件压缩到最小的
  },
  plugins: [
    new HtmlWebpackPlugin({//由于HTML文件很大，如果一个一个数据往里面加非常麻烦而且不易维护，所以直接引用一个模板来做
      template: path.resolve(__dirname, '../index.html')//此处有一个坑，由于webpack.production.js是要在webpack.config.js中当做插件来引进去的，而不是在本身文件执行，所以此处的模板路径要按照webpack.production.js的文件位置来设置，所以此处是./index.html而不是../index.html。
      // 而且相对路径容易出问题，这里直接换成绝对路径，而在这里呢，用的是path的一个方法，这是要运算的，运算的环境依然是webpack.production.js，相对于它本身来说依然是../index.html,然后在此计算出来路径然后放到这里。
    })
  ]
}

~~~

然后

webpack.config.js

~~~js
const path = require('path');

module.exports = function (env,argv) {//env=环境配置（mode值）默认是development，在pack.json中用到,argv=存了所有的webpack中要用到的选项。
  env = env || {development: true};
   return {
     entry:'./src/js/index',
     module: {
         rules: [
           {
             test: /\.css$/i,
             use: [
               'style-loader',
               'css-loader'
             ]
           },
           {
             test: /\.(png|jpg|gif)$/i,
             use: [
               {
                 loader: 'url-loader',
                 options: {
                   outputPath: 'imgs/',
                   // publicPath: '/imgs',  由于此处start和build相矛盾，所以写两套配置
                   limit: 4*1024
                 }
               }
             ]
           }
         ]
       },
     ...env.production?require('./config/webpack.production.js'):require('./config/webpack.development.js')//此处做一个判断，来选择性引入文件，...是ES6中的展开符。由于两个文件都是json形式的数据，所以可以用展开符将里面的项全展开到当前文件
   }
};
~~~



热更新只能更新源文件，如果HTML文件或webpack.config.js文件变化是不会触发热更新的，此时需要重新启动webpack文件

### html-webpack-plugin

代码如上

它是一个帮webpack生成并处理HTML文件的插件。



!img](file://C:/Users/%E7%8E%8B%E6%95%AC%E4%BC%9F/AppData/Roaming/Typora/typora-user-images/1548818623351.png?lastModify=1548820769)

## 代码质量管理-eslint

#### 安装：

> cnpm i eslint eslint-loader -d

eselint是核心，eslint-loader只是一个桥梁，让webpack认识eslint。

#### webpack.config.js

```js
const path=require('path');\

module.exports={
    mode,entry,output,
    module: {
        rules: [
            {
                test:/\.jsx?$/i,
                loader: 'eslint-loader',
                exclude: /node_modules|bower_modules/,
                options: {
                    
                }
            }
        ]
    },
    devtool: 'source-map'
};
```

#### 配置eslint

> node node_modules/eslint/bin/eslint.js --init

或

添加配置

~~~js
{
    ...
    "script" : {
        "eslint_init": "eslint --init"
    }
}
~~~



## 代码测试-jest

#### 安装：

> cnpm i jest jest-webpack -d

#### js测试框架

https://jestis.io/

#### 应用

##### 添加scripts

~~~js
"script" : {
    ...
    "test": "jest-webpack"
}
~~~

##### 执行测试

> cnpm run test

##### 测试文件(斐波那契数列)

~~~js
export function fab(n) {
    if(n==1 ||n==2) {
        return 1;
    }else{
        return fab(n-1)+fab(n-2);
    }
}
~~~

##### 测试集（index.test.js）

~~~js
const {fab} = require('../src/js/index');
//import {fab} from '../src/js/index';

//1  2  3  4  5  6  7  8  9
//1  1  2  3  5  8  13 21 34

test('fab 1~9',()=>{		//测试项（ test）
    //expect(fab(3)).toBe(2);//--期待着fab 3 的值为 2
    
    let arr = [0,1,1,2,3,5,8,13,21,34];
    
    arr.forEach((item, index)=>{
        if(index<1) return;
        
        expect
    })
});
~~~

##### 启动

> npm run test

