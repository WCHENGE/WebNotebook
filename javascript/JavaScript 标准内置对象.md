# JavaScript 标准内置对象

## Array

### 属性

#### Array.prototype.length

`length`是`Array`的实例属性。返回或设置一个数组中的元素个数。该值是一个无符号 32-bit 整数，并且总是大于数组最高项的下标。

`length` 属性的值是一个 0 到 2<sup>32</sup>-1 的整数。

你可以设置 `length` 属性的值来截断任何数组。当通过改变`length`属性值来扩展数组时，实际元素的数目将会增加。例如：将一个拥有 2 个元素的数组的 `length` 属性值设为 3 时，那么这个数组将会包含3个元素，并且，第三个元素的值将会是 `undefined` 。

**示例：**

```javascript
var arr1 = [1, 2, 3, 4]
arr1.length = 6
// > [1, 2, 3, 4, empty × 2]

var arr2 = [1, 2, 3, 4]
arr2.length = 3
// > [1, 2, 3]
```

### 方法

#### Array.from()

`Array.from()`方法从类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

**语法：**

```javascript
Array.from(arrayLike[, mapFn[, thisArg]])
```

**参数：**

* arrayLike：想要转换成数组的伪数组对象或可迭代对象。
* mapFn：如果指定了该参数，新数组中的每个元素会执行该回调函数。
* thisArg：可选参数，执行回调函数`mapFn`时`this`对象。

**返回值：**

一个新的数组实例。

**示例：**

```javascript
Array.from('foo')
// ["f", "o", "o"]

Array.from([1, 2, 3], x => x + x)
// [2, 4, 6]
```

> `from()` 的 `length` 属性为 1 ，即 `Array.from.length === 1`。

#### Array.isArray()

`Array.isArray()` 用于确定传入的值是否是一个 `Array`。

**语法：**

```javascript
Array.isArray(obj)
```

**参数：**

* `obj`：需要检测的值

**返回值：**

如果值是 `Array`，则为 true；否则为 false。

**示例：**

```javascript
Array.isArray([1, 2, 3])	// true
Array.isArray({foo: 123})	// false
Array.isArray("foobar")	// false
Array.isArray(undefined)	// false
```

#### Array.of()

`Array.of()`方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

**语法：**

```javascript
Array.of(element1[, element1[, element2[, ...[, elementN]]])
```

**参数：**

* elementN：任意个参数，将按顺序称为返回数组中的元素。

**返回值：**

新的`Array`实例。

**示例：**

```javascript
Array.of(7)	// [7]
Array.of(1, 2, 3)	// [1, 2, 3]
Array.of(undefined)	// [undefined]

Array(7)	// [ , , , , , , ]
Array(1, 2, 3)	// [1, 2, 3]
```

#### Array.prototype.concat()

`concat()`方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

**语法：**

```javascript
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

**参数：**

* `valueN`<可选>：数组或值，将合并到一个新的数组中。如果省略了所有 `valueN` 参数，则`concat`会返回调用此方法的现存数组的一个浅拷贝。

**返回值：**

新的`Array`实例。

**示例：**

```javascript
const array1 = ['a', 'b', 'c']
const array2 = ['d', 'e', 'f']
const array3 = array1.concat(array2)

console.log(array3)
// ['a', 'b', 'c', 'd', 'e', 'f']
```

#### Array.prototype.copyWithin()

`copyWithin()`方法是浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。

**语法：**

```javascript
arr.copyWithin(target[, start[, end]]])
```

**参数：**（参数 target、start 和 end 必须为整数）

* `target`：0 为基底的索引，复制序列到该位置。如果是负数，`target` 将从末尾开始计算。如果 `target` 大于等于 `arr.length`，将会不发生拷贝。如果 `target` 在 `start` 之后，复制的序列将被修改以符合 `arr.length`。
* `start`：0 为基底的索引，开始复制元素的起始位置。如果是负数，`start` 将从末尾开始计算。如果 `start` 被忽略，`copyWithin` 将会从0开始复制。
* `end`：0 为基底的索引，开始复制元素的结束位置。`copyWithin` 将会拷贝到该位置，但不包括 `end` 这个位置的元素。如果是负数， `end` 将从末尾开始计算。如果 `end` 被忽略，`copyWithin` 方法将会一直复制至数组结尾（默认为 `arr.length`）。

**返回值：**

改变后的数组。

**示例：**

```javascript
[1, 2, 3, 4, 5].copyWithin(-2)
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1)
// [1, 2, 3, 3, 4]
```

## Object

`Object`构造函数创建一个对象包装器。

**语法：**

```javascript
// 对象初始化器（Object initialiser）或对象字面量（literal）
{ [ nameValuePair1[, nameValuePair2[, ...nameValuePairN]]] }

// 以构造函数形式来调用
new Object([value])
```

**参数：**

* `nameValuePair1, nameValuePair2, ...nameValuePairN`：成对的名称（字符串）与值（任何值），其中名称通过冒号与值分隔。
* `value`：任何值。

**描述：**

在JavaScript中，几乎所有的对象都是`Object`类型的实例，它们都会从`Object.prototype`继承属性和方法。`Object`构造函数，会根据给定的参水创建对象，具体有以下情况：

* 如果给定值`null`或`undefined`，将会创建并返回一个空对象；
* 如果传进去的是一个基本类型的值，则会构造其包装类型的对象；
* 如果传进去的是引用类型的值，仍然会返回这个值，经它们复制的变量保有和源对象相同的引用地址；

当以非构造函数形式被调用时，`Object`的行为等同于`new Object()`。

```javascript
// 以下例子，都将一个空的 Object 对象存到 o 中
var o = new Object()
var o = new Object(null)
var o = new Object(undefined)
```

```javascript
// 等价于 o = new Boolean(true)
var o = new Object(true)

// 等价于 o = new Boolean(false)
var o = new Object(Boolean())
```

### Object 构造函数的属性

* `Object.length`：值为 1。
* `Object.prototype`：可以为所有 Object 类型的对象添加属性。

#### Object.prototype

`Object.prototype`属性表示`Object`的原型对象。

### Object 构造函数的方法

* `Object.assign()`：通过复制一个或多个对象来创建一个新的对象。
* `Object.create()`：使用指定的原型对象和属性创建一个新对象。
* `Object.defineProperty()`：给对象添加一个属性并分别指定该属性的配置。
* `Object.defineProperties()`：给对象添加多个属性并分别执行它们的配置。
* `Object.entries()`：返回给定对象自身可枚举属性的`[key, value]`数组。
* `Object.freeze()`：冻结对象：其他代码不能删除或更改任何属性。
* `Object.getOwnPropertyDescriptor()`：返回对象指定的属性配置。
* `Object.getOwnPropertyNames()`：返回一个数组，它包含了指定对象所有的可枚举或不可枚举的属性名。
* `Object.getOwnPropertySymbols()`：返回一个数组，它包含了指定对象自身所欲的符号属性。
* `Object.getPrototypeOf()`：返回指定对象的原型对象。
* `Object.is()`： 比较两个值是否相同。所有 NaN 值都相等（这与`==`和`===`不同）。
* `Object.isExtensible()`：判断对象是否可扩展。
* `Object.isFrozen()`： 判断对象是否已经冻结。
* `Object.isSealed()`：判断对象是否已经蜜封。
* `Object.keys()`：返回一个包含所有给定对象自身可枚举属性名称的数组。
* `Object.preventExtensions()`：防止对象的任何扩展。
* `Object.seal()`：防止其他代码删除对象的属性。
* `Object.setPrototypeOf()`：设置对象的原型（即内部`[[Prototype]]`属性）。
* `Object.values()`：返回给定对象自身可枚举值的数组。

#### Object.assign()

`Object.assign()`方法用于欧将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

**语法：**

```javascript
Object.assign(target, ...sources)
```

**参数：**

* `target`：目标对象。
* `sources`：源对象。

**返回值：**

目标对象。

**描述：**

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

**示例：**

```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// { a: 1, b: 4, c:5 }

console.log(returnedTarget);
// { a: 1, b: 4, c:5 }
```

#### Object.create()

`Object.create()`方法创建一个新对象，使用现有的对象来提供新创建的对象的 `__proto__`。

**语法：**

```javascript
Object.create(proto, [propertiesObject])
```

**参数：**

* `proto`：新创建对象的原型对象。

* `propertiesObject`：可选。需要传入一个对象，该对象的属性类型参照`Object.defineProperties()`的第二个参数。如果该参数被指定且不为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)，该传入对象的自有可枚举属性(即其自身定义的属性，而不是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符。如果`propertiesObject`参数是 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) 或非原始包装对象，则抛出一个 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 异常。

  ```javascript
  Object.create(Object.prototype, {
    // foo会成为所创建对象的数据属性
    foo: {
      writable:true,
      configurable:true,
      value: "hello"
    },
    // bar会成为所创建对象的访问器属性
    bar: {
      configurable: false,
      get: function() { return 10 },
      set: function(value) {
        console.log("Setting `o.bar` to", value);
      }
    }
  })
  ```

**返回值：**

一个新对象，带着指定的原型对象和属性。

**示例：**

```javascript
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```

#### Object.defineProperty()

`Object.defineProperty()`方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

**语法：**

```javascript
Object.defineProperty(obj, prop, descriptor)
```

**参数：**

* `obj`：要定义属性的对象；
* `prop`：要定义或修改的属性的名称或`Symbol`；
* `descriptor`：要定义或修改的属性描述符；

**返回值：**

被传递给函数的对象。

**描述：**

默认情况下，使用 `Object.defineProperty()` 添加的属性值是不可修改（immutable）的。

属性描述符有两种主要形式：*数据描述符*和*存取描述符*。

1. *数据描述符*是一个具有值的属性，该值可以是可写的，也可以是不可写的。

2. *存取描述符*是由 getter 函数和 setter 函数所描述的属性。

描述符：

* configurable

  当且仅当该属性的 `configurable` 键值为 `true` 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。
  **默认为** **`false`**。
  
* enumerable

  当且仅当该属性的 `enumerable` 键值为 `true` 时，该属性才会出现在对象的枚举属性中。
  **默认为 `false`**。

* value

  该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。
  **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。

* writable

  当且仅当该属性的 `writable` 键值为 `true` 时，属性的值，也就是上面的 `value`，才能被[`赋值运算符`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Assignment_Operators)改变。
  **默认为 `false`。**

* get

  属性的 getter 函数，如果没有 getter，则为 `undefined`。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 `this` 对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。
  **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。

* set

  属性的 setter 函数，如果没有 setter，则为 `undefined`。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 `this` 对象。
  **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。

描述符默认值汇总：

* 拥有布尔值的键 `configurable`、`enumerable` 和 `writable` 的默认值都是 `false`。
* 属性值和函数的键 `value`、`get` 和 `set` 字段的默认值为 `undefined`。

**示例：**

```javascript
var o = {}; // 创建一个新对象

// 在对象中添加一个属性与数据描述符的示例
Object.defineProperty(o, "a", {
  value : 37,
  writable : true,
  enumerable : true,
  configurable : true
});
```











#### Object.getPrototypeOf()

`Object.getPrototypeOf()`方法返回指定对象的原型（内部[[Prototype]]属性的值）。

**语法：**

```javascript
Object.getPrototypeOf(Object)
```

**参数：**

* `obj`：要返回其原型的对象。

**返回值：**

给定对象的原型。如果没有继承属性，则返回`null`。

**示例：**

> **Object.getPrototypeOf(Object) 不是 Object.prototype**

```javascript
// JavaScript中的 Object 是构造函数（创建对象的包装器）。
/* 一般用法是：*/
var obj = new Object();

/* 所以：*/
Object.getPrototypeOf( Object );               // ƒ () { [native code] }
Object.getPrototypeOf( Function );             // ƒ () { [native code] }
Object.getPrototypeOf( Object ) === Function.prototype;        // true

/* Object.getPrototypeOf( Object )是把 Object 这一构造函数看作对象，
返回的当然是函数对象的原型，也就是 Function.prototype。*/
/* 正确的方法是，Object.prototype是构造出来的对象的原型。*/

var obj = new Object();
Object.prototype === Object.getPrototypeOf( obj );              // true
Object.prototype === Object.getPrototypeOf( {} );               // true
```









### Object 实例和 Object 原型对象

JavaScript 中的所有对象都来自 Object；所有对象从`Object.prototype`继承方法和属性，尽管它们可能被覆盖。

#### 属性

* `Object.prototype.constructor`：特定的函数，用于创建一个对象的原型。
* `Object.prototype.__proto__`：指向当对象被实例化的时候，用作原型的对象。
* `Object.prtotype.__noSuchMethod__`：当未定义的对象成员被调用作方法的时候，允许定义并执行的函数。

#### 方法

* `Object.prototype.__defineGetter__()`：关联一个函数到一个属性。访问该函数时，执行该函数并返回其返回值。
* `Object.prtotype.__defineSetter__()`：关联一个函数到一个属性。设置该函数时，执行该修改属性的函数。
* `Object.prototype.__lookupGetter__()`：返回使用`__defineGetter__`定义的方法函数。
* `Object.prototype.__lookupSetter__()`：返回使用`__definedSetter__`定义的方法函数。
* `Object.prototype.hasOwnProperty()`：返回一个布尔值，表示某个对象是否含有指定的属性，而且此属性非原型链继承的。
* `Object.prototype.isPrototypeOf()`：返回一个布尔值，表示指定的对象是否在本对象的原型链中。
* `Object.prototype.prototypeIsEnumerable()`：判断指定属性是否可枚举
* `Object.prototype.toSource()`：返回字符串表示此对象的源代码形式，可以使用此字符串生成一个新的相同的对象。
* `Object.prototype.toLocaleString()`：直接调用`toString()`方法。
* `Object.prototype.toString()`：返回对象的字符串表示。
* `Object.prototype.unwatch()`：移除对象某个属性的监听。
* `Object.prototype.valueOf()`：返回指定对象的原始值。
* `Object.prototype.watch()`：给对象的某个属性增加监听。























