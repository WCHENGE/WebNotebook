### 1. 谈一下你对MVVM原理的理解 

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6iii1jilj20no0j2tcy.jpg)

### 2.请说一下响应式数据的原理？

**理解：**

1. 核心点: `Object.defineProperty` 
2. 默认 Vue 在初始化数据时，会给 data 中的属性使用 `Object.defineProperty` 重新定义所有属性，当页面取到对应属性时。会进行依赖收集（收集当前组件的 watcher）如果属性发生变化会通知相关依赖进行更新操作。

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6imgfmk2j20nb0jx41p.jpg)

```javascript
Object.defineProperty(obj, key, {    
  enumerable: true,    
  configurable: true,    
  get: function reactiveGetter () {      
    const value = getter ? getter.call(obj) : val      
    if (Dep.target) {        
      dep.depend() /* 收集依赖 */        
      if (childOb) {          
        childOb.dep.depend()          
        if (Array.isArray(value)) {            
          dependArray(value)          
        }        
      }      
    }      
    return value    
  },    
  set: function reactiveSetter (newVal) {      
    const value = getter ? getter.call(obj) : val      
    if (newVal === value || (newVal !== newVal && value !== value)) {        
      return      
    }      
    if (process.env.NODE_ENV !== 'production' && customSetter) {        
      customSetter()      
    }
    val = newVal      
    childOb = !shallow && observe(newVal)      
    dep.notify() /* 通知相关依赖进行更新 */    
  }  
})
```
### 3. Vue中是如何检测数组变化? 

**理解：**

使用函数劫持的方式，重写了数组的方法 Vue 将 data 中的数组，进行了原型链重写。指向了自己定义的数组原型方法，这样当调用数组 api 时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次进行监控。

**原理：**

```javascript
const arrayProto = Array.prototype 
export const arrayMethods = Object.create(arrayProto) 
const methodsToPatch = [  'push',  'pop',  'shift',  'unshift',  'splice',  'sort',  'reverse' ] 
methodsToPatch.forEach(function (method) {
  // 重写原型方法  const original = arrayProto[method] 
  // 调用原数组的方法  
  def(arrayMethods, method, function mutator (...args) {    
    const result = original.apply(this, args)    
    const ob = this.__ob__    
    let inserted
    switch (method) {      
      case 'push':      
      case 'unshift':        
        inserted = args        
        break      
      case 'splice':        
        inserted = args.slice(2)        
        break    
    }    
    if (inserted) ob.observeArray(inserted)    // notify change    
    ob.dep.notify() // 当调用数组方法后，手动通知视图更新    
    return result  
  }) 
})
this.observeArray(value) // 进行深度监控
```

### 4.为何Vue采用异步渲染? 

**理解：**

因为如果不采用异步更新，那么每次更新数据都会对当前组件进行重新渲染。所以为了性能考虑， Vue 会在本轮数据更新后，再去异步更新视图。

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6j0i0zujj20me0biac0.jpg)


```javascript
/* istanbul ignore else */ 
update () {  
  if (this.lazy) {     
    this.dirty = true    
  } else if (this.sync) {      
    this.run()    
  } else {     
    queueWatcher(this); // 当数据发生变化时会将watcher放到一个队列中批量更新   
  }
} 
export function queueWatcher (watcher: Watcher) {  
  const id = watcher.id // 会对相同的watcher进行过滤  
  if (has[id] == null) {    
    has[id] = true    
    if (!flushing) {      
      queue.push(watcher)    
    } else {      
      let i = queue.length - 1      
      while (i > index && queue[i].id > watcher.id) {        
        i--     
      }      
      queue.splice(i + 1, 0, watcher)   
    }    
    // queue the flush    
    if (!waiting) {     
      waiting = true
      if (process.env.NODE_ENV !== 'production' && !config.async) {        
        flushSchedulerQueue()        
        return
      }      
      nextTick(flushSchedulerQueue) // 调用nextTick方法 批量的进行更新    
    }  
  } 
}
```

### 5. nextTick实现原理？

**理解：**

(宏任务和微任务) 异步方法 
nextTick 方法主要是使用了宏任务和微任务，定义了一个异步方法。多次调用 nextTick 会将方法存入队列中，通过这个异步方法清空当前队列。 所以这个 nextTick 方法就是异步方法。

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6j53blk5j20lz0beq4z.jpg)

```javascript
let timerFunc  // 会定义一个异步方法
if (typeof Promise !== 'undefined' && isNative(Promise)) {  // promise  
  const p = Promise.resolve()  
  timerFunc = () => {    
    p.then(flushCallbacks)    
    if (isIOS) setTimeout(noop)  
  }  
  isUsingMicroTask = true 
} else if (!isIE && typeof MutationObserver !== 'undefined' && ( // MutationObserver  
  isNative(MutationObserver) ||  MutationObserver.toString() === '[object MutationObserverConstructor]' )) {  
  let counter = 1  
  const observer = new MutationObserver(flushCallbacks)  
  const textNode = document.createTextNode(String(counter))  
  observer.observe(textNode, {    characterData: true  })  
  timerFunc = () => {    
    counter = (counter + 1) % 2    
    textNode.data = String(counter)  }  
  isUsingMicroTask = true 
} else if (typeof setImmediate !== 'undefined' ) { // setImmediate  
  timerFunc = () => {    
    setImmediate(flushCallbacks)  
  } } else {  timerFunc = () => {   // setTimeout    
    setTimeout(flushCallbacks, 0)  } } // nextTick实现 
export function nextTick (cb?: Function, ctx?: Object) {  
  let _resolve  
  callbacks.push(() => {    
    if (cb) {      
      try {        
        cb.call(ctx)      
      } catch (e) {        
        handleError(e, ctx, 'nextTick')      
      }    
    } else if (_resolve) {      
      _resolve(ctx)    
    }  
  })  
  if (!pending) {    
    pending = true    
    timerFunc()  
  } 
}
```
### 6. Vue中Computed的特点

**理解：** 默认 computed 也是一个 watcher 是具备缓存的，只要当依赖的属性发生变化时才会更新视图

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6jb9sykjj20ne0lbacl.jpg)


```javascript
function initComputed (vm: Component, computed: Object) {  
  const watchers = vm._computedWatchers = Object.create(null)  
  const isSSR = isServerRendering()
  for (const key in computed) {    
    const userDef = computed[key]    
    const getter = typeof userDef === 'function' ? userDef : userDef.get    
    if (!isSSR) { 
      // create internal watcher for the computed property.      
      watchers[key] = new Watcher(vm, getter || noop, noop, computedWatcherOptions)
    }
    // component-defined computed properties are already defined on the    
    // component prototype. We only need to define computed properties defined
    // at instantiation here.    
    if (!(key in vm)) {      
      defineComputed(vm, key, userDef)    
    } else if (process.env.NODE_ENV !== 'production') {      
      if (key in vm.$data) {        
        warn(`The computed property "${key}" is already defined in data.`, vm)      
      } else if (vm.$options.props && key in vm.$options.props) {        
        warn(`The computed property "${key}" is already defined as a prop.`, vm)     
      }   
    }  
  }
} 
function createComputedGetter (key) {  
  return function computedGetter () {    
    const watcher = this._computedWatchers && this._computedWatchers[key]    
    if (watcher) {      
      if (watcher.dirty) { // 如果依赖的值没发生变化,就不会重新求值        
        watcher.evaluate()      
      }      
      if (Dep.target) {        
        watcher.depend()      
      }      
      return watcher.value    
    }  
  } 
}
```

### 7. Watch中的 deep:true 是如何实现的？

**理解：**

当用户指定了 watch 中的 deep 属性为 true 时，如果当前监控的值是数组类型。会对对象中的每一项进行求值，此时会将当前 watcher 存入到对应属性的依赖中，这样数组中对象发生变化时也会通知数据更新。

**原理：**    

```javascript
get () {    
  pushTarget(this) // 先将当前依赖放到 Dep.target上    
  let value    
  const vm = this.vm    
  try {      
    value = this.getter.call(vm, vm)   
  } catch (e) {      
    if (this.user) {        
      handleError(e, vm, `getter for watcher "${this.expression}"`)      
    } else {        
      throw e      
    }    
  } finally {      
    if (this.deep) { // 如果需要深度监控        
      traverse(value) // 会对对象中的每一项取值,取值时会执行对应的get方法      
    }      
    popTarget()  
  }
  return value 
} 
function _traverse (val: any, seen: SimpleSet) {  
  let i, keys 
  const isA = Array.isArray(val)  
  if ((!isA && !isObject(val)) || Object.isFrozen(val) || val instanceof VNode) {    
    return 
  }  
  if (val.__ob__) {    
    const depId = val.__ob__.dep.id    
    if (seen.has(depId)) {      
      return    
    }    
    seen.add(depId) 
  }  
  if (isA) {    
    i = val.length    
    while (i--) _traverse(val[i], seen)  
  } else {    
    keys = Object.keys(val)    
    i = keys.length    
    while (i--) _traverse(val[keys[i]], seen)  
  } 
}
```

### 8. Vue组件的生命周期

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6ju6n3jcj20nb0hen17.jpg)![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6juhshj3j20ri0j7wgo.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6jymmpwuj20tt0m7ad2.jpg)![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6jz48vq1j20ta0ifwgl.jpg)

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6jzw6f3tj20kl0wwjwq.jpg)

### 9. ajax请求放在哪个生命周期中

**理解：**

1. 在 created 的时候，视图中的 dom 并没有渲染出来，所以此时如果直接去操 dom 节点，无法找到相关的元素；
2. 在 mounted 中，由于此时 dom 已经渲染出来了，所以可以直接操作 dom 节点。一般情况下都放到 mounted 中，保证逻辑的统一性，因为生命周期是同步执行的，ajax 是异步执行的服务端渲染不支持 mounted 方法，所以在服务端渲染的情况下统一放到 created 中

### 10. 何时需要使用 beforeDestroy

**理解：**

1. 可能在当前页面中使用了 $on 方法，那需要在组件销毁前解绑。

2. 清除自己定义的定时器

3. 解除事件的绑定 scroll mousemove ....

### 11. Vue中模板编译原理

将 template 转化成 render 函数

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kfs7cr2j20mn0atmyq.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kg0jj6fj20mo0dt0vd.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kggfr4lj20fx0s30uk.jpg)

### 12. Vue中 v-if 和 v-show 的区别

**理解：** 

v-if 如果条件不成立不会渲染当前指令所在节点的 dom 元素

v-show 只是切换当前 dom 的显示或者隐藏

**原理：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6khwkkj2j20fv0kpgn8.jpg)

### 13. 为什么v-for 和 v-if 不能连用 

**理解：**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kj8ly5sj20pv09jt9g.jpg)

v-for 会比 v-if 的优先级高一些，如果连用的话会把 v-if 给每个元素都添加一下，会造成性能问题。

### 14. 用 vnode 来描述一个 DOM 结构

虚拟节点就是用一个对象来描述真实的 dom 元素

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kjzgngij20pn0h7q4d.jpg)

### 15. diff算法的时间复杂度

两个树的完全的 diff 算法是一个时间复杂度为 O(n3) , Vue 进行了优化·O(n3) 复杂度的问题转换成 O(n) 复杂度的问题(只比较同级不考虑跨级问题)  在前端当中，你很少会跨越层级地移动Dom元素。 所以 Virtual Dom只会对同一个层级的元素进行对比。

### 16. 简述Vue中diff算法原理

**理解：** 

1. 先同级比较，在比较子节点
2. 先判断一方有儿子一方没儿子的情况
3. 比较都有儿子的情况
4. 递归比较子节点

![image-20210303114841725](https://tva1.sinaimg.cn/large/e6c9d24egy1go6knn6f88j20xd0u0tn8.jpg)


原理: 
core/vdom/patch.js

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6knzsfr6j20pl045aa6.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6ko6aluvj20ko0q3tc0.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1go6kogd9z8j20pp0b3wg6.jpg)





