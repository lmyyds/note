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

``` 
{
    <!-- 配置选项 -->
    "compilerOptions":{
        "target": "ES05"  //目前默认值  决定了ts编译成es后使用的语法标准  取值如下：
        /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
        "module":"commonJs"  //默认值  使用的模块化标准  取值如下：
        /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
        "outDir": "./dist"  //设置ts编译后文件存放的路径  注意：使用tsc全局编译时生效，单个编译无效 
        "strictNullChecks": true  //更加严格的空类型检查，null和undefined只能赋值给自身
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
