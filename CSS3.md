# CSS3

- 边框

  - ```
    border：2px solid；
    ```

  - 圆角 border-radius，

  ```
  div{
  	border:2px solid;
  	border-radius:25px; //也可以单独设置4个角的值
  }
  ```

  - 盒阴影 box-shadow

    ```
    div
    {
    	box-shadow: 10px 10px 5px #888888; // 参数分别为：水平阴影的位置（可以是负数），垂直阴影的位置，模糊距离，(模糊尺寸)，颜色，inset
    }
    ```

  - 边界图片 border-image

- 背景

  - background-image：

    ```
     background-image: url(img_flwr.gif), url(paper.gif); 
    ```

  - background-size 设置背景图片大小

  - background-origin：指定了背景图像的位置区域

  - background-clip：从指定位置开始绘制

    ```
    background-clip: content-box /padding-box ；
    ```

    ​

- 渐变

  ```
  #grad {
    background: -webkit-linear-gradient(red, blue); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(red, blue); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(red, blue); /* Firefox 3.6 - 15 */
    background: linear-gradient(red, blue); /* 标准的语法 */ //注意，没有设置默认方向，则默认是从上到下
  }

  //从左到右的写法
  #grad {
    background: -webkit-linear-gradient(left, red , blue); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(right, red, blue); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(right, red, blue); /* Firefox 3.6 - 15 */
    background: linear-gradient(to right, red , blue); /* 标准的语法 */
  }

  对角线将right改成bottom right，如果想要定制角度，可以把right改成一个特殊的角度比如
  #grad {
    background: -webkit-linear-gradient(180deg, red, blue); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(180deg, red, blue); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(180deg, red, blue); /* Firefox 3.6 - 15 */
    background: linear-gradient(180deg, red, blue); /* 标准的语法 */
  }
  ```

  ​

- 文本效果

  - text-shadow，文本阴影
  - box-shadow，盒子阴影
  - text-overflow，属性指定应向用户如何显示溢出内容，可选值为clip（修建文本），ellipsis（显示省略号），string 使用给定的字符串代表修建的文本
    - 注意和overflow的区别
  - word-wrap，设定长单词是否可以换行，可选值：normal（默认，只在允许的断字点换行），break-word（在长单词或者URL地址内部进行换行）
  - word-break，设置浏览器默认的换行规则

- 字体，掠过不表

- 2D转换

  - transform 属性向元素应用 2D 或 3D 转换。

  - translate()方法，根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素位置移动。

  - rotate()方法，在一个给定度数顺时针旋转的元素。负值是允许的，这样是元素逆时针旋转。

  - scale()方法，该元素增加或减少的大小，取决于宽度（X轴）和高度（Y轴）的参数：

  - skew()包含两个参数值，分别表示X轴和Y轴倾斜的角度，如果第二个参数为空，则默认为0，参数为负表示向相反方向倾斜

    - skewX(<angle>);表示只在X轴(水平方向)倾斜。
    - skewY(<angle>);表示只在Y轴(垂直方向)倾斜。

  - matrix 方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。

  - ```
    div
    {
    transform: rotate(30deg); // 角度用deg表示
    transform: translate(50px,100px);
    transform: scale(2,3); /* 标准语法 */ //这个是倍数，X轴变成原来的2倍，Y轴成为原来的3倍
    }
    ```

    ​

- 3D转换

  - rotateX() 方法，元素围绕其 X 轴以给定的度数进行旋转。同理 rotateY()
  - 啊其实将2D转换定义到某个坐标，就可以实现3D转换

- 过渡

  - CSS3 过渡是元素从一种样式逐渐改变为另一种的效果。

    ```
    div
    {
    width:100px;
    height:100px;
    background:yellow;
    transition: width 2s, height 2s, transform 2s; // 这里定义了transition的时间是2S，如果是0S就看不到变化效果
    -moz-transition:width 2s; /* Firefox 4 */
    -webkit-transition:width 2s; /* Safari and Chrome */
    -o-transition:width 2s; /* Opera */
    }

    div:hover
    {
    width:300px;  //设置鼠标悬停显示的效果
    }
    ```

    ​

- 动画

  - ```
    @keyframes myfirst
    {
    	from {background: red;}
    	to {background: yellow;}
    }

    定义了关键帧，然后要捆绑到某个选择器

    div
    {
    	animation: myfirst 5s;
    }
    ```

    ​

- 多列，可以创建多个列对文本进行布局

  - column-count，定义元素应该被分隔的列数
  - column-gap，列之间的间隔
  - column-rule，设置列之间的宽度、样式和颜色规则

- 用户界面，没看懂啊

- 图片

  - border-radius，设置图片圆角
  - max-width 设置响应式尺寸的最大值
  - filter：滤镜

- 按钮

- 分页

  - http://www.runoob.com/css3/css3-pagination.html#

- 框大小

  - 总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距（哇，我居然一直记错了）
  - 但是如果设置了box-sizing: border-box，那么width和height就会包含pedding和border

- 弹性盒子（其实就是Flex）

  - 弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行

- 多媒体查询

  - viewport(视窗) 的宽度与高度
  - 设备的宽度与高度
  - 朝向 (智能手机横屏，竖屏) 。
  - 分辨率

- 多媒体查询实例