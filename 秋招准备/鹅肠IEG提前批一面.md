- https://www.nowcoder.com/discuss/89155?type=2
   ​	1.自我介绍 

- 2.JS中的this了解吗，this指向是执行时定义还是什么时候定义的，箭头函数的this指向呢 

   - https://blog.csdn.net/marcelwu/article/details/78934069

   - 但是特别指出，箭头函数的this还是上下文定义的

   - this指针是执行时定义

   - 如果在函数里使用严格模式，全局函数里this的值就是`undefined`。而在匿名函数里则不会绑定任何对象。 

   - ```
      var person = {
          firstName   :"Penelope",
          lastName    :"Barrymore",
          showFullName:function () {
              // “上下文环境”
              console.log (this.firstName + " " + this.lastName);
          }
      }
      
      //当调用在person对象上定义的showFullName方法时，其“上下文环境”就是person对象
      //showFullName()方法里的this储存着person对象的值
      person.showFullName (); // Penelope Barrymore
      
      // If we invoke showFullName with a different object:
      //如果用其他对象来调用showFullName()方法：
      var anotherPerson = {
          firstName   :"Rohit",
          lastName    :"Khan"
      };
      
      //可以使用apply方法将this的设为特定的值 - 稍微会继续讨论apply()方法
      //无论哪个对象调用了this，this都会获取该对象的值，所以：
      person.showFullName.apply (anotherPerson); // Rohit Khan
      
      //所以上下文环境现在就是anotherPerson对象，因为是它通过使用apply()方法调用了person.showFullName ()这个方法
      ```

      当方法作为回调函数时，让this获取正确值的方式

      ​	如果要让`this.data`指代`user`对象的`data`属性，可以使用`Bind ()`，`Apply ()`或者`Call ()`方法给`this`设置特定的值。 

   - 匿名函数里的`this`无法访问外部函数的this，所以在非严格模式下其被绑定了window对象上。 

   - #### 在匿名函数里让this获取正确的值

      - #### 解决这个问题可以用JavaScript里一种常用的手法。在将匿名函数传给forEach()方法前，将this赋值给其他变量： 

   - ### 当使用this的方法被“借用”时

   - ```
      
      ```

      ```
      	var　a = 2;
      	function test(){
      		var a = 4;
      		console.log(this.a);
      		this.a = 1;
      	}
      	test();//2
      	//这里为什么是2？因为调用test()函数的是window，上述test()可以写成window.test(),test()内部的this指向的是window，而window中的a=2，所以console.log(this.a)打印出的是2
      	
      	var h = new test();//undefined 声明一个变量h，new一个构造函数用h接住，那么test()的指针this指向的就是h，输出的this.a自然就是undefined
      	console.log(h.a);//1  上述表达式是创建了一个构造函数，打印了this.a是undefined，注意：接着人家说：this.a=1，那么是的h这个对象的a=1，所有console.log(h.a)输出1咯
      	
      	
      	var k = {a:1000};
      	k.m = test; //将test（函数）变量赋值给k的m
      	k.m();//1000 ，这里this指的就是k，所有输出的就是1000咯，注意后面this.a=1,改变了k对象中的a，从原来的1000变成了this.a=1;
      	
      	k.m.apply();//1 上述所说的this.a=1指的就是k.a=1,apply(),括号内未写this指向谁那么默认指向调用该函数的对象，所以，this指向调用该函数的对象m（m是k对象的一个函数），这个函数里面的this指向的还是k
      	
      	k.m.call(k);//1 this指向k，k.a=1
      
      ```

   - https://www.cnblogs.com/xxcanghai/p/5189353.html  一个很恐怖的前端题目

- apply 是什么

- 3.改变this指向的方法有哪些，有什么区别呢，为什么bind要生成一个函数副本呢 

   - call

      - 该方法的作用和 [`apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法类似，只有一个区别，就是`call()`方法接受的是**若干个参数的列表**，而`apply()`方法接受的是**一个包含多个参数的数组**。 

   - apply

      - https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

      - ```
         func.apply(thisArg, [argsArray])
         
         thisArg
         	可选的。在 func 函数运行时使用的 this 值。需要注意的是，使用的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的包装对象。
         argsArray
         	可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。浏览器兼容性请参阅本文底部内容。
         ```

   - bind 

- 4.说一下JS的原型吧，原型链呢，怎么遍历一个对象上不是原型上的所有属性呢 

   - https://github.com/mqyqingfeng/Blog/issues/2  JS的原型，原型链

   - https://segmentfault.com/a/1190000012849832 遍历对象属性详解

      - 遍历自身可枚举的属性 (可枚举，非继承属性) **Object.keys()** 方法 

      - 遍历自身的所有属性(可枚举，不可枚举，非继承属性) **Object.getOwnPropertyNames()**方法 

         - ```
            var arr = ["a", "b", "c"];
            console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]
            ```

      - for...in ...会输出自身以及原型链上可枚举的属性

      - https://segmentfault.com/a/1190000014762317

- 5.怎么判断一个对象为空对象呢，回答了JSON方法，还有吗

   - 最常见的思路，`for...in...` 遍历属性，为真则为“非空数组”；否则为“空数组” 

      ```
      for (var i in obj) { // 如果不为空，则会执行到这一步，返回true
          return true
      }
      return false // 如果为空,返回false
      ```

       

   - 通过 `JSON` 自带的 `stringify()` 方法来判断:，`JSON.stringify()` 方法用于将 `JavaScript` 值转换为 `JSON` 字符串。 

      ```
      if (JSON.stringify(data) === '{}') {
          return false // 如果为空,返回false
      }
      return true // 如果不为空，则会执行到这一步，返回true
      ```

      

   - `ES6` 新增的方法 `Object.keys()`: 

      ```
      if (Object.keys(object).length === 0) {
          return false // 如果为空,返回false
      }
      return true // 如果不为空，则会执行到这一步，返回true
      ```

   - 那为什么会这样呢

      ```
      var a =[];a==[]
      =》  false
      var a =[];a===[]
      =》  false
      
      因为两个对象比较，会比较它们在内存中是不是同一个对象，虽然都是[],但是不是同一个，所以判断条件为false，可以用JSON.stringify(obj) === '[]'判断是否为空数组
      
      []可以理解为new Array()，相当于声明了一个新的空数组，程序会自动在堆中为其开辟一块内存空间，它和之前a = []生成的内存空间不是同一块，所以自然不相等。
      ```

      

- 6.用过cookie吗，怎么修改cookie呢，除了JS方法还有什么方法呢 

   - ```
      　document.cookie
      ```

- 7.现在我想修改cookie方法中的username，怎么做到呢 

   - JS设置啊我靠？可以在console里用JavaScript修改

      ```
      document.cookie="keyofcookie=valueofcookie"
      ```

- 8.cookie都有哪些参数呢，cookie的本质是什么，如果让你封装一个cookie方法你怎么实现 

   - https://segmentfault.com/a/1190000004556040

   - https://blog.csdn.net/helloliuhai/article/details/18351439

   - expires（现在已经被**max-age**属性所取代，**max-age用秒来设置cookie的生存期。** ）

   - secure，https协议中才会被发送

   - httpOnly，客户端无法修改

   - #### **domain属性** ，**path属性** 

      - #### `omain`是域名，`path`是路径，两者加起来就构成了 URL，`domain`和`path`一起来限制 cookie 能被哪些 URL 访问。

         一句话概括：某cookie的 `domain`为“baidu.com”, `path`为“/ ”，若请求的URL(URL 可以是js/html/img/css资源请求，**但不包括 XHR 请求**)的域名是“baidu.com”或其子域如“api.baidu.com”、“dev.api.baidu.com”，且 URL 的路径是“/ ”或子路径“/home”、“/home/login”，则浏览器会将此 cookie 添加到该请求的 cookie 头部中。

         所以`domain`和`path`2个选项共同决定了`cookie`何时被浏览器自动添加到请求头部中发送出去。如果没有设置这两个选项，则会使用默认值。`domain`的默认值为设置该`cookie`的网页所在的域名，`path`默认值为设置该`cookie`的网页所在的目录。

   - koa里session 配置项有一个overwrite：是否会覆盖之前设置同名的 session

      - https://segmentfault.com/a/1190000012412299

- 9.知道POST方法吗，POST方法如何设置传输的数据的格式 

   - https://www.cnblogs.com/zuobaiquan01/p/8414272.html
   - 最常见的是application/json
   - text/xml
   - multipart/form-data，我们使用表单上传文件时，必须让 表单的 enctype 等于 multipart/form-data 
   - application/x-www-form-urlencoded 浏览器的原生 表单，如果不设置 `enctype` 属性，那么最终就会以 `application/x-www-form-urlencoded`方式提交数据 

- 10.除了POST方法你还知道哪些方法

   - 不懂

- 11.前端向后端传输数据的方法有哪些，ajax?还有呢，fetch？还有呢，JSONP？还有吗，表单传输？ 

   - 不懂

- 12.详细说一下表单是怎么传输数据的 

   - 不懂

- 13.HTTP状态码知道哪些，有哪些场景会用到301呢 

- 14.浏览器缓存的过程是怎么样的 

- 15.cookie和session的区别 

- 17.知道事件代理吗，为什么使用事件代理，target和currenttarget 

- 18.POST传输的数据都有哪些格式呢 

- 19.CSS怎么样（我说不怎么样，然后...噩梦开始）知道background-size属性吗，具体是干嘛的呢 

- 20.让你用CSS实现一个类似于Word文档的布局，你怎么做？还有其他方法吗 

- 21.CSS实现一个旋转的动画如何实现，animation和transition有什么区别 

- 22.听说过逐帧动画吗，如果让你实现，你会如何实现 

- 23.前端如何实现直播的功能你了解吗，让你设计弹幕效果如何实现 

- 24.知道canvas吗，canvas怎么实现一个旋转的矩形 

- 25.CSS盒子模型了解吗，IE盒子模型会在IE的什么版本出现呢 

- 26.inline-block知道吗，应用场景都有哪些呢 

- 27.如何实现图片的懒加载呢，如何才能监听到图片进入屏幕，利用了哪些属性 

- 28.我们再来说说JS吧（可能是我的CSS太菜了吧...）说说前端跨域的几种方案吧 

- 29.具体说一下JSONP方法是如何实现的 

- 30.CORS方法呢，简单请求非简单请求了解吗，预检阶段发生在哪个阶段呢 

- 31.XSS CSRF知道吗，怎么防范呢 

- 32.回调地狱知道吗，怎么解决呢 

- 33.异步的解决方案有哪些呢 

- 34.Promise怎么执行一个串行的请求，并行呢？ 

- 35.async/await怎么实现上述请求呢，generator呢 

- 36.generator的自动化执行怎么实现的呢，co实现generator自执行的条件 

- 37.jQuery你用过是吧（我真想说没用过...）$符号是什么你能说一下吗 

- 38.jQuery XXX部分的源码你读过吗（没有...） 

- 39.设计模式了解吗，说一下单例模式，观察者模式 

- 40.让你写一个观察者模式你怎么写 

- 41.git你用过吧（求求你了...我真想说没用过） 

- 42.git merge 和git merge --no--ff有什么区别 

- 43.如果我想把暂存区中的修改回退用什么命令 

- 44.实习的时候都用过哪些命令呢 

- 45.算法数据结构怎么样，说一下快排？堆排？ 

- 46.你还有什么要问我的吗