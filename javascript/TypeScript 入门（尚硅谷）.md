# TypeScript 中的基本类型

## 类型声明

类型声明是 typescript 非常重要的一个特点，通过类型声明可以指定 typescript 中变量（参数、形参）的类型。指定类型后，当为变量赋值时，typescript 编译器会自动检查值是否符合类型声明，符合则赋值，否则报错。

> 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值。

**语法：**

```typescript
let 变量：类型
let 变量：类型 = 值
function fn(参数：类型，参数：类型)：类型 {
	...code something
}
```

**自动类型判断：**

TypeScript 拥有自动的类型判断机制。当对变量的声明和赋值是同时进行的，typescript 编译器会自动判断变量的类型。所以如果你的变量的声明和赋值是同时进行的，可以省略掉类型声明。

## TypeScript 类型

| 类型    | 例子               | 描述                            |
| ------- | ------------------ | ------------------------------- |
| number  | 1，-33，2.5        | 任意数字                        |
| string  | 'hi'，"hi"         | 任意字符喘                      |
| boolean | true、false        | 布尔值 true 或 false            |
| 字面量  | 其本身             | 限制变量的值就是该字面量的值    |
| any     | *                  | 任意类型                        |
| unknown | *                  | 类型安全的 any                  |
| void    | 空值（undefined）  | 没有值（或 undefined）          |
| never   | 没有值             | 不能是任何值                    |
| object  | { name: '孙悟空' } | 任意的 JS 对象                  |
| array   | [1, 2, 3]          | 任意 JS 数组                    |
| tuple   | [4, 5]             | 元组，TS 新增类型，固定长度数组 |
| enum    | enum{A, B}         | 枚举，TS 中新增类型             |

### number

```typescript
let decimal: number = 6
let hex: number = 0xf00d
let binary: number = 0b1010
let octal: number = 0o744
let big: bigint = 100n
```

### boolean

```typescript
let isDone: boolean = false
```

### string

```typescript
let color: string = "blue"
color = 'red'

let fullName: string = `Bob Bobbington`
let age: number = 37
let sentence: string = `Hello, my name is ${fullName}. I'll be ${age + 1} years old next month.`
```

### 字面量

使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围。

```typescript
let color: 'red' | 'blue' | 'black'
let num: 1 | 2 | 3 | 4 | 5
```

### any

```typescript
let d: any = 4
d = 'hello'
d = true
```

### unknown

```typescript
let notSure: unknown = 4
notSure = 'hello'
```

### void

```typescript
let unusable: void = undefined
```

### never

```typescript
function error(message: string): never {
	throw new Error(message)
}
```

### object（没啥用）

```typescript
let obj: object = {}
```

### array

```typescript
let list: number[] = [1, 2, 3]
let list: Array<number> = [1, 2, 3]
```

### tuple

```typescript
let x: [string, number]
x = ["hello", 10]
```

### enum

```typescript
enum Color {
	Red,
  Green,
  Blue
}
let c: Color = Color.Green

enum Color {
	Red = 1,
  Green,
  Blue
}
let c: Color = Color.Green

enum Color {
	Red = 1,
  Green = 2,
  Blue = 4
}
let c: Color = Color.Green
```

### 类型断言

有些情况下，变量的类型对于我们来说是很明确的，但是 TS 编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式：

* 第一种：

  ```typescript
  let someValue: unknown = 'this is a string'
  let strLength: number = (someValue as string).length
  ```

* 第二种：

  ```typescript
  let someValue: unknown = 'this is a string'
  let strLength: number = (<string>someValue).length
  ```

### 泛型（Generic）

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定），此时泛型便能够发挥作用。

举个例子：

```typescript
function test(arg: any): any {
	return arg
}
```

> 上例中，test 函数有一个参数类型不确定，但是能确定的是，返回值的类型和参数的类型是相同的；由于类型不确定所以参数和返回值均使用了`any`。但是很明显这样做是不合适的，首先使用`any`会关闭 TS 的类型检查，其次这样设置不能体现出参数和返回值是相同的类型。

#### 泛型函数

##### 创建泛型函数

```typescript
function test<T>(arg: T): T {
	return arg
}
```

> 这里的`<T>`就是泛型。
>
> T 是我们给这个类型起的名字（不一定非叫 T），设置泛型后即可在函数中使用 T 来表示该类型，所以泛型其实就是表示某个类型。

##### 使用泛型函数

方式一（直接使用）：

```typescript
test(10)
```

> 使用时可以直接传递参数使用，类型会由 TS 自动推断出来，但有时编译器无法自动推断时还需要使用下面的方式。

方式二（指定类型）：

```typescript
test<number>(10)
```

> 也可以在函数后手动指定泛型

##### 函数中声明对个泛型

可以同时制定多个泛型，泛型间使用逗号隔开：

```typescript
function test<T, K>(a: T, b: K): K {
	return b
}

test<number, string>(10, "hello")
```

> 使用泛型时，完全可以将泛型当成是一个普通的类去使用。

####  泛型类

类中同样可以使用泛型：

```typescript
class MyClass<T> {
	prop: T
  
  constructor(prop: T) {
  	this.prop = prop
  }
}
```

#### 泛型继承

除此之外，也可以对泛型的范围进行约束

```typescript
interface MyInter {
	length: number
}

function test<T extends MyInter>(arg: T): number {
	return arg.length
}
```

> 使用T extends MyInter 表示泛型 T 必须是 MyInter 的子类，不一定非要使用接口类和抽象类。













## TypeScript 面向对象

要想面向对象，操作对象，首先便要拥有对象；要创建对象，必须要先定义类，所谓的类可以理解为对象的模型；程序中可以根据类创建指定类型的对象。

> 举例来说：
>
> 可以通过 Person 类来创建人的对象，通过 Dog 类创建狗的对象，不同的类可以用来创建不同的对象。

### 定义类

```typescript
class 类名 {
	属性名：类型
  
  constructor(参数：类型) {
  	this.属性名 = 参数
  }
  
  方法名() {
  	...
  }
}
```

示例：

```typescript
class Person {
	name: string
  age: number
  
  constructor(name: string, age: number) {
  	this.name = name
    this.age = age
  }
  
  sayHello() {
  	console.log(`大家好，我是${this.name}`)
  }
}
```

使用类：

```typescript
const p = new Person('孙悟空', 18)
p.sayHello()
```

### 构造函数

使用`constructor`定义一个构造器方法。

> 在 TS 中只能有一个构造器方法

例如：

```typescript
class C {
	name: string
  age: number
  
  constructor(name: string, age: number) {
  	this.name = name
    this.age = age
  }
}
```

也可以直接将属性定义在构造函数中：

```typescript
class C {
	constructor(public name: string, public age: number) {
  	...
  }
}
```

以上两种定义方法是完全相同的。

**子类继承父类时，必须调用父类的构造方法（如果子类中也定义了构造方法）**

例如：

```typescript
class A {
	protected num: number
  constructor(num: number) {
  	this.num = num
  }
}

class X extends A {
	protected name: string
  constructor(num: number, name: string) {
  	super(num)
    this.name = name
  }
}
```

> 如果在 X 类中不调用`super`将会报错

### 封装

对象实质上就是属性和方法的容器，它的主要作用就是存储属性和方法，这就是所谓的封装。

默认情况下，对象的属性是可以任意修改的，为了确保数据的安全性，在 TS 中可以对属性的权限进行设置：

* 静态属性（static）：

  > 声明为 static 的属性或方法不再属于实例，而是属于类的属性。

* 只读属性（readonly）：

  > 如果在声明属性时添加一个 readonly，则属性便成了制度属性，无法修改。

* TS 中属性具有三种修饰符：

  > `public`（默认值）：可以在类、子类和对象中修改；
  >
  > `protected`：可以在类、子类中修改；
  >
  > `private`：可以在类中修改；

示例：

#### public

```typescript
class Person {
	name: string	// 写或什么都不写都是 public
  public age: number
  
  constructor(name: string, age: nmber) {
  	this.name = name	// 可以在类中修改
    this.age = age
  }
  
  sayHello() {
  	console.log(`大家好，我是${this.name}`)
  }
}

class Employee extends Person {
	constructor(name: string, age: number) {
  	super(name, age)
    this.name = name	// 子类中可以修改
  }
}

const p = new Person('孙悟空', 18)
p.name = '猪八戒'	// 可以通过对象修改
```

#### protected

```typescript
class Person {
	protected name: string
  protected age: number
  
  constructor(name: string, age: number) {
  	this.name = name	// 可以修改
    this.age = age
  }
  
  sayHello() {
  	console.log(`大家好，我是${this.name}`)
  }
}

class Employee extends Person {
	constructor(name: string, age: number) {
  	super(name, age)
    this.name = name	// 子类中可以修改
  }
}

const p = new Person('孙悟空', 18)
p.name = '猪八戒'	// 不能修改
```

#### private

```typescript
class Person {
	private name: string
  private age: number
  
  constructor(name: string, age: number) {
  	this.name = name	// 可以修改
    this.age = age
  }
  
  sayHello() {
  	console.log(`大家好，我是${this.name}`)
  }
}

class Employee extends Person {
	constructor(name: string, age: number) {
  	super(name, age)
    this.name = name	// 子类中不能修改
  }
}

const p = new Person('孙悟空', 18)
p.name = '猪八戒'	// 不能修改
```

### 属性存取器

对于一些不希望被任意修改的属性，可以将其设置为`private`，直接将其设置为`private`将导致无法再通过对象修改其中的属性，我们可以在类中定义一组读取、设置属性的方法，这种**对属性读取或设置属性被称为属性的存取器**。

**读取属性**的方法叫做 `setter`方法，**设置属性**的方法叫做`getter`方法。

示例：

```typescript
class Person {
	private _name: string
  
  constructor(name: string) {
  	this._name = name
  }
  
  get name() {
  	return this._name
  }
  
  set name(name: string) {
  	this._name = name
  }
}

const p1 = new Person('孙悟空')
console.log(p1.name)	// 实际通过调用 getter 方法读取 name 属性
p1.name = '猪八戒'	// 实际通过调用 setter 方法修改 name 属性
```

### 静态属性

静态属性（方法），也称为类属性。使用静态属性无需创建实例，通过类即可直接使用。

静态属性（方法）使用 static 开头。

示例：

```typescript
class Tools {
	static PI = 3.1415926
  
  static sum(num1: number, num2: number) {
  	return num1 + num2
  }
}

console.log(Tools.PI)
console.log(Tools.sum(123, 456))
```

### this

在类中，使用 this 表示当前对象。

### 继承

继承是面向对象中的又一特性。通过继承可以将其他类中的属性和方法引入到当前类中。

示例：

```typescript
class Animal {
	name: string
  age: number
  
  constructor(name: string, age: number) {
  	this.name = name
    this.age = age 
  }
}

class Dog extends Animal {
	bark() {
  	console.log(`${this.name}汪汪叫`)
  }
}

const dog = new Dog('旺财', 4)
dog.bark()
```

通过继承可以在不修改类的情况下完成对类的扩展。

### 重写

发生继承时，如果子类中的方法会替换掉父类中的同名方法，这就称为方法的重写。

示例：

```typescript
class Animal {
	name: string
  age: number
  
  constructor(name: string, age: number) {
  	this.name = name
    this.name = age
  }
  
  run() {
  	console.log(‘父类中的 run 方法！’)
  }
}

class Dog extends Animal {
	bark() {
  	console.log(`${this.name}在汪汪叫！`)
  }
  
  run() {
  	console.log('子类中的 run 方法，会重写父类中的 run 方法')
  }
}

const dog = new Dog('旺财', 4)
dog.bark()
```

在子类中可以使用 super 来完成对父类的引用。

#### 抽象类（abstract class）

抽象类是专门用来被其他类所继承的类，它只能被其他类所继承，不能用来创建实例。

```typescript
abstract class Animal {
	abstract run(): void
  bark() {
  	console.log('动物在叫～')
  }
}

class Dog extends Animals {
	run() {
  	console.log('狗在跑～')
  }
}
```

### 接口（interface）

接口的作用类似于抽象类，不同点在于：接口中的所有方法和属性都是没有实值的，换句话说接口中的所有方法都是抽象方法；

接口主要负责定义一个类的结构，接口可以去限制一个对象的接口：对象只有包含接口中定义的所有属性和方法时才能匹配接口。

同时，可以让一个类去实现接口，实现接口时，类中要包含接口中的所有属性。

示例（检查对象类型）：

```typescript
interface Person{
	name: string
  sayHello(): void
}

function fn(per: Person) {
	per.satHello()
}
  
fn({
  name: '孙悟空',
  sayHello() {
		console.log(`Hello, 我是${this.name}`)
	}
})
```

示例（实现）：

```typescript
interface Person {
	name: string
  sayHello(): void
}

class Student implements Person {
	constructor(public name: string) {
  	
  }
  
 	sayHello() {
  	console.log('大家好，我是' + this.name)
  }
}
```

## 编译选项

### 自动编译文件

编译文件时，使用`-w`指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译。

示例：

```shell
tsc xxx.ts -w
```

### 自动编译整个项目

如果直接使用`tsc`指令，则可以自动将当前项目下的所有 ts 文件编译为 js 文件。但是能直接使用 `tec`命令的前提是，要先在项目根目录下创建一个 TS 的配置文件 `tsconfig.json`。`tsconfig.json`是一个 JSON 文件，添加配置文件后，只需 `tsc`命令即可完成对整个项目的编译。

**配置选项：**

#### include

* 定义希望被编译文件所在的目录
* 默认值：`["**/*"]`

* 示例：

  ```
  "include": ["src/**/*", "tests/**/*"]
  ```

  > 上述示例中，所有 src 目录和 tests 目录下的文件都会被编译。

#### exclude

* 定义需要排除在外的目录

* 默认值：[''node_modules", "bower_components", "jspm_package"]

* 示例：

  ```
  "exclude": ["./src/hello/**/*"]
  ```

  > 上述示例中，src 下 hello 目录下的文件都不会被编译

#### extends

* 定义被继承的配置文件

* 示例：

  ```
  "extends": "./configs/base"
  ```

  > 上述示例中，当前配置文件中会自动包含 config 目录下 base.json 中的所有配置信息

#### files

* 指定被编译文件的列表，只有需要编译的文件少时才会用到。

* 示例：

  ```
  "files": [
  	"core.ts",
  	"sys.ts",
  	"types.ts",
  	"scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "tsc.ts"
  ]
  ```

  > 列表中的文件都会被 TS 编译器所编译

#### compilerOptions

* 编译选项是配置文件中非常重要也比较复杂的配置选项；

* 在 compilerOptions 中包含多个子选项，用来完成对编译的配置；

* 项目选项：

  ##### target

  * 设置 TS 代码编译的目标版本

  * 可选值：

    > ES3（默认）、ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext

  * 示例：

    ```json
    "compilerOptions": {
    	"target": "ES6"
    }
    ```

    > 如上设置，我们所编写的 TS 代码将会被编译为 ES6 版本的 js 代码

  ##### lib

  * 指定代码运行时所包含的库（宿主环境）

  * 可选值：

    > ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost ......
    
  * 示例：

    ```json
    "compilerOptions": {
    	"target": "ES6",
    	"lib": ["ES6", "DOM"],
    	"outDir": "dist",
    	"outFile": "dist/aa.js"
    }
    ```

  ##### module

  * 设置编译后代码使用的模块化系统

  * 可选值：

    > CommonJS、UMD、AMD、System、ES2020、ESNext、None

  * 示例：

    ```json
    "compilerOptions": {
    	"module": "CommonJS"
    }
    ```

  ##### outDir

  * 编译后文件的所在目录

  * 默认情况下，编译后的 js 文件会和 ts 文件位于相同的目录，设置 outDir 后可以改变编译后文件的位置

  * 示例：

    ```json
    "compilerOptions": {
    	"outDir": "dist"
    }
    ```

    > 谁编译后的 js 文件将会生成到 dist 目录下。

  ##### outFile

  * 将所有的文件编译为一个文件

  * 默认会将所有的全局作用域中的代码合并为一个 js 文件，如果 module 制定了 None、System或  AMD 则会将模块一起合并到文件中

  * 示例：

    ```json
    "compilerOptions": {
    	"outFile": "dist/app.js"
    }
    ```

  ##### rootDir

  * 指定代码的根目录，默认情况下编译后的文件目录结构会以最长的公共目录为根目录，通过 rootDir 可以手动指定根目录

  * 示例：

    ```json
    "compilerOptions": {
    	"rootDir": "./src"
    }
    ```

  ##### allowJs

  * 是否对 js 文件编译

  ##### checkJs

  * 是否对 js 文件进行检查

  * 示例：

    ```json
    "compilerOptions": {
    	"allowJs": true,
    	"checkJs": true
    }
    ```

  ##### removeComments

  * 是否对 js 文件进行检查

  * 示例：

    ```json
    "compilerOptions": {
    	"allowJs": true,
    	"checkJs": true
    }
    ```

  ##### noEmit

  * 不对代码进行编译
  * 默认值：false

  ##### sourceMap

  * 是否生成 sourceMap
  * 默认值：false

  ##### 严格模式

  * strict

    > 启用所有的严格检查，默认值为true，设置后相当于开启了所有的严格检查

  * alwaysStrict

    > 总是以严格模式对代码进行编译

  * nolmplicitAny

    > 禁止隐式的 `any`类型

  * nolmplicitThis

    > 禁止类型不明确的 `this`

  * strictBindCallApply

    > 严格检查 bind、call和 apply的参数列表

  * strictFunctionTypes

    > 严格检查函数类型

  * StrictNullChecks

    > 严格的空值检查

  * strictPropertyInitialization

    > 严格检查属性是否初始化

  ##### 额外检查

  * noFallthroughCasesInSwitch

    > 检查switch语句包含正确的break

  * noImplicitReturns

    > 检查函数没有隐式的返回值

  * noUnusedLocals

    > 检查未使用的局部变量

  * noUnusedParameters

    > 检查未使用的参数

  ##### 高级

  - allowUnreachableCode
    - 检查不可达代码
    - 可选值：
      - true，忽略不可达代码
      - false，不可达代码将引起错误
  - noEmitOnError
    - 有错误的情况下不进行编译
    - 默认值：false



## Typescript 打包📦

### webpack整合

通常情况下，实际开发中我们都需要使用构建工具对代码进行打包；

TS 同样也可以结合构建工具一起使用，下边以 webpack 为例介绍一下如何结合构建工具使用TS；

步骤如下：

#### 初始化项目

进入项目根目录，执行命令 `npm init -y`，创建package.json文件

#### 下载构建工具

命令如下：

```shell
npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader clean-webpack-plugin
```

共安装了7个包:

- webpack：构建工具webpack
- webpack-cli：webpack的命令行工具
- webpack-dev-server：webpack的开发服务器
- typescript：ts编译器
- ts-loader：ts加载器，用于在webpack中编译ts文件
- html-webpack-plugin：webpack中html插件，用来自动创建html文件
- clean-webpack-plugin：webpack中的清除插件，每次构建都会先清除目录

#### 配置webpack

根目录下创建webpack的配置文件`webpack.config.js`：

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
   optimization:{
       minimize: false // 关闭代码压缩，可选
   },

   entry: "./src/index.ts",

   devtool: "inline-source-map",

   devServer: {
       contentBase: './dist'
   },

   output: {
       path: path.resolve(__dirname, "dist"),
       filename: "bundle.js",
       environment: {
           arrowFunction: false // 关闭webpack的箭头函数，可选
       }
   },

   resolve: {
       extensions: [".ts", ".js"]
   },

   module: {
       rules: [
           {
               test: /\.ts$/,
               use: {
                   loader: "ts-loader"     
               },
               exclude: /node_modules/
           }
       ]
   },

   plugins: [
       new CleanWebpackPlugin(),
       new HtmlWebpackPlugin({
           title:'TS测试'
       }),
   ]
}
```

#### 配置TS编译选项

根目录下创建tsconfig.json，配置可以根据自己需要

```json
{
   "compilerOptions": {
       "target": "ES2015",
       "module": "ES2015",
       "strict": true
   }
}
```

#### 修改package.json配置

修改package.json添加如下配置

```json
{
   ...
   "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "webpack",
       "start": "webpack serve --open chrome.exe"
   },
   ...
}
```

#### 项目使用

在src下创建ts文件，并在并命令行执行`npm run build`对代码进行编译；

或者执行`npm start`来启动开发服务器；

### Babel

除了webpack，开发中还经常需要结合babel来对代码进行转换；

以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将babel引入到项目中；

> 虽然TS在编译时也支持代码转换，但是只支持简单的代码转换；
>
> 对于例如：Promise等ES6特性，TS无法直接转换，这时还要用到babel来做转换；

安装依赖包：

```shell
npm i -D @babel/core @babel/preset-env babel-loader core-js
```

共安装了4个包，分别是：

- @babel/core：babel的核心工具
- @babel/preset-env：babel的预定义环境
- @babel-loader：babel在webpack中的加载器
- core-js：core-js用来使老版本的浏览器支持新版ES语法

修改webpack.config.js配置文件

```javascript
...
module: {
    rules: [
        {
            test: /\.ts$/,
            use: [
                {
                    loader: "babel-loader",
                    options:{
                        presets: [
                            [
                                "@babel/preset-env",
                                {
                                    "targets":{
                                        "chrome": "58",
                                        "ie": "11"
                                    },
                                    "corejs":"3",
                                    "useBuiltIns": "usage"
                                }
                            ]
                        ]
                    }
                },
                {
                    loader: "ts-loader",

                }
            ],
            exclude: /node_modules/
        }
    ]
}
...
```

如此一来，使用ts编译后的文件将会再次被babel处理；

使得代码可以在大部分浏览器中直接使用；

同时可以在配置选项的targets中指定要兼容的浏览器版本；







**学习视频：**

- [尚硅谷2021版TypeScript教程（李立超老师TS新课）](https://www.bilibili.com/video/BV1Xy4y1v7S2?p=6)

**其他学习：**

-   [TypeScript 入门教程](https://ts.xcatliu.com/)
-   [深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/)
-   [TS官方文档](https://www.tslang.cn/docs/home.html)
-   [TS与各框架整合官方案例](https://www.tslang.cn/samples/index.html)































