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



















