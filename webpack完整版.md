

# 1、安装模块

- webpack类
  - webpack(核心) webpack-cli(用来运行一些命令，比如dev-server) webpack-dev-server(服务器)

- 样式类
  - css-loader(解析css文件)  style-loader(将样式输出到页面中)
  - postcss-loader(给css加合理的浏览器前缀)  autoprefixer(与postcss-loader配合使用，前缀如何加，浏览器兼容到什么程度)
  - less-loader(桥梁，处理less文件)  less(与css配合使用)

- 文件类
  - file-loader(不常用但要有，因为与url-loader是内部依赖的)  url-loader(处理文件，比如图片，字体等)

- ES6编译
  - babel-loader(用来处理低版本浏览器，将高级语法编译为低级语法给低版本浏览器使用)  @babel/core(核心)  @babel/preset-env(预设)

- HTML生成
  - html-webpack-plugin(用来输出HTML，不是唯一的plugin)

- 代码质量管理
  - eslint(评估js代码质量管理)  eslint-loader(与之对应) （eslint并没有用它自己的配置是因为它自己的配置实在是太恶心了，不是人用的）
  - stylelint(评估css代码质量管理)  stylelint-webpack-plugin(与之对应，和-loader工作方式是不同的)  stylelint-config-standard(它里面的各种各样的配置，标准的配置不是那么恶心)

- 测试
  - jest(核心，代码测试，既能测试普通的js，还能测试带库的js，比如vue，react，ajax)   jest-webpack(主体)

browserlist官网（browserl.ist）中有各个浏览器使用情况的数据，可以作为参考。

# 2、目录结构



# 3、配置package.json



# 4、配置webpack

详情请见webpackComplete	








