# npm命令

- ### --save

  - --save表示我们安装模块的时候，同时把它写到package.json文件中。这时打开package.json文件，我们看到多了一个dependencies字段，它包括了我们刚安装的依赖。

- #### --save-dev

  - --save-dev表示安装依赖的时候把它写到package.json中，不过它写入的不是dependencies，而是devDependencies中。

- ####  devDependencies和dependencies的区别

  - devDependencies是开发依赖；
  - dependencies是运行依赖。
  - 无论是DevDependencies还是dependencies中，安装的模块版本号前面还有符号^，其实这里还有很多符号也可以无符号，符号主要是先动版本

- #### init

  - 创建项目
  - 比如 创建一个vue-cli项目： vue init webpack projectName