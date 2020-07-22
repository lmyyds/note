# TypeScript（javaScript的超集）

**什么是超集**

> typeScript包含javaScript的所有语法，并在javaScript的基础上新增了新特性, 例：类型系统, javaScript无法实现的功能typeScript依旧无法实现。

### 特点

### 优点

## 配置文件

**tsconfig.json**

**创建配置文件**

``` 
方法一：tsc --init  指令生成
方法二：创建文件夹命名为tsconfig.json 手动生成
```

**配置**

|      配置名称        |              含义              |
| :-----------------: | :----------------------------: |
|       target        |  决定了ts编译成es后使用的语      |
|       module        |       使用的模块化标准          |
|       outDir        |  设置ts编译后文件存放的路径      |
|   strictNullChecks  |        更加严格的空类型检查，null, undefined只能赋值给自身 |
|  moduleResolution   |       设置解析模块的模式       |
| noImplicitUseStrict/alwaysStrict |  编译结果中不包含"use strict"  |
|   removeComments    |        编译结果移除注释        |
|    noEmitOnError    |      错误时不生成编译结果      |
|   esModuleInterop   |  启用es模块化交互非es模块导出  |

``` 
{
    <!-- 配置选项 -->
    "compilerOptions":{
        "target": "ES5"  //目前默认值  决定了ts编译成es后使用的语法标准  取值如下：
        /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
        "module":"commonJs"  //默认值  使用的模块化标准  取值如下：
        /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
        "outDir": "./dist"  //设置ts编译后文件存放的路径  注意：使用tsc全局编译时生效，单个编译无效 
        "strictNullChecks": true  //更加严格的空类型检查，null和undefined只能赋值给自身
        "removeComments":true  //是否编译过滤注释
    },
    "include":["./src"],  //设置tsc要编译的ts文件夹路径,默认全局编译
    "files": ["./aa.ts", "./src/tsDemo.ts"]  //设置tsc要编译的文件路径
}
```

## 工具库

**@types/node**

> @types是一个t官方的类型库，其中包含了很多对js代码的类型描述

**ts-node**

> 将ts代码在内存中完成编译，同时完成运行！  不能-D安装  推荐全局安装-g

``` 
npm i -g ts-node
```

**nodemon**

> 监控目录实时更新(热更新)  不能-D安装  推荐全局安装-g

``` 
npm i -g nodemon
```

``` 
 --watch 目录名 监控目录
 -e ts(文件后缀名) 监控ts文件
 nodemon --watch src -e ts --exec ts-node src/index.js
```

## 约束(类型检查)

* **变量约束**

``` 
let a:number = 1  //正确  只接收number类型的变量
let a:number = "1"  //错误
```

* **返回值约束**

``` 
function handle(a,b):number {
    return a+b;   //a+b必须是一个number类型
}

let a:number = handle()
```

* **参数约束**

``` 
function handle(a:number,b:string){
    return a+b;
}

```

* **类型推断**

> ts会根据你传入的值或者接收的值进行推断出你当前变量或者返回值和参数是什么类型的值  

``` 
function handle(a:number,b:string){
    return a+b;
}

函数handle的返回参数类型为string  因为a的类型为number，b的类型为string 数字加字符串为字符串

```

### 基本类型

1. **number**

``` 
 let a:number = 1
```

2. **string**

``` 
 let a:string = "1"
```

3. **boolean**

``` 
 let a:boolean = true
```

4. **null**

``` 
 let a:null = null
```

5. **undefined**

``` 
 let a: undefined;
 let a: undefined = undefined;
```

6. **object**

``` 
 let a:number = 1
```

7. **Array**

``` 
 let a:Array<number> = [1]
 let a:Array<number> = [1]
 let a:number[] = [1,2,3]               //:number[] 是 :Array[number]的语法糖
 let a:boolean[] = [true,false]
```

### 其他类型

* **联合类型**

> 多种类型任选其一

``` 
let a:number|string;  //变量a即接收number也接收string类型的值
```

* **联合类型(类型保护)**

> 当某个变量进行类型判断后，在判断的语句块中便可以确定它的确切类型
> typeof可以触发类型保护，但是只限简单类型 Array和object无法触发

``` 
let a:number|undefined; //可能是number可能是undefined
if(typeof a === "number"){
    <!-- 这里的变量a一定是一个number -->
}

```

* **联合类型(函数重载)**

``` 
```

* **元祖类型**

>一个固定长度的数组,并且数组中每一项的类型确定
```

 let arr = [number, string, boolean]

     arr = [1, "1", true] //正确
     arr = [1, "1", true, 1] //错误
     arr = [1, "1"] //错误
     arr = [1, 1, true] //错误

``` 
* **any**

> any类型ts不会检查，绕过了类型检查,因此，any类型的数据可以赋值给任意类型

```

let a:any = "1"

    a = 1
    a = true

let b:any = "1"
let c:string = b  //不建议随意使用any类型  存在隐患类型指定不明确

``` 
* **字面量类型**

```

let sex:"男"|"女" = "男"  //正确
let sex:"男"|"女" = "nan"  //错误

let user:{

    name:string
    age:number

}

let user = {name:"limao", age:18} //正确 必须存在name和age并且符合类型

``` 
* **void类型**

>通常用来约束函数的返回值,表示该函数没有任何返回值

```

function handle():void {

    console.log("我就打印打印!")

}

``` 
* **never类型**

函数不会结束，不会有返回值

```

    function handle():never {
        throw new Error("对方不想和你说话，并抛出了一个错误!")

    }

``` 

### 类型别名

```

type 类型名 = xxx

``` 

### 函数的相关约束

1. **可选参数**

>在某些参数名后加上问号，表示该参数可以不用传递
>c?:number/c:number = 0 可选参数(默认参数) 两种写法一样的意思

```

    function handle(a:number,b:sring,c?:number){

    }

    function handle(a:number, b:sring, c:number=0){

    }

``` 
2. **函数重载**

> 在函数实现之前，对函数调用的多种情况进行声明

```

<!-- 当前函数的传参只有两种情况，并对每一种情况进行约束 -->
<!-- 情况一：两个参数都传的字符串 -->
function greeter(num: string, str: string): string; 
<!-- 情况一：两个参数都传的数字 -->
function greeter(num: number, str: number): number; 
function greeter(num: number | string, str: number | string): number | string {

    <!-- 类型保护 -->
    if (typeof num === "number" && typeof str === "number") {
        return num + str; 

    }

    if (typeof num === "string" && typeof str === "string") {
        return num + str; 

    }

    <!-- 当接收的值不符合以上两种情况的时候抛错，不返回任何值！ -->
    throw new Error("a和b必须是相同的类型")

}

    greeter("1", 1)  //如果没有使用函数重载这里是不会报错的！ 当前函数的逻辑不支持参数一个传string一个传number
    greeter(1, 1)    //类型检查自动判断函数的返回值为number
    greeter("1", "1")  //类型检查自动判断函数的返回值为string

``` 

## 枚举(扩展类型)

> 扩展类型：类型别名，枚举，接口，类
> 定义: 枚举通常用于约束某个变量的取值范围。

字面量和联合类型配合使用，也可以达到同样的目标。

如何定义一个枚举：
```

enum 枚举名{

    枚举字段1 = 值1,
    枚举字段2 = 值2,
    ...

}

``` 
**字面量类型的问题** 

1. 在类型约束位置，会产生重复代码。可以使用类型别名解决该问题。
2. **逻辑含义**和**真实的值**产生了混淆，会导致当修改真实值的时候，产生大量的修改。

```

<!-- 使用真实值 -->
type gender = "男" | "女"; 
let sex: gender = "男"
function isSex(type: gender) {

    return type

}
isSex("男")

``` 
>缺点：当我们需要去修改gender的值的时候，比如将"男"和"女"x修改成"先生"和"女士"由于之后使用的是真实值("男"|"女"), 而我们修改的也是真实值所有涉及到的所有真实值都需要去修改，这样就需要大量的修改。
```

<!-- 使用逻辑值 -->
enum gender {

    <!-- male 是逻辑值  "男" 是真实值 -->
    "male" = "男",
    "female" = "女"

}; 
let sex: gender = gender.male
function isSex(type: gender) {

    return type

}
isSex(gender.male)
```

> 由于使用的是逻辑值，没有使用真实值，如果遇到更改"male"为"男士", 那么只需要修改枚举中的真实值即可

3. 字面量类型不会进入到编译结果。

> **枚举会出现在编译结果中，编译结果中表现为对象**。

**枚举的规则：**

1. 枚举的字段值可以是字符串或数字
2. 数字枚举的值会自动自增
3. 被数字枚举约束的变量，可以直接赋值为数字
4. 数字枚举的编译结果 和 字符串枚举有差异

**注意事项：**

1. 尽量不要在一个枚举中即出现数字又出现字符串
2. 使用枚举时，尽量使用枚举值，不使用真实值

## 模块化(了解)

|      配置名称       |              含义              |
| :-----------------: | :----------------------------: |
|       module        | 设置编译结果中使用的模块化标准 |
|  moduleResolution   |       设置解析模块的模式       |
| noImplicitUseStrict |  编译结果中不包含"use strict"  |
|   removeComments    |        编译结果移除注释        |
|    noEmitOnError    |      错误时不生成编译结果      |
|   esModuleInterop   |  启用es模块化交互非es模块导出  |

> 前端领域中的模块化标准：ES6、commonjs、amd、umd、system、esnext

**TS中如何书写模块化语句**

TS中，导入和导出模块，统一使用ES6的模块化标准(尽量使用es6模块化规范，简单！)

**编译结果中的模块化**

TS中的模块化在编译结果中：

* 如果编译结果的模块化标准是ES6： 没有区别
* 如果编译结果的模块化标准是commonjs：导出的声明会变成exports的属性，默认的导出会变成exports的default属性；

**如何在TS中书写commonjs模块化代码**

导出：export = xxx

导入：import xxx = require("xxx")

**模块解析**

模块解析：应该从什么位置寻找模块

TS中，有两种模块解析策略

* classic：经典
* node：node解析策略（唯一的变化，是将js替换为ts）
  + 相对路径 ` `  ` require("./xxx") `  ` `
  + 非相对模块 ` `  ` require("xxx") `  ` `

## 接口(扩展类型)
