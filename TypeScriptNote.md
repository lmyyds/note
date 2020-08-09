# TypeScript（javaScript的超集）

**什么是超集**

> typeScript包含javaScript的所有语法，并在javaScript的基础上新增了新特性, 例：类型系统, javaScript无法实现的功能typeScript依旧无法实现。

### 特点

### 优点

---

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

---

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
---

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
---

## 接口(扩展类型)

接口：inteface

> 扩展类型：类型别名、枚举、接口、类

TypeScript的接口：用于约束类、对象、函数的契约（标准）

契约（标准）的形式：

* API文档，弱标准
* 代码约束，强标准

和类型别名一样，接口，不出现在编译结果中

```

<!-- 声明一个"生物"的接口 -->
interface  biology{

            name:string,
            age:number,
            introduce():string

}

``` 

### interface和type的区别

#### 相同点

* **都可以描述一个对象或者函数**

**interface**
```

    interface User {

    name: string
    age: number

    }

    interface SetUser {
    (name: string, age: number): void; 

    }

``` 
**type**
```

    type User = {

    name: string
    age: number

    }; 

    type SetUser = (name: string, age: number)=> void; 

``` 
* **都允许拓展(extends)**

**interface extends interface**

```

    interface Name { 
    name: string; 

    }

    interface User extends Name { 
    age: number; 

    } 

``` 
**type extends type**

```

    type Name = { 
    name: string; 

    }

    type User = Name & { age: number  }; 

``` 
**interface extends type**
```

    type Name = { 
    name: string; 

    }

    interface User extends Name { 
    age: number; 

    }

``` 
**type extends interface**
```

    interface Name { 
    name: string; 

    }

    type User = Name & { 
    age: number; 

    }

``` 

#### 不同点

* **type 可以而 interface 不行**

**type 可以声明基本类型别名，联合类型，元组等类型**
```

    // 基本类型别名
    type Name = string

    // 联合类型
    interface Dog {

        wong();

    }

    interface Cat {

        miao();

    }

    type Pet = Dog | Cat

    // 具体定义数组每个位置的类型
    type PetList = [Dog, Pet]

``` 
**type 语句中还可以使用 typeof 获取实例的 类型进行赋值**
```

    // 当你想获取一个变量的类型时，使用 typeof
    let div = document.createElement('div'); 
    type B = typeof div 

``` 
**其他骚操作**
```

    type StringOrNumber = string | number; 
    type Text = string | { text: string }; 
    type NameLookup = Dictionary<string, Person>; 
    type Callback<T> = (data: T) => void; 
    type Pair<T> = [T, T]; 
    type Coordinates = Pair<number>; 
    type Tree<T> = T | { left: Tree<T>, right: Tree<T> }; 

``` 
* **interface 可以而 type 不行**

**interface能够声明合并**
```

    interface User {

    name: string
    age: number

    }

    interface User {

    sex: string

    }

   // User 接口为 {
   // name: string
   // age: number
   // sex: string 

   // } 

``` 
**interface 有可选属性和只读属性**

```

    interface Person {
    name: string; 
    age?: number; 
    gender?: number; 

    }

    interface User {

        readonly loginName: string;
        password: string;

    }

``` 

### 接口继承

可以通过接口之间的继承，实现多种接口的组合
```

    <!-- 声明一个水果接口 -->
    interface fruits{
        name:string, 
        price:number

    }

    <!-- 声明一个香蕉接口 -->
    interface banana extends fruits{

        introduce(): string

    }

    <!-- 创建一个香蕉 -->

    let createBanana: banana = {

    name: "香蕉", 
    price: 1, 
    introduce() {
        return "我是香蕉~"

    }

    //不仅需要拥有introduce方法还需要拥有name和price属性，继承自fruits

}

``` 
* 接口还可以继承类型别名(偶然发现滴~)

```

    <!-- 这是类型别名不是接口 -->
    type fruits = {

        name: string,
        price: number

    }

    interface banana extends fruits {

        introduce(): string

    }

    let createBanana: banana = {

        name: "香蕉", 
        price: 1, 
        introduce() {
            return "我是香蕉~"

        }

    }

``` 
使用类型别名可以实现类似的组合效果，需要通过 ` `  ` & `  ` ` ，它叫做交叉类型

```

    type fruits = {

        name: string,
        price: number

    }

    <!-- 通过&符号关联,表示banana同时拥有banana和fruits -->

    type banana = {

        introduce(): string

    } & fruits

    let createBanana: banana = {

        name: "香蕉", 
        price: 1, 
        introduce() {
            return "我是香蕉~"

        }

    }

``` 
它们的区别：

1.  子接口不能覆盖父接口的成员

>类型别名(type)
```

    type fruits = {

        name: string,
        price: number

    }

    type banana = {

        introduce(): string,

        name: number  //影响了fruits中的name的类型 

    } & fruits

    let createBanana: banana = {

        name: "123",   // 这里提示name的类型为never
        price: 1, 
        introduce() {
            return "我是香蕉~"

        }

    }

``` 
>接口(interface)

```

    interface fruits {

        name: string,
        price: number

    }

    //报错：信息如下
    //接口“banana”错误扩展接口“fruits”。
    //属性“name”的类型不兼容。
    //不能将类型“number”分配给类型“string”。

    interface banana extends fruits {

        introduce(): string, 
        name: number   

    }

    let createBanana: banana = {

        name: "123", 
        price: 1, 
        introduce() {
            return "我是香蕉~"

        }

    }

``` 
2.  交叉类型会把相同成员的类型进行交叉

###  readonly

只读修饰符，修饰的目标是只读

只读修饰符不在编译结果中

```

<!-- 声明一个生物的接口 -->
interface  biology{

   readonly species:"person", //readonly 声明赋值后将无法修改(只读)

            name:string,
            age:number,
            introduce():string

            hobby?:string[]   //选传属性 也可以写成hobby:string[] = []

}

``` 
>声明一个不可变的数组
```

<!-- 该数组创建后无法修改里面的值，也无法使用改变数组的方法 -->
let readonlyArr: ReadonlyArray<number> = [1, 2, 3, 4]

``` 
* **怎么判断是使用const还是readonly**
* 如果作用于变量使用const

```

const name = "limao"

``` 
* 如果作用于属性使用readonly

```

class User{

    readonly name = "limao"    

}

``` 

###  类型兼容性

B->A，如果能完成赋值，则B和A类型兼容

鸭子辨型法（子结构辨型法）：目标类型需要某一些特征，赋值的类型只要能满足该特征即可

* 基本类型：完全匹配

* 对象类型：鸭子辨型法（子结构辨型法）

>场景：
**对象字面量**当我们直接使用对象字面量的方式对对象进行添加属性或者方法的时候,ts会严格根据类型检查我们的对象属性和方法有没有问题
**变量**当我们使用一个变量，变量里存放一个对象的方式来赋值给指定类型了的值的时候会采用**子结构辨型法**只要赋值的对象中包含了类型约束中的属性和函数的时候就判定当前变量里存放的值就是当前类型约束的值
```

    interface Duck {

        sound: "嘎嘎嘎",
        swin(): void

    }

    let person = {

        name: "limao", 
        age: 18, 
        sound: "嘎嘎嘎" as "嘎嘎嘎", //断言位"嘎嘎嘎"类型
        swin() {
            console.log(this.name + "正在游泳，并发出了" + this.sound + "的声音"); 

        }

    }

    let duck: Duck = person //不报错

``` 
```
    <!-- 报错 -->
    let duck: Duck = {

        name: "limao", 
        age: 18, 
        sound: "嘎嘎嘎" as "嘎嘎嘎", //断言位"嘎嘎嘎"类型
        swin() {
            console.log(this.name + "正在游泳，并发出了" + this.sound + "的声音"); 

        }
        
    }
```

### 类型断言

当直接使用对象字面量赋值的时候，会进行更加严格的判断

* 写法一

``` 
    let createBanana: banana = {

        name: "123",
        price: 1,
        introduce() {
            return "我是香蕉~"
        },
        color: "yellow"  //由于新增了一个属性ts判断当前对象不是banana类型但其实我知道这就是banana类型

    } as banana  //我们直接断言这就是banana类型通过在末尾跟上 "as 类型"

```

* 写法二

``` 
    //我们直接断言这就是banana类型通过在开头跟上 "<类型>"

    let createBanana: banana = <banana>{

        name: "123",
        price: 1,
        introduce() {
            return "我是香蕉~"
        },
        color: "yellow"  //由于新增了一个属性ts判断当前对象不是banana类型但其实我知道这就是banana类型

    } 

```

### 函数类型

> 一切无比自然

1. **参数**：传递给目标函数的参数可以少，但不可以多

2. **返回值**：要求返回必须返回；不要求返回，你随意

---

## 类(扩展类型)

> 面向对象思想

### 属性

使用属性列表来描述类中的属性

``` 
    class User {
        name: string;
        age: number;
        constructor(name: string, age: number) {
            this.name = name;
            this.age = age;
        }

        intro() {
            console.log( `大家好我叫${this.name}今年${this.age}岁啦~~~` )
        }
    }

```

### 属性的初始化检查

**tsconfig.json 配置**
` `  ` strictPropertyInitialization:true 开启类型初始化检查`  ` `
属性的初始化位置：

1. 构造函数中
2. 属性默认值

``` 
    class User {
        name: string = "limao";
        age: number;
        constructor(name: string, age: number = 18) {
            this.name = name;
            this.age = age;
        }

        intro() {
            console.log( `大家好我叫${this.name}今年${this.age}岁啦~~~` )
        }
    }

    new User()  //{ name: 'limao', age: 18 }
```

**属性可以修饰为可选的**

``` 
```

**属性可以修饰为只读的**
>通过关键字readonly将属性修饰成只读属性
```

    class User {

  readonly  name: string; 

            age: number;
            constructor(name: string = "limao", age: number = 18) {
                this.name = name;
                this.age = age;

            }

            intro() {
                console.log( `大家好我叫${this.name}今年${this.age}岁啦~~~` )

            }

        }

    new User()  //{ name: 'limao', age: 18 }

``` 

### 使用访问修饰符

访问修饰符可以控制类中的某个成员的访问权限

* public：默认的访问修饰符，公开的，所有的代码均可访问
* private：私有的，只有在类中可以访问
* protected：暂时不讲

```

    class User {

            name: string; 
            age: number;
            constructor(name: string = "limao", age: number = 18) {
                this.name = name;
                this.age = age;

            }

            public intro() {
                console.log( `大家好我叫${this.name}今年${this.age}岁啦~~~` )

            }

            private secret(){
                console.log( `其实我已经${this.age+1}岁啦~~` )

            }

        }

   let user =  new User()
   user.intro(); //大家好我叫limao今年18岁啦~~~
   user.secret() //error 无法调用该方法 该方法为私有方法，只能在类中调用

``` 

### 属性简写

如果某个属性，通过构造函数的参数传递，并且不做任何处理的赋值给该属性。可以进行简写

```

    class User {

            constructor(public name: string = "limao",public age: number = 18) {

            }

            public intro() {
                console.log( `大家好我叫${this.name}今年${this.age}岁啦~~~` )

            }

            private secret(){
                console.log( `其实我已经${this.age+1}岁啦~~` )

            }

        }

``` 

### 访问器

作用：用于控制属性的读取和赋值,避免直接操作属性，通过暴露的指定方法进行操作
>方法一

```

    class User {

        constructor(public name: string, private _age: number) {

        }

        intro() {
            console.log( `大家好我叫${this.name}今年${this._age}岁啦~~~` )

        }

        public setAge(age: number) {
            if (age >= 200) {
                this._age = 200; 

            }

            else if (age < 0) {
                this._age = 0; 

            }

            else {
                this._age = Math.floor(age)

            }

        }

        public getAge() {
            return this._age; 

        }

    }

``` 
>方法二 类似vue的计算属性

```

    class User {

        constructor(public name: string, private _age: number) {

        }

        intro() {
            console.log( `大家好我叫${this.name}今年${this._age}岁啦~~~` )

        }

        <!-- 注意方法名不能和属性名一致否则会进入无限递归，造成栈溢出 -->
        set age(age: number) {
            if (age >= 200) {
                this._age = 200; 

            }

            else if (age < 0) {
                this._age = 0; 

            }

            else {
                this._age = Math.floor(age)

            }

        }

        get age() {
            return this._age; 

        }

    }

    let user = new User("limao", 18); //{ name: 'limao', age: 18 }
    console.log(user.age = 500)
    console.log(user)  //{ name: 'limao', age: 200 }

``` 

## 泛型(扩展类型)

> 泛型：是指附属于函数、类、接口、类型别名之上的类型

> 泛型相当于是一个类型变量，在定义时，无法预先知道具体的类型，可以用该变量来代替，只有到调用时，才能确定它的类型

> 很多时候，TS会智能的根据传递的参数，推导出泛型的具体类型
> 如果无法完成推导，并且又没有传递具体的类型，默认为空对象

* 泛型可以设置默认值

**在函数中使用泛型**

* 在函数名之后写上 ` `  ` <泛型名称> `  ` `
```

    function concatArray<T>(targetArr: T[], originArr: T[]): T[] {

        return targetArr.concat(originArr);

    }

    concatArray<number>([1, 1, 2], [2, 3])

``` 
**如何在类型别名、接口、类中使用泛型**
```

在类型别名中使用泛型

    type callback<T> = (n:T,i:number) => boolean;

    function filter<T>(arr: T[], fn: callback<T>) {

    }

    let arr: string[] = ["1", "1", "2"]
    filter<string>(arr, n => !n)

``` 
```

    在接口中使用泛型
    interface callback<T> {

        (a: T, b: T): boolean

    }

    function filter<T>(arr: T[], fn: callback<T>) {

    }

    let arr: string[] = ["1", "1", "2"]
    filter<string>(arr, n => !n)

```

``` 
    在类中使用泛型
    class ArrayHlper<T> {
    constructor(private arr: T[]) { }
    }

```

<!-- **如何在类型别名、接口、类中使用泛型** -->
<!-- 直接在名称后写上 ` `  ` <泛型名称> `  ` ` -->

### 泛型约束

泛型约束，用于现实泛型的取值

### 多泛型

---

## 类的继承(extends)

 **继承的作用**

继承可以描述类与类之间的关系

> 坦克、玩家坦克、敌方坦克
> 玩家坦克是坦克，敌方坦克是坦克

如果A和B都是类，并且可以描述为A是B，则A和B形成继承关系：

* B是父类，A是子类
* B派生A，A继承自B
* B是A的基类，A是B的派生类

如果A继承自B，则A中自动拥有B中的所有成员

**成员的重写**

重写(override)：子类中覆盖父类的成员

子类成员不能改变父类成员的类型

无论是属性还是方法，子类都可以对父类的相应成员进行重写，但是重写时，需要保证类型的匹配。

注意this关键字：在继承关系中，this的指向是动态——调用方法时，根据具体的调用者确定this指向

super关键字：在子类的方法中，可以使用super关键字读取父类成员

``` 
enum type {
    "dog" = "狗",
    "tiger" = '老虎'
}

class Animal {
    constructor(public type: type, public name: string, public age: number) { }
    sayHello() {
        console.log( `大家好！` )
    }
}

class dog extends Animal {

    constructor(public type: type, public name: string, public age: number) {
        //当父类的constructor中存在的参数有必填项的时候他的派生类或者说子类中必须执行super并传入
        //父类中必填项的属性名
        super(type, name, age)
        //分别打印   大家好！              大家好！
        //两种调用打印结果一致，因为子类中不存在sayhello方法，所以在用this调用的时候
        //会先去子类(自己)身上找没找到就去父类身上找，找到了就执行。super.sayhello则是
        //表示调用父类身上的方法，会直接去父类身上找
        console.log(super.sayHello(), this.sayHello())
    }
}

```

``` 
enum type {
    "dog" = "狗",
    "tiger" = '老虎'
}

class Animal {
    constructor(public type: type, public name?: string, public age?: number) { }
    sayHello() {
        console.log( `大家好！` )
    }
}

class dog extends Animal {

    constructor(public type: type, public name: string, public age: number) {
        //由于父类的name和age的值是选传而不是毕传所以super可以不用传入选传的参数
        super(type)
        //如果父类中的所有参数都是选传那么则可以不用传入任何参数
        // super()
        //分别打印   大家好！              大家好我是子类!
        //super会直接去找父类的方法找到并执行，this则是先找子类再找父类
        console.log(super.sayHello(), this.sayHello())
    }

    sayHello(){
        console.log( `大家好我是子类!` )
    }
}

```

**类型匹配**

鸭子辨型法(为啥叫这玩意我也不晓得。。。官方点就叫类型匹配)

**子类的对象，始终可以赋值给父类**

> 因为子类中始终包含父类(必填)的属性和方法, 所以子类的对象一定满足父类的类型匹配要求, 注意：此类型匹配只适用于变量赋值给变量不适用于直接字面量赋值，直接字面量赋值会有严格的类型检查
> 为什么会有这种差异！字面量赋值一定是你自己赋值编写的，ts不允许你犯这种错误所以严格执行类型检查，但是变量考虑到比如你在发起请求的时候，因为请求到的数据会返回除开你使用的属性之外还有许多其他属性，但是这里关系就是你使用的属性一定是返回数据里的属性也就是说返回的数据一定包含你要使用的属性，你要使用的属性又会伴随着类型约束，这里如果采用严格的类型检查则回报错，所以ts很智能的改为了弱类型检查

面向对象中，这种现象，叫做里氏替换原则

如果需要判断一个数据的具体子类类型，可以使用instanceof

``` 
enum type {
    "dog" = "狗",
    "tiger" = '老虎'
}

enum gender {
    "male" = "公",
    "female" = "母"
}

class Animal {
    constructor(protected type: type, protected name: string, public age: number) { }
    sayHello() {
        console.log( `大家好！` )
    }
}

class Dog extends Animal {

    constructor(protected type: type, public name: string, public age: number, public gender: gender) {
        super(type, name, age)
    }
}

let dog = new Dog(type.dog, "来福", 2, gender.male)
let animal = new Animal(type.tiger, "阿威", 2)
console.log(dog, animal) //Dog { type: '狗', name: '来福', age: 2, gender: '公' } Animal { type: '老虎', name: '阿威', age: 2 }
animal = dog
// dog = animal  报错
console.log(animal)  //Dog { type: '狗', name: '来福', age: 2, gender: '公' }
```

**类中遵循这种类型匹配接口中也是一样**

``` 
interface father {
    name: string
    age: number

}
interface son extends father {
    sayHello: () => void
}

let father: father = {
    name: "李四",
    age: 40
}

let son: son = {
    name: "王五",
    age: 10,
    sayHello() {
        console.log( `爸,我是从哪里来的!` )
    }
}
//可以正常赋值 变量赋值会遵循类型匹配采用较宽松的类型检查
let father_: father = son;
//报错
let son_: son = father;
//报错  字面量赋值会有严格的类型检查
//let father_: father = {
//    name: "王五",
//    age: 10,
//    sayHello() {
//        console.log( `爸,我是从哪里来的!` )
//    }
//}

```

**protected修饰符**

readonly：只读修饰符

访问权限修饰符：private public protected
public: 开放的，都可以访问的
protected: 私有的，处了类自身其他人都不可以访问
protected: 受保护的成员，只能在自身和子类中访问

``` 
enum type {
    "dog" = "狗",
    "tiger" = '老虎'
}

class Animal {
    constructor(protected type: type, protected name: string, public age: number) { }
    sayHello() {
        console.log( `大家好！` )
    }
}

class dog extends Animal {

    constructor(protected type: type, public name: string, public age: number) {
        super(type, name, age)
    }
}

let d = new dog(type.dog, "来福", 2)

//报错 无法通过ts的类型检查 因为type属性是protected受保护的成员，只能自身和子类访问
console.log(d.type)

//可以访问，虽然父类是protected，但是子类重写了该属性的修饰符
console.log(d.name)
```

**单根性和传递性**

单根性：每个类最多只能拥有一个父类

传递性：如果A是B的父类，并且B是C的父类，则，可以认为A也是C的父类

---

## 抽象类

### 为什么需要抽象类

``` puml
@startuml

棋子 <|-- 马
棋子 <|-- 兵
棋子 <|-- 炮

@enduml
```

有时，某个类只表示一个抽象概念，主要用于提取子类共有的成员，而不能直接创建它的对象。该类可以作为抽象类。

给类前面加上 ` `  ` abstract `  ` ` ，表示该类是一个抽象类，不可以创建一个抽象类的对象。

### 抽象成员

父类中，可能知道有些成员是必须存在的，但是不知道该成员的值或实现是什么，因此，需要有一种强约束，让继承该类的子类，必须要实现该成员。

**抽象类中**，可以有抽象成员，这些抽象成员必须在子类中实现

### 设计模式 - 模板模式

设计模式：面对一些常见的功能场景，有一些固定的、经过多年实践的成熟方法，这些方法称之为设计模式。

模板模式：有些方法，所有的子类实现的流程完全一致，只是流程中的某个步骤的具体实现不一致，可以将该方法提取到父类，在父类中完成整个流程的实现，遇到实现不一致的方法时，将该方法做成抽象方法。

---

## 静态成员

### 什么是静态成员

静态成员是指，附着在类上的成员（属于某个构造函数的成员）

使用static修饰的成员，是静态成员

实例成员：对象成员，属于某个类的对象

静态成员：非实例成员，属于某个类

### 静态方法中的this

实例方法中的this指向的是**当前对象**

而静态方法中的this指向的是**当前类**

### 设计模式 - 单例模式

单例模式：某些类的对象，在系统中最多只能有一个，为了避免开发者造成随意创建多个类对象的错误，可以使用单例模式进行强约束。

## 再谈接口

接口用于约束类、对象、函数，是一个类型契约。

> 有一个马戏团，马戏团中有很多动物，包括：狮子、老虎、猴子、狗，这些动物都具有共同的特征：名字、年龄、种类名称，还包含一个共同的方法：打招呼，它们各自有各自的技能，技能是可以通过训练改变的。狮子和老虎能进行火圈表演，猴子能进行平衡表演，狗能进行智慧表演

> 马戏团中有以下常见的技能：

> - 火圈表演：单火圈、双火圈
> - 平衡表演：独木桥、走钢丝
> - 智慧表演：算术题、跳舞

不适用接口实现时：

* 对能力（成员函数）没有强约束力
* 容易将类型和能力耦合在一起

系统中缺少对能力的定义 —— 接口

面向对象领域中的接口的语义：表达了某个类是否拥有某种能力

某个类具有某种能力，其实，就是实现了某种接口

类型保护函数：通过调用该函数，会触发TS的类型保护，该函数必须返回boolean

接口和类型别名的最大区别：接口可以被类实现，而类型别名不可以

> 接口可以继承类，表示该类的所有成员都在接口中。

---

## 索引器

` `  ` 对象[值] `  ` ` ，使用成员表达式

在TS中，默认情况下，不对索引器（成员表达式）做严格的类型检查

使用配置 ` `  ` noImplicitAny `  ` ` 开启对隐式any的检查。

隐式any：TS根据实际情况推导出的any类型

在索引器中，键的类型可以是字符串，也可以是数字

在类中，索引器书写的位置应该是所有成员之前

TS中索引器的作用

* 在严格的检查下，可以实现为类动态增加成员
* 可以实现动态的操作类成员

在JS中，所有的成员名本质上，都是字符串，如果使用数字作为成员名，会自动转换为字符串。

在TS中，如果某个类中使用了两种类型的索引器，要求两种索引器的值类型必须匹配

---

## this指向约束

https://yehudakatz.com/2011/08/10/understanding-javascript-function-invocation-and-this/

### 在JS中this指向的几种情况

明确：大部分时候，this的指向取决于函数的调用方式

* 如果直接调用函数（全局调用），this指向全局对象或undefined (启用严格模式)
* 如果使用 ` `  ` 对象. 方法 `  ` ` 调用，this指向对象本身
* 如果是dom事件的处理函数，this指向事件处理对象

特殊情况：

* 箭头函数，this在函数声明时确定指向，指向函数位置的this
* 使用bind、apply、call手动绑定this对象

### TS中的this

配置noImplicitThis为true，表示不允许this隐式的指向any

在TS中，允许在书写函数时，手动声明该函数中this的指向，将this作为函数的第一个参数，该参数只用于约束this，并不是真正的参数，也不会出现在编译结果中。

---

## 装饰器

### 概述

> 面向对象的概念（java：注解，c#：特征），decorator
> angular大量使用，react中也会用到
> 目前JS支持装饰器，目前处于建议征集的第二阶段

#### 解决的问题

装饰器，能够带来额外的信息量，可以达到分离关注点的目的。

* 信息书写位置的问题
* 重复代码的问题

上述两个问题产生的根源：某些信息，在定义时，能够附加的信息量有限。

装饰器的作用：为某些属性、类、参数、方法提供元数据信息(metadata)

元数据：描述数据的数据

#### 装饰器的本质

在JS中，装饰器是一个函数。（装饰器是要参与运行的）

装饰器可以修饰：

* 类
* 成员（属性+方法）
* 参数

### 类装饰器

类装饰器的本质是一个函数，该函数接收一个参数，表示类本身（构造函数本身）

使用装饰器 ` `  ` @得到一个函数 `  ` `
在TS中，如何约束一个变量为类

* Function
* ` `  ` new (参数)=>object `  ` `
在TS中要使用装饰器，需要开启 ` `  ` experimentalDecorators `  ` `
装饰器函数的运行时间：在类定义后直接运行

类装饰器可以具有的返回值：

* void：仅运行函数
* 返回一个新的类：会将新的类替换掉装饰目标

多个装饰器的情况：会按照后加入先调用的顺序进行调用。

### 成员装饰器

* 属性

属性装饰器也是一个函数，该函数需要两个参数：

1. 如果是静态属性，则为类本身；如果是实例属性，则为类的原型；
2. 固定为一个字符串，表示属性名

* 方法

  
方法装饰器也是一个函数，该函数需要三个参数：

1. 如果是静态方法，则为类本身；如果是实例方法，则为类的原型；
2. 固定为一个字符串，表示方法名
3. 属性描述对象

可以有多个装饰器修饰

---

## 类型演算

> 根据已知的信息，计算出新的类型

### 三个关键字

* typeof

TS中的typeof，书写的位置在类型约束的位置上。

表示：获取某个数据的类型

当typeof作用于类的时候，得到的类型，是该类的构造函数

* keyof

作用于类、接口、类型别名，用于获取其他类型中的所有成员名组成的联合类型

* in

该关键字往往和keyof联用，限制某个索引类型的取值范围。

### TS中预设的类型演算

``` ts

Partial<T>  // 将类型T中的成员变为可选

Required<T>  // 将类型T中的成员变为必填

Readonly<T> // 将类型T中的成员变为只读

Exclude<T, U> // 从T中剔除可以赋值给U的类型。

Extract<T, U> // 提取T中可以赋值给U的类型。

NonNullable<T> // 从T中剔除null和undefined。

ReturnType<T> // 获取函数返回值类型。

InstanceType<T> // 获取构造函数类型的实例类型。

```
