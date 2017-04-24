## 前端面试题  CSS
#### 1、介绍一下标准的盒子模型？低版本IE的盒子模型有什么不同？
    * 有两种：IE盒子模型、W3C盒子模型
    * 盒模型：内容content，填充padding，边界margin，边框border
    * 区别：IE的content把padding和border也包含了进去


#### 2、CSS选择符有哪些？哪些属性可以继承？
    - id选择器、类选择器、标签选择器、属性选择器、伪类选择器等等
    - 可继承的样式：font-size font-family color， UL LI DL DD DT；
    - 不可继承的样式：border padding margin width height

#### 3、CSS优先级如何计算？
    - 同权重情况下：内联样式表（标签内部） > 嵌入样式表（当前文件中） > 外部样式表（外部文件中）
    - 载入样式以最后载入的定位为准
    - !important > id > class > tag
    - important 比内联样式（标签内）优先级高

#### 4、CSS 3新增的伪类
举例：

    p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。

    p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
    
    p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
    
    p:only-child        选择属于其父元素的唯一子元素的每个 <p> 元素。
    
    p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。

    :after          在元素之前添加内容,也可以用来做清除浮动。
    :before         在元素之后添加内容
    :enabled        
    :disabled       控制表单控件的禁用状态。
    :checked        单选框或复选框被选中。

#### 5、如何居中div？

* auto只对水平居中有效，垂直方向设置auto无用
* 使用auto时，必须具体设定元素宽度，高度可以不具体指明
* 绝对定位的居中（absolute centering），利用margin和top、left的位移和，来进行居中，[详见abosolute centering](https://codemyviews.com/blog/how-to-center-anything-with-css)
* 图片垂直居中利用background-position属性，有center、top、bottom、left、right等可以选择，以下是垂直居中的设定：

        {
            position: absolute;
            background-position: center center
        }
* top, bottom, left, right 表示元素距离**父元素边界**的距离
- 水平居中：给div设置一个宽度，然后添加margin:0 auto属性


    div{
        width:200px;
        margin:0 auto;
        
    }

- 水平垂直居中一


    确定容器的宽高 宽500 高 300 的层
    设置层的外边距


    div {
        position: relative;     /* 相对定位或绝对定位均可 */
        width:500px;
        height:300px;
        top: 50%;
        left: 50%;
        margin: -150px 0 0 -250px;      /* 外边距为自身宽高的一半 */
        background-color: pink;     /* 方便看效果 */
        
    }
 
 
- 水平垂直居中二：[transform:translate](https://css-tricks.com/almanac/properties/t/transform/)

        translate（水平位移，垂直位移）//参数为百分比，
    跟margin的区别在于，**margin是父元素的百分比，而translate是元素自身的百分比**

        未知容器的宽高，利用 `transform` 属性
        
        transform 
        div {
          position: absolute;     /* 相对定位或绝对定位均可 */
          width:500px;
          height:300px;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          background-color: pink;     /* 方便看效果 */
        
        }

- 水平垂直居中三**--------改回查-----------**

    利用 flex 布局
    实际使用时应考虑兼容性

        .container {
        display: flex;
        align-items: center;        /*     垂直居中 */
        justify-content: center;    /* 水平居中 */
    
        }
    
        .container div {
          width: 100px;
          height: 100px;
         background-color: pink;     /* 方便看效果 */
        }  
        
        
#### 6、display有哪些值？有哪些作用？

    none          缺省值。象行内元素类型一样显示。
    
    - block, inline & inline-block
    block         块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
    inline        行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
    inline-block  默认宽度为内容宽度，可以设置宽高，同行显示。
    
    list-item     象块类型元素一样显示，并添加样式列表标记。
    table         此元素会作为块级表格来显示。
    inherit       规定应该从父元素继承 display 属性的值。

#### 7、position的值relative和absolute定位原点是？
- absolute    
    - 生成绝对定位元素，相对于**值不为static的第一个父元素**进行定位
    - 换言之，如果父元素是static，那就从节点树往上寻找第一个不为static的节点，把这个父节点当做原点来定位
- fixed (老版IE不支持)
    - 也是生成绝对定位的元素，**相对于浏览器窗口进行定位**，这是跟absolute的区别所在
    - **想相对于窗口进行绝对定位，用fixed；相对于父元素绝对定位，用absolute**
- relative
    - 生成相对定位的元素，相对于其正常位置进行定位
- static
    - **默认值**。没有定位，元素出现在正常流中
    - **忽略 z-index 声明**
    - **忽略 top, bottom, left, right 声明**
- inherit
    - 从**父元素继承**position的值

#### 8、CSS3 有哪些新特性？
    新增各种CSS选择器，比如   
        :not(.input){
            /* 选择不是input类的节点。not()的参数可以是标签名、类名、id等，非常多样化*/
        } 
        
    圆角      border-radius:8px;
    变形      transform：scale(), translate(), rotate()，skew（）
    线性渐变  gradient
    ...等等（有些就不从面试题上抄了）

#### 9、纯CSS创建三角形
- [详解画三角形原理](http://jingyan.baidu.com/article/08b6a591a3208914a809222f.html)
        

    
    把上、左、右三条边隐藏掉（颜色设为 transparent）
    #demo {
      width: 0;
     height: 0;
     border-width: 20px;
     border-style: solid;
     border-color: transparent transparent red transparent;
    }
    
#### 10、overflow有哪些属性？
    visible     默认值。超出部分直接显示
    hidden      超出部分隐藏
    
    scroll      超出部分隐藏，滚动显示（就算不超出，也显示滚动条）
    auto        超出部分隐藏，滚动显示（如果没有超出，则不显示滚动条）
    
    inherit     继承父元素的overflow
    
    























