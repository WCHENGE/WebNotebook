# 移动WEB开发之响应式布局

## 1. rem 单位

rem（root em）是一个相对单位，类似于 em，==em== 是==父元素==字体大小。不同的是 ==rem== 的基准是==相对于 html 元素==的字体大小。

> 比如，根元素（html）设置 font-size = 12px 非根元素设置 width: 2rem；则换成 px 表示就是 24px。

## 2. 媒体查询

CSS 媒体查询为我们提供了一种应用 CSS 的方法，仅在浏览器和设备的环境与你指定的规则相匹配的时候CSS 才会真的被应用。

### 2.1 什么是媒体查询

媒体查询（Media Query）是 CSS3 新语法。

* 使用 @media 查询，可以针对不同的媒体类型定义不同的样式；
* @media 可以针对不同的屏幕尺寸设置不同的样式；
* 当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面；
* 目前针对很多苹果手机、Android 手机、平板等设备都用到多媒体查询；

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glwhlgmhxuj30pu098q8v.jpg" alt="image-20201222114627062" style="zoom: 50%;" />

### 2.2 语法规范



```css
@media media-type and|not|only (media-feature-rule) {
	/* CSS rules go here */
}
```

* 用 @media 开头，注意 @ 符号；

* media-type  媒体类型；

  > 一个媒体类型，告诉浏览器这段代码是用在什么类型的媒体上的（例如印刷品或者屏幕）；

* 关键字 and | not | only ；

* media-feature-rule 媒体特性规则，必须有小括号包含。

  > 一个媒体表达式，是一个被包含的CSS生效所需的规则或者测试；

#### 2.2.1 mediatype 查询类型

将不同的终端设备划分成不同的类型，称为媒体类型。

|    值     | 解释说明                               |
| :-------: | -------------------------------------- |
|    all    | 用于所有设备                           |
|   print   | 用于打印机和打印预览                   |
| ==scree== | ==用于电脑屏幕，平板电脑，智能手机等== |

#### 2.2.2 关键字

关键字将媒体类型或多个媒体特性链接到一起做为媒体查询的条件。

* and：可以将多个媒体特性连接到一起，相当于“且”的意思；
* not：排除某个媒体类型，相当于“非”的意思，可以省略；
* only：指定某个特定的媒体类型，可以省略。

#### 2.2.3 媒体特性

每种媒体类型都具有各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格。

|    值     | 解释说明                                       |
| :-------: | ---------------------------------------------- |
|   width   | 定义输出设备中页面可见区域的宽度               |
| min-width | 定义输出设备中页面最小可见区域宽度（大于等于） |
| max-width | 定义输出设备中页面最大可见区域宽度（小于等于） |

（注意它们要加小括号包含，且最大值 max-width 和最小值 min-width 都包含等于的）

### 2.3 媒体查询 + rem 实现元素动态大小变化

* rem 单位是跟着 html 元素字体大小的变化而变化，有了 rem，页面元素可以设置不同大小尺寸；
* 媒体查询可以根据不同设备宽度来修改样式；
* 媒体查询 + rem 就可以根据不同设备的宽度，来实现元素大小的动态变化；

### 2.4 引入资源

当样式比较繁多的时候，我们可以针对不同的媒体使用不同 stylesheets（样式表）。

原理：直接在 link 中判断设备的尺寸，然后引入不同的 css 文件。

#### 2.4.1 语法规范

```css
<link rel="stylesheet" media="mediatype and|not|only (media-feature-rule)" href="mystylesheet.css" >
```

> 引入资源就是针对不同的屏幕尺寸，调用不同的 css 文件。

## 3. Less 基础

### 3.1 维护 css 的弊端

CSS 是一门非程序式语言，没有变量、函数、scope（作用域）等概念。

* CSS 需要输血大量看似没有逻辑的代码，CSS冗余是比较高的；
* 不方便维护及扩展，不利于复用；
* CSS 没有很好的计算能力；
* 非前端开发工程师来讲，往往会因为缺少 CSS 编写经验而很难写出组织良好且易于维护的 CSS 代码项目。

### 3.2 Less 介绍

Less（Leaner Style Sheets）是一门 CSS 扩展语言，也成为CSS预处理器。

> 常见的 CSS 预处理器：Sass、Less、Stylus。

作为 CSS 的一种形式的扩展，它并没有减少 CSS的功能，而是在现有的 CSS 语法上，为 CSS 加入程序式语言的特性。它在 CSS 的语法基础之上，引入了变量，Mixing（混入），运算以及函数等功能，大大简化了CSS 的编写，并且降低了CSS 的维护成本，就像他的名称所说的那样，Less 可以让我们用更少的代码做更多的事情。

Less 是一门 CSS 预处理语言，它扩展了CSS的动态特性。

Less中文网站：http://lesscss.cn/

### 3.3 Less 使用

首先新建一个后缀名为 less 的文件，在这个 less 文件里面书写 less 语句。

#### 3.3.1 less 变量

变量是指没有固定的值，可以改变的。因为我们 CSS 中的一些颜色和数值等经常使用。

```
@变量名：值；
```

**变量命名规范：**

* 必须有 @ 为前缀；
* 不能包含特殊字符；
* 不能以数字开头；
* 大小写敏感；

#### 3.3.2 Less 嵌套

经常用到选择器的嵌套：

```css
#header .logo {
	width: 300px;
}
```

Less 嵌套写法：

```less
#header {
  .logo {
  	width: 300px;
  }
}
```

如果遇到（**交集｜伪类｜伪元素选择器**）:

* 内层选择器的前面没有 & 符号，则它被解析为副选择器的后代；
* 如果有 & 符号，它就被解析为父元素自身或父元素的伪类；

```css
a:hover {
	color: red;
}
```

Less 嵌套写法：

```less
a {
  &:hover {
  	color: red;
  }
}
```

#### 3.3.3 Less 运算

任何数字、颜色或者变量都可以参与运算。Less 提供了加（+）、减（-）、乘（*）、除（/）算术运算。

```less
@width: 10px + 5;
div {
	border: @width solid red;
}

/* 甚至还可以这样 */
width: (@width + 5) * 2;
```

⚠️**注意：**

* 乘号（*）和除号（/）的写法；
* 运算符中间左右必须要用空格隔开（1px + 5）；
* 如果两个值之间只有一个值有单位，则运算结果就取该单位；
* 对于两个不同的单位的值之间的运算，运算结果的值取第一个值的单位；

## 4. rem 适配方案

使用媒体查询根据不同的设备按比例设置 html 的字体大小，然后页面元素使用 rem 做尺寸单位，当 html 字体大小变化，元素尺寸也会发生变化，从而达到等比例缩放的适配。

### 4.1 rem 实际开发适配方案：

1. 按照设计稿与设备宽度的比例，动态计算并设置 html 根标签的 font-size 大小；（媒体查询）
2. CSS中，设计稿元素的款、高、相对位置等取值，按照同等比例换算为 rem 为单位的值；

### 4.2 rem 适配方案技术使用

技术方案1：

* less
* 媒体查询
* rem

技术方案2：

* [flexible .js](https://github.com/amfe/lib-flexible)
* rem



动态设置 html 标签 font-size 大小：

1. 假设设计稿时 750px;
2. 假设我们把整个屏幕划分为 15等份（划分标准不一，可以是 20等份，也可以是 10等份）；
3. 那么每一份作为 html 字体大小，这里就是 50px;
4. 那么在 320px 设备的时候，字体大小为 320/15 就是 21.33px；
5. 用我们页面元素的大小除以不同的 html 字体大小会发现它们比例还是相同的；
6. 比如我们以 750 为标准设计稿；
7. 一个 100 * 100 像素的页面元素在 750屏幕下，就是 100/50 转换为 rem 是 2rem ;
8. 320 屏幕下，html 字体大小为 21.33，则 2rem = 42.66px，此时宽和高都是 42.66；
9. 已经实现了在不同屏幕下，页面元素盒子等比例缩放的效果。

### 4.3 页面元素的 rem 值

元素大小取值方法：

公式：页面元素的 rem 值 = 页面元素值（px）/（屏幕宽度 / 划分的份数）

或者：页面元素的 rem 值 = 页面元素值（px）/ html font-size 字体大小

（html font-size 大小：屏幕宽度 / 划分的份数）

### 4.4 设置视窗标签

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0" >
```





