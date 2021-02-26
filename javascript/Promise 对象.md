## JavaScript Promise 对象

ECMAscript 6 原生提供了 Promise 对象。

Promise 对象代表了未来将要发生的事件，用来传递异步操作的消息。

### Promise 对象有以下两个特点:

1、对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：

* *pending*: 初始状态，不是成功或失败状态。
* *fulfilled*: 意味着操作成功完成。
* *rejected*: 意味着操作失败。

2、一旦状态改变，就不会再变，任何时候都可以得到这个结果。

Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolved 和从 Pending 变为 Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。

### Promise 优缺点

* **优点**：避免回调地狱

  有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，*避免了层层嵌套的回调函数*。此外，Promise 对象提供统一的接口，使得控制异步操作更加容易。

* **缺点**：
  1. 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
  2. 不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
  3. 当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

### Promise 创建

要想创建一个 promise 对象、可以使用 new 来调用 Promise 的构造器来进行实例化。

```javascript
var promise = new Promise(function(resolve, reject) {
    // 异步处理
    // 处理结束后、调用resolve 或 reject
});
```

Promise 构造函数包含一个参数和一个带有 resolve（解析）和 reject（拒绝）两个参数的回调。在回调中执行一些操作（例如异步），如果一切都正常，则调用 resolve，否则调用 reject。

```javascript
// 用 Promise 对象实现的 Ajax 操作的例子

function ajax(URL) {
    return new Promise(function (resolve, reject) {
        var req = new XMLHttpRequest(); 
        req.open('GET', URL, true);
        req.onload = function () {
        if (req.status === 200) { 
                resolve(req.responseText);
            } else {
                reject(new Error(req.statusText));
            } 
        };
        req.onerror = function () {
            reject(new Error(req.statusText));
        };
        req.send(); 
    });
}
```





















### then 方法

then 方法接收两个函数作为参数，第一个参数是 Promise 执行成功时的回调，第二个参数是 Promise 执行失败时的回调，两个函数只会有一个被调用。



















