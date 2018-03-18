# Vue入门与基本原理

- #### Vue是什么

  - Vue是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以**自底向上逐层应用**。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，**Vue 也完全能够为复杂的单页应用提供驱动**。

  - Vue是一套MVVM下的前端框架

    - 首先我们看下什么是MVC框架
      - MVC也是我们常见的客户端开发的模式
      - MVC包含3个部分：view，model，controller
      - View:：就是展现出来的用户界面。
      - model：就是业务逻辑相关的数据对象，通常从数据库映射而来，我们可以说是与数据库对应的model。
      - controller：除了简单的 Model、View 以外的所有部分都被放在了 Controller 里面。Controller 负责显示界面、响应用户的操作、网络请求以及与 Model 交互。直观的说，JSP从服务器返回html代码？**这就导致了 Controller臃肿，逻辑复杂，难以维护；与view结合太紧密，无法测试**
        - 本质原因是：view和model的复用性高，而剩下的controller层代码复用性太低
        - 但是现在通过合理的切分模块，可以一定程度解决这个问题
        - 所以微软提出MVVM的概念
    - 什么是MVVM（View - Viewmodel - model）
      - view和model没有不同
      - view model：就是与界面(view)对应的Model。因为，数据库结构往往是不能直接跟界面控件一一对应上的，所以，需要再定义一个数据对象专门对应view上的控件。而ViewModel的职责就是把model对象封装成可以显示和接受输入的界面数据对象。
      - 将MVC中的部分职责给view model
        - 校验用户输入。（比如我们常见的在前端就检查 你输入是否是个邮箱啊之类的）
        - 网络请求。
        - 展示层的逻辑，比如格式化字符串。
        - 其他不能放入 Model，与 View 无关的逻辑。
      - MVVM的优点在
        - MVVM 兼容 MVC，可以先创建一个简单的 View Model，再慢慢迁移。
        - **MVVM 使得 app 更容易测试，因为 View Model 部分不涉及 UI。**
      - http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html  阮一峰的概念
      - http://www.cnblogs.com/carr-css/p/6140450.html  具体到Vue的MVVM概念

- #### 感受一下Vue吧！

    - 声明式渲染：

      - > file:///C:/users/zhanzekun/documents/github/zekun-reading-note/3.17%E5%9F%B9%E8%AE%AD%E8%AF%BEvue%E5%85%A5%E9%97%A8%E7%9A%84%E6%96%87%E4%BB%B6/hello-vue.html

    - 条件与循环

      - 数据不仅可以绑定DOM文本与特性，还可以绑定到DOM结构
      - 此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用[过渡效果](https://cn.vuejs.org/v2/guide/transitions.html)。

    - 监听用户输入

      - 注意在 `reverseMessage` 方法中，我们更新了应用的状态，但没有触碰 DOM
      - 这样会让网页性能快很多
        - 介绍一下 浏览器重新渲染的机制
      - Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

  - Vue模板语法

    - 插值

      - 文本和JS表达式： 用{{ meg }} 双花括号

        - 使用 [v-once 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定

          ```
          <span v-once>这个将不会改变: {{ msg }}</span>
          ```

      - 原始html代码

        ```
        <p>Using mustaches: {{ rawHtml }}</p>
        <p>Using v-html directive: <span v-html="rawHtml"></span></p>
        ```

      - 特性（标签属性）不能使用{{}}的语法，所以我们用 v-bind

        ```
        <div v-bind:id="dynamicId"></div>
        ```

    - 指令

      - v-开头的
      - v-bind简写为  ：
      - v-on简写为  @

- 计算属性和监听器

    - 计算属性

        ```
        <div id="example">
          <p>Original message: "{{ message }}"</p>
          <p>Computed reversed message: "{{ reversedMessage }}"</p>
        </div>

        var vm = new Vue({
          el: '#example',
          data: {
            message: 'Hello'
          },
          computed: {
            // 计算属性的 getter
            reversedMessage: function () {
              // `this` 指向 vm 实例
              return this.message.split('').reverse().join('')
            }
          }
        })
        ```

    - 计算属性 与 方法的对比

        - 结果都一样，效果也一样
        - 计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数，这对一个性能开销很大的计算属性来说，是很赞的
        - 而方法是触发重新渲染时，调用方法总会再次执行函数

    - 计算属性 与 侦听属性的对比

        - 好像只有写法的简洁好处

    - 计算属性默认只有getter，但是我们也可以设置一个setter

- **Class与Style绑定**可以动态的控制某个元素的Class

- **条件渲染**

    - v-if
    - v-show

- **列表渲染** v-for

    - 这里我们特别提一下key

        - 当 Vue.js 用 `v-for` 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素
        - 这个默认的模式是高效的，但是只适用于**不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。
        - 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` 属性。理想的 `key` 值是每项都有的且唯一的 id

    - 一段取值范围的v-for

        - `v-for` 也可以取整数。在这种情况下，它将重复多次模板。

            ```
            <div>
              <span v-for="n in 10">{{ n }} </span>
            </div>

            // 12345678910
            ```

    - v-for 的优先级 比v-if 高

    - 2.2.0+ 的版本里，当在组件中使用 `v-for` 时，`key` 现在是必须的。

- **事件处理** v-on

    - 事件修饰符，修饰符可以串联，但是顺序很重要相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。
        - .stop： 阻止单击事件继续传播
        - .prevent： 阻止单击事件继续传播
        - .capture：添加事件监听器时使用事件捕获模式，即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 
        - .self
        - .once：点击事件只会触发一次
    - 按键修饰符：意思就是意思键盘事件的时候，检查键值，在xx值就触发xx事件
    - 系统修饰符：意思就是监听鼠标or键盘事件的监听器，比如ctrl,alt,shitf之类

- 表单输入绑定 v-model


  - Vue的组件化概念

      - 什么是组件？

          - 积木块
          - 所有的 Vue 组件同时也都是 Vue 的实例，所以可接受相同的选项对象 (除了一些根级特有的选项) 并提供相同的生命周期钩子。

    - vue提倡组件系统，许我们使用小型、独立和通常可复用的组件构建大型应用（搭积木）

    - 全局注册，组件在注册之后，便可以作为自定义元素 `<my-component></my-component>` 在一个实例的模板中使用

      ```
      <div id="example">
        <my-component></my-component>
      </div>

      // 注册
      Vue.component('my-component', {
        template: '<div>A custom component!</div>'
      })

      // 创建根实例
      new Vue({
        el: '#example'
      })
      ```

    - 局部注册

      - ```
        var Child = {
          template: '<div>A custom component!</div>'
        }

        new Vue({
          // ...
          components: {
            // <my-component> 将只在父组件模板中可用
            'my-component': Child
          }
        })
        ```

    - data必须是函数，如果不是就报错

      ```
      <div id="example-2">
        <simple-counter></simple-counter>
        <simple-counter></simple-counter>
        <simple-counter></simple-counter>
      </div>

      var data = { counter: 0 }

      Vue.component('simple-counter', {
        template: '<button v-on:click="counter += 1">{{ counter }}</button>',
        // 技术上 data 的确是一个函数了，因此 Vue 不会警告，
        // 但是我们却给每个组件实例返回了同一个对象的引用
        data: function () {
          return data
        }
      })

      new Vue({
        el: '#example-2'
      })

      //由于这三个组件实例共享了同一个 data 对象，因此递增一个 counter 会影响所有组件！
      ```

      ```
      data: function () {
        return {
          counter: 0
        }
      }

      // 这样每个组件就有自己的内部状态了
      ```

    - 组件之间的通信/组合：在 Vue 中，父子组件的关系可以总结为 **prop 向下传递，事件向上传递**。父组件通过 **prop**给子组件下发数据，子组件通过**事件**给父组件发送消息。看看它们是怎么工作的。

    - prop

      - 组件实例的作用域是**孤立的**。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。父组件的数据需要通过 **prop** 才能下发到子组件中

      - 动态prop，我们可以用 `v-bind` 来动态地将 prop 绑定到父组件的数据。每当父组件的数据变化时，该变化也会传导给子组件：

      - 单向数据流：Prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。我们不应该在子组件内部改变Prop，如果做了，console会警告你。

        - 那假如prop作为初始值传入后，子组件想把它当局部数据使用；或者子组件要处理成其他数据输出

        - 请定义一个局部变量并用prop值来初始化它

          ```
          props: ['initialCounter'],
          data: function () {
            return { counter: this.initialCounter }
          }
          ```

          ​

        - 定义一个计算属性，处理 prop 的值并返回

          ```
          props: ['size'],
          computed: {
            normalizedSize: function () {
              return this.size.trim().toLowerCase()
            }
          }
          ```

      - prop验证：我们可以为组件的 prop 指定验证规则。如果传入的数据不符合要求，Vue 会发出警告

      - 非prop特性——会替换/合并现有特性

        - 当对待clas / style 会合并

      - 自定义事件，子组件与父组件通信的手段

        - v-on绑定自定义事件
          - $on(eventName) 监听事件
          - $emit(eventName, optionalPayload)触发事件
        - v-on.native绑定原生事件
        - .sync可以让prop变成双向绑定
        - 表单里可以用v-model 进行数据双向绑定

      - 非父子组建的通信

        - 简单情况可以用$emit和$on组合
        - 复杂情况用状态管理模式

  - Vue2.0生命周期
    - **beforeCreate**： 组件实例刚被创建，组件属性计算之前，如data属性，可以在这加个loading事件 
    - **created**：组件实例创建完成，属性已绑定，但是DOM没生成，$el属性不存在，在这结束loading，还做一些初始化，实现函数自执行 
    - **beforeMount**：模板编译/挂载之前
    - **mounted**：模板编译/挂载之后，在这发起后端请求，拿回数据，配合路由钩子做一些事情
    - **beforeUpdate**：组建更新之前
      - （执行下app.message= 'yes !! I do';）
    - **updated**：组建更新之后
    - activated：for keep-alive，组件被激活时调用
    - deactivated：for keep-alive，组件被移除时调用
    - **beforeDestory**：组件销毁前调用，你确认删除XX吗？ destoryed ：当前组件已被删除，清空相关内容
      - （执行下app.$destroy();）
    - **destoryed**：组建销毁后调用

  - Vue 实例的概念：所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 

  - vue的响应式原理

      - 如何追踪变化
      - 检测变化的注意事项
      - 声明响应式属性
      - 异步更新队列

  - 虚拟Dom技术
    - 在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

  - Vue的双向绑定原理