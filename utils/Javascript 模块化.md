# Javascript 模块化

随着前端 js 代码复杂度的提高，js 模块化是必然趋势。代码模块化不仅好维护，同时依赖很明确，不会全局污染。首先梳理一下模块化的发展情况：

​	**无模块化-->CommonJS规范-->AMD规范-->CMD规范-->ES6模块化**

## 1. 无模块化

script 标签引入 js 文件，相互罗列，被依赖的文件需要放在前面，否则使用就会报错。如下：

```html
   <script src="jquery.js"></script>
　　<script src="jquery_scroller.js"></script>
　　<script src="main.js"></script>
　　<script src="other1.js"></script>
　　<script src="other2.js"></script>
　　<script src="other3.js"></script>复制代码
```

即简单的将所有的 js 文件统统放在一起。但是这些文件的顺序还不能出错，比如 jquery 需要先引入，才能引入jquery 插件，才能在其他的文件中使用 jquery。缺点很明显：

- 污染全局作用域
- 维护成本高
- 依赖关系不明显

## 2. CommonJS 规范

该规范最初是用在服务器端的 node，它有四个重要的环境变量为模块化的实现提供支持：`module`、`exports`、`require`、`global`。实际使用时，用`module.exports`定义当前模块对外输出的接口（不推荐直接用`exports`），用`require`加载模块（同步）。

```javascript
// a-commonJs.js (导出)
var a = 5;
var add = function(param){  
	return a + param
}
//在这里写上需要向外暴露的函数、变量  
module.exports.a = a;
module.exports.add = add;

// b-commonJs.js引用自定义模块，参数包含路径，可省略.js（导入）
var addFn = require('./a-commonJs');
console.log(addFn.add(3)) //8
console.log(addFn.a) //5
```

> exports 是对 module.exports 的引用。比如我们可以认为在一个模块的顶部有这句代码：exports = module.exports 所以，我们不能直接给exports赋值，比如number、function等。

**注意：**因为**module.exports本身就是一个对象**，所以，我们在导出时可以使用 

```javascript
module.exports = {foo: 'bar'}
// module.exports.foo = 'bar'
```

但是 exports 是 module.exports 的一个引用，或者理解为 exports 是一个指针，exports 指向 module.exports，这样我们就只能使用 `exports.foo = 'bar' ` 的方式，而不能使用

```javascript
exports = {foo: 'bar'} // error 这种方式是错误的，相当于重新定义了exports
```

**优点：**解决了依赖、全局变量污染的问题

**缺点**： **CommonJS用同步的方式加载模块**。在服务端，模块文件都存在本地磁盘，读取非常快，所以这样做不会有问题。但是在浏览器端，限于网络原因，CommonJS不适合浏览器端模块加载，更合理的方案是使用异步加载，比如下边AMD规范。

## 3. AMD 规范

**承接上文，AMD规范则是非同步加载模块**，**允许指定回调函数，**AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。

AMD标准中，定义了下面三个API：

1. `require([module], callback)`
2. `define(id, [depends], callback)`
3. `require.config()`

即通过 define 来**定义**一个模块，然后使用 require 来**加载**一个模块, 使用`require.config()`指定引用路径。

先到[require.js官网](http://www.requirejs.cn/docs/download.html)下载最新版本,然后引入到页面，如下：

```html
<script data-main="./alert" src="./require.js"></script>
```

data-main属性不能省略。

```javascript
// alert.js
define(() => {
	const alterName = () => {
		alter('I am' + str);
	}
  const alterAge = () => {
    alter('I am' + num + 'years old');
  }
  return {
    alterName,
    alterAge
  }
});

require(['alter'], (alter) => {
	alter.alterName('JohnZhu');
  alter.alterAge(21);
});
```

以上分别是定义模块，引用模块，运行在浏览器弹出提示信息。

引用模块的时候，我们将模块名放在`[]`中作为`reqiure()`的第一参数；如果我们定义的模块本身也依赖其他模块,那就需要将它们放在`[]`中作为`define()`的第一参数。

在使用require.js的时候，我们必须要提前加载所有的依赖，然后才可以使用，而不是需要使用时再加载。

**优点：**适合在浏览器环境中异步加载模块、并行加载多个模块；

**缺点：**不能按需加载、开发成本大；

## 4. CMD

**AMD 推崇依赖前置、提前执行**，***CMD 推崇依赖就近、延迟执行***。CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

```javascript
/* AMD 写法 */
define(['a','b','c','d','e','f'], (a,b,c,d,e,f) => {
  // 等于在最前面声明并初始化了要用到的所有模块
	a.doSomething();
  if (false) {
    // 即便没用到模块 b，但 b 还是提前执行了
  	b.doSomething();
  }
});

/* CMD 写法 */
define((require, exports, module) => {
  const a = require('./a');	// 在需要时申明
  a.doSomething();
  if (false) {
    const b = require('./b');
    b.doSomething();
  }
});
```

很明显，CMD是按需加载，就近原则。

## 5. ES6 模块化

在ES6中，我们可以使用 import 关键字引入模块，通过 exprot 关键字导出模块，功能较之于前几个方案更为强大，也是我们所推崇的，**但是由于ES6目前无法在浏览器中执行，所以，我们只能通过 babel 将不被支持的import 编译为当前受到广泛支持的 require**。 

```javascript
/* 定义模块 math.js */
const basicNum = 0;
const add = () => {
	return a + b;
}
export { basicNum, add };

/* 引用模块 */
import { basicNum, add } from './math';
function test(ele) {
  ele.textContent = add(99 + basicNum);
}
```

ES6 在导出的时候有一个默认导出，`export default`，使用它导出后，在 import 的时候，不需要加上 {}，模块名字可以随意起。该名字实际上就是个对象，包含导出模块里面的函数或者变量。

```javascript
// 定义输出
export default { basicNum, add };

// 引入
import math from './math';
function test(ele) {
  ele.textContent = math.add(99 + math.basicNum);
}
```

但是一个模块只能有一个`export default`。

## 6. CommonJs 和 ES6 区别

以下引用阮一峰老师的内容：

#### 1.  CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

- CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
- ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令`import`，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的`import`有点像 Unix 系统的“符号连接”，原始值变了，`import`加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

#### 2.  CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

- 运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。
- 编译时加载: ES6 模块不是对象，而是通过 `export` 命令显式指定输出的代码，`import`时采用静态命令的形式。即在`import`时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

CommonJS 加载的是一个对象（即`module.exports`属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。