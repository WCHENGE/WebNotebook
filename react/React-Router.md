# React Router 简明教程

React Router 现在已经被划分成了三个包：`react-router`，`react-router-dom`，`react-router-native`。

在React router中有三种类型的组件，一是`路由组件` ，第二种是`路径匹配组件` ，第三种是`导航组件`。

路由组件: `BrowserRouter` 和 `HashRouter`

路径匹配的组件: `Route` 和 `Switch`

导航组件: `Link`

> 注意：一定要使用 BrowserRouter 或 HashRouter 作为祖先组件包裹我们的根组件！

## 1. 安装

> 你不应该直接安装 `react-router`，这个包为 React Router 应用提供了核心的路由组件和函数，另外两个包提供了特定环境的组件（浏览器和 `react-native` 对应的平台），不过他们也是将 `react-router` 导出的模块再次导出。

需要构建一个网站（在浏览器中运行），所以我们要安装 `react-router-dom`

```shell
npm install --save react-router-dom
```

## 2. 简单用法

```react
import TodoList from './components/TodoList'
import { BrowserRouter as Router, Route, Link } from 'react-router-dom'
import './App.css'

const App: React.FC = () => {
  return (
    <div className='App'>
      <Router>
        <Link to='/'>root</Link> <br />
        <Link to='/hello'>hello</Link> <br />
        <Link to='/todolist'>todolist</Link>
        <div>
          <Route path='/'  exact render={() => {
            return <div>root page</div>
          }} />
          <Route path='/hello' render={() => {
            return <div>hello world</div>
          }} />
          <Route path='/todolist' component={TodoList} />
        </div>
      </Router>
    </div>
  )
}

export default App
```

通过上面的代码就实现了一个项目的基本路由功能，`Router` 组件决定了我们使用html5 history api，`Route`组件定义了路由的匹配规则和渲染内容，使用 `Link` 组件进行路由之间的导航。使用 `exact` 属性来定义路径是不是精确匹配。

### 匹配方式：

* **不精确匹配：**这种情况下如果地址栏输入'/tv' 则 ‘/’ 和 ‘/tv’ 都会匹配上

  ```react
  <Route path='/tv' component={TV} />
  <Route path='/' component={Movie} />
  ```

* **精准匹配：**

  方法1：在标签中加上 ‘exact’ 属性：

  ```react
  <Route path='/' exact component={Movie} />
  ```

  方法2：使用 Switch：

  ```react
  import { Switch } from 'react-router-dom'
  
  <Switch>
    <Route path='/tv' component={TV} />
  	<Route path='/' component={Movie} />
  </Switch>
  ```

  注意：使用 Switch 时将一般路由写在最下，特殊路由写在上面。否则可能会匹配到普通路由上。

### 使用 URL 传参数

```react
<Route path='/hello/:name' render={({match}) => {
	return <div>hello {match.params.name}</div>
}} />
```

使用 `<Route>` 匹配的react 组件会在 props 中包含一个 `match` 的属性,通过 `match.params` 可以获取到参数对象。

### 调用方法跳转

```react
<Route path='/hello/:name' render={({ match, history }) => {
    return <div>
      hello {match.params.name}
      <button onClick={()=>{ history.push('/hello')}}>to hello</button>
    </div>
}} />
```

使用 `<Route>` 匹配的 react 组件会在 props 中包含一个 `history` 的属性，history 中的方法

1. `history.push(url)` 路由跳转
2. `hisroty.replace(url)` 路由跳转不计入历史记录
3. `history.go(n)` 根据索引前进或者后退
4. `history.goBack()` 后退
5. `history.goForward()` 前进

## 3. 路由导航

react-router-dom 的路由导航分为**组件式**导航和**编程式**导航。

> 顾名思义，组件式导航即通过组件进行导航完成路由的跳转，编程式导航就是在我们的逻辑代码内控制路由进行跳转.

### 3.1 组件式导航

使用 Link 标签进行跳转：

```react
<Link to='/movie'>电影</Link>
<Link to='/tv'>电视剧</Link>
 
<Route path='/tv' component={TV} />
<Route path='/movie' component={Movie} />
```

> 其实就是在页面内加入了一个 a 标签。但是经过实验，我们直接写 a 标签的话也可以跳转，但会有延迟，不如由 Link 为我们生成的 a 标签跳转速度快。通过 devtools 可以发现，结构上写法上没有任何区别，但由 Link 生成的 a 标签不会发生页面跳转，只会DOM操作。
>
> ```react
> <a href="/tv">电视剧</a>
> ```
>
> 总结：（由 Link 生成的 a 标签）
>
> 1. 由 Link 生成的 a 标签跳转速度快。
> 2. Link 生成的 a 标签不会发生页面跳转，只会DOM操作。

#### 路由高亮：

使用 NavLink 标签

```react
import { NavLink } from 'react-router-dom' 
import './style.css' 
 
<NavLink activeClassName='active' to='/movie'>电影</NavLink></li>
```

#### 路由重定向

使用 Redirect 标签，一般配合 Switch 一块使用。

```react
import { Redirect } from 'react-router-dom'

<Redirect from='/' to='/movie' />
```

Redirect 标签一样属于普通路由，要放在特殊路由下边。

from 和 to 表示当路由匹配到 '/' 时重定向到 '/movie'。

### 3.2 编程式导航：

在组件实例中， React-router-dom 自动帮我们在 props 中绑定了 history、location、match 三个对象，这样我们进行路由跳转就特别方便了。

- history：主要包括路由跳转函数。
- location：主要包含路径信息。其中的 state 属性包含上一个路由传递过来的参数。
- match：主要使用其中的 params 获取动态路由的信息。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjj57terxuj30gn0bkaaf.jpg)

```javascript
// 跳转到某个路由
this.props.hsitory.push('/...')
```

## 4. 路由传参

### 4.1 query 传参：

react-router-dom 不会主动将 query 帮我们解析成对象，如果使用 query 进行传参的话我们需要自己去解析。

可以在 props-history-location-search 中拿到完整的 query 字符串。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjj5dcg403j308f07ldfy.jpg)

### 4.2 动态路由传参

在路由中设置动态路由，比如动态的 id，在 Movie 组件实例内 props-match-params 中可以拿到 id。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjj5edlaoaj307s04xaa1.jpg)

### 4.3 路由跳转时传参

路由跳转时，可在 push 的第二个参数处传一个对象，我们在 '/tv' 组件实例上 props-location-state 上就可以拿到这个对象了。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjj5f5aps5j30co04vmx6.jpg)

## 5. 子路由

在 react-router-dom 中没有很强的子路由的概念，所谓的子路由其实是由路由的嵌套来实现的，比如，我在 `/movie` 路由渲染的组件为 Movie 组件，我们继续在 Movie 组件中插入路由，并且路径设置为 `/movie/jingsong` ，就可以实现类似于子路由的效果。

```react
<Route path='/movie' component={Movie} />
<Route path="/movie/jingsong" component={Jingsong}></Route>
```

## 总结

**React路由传参的三种方式**

### 方式 一 ：通过params

1. 组件式

   ```react
   // 路由表中
   <Route path='/sort/:id' component={Sort}></Router>
   // Link 处
   <link to={'/sort/' + '2'} activeClassName='active'>XXXX</link>
   ```

2. 编程式

   ```javascript
   this.props.history.push('/sort/' + '2')
   ```

**Sort 页面：**

通过 `this.props.match.params.id` 就可以接受传递过来的参数（id）。

### 方式二：通过 query

前提：必须由其他页面跳转过来，参数才会被传递过来。

注：不需要配置路由表。

> 路由表中的内容照常：`<Route path='/sort' component={Sort}></Route>`

1. Link 处

   ```react
   // HTML 方式
   <Link to={{path:'/sort', query: {name: 'sunny'}}}></Link>
   ```

   ```javascript
   // JS 方式
   this.props.history.push({path: '/sort', query: {name: 'sunny'}})
   ```

2. sort 页面

   ```javascript
   this.props.location.query.name  // 获取参数
   ```

### 方式三： 通过 state

同 query 差不多，只是属性不一样，而且 state 传的参数是加密的，query 传的参数是公开的，在地址栏。

1. Link 处

   ```react
   // HTML 方式
   <Link to={{ path: '/sort', state: {name: 'sunny'}}}></Link>
   
   // JS 方式
   this.props.history.push({pathname: '/sort', state: {name: 'sunny'}})
   ```

2. sort 页面

   ```javascript
   this.props.location.state.name
   ```

   