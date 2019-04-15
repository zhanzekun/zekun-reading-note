## JavaScript设计模式——结构型设计模式（第9-15章）

- 套餐服务——外观模式

  - 就是为了兼容不同的系统环境/API接口/版本差异等等，在现有底层的API上封装一层更高级的统一接口，让接口使用者无需关注底层是怎么实现功能的。比如可以理解为node.j中的libuv提供了跨平台（win and linux）的统一接口，win系统下使用libev，linux下使用IOCP，就是一种外观模式的运用。

  - 写html的时候我们不能直接写document.onclick~ 因为onclick是DOM 0级事件，也就是说一个元素只能有一个onclick事件，我们随便写容易覆盖掉同事写的onclick事件，要用DOM2级的addEventListener来实现

  - DOM分级事件

    - > <https://www.cnblogs.com/chaogex/p/3959723.html> 
      >
      > 1级DOM仅以映射文档结构为目标，DOM 2级面向更为宽广。通过对原有DOM的扩展，2级DOM通过对象接口增加了对鼠标和用户界面事件
      >
      >  
      >
      > <https://kingapex.iteye.com/blog/401168>

- 水管弯弯——适配器模式

  - 当我们引入外来的三方库（甚至是三方底层库）的时候，有时候需要我们修改已有代码，做一些兼容。让API使用者无法感知到我们换了库，保持顶层API不变化，就是做了适配器模式。

  - 参数适配器

    - 意思就是不要排列参数咯

      ```javascript
      // 不可取的写法(也不要写成数组)
      function A(name, title, age, color,size, prize){
          // todo
      }
      
      // 正确写法 👇
      
      // obj = {
      //     name: 'duanwei'
      //     title:
      //     age:
      //     size:
      //     color:
      //     prize:
      // }
      
      function A(obj){
          // todo
      }
      
      // 那么参数适配器就是
      
      function adapter(name, title, age, color, size, prize) {
          var obj = {
              name: name,
              title: title,
              age: age,
              size: size,
              color: color,
              prize: prize
          }
          return obj
      }
      
      ```

- 牛郎织女——代理模式

  - 这里乱七八糟讲了Jsonp跨域…fine

- 房子装修——装饰者模式

  - 思路就是方法+新方法添加到目标对象上咯…

    ```javascript
    var decorator = function(input, fn){
        var input = document.getElementById(input);
        // 该事件源已经绑定了onclick事件
        if(typeof input.onclick === 'function'){
            var old_click_fn = input.onclick;
            input.onclick = function(){
                old_click_fn();
                fn();
            }
        }
        // 如果事件源没有绑定onclick，那就绑定新的onclick函数给它
        else{
            input.onclick = fn;
        }
    }
    ```

- 👍 **城市间的公路——桥接模式**

  - 这个还是挺重要的，定义是：**在系统沿着多个维度变化的时候，我们使用桥接模式，在不增加其复杂度的情况下完成解耦**

  - **桥接模式最重要的特点是将实现层（比如元素绑定事件）与抽象层（修饰页面UI逻辑）解耦分离，让2部分可以独立变化**

  - ```javascript
    // 抽象成（主题，{变化目的/变化事件}）, 这个主题在调用的时候用this指代
    function changeColor(dom, color){
        dom.style.color = color
    }
    
    // 实现
    var spans = document.getElementById('span')
    spans.onmousemove = function(){
        changeColor(this, 'red')
    }
    ```

- 超值午餐——组合模式

  - 把复杂的系统分解成细小的模块，然后按需添加…
  - 这里代码可以看一下…但是作者写的也太TM的冗杂了

- 城市公交车——享元模式

  - 意思就是公用方法写在原型链上…让大家一起用...

- 总结：

  - 外观/适配器模式的意思就是：让API调用者感知不到底层实现的变化，对API使用者来说变更是无痛的