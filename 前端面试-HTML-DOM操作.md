#### 本文章知识点结构主要按照红宝书关于DOM的第10章



##### 10.1 节点层次

- 文档节点是每个文档的根节点，而根节点往往只有一个子节点，就是<html>元素，我们成为文档元素，文档元素是文档的最外层元素。而在XML中，没有预定义的元素，所以任何元素都可能成为文档元素

- 每一个标记都可以用节点树（也就是HTML DOM Tree）中的一个节点表示

- > http://www.w3school.com.cn/htmldom/dom_nodes.asp

- 10.1.1 节点类型

  - 每个节点都有一个nodeType属性，表明节点的类型，一共有12个节点，并不是所有节点类型都受到web浏览器的支持，我们常用的是元素（Node.ELEMENT_NODE)和文本节点(Node.TEXT_NODE)

  - ```
    // 适用于所有浏览器的确定节点类型方法
    if(somenode.nodeType == 1){
    	alert("Node is an element);
    }
    ```

  - 2 节点关系

    - 父元素，子元素，兄弟（同胞）元素

    - 每个节点都有一个childNodes属性，其中保存着一个NodeList对象。

      - NodeList是一种<u>**类**</u>数组对象，保存一组**<u>有序</u>**的节点，可以通过位置访问这些节点。NodeList是基于DOM结构**<u>动态</u>**执行查询的结果，因此DOM结构的变化能自动反应在NodeList中

    - ```javascript
      // 访问方式同数组
      var firsChild = someNode.childNodes[0];
      var secondChild = someNode.childNodes.item(1);
      var count = someNode.childNodes.length //是存在length属性的
      ```

      - NodeList可以转化为数组

      ```javascript
      // 这是红宝书提供的方法
      var arrayOfNodes = Array.prototype.slice.call(nodes, 0);

      // 在ES6中，有Array.from()的方法
      // 关于Array.from     https://segmentfault.com/a/1190000004450221  
      var arrayOfNodes = Array.from(nodes, 0);

      // 实际应用场景中，一般document.querySelectorAll('img')这个api会返回一个NodeList数组
      ```

      - 访问兄弟（Sibling）节点/ 确定节点是不是父节点的子节点列表中的首个还是末个

      ```javascript
      if(someNode.nextSibling === null){
          说明这是最后一个
      }else if(someNode.previousSibling === null){
          这是第一个
      }
      ```

    - 所有节点都有属性ownerDocument，指向文档节点

  - 3 操作节点

    - appendChild(someNode) 
      - 用于向childNodes列表的末尾添加一个节点，方法返回新增的节点
      - 如果传入的参数node已经是文档的一部分了，那结果是这个节点从原来的位置转移到新位置
    - insertBefore(someNode_1,someNode_2) 
      - 将node1节点放在childNode列表中node2节点后面
    - replaceChild(node1, node2) 
      - node2将会被替换成node1
      - 当node1插入的时候，node1的所有关系指针会从Node2复制过来
    - removeChild（node） 顾名思义
    - node.cloneNode(true/false) 
      - 用于创建一个与node相同的副本，当参数为true的时候执行深复制，也就是复制节点和整个子节点树；浅复制则只复制节点本身
      - 调用之后返回副本，但是属于文档所有，并没有指定父节点，所以我们往往需要appendChild（）等方法将这个副本添加到文档中
      - 该方法不会复制原节点的JS属性，比如事件处理啊什么的
    - node.normalize（）
      - 该方法会查找node节点的所有childNode，如果找到空文本节点，删掉；如果找到相邻的文本节点，则合并为一个文本节点

- 10.1.2 Document类型

  - 文档的子节点
    - .documentElement
      - 我们常用documentElement属性来访问元素，因为documentElement永远指向<html>元素
      - documentElement.balaba 或者 documentElement.childNodes[0] 都可以
    - document.body
      - 指向<body>
    - document.doctype
      - 指向<!DOCTYPE>
  - 文档信息
    - document.title  文档标题，显示在浏览器的标题栏/标签栏上
    - document.url
    - document.domain       取得域名
    - document.referrer        取得来源页面的URL
  - 查找信息
    - document.getElementById()   
      - 根据id来拿到对应DOM元素的引用，如果不存在就返回Null，注意这里的id是严格匹配的，包括**<u>大小写</u>**
      - 如果页面中多个元素ID相同，该方法返回第一次出现的那个
    - document.getElementsByTagName("img")
      - 接受一个参数——元素标签名，对于HTML来说这个参数**<u>不区分大小写</u>**
      - 返回包含至少0个元素的HTMLCollection对象，这个对象和NodeList类似
        - HTMLCollection[0]  这是调用了item()
        - HTMLCollection("myImage") 这是调用了namedItem（）
      - document.getElementsByTagName("*")返回文档中所有元素，按照出现的顺序排列
    - document.getElementsByName("color") 得到所有对应name名字的DOM对象
  - 特殊集合
    - document.forms 返回文档中所有的form元素
    - document.image 同上
    - document.links 返回文档中所有带href特性的<a>元素
  - DOM一致性检测
  - 文档写入(没有仔细看)
    - document.write（string） 接受一个字符串参数，写入到输入流的文本
    - document.writeln(string)  同行，但是末尾会加一个换行符\n 
      - document.write()和document.writeln（可以用来动态的加载外部资源，红宝书P260页）
    - document.open()  和  document.close()  用于打开和关闭网页的输出流。如果是在页面加载期间使用write()和writeln（）方法则不需要用open()和close()

- 10.1.3 Element类型

  - element类型用于表现xml或者html元素，提供了对元素标签名，子节点及属性的访问

  - element节点的属性

    - div.tagName/div.nodeName  得到标签名，这个标签名是**<u>大写的</u>**
    - div.nodeType 恒等为1
    - div.nodeValue 为Null 

  - HTML元素

    - 这里不展开了，都是基础

  - 操作-属性/特性/attribute，参数都为属性的名（**<u>不区分大小写</u>**），如果属性不存在会返回null

    - div.getAttribute(string)
    - div.setAttribute(string)
    - div.removeAttribute(string)
    - 有2种情况是特殊，
      - 第一个访问属性“sytle"，也就是访问CSS。如果通过iv.getAttribute(string)访问，返回值是CSS文本；如果用属性来访问，会得到一个对象
      - 第二个是访问onclick这样的事件处理程序，也就是访问JS代码。如果通过div.getAttribute(string)访问同样得到文本；通过属性访问会得到一个JS函数
      - 所以我们常用对象的属性值，像div.id 这种

  - attributes 属性

    - element类型是唯一一个使用attributes属性的DOM节点类型（element.attributes)，attributes属性中包含一个NamedNodeMap，与NodeList类似是一个动态的集合，它包含以下方法

      - element.attributes.getNamedItem(name)  返回nodeName属性等于name的节点

      - element.attributes.removeNamedItem(name) 同上，顾名思义

      - element.attributes.setNamedItem(node) 向列表中添加节点，以节点的nodeName作索引

        - ```
          这个没看懂啊...
          http://www.w3school.com.cn/jsref/met_namednodemap_setnameditem.asp
          ```

      - attributes属性中包含一系列的节点，每个节点的.nodeName就是属性名称，每个节点的.nodeValue就是属性的值

        - ```javascript
          // 得到这个属性的值
          var id = element.attributes.getNamedItem("id").nodeValue 

          //或写成
          var id = element.attributes[id].nodeValue 
          ```

        - attributes[i] 这种用法常用来遍历元素的属性 

  - 创建元素

    - document.createElement(string) string是创建元素的标签名，不区分大小写
    - 创建之后并没有位置信息，需要使用appendChild()等方法添加到DOM Tree中

  - 元素的子节点

    - 没仔细看

- 10.1.4  Text类型

  - **<u>没有子节点</u>**
  - nodeType == 3
  - .nodeName的值： ”#text“
  - .nodeValue 或者 .data 的值：节点所包含的文本
  - 常见API
    - **div.splitText(offset)  从offset制定的位置将当前的文本节点分为2个文本节点**
    - **div.substringData(offset,count)  提取子串**
    - div.appendData(text) 把text添加到节点的末尾
    -  div.deleteData（offset，count） 从offset指定位置删除count个字符
    - div.insertData（offset，text）, 顾名思义
    - div.replaceData（offset,count,text)  顾名思义
    - div.createTextNode(text)   顾名思义

- 10.1.5 comment类型

  - 没有细看了

- 后面都没有细看了~