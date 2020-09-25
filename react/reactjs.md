# React Learn

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

> JSX 是一种 JavaScript 语言扩展，有点类似于 HTML/XML。
>
> 在使用 react 库中，需要时常引入这段代码：
>
> ```javascript
> imput React from 'react';
> ```
>
> 这段代码片段是能够自解释的。因为 Babel 在 JSX 之上转换为 `React.createElement`。











## React 生命周期函数

从 16.7.0 开始，生命周期方法，只能用类组件。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj1uxuxlrcj31bs0s074l.jpg)

