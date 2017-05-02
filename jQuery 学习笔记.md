# jQuery 学习笔记
## 1、选择器
    //语法形式
    $('button').click(function(){
      //选择按钮  
    })
    

选择符 | 选择内容
---|---
'*' | 选择所有
'button' | 选择按钮
'#'      |   选择id
'.'    |     选择类
"a[target='_blank']"  |  选择target属性是_blank的a标签
'p:first' | 选择第一个p元素
"div,span,p.myClass" | 选择多个元素，用逗号隔开
---

## 2、事件

事件 | 描述 | 备注 
---|---|---
$(document).ready() | 文档加载完成 |   
.click() | 单击 | 左键  | 
.dblclick() | 双击 | 左键   
$(document).ready() | 鼠标按下 |    
.hover() | 鼠标移动到元素上 | | 
.mousedown() | 按下鼠标 | 左键  
.mouseup() | 松开鼠标 | 左键  

---

## 3、效果（感觉也算事件）

方法 | 效果 | 说明（都带时间和回调）  
---|---|---
$(selector).hide(speed,callback) | 隐藏 |不占用位置
show(speed,callback) | 显示 |
toggle(speed,callback) | 隐藏显示切换 |
$(selector).fadeIn(speed,callback); | 淡入 | 
fadeOut(speed,callback) | 淡出 | 
fadeToggle(speed,callback) | 淡入淡出切换 | 
fadeTo(speed, opacity, callback) | 透明度渐变 | opacity取值0~1
animate({params},speed,callback) | 动画 | 第一个参数是改变后的部分样式，第二个是速度
stop(stopAll, goToEnd) | 停止动画, 暂停或者快速完成  | 参数是boolean，stopAll暂时停止所有动画，goToEnd快速完成动作并停止
callback | 回调函数 | 以上函数都有回调，在事件执行完成后执行
链 | 连续执行多个动画 |$(selector).css("color","red").slideUp(2000).slideDown(2000);

---

### 4、操作dom

- ### 获取和修改
方法 | 描述 | 说明 
---|---|---
selector.text() | 获取文本内容 | 
.html() | 获取元素内部的html | 不包含selector本身的标签
.val() | 获取表单字段的值
selector.attr(attr, value) | 设置属性 | attr为属性名字符串，value为设置值

* text、html、val方法，括号内加字符串参数，可以设置文本内容
* 以上三个方法和attr方法，也可以加callback函数，写法是类似text(callback)的形式，回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。



```

```

- ### 添加元素
方法 | 描述 | 说明 
---|---|---
selector.append(node \| text) | 在被选元素结尾插入内容 | 可以是节点或者字符串（文本），可以一次插入多个，用逗号隔开
.prepend(node \| text) | 在被选元素开头插入内容 | 插头，其他类似append
.before(node \| text) | 在被选元素之前插入内容 | 在元素外部插，也能一次插入多个
.after(node \| text) | 在被选元素之后插入内容 | 在元素外部插
* after、before和append、prepend的区别在于，前者是在被选元素外插入节点，后者是在元素内部插入

```

```

- ### 删除元素
方法 | 描述 | 说明 
---|---|---
selector.remove() | 删除整个被选元素 | remove里面也可以加选择器过滤，不过感觉意义不大，毕竟selector已经可以选好了
.empty() | 删除所有子元素 | 不删除元素本身，留个外壳儿


```

```

- ### 修改样式
方法 | 描述 | 说明 
---|---|---
selector.css(propertyname) | 获取属性 | 
.css(propertyname, value) | 设置属性 |
.css(propertyname1:value1, property2:value2) | 设置多个属性 | 参数都是字符串
selector.addClass(classname) | 为元素添加类 | 
.removeClass(classname) | 删除类 |
.toggleClass(classname) | 增删切换 |


```

```

- ### 尺寸
![image](http://www.runoob.com/images/img_jquerydim.gif)
方法 | 描述 | 说明 
---|---|---
.width(value) | 设置或获取内容宽度 | 有参数即set，没有则get
.height(value) | 设置或获取高度 |
.innerWidth(value) | 设置或获取 内容+padding 宽度 | padding不变，只变内容
.innerHeight(value) | 设置或获取 内容+padding 高度 | 同上
.outerWidth(value) | 设置或获取 内容+padding+border 高度 | 参数为true，则获取加上margin的值；为值，则设置值
.outerHeight(value) | 设置或获取 内容+padding+border 高度 | 同上


---

### 5、遍历

- ### 父元素、祖先
方法 | 描述 | 说明 
---|---|---
selector.parent() | 获取直接父元素 | 
.parents() | 获取所有祖先元素，直到html | 获取值为node数组
.parentsUntil(node) | 向上获取祖先元素，直到node | 

```

```

- ### 后代
方法 | 描述 | 说明 
---|---|---
selector.children(selector（可选）) | 获取所有子元素 | 只往下走一级，不包含孙子元素。过滤为可选项，不填则获取所有
.find(selector) | 获取所有符合selector的子元素 | selector必须有，负责无效
.parentsUntil(node) | 向上获取祖先元素，直到node | 


```

```

- ### 同胞（sibling）
方法 | 描述 | 说明 
---|---|---
selector.siblings(selector（可选）) | 获取所有同胞 | 获取所有同胞元素。过滤为可选项，不填则获取所有
.next() | 获取下一个同胞元素 | 只获取一个元素
.nextAll(selector) | 获取所有之后的同胞元素 | selector可选，无则全选，有则筛选
.nextUntil(selector) | 获取介于当前元素和给定selector之间的同胞元素 | 
prev(), prevAll() & prevUntil() | 获取前面的元素 | 与上面的类似


```

```

- ### 过滤
方法 | 描述 | 说明 
---|---|---
selector.first() | 获取符合的第一个元素 | 
.last() | 获取符合的最后一个元素 | 
.eq(index) | 获取符合的第index个元素 | 
.filter(selector) | 获取符合的元素 | 这个方法没太大必要，直接在选择器里面选好就行了，重要的是not
.not(selector) | 获取非selector的元素 |


---


### 6、Ajax

### load(URL, data, callback)
* 功能：从服务器加载数据，并把返回的数据放入被选元素中
* 参数data是规定与请求一同发送的查询字符串的键值对
* 参数callback(responseTxt, statusTxt, xhr)
    - responseTxt为请求返回的内容
    - statusTxt值为“success”或者“error”，用于判断请求是否成功
    - xhr为请求对象本身

### $.get(URL, callback)
* 功能：即get请求，从服务器或者缓存获取数据
* callback(data, status)
   - data是返回的数据
   - status是请求的状态（success、error）

### $.post(URL, data, callback)
* 功能：即post请求，data是url中携带的参数值
* callback(data, status)
   - data是返回的数据
   - status是请求的状态（success、error）

---

### 7、其他
* noConflict()方法
    - 释放对$的控制。当有其他框架也在混合使用时，为避免兼容性问题，可以通过这个方法解除对$的控制。
    - 虽然解除了对$的控制，但依然可以使用jQuery，把$替换成jQuery即可，$('document')改成jQuery('document')
