##### 从浏览器地址栏输入url到显示页面的步骤

https://zhuanlan.zhihu.com/p/34453198?group_id=957277540147056640

https://segmentfault.com/a/1190000006879700

1. 输入地址

   1. 浏览器会首先解析URL从而获得如下信息
      1. protocol 协议，如”http“
      2. resource “/“ 请求的资源是balabala
   2. 转化非ASCII的Unicode字符，浏览器检查输入是否含有不是 `a-z`， `A-Z`，`0-9`， `-` 或者 `.` 的字符
   3. 当输入的URL协议或主机名不合法的时候，浏览器会将地址栏中输入的文字传给默认的搜索引擎；大部分情况下，这穿文字传递给搜索引擎的时候，URL会带有特定的一串字符，用来告诉搜索引擎这次搜索来自这个特定浏览器。

2. 对浏览器模型的整体概念

   1. 浏览器是多进程的，有一个主控进程，以及每一个tab页面都会新开一个进程
   2. 浏览器内核是多线程的，输入url后会开一个新的网络线程
      1. GUI线程
      2. JS引擎线程，这也是为什么常说JS引擎是单线程的
      3. 事件触发线程
      4. 定时器线程
      5. 网络请求线程
   3. 进程与线程的区别
      1. 一个进程管着多个线程
      2. 进程是cpu资源分配的最小单位，线程是cpu调度的最小单位，线程是建立在进程的基础上的一次程序运行单位
      3. 进程拥有独立的内存单元，而多个线程共享内存
      4. 进程是操作系统分配的，对于开发者来说，线程是你自己的程序分配的

3. 浏览器查找域名的 `IP` 地址（也称为DNS查询得到IP）

   1. 插播一下DNS = Domain Name System，域名**系统**，是一个域名和IP地址相互映射的一个分布式数据库

   2. **浏览器缓存 -**浏览器缓存DNS记录一段时间。操作系统并没有告诉浏览器每个DNS记录的生存时间，因此浏览器将它们缓存一段固定的时间（根据浏览器的不同而不同，2到30分钟）。

   3. **操作系统缓存** - 如果浏览器缓存不包含所需的记录，浏览器会进行系统调用（Windows中为gethostbyname）

   4. **路由器高速缓存** - 请求继续到您的路由器，路由器通常有自己的DNS高速缓存。

   5. **ISP DNS缓存** - 检查的下一个地方是缓存ISP（Internet Service Provider，互联网服务提供商）的DNS服务器。自然，有了缓存。

   6. **递归搜索** - 您的ISP的DNS服务器开始递归搜索，从根名称服务器，.com顶级域名服务器到Facebook的域名服务器。通常，DNS服务器在高速缓存中将具有.com名称服务器的名称，因此命名根名称服务器不是必需的。

   7. DNS查询的拓展

      1. CDN调度器

         - CDN，Content Delivery Network，内容分发网络，CDN系统能够实时地将用户的请求重新导向离用户最近的服务节点上。
         - 客户端浏览器先检查是否有本地缓存是否过期，如果过期，则向**CDN边缘节点发起请求，CDN边缘节点会检测用户请求数据的缓存是否过期**，如果没有过期，则直接响应用户请求，此时一个完成http请求结束;如果数据已经过期，那么CDN还需要向源站发出回源请求(back to the source request),来拉取最新的数据。
         - 解决因分布、带宽、服务器性能带来的访问延迟问题

      1. DNS循环复用，Round-robin DNS

         1. > 循环DNS（Round-robin DNS）技术是负载平衡最常用的方法之一。最早的负载均衡技术是通过DNS服务中的随机名字解析来实现的。在DNS服务器中，可以为多个不同的地址配置同一个名字，这个数据被发送给其他名字服务器，而最终查询这个名字的客户机将在解析这个名字时随机使用其中一个地址。因此，对于同一个名字，不同的客户机会得到不同的地址，因此不同的客户访问的也就是不同地址的Web服务器，从而达到负载均衡的目的
            >
            > http://blog.csdn.net/vajoy/article/details/12284483

            ​

      3. 负载均衡 load-balance 

      4. **nginx**（鹅肠考到我这个东西 ），**调度服务器根据实际的调度算法，分配不同的请求给对应集群中的服务器执行，然后调度器等待实际服务器的HTTP响应，并将它反馈给用户**    

      5. DNS解析是很慢的行为，浏览器可以使用DNS预解析，dns-prefetch，当浏览网页时，浏览器会在加载网页时对网页中的域名进行解析缓存，这样在单击当前网页中的连接时就无需进行DNS的解析，减少用户等待时间，提高用户体验

      3. 地理DNS通过将域名映射到不同的IP地址提高可伸缩性

         1. > 许多DNS还支持基于地理位置的域名解析，即会将域名解析成距离用户地理最近的一个服务器地址，这样就可以加速用户访问，改善性能。

      4. 大多数DNS服务器 anycast 来实现查询DNS的高可用性和低延迟（这个也不清楚啊..)

   8. ARP过程，既然以上通过DNS，用域名查到了IP地址，接下来要利用IP地址查找对应的MAC地址

      1. ARP，address Resolution Protocol  根据IP地址获取MAC地址的TCP/IP协议

      2. 主机发送信息时将包含目标IP地址的ARP请求广播到网络上的所有主机，并接收返回消息，以此确定目标的物理地址；

         1. 地址解析协议是建立在网络中各个主机互相信任的基础上的，网络上的主机可以自主发送ARP应答消息，其他主机收到应答报文时不会检测该报文的真实性就会将其记入本机ARP缓存；

            由此攻击者就可以向某一主机发送伪ARP应答报文，使其发送的信息无法到达预期的主机或到达错误的主机，这就构成了一个[ARP欺骗](http://baike.baidu.com/view/155386.htm)。

      3. 得到的ARP reply

         1. > `ARP Reply`:
            >
            > ```
            > Sender MAC: target:mac:address:here
            > Sender IP: target.ip.goes.here
            > Target MAC: interface:mac:address:here
            > Target IP: interface.ip.goes.here
            > ```

   9. DNS和ARP的关系（？其实是2个玩意emm）

      1. > http://blog.163.com/happy_bupt/blog/static/2190961192013816112633180/
         >
         > DNS是应用层协议，简单点说就是将域名网址转为IP地址，是将域名与IP联系。而ARP是网络层协议，是在以太网中通过IP地址得到[物理地址](http://zhidao.baidu.com/search?word=%CE%EF%C0%ED%B5%D8%D6%B7&fr=qb_search_exp&ie=gbk)，将IP与mac联系。他们2个是完全不同的东西。

         > MAC地址它是为了解决相邻主机间的通信而存在的，什么是相邻？在一个子网内就可以认为是相邻的。
         >
         > IP地址它是为了[解决网](http://zhidao.baidu.com/search?word=%BD%E2%BE%F6%CD%F8&fr=qb_search_exp&ie=gbk)络与网络之间的通信而存在的。这里的网络与网络可以是相同子网或不同子网。
         >
         > 好了，一个网络中的主机想要与另一个网络中的主机通信（假设发送方知道对端IP），这个数据可能经过中间多个主机，才能到达目的地吧，所以发送方得知道数据下一站邻居主机的MAC地址，这里就是在解决相邻主机间的通信问题。这就是IP与MAC关系，完成一次通信，他们都得用到。
         >
         > 那我怎么知道邻居主机的MAC地址呢？那就是通过[ARP协议](http://zhidao.baidu.com/search?word=ARP%D0%AD%D2%E9&fr=qb_search_exp&ie=gbk)。  
         >
         > 最后，发送方是怎么知道另一个网络中目标主机IP地址的？
         > 你在浏览网页的时候，不是输了一个域名比如[www.baidu.com](http://www.baidu.com/)
         > 就是你电脑能过DNS协议与电信的DNS服务器交互，DNS服务器告诉你电脑对方IP地址，才知道的，这就是IP地址与DNS的关系。

         ​

4. 浏览器向 `web` 服务器发送一个 `HTTP` 请求

   1. http请求长这样

      1. > ```
         > //请求行，包括了请求方法，请求URL，使用的协议（HTTP）和版本（1.1）
         > GET http://facebook.com/ HTTP/1.1
         >
         > // accept 指出了浏览器端可以接受的媒体类型，text/html代表html文档，如果服务器无法返回指定类型的数据，服务器会返回406错误（non acceptable），可以使用通配符*代表任意类型
         > Accept: application/x-ms-application, image/jpeg, application/xaml+xml, [...]
         >
         > // 告诉HTTP服务器， 客户端使用的操作系统和浏览器的名称和版本.
         > User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; [...]
         >
         > // 浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate）
         > Accept-Encoding: gzip, deflate
         >
         > // Connection: keep-alive 指出当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭
         > Connection: Keep-Alive
         > Host: facebook.com
         >
         > // 这里展示下cookie的语法和一些常见使用https://www.cnblogs.com/christineHu/p/6136825.html
         > // 这个cookie的笔记下一次再整理吧
         > Cookie: datr=1265876274-[...]; locale=en_US; lsd=WW[...]; c_user=2101[...]
         > ```

   2. 请求头的详解

      1. > https://www.cnblogs.com/printN/p/6534529.html
         >
         > http://blog.csdn.net/u010256388/article/details/68491509 （这篇详细一点）

   3. tcp/ip请求

      1. 三次握手：

         - TCP握手：
           - *SYN*(synchronous)是*TCP*/IP建立连接时使用的握手信号。在标志位上，在客户机和服务器之间建立正常的*TCP*网络连接时,客户机首先发出一个*SYN*消息,服务器使用*SYN*+ACK应答表示接收到了这个消息，最后客户机再以[ACK](https://baike.baidu.com/item/ACK)消息响应。这样在客户机和服务器之间才能建立起可靠的TCP连接
           - seq是序列号，第一个seq是客户端随机生成的；
           - 确认号ack是4个字节，32个位。期待收到对方下一个报文段的第一个数据字节的序号；序列号表示报文段携带数据的第一个字节的编号；而确认号指的是期望接收到下一个字节的编号；**因此当前报文段最后一个字节的编号+1即为确认号。** 也就是收到的seq+1
           - 确认ACK，占1位，当ACK=1的时候，确认号Ack才有效，否则无效
           - 第一次握手
             - 主机A向主机B发送TCP连接请求数据包（syn=1），随机产生一个初始序列号seq = j。（其中报文中同步标志位SYN=1，ACK=0，表示这是一个TCP连接请求数据报文；序号seq=x，表明传输数据时的第一个数据字节的序号是x）；
           - 第二次握手
             - 服务器收到syn包,必须确认客户的SYN，将标志位的SYN 和 ACK 都设置为1，ack = j+1,同时自己也发送一个SYN包（seq 随机出一个 k）,即SYN+ACK包,此时服务器进入SYN_RECV状态；
           - 第三次握手
             - 客户端收到服务器的SYN＋ACK包,检查ack是不是j+1 ACK是否为1,如果是，则将标志位的ACK设置为1,向服务器发送确认包ACK(ack=k+1),此包发送完毕,Server检查ack是否为K+1，ACK是否为1，如果是，客户端和服务器进入ESTABLISHED状态,完成三次握手.
         - 第三次握手，主机A发送一次确认是为了防止：如果客户端迟迟没有收到服务器返回的确认报文，这时他会放弃连接，重新启动一条连接请求；但问题是：服务器不知客户端没收到，所以他会收到两个连接请求，白白浪费了一条连接开销。       

      2. 四次挥手，TCP释放的四次挥手

         - TCP连接时全双工的，因此，每个方向都必须要单独进行关闭，这一原则是当一方完成数据发送任务后，发送一个FIN来终止这一方向的连接，收到一个FIN只是意味着这一方向上没有数据流动了，即不会再收到数据了，但是在这个TCP连接上仍然能够发送数据，直到这一方向也发送了FIN。首先进行关闭的一方将执行主动关闭，而另一方则执行被动关闭

         - > 假设主机A为客户端，主机B为服务器，其释放TCP连接的过程如下：
           >
           > ​    1） 关闭客户端到服务器的连接：首先客户端A发送一个FIN，用来关闭客户到服务器的数据传送，然后等待服务器的确认。其中终止标志位FIN=1，序列号seq=u
           >
           >  　 2） 服务器收到这个FIN，它发回一个ACK，确认号ack为收到的序号加1。
           >   　3） 关闭服务器到客户端的连接：也是发送一个FIN给客户端。
           >  　 4） 客户段收到FIN后，并发回一个ACK报文确认，并将确认序号seq设置为收到序号加1。

         - 第一次挥手

           - Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态

         - 第二次挥手

           - Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态

         - 第三次挥手

           - Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态

         - 第四次挥手

           - Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手

         - 为什么是四次挥手

           - 这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送。
           - 因为当Server端收到Client端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。但是关闭连接时，当Server端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉Client端，"你发的FIN报文我收到了"。只有等到我Server端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四步握手

      3. 浏览器对同一域名下的Tcp连接请求是有并发限制的

      4. 五层因特网协议栈

         1. 应用层(dns,http) DNS解析成IP并发送http请求
         2. 传输层(tcp,udp) 建立tcp连接（三次握手）
         3. 网络层(IP,ARP) IP寻址
         4. 数据链路层(PPP) 封装成帧
         5. 物理层(利用物理介质传输比特流) 物理传输

5. 服务器响应重定向（从 `http://example.com` 到 `http://www.example.com`）

   1. 响应内容

      1. > ```
         > // 这里返回状态码301，代表永久性转移(Permanently Moved)，301重定向是网页更改地址后对搜索引擎友好的最好方法，只要不是暂时搬移的情况，都建议使用301来做转址。
         > // 302 redirect　　302代表暂时性转移(Temporarily Moved )
         > HTTP/1.1 301 Moved Permanently
         > Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
         >       pre-check=0
         > Expires: Sat, 01 Jan 2000 00:00:00 GMT
         > Location: http://www.facebook.com/
         > P3P: CP="DSP LAW"
         > Pragma: no-cache
         > Set-Cookie: made_write_conn=deleted; expires=Thu, 12-Feb-2009 05:09:50 GMT;
         >       path=/; domain=.facebook.com; httponly
         > Content-Type: text/html; charset=utf-8
         > X-Cnection: close
         > Date: Fri, 12 Feb 2010 05:09:51 GMT
         > Content-Length: 0
         > ```

   2. 什么是重定向

      1. 重定向就是服务器返回301或者302之后，浏览器会根据返回的location上的url 重新请求一次，称为重定向

      2. 301,redirect: 301 代表永久性转移(Permanently Moved)

      3. 302,redirect: 302 代表暂时性转移(Temporarily Moved )

      4. 尽量要使用301跳转

         1. > 从网址A 做一个302 重定向到网址B 时，主机服务器的隐含意思是网址A 随时有可能改主意，重新显示本身的内容或转向其他的地方。大部分的搜索引擎在大部分情况下，当收到302 重定向时，一般只要去抓取目标网址就可以了，也就是说网址B。如果搜索引擎在遇到302 转向时，百分之百的都抓取目标网址B 的话，就不用担心网址URL 劫持了。问题就在于，有的时候搜索引擎，尤其是Google，并不能总是抓取目标网址。比如说，有的时候A 网址很短，但是它做了一个302 重定向到B 网址，而B 网址是一个很长的乱七八糟的URL 网址，甚至还有可能包含一些问号之类的参数。很自然的，A 网址更加用户友好，而B 网址既难看，又不用户友好。这时Google 很有可能会仍然显示网址A。由于搜索引擎排名算法只是程序而不是人，在遇到302 重定向的时候，并不能像人一样的去准确判定哪一个网址更适当，这就造成了网址[URL](http://baike.baidu.com/view/1496.htm) 劫持的可能性。也就是说，一个不道德的人在他自己的网址A 做一个302 重定向到你的网址B，出于某种原因， Google 搜索结果所显示的仍然是网址A，但是所用的网页内容却是你的网址B 上的内容，这种情况就叫做网址URL 劫持。你辛辛苦苦所写的内容就这样被别人偷走了。302 重定向所造成的网址URL 劫持现象，已经存在一段时间了。不过到目前为止，似乎也没有什么更好的解决方法。在正在进行的谷歌大爸爸数据中心转换中，302 重定向问题也是要被解决的目标之一。从一些搜索结果来看，网址劫持现象有所改善，但是并没有完全解决。

   3. 为什么要重定向

      1. 大多数情况是某些大体量的网站注册了多个域名，需要通过重定向让访问的用户自动跳转到主站点
      2. 或者是网站调整（改变网页目录结构），网页被移到一个新地址，网页扩展名改变等情况

   4. 重定向与服务器请求转发的区别

      1. > http://blog.csdn.net/qq_32010299/article/details/51829539
         >
         > 一句话，转发是服务器行为，重定向是客户端行为。
         >
         > ​
         >
         > 重定向，其实是两次request
         >
         > 第一次，客户端request   A,服务器响应，并response回来，告诉浏览器，你应该去B。这个时候IE可以看到地址变了，而且历史的回退按钮也亮了。重定向可以访问自己web应用以外的资源。在重定向的过程中，传输的信息会被丢失。
         >
         > ​
         >
         > 请求转发  是服务器内部把对一个request/response的处理权，移交给另外一个服务器
         >
         > 对于客户端而言，它只知道自己最早请求的那个A，而不知道中间的B，甚至C、D。传输的信息不会丢失。

5. 浏览器跟踪重定向地址，也就是重新发起一个定向请求

6. 服务器处理请求

   1. 这里值得注意的是 这里会进行验证用户权限啊方法权限等

7. 服务器返回一个 `HTTP` 响应

   1. > ```
      > HTTP/1.1 200 OK
      > Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
      >     pre-check=0
      > Expires: Sat, 01 Jan 2000 00:00:00 GMT
      > P3P: CP="DSP LAW"
      > Pragma: no-cache
      > Content-Encoding: gzip
      > Content-Type: text/html; charset=utf-8
      > X-Cnection: close
      > Transfer-Encoding: chunked
      > Date: Fri, 12 Feb 2010 09:05:55 GMT
      > ```

   2. 注意content-type设置为text/html。 这个header指示浏览器内容呈现为HTML

8. 浏览器显示 HTML，浏览器获得资源后（如HTML，CSS，JS，图片等）

   1. > http://taligarsiel.com/Projects/howbrowserswork1.htm  权威说明

   2. 解析 —— HTML，CSS，JS

      1. 首先解析HTML（或者是SVG/XHTML），会产生一个DOM Tree，这个构建过程是一个深度遍历：当前节点的所有子节点都构建好后才会去构建当前节点的下一个兄弟节点。 
      2. 然后解析CSS 会产生CSS规则树，CSS Rule Tree
      3. 然后根据DOM树和CSSOM来构造渲染树，Rendering Tree
      4. 有了Render Tree，浏览器已经能知道网页中有哪些节点、各个节点的CSS定义以及他们的从属关系。下一步操作称之为Layout，顾名思义就是计算出每个节点在屏幕中的位置。 
      5. 再下一步就是绘制，即遍历render树，并使用UI后端层绘制每个节点
         1. 重点：上述这个过程是`逐步完成`的，为了更好的用户体验，渲染引擎将会尽可能早的将内容呈现到屏幕上，并不会等到所有的html都解析完成之后再去构建和布局render树。它是解析完一部分内容就显示一部分内容，同时，可能还在通过网络下载其余内容
         2. 两个概念
            1. Reflow（回流）：浏览器要花时间去渲染，当它发现了某个部分发生了变化影响了布局，那就需要倒回去重新渲染。
               1. reflow的原因有
                  - 页面初始化
                  - 操作DOM（这也就是MVVM不希望操作DOM的原因）
                  - 某些元素的尺寸变了
                  - 如果 CSS 的属性发生变化了
               2. 如何减少reflow
                  1. 不要一条一条地修改 DOM 的样式，与其这样，还不如预先定义好 css 的 class，然后修改 DOM 的 className。
                  2. 不要把 DOM 结点的属性值放在一个循环里当成循环里的变量。 
                  3.  千万不要使用 table 布局。因为可能很小的一个小改动会造成整个 table 的重新布局
            2. Repaint（重绘）：如果只是改变了某个元素的背景颜色，文字颜色等，不影响元素周围或内部布局的属性，将只会引起浏览器的repaint，重画某一部分。
            3.  **Reflow要比Repaint更花费时间，也就更影响性能。所以在写代码的时候，要尽量避免过多的Reflow**
      6. 解析JS脚本，主要通过DOM API 和 CSSOM API 来操作 DOM Tree 和 CSS Rule Tree
         1. JavaScript载入后马上执行
         2. 执行时会阻塞页面后续的内容（包括页面的渲染、其它资源的下载）。
            - 原因：因为浏览器需要一个稳定的DOM树结构，而JS中很有可能有 代码直接改变了DOM树结构，比如使用 document.write 或 appendChild,甚至是直接使用的location.href进行跳转，浏览器为了防止出现JS修 改DOM树，需要重新构建DOM树的情况，所以 就会阻塞其他的下载和呈现
         3. **我们大都会将script标签放在body结束标签之前，那原因是什么**（这个其实我不太懂，没仔细看）
            1. 将所有的script标签放到页面底部，也就是body闭合标签之前，这能确保在脚本执行前页面已经完成了DOM树渲染

   3. 渲染（render） —— 构建 DOM 树 -> 渲染 -> 布局 -> 绘制

   4. 这里提供一个写的好的版本

      1. > http://blog.csdn.net/xifeijian/article/details/10813339
         >
         > **HTML页面加载和解析流程** 
         >
         > 1. 用户输入网址（假设是个html页面，并且是第一次访问），浏览器向服务器发出请求，服务器返回html文件； 
         > 2. 浏览器开始载入html代码，发现＜head＞标签内有一个＜link＞标签引用外部CSS文件； 
         > 3. 浏览器又发出CSS文件的请求，服务器返回这个CSS文件； 
         > 4. 浏览器继续载入html中＜body＞部分的代码，并且CSS文件已经拿到手了，可以开始渲染页面了； 
         > 5. 浏览器在代码中发现一个＜img＞标签引用了一张图片，向服务器发出请求。此时浏览器不会等到图片下载完，而是继续渲染后面的代码； 
         > 6. 服务器返回图片文件，由于图片占用了一定面积，影响了后面段落的排布，因此浏览器需要回过头来重新渲染这部分代码； 
         > 7. 浏览器发现了一个包含一行Javascript代码的＜script＞标签，赶快运行它； 
         > 8. Javascript脚本执行了这条语句，它命令浏览器隐藏掉代码中的某个＜div＞ （style.display=”none”）。突然少了这么一个元素，浏览器不得不重新渲染这部分代码； 
         > 9. 终于等到了＜/html＞的到来，浏览器泪流满面…… 
         > 10. 等等，还没完，用户点了一下界面中的“换肤”按钮，Javascript让浏览器换了一下＜link＞标签的CSS路径； 
         > 11. 浏览器召集了在座的各位＜div＞＜span＞＜ul＞＜li＞们，“大伙儿收拾收拾行李，咱得重新来过……”，浏览器向服务器请求了新的CSS文件，重新渲染页面。

9. 浏览器发送请求获取嵌入在 HTML 中的资源（如图片、音频、视频、CSS、JS等等）

   1. 每个对应资源的URL请求都会经历一个类似于HTML页面的过程
   2. 但是静态文件将允许浏览器缓存他们

10. 浏览器发送异步请求（AJAX）