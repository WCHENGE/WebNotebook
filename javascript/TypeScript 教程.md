# TypeScipt 教程

## 1. TypeScript 简介

### 1.1 什么是 TypeScript

TypeScript 是 JavaScript 的类型的超集，它可以编译成 JavaScript。编译出来的JavaScript 可以运行在任何浏览器上。

![image-20201223101206481](/Users/wcheng/Library/Application Support/typora-user-images/image-20201223101206481.png)

### 1.2 TypeScript 增加了什么

* 类型
* 添加 ES 不具备的新特性
* 支持 ES 的新特性
* 丰富的配置选项
* 强大的开发工具

### 1.3 安装 TypeScript

TypeScript 的命令行工具安装方法：

```shell
npm install -g typescript
```

编译一个 TypeScript 文件很简单：

```shell
tsc hello.ts
```

> 约定使用 TypeScript 编写的文件以`.ts`为后缀，用 TypeScript 编写 React 时，以`.tsx`为后缀。

## 2. TypeScript 基础

### 2.1 原始数据类型

JavaScript 的类型分为两种：**原始数据类型**（Primitive data types）和**对象类型**（Object types）。

原始数据类型包括：布尔值、数值、字符串、`null`、`undefined`以及 ES6 中的新类型`symbol`和`BigInt`。

简单介绍前五种原始数据类型在 TypeScript 中的应用。

#### 2.1.1 布尔值

布尔值是最基础的数据类型，在 TypeScript 中，使用 `boolean`定义布尔值类型：

```typescript
let isDone: boolean = false
```

编译结果：

```javascript
var isDone = false
```

#### 2.1.2 数值

使用`number`定义数值类型：

```typescript
let decliteral: number = 6
let hesLiteral: number = 0xf00d
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010
// ES6 中的八进制表示法
let octalLiteral: number = 0o744
let notANumber: number = NaN
let infinityNumber: number = Infinity
```

编译结果：

```javascript
var decLiteral = 6
var hexLiteral = 0xf00d
// ES6 中的二进制表示法
var binaryLiteral = 10
// ES6 中的八进制表示法
var octalLiteral = 484
var notANumber = NaN
var infinityNumber = Infinity
```

其中`0b1010`和`0o744`是 ES6 中的二进制和八进制表示法，与他们会被编译为十进制数字。

#### 2.1.3 字符串

使用`string`定义字符串类型：

```typescript
let myName: string = 'Tom'
let myAge: number = 25

// 模版字符串
let sentence: string = `Hello, my name is ${myName}. I'll be ${myAge +1} years old next month`
```

编译结果：

```javascript
var myName = 'Tom'
var myAge = 25

// 模版字符串
var sentence = "Hello, my name is" + myName + ". I'll be " + (myAge + 1) + " years old next month."
```

其中 `` ` 用来定义 [ES6 中的模板字符串](http://es6.ruanyifeng.com/#docs/string#模板字符串)，`${expr}` 用来在模板字符串中嵌入表达式。

#### 2.1.4 空值

JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用`void`表示没有任何返回值的函数：

```typescript
function alertName(): void {
	alert('My name is Tom')
}
```

声明一个`void`类型的变量没有什么用，因为你只能将它赋值为`undefined`和`null`：

```typescript
let unusable: void = undefined
```

#### 2.1.5 Null 和 Undefined

在 TypeScript 中，可以使用`null`和`undefined`来定义这两个原始数据类型：

```typescript
let u: undefined = undefined
let n: null = null
```

与`void`的区别是，`undefined`和`null`是所有类型的子类型。也就是说`undefined`类型的变量，可以赋值给`number`类型的变量：

```typescript
// 这样不会报错
let num: number = undefined

// 这样也不会报错
let u: undefined
let num: number = u
```

而`void`类型的变量不能赋值给`number`类型的变量：

```typescript
let u: void
let num: number = u
// type 'void' is not assignable to type 'number'.
```

### 2.2 任意值

任意值（Any）用来表示允许赋值为任意类型。

#### 2.2.1 什么是任意值类型

如果是一个普通类型，在赋值过程中改变类型是不被允许的：

```typescript
let myFavoriteNumber: string = 'seven'
myFavoriteNumber = 7

// index.ts(2,1): error TS2322: Type 'number' isnot assignable to type 'string'
```

但如果是`any`类型，则允许被赋值为任意类型。

```typescript
let myFavoriteNumber: any = 'seven'
myFavoriteNumber = 7
```

#### 2.2.2 任意值的属性和方法

在任意值上访问任何属性都是允许的：

```typescript
let anything: any = 'hello'
console.log(anyThing.myName)
console.log(anyThing.firstName)
```

也允许调用任何方法：

```typescript
let anyThing: any = 'Tom'
anyThing.setName('Jerry')
anyThing.setName('Jerry').sayHello()
anyThing.myName.setFirstName('Cat')
```

可以认为，**声明一个变量为任意值之后，对他的任何操作，返回的内容的类型都是任意值**。

#### 2.2.3 未声明类型的变量

变量如果在声明的时候，未指定器类型，那么它会被识别为任意值类型：

```typescript
let something;
something = 'seven'
something = 7

something.setName('Tom')
```

等价于：

```javascript
let something: any
something = 'seven'
something = 7

something.setName('Tom')
```

### 2.3 类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

#### 2.3.1 什么是类型推论

以下代码虽然没有指定类型，但是会在编译的时候报错：

```typescript
let myFavoriteNumber = 'seven'
myFavoriteNumber = 7

// index.ts(2,1): error TŠ322: Type 'number' is not assignable to type 'string'
```

事实上，它等价于：

```typescript
let myFavoriteNumber: string = 'seven'
myFavoriteNumber = 7

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'
```

**TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论。**

如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成`any`类型而完全不被类型检查：

```typescript
let myFavoriteNumber
myFavoriteNumber = 'seven'
myFavoriteNumber = 7
```

### 2.4 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种。

简单的例子：

```typescript
let myFavoriteNumber: string | number
myFavoriteNumber = 'seven'
myFavoriteNumber = 7
```

```typescript
let myFavoriteNumber: string | number
myFavoriteNumber = true

// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'. Type 'boolean' is not assignable to type 'number'.
```

联合类型使用`|`分割每个类型。

这里的`let myFavoriteNumber: string | number`的含义是，允许`myFavoriteNumber`的类型是`string`或者`number`，但是不能是其他类型。

#### 2.4.1 访问联合类型的属性或方法

当 TypeScript 不确定一个联合类型的变量到底是那个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**。

```typescript
function getLength(something: string | number): number {
	return something.length
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'. Property 'length' does not exist on type 'number'.
```

上例中，`length`不是`string`和`number`的共有属性，所以会报错。

访问 `string`和`number`的共有属性是没问题的：

```typescript
function getString(somethng: string | number): string {
	return something.toString()
}
```

联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型：

```javascript
let myFavoriteNumber: string | number
myFavoriteNumber = 'seven'
console.log(myFavoriteNumber.length)  // 5
myFavoriteNumber = 7
console.log(myFavoriteNumber.length)  // 编译时报错

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'
```

上例中，第二行的`myFavoriteNumber`被推断成了`string`，访问它的`length`属性不会报错。而第四行的 `myFavoriteNumber`被推断成了`number`，访问它的`length`属性时就报错了。

### 2.5 对象的类型—接口

在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

#### 2.5.1 什么是接口

在面向对象语言中，接口（interfaces）是一个很重的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

#### 2.5.2 简单的例子

```typescript
interface Person {
	name: string;
  age: number;
}

let tom: Person = {
	name: 'Tom',
  age: 25
}
```

> 上面的例子中，我们定义了一个接口`Person`，接着定义了一个变量`tom`，它的类型是`Person`。这样，我们就约束了`tom`的形状必须和接口`Person`一致。

接口一般首字母大写。有的编程语言中会建议接口的名称加上`I`前缀。

**定义的变量比接口少了一些属性是不允许**的：

```typescript
interface Person {
	name: string;
	age: number;
}

let tom: Person = {
	name: 'Tom'
}

// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'. Property 'age' is mssing in type '{ name: string; }'
```

多一些属性也是不允许的：

```typescript
interface Person {
	name: string;
  age: number;
}

let tom: Person = {
	name: 'Tom',
  age: 25,
  gender: 'male'
}

// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'. Object literal may only specify known properties, and 'gender' does not exist in type 'Person'
```

可见，**赋值的时候，变量的形状必须和接口的形状保持一致**。

#### 2.5.3 可选属性

有时我们希望不要完全匹配一个形状，那么可以用可选属性：

```typescript
interface Person {
	name: string;
  age?:number;
}

let tom: Person = {
	name: 'Tom'
}

let bom: Person = {
	name: 'Bom',
  age: 25
}
```

**可选属性的含义是该属性可以不存在**。

这时**仍然不允许添加为定义的属性**：

```typescript
interface Person {
	name: string;
  age?: number;
}

let tom: Person = {
	name: 'Tom',
  age: 25,
  gender: 'male'
}

// examples/playground/index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'. Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

#### 2.5.4 任意属性

有时候我们希望一个接口允许有任意的属性，可以使用如下方式：

```typescript
interface Person {
	name: string;
  age?: number;
  [propName: string]: any;
}

let tom: Person = {
	name: 'Tom',
  gender: 'male'
}
```

使用`[propName: string]`定义了任意属性取`string`类型的值。

需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**：

```typescript
interface Person {
	name: string;
  age?: number;
  [propName: string]: string;
}

let tom: Person = {
	name: 'Tom',
  age: 25,
  gender: 'male'
}

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
// Index signatures are incompatible. Type 'string | number' is not assignable to type 'string'. Type 'number' is not assignable to type 'string'.
```

上例中，任意属性的值允许是`string`，但是可选属性`age`的值却是`number`，`number`不是`string`的子属性，所以报错了。

另外，在报错信息中可以看出，此时 `{ name: 'Tom', age: 25, gender: 'male' }` 的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }`，这是联合类型和接口的结合。

**一个接口中只能定义一个任意属性**。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

```typescript
interface Person {
	name: string;
	age?: number;
	[propName: string]: string | number;
}

let tom: Person = {
	name: 'Tom',
  age: 25,
  gender: 'male'
}
```

#### 2.5.5 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用`readonly`定义只读属性：

```typescript
interface Person {
	readonly id: number;
	name: string;
  age?: number;
  [propName: string]: any;
}

let tom: person = {
	id: 89757,
  name: 'Tom',
  gender: 'male'
}

tom.id = 9527

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

上例中，使用`readonly`定义的属性`id`初始化后，有被赋值了，所以报错了。

注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候：

```typescript
interface Person {
	readonly id: number;
  name: string;
  age?: number;
  [propName: string]: any;
}

let tom: Person = {
	name: 'Tom',
  gender: 'male'
}

tom.id = 89757;

// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
// Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

上例中，报错信息有两处，第一处是在对 `tom` 进行赋值的时候，没有给 `id` 赋值。

第二处是在给 `tom.id` 赋值的时候，由于它是只读属性，所以报错了。

### 2.6 数组的类型

在 TypeScript 中，数组类型有多种定义方式，比较灵活。

#### 2.6.1 「类型 + 方括号」表示法

最简单的方法是使用「类型 + 方括号」来表示数组：

```typescript
let fibonacci: number[] = [1, 1, 2, 3, 5]
```

数组的项中**不允许**出现其他的类型：

```typescript
let fibonacci: number[] = [1, '1', 2, 3, 5]
// Type 'string' is not assignable to type 'number'.
```

数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：

```typescript
let fibonacci: number[] = [1, 1, 2, 3, 5]
fibonacci.push('8')

// Argument if type '"8"' is not assignable to parameter of type 'number'.
```

上例中，`push`方法只允许传入`number`类型的参数，但是却传了一个`"8"`类型的参数，所以报错了。这里`"8"`是一个字符串字面量类型。

#### 2.6.2 数组泛型

我们也可以使用数组泛型（Array Generic）`Array<elemType>`来表示数组：

```typescript
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

#### 2.6.3 用接口表示数组

接口也可以用来描述数组：

```typescript
interface NumberArray {
	[index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

`NumberArray`表示：只要索引的类型是数字时，那么值的类型必须是数字。

虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种方式复杂多了。

不过有一种情况例外，那就是它常用来表示类数组。

#### 2.6.4 类数组

类数组（Array-like Object）不是数组类型，比如`arguments`：

```typescript
function sum() {
	let args: number[] = arguments;
}

// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

上例中，`arguments`实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：

```typescript
function sum() {
	let args: {
    [index: number]: number;
    length: number;
    callee: Function;
  } = arguments;
}
```

在这个例子中，我们出了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有`length`和`callee`两个属性。

事实上常用的类数组都有自己的接口定义，如`IArguments`, `NodeList`，`HTMLCollection`等：

```typescript
function sum() {
	let args: IArguments = arguments;
}
```

其中`IArguments`是 TypeScript 中定义好了的类型，它实际上就是：

```typescript
interface IArguments {
	[index: number]: any;
  length: number;
  callee: Function;
}
```

#### 2.6.5 any 在数组中的应用

一个比较常见的做法是，用`any`表示数组中允许出现任意类型：

```typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcstliu.com' }];
```

### 2.7 函数的类型

[函数是 JavaScript 中的一等公民](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch2.html)

#### 2.7.1 函数声明

在 Javascript 中，有两种常见的定义函数的方式——函数声明（Fuction Declaration）和函数表达式（Function Expression）：

```javascript
// 函数声明（Function Declaration）
function sum(x, y) {
	return x + y
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
	return x + y
}
```

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：

```typescript
funciton sum(x: number, y: number): number {
	return x + y
}
```

注意，**输入多余的（或者少于要求的）参数，是不被允许**的：

```typescript
function sum(x: number, y: number): number {
	return x + y
}
sum(1, 2, 3)

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

```typescript
function sum(x: number, y: number): number {
	return x + y
}
sum(1)

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

#### 2.7.2 函数表达式

如果要我们现在写一个对函数表达式（Function Expression）的定义，可能会写成这样：

```typescript
let mySum = function(x: number, y: number): number {
	return x + y
}
```

这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的`muSum`，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给`mySum`添加类型，则应该是这样：

```typescript
let mySum: (x: number, y: number) => number
mySum = function (x: number, y: number): number {
	return x + y
}
```

> 注意不要混淆了 TypeScript 中的 `=>`和 ES6 中的`=>`。
>
> 在 TypeScript 的类型定义中，`=>`用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
>
> 在 ES6 中，`=>`叫做箭头函数，应用十分广泛，可以参考 [ES6 中的箭头函数](http://es6.ruanyifeng.com/#docs/function#箭头函数)。

#### 2.7.3 用接口定义函数的形状

我们也可以使用接口的方式来定义一个函数需要符合的形状：

```typescript
interface SearchFunc {
	(source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
	return source.search(subString) !== -1
}
```

采用函数表达式｜接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。

#### 2.7.4 可选参数

前面提到，输入多余的（或者少于要求的）参数，是不允许的。那么如何定义可选的参数呢？

与接口中的可选属性类似，我们用`?`表示可选的参数：

```typescript
function buildName(firstName: string, lastName?: string) {
	if (lastname) {
  	return firstName + '' + lastName
  } else {
  	return firstName
  }
}
let tomcat = buildName('Tom', 'Cat')
let tom = buildName('Tiom')
```

需要注意的是，可选参数必须接在必需参数后面。换句话说，**可选参数后面不允许再出现必需参数了**：

```typescript
function buildName(firstName?: string, lastName: string) {
	if (firstName) {
  	return firstName + ' ' + lastName
  } else {
  	return lastName
  }
}
let tomcat = buildName('Tom', 'Cat')
let tom = buildName(undefined, 'Tom')
// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```

#### 2.7.5 参数默认值

在 ES6 中，我们允许给函数的参数添加默认值，**TypeScript 会将添加了默认值的参数识别为可选参数**：

```typescript
function buildName(firstName: string, lastName: string = 'Cat') {
	return firstName + ' ' + lastName
}
let tomcat = buildName('Tom', 'Cat')
let tom = buildName('Tom')
```

此时就不受「可选参数必须接在必需参数后面」的限制了：

```typescript
function buildName(firstName: string = 'Tom', lastName: string) {
	return firstname + ' ' + lastName
}
let tomcat = buildName('Tom', 'Cat')
let cat = buildName(undefined, 'Cat')
```

#### 2.7.6 剩余参数

ES6 中，可以使用`...rest`的方式获取函数中的剩余参数（rest 参数）：

```typescript
function push(array, ...items) {
	items.forEach(function(item) {
  	array.push(item)
  })
}
let a: any[] = []
push(a, 1, 2, 3)
```

事实上，`items`是一个数组。所以我们可以用数组的类型来定义它：

```typescript
function push(array: any[], ...items: any[]) {
	items.forEach(function(item) {
  	array.push(item)
  })
}
let a = []
push(a, 1, 2, 3)
```

注意，rest 参数只能是最后一个参数，关于 rest 参数，可以参考 [ES6 中的 rest 参数](http://es6.ruanyifeng.com/#docs/function#rest参数)。

#### 2.7.7 重载

重载允许一个函数接受不同数量或类型的参数是，作出不同的处理。

比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`。

利用联合类型，我们可以这么实现：

```typescript
function reverse(x: number | string): number | string {
	if (typeof x === 'number') {
  	return Number(x.toString().split('').reverse().join(''))
  } else if (typeof x === 'string') {
  	return x.split('').reverse().join('')
  }
}
```

然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。

这时，我们可以使用重载定义多`reverse`的函数类型：

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
	if (typeof x === 'number') {
  	return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
  	return x.split('').reverse().join('')
  }
}
```

上例中，我们重复定义了多个函数`revese`，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

注意，TypeScript 会优先从前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。

### 2.8类型断言

类型断言（Type Assertion）可以用来手动置顶一个值的类型。

### 2.9 声明文件

### 2.10 内置对象

## 3. TypeScript 进阶

### 3.1 类型别名

类型别名用来给一个类型起个新名字。

例子：

```typescript
type Name = string
type NameResolver = () => string
type NameOrResolver = Name | NameResolver
function getName(n: NameOrResolver): Name {
	if (typeof n === 'string') return n
  else return n()
}
```

> 上例中，我们使用`type`创建类型别名。

类型别名常用于联合类型。

### 3.2 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

例子：

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove'
function handleEvent(ele: Element, event: EventNames) {
	// do something
}
handleEvent(document.getElementById('hello'), 'scroll')  // 没问题
handleEvent(document.getElementById('world'), 'dblclick')		// 报错，event 不能为 ‘dblclick’

// index.ts(7,47): error TS2345: Argument of type '"dblclick"' is not assignable to parameter of type 'EventNames'.
```

> 上例中，我们使用`type`定了一个字符串字面量类型`EventNames`，它只能取三种字符串中的一种。

注意，**类型别名与字符串字面量类型都是使用`type`进行定义**。

### 3.3 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

> 元组起源于函数编程语言（如 F#），这些语言中会频繁使用元组。

#### 3.3.1 例子：

定义一对值分别为`string`和`number`的元组：

```typescript
let tom: [string, number] = ['Tom', 25]
```

当赋值或访问一个已知索引的元素是，会得到正确的类型：

```typescript
let tom: [string, number]
tom[0] = 'Tom'
tom[1] = 25

tom[0].slice(1)
tom[1].toFixed(2)
```

也可以只赋值其中一项：

```typescript
let tom: [string, number]
tom[0] = 'Tom'
```

但是当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中置顶的项。

```typescript
let tom: [string, number]
tom = ['Tom', 25]
```

```typescript
let tom: [string, number]
tom = ['Tom']

// Property '1' is missing in type '[string]' but required in type '[string, number]'.
```

#### 3.3.2 越界的元素

当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型：

```typescript
let tom: [string, number]
tom = ['Tom', 25]
tom.push('male')
tom.push(true)
// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

### 3.4 枚举

**枚举（Enum）类型用于取值被限定在一定范围内的场景**，比如一周只能有七天，颜色限定为红绿蓝等。

#### 3.4.1 简单的例子

枚举使用`enum`关键字来定义：

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}
```

枚举成员会被赋值为从`0`开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}

console.log(Days["Sun"] === 0)	// true
console.log(Days["Mon"] === 1)	// true
console.log(Days["Tue"] === 2)	// true
console.log(Days["Sat"] === 6)	// true

console.log(Days[0] === "Sun")
console.log(Days[1] === "Mon")
console.log(Days[2] === "Tue")
console.log(Days[6] === "Sat")
```

事实上，上面的例子会被编译为：

```javascript
var Days
(function (Days) {
	Days[Days["Sun"] = 0] = "Sun"
  Days[Days["Mon"] = 1] = "Mon"
  Days[Days["Tue"] = 2] = "Tue"
  Days[Days["Wed"] = 3] = "Wed"
  Days[Days["Thu"] = 4] = "Thu"
  Days[Days["Fri"] = 5] = "Fri"
  Days[Days["Sat"] = 6] = "Sat"
})(Days || (Days = {}))
```

#### 3.4.2 手动赋值

我们也可以给枚举项手动赋值：

```typescript
enum Day {Sum = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat}

console.log(Days["Sun"] === 7)	// true
console.log(Days["Mon"] === 1)	// true
console.log(Days["Tue"] === 2)	// true
console.log(Days["Sat"] === 6)	// true
```

上面的例子中，未手动赋值的枚举项会接着上一个枚举项递增

如果未手动赋值的枚举项与手动赋值的重复了，TypeScript 是不会察觉到这一点的：

```typescript
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat}

console.log(Days["Sun"] === 3)	// true
console.log(Days["Wed"] === 3)	// true
console.log(Days[3] === "Sun")	// false
console.log(Days[3] === "Web")	// true
```

上面的例子中，递增到 `3` 的时候与前面的 `Sun` 的取值重复了，但是 TypeScript 并没有报错，导致 `Days[3]` 的值先是 `"Sun"`，而后又被 `"Wed"` 覆盖了。编译的结果是：

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 3] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

所以使用的时候需要注意，最好不要出现这种覆盖的情况。

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查 (编译出的 js 仍然是可用的)：

```ts
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
var Days;
(function (Days) {
    Days[Days["Sun"] = 7] = "Sun";
    Days[Days["Mon"] = 8] = "Mon";
    Days[Days["Tue"] = 9] = "Tue";
    Days[Days["Wed"] = 10] = "Wed";
    Days[Days["Thu"] = 11] = "Thu";
    Days[Days["Fri"] = 12] = "Fri";
    Days[Days["Sat"] = "S"] = "Sat";
})(Days || (Days = {}));
```

当然，手动赋值的枚举项也可以为小数或负数，此时后续未手动赋值的项的递增步长仍为 `1`：

```ts
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

#### 3.4.3 常数项和计算所得项

枚举项有两种类型：**常数项**（constant member）和**计算所得项**（computed member）。

前面我们所举的例子都是常数项，一个典型的计算所得项的例子：

```typescript
enum Color {Red, Green, Blue = "blue".length}
```

> 上面的例子中，`"blue".length`就是一个计算所得项。

上面的例子不会报错，但是**如果进阶在计算所得项后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错**：

```typescript
enum Color {Red = "red".length, Green, Blue}

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

下面是常数项和计算所得项的完整定义，部分引用自[中文手册 - 枚举](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Enums.html)：

**当满足以下条件时，枚举成员被当作是常数**：

* 不具有初始化函数并且之前的枚举成员是常数。在这种情况下，当前枚举成员的值为上一个枚举成员的值加`1`。但第一个枚举元素是个例外。如果它没有初始化方法，那么它的初始值为`0`。

* 枚举成员使用常数枚举表达式初始化。常数枚举表达式是 TypeScript 表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，它就是一个常数枚举表达式：

  * 数字字面量
  * 引用之前定义的常数枚举成员（可以是在不同的枚举类型中定义的）如果这个成员是在同一个枚举类型中定义的，可以使用非限定名来引用
  * 带括号的常数枚举表达式
  * `+`、`-`、`~`一元运算符应用于常数枚举表达式
  * `+`，`-`，`*`，`/`，`%`，`<<`，`>>`，`>>>`，`&`，`|`，`^` 二元运算符，常数枚举表达式做为其一个操作对象。若常数枚举表达式求值后为 NaN 或 Infinity，则会在编译阶段报错

  所有其他情况的枚举成员被当作是需要计算得出的值。

#### 3.4.4 常数枚举

常数枚举是使用`const enum `定义的枚举类型：

```typescript
const enum Directions {
	Up,
  Down,
  Left,
  Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right]
```

常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。

上例的编译结果是：

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */]
```

假如包含了计算成员，则会在编译阶段报错：

```ts
const enum Color {Red, Green, Blue = "blue".length};

// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.
```

#### 3.4.5 外部枚举

外部枚举（Ambient Enums）是使用 `declare enum`定义的枚举类型：

```ts
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

之前提到过，`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除。

上例的编译结果是：

```js
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

外部枚举与声明语句一样，常出现在声明文件中。

同时使用 `declare` 和 `const` 也是可以的：

```ts
declare const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果：

```js
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

> TypeScript 的枚举类型的概念[来源于 C#](https://msdn.microsoft.com/zh-cn/library/sbbt4032.aspx)。

### 3.5 类

#### 3.5.1 类的概念

对类相关的概念做一个简单的介绍：

* 类（Class）：定义了一件事物的抽象特点，包含它的属性和方法；

* 对象（Object）：类的实例，通过`new`生成；

* 面向对象（OOP）的三大特性：封装、继承、多态；

* 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据；

* 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性；

* 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应；

  > 比如`Cat`和`Dog`都继承自`Animal`，但是分别实现了自己的`eat`方法。此时针对某一个实例，我们无需了解它是`Cat`还是`Dog`，就可以直接调用`eat`方法，程序会自动判断出来应该如何执行`eat`。

* 存取器（getter& setter）：用以改变属性的读取和赋值行为；

* 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如`public`表示共有属性或方法；

* 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现；

* 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（i mplements）。一个类只能继承自另一个类，但是可以实现多个接口。

####  3.5.2 ES6 中类的用法

ES6 中类的用法，更详细的介绍可以参考 [ECMAScript 6 入门 - Class](http://es6.ruanyifeng.com/#docs/class)。

##### 3.5.2.1 属性和方法

使用`class`定义类，使用`constructor`定义构造函数。

通过`new`生成新实例的时候，会自动调用构造函数。

```javascript
class Animal {
	public name
  constructor(name) {
  	this.name = name
  }
  sayHi() {
  	return `My name is ${this.name}`
  }
}

let a = new Animal('Jack')
console.log(a.sayHi())	// My name is Jack
```

##### 3.5.2.2 类的继承

使用`extends`关键字实现继承，子类中使用`super`关键字来调用父类的构造函数和方法。

```javascript
class Cat extends Animal {
	constructor(name) {
  	super(name)		// 调用父类的 constuctor(name)
    console.log(this.name)
  }
  sayHi() {
  	return 'Meow，' + super.sayHi()	// 调用父类的 sayHi()
  }
}
```

##### 3.5.2.3 存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为：

```javascript
class Animal {
	constructor(name) {
  	this.name = name
  }
  get name() {
  	return 'Jack'
  }
  set name(value) {
  	console.log('setter: ' + value)
  }
}

let a = new Animal('Kitty')		// setter: kitty
a.name = 'Tom'	// setter: Tom
console.log(a.name)		// Jack
```

##### 2.5.2.4 静态方法

使用`static`修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用：

```typescript
class Animal {
	static isAnimal(a) {
  	return a instaceof Animal
  }
}

let a = new Animal('Jack')
Animal.isAnimal(a)	// true
a.isAnimal(a)		// TypeError: a.isAnimal is not a function 
```

#### 3.5.3 ES7 中类的用法

##### 3.5.3.1 实例属性

ES6 中实例的属性只能通过构造函数中的`this.xxx`来定义，ES7 提案中可以直接在类里面定义：

```typescript
class Animal {
	name = 'Jack'
	
	constructor() {
		// ...
	}
}

let a = new Animal()
console.log(a.name)		// Jack
```

##### 3.5.3.2 静态属性

ES7 提案中，可以使用`static`定义一个静态属性：

```typescript
class Animal {
	static num = 42
  
  constructor() {
  	// ...
  }
}

console.log(Animal.num)		// 42
```

#### 3.5.4 TypeScript 中类的用法

##### 3.5.4.1 public、private 和 protected

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是`public`、`private`和`protected`。

* `public`修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是`public`的；
* `private`修饰的属性或方法是私有的，不能在声明它的类的外部访问；
* `protected`修饰的属性或方法是受保护的，它和`private`类似，区别是它在子类中也是允许被访问的。

下面举一些例子：

```typescript
class Animal {
	public name
  public constructor(name) {
  	this.name = name
  }
}

let a = new Animal('Jack')
console.log(a.name)		// Jack
a.name = 'Tom'
console.log(a.name)		// Tom
```

> 上面的例子中，`name`被设置为了`public`，所以直接访问实例的`name`属性是允许的。

很多时候，我们希望有的属性是无法直接存取的，这时候就可以用`private`了：

```typescript
class Animal {
	private name
	public constructor(name) {
  	this.name = name
  }
}

let a = new Animal('Jack')
console.log(a.name)		// Jack
a.name = 'Tom'

// index.ts(9,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
// index.ts(10,1): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

需要注意的是，TypeScript 编译之后的代码中，并没有限制 `private` 属性在外部的可访问性。

上面的例子编译后的代码是：

```js
var Animal = (function () {
  function Animal(name) {
    this.name = name;
  }
  return Animal;
})();
var a = new Animal('Jack');
console.log(a.name);
a.name = 'Tom';
```

使用 `private` 修饰的属性或方法，在子类中也是不允许访问的：

```ts
class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}

// index.ts(11,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

而如果是用 `protected` 修饰，则允许在子类中访问：

```ts
class Animal {
  protected name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}
```

当构造函数修饰为 `private` 时，该类不允许被继承或者实例化：

```ts
class Animal {
  public name;
  private constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');

// index.ts(7,19): TS2675: Cannot extend a class 'Animal'. Class constructor is marked as private.
// index.ts(13,9): TS2673: Constructor of class 'Animal' is private and only accessible within the class declaration.
```

当构造函数修饰为 `protected` 时，该类只允许被继承：

```ts
class Animal {
  public name;
  protected constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');

// index.ts(13,9): TS2674: Constructor of class 'Animal' is protected and only accessible within the class declaration.
```

##### 3.5.4.2 参数属性

修饰符和`readonly`还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更简洁。

```typescript
class Animal {
	// public name: string
  public constructor(public name) {
  	// this.name = name
  }
}
```

##### 3.5.4.3 readonly

只读属性关键字，只允许出现在属性声明或索引签名或构造函数中。

```typescript
class Animal {
	readonly name
  public constructor(name) {
  	this.name = name
  }
}

let a = new Animal('Jack')
console.log(a.name)		// Jack
a.name = 'Tom'
// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.
```

注意如果 `readonly` 和其他访问修饰符同时存在的话，需要写在其后面。

```ts
class Animal {
  // public readonly name;
  public constructor(public readonly name) {
    // this.name = name;
  }
}
```

##### 3.5.4.4 抽象类

`abstract`用于定义抽象类和其中的抽象方法。

什么是抽象类？

首先，抽象类是不允许被实例化的：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

let a = new Animal('Jack');

// index.ts(9,11): error TS2511: Cannot create an instance of the abstract class 'Animal'.
```

上面的例子中，我们定义了一个抽象类 `Animal`，并且定义了一个抽象方法 `sayHi`。在实例化抽象类的时候报错了。

其次，抽象类中的抽象方法必须被子类实现：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public eat() {
    console.log(`${this.name} is eating.`);
  }
}

let cat = new Cat('Tom');

// index.ts(9,7): error TS2515: Non-abstract class 'Cat' does not implement inherited abstract member 'sayHi' from class 'Animal'.
```

上面的例子中，我们定义了一个类 `Cat` 继承了抽象类 `Animal`，但是没有实现抽象方法 `sayHi`，所以编译报错了。

下面是一个正确使用抽象类的例子：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');
```

上面的例子中，我们实现了抽象方法 `sayHi`，编译通过了。

需要注意的是，即使是抽象方法，TypeScript 的编译结果中，仍然会存在这个类，上面的代码的编译结果是：

```js
var __extends =
  (this && this.__extends) ||
  function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() {
      this.constructor = d;
    }
    d.prototype = b === null ? Object.create(b) : ((__.prototype = b.prototype), new __());
  };
var Animal = (function () {
  function Animal(name) {
    this.name = name;
  }
  return Animal;
})();
var Cat = (function (_super) {
  __extends(Cat, _super);
  function Cat() {
    _super.apply(this, arguments);
  }
  Cat.prototype.sayHi = function () {
    console.log('Meow, My name is ' + this.name);
  };
  return Cat;
})(Animal);
var cat = new Cat('Tom');
```

#### 3.5.5 类的类型

给类加上`TypeScript`的类型很简单，与接口类似：

```javascript
class Animal {
	name: string
  constructor(name: string) {
  	this.name = name
  }
	sayHi(): string {
  	return `My name is ${this.name}`
  }
}

let a: Animal = new Animal('Jack')
console.log(a.sayHi())		// My name is Jack
```

























