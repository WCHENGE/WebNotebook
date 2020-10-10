# React 核心概念

React 是一个声明式的基于组件的视图库，可以帮助我们构建 UI。

## 创建新的 React 应用

### Create React App

`Create React App` 是用 React 创建新的**单页应用**的最佳方式。

```shell
npx create-react-app my-app
cd my-app
npm start
```

## 1. JSX 简介

```react
const element = <h1>Hello, world!</h1>
```

它被称为 JSX，是一个 JavaScript 的语法扩展。JSX 可以生成 React “元素”。

### 为什么使用 JSX ?

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，*比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI ，以及需要在 UI 中展示准备好的数据。*

React 并没有采用将标记于逻辑进行分离到不同文件这种人为地分离方式，而是通过将两者共同存放在称为“组件”的松散耦合单元之中，来实现关注点分离。

### 在 JSX 中嵌入表达式

在 JSX 语法中，你可以在大括号内放置任何有效的 **JavaScript 表达式**。

```react
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### JSX 也是一个表达式

在编译之后，JSX 表达式会被转为普通 Javascript 函数调用，并且对其取值后得到 JavaScript 对象。也就是说，你可以在 `if` 语句和 `for` 循环的代码中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```react
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSX 特定属性

你可以通过使用引号，来将属性值指定为字符串字面量：

```react
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```react
const element = <img src={user.avatarUrl}/>;
```

注意⚠️：在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。

> 应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

### 使用 JSX 指定子元素

假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 XML 语法一样：

```react
const element = <img src={user.avatarUrl} />;
```

JSX 标签里能够包含很多子元素：

```react
const element = (
	<div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX 防止注入攻击

React DOM 在渲染所有输入内容之前，默认会进行转移。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

### JSX 表示对象

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

以下两种示例代码完全等效：

```react
// 使用 JSX 表示
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
// 使用 createElement 创建虚拟 DOM
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` 实际上它创建了一个这样的对象：

```react
// 这是简化后的结构
const element = {
	type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
}
```

 这些对象被称为“React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以保持随时更新。

实际上，JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖。

> JSX 是一种 JavaScript 语言扩展，有点类似于 HTML/XML。
>
> 在使用 react 库中，需要时常引入这段代码：
>
> ```javascript
> imput React from 'react';
> ```
>
> 这段代码片段是能够自解释的。因为 Babel 在 JSX 之上转换为 `React.createElement`。

## 2. React 组件

组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

### 函数组件

```react
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>
}
```

该函数是一个有效的 React 组件，因为它接收唯一带有数据的“props”（代表属性）对象并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

### class 组件

```react
class Welcome extends React.Component {
	render() {
    return <h1>Hello, {this.props.name}</h1>
  }
}
```

上述两个组件在 React 里是等效的。

### 渲染组件

React 元素是 DOM 标签：

```react
const element = <div />;
```

React 元素是用户自定义组件：

```react
const element = <Welcome name="Sara" />
```

> 当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为“props”。

例如：

```react
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>
}
const element = <Welcome name="Sara" />
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

这段代码是怎样在页面上渲染成“Hello, Sara”：

1. 我们调用 `ReactDOM.render()` 函数，并传入 `<Welcome name="Sara" />` 作为参数。
2. React 调用 `Welcome` 组件，并将 `{name: 'Sara'}` 作为 props 传入。
3. `Welcome` 组件将 `<h1>Hello, Sara</h1>` 元素作为返回值。
4. React DOM 将 DOM 高效地更新为 `<h1>Hello, Sara</h1>`。

> 注意⚠️：**组件名称必须以大写字母开头**。
>
> React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 `Welcome`。

## 3. Props

### Props 的只读性

组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。

**React 组件都必须像纯函数一样保护它们的 props 不被更改。**

## 4. State

State 与 props 类似，但 state 是私有的，并且完全受控于当前组件。

### 正确地使用 State

#### 不要直接修改 State

例如，此代码不会重新渲染组件：

```react
// Wrong
this.state.comment = 'Hello';
```

而应该使用 `setState()`：

```react
// Correct
this.setState({comment: 'Hello'});
```

#### State 的更新可能是异步的

出于性能考虑，React 可能会把对个 `setState()` 调用合并成一个调用。

因为 `this.props` 和 `this.state` 可能会异步更新，所以不要依赖它们的值来更新下一个状态。

```react
// Wrong
this.setState({
	counter: this.state.counter + this.props.increment,
})
```

要解决这个问题，可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数：

```react
// Correct
this.setState((state, props)=> ({
	counter: state.counter + props.increment
}));
```

#### State 的更新会被合并

当你调用 `setState()` 的时候，React 会把你提供的对象合并到当前的 state。这里的合并是**浅合并**。

### 数据是向下流动的

任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。

## 5. 生命周期

从 16.7.0 开始，生命周期方法，只能用类组件。

![image-20200926113707892](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj3wewqfo6j31g00u0q8k.jpg)

### 组件的生命周期

#### 挂载

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

* [**`constructor()`**](https://zh-hans.reactjs.org/docs/react-component.html#constructor)
* [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
* [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)
* [**`componentDidMount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

> 注意：[`UNSAFE_componentWillMount()`](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillmount) 生命周期方法即将过时，在新代码中应该避免使用它们。（此方法是服务端渲染唯一会调用的生命周期函数。）

#### 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

* [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
* [`shouldComponentUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)
* [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)
* [`getSnapshotBeforeUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
* [**`componentDidUpdate()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)

> 注意：下述方法即将过时，在新代码中应该避免使用它们：
>
> * [`UNSAFE_componentWillUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillupdate)
> * [`UNSAFE_componentWillReceiveProps()`](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillreceiveprops)

#### 卸载

当组件从 DOM 中移除时会调用如下方法：

* [**`componentWillUnmount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount)

#### 错误处理

当渲染过程中，生命周期或子组件的构造函数中抛出错误时，会调用如下方法：

* [`static getDerivedStateFromError()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromerror)
* [`componentDidCatch()`](https://zh-hans.reactjs.org/docs/react-component.html#componentdidcatch)

## 6. 事件处理

React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同：

* React 事件的命名采用小驼峰式（camelCase），而不是纯小写。
* 使用 JSX 语法时，需要传入一个函数作为事件处理函数，而不是一个字符串。

```react
// 传统的 HTML
<button onclick="activateLasers()">
  Activate Lasers
</button>

// React 中的
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

### 事件 this 的绑定

在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this` 的值为 `undefined`。

```react
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

如果觉得使用 `bind` 很麻烦，这里有两种方式可以解决：

* 可以使用 class fields 正确的绑定回调函数：

  ```react
  class LoggingButton extends React.Component {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    // 注意: 这是 *实验性* 语法。
    handleClick = () => {
      console.log('this is:', this);
    }
  
    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      )
    }
  }
  ```

* 还可以在回调中使用[箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：

  ```react
  class LoggingButton extends React.Component {
    handleClick() {
      console.log('this is:', this);
    }
  
    render() {
      // 此语法确保 `handleClick` 内的 `this` 已被绑定。
      return (
        <button onClick={() => this.handleClick()}>
          Click me
        </button>
      )
    }
  }
  ```

  > 此语法问题在于每次渲染 `LoggingButton` 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。

### 向事件处理程序传递参数

以下两种方式都可以向事件处理函数传递函数：

```react
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

以上两种方式是等价的，分别通过[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)和 [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) 来实现。

## 7. 条件渲染

React 中的条件渲染和 JavaScript 中的一样，使用 JavaScript 运算符 [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 或者[条件运算符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)去创建元素来表现当前的状态，然后让 React 根据它们来更新 UI。

```react
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

声明一个变量并使用 `if` 语句进行条件渲染是不错的方式，但有时你可能会想使用更为简洁的语法。接下来，我们将介绍几种在 JSX 中内联条件渲染的方法。

* 与运算符 &&

  如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。

  ```react
  function Mailbox(props) {
    const unreadMessages = props.unreadMessages;
    return (
      <div>
        <h1>Hello!</h1>
        {unreadMessages.length > 0 &&
          <h2>
            You have {unreadMessages.length} unread messages.
          </h2>
        }
      </div>
    );
  }
  
  const messages = ['React', 'Re: React', 'Re:Re: React'];
  ReactDOM.render(
    <Mailbox unreadMessages={messages} />,
    document.getElementById('root')
  );
  ```

* 三元运算符

  另一种内联条件渲染的方法是使用 JavaScript 中的三元运算符 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。

  ```react
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
      <div>
        {isLoggedIn
          ? <LogoutButton onClick={this.handleLogoutClick} />
          : <LoginButton onClick={this.handleLoginClick} />
        }
      </div>
    );
  }
  
  ```

* 阻止组件渲染

  在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。

  ```react
  function WarningBanner(props) {
    if (!props.warn) {
      return null;
    }
  
    return (
      <div className="warning">
        Warning!
      </div>
    );
  }
  
  class Page extends React.Component {
    constructor(props) {
      super(props);
      this.state = {showWarning: true};
      this.handleToggleClick = this.handleToggleClick.bind(this);
    }
  
    handleToggleClick() {
      this.setState(state => ({
        showWarning: !state.showWarning
      }));
    }
  
    render() {
      return (
        <div>
          <WarningBanner warn={this.state.showWarning} />
          <button onClick={this.handleToggleClick}>
            {this.state.showWarning ? 'Hide' : 'Show'}
          </button>
        </div>
      );
    }
  }
  
  ReactDOM.render(
    <Page />,
    document.getElementById('root')
  );
  ```

  > 在组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期。例如，上面这个示例中，`componentDidUpdate` 依然会被调用。

## 8. 列表 & Key

### 渲染多个组件

你可以通过使用 `{}` 在 JSX 内构建一个元素集合。

```react
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

### key

* key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

  ```react
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  ```

* 一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。

  ```react
  const todoItems = todos.map((todo) =>
    <li key={todo.id}>
      {todo.text}
    </li>
  );
  ```

* 当元素没有确定 id 的时候，万不得已你可以使用元素索引 index 作为 key。

  ```react
  const todoItems = todos.map((todo, index) =>
    // Only do this if items have no stable IDs
    <li key={index}>
      {todo.text}
    </li>
  );
  ```

  > 如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。

#### 用 key 提取组件

元素的 key 只有放在就近的数组上下文中才有意义。

> 比方说，如果你[提取](https://zh-hans.reactjs.org/docs/components-and-props.html#extracting-components)出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。

#### key 只是在兄弟节点之间必须唯一

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的 key 值：

```react
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

key 会传递信息给 React ，但不会传递给你的组件。如果你的组件中需要使用 `key` 属性的值，请用其他属性名显式传递这个值：

```react
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

上面例子中，`Post` 组件可以读出 `props.id`，但是不能读出 `props.key`。

## 9. 表单

### 受控组件

使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

```react
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

#### 默认值

在受控组件上指定 value 的 prop 会阻止用户更改输入。如果你指定了 `value` ，但输入仍可编辑，则可能是你意外地将 `value` 设置为 `undefined` 或 `null`。

```react
// 不可编辑
ReactDOM.render(<input value="hi" />, mountNode);
// 可编辑
ReactDOM.render(<input value={null} /> mountNode>);
```

### 非受控组件

要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据。

```react
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

```

#### 默认值

在 React 渲染生命周期时，表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新。 在这种情况下, 你可以指定一个 `defaultValue` 属性，而不是 `value`。

```react
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={this.input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

## 10. 状态提升

通常，多个组件需要反映相同的变化数据，这时可以将共享状态提升到最近的共同父组件中去。



## 11. 组合 VS 继承

React 推荐我们使用组合而非继承来实现组件间的代码重用。

### 包含关系

#### 使用一个特殊的 `children` prop 来将它们的子组件传递到渲染结果中：

```react
function FancyBorder(props) {
	return (
  	<div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
	return (
  	<FancyBorder color="blue">
      <h1 className="Dialog-title">Welcome</h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecrafe!
      </p>
    </FancyBorder>
  );
}
```

#### 自行约定：将所需内容传入 props，并使用相应的 prop：

```react
function SplitPane(props) {
	return (
  	<div className="SplitPane">
      <div className="SplitPane-left">{props.left}</div>
    	<div className="SplitPane-right">{props.right}</div>
    </div>
  );
}

function App() {
	return (
  	<SplitPane 
      left={<Contacts />}
      right={<Chat />}
      />
  );
}
```

### 特例关系

#### “特殊“组件可以通过 props 定制并渲染”一般”组件：

```react
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

#### 组合也同样适用于以 class 形式定义的组件：

```react
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

### 继承

React 不推荐我们使用继承来构建组件层次。

Props 和组合为我们提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意 props，包括基本数据类型，React 元素以及函数。

