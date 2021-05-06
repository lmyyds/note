# webpack(4-5)

## 安装webpack

``` 

npm

npm install webpack webpack-cli

yarn

yarn add webpack webpack-cli
```

---

## webpack模块引入原理

``` 

(funcrtion(modules){
    var installedModules = {};
    function __webpack_require__(moduleId){
        <!-- 缓存 -->
        if(installedModules[moduleId]){
           return installedModules[moduleId].exports;
        }
        <!-- 创建一个module -->
        var module =  installedModules[moduleId] = {
            <!-- 模块名 -->
            i:moduleId,
            <!-- 模块是否加载完毕 -->
            l:false,
            exports:{}
        }

        modules[moduleId].call(module.exports,module,module.exports,__webpack_require__);
        module.l = true;
        return module.exports;
    }

    return __webpack_require__("./src/index")
}({
    "./src/index.js":(function(module,module.__webpack_exports__,__webpack_require__){
        __webpack_require__("./src/user.js");
        <!-- 使用eval的好处，方便调试避免当代码报错的时候报错显示的是编译合并压缩后的代码杂乱看不懂 -->
        <!-- //# sourceURL=webpack:///./src/index.js? 指定报错后的文件地址指引，会将代码归纳到webpack文件下的src目录下的index.js中 -->
        eval("xxxxx //# sourceURL=webpack:///./src/index.js?")
    }),
    "./src/user.js":(function(module,module.__webpack_exports__,__webpack_require__){
        const a = "1";
       __webpack_exports__["default"] = a;
    })
}))
```

---

### devtool配置

#### source map(源码地图)

> 本知识点与webpack无关

**解决的问题**

> 解决调试的问题，显示源码中的错误而不是编译转换后的代码中的错误

**最佳实践**

1. source map应该在开发环境中使用，作为一种调试手段
2. source map不应该在生产环境中使用，不仅导致额外的网络传输还容易暴露原始代码

#### webpack中的source map

[webpack配置:devtool文档](https://v4.webpack.docschina.org/configuration/devtool/)
**devtool配置**

``` 

module.exports = {

    mode:"development",
    devtool:"none"   //关闭源码地图
    devtool:"eval"   //默认值,原理就是使用eval运行模块中的代码
    devtool:"eval-source-map"   //生成源码地图展示真实代码
}
```

---

### webpack编译过程

#### 初始化

融合配置

#### 编译

创建chunk

#### 输出

---

### 输入输出

``` 

module.exports = {
    entry:"",  //打包入口
    output:""  //打包出口
}
```

* **entry(入口)**

``` 

写法一:
entry:"./src/index"  //默认值

写法二:
多个chunk
entry:{
    chunkName:"./src/index",
    user:"./src/user"
}

写法三:
一个chunk 多个入口
entry:{
    chunkName:["./src/index","./src/user"]  
}
```

* **output(出口)**

``` 

    output:{
        path:path.resolve(__dirName,"dist"),  //这里必须是绝对路劲
        filename:"xxx",  //文件名
        filename:"[name].[hash:4].[chunkHash]" //name为chunkname，hash为总的hash值，chunkHash为每个chunk的hash值，hash:4 表示取hash值中的前四位
    }
```

* **当项目内容发生变化则hash值发生改变，否则无变化, hash值一般用来解决浏览器缓存问题**

---

### loader

#### 作用

> loader 用于对模块的源代码进行转换  [loader的概念](https://v4.webpack.docschina.org/concepts/loaders/)

#### 用法

``` 

module.exports = {
    module:{
        rules:[
            {
                test:/\.css$/,   //使用正则匹配文件后缀名
                use:"style-loader"  //写法一
                use:[style-loader,"css-loader"]  //写法二 当存在多个loader的时候执行顺序从右到左
                use:[
                    {
                        loader:"style-loader"   //写法三
                    }
                ]
            }
        ]
    }
}
```

#### 自定义loader

``` 

    <!-- 自定义loader   loaders/css-loader.js-->
    module.exports = function(params){
        return "";
    }
    <!-- webpack.config.js -->
    module.exports = {
        module:{
            rules:[
                {
                    test:"/\.css$/",
                    <!-- 使用自定义loader -->
                    use:["./loaders/css-loader.js"]
                }
            ]
        }
    }
```

---

### plugin

#### 作用

> webpack 插件是一个具有 **apply** 方法的 JavaScript 对象。apply 方法会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问

> 在webpack打包运行的时候会执行导出的插件，并在apply方法中注册事件，待到达特定时机触发事件执行相应的处理

#### 用法

[tapable类, 提供了插件类型](https://v4.webpack.docschina.org/api/plugins/#tapable)

> 这个类暴露 tap, tapAsync 和 tapPromise

1. **tap** 以同步方式触及 compile 钩子
2. **tapAsync** 以异步方式触及 run 钩子

3. 

``` 

<!-- 声明自定义插件 plugins/myPlugin.js-->
module.exports = class MyPlugin {
    constructor(params) {
   
    }
    <!-- 插件必须存在一个apply方法 -->
    <!-- webpack打包运行时会执行该插件的apply方法 -->
    apply(compiler){
        <!-- 注册事件 -->
        <!-- Tapable类 -->
    
        compiler.hooks.事件名.tap("",(compilation)=>{
            <!-- 事件回调 -->
        })
    }
}

<!-- 引入自定义插件 -->
const MyPlugin = require("./plugins/myPlugin");

module.exports = {
    mode:"development",
    plugins:[
        new MyPlugin(xxx);
    ]
}

```

---

### 环境配置

#### 方法一

``` 

<!-- package.json -->
scripts:{
"dev":"webpack --mode=development"
}
```

#### 方法二

``` 

<!-- webpack.config.js -->
module.exports = {
    mode:"development"
}
```

#### 方法三

``` 

<!-- package.json -->
scripts:{
"dev":"webpack --env mode=development"
}

<!-- webpack.config.js -->
module.exports = (env)=>{
    if(env.mode==="development"){
        <!-- 环境为开发环境 -->
    }
}

```

#### --env写法

1. --env development

``` 

获取到的env参数为：
{
    development:true
}

```

2. --env mode=development

``` 

获取到的env参数为：
{
    mode:development
}

```

**注意mode=development中间不能出现空格，如:mode= development**

### 其他细节配置

#### context

> 基础目录，绝对路径，用于从配置中解析入口点(entry point)和 加载器(loader)

``` 

module.exports = {
    context:path.resolve(__dirName,"src")
}
```

#### output.library

[output.library ](https://webpack.docschina.org/configuration/output/#outputlibrary)

* 将打包的结构暴露给变量abc, 可以通过访问abc获取打包结果

#### output.libraryTarget

[output.libraryTarget ](https://webpack.docschina.org/configuration/output/#outputlibrarytarget)

*  配置如何暴露 library

### target

> 构建的目标环境，构建好的文件是在什么环境运行

``` 

{
    target:"web" // 默认值
    target:"node" //node环境  解析fs，path等node内置模块的时候不回去解析依赖
}

```

### module.noParse

> 对配置的模块不进行解析(提高打包效率)

``` 

module.exports =  {
    module:{
        rules:[], //loader
        noParse:/a\.js$/  //不对a模块对任何操作，直接将源代码放置到模块内容中
    }
}

```

#### resolve.modules

> 模块的查找位置, 当遇到require("vant")的时候, 会在当前目录查找**node_modules**文件如果没有会去上级目录查找依次循环没找到就会报错, 这里控制的就是查找的文件夹默认值是**node_modules**

``` 

module.exports = {

    resolve:{
        modules:["node_modules"]  //默认值
    }
}

```

#### resolve.extensions

> 当解析模块时，遇到无具体后缀的导入语句，例如require("test"), 会依次测试它的后缀名

``` 

module.exports = {
    resovle:{
        extensions:[".js",".json"] // 默认值
    }
}

```

* 当导入模块没有书写后缀名的时候，更具默认配置[".js", ".json"]先查找.js的文件没有在找.json在没有就找不到模块了

#### resolve.alias

> 别名, 类似vue中的@别名

``` 

module.exports = {
    resolve:{
        alias:{
            "@":path.resolve(__dirName,"src")
        }
    }
}

```

#### externals

> 配置webpack不需要导入的模块，例: 页面引入了jquery但是是通过cdn引入所以页面中依赖的jq不需要导入到打包模块中

``` 

module.exports = {
    externals:{
        <!-- 忽略的模块名:导出的内容 -->
        jquery:"$",
        lodash:"_"
    }
}
```

#### stats

> 控制命令行的输出

[stats配置](https://webpack.docschina.org/configuration/stats/)

### 常用扩展

#### clean-webpack-plugin

> 插件 清除打包时的文件

#### html-webpack-plugin

> 插件 打包的时候自动生成html页面

#### copy-webpack-plugin

> 插件 复制文件

#### webpack-dev-server

开启本地服务器
 

``` json
 {
     "scripts":{
        "dev":"webpack-dev-server"
        }
 }
 ```

 **开启服务器代理**
 

``` js
    module.exports = {
        devServer: {
            port: 8080, //设置端口
            open: true,
            proxy: {
                "/api": {
                    target: "https://read.douban.com", //代理的地址
                    changeOrigin: true, //默认情况下，代理时会保留主机头的来源，可以将 changeOrigin 设置为 true 以覆盖此行为
                    secure: true //默认情况下，将不接受在 HTTPS 上运行且证书无效的后端服务器。 如果需要，可以这样修改配置
                }
            }
        }
    }
```

#### file-load

> 生成依赖的文件到输出目录，然后将模块文件设置为：导出一个路径

``` js
 module.exports = {
     module: {
         rules: [{
             test: /\.(png)|(gif)|(jpg)$/,
             use: [{
                 loader: "file-loader",
                 options: {
                     name: "[name].[hash:5].[ext]"
                     name: "img/[name].[hash:5].[ext]"
                 }
             }]
         }]
     }
 }
```

#### url-load

> 将依赖的文件转换为: 导出一个base64格式的字符串, 内部会使用file-loader, 当url-loader无法处理不了的时候会交给file-loader, options配置中可以写file-loader和url-loader的配置

 

``` js
 module.exports = {
     module: {
         rules: [{
             test: /\.(png)|(gif)|(jpg)$/,
             use: [{
                 loader: "url-loader",
                 options: {
                     limit: false //不限制任何大小，所有经过loader的文件进行base64编码返回
                     limit: 100 * 1024 //只要文件不超过100*1024字节则使用base64，否则，交给file-loader处理
                 }
             }]
         }]
     }
 }
```

#### webpack内置插件

##### definePlugin

> 定义全局常量, js中可以直接使用, webpack在编译的时候会替换

``` js
const webpack = require("webpack");
module.exports = {
    plugins: [
        new webpack.DefinePlugin({
            PI: `Math.PI`
        })
    ]
}
```

##### ProvidePlugin

> 自动引入模块, 遇到页面中使用该模块会自动引入模块，没有使用则不会处理

``` js
const webpack = require("webpack");
module.exports = {
    plugins: {
        new webpack.ProvidePlugin({
            $: "jquery",
            _: "lodash"
        })
    }
}
```

## css工程化

**解决重复样式的方案**

1. css in js
2. 预编译器scss,less
3. css  module

> 开启css module后

``` js
module.exports = {
    module: {
        rules: [{
            test: /\.css$/,
            // 开启css module
            // use: ["style-loader", "css-loader?module"]
            use: ["style-loader", {
                loader: "css-loader",
                options: {
                    module: true //开启css module
                }
            }]
        }]
    }
}
```

#### 抽离Css

##### mini-css-extract-plugin

``` js
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

module.exports = {
    output: {
        // 解决因为将打包文件放入文件夹后出现的资源引入错误问题
        punlicPath: "/"
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: [MiniCssExtractPlugin.loader, "css-loader"]
        }]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: "css/xxx.css"
        })
    ]
}
```
