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

- 4.说一下JS的原型吧，原型链呢，怎么遍历一个对象上不是原型上的所有属性呢 

- 5.怎么判断一个对象为空对象呢，回答了JSON方法，还有吗 

- 6.用过cookie吗，怎么修改cookie呢，除了JS方法还有什么方法呢 

- 7.现在我想修改cookie方法中的username，怎么做到呢 

- 8.cookie都有哪些参数呢，cookie的本质是什么，如果让你封装一个cookie方法你怎么实现 

- 9.知道POST方法吗，POST方法如何设置传输的数据的格式 

- 10.除了POST方法你还知道哪些方法

- 11.前端向后端传输数据的方法有哪些，ajax?还有呢，fetch？还有呢，JSONP？还有吗，表单传输？ 

- 12.详细说一下表单是怎么传输数据的 

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