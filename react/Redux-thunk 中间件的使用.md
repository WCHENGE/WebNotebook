# Redux-thunk 中间件使用详情

## 1. 概念

dispatch 一个 action之后，到达 reducer之前，进行一些额外的操作，就需要用到 middleware。

## 2. 安装

```shell
npm install --save redux-thunk
```

## 3. 应用

```javascript
import { applyMiddleware, createStore } from 'redux'
import thunk from 'redux-thunk'

const store = createStore(
	reducers,
  applyMiddleware(thunk)
)
```

直接将 thunk 中间件引入，放在 applyMiddleware 方法中，传入 createStore 方法，就完成了 store.dispatch() 的功能增强。即可以在 reducer 中进行一些异步的操作。

## 4. redux-thunk 源码

分析 redux-thunk 的源码 node_modules/redux-thunk/src/index.js

```javascript
function createThunkMiddleware(extraArgument) {
	return ({dispatch, getState}) => next => action => {
  	if (typeof action === 'function') {
    	return action(dispatch, getState, extraArgument)
    }
    return next(action)
  }
}
const thunk = createThunkMiddleware()
thunk.withExtraArgument = createThunkMiddleware
export default thunk
```

redux-thunk 中间件 export default 的就是 createThunkMiddleware() 过的 thunk，再看 createThunkMiddleware 这个函数，返回的是一个柯里化过的函数。

翻译成 ES5 的代码容易看一点：

```javascript
function createThunkMiddleware(extraArgument) {
	return function({dispatch, getState}) {
  	return function(next){
    	if (typeof action === 'function') {
      	return action(dispatch, getState, extraArgument)
      }
      return next(action)
    }
  }
}
```

可以看出来 redux-thunk 最重要的思想，就是可以接受一个返回函数的 action creator。如果这个 action creator 返回的是一个函数，就执行它，如果不是，就按照原来的 next(action) 执行。

正因为这个 action creator 可以返回一个函数，就可以在这个函数中执行一些异步的操作。

```javascript
export function addCount {
	return { type: ADD_COUNT }
}

export function addCountAsync() {
	return dispatch => {
  	setTime(() => {
    	dispatch(addCount())
    }, 2000)
  }
}
```

> addCountAsync 函数就返回了一个函数，将 dispatch 作为函数的第一个参数传递进去，在函数内进行异步操作就可以了。