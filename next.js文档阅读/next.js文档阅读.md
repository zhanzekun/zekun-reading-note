# Next.js文档阅读  （7.0.0）         

- 前置提醒，next.js目前只支持react  16以上的版本

- css

  - 内置styled-jsx来编写 独立作用域的css

  - 支持内嵌样式，用花括号括起来

    ```javascript
    export default () => <p style={{ color: 'red' }}>hi there</p>
    ```

  - 服务端渲染时的css更改可以通过包裹自定义的document实现

  - 使用.css .scss .less .styl需要下载对应插件并配置next.config.js文件

- 静态文件服务

  - 根目录下新建文件夹，名为static（名字为约定，不可更改）

- key属性可以避免重复渲染相同的标签，如head等标签

- next.js中的组件和react中一样，可以是有状态组件也可以是无状态组件，如果是有状态组件，除了react客户端的生命周期之外还多了一个在服务端的生命周期，为getInitalProps（），该方法能异步获取JS普通对象并绑定在props上。这样客户端的生命周期就能获取到服务端生命周期内的数据（通过props传递）

  - getInitalProps（）的入参对象属性
    - `pathname` - URL 的 path 部分
    - `query` - URL 的 query 部分，并被解析成js对象
    - `asPath` - 显示在浏览器中的实际路径（包含查询部分），为`String`类型
    - `req` - HTTP 请求对象 (只有服务器端有)
    - `res` - HTTP 返回对象 (只有服务器端有)
    - `jsonPageRes` - [获取数据响应对象](https://github.com/zeit/next.js/tree/7.0.0-canary.8/examples/with-aphrodite) (只有客户端有)
    - `err` - 渲染过程中的任何错误

- 路由，这里路由主要分为客户端路由切换和服务端路由切换

  - 客户端路由切换主要用link组件，它支持每个组件所支持的onClick事件，它有一个默认行为是滚动到页面顶部，如果页面有hash(#)定义时，页面会滚动到定义处；如果不想要滚动到顶部，添加sroll=false属性。

    - 可以使用<link prefetch>使链接和预加载在后台同时进行，来达到页面的最佳性能。

  - 服务端路由切换主要用next/router这个组件来实现

    - 它支持拦截功能（利用popstate），用这个可以做类似于权限路由的事情；
    - next/router有一系列对应的路由相关事件供监听，监听可以手动开启/取消；
      - `routeChangeStart(url)` - 路由开始切换时触发
      - `routeChangeComplete(url)` - 完成路由切换时触发
      - `routeChangeError(err, url)` - 路由切换报错时触发
      - `beforeHistoryChange(url)` - 浏览器 history 模式开始切换时触发
      - `hashChangeStart(url)` - 开始切换 hash 值但是没有切换页面路由时触发
      - `hashChangeComplete(url)` - 完成切换 hash 值但是没有切换页面路由时触发
        - 这里的`url`是指显示在浏览器中的 url。如果你用了`Router.push(url, as)`（或类似的方法），那浏览器中的 url 将会显示 as 的值。

    ```jsx
    import Router from 'next/router'
    
    export default () =>
      <div>
        Click <span onClick={() => Router.push('/about')}>here</span> to read more
      </div>
    ```

  - next.js中的浅层路由（shallow routing），允许我们改变服务端路由跳转但是不执行getInitialProps生命周期。一般用法是加载相同页面的 URL，得到更新后的路由属性`pathname`和`query`，并不失去 state 状态。**注意:浅层路由只作用于相同 URL 的参数改变**

  - 注意，客户端路由是有状态的，而服务端渲染中的服务端路由是无状态的

- 预加载页面（预渲染？），这个功能只有在生产环境里才有。

  - Next.js 有允许你预加载页面的 API。用 Next.js 服务端渲染你的页面，可以达到所有你应用里所有未来会跳转的路径即时响应，最大程度的初始化网站性能，但是预加载只加载js代码，页面渲染要等待数据请求。
  - 这个预加载只支持客户端路由加载的；建议 prefetch 事件写在`componentDidMount()`生命周期里。

- 自定义服务端路由

- next.js支持动态导入

  - 基础支持

    ```jsx
    import dynamic from 'next/dynamic'
    
    const DynamicComponent = dynamic(import('../components/hello'))
    
    export default () =>
      <div>
        <Header />
        <DynamicComponent />
        <p>HOME PAGE is here!</p>
      </div>
    ```

  - 加载组件后 修改再引入

    ```jsx
    import dynamic from 'next/dynamic'
    
    const DynamicComponentWithCustomLoading = dynamic(
      import('../components/hello2'),
      {
        loading: () => <p>...</p>
      }
    )
    
    export default () =>
      <div>
        <Header />
        <DynamicComponentWithCustomLoading />
        <p>HOME PAGE is here!</p>
      </div>
    ```

  - 还可以设置禁止使用SSR

  - 也支持同时加载多个模块拼接在一起

    ```jsx
    import dynamic from 'next/dynamic'
    
    const HelloBundle = dynamic({
      modules: () => {
        const components = {
          Hello1: import('../components/hello1'),
          Hello2: import('../components/hello2')
        }
    
        return components
      },
      render: (props, { Hello1, Hello2 }) =>
        <div>
          <h1>
            {props.title}
          </h1>
          <Hello1 />
          <Hello2 />
        </div>
    })
    
    export default () => <HelloBundle title="Dynamic Bundle" />
    ```

- 自定义<App>，可以通过重写./pages/_app.js文件来控制页面初始化

  - 当页面变化时保持页面布局
  - 当路由变化时保持页面状态
  - 使用`componentDidCatch`自定义处理错误
  - 注入额外数据到页面里 (如 GraphQL 查询)

- 自定义<Document>，重写./pages/_document.js文件

  - next.js自动定义了html文件头部的一些html标签，比如head meta之类的，但是如果我们想重写，比如添加上一些css库/一些meta属性等等，就重写这个文件

    ```jsx
    // _document is only rendered on the server side and not on the client side
    // Event handlers like onClick can't be added to this file
    
    // ./pages/_document.js
    import Document, { Head, Main, NextScript } from 'next/document'
    
    export default class MyDocument extends Document {
      static async getInitialProps(ctx) {
        const initialProps = await Document.getInitialProps(ctx)
        return { ...initialProps }
      }
    
      render() {
        return (
          <html>
            <Head>
              <style>{`body { margin: 0 } /* custom! */`}</style>
            </Head>
            <body className="custom_class">
                这个main是渲染html主页面，可以通俗的理解为就是放页面中的react组件，所有的react组件会被打包放在这个main标签里。如果渲染发生错误，main会自动渲染error.js页面
                这个nextsctipt……我也不知道是干嘛的
                英文文档里是这些写的：All of <Head />, <Main /> and <NextScript /> are required for page to be properly rendered. 
              <Main />
              <NextScript />
            </body>
          </html>
        )
      }
    }
    ```

- 自定义错误处理

  - 404和500错误客户端和服务端都会通过`error.js`组件处理。如果你想改写它，则在page目录下新建`_error.js`
    - 意思就是说其实4xx错误和5xx错误都在error.js处理，想为特殊的http错误处理不同的请求就要用组件化的方式

- 自定义配置，在根目录下新建next.config.js，有2种方式编写

  - 直接module.exports

    ```JavaScript
    // next.config.js
    module.exports = {
      // 巴拉巴拉
    }
    ```

  - 用函数，这里phase是配置文件被加载时的当前内容（create-next-app的脚手架就是这么写的）

    ```javascript
    module.exports = (phase, {defaultConfig}) => {
      return {
        // 巴拉巴拉
      }
    }
    ```

  - next.js默认生成etags到每个页面中，可以在配置里禁止

  - next.config.js里海暴露了若干个选项来控制服务器部署和缓存页面：onDemandEntries属性

  - next.config.js可配置页面后缀名解析扩展，以此支持typescript

    ```javascript
    // next.config.js
    module.exports = {
      pageExtensions: ['jsx', 'js']
    }
    ```

  - 支持配置构建id

- 自定义webpack配置，也是在next.config.js里配置。注意：*webpack方法将被执行两次，一次在服务端一次在客户端。你可以用isServer属性区分客户端和服务端来配置*。

- 自定义babel配置：在根目录下新建.babelrc文件

- Next.js的优势

  - 路由不需要被提前知道
  - 路由总是被懒加载
  - 顶层组件可以定义生命周期`getInitialProps`来阻止路由加载（当服务端渲染或路由懒加载时）

- 服务端渲染究竟是怎么提高了首屏加载的性能呢

  - > <https://segmentfault.com/a/1190000012928465>
    >
    > 其实可以看 《深入React技术栈》的第七章， 介绍的非常详细。
    > 概括来说 React 之所以可以做到服务端渲染 是因为ReactDOM提供了服务端渲染的API
    >
    > - renderToString  把一个react 元素转换成带reactid的html字符串。
    > - renderToStaticMarkup 转换成不带reactid的html字符串，如果是静态文本，用这个方法会减少大批的reactid.
    >
    > 这两个方法的存在 ，实际上可以把react看做是一个模板引擎。解析jsx语法变成普通的html字符串。
    >
    > 我们可以调用这两个API 实现传入ReactComponent 返回对应的html字符串到客户端。浏览器端接收到这段html以后不会重新去渲染DOM树，只是去做事件绑定等操作。这样就提高了首屏加载的性能。

- 服务端渲染的缺点：

  - > 服务端渲染相当于是把客户端的处理流程部分移植到了服务端，这样就增加了服务端的负载。因此要做一个好的SSR方案，缓存是必不可少的。与此同时工程化方面也是有很多值得优化的地方。

- 参考文献

  - > <http://jartto.wang/2018/06/08/nextjs-3/>
    >
    > 