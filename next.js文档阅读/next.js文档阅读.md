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

- 

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