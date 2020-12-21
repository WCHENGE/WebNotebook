# Next.js 入门教程

Next.js 是一个轻量级的 React 服务端渲染应用框架。它可以帮助我们简单轻松的实现React的服务端渲染，从而加快首屏打开速度，也可以作SEO（收索引擎优化了）。

> 在没有Next.js的时候，用React开发需要配置很多繁琐的参数，如Webpack配置，Router配置和服务器端配置等....。如果需要作SEO，要考虑的事情就更多了，怎么样服务端渲染和客户端渲染保持一致就是一件非常麻烦的事情，需要引入很多第三方库。但有了Next.js，这些问题都解决了，使开发人员可以将精力放在业务逻辑上，从繁琐的配置中解放出来。

## 1. Next.js 简介

 **Next.js 是一个轻量级的 React 服务器渲染应用框架。**

Next.js 的优点：

* 完善的React项目架构，搭建轻松。

  > 比如：Webpack配置，服务器启动，路由配置，缓存能力，这些在它内部已经完善的为我们搭建完成了。

* 自带数据同步策略，解决服务端渲染最大难点。

  > 把服务端渲染好的数据，拿到客户端重用，这个在没有框架的时候，是非常复杂和困难的。有了Next.js，它为我们提供了非常好的解决方法，让我们轻松的就可以实现这些步骤。

* 丰富的插件帮开发人员增加各种功能。

  > 每个项目的需求都是不一样的，包罗万象。无所不有，它为我们提供了插件机制，让我们可以在使用的时候按需使用。你也可以自己写一个插件，让别人来使用。

* 灵活的配置，让开发变的更简单。

  > 它提供很多灵活的配置项，可以根据项目要求的不同快速灵活的进行配置。

*目前Next.js是React服务端渲染的最佳解决方案，所以如果你想使用React来开发需要SEO的应用，基本上就要使用Next.js。*

## 2. 安装设置

### 自动安装设置：

建议使用 `create-next-app`创建新的 Next.js 应用程序，它会自动为你设置所有内容。创建项目，请运行：

```shell
npx create-next-app
# or 
yarn create next-app
```

### 手动安装设置：

为你的项目安装 `next`、`react` 和 `react-dom` ：

```shell
npm install next react react-dom
# or 
yarn add next react react-dom
```

打开 `package.json` 文件并添加 `scripts` 配置段：

```json
"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
```

这些脚本涉及开发应用程序的不同阶段：

- `dev` - 运行 `next`，在开发模式下启动 Next.js
- `build` - 运行 `next build`，生成用于生产环境的应用程序
- `start` - 运行 `next start`，启动 Next.js 生产环境服务器

## 3. 页面渲染（pages render）与获取数据

在 Next.js 中，一个 **page（页面）** 就是一个从 `.js`、`jsx`、`.ts` 或 `.tsx` 文件导出（export）的 [React 组件](https://reactjs.org/docs/components-and-props.html) ，这些文件存放在 `pages` 目录下。每个 page（页面）都使用其文件名作为路由（route）。

### 预渲染

默认情况下，Next.js 将 **预渲染** 每个 page（页面）。这意味着 Next.js 会预先为每个页面生成 HTML 文件，而不是由客户端 JavaScript 来完成。预渲染可以带来更好的性能和 SEO 效果。

每个生成的 HTML 文件都与该页面所需的最少 JavaScript 代码相关联。当浏览器加载一个 page（页面）时，其 JavaScript 代码将运行并使页面完全具有交互性。（此过程称为 *水合（hydration）*。）

#### 两种形式的预渲染

Next.js 具有两种形式的预渲染： **静态生成（Static Generation）** 和 **服务器端渲染（Server-side Rendering）**。这两种方式的不同之处在于为 page（页面）生成 HTML 页面的 **时机** 。

- [**静态生成 （推荐）**](https://www.nextjs.cn/docs/basic-features/pages#static-generation-recommended)：HTML 在 **构建时** 生成，并在每次页面请求（request）时重用。
- [**服务器端渲染**](https://www.nextjs.cn/docs/basic-features/pages#server-side-rendering)：在 **每次页面请求（request）时** 重新生成 HTML。

### [静态生成（推荐）](https://www.nextjs.cn/docs/basic-features/pages#静态生成（推荐）)

如果一个页面使用了 **静态生成**，在 **构建时（build time）** 将生成此页面对应的 HTML 文件 。这意味着在生产环境中，运行 `next build` 时将生成该页面对应的 HTML 文件。然后，此 HTML 文件将在每个页面请求时被重用，还可以被 CDN 缓存。

在 Next.js 中，你可以静态生成 **带有或不带有数据** 的页面。接下来我们分别看看这两种情况。

#### 生成不带数据的静态页面

默认情况下，Next.js 使用 “静态生成” 来预渲染页面但不涉及获取数据。如下例所示：

```jsx
function About() {
  return <div>About</div>
}

export default About
```

请注意，此页面在预渲染时不需要获取任何外部数据。在这种情况下，Next.js 只需在构建时为每个页面生成一个 HTML 文件即可。

#### [需要获取数据的静态生成](https://www.nextjs.cn/docs/basic-features/pages#需要获取数据的静态生成)

某些页面需要获取外部数据以进行预渲染。有两种情况，一种或两种都可能适用。在每种情况下，您都可以使用 Next.js 提供的特殊功能：

1. 您的页面 **内容** 取决于外部数据：使用 `getStaticProps`。
2. 你的页面 **paths（路径）** 取决于外部数据：使用 `getStaticPaths` （通常还要同时使用 `getStaticProps`）

##### [1. 页面 **内容** 取决于外部数据](https://www.nextjs.cn/docs/basic-features/pages#场景-1：-页面-内容-取决于外部数据)

Next.js 允许你从同一文件 `export（导出）` 一个名为 `getStaticProps` 的 `async（异步）` 函数。该函数在构建时被调用，并允许你在预渲染时将获取的数据作为 `props` 参数传递给页面。

```jsx
function Blog({ posts }) {
  // Render posts...
}

// 此函数在构建时被调用
export async function getStaticProps() {
  // 调用外部 API 获取博文列表
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // 通过返回 { props: posts } 对象，Blog 模块
  // 在构建时将接收到 `posts` 参数
  return {
    props: {
      posts,
    },
  }
}

export default Blog
```

##### [2. 页面路径取决于外部数据](https://www.nextjs.cn/docs/basic-features/pages#场景-2：页面路径取决于外部数据)

Next.js 允许你创建具有 **动态路由** 的页面。

> 例如，你可以创建一个名为 `pages/posts/[id].js` 的文件用以展示以 `id` 标识的单篇博客文章。当你访问 `posts/1` 路径时将展示 `id: 1` 的博客文章。

预渲染的页面 **paths（路径）** 取决于外部数据。为了解决这个问题，Next.js 允许你从动态页面（在这里是 `pages/posts/[id].js`）中 `export（导出）` 一个名为 `getStaticPaths` 的 `async（异步）` 函数。该函数在构建时被调用，并允许你指定要预渲染的路径。

> **例如**： 假设你只向数据库添加了一篇博客文章（标记为 `id: 1`）。这种情况下，你只想在构建时针对 `posts/1` 进行预渲染。稍后，你又添加了第二篇文章，标记为 `id: 2`。这是，你希望对 `posts/2` 也进行预渲染。

```jsx
// 此函数在构建时被调用
export async function getStaticPaths() {
  // 调用外部 API 获取博文列表
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // 根据博文列表生成所有需要预渲染的路径
  const paths = posts.map((post) => `/posts/${post.id}`)

  // We'll pre-render only these paths at build time.
  // { fallback: false } means other routes should 404.
  return { paths, fallback: false }
}
```

同样在 `pages/posts/[id].js` 中，你还需要export（导出） `getStaticProps` 以便可以获取 `id` 所对应的博客文章的数据并进行预渲染：

```jsx
function Post({ post }) {
  // Render post...
}

export async function getStaticPaths() {
  // ...
}

// 在构建时也会被调用
export async function getStaticProps({ params }) {
  // params 包含此片博文的 `id` 信息。
  // 如果路由是 /posts/1，那么 params.id 就是 1
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()

  // 通过 props 参数向页面传递博文的数据
  return { props: { post } }
}

export default Post
```

### 服务器端渲染

如果 page（页面）使用的是 **服务器端渲染**，则会在 **每次页面请求时** 重新生成页面的 HTML 。

要对 page（页面）使用服务器端渲染，你需要 `export` 一个名为 `getServerSideProps` 的 `async` 函数。服务器将在每次页面请求时调用此函数。

> 例如，假设你的某个页面需要预渲染频繁更新的数据（从外部 API 获取）。你就可以编写 `getServerSideProps` 获取该数据并将其传递给 `Page` ，如下所示：

```jsx
function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}

export default Page
```

如你所见，`getServerSideProps` 类似于 `getStaticProps`，但两者的区别在于 `getServerSideProps` 在每次页面请求时都会运行，而在构建时不运行。

### 小结：

Next.js 的两种预渲染形式：

- **静态生成（推荐）**： HTML 是在 **构建时（build time）** 生成的，并重用于每个页面请求。要使页面使用“静态生成”，只需导出（export）页面组件或导出（export） `getStaticProps` 函数（如果需要还可以导出 `getStaticPaths` 函数）。对于可以在用户请求之前预先渲染的页面来说，这非常有用。你也可以将其与客户端渲染一起使用以便引入其他数据。
- **服务器端渲染**： HTML 是在 **每个页面请求** 时生成的。要设置某个页面使用服务器端渲染，请导出（export） `getServerSideProps` 函数。由于服务器端渲染会导致性能比“静态生成”慢，因此仅在绝对必要时才使用此功能。

## 4. 获取数据（详细）

Next.js 获取数据以进行预渲染的三个独特函数：

* [`getStaticProps`](https://www.nextjs.cn/docs/basic-features/data-fetching#getstaticprops-static-generation)（静态生成）：在**构建时**获取数据。
* [`getStaticPaths`](https://www.nextjs.cn/docs/basic-features/data-fetching#getstaticpaths-static-generation)（静态生成）：指定[动态路由](https://www.nextjs.cn/docs/routing/dynamic-routes)以根据数据进行预渲染。
* [`getServerSideProps`](https://www.nextjs.cn/docs/basic-features/data-fetching#getserversideprops-server-side-rendering)（服务器端渲染）：在**每个页面请求**时获取数据。

### `getStaticProps`（静态生成）

如果在该页面通过 `async` 导出 `getStaticProps` 函数，则 Next.js 在构建预渲染此页面时将调用 `getStaticProps` 函数。

```jsx
export async function getStaticProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

`context` 参数包含一下对象：

- `params`包含使用动态路由的页面的路由参数。例如，如果页面名称是`[id].js`，`params`则将看起来像`{ id: ... }`。要了解更多信息，请查看[动态路由文档](https://www.nextjs.cn/docs/routing/dynamic-routes)。您应该将其与一起使用`getStaticPaths`，我们将在后面说明。
- `preview`是`true`，如果该页面在预览模式和`false`其他。请参阅[预览模式文档](https://www.nextjs.cn/docs/advanced-features/preview-mode)。
- `previewData`包含设置的预览数据`setPreviewData`。请参阅[预览模式文档](https://www.nextjs.cn/docs/advanced-features/preview-mode)。

什么情况下使用`getStaticProps`：

- 在用户请求之前，呈现页面所需的数据可在构建时获得。
- 数据来自 CMS。
- 数据可以公开缓存（不是特定于用户的）。
- 该页面必须预渲染（对于SEO）并且必须非常快-`getStaticProps`生成HTML和JSON文件，CDN可以将它们都缓存以提高性能。

### `getStaticPaths`（静态生成）

如果从使用动态路由的页面导出`async`调用的函数`getStaticPaths`，则Next.js将静态预呈现由指定的所有路径`getStaticPaths`。

```jsx
export async function getStaticPaths() {
  return {
    paths: [
      { params: { ... } } // See the "paths" section below
    ],
    fallback: true or false // See the "fallback" section below
  };
}
```





## 5. 内置对 CSS 的支持

Next.js 允许你在 JavaScript 文件中导入（import） CSS 文件。

### 添加全局样式表

要将样式表添加到您的应用程序中，请在 `pages/_app.js` 文件中导入（import）CSS 文件。

```jsx
import '../styles.css'

// This default export is required in a new `pages/_app.js` file.
export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

这些样式 (`styles.css`) 将应用于你的应用程序中的所有页面和组件。 由于样式表的全局特性，并且为了避免冲突，你应该 **只在 [`pages/_app.js`](https://www.nextjs.cn/docs/advanced-features/custom-app) 文件中导入（import）样式表**。

### 添加组件级 CSS

Next.js 通过 `[name].module.css` 文件命名约定来支持 [CSS 模块](https://github.com/css-modules/css-modules) 。

CSS 模块通过自动创建唯一的类名从而将 CSS 限定在局部范围内。 这使您可以在不同文件中使用相同的 CSS 类名，而不必担心冲突。

 这样就可以使 CSS 模块文件**可以导入（import）到应用程序中的任何位置**。

CSS 模块是一项 *可选功能*，**仅对带有 `.module.css` 扩展名的文件启用**。 普通的 `<link>` 样式表和全部 CSS 文件仍然是被支持的。

### CSS-in-JS

最简单的一种是内联样式：

```jsx
function HiThere() {
  return <p style={{ color: 'red' }}>hi there</p>
}

export default HiThere
```

使用 `styled-jsx` 的组件就像这样：

```jsx
function HelloWorld() {
  return (
    <div>
      Hello world
      <p>scoped!</p>
      <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
      <style global jsx>{`
        body {
          background: black;
        }
      `}</style>
    </div>
  )
}

export default HelloWorld
```

## 6. 静态文件服务

Next.js 支持将静态文件（例如图片）存放到根目录下的 `public` 目录中，并对外提供访问。`public` 目录下存放的静态文件的对外访问路径以 (`/`) 作为起始路径。

```jsx
// 例如，如果你添加了一张图片到 public/my-image.png 路径，则以下代码就能访问到此图片：

function MyImage() {
  return <img src="/my-image.png" alt="my image" />
}

export default MyImage
```

**注意**： 请勿为 `public` 改名。此名称是写死的，不能修改，并且只有此目录能过够存放静态资源并对外提供访问。

## 7. 路由简介



Next.js 是一个基于[pages概念](https://www.nextjs.cn/docs/basic-features/pages)的文件系统的路由器。将文件添加到`pages`目录后，它会自动用作路由。

### 路由简介

#### Index 路由

路由器将自动将名为`index`目录的文件路由到目录的根目录。

- `pages/index.js` → `/`
- `pages/blog/index.js` → `/blog`

#### 嵌套路由

路由器支持嵌套文件。如果创建嵌套的文件夹结构，则文件将仍然以相同的方式自动路由。

- `pages/blog/first-post.js` → `/blog/first-post`
- `pages/dashboard/settings/username.js` → `/dashboard/settings/username`

#### 动态路由

要匹配动态细分，您可以使用方括号语法。这使您可以匹配命名参数。

- `pages/blog/[slug].js` → `/blog/:slug` (`/blog/hello-world`)
- `pages/[username]/settings.js` → `/:username/settings` (`/foo/settings`)
- `pages/post/[...all].js` → `/post/*` (`/post/2020/id/title`)

### 页面之间的链接

Next.js路由器允许您在页面之间进行客户端路由转换，类似于单页面应用程序。

`Link`提供了称为的React组件来执行页面之间的路由转换。

```jsx
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/about">
          <a>About Us</a>
        </Link>
      </li>
    </ul>
  )
}

export default Home
```

当链接到静态路由时，`as`别名将显示在浏览器中的网址。

当链接到[动态](https://www.nextjs.cn/docs/routing/dynamic-routes)路由时，您必须提供`href`并`as`确保路由器知道要加载哪个JavaScript文件。

- `href`-`pages`目录中页面的名称。例如`/blog/[slug]`。
- `as`-将在浏览器中显示的网址。例如`/blog/hello-world`。

```jsx
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        <Link href="/blog/[slug]" as="/blog/hello-world">
          <a>To Hello World Blog post</a>
        </Link>
      </li>
    </ul>
  )
}

export default Home
```

该`as`道具也可以动态生成。例如，要显示已作为道具传递到页面的帖子列表：

```jsx
function Home({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href="/blog/[slug]" as={`/blog/${post.slug}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
  )
}
```