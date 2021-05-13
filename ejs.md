# ejs

## 构建项目

``` 

<!-- 使用npm全局安装 yarn安装后好像不行 -->
 1. npm install express express-generator -g
<!-- express-generator express生成器，可快速生成一个应用的骨架 -->
<!-- 检查是否安装成功 -->
 2. express --version
<!-- 生成项目 -->
<!-- -e 安装ejs模板引擎 -->
 3.  express -e myapp(项目名称)
<!-- 安装依赖 -->
 4.npm install
<!-- 运行项目 -->
 5.npm start
```

## 工程目录

* bin：存放启动脚本文件
    - bin/www：启动脚本文件，可修改端口号，等功能。
* public：存放图片，css，js等静态文件
* routes：存放路由模块文件
* views：存放视图文件，使用的ejs模板引擎
* app.js：入口文件，重要的配置文件
* package.json：工程信息和安装依赖文件

## 解读bin/www文件

* 配置环境的端口
``
