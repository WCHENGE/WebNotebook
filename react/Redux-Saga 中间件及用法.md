# Redux-Saga 中间件及用法

## 1. redux-thunk

### 1.1 redux 的副作用处理

**redux 中的数据流：**

![redux](https://poetries1.gitee.io/img-repo/2019/10/484.png)

redux 是遵循函数式编程的规则，在 redux 数据流中，`action`是一个原始 js 对象（plain object）且`reducer` 是一个纯函数，只有同步且没有副作用的操作，上述的数据流可以起到管理数据，从而控制并更新视图层的目的。

如果要存在副作用函数，那么我们需要先处理副作用函数，生成原始的 js 对象，然后在触发`action`到`reducer`处理函数之间，使用中间件处理副作用。

**redux 增加中间件处理副作用后的数据流：**

![](https://poetries1.gitee.io/img-repo/2019/10/485.png)

> 从图中我们就可以看出，中间件的作用就是：
>
> 转换异步操作，生成原始的`action`，这样`reducer`函数就能处理相应的`action`，从而改变`state`更新 UI。

### 1.2 redux-thunk 源码

在 redux 中，thunk 是 redux 作者给出的中间件，实现极为简单，10多行代码

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

这几行代码做的事情也很简单，判断`action`的类型，如果`action`是函数，就调用这个函数`action(dispatch, getState, extraArgument)`。

从源码中可以发现，调用的函数实参为`dispatch`和`getState`，因此我们在定义`action`为`thunk`函数时，一般形参分别为`dispatch`和`getState`。

### 1.3 redux-thunk 缺点

`thunk`的缺点也是很明显，`thunk`仅仅做了执行这个函数，并不在乎函数主体内是什么，也就是说`thunk`使得`redux`可以接受函数作为`action`，但是函数的内部可以是多种多样的，这样就导致`action`不易维护。

`action`不易维护的原因：

* `action`的形式不统一；
* 异步操作太为分散，分散在各个`action`中；

## 2. Redux-saga 简介

`redux-saga`是一个`redux`中间件，它具有如下特性：

* 集中处理`redux`副作用问题；
* 被实现为`generator`；
* 类`redux-thunk`中间件；
* `watch`/`worker`（监听->执行）的工作形式；

`redux-saga`的优点：

* 集中处理了所有的异步操作，异步接口部分一目了然；
* `action`是普通对象，这与`redux`同步的`action`一模一样；
* 通过`effect`，方便异步接口的测试；
* 通过`worker`和`watcher`可以实现非阻塞异步调用，并且同时可以实现非阻塞调用下的事件监听
* 异步操作的流程是可以控制的，可以随时取消相应的异步操作

### 2.1 安装

```shell
npm install --save redux
npm install --save redux-saga
```

###2.2  基本用法

* 使用`createSagaMiddleware`方法创建 saga 的 Middleware，然后在`redux`创建`store`时，使用`applyMiddleware`函数将创建的 saga Middleware 实例绑定到 `store`上，最后可以调用 saga Middleware 的`run`函数来执行某个或某些 Middleware。
* 在 saga 的 Middleware 中，可以使用`takeEvery`或者`takeLatest`等 API 来监听某个`action`，当某个`action`触发后，saga 可以使用`call`发起异步操作，操作完成后使用`put`函数触发`action`，同步更新`state`，从而完成整个 State 的更新。

```javascript
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

import { helloSaga } from './sagas'

const store = createStore(
	reducer,
  applyMiddleware(createSagaMiddleware(helloSaga))
)
```

## 3. Redux-saga 适用案例

redux-saga 是控制执行的`generator`，在 redux-saga 中 `action`是原始的 js 对象，把所有的异步副作用操作放在了 saga 函数里面。这样既统一了`action`的形式，有使得异步操作可以集中处理。

> redux-saga 是通过`generator` 实现的，如果不支持`generator`需要通过插件 babel-polyfill 转译。
>
> （在 Generator 函数中，`yeild`右边的任何表达式都会被求值，结果会被 yield 给调用者）

```javascript
// helloSaga.js
export function * helloSaga() {
  console.log('Hello Sagas!')
}

// main.js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'
import { helloSaga } from './sagas'
const sagaMiddleware = createSagaMiddleware();
const store = createStore(
 reducer,
 applyMiddleware(sagaMiddleware)
);
sagaMiddleware.run(helloSaga);
```

## 4. Redux-saga 使用

在`redux-saga`的世界里，Sagas 都用 Generator 函数实现。我们从 Generator 里 yield 纯 JavaScript 对象以表达 Saga 逻辑，我们称呼那些对象为 Effect。Effect 是一个简单的对象，这个对象包含了一些给 Middleware 解释执行的信息。（包括调用某些异步函数，发起一个 action 到 store，等等）

使用`redux-saga/effects`包里提供的函数来创建 Effect。

### 4.1 声明式的 Effect

在 redux-saga 中提供了一系列的 API ，比如`take`、`put`、`all`、`select`等 API，在 redux-saga 中将这一些列的 API 都定义为 Effect 。当函数 `resolve`时返回一个描述对象，然后 redux-saga 中间件根据这个描述对象恢复执行 `generstor` 中的函数。

redux-saga 的大体过程：

![](https://poetries1.gitee.io/img-repo/2019/10/487.png)

> redux-saga 监听到了原始的 `action`，并不会马上执行副作用操作，会先通过`Effect`方法将其转化成一个描述对象，然后再将描述对象，作为标示，再恢复执行副作用函数。

### 4.2 Effect 提供的具体方法

接下来介绍几个`Effect`中常用的几个方法，从低阶的 API，比如`take`，`call(apply)`，`fork`，`put`，`select`等，以及高阶 API，比如`takeEvery`和`takeLatest`等。

```javascript
import { take, call, put, select, fork, takeEvery, takeLatest } from 'redux-saga/effects'
```

#### 4.2.1 take

`take`这个方法，是用来监听`action`，返回的是监听到的`action`对象。

```javascript
// action 对象
const loginAction = {
	type: 'login'
}

// 在 UI Component 中 dispatch 一个 action
dispatch(loginAction)

// saga.js
function* watchLoginAction() {
  while(true) {
  	const action = yield take('login')
  
  	console.log(action) // {type: 'login'}
  }
}
```

注意，我们运行了一个无限循环的`while(true)`。这是一个 Generator 函数，它不具备`从运行至完成`的行为（run-to-completion behavior）。Generator 将在每次迭代阻塞以等待 action 发起。

#### 4.2.2 call (apply)

`call`和`apply`方法与 js 中的 call 和 apply 相似，这里以`call`方法为例。

```javascript
call(fn, ...args)
```

`call`方法调用 fn，参数为 args，返回一个描述对象。不过这里的`call`可以是普通函数，也可以是`generator`。`call`方法应用很广泛，在 redux-saga 中使用异步请求等常用`call`方法来实现。

```javascript
yield call(fetch, '/userInfo', username)
```

#### 4.2.3 put

redux-saga 作为中间件，工作流是这样的。redux-saga 执行完副作用函数后，必须发出`action`，然后这个`action`被`reducer`监听，从而达到更新`state`的目的。相应的这里的`put`对应 redux 中的 `dispatch`，工作流程如下：

![](https://poetries1.gitee.io/img-repo/2019/10/488.png)

> 可以看出 redux-saga 执行副作用方法转化`action`时，`put`这个`Effect`方法 redux 原始的`dispatch`相似，都是可以发出`action`，且发出的`action`都会被`reducer`监听到。

`put`的使用方法：

```javascript
yield put({ type: 'login' })
```

#### 4.2.4 select

如果我们想在中间件中获取`state`，那么需要使用`select`。`select`方法对应的是 redux 中的 `getState`，用来获取`store`中的`state`，使用方法：

```javascript
const id = yield select(state => state.id)
```

 #### 4.2.5 fork

`fork`方法相当于 web work，`fork`方法不会阻塞主线程，在非阻塞调用中十分有用。

#### 4.2.6 takeEvery 和 takeLatest

`takeEvery`和`takeLatest`用于监听相应的动作并执行相应的方法，是构建在`take`和`fork`上面的高阶 API。

```javascript
// 比如要监听 login 动作，用 takeEvery 方法就可以
takeEvery('login', loginFunc)
```

`takeEvery`监听到 login 动作，就会执行 loginFunc 方法，除此之外，`takeEvery`可以同时监听到多个相同的 `action`，并且在此之前还有一个或多个`action`尚未结束，还是可以启动一个新的`action`任务。

`takeLatest`方法跟`takeEvery`是相同的方式调用，与`takeLatest`不同的是，在任何时刻`takeLatest`只允许一个`action`任务在执行，并且这个任务是最后被启动的那个。如果已经有一个任务在执行的时候启动另一个`action`，那之前的这个任务会被自动取消。

```javascript
takeLatest('login', loginFunc)
```

## 5. Redux-saga 高级

### 5.1 同时执行多个任务

`yield`指令可以很简单的将异步控制流以同步的写法表现出来，但与此同时我们将也会同时执行多个任务，我们不能直接这样写：

```javascript
// 错误写法，effects 将按照顺序执行
const users = yield call(fetch, '/users'),
		  repos = yield call(fetch, '/repos')
```

由于第二个 effect 将会在第一个`call`执行完毕才开始。所以我们需要这样写：

```javascript
import { call } from 'redux-saga/effects'

// 正确写法，effects 将会同步执行
const [users, repos] = yeild [
	call(fetch, '/users'),
  call(fetch, '/repos')
]
```

当我们需要`yield`一个包含 effects 的数组，generator 会被阻塞知道所有的 effects 都执行完毕（就像 `Promise.all`的行为）。

### 5.2 在多个 Effects 之间启动 race

有时候我们同时启动多个任务，但又不想等待所有任务完成，我们只希望拿到*胜利者*：即第一个被 resolve（或 reject）的任务。`race`Effect 提供了一个方法，在多个 Effects 之间戳发一个竞赛（race）。

```javascript
// 示例演示了触发一个远程的获取请求，并且限制了1秒内响应，否则作超时处理
import { race, call, put } from 'redux-saga/effects'
import { delay } from 'redux-saga'

function* fetchPostsWithTimeout() {
	const {} = yield race({
  	posts: call(fetchApi, '/posts'),
    timeout: call(delay, 1000)
  })
  if (posts) put({type: 'POSTS_RECEIVED', posts})
  else ({type: 'TIMEOUT_ERROR'})
}
```

`race`的另一个有用的功能是，它会自动取消那些失败的 Effects。例如，假设我们有 2 个 UI 按钮：

* 第一个用于在后台启动一个任务，这个任务运行在一个无限循环的`while(true)`中（例如：每 x 秒钟从服务器上同步一些数据）
* 一旦该后台任务启动了，我们启用第二个按钮，这个按钮用于取消该任务。

```javascript
import { race, take, call } from 'redux-saga/effects'

function* backgroundTask() {
	while (true) {...}
}

function* watchStartBackgroundTask() {
  while (true) {
  	yield take('START_BACKGROUND_TASK')
    yield race({
    	task: call(backgroundTask),
      cancel: take('CANCEL_TASK')
    })
  }
}
```

在`CANCEL_TASK`action 被发起的情况下，`race`Effect 将自动取消`backgroundTask`，并在`backgroundTask`中抛出一个取消错误。

### 5.3 使用`yeild*`对 Sagas 进行排序

你可以使用内置的`yield*`操作符来组合多个 Sagas，使得它们保持顺序。这让你可以以一种简单的程序风格来排列你的*宏观任务（macro-tasks）*。

```javascript
function* palyLevelOne() { ... }

function* playLevelTwo() { ... }

function* playLevelThree() { ... }

function* game() {
	const score1 = yield* palyLevelOne()
  yield put(showScore(score1))
  
  const score2 = yield* playLevelTwo()
  yield put(showScore(score2))
  
  const score3 = yield* playLevelThree()
  yield put(showScore(score3))
}
```

注意，使用`yield*`将导致该 Javascript 运行环境*蔓延*至整个序列。由此产生的迭代器，将 yield 所有来自嵌套迭代器里的值。

### 5.4 节流（Throttling）

通过在监听的 Saga 里调用一个 throttle  函数，针对一系列发起的 action 进行节流。举个例子，假设用户在文本框输入文字的时候，UI 出发了一个`INPUT_CHANGED` action：

```javascript
import { throttle } from 'redux-saga/effects'

function* handleInput(input) {
	// ...
}

function* watchInput() {
	yeild throttle(500, 'INPUT_CHANGED', handleInput)
}

```

### 5.5 防抖动（Debouncing）

为了对 action队列进行防抖动，可以在被`fork` 的任务里放一个内置的`delay`。

```javascript
import { call, cancel, fork, take, delay } from 'redux-saga/effects'

function* handleInput(input) {
	// 500ms 防抖动
  yeild delay(500)
  ...
}
  
function* watchInput() {
  let task
  while (true) {
  	const { input } = yeild take('INPUT_CHANGED')
    if (task) {
    	yield cancel(task)
    }
    task = yeild fork(handleInput, input)
  }
}
```

> 在上面的示例中，`handleInput` 在执行之前等待了 500ms。如果用户在此期间输入了更多文字，我们将收到更多的`INPUT_CHANGED` action。并缺由于`handleInput`仍然会被`delay`阻塞，所以在执行自己的逻辑之前它会被`watchInput`取消。