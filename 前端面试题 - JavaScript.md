## 前端面试题 - JavaScript
#### 1、null undefined 区别？

    null        表示一个值为空的对象
    undefined   表示一个没有初始化的变量
    
    undefined不是一个有效的JSON，而null是；
    
    typeof undefined // "undefined"
    typeof null      // "object"
    
    验证null时，一定使用 === ：
    null == undefined //true
    
    null 转化为数值时，等于0；
    undefined 转化为数值，等于 NaN
    
#### 2、== 和 ===
    定义上说，
    ==，比较时会自动转换类型； 
    ===，比较变量值和类型，不会转型
    
    因为“==”存在太多不可预见的情况，推荐一律使用“===”
    
    比如，用“==”比较时，字符和数字会相互转化
    16 == ‘16’
    16 == '0x10';   //true
    '0x10' == 16;   //true
    '0x10' == '16';   //false


#### 3、闭包
    闭包是指有权访问另一个函数作用域变量中的函数，创建闭包最常见的方式就是：
    在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量
    突破作用域链，将函数内部的变量和方法传递到外部
    
    闭包的特性：
    1、函数内部嵌套函数
    2、内部函数可以引用外层的函数参数和变量
    3、参数和变量不会被垃圾回收机制回收
    
    
#### 4、创建对象的方式
    ①对象字面量
        person = {
            name: 'Ag'
        }
    
    ②工厂模式
        //在函数内new一个object对象o，往里面添加属性方法，返回o。没有用到new
        function createPerson(name){
            var o = new Object();
            o.name = name;
            return o;
        }
        
        var person1 = createPerson('Ag');
        
        缺点：没有解决对象识别的问题，instanceof 没法用
        

    ③用function模拟无参构造函数
        //定义一个空函数类。定义实例时，往对象里面加属性，跟构造函数类似
        function Person(){};
        
        var person = new Person();
        person.name = 'Ag';
        
        console.log(person.name); //Ag
        
    ④用function模拟有参构造函数模式
        //函数本身就是对象，函数名就是类名，用new创建实例，通过this给属性和方法赋值
        function Person(name){
            this.name = name;
        }

        var person1 = new Person('Ag');
        console.log(person1 instanceof Person); //true
        
        优点：解决了对象识别的问题，可以用instanceof
        缺点：各个实例分别独立，没有共享的属性和方法，容易造成资源浪费
    
    ⑤原型方式
        function Cat(){
            
        }
        Cat.prototype.species = '猫科动物';
        Cat.prototype.habit = function(){
            console.log(this.species + '爱抓老鼠');
        }
        var kitty = new Cat();
        kitty.habit();
    
    ⑥混合方式（原型 + 构造函数）
        function Cat(name, hobby){
            this.name = name;
            this.hobby = hobby;
        }

        Cat.prototype.sayHobby = function(){
            console.log(this.name + ' loves ' + this.hobby);
        }
        var coffeeCat = new Cat('Coffee', 'eating');
        coffeeCat.sayHobby(); //Coffee loves eating.
        
    各个方式比较：
    工厂模式：能实现批量创建对象，但是无法使用instanceof核对对象的类型
    构造函数模式：能实现批量创建对象，也可以使用instanceof核对对象类型
    但是没有共享的属性和方法，资源利用率低
    原型模式：有共享的属性和方法，避免重复造轮
    
    功能最强悍的还是“原型 + 构造函数”的混合方式
    如果访问一个对象的属性，先会访问对象私有的属性，如果私有的属性里没有，就会到原型的属性里边去找
    
    
    
    
    
    
#### 5、继承机制
    new（即 构造函数）创建对象的缺点：
    每一个实例对象，都有自己属性、方法的副本，无法做到数据共享，造成资源浪费
    
    因此prototype应运而生。
    每个函数包含一个prototype属性，这个属性包含一个prototype对象，
    所有实例对象需要共享的属性和方法，都放在这个对象里面；
    那些不需要共享的属性和方法，就放在构造函数里面
    
    实例对象一旦创建，将自动引用prototype对象的属性和方法。也就是说，实例对象的属性和方法，分成两种，一种是本地的，另一种是引用的
    
    继承方式
    
    1、借用构造函数
    
        子类的内部直接调用父类的构造函数，即‘借用’。直接复制父类的属性，
        子类、父类各有一套自己的完整的属性，且互不影响
        
        function SuperType(){
            this.colors = ['red', 'blue'];
        }
        function SubType(){
            //借用构造函数
            SuperType.call(this); //调用父类的构造函数，复制父类的属性
        }
        
        var instance1 = new SubType();
        instance1.colors.push('black');
        console.log(instance1.colors); //'red, blue, black'
    
        var instance2 = new SubType();
        console.log(instance2.colors); //'red, blue'
    
    
    2、组合继承（为最常用的继承方法，原型链 + 借用构造函数）
        function SuperType(name){
            this.name = name;
            this.colors = ['red', 'blue'];
        }
        SuperType.prototype.sayName = funciton(){
            alert(this.name);
        }
        function SubType(name, age){
            //继承属性
            SuperType.call(this, name); //借用构造函数
            this.age = age;
        }
        
        
    3、原型式继承
    
        * 特点：与原型模式类似，共享引用类型属性
        function object(o){
            function F(){}
            F.prototype = o;
            return new F();
        }
        var person = {name: 'Ag'};
        var anotherPerson = object(person); //创建person的子类anotherPerson
        
        ECMAScript5新增Object.create()方法，规范化原型式继承：
        
        //一个参数的形式，等效于object(person)
        var anotherPerson = Object.create(person); 
        
        //两个参数的形式，第一个参数同上，第二个类似于defineProperties方法
        var anotherPerson = Object.create(person, { 
                                name: {
                                    value: 'Ag'
                                }
                            });
        
        
    4、寄生式继承（封装继承过程，暗箱操作）
    
        *  创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真的是他做了所有工作一样返回对象
        function createAnother(original){
            //step 1: 创建一个新对象
            var clone = object(original);   
            //step 2：增强对象，添加clone对象的方法和属性。比如:
            clone.sayHi = function(){
                console.log('Hi!');
            }
            //step 3：返回这个对象
            return clone; 
        }
        var anotherPerson = createAnother(person);


    5、寄生组合式继承（引用类型最理想的继承范式）
    
        * 组合继承的缺点
        
            无论什么情况，都会调用两次超类型的构造函数：
            第一次是在创建子类型原型的时候，
                SubType.prototype = new SuperType(); //①原型方面，子类和超类共享一个原型 ②超类的实例属性，会被复制到 子类的原型 中
            第二次是在子类型构造函数内部，
                SuperType.call(this); //调用超类型的构造函数，超类型的实例属性，复制成子类的实例属性
                                      //此时，实例属性会屏蔽原型中的同名属性
            
            综上，共有两组同名属性，一组在实例上，一组在SubType原型中，这就是两次调用超类构造函数的结果
        
        * 寄生组合式继承思想：不必为了指定子类的原型，而调用超类的构造函数，我们需要的，就是超类原型的一个副本而已。
          本质上就是，设定一个中间对象，来获取超类型的原型，然后再将结果指定给子类型的原型
         
            function inheritPrototype(subType, superType){
                //step 1:创建对象，prototype相当于超类的一个“特殊实例”，他共享超类的原型，但不具有超类的实例属性和方法，这样就避免多余的复制。
                //可以理解为丢掉实例属性的prototype = new SuperType();
                var prototype = object(superType.prototype);    
                //step 2:增强对象, 目前constructor是SuperType，修正成SubType
                prototype.constructor = subType;
                //step 3:指定对象, prototype的原型就是超类的原型，类似于在原型上继承
                subType.prototype = prototype;
            }
            
          例：
            function SuperType(name){
                this.name = name;
                this.colors = ['red', 'blue'];
            }
            function SubType(name, age){
                SuperType.call(this, name); //构造函数继承
                this.age = age;
            }
            
            inheritPrototype(SubType, SuperType); //SubType
            
            
            
            
#### 6、JS基本数据类型
    Undefined  Null Boolean Number String
    ECMAScript 2015 新增：Symbol（创建后独一无二且不可变的数据类型）

#### 7、JS内置对象
    Object是所有对象的父对象
    
    基本包装类型：Object，Array，Number， Boolean和String
    其他对象：Function，Arguments，Math，Date，RegExp，Error等

#### 8、说一些JS基本规范
    1、不要在同一行声明多个变量
    2、 使用!==和===来比较true/false或者数值
    3、使用对象字面量替代new的形式
    4、不要使用全局函数
    5、switch必须带default分支
    6、for、if、while用大括号
    7、for、while在声明循环变量时要用var，避免作用域污染

#### 9、String有哪些方法？如何使用？
    var str = 'hello world';
    
    //输出位置0的字符、字符编码
    str.charAt(0)           //'h'，等效于str[[0]
    str.charCodeAt(0)       //'101',输出位置0的字符编码
    
    
    //字符串拼接
    str.concat(str1, str2)  //str1+str2, 一般不用这个方法，直接相加就行
    str.slice(3)
    
    
    //字符串截取
    slice(start, end), //包括str[start]，不包括str[end]
    substring(start, end), //包括str[start]，不包括str[end]
    substr(start, length)
    
    //其中，end、length非必需。省略end，则截取到最后一位字符。如果start也省略，则不作处理。
    //负数的情况,从尾部开始数，比如‘hello’，-1位置是‘o’
    str.slice(3);       //'lo world'
    str.slice(3, 7);    //'lo w'
    str.slice(-3);      //'ord'
    str.slice(3, -4);   //'lo w', 截取部分不包括-4位置的字符
    
    str.substring(3);   //'lo world'
    str.substring(3, 7); //'lo w'
    str.substring(-3);  //'hello world', substring会将负值转换为0
    str.substring(3, -4) //‘hel’，end为负，则直接截取前3个字符（前start个字符）
    
    str.substr(3);      //'lo world'
    str.substr(3, 7);   //'lo worl'，3起，长度为7的字符
    str.substr(3，-4);  //‘’，空字符串
    
    
    //字符串位置方法
    var str = 'hello world';
    
    str.indexOf('o');       //4，从前往后数，返回首个匹配位置
    str.indexOf('o', 6);    //7，从第六个字符(即str[5])，往后数
    lastIndexOf('o');       //7，从最后往前数，返回首个匹配位置
    lastIndexOf('o', 6);    //4, 从倒数第6的位置往前数
    
    
    //去掉字符串两侧空格 str.trim()
    var str = '  hello'
    console.log(str.trim());    //'hello'
    
    
    //大小写转换
    //toUpperCase(), toLowerCase()是两个经典方法
    //toLocaleUpperCase(), toLocaleLowerCase()则是针对特定地区的实现，涉及Unicode转换时，可能结果会与上面俩有一定区别
    var str1 = 'hello';
    var str2 = 'NTFS';
    str1.toUpperCase(str)    //'HELLO'
    str2.toLowerCase(str)    //'ntfs'
    
    
    //字符串的模式匹配方法 match()
    //返回匹配结果的数组（单个或多个，看查询标志i g m）
    //如果没有设定g（全局匹配），那么matches[0]为第一个匹配项,matches.index为该项在字符串中的位置
    //如果设定了g，那么matches会是一个匹配了多个元素的数组，就没有index属性了（即便只有一个匹配项，也不会有index）
    var text = 'cat, bat, sat, fat';
    var pattern = /.at/;
    //与pattern.exec(text)相同
    var matches = text.match(pattern);
    console.log(matches.index);     //0，第一个cat就匹配了，所以是第0位置的单个字符，index=0
    console.log(matches[0]);        //'cat'，第一个匹配项
    console.log(pattern.lastIndex); //0
    
    
    //另一个查找的方法 search()
    //返回字符串中第一个匹配的索引，如果RegExp中给出了g，会被忽略，依然只返回第一个匹配元素的位置。
    //如果没有找到，返回-1
    //注意，返回的是单个字符的位置
    var text = 'spider bat';
    var pos = text.search(/at/);
    console.log(pos); //8
    
    
    //字符串替换 str.replace（RegExp | 字符串, newstr)
    //第一个参数可以是RegExp或者字符串，如果是字符串，只替换第一个匹配项；如果是正则表达式，则具体看表达式需求咯
    var text = 'cat, bat';
    var result1 = text.replace('at', 'ond');    //'cond, bat'
    var result2 = text.replace(/at/g, 'ond');   //'cond, bond'
    
#### 10、RegExp类型
    var expression = /pattern /flags; 
    
    pattern部分可以是任何简单或者复杂的正则表达式，包含字符类、分组等；
    
    flags表示附加选项，
        g   全局模式，pattern将应用于所有字符串（若没有g，则只匹配首个字符串）
        i   不区分大小写模式，不区分大小写
        m   多行(multiline)模式，在到达一行末尾时，继续往下一行查找是否存在匹配字符串
    
    字符类
        .   [^\n\r]     除了回车和换行之外的任意字符
        \d  [0-9]       数字
        \D  [^0-9]      非数字
        \w  [a-zA-Z_0-9]    单词字符
        \W  [^a-zA-Z_0-9]   非单词字符
        \s  [ \t\n\x0B\f\r]     空白字符
        \S  [ ^\t\n\x\x0B\f\r]  非空白字符
        
    简单量词
        ?       0次或1次
        *       0次或多次
        +       1次或多次
        {n}     0次或n次
        {n, m}  n~m次，含n、m
        {n,}    大于等于n次
    
    分组
        小括号为 捕获组，先匹配整个字符串，然后开始匹配捕获组
        如下例子中出现两次mon and dad and baby，一次是匹配整个RegExp，一次是匹配捕获组的
        console.log("mon and dad and baby".match(/(mon( and dad (and baby)?)?)/)); 
        //mom and dad and baby, mon and dad and baby,  and dad and baby, and baby
        
    大括号表示重复次数，中括号表示范围内选择。小括号允许我们重复多个字符
        
    前瞻
        ?=exp   正向前瞻    匹配exp前面的位置
        ?!exp   负向前瞻    匹配后面不是exp的位置
        
        var str1 = "bedroom";
        var str2 = "bedding";
        var reBed = /(bed(?=room))///在我们捕获bed这个字符串时，抢先去看接下来的字符串是不是room
        alert(reBed.test(str1));//true
        alert(RegExp.$1)//bed
        alert(RegExp.$2 === "")//true
        alert(reBed.test(str2))//false
    
    边界
        ^   开头       注意不能紧跟于左中括号的后面
        $   结尾
        /b  单词边界    指[a-z] [A-Z] [0-9]之外的字符
        /B  非单词边界
        
        
    例：
        var pattern1 = /at/g;   //匹配字符串中所有at的实例
        var pattern2 = /[bc]at/i;   //匹配第一个'bat'或‘cat’，不区分大小写
        var pattern3 = /.at/gi;     //匹配所有以‘at’结尾的字符串的组合，不区分大小写
    
    实例属性：
        lastIndex，整数，表示开始搜索下一个匹配项的字符位置(相当于一个标记位，匹配过程中才会变化，一般不操作它)
        global，对应g；
        ignoreCase，对应i；
        multiline，对应m；
        source
    
    主要方法：
        exec()  
            接受一个参数，然后返回匹配的数组，与第9点的match()方法类似
            var text = 'cat, bat';
            var pattern = /.at/;
            var matches = text.exec(pattern);
            console.log(matches.index); //0
            console.log(matches[0]);    //cat
            console.log(pattern.lastIndex); //0;
        
        test()  
            匹配则返回true，否则返回false，常用于 验证用户输入
            var text = '000-00';
            var pattern = /\d{3}-\d{3}/; // \d指数字，{}指数字的个数，\d{3}即三个数字
            console.log(pattern.test(text)); //true
        
        toLocaleString(), toString()    
            获取正则表达式的字面量（结果为字符串）
            var pattern = /\d{3}-\d{3}/;
            console.log(pattern.toString()); // /\d{3}-\d{3}/
            console.log(pattern.toLocaleString()); // /\d{3}-\d{3}/
            
        valueOf()   
            获得正则表达式pattern本身(一个对象，区别于上面两个tostring，他们获取的是字符串)
            var pattern = /\d{3}-\d{3}/; 
            console.log(value instanceof RegExp);   //true
            console.log(pattern.valueOf());         // /\d{3}-\d{3}/
            
#### 11、如何将字符串转化为数字，例如'12.3b'?
    * parseFloat('12.3b'); //parseFloat是将字符串转化为数字，若字符串第一个字符是数字，则进行处理，转化到数字的末位（下一位是字母就截止，比如本例）
    * 一般来说可以用+str将字符串转化为数字，但是如果字符串里包含字母，则可能会影响转化
    比如12.3b，会转化成NaN
    
#### 12、谈谈对this对象的理解
    * this总是指向函数的直接调用者（而非间接调用者）
    * 如果有new关键字，this指向new出来的那个对象
    * 事件中，this指向触发这个事件的对象。特殊的是，IE中的attachEvent中this总是指向全局变量window
    
#### 13、函数表达式
    * 定义函数的两种方式
        - ①函数声明：
            function functionName(arg){}
        - ②函数表达式（匿名函数）： 
            var functionName = function(arg){}
            
        - 函数声明的函数有name属性，属性值为函数的名字。而匿名函数没有这个属性
        - 函数声明有提升，匿名函数无提升

#### 14、递归
    例如，阶乘函数：
        function factorial(num){
            if(num <= 1){
                return 1;
            }else{
                return num * factorial(num-1);
            }
        }
        
    表面看起来没什么问题，但下面的代码会导致它出错

        var anotherFactorial = factorial;
        factorial = null;
        
        console.log(anotherFactorial(4)); //报错，空指针，在return num * factorial(num-1)处
        
    因为在factorial内部，else部分用到了factorial，调用anotherFactorial时依然可能调用factorial，会指向空，报错
    
    这种问题，用arguments.callee可以解决，这是一个指向正在执行的函数的指针，因此可以通过它来实现对函数的递归调用：
    
        function factorial(num){
            if(num <= 1){
                return 1;
            }else{
                return num * arguments.callee(num-1);//将函数名解耦，避免更换函数名时带来的麻烦
          }
        }
    
    严格模式下，不能通过脚本访问arguments.callee, 可以使用明明函数表达式来达成相同的结果：
        var factorial = (function f(num){
            if(num <= 1){
                return 1;
            }else{
                return num * f(num-1);
            }
        });

#### 15、Array类型
    * 不同于Java, JS的数组大小可以动态调整
    
    * 两种创建方式
        - ① var colors = new Array();
        - ② var colors = new Array('red', 'blue');
        
        
    * 检测数组 Array.isArray(array) 
        if(Array.isArray(array)){
            //对数组执行某些操作
        }
        
        
    * 添加和删除
        - 设置数组length处的值，可以直接添加元素
        - 设置length，可以移除大于length的项
        
        - 栈方法（加删末尾） push & pop
            push可以接受任意数量的参数，把他们逐个添加到数组末尾，并返回修改后数组的长度
            pop从数组末尾删除一项，减少length的值，并返回删除项的值
            var colors = new Array();
            var count = colors.push('red', 'blue'); //推入两项
            console.log(count); //2

            var item = colors.pop(); //取得最后一项
            console.log(item); //'blue'
            console.log(colors.length); //1
        
        - 队列方法（添尾删头） push & shift(unshift)
            添尾：push，在数组尾巴上添加元素，同栈方法的push
            删头：shift，删除数组头部的元素，并返回这个元素值
            添头：unshift，在头部添加元素，可以添加多项，返回该元素值
            var colors = new Array();
            var count = colors.push('red', 'blue'); //推入两项
            console.log(count); //2

            var item = colors.shift(); //取得第一项
            console.log(item); //'red'
            console.log(colors.length); //1
            
            var count = colors.unshift('red', 'green'); //在头部添加两项
            console.log(count); //3
            
            
    * 转变为字符串
    
        toLocaleString()，toString()和valueOf()方法，默认情况下以逗号分隔的字符串的形式返回数组项
        join(str)方法，则可以用不同的分隔符参数str来构建这个字符串
        
        var colors = ['red', 'blue','green'];
        
        以下四种方式的输出结果都是一致的
        
        console.log(colors.toString()); //'red,blue,green'
        console.log(colors.toLocaleString()); //'red,blue,green'
        console.log(colors.valueOf()); //'red,blue,green'
        console.log(colors.join(',')); //'red,blue,green'
        
        其中join方法，可以使用不同的字符分隔字符串：
        console.log(colors.join('||'); //'red||blue||green'
        
        
    * 重排序
        - 反转 reverse()
        
        - 升序排列 sort()
        
            原生的升序排列有个问题，就是sort函数是按位比较字符串的，比如'10'会比'5'小，因为'1'比'5'小
            因此，sort可以有入参，参数是一个比较函数，以便我们能指定哪个值在前
            比较函数接受两个参数，第一个参数在第二个前面，返回一个负数；第一个在第二个后面，返回一个正数；相等，返回0
            
            //升序排列
            //降序则对调一下return的值即可
            function compare(value1, value2){
                if(value1 < value2){
                    return -1; //value1小，return负值，value1在前
                }else if (value1 > value2){
                    return 1;
                }else{
                    return 0;
                }
            }
            
            最简单的compare函数：
            function compare(value1, value2){
                return value1 - value2; //value1大，return正值，value1在后
                                        //大在后，为降序排列
            }
            
            
    * 操作方法
        - 合并两个数组 concat
            array1.concat(array2);
            也可以用push实现
            
        - 截取数组的一部分 slice
            var nums = [1, 2, 3, 4, 5];
            var nums2 = nums.slice(1); //[2,3,4,5], nums[1]-末尾的元素
            var nums2 = nums.slice(1, 4); //[2,3,4], nums[1]-nums[3]的元素,元素数为4-1 = 3
    
    
    * 位置方法
        indexOf 从前往后找
        lastIndexOf 从后往前找
    
    
    * 迭代方法
        对数组的每一项执行一个函数，这个函数接受三个参数：数组项的值、该项在数组中的位置和数组对象本身
        
        五种迭代方法：
        
        ①every()    如果该数组对每一项都返回true，则返回true
        ②filter()   返回true的项组成的数组
        ③forEach()  这个方法没有返回值
        ④map()      返回每次调用的结果组成的数组
        ⑤some()     如果函数对任一项返回true，则返回true
        
        注意，以上方法都不会修改数组本身的值
        
        var numbers = [1,2,3,4,5,4,3,2,1];

        var everyRes = numbers.every(function(item, index, array){
            return (item > 2);
        });
        
        var someRes = numbers.some(function(item, index, array){
            return (item > 2);
        });
        
        var filterRes = numbers.filter(function(item, index, array){
           return (item > 2);
        });
        
        var mapRes = numbers.map(function(item, index, array){
            return (item > 2);
        });
        
        numbers.forEach(function(item, index, array){
           //执行某些操作
        });
        
        console.log(everyRes); //false
        console.log(someRes); //true
        console.log(filterRes); //[3,4,5,4,3]
        console.log(mapRes); //[false, false, true, true, true, true, true, false, false]

    * 归并方法（可用于求和）
        reduce(function(prev, cur, index, array))       从前往后遍历
        reduceRight(function(prev, cur, index, array))  从后往前遍历
        
        函数的四个参数：前一个值，当前值，项的索引，数组对象。
        函数返回值会作为前一个值传递给下一项
        第一次迭代，发生在数组的第二项上，此时prev为数组的第一项，index为数组的第二项
        
        var values = [1,2,3,4,5];
        var sum = values.reduce(function(prev, cur, index, array){
            return prev + cur; //求和,1+2+3+4+5
        })
        console.log(sum); //15
        
        同理，用reduceRight求和，5+4+3+2+1, 也会获得相同的结果
        
        
#### 16、JSON
    JSON(JavaScript Object Notation)是一种轻量级的数据交换格式
    它是基于JS的一个子集。数据格式简单，易于读写，占用带宽小
    如{"name":"Ag", "age":"24"}
    
    JSON字符串转JSON对象的三种方法：
    var obj = eval('(' + str + ')');
    var obj = JSON.parse(str); //可以接收一个还原函数作为参数
    var obj = str.parseJSON(); //主要用前两个吧，这个方法有点问题，提示方法undefined

    
    JSON对象转为JSON字符串：
    var last = obj.toJSONString();
    var last = JSON.stringify(obj); //高程P566,可以加过滤器、缩进字符的参数
    
    JSON 和 对象字面量 的区别
    1、JSON没有声明变量，也不用加分号
    2、对象字面量的属性名称，可加双引号也可以不加
       JSON的属性名称，必须加双引号（单引号或者不加引号都不可行）
    
#### 17、事件
    1、事件流
    
        即描述从页面接收事件的顺序
        - 事件冒泡：由最具体的元素接收，逐级向上传播到document
        - 事件捕获：由document开始接收，逐级传播到事件的实际目标。
        
        老版本浏览器不支持捕获，所以不常用。建议尽量使用事件冒泡，有特殊需要时再用捕获
        
        DOM2规定事件流包括三个阶段：捕获 → 目标 → 冒泡
    
    
    2、事件处理程序
    
        - HTML事件处理程序  
            在标签中定义onclick等属性，属性值如果包含特殊符号（比如双引号），需要转义
            缺点：
            ①时差问题，可能触发事件的时候函数没准备好，造成错误
            ②扩展事件处理程序的作用域链在不同浏览器中会导致不同结果
            ③HTML和JS紧密耦合，如果要更换事件处理程序，就要改动HTML和JS两个地方。
            这也是很多人摒弃HTML事件处理程序，转而使用JS指定事件处理程序的原因所在
            
        - DOM0级事件处理程序
            JS中指定onclick对应的函数
            
        - DOM2级事件处理程序
            addEventListener、removeEventListener
    
    
    3、事件对象 event
    
        兼容DOM的浏览器会将一个event对象传入到事件处理程序中，无论事件处理程序是DOM0级还是DOM2级
        
        event的常用属性和方法
        
        ① 属性
        - currentTarget     listener、onclick指定的元素
        - target            事件直接触发的目标
        - eventPhase        确定事件正位于事件流的哪个阶段（捕获、目标、冒泡）
        ② 方法
        - preventDefault    阻止特定行为，比如点击链接不弹出或跳转link
        - stopPropagation   立即停止事件在dom中的传播，取消进一步的事件捕获或冒泡
        * 事件处理程序中的this值，等同于currentTarget
    
    4、事件类型
    
        ① UI事件
          指不一定与用户操作有关的事件，这些事件在dom规范出现以前，都是以这种形式存在的，而在dom规范中保留是为了向后兼容
        - load      页面加载完成后触发。可以通过onload属性、JS定义
        - unload    页面被卸载；页面跳转。避免调用页面元素，此时可能已经有元素不存在了
        - resize    窗口大小变化
        - scroll    文档滚动期间触发
        
        ② 焦点事件
          在元素获得或失去焦点时触发
        - focus         获得焦点。不冒泡
        - DOMFocusIn    获得焦点。冒泡
        - focusin（DOM3) 获得焦点。冒泡
        - blur          失去焦点。不冒泡
        - DOMFocusOut   失去焦点。冒泡
        - focusout      失去焦点。冒泡
        
        ③ 鼠标与滚轮事件
        - click     单击主鼠标按钮/按下回车键
        - dblclick  双击主鼠标按钮
        - mousedown 按下任意鼠标按钮
        - mouseup   释放鼠标按钮
        - mouseout  鼠标从元素上方移走
        - mouseover 鼠标移到元素上方  
        
        ④ 键盘事件
        
        ⑤ 复合事件
        
        ⑥ 变动事件
          DOM中某一部分变化的时候触发
        - DOMSubtreeModified    DOM中发生任何变化即触发。这个事件在其他任何变动事件触发后都会触发
        - DOMNodeInserted       插入子节点
        - DOMNodeRemoved        删除子节点
        - DOMAttrModified       特性被修改
        - DOMCharacterDataModified  文本节点的值变化
        
        - DOMNodeInsertedIntoDocument   在新插入的节点上触发，不冒泡
        - DOMNodeRemovedFromDocument    在移除节点和其子节点上触发，不冒泡，指定给其中一个子节点的事件处理程序才会调用
        
        ⑦ HTML5事件
        - contextmenu   显示上下文菜单
        - beforeunload  关闭页面时的提示框
        - DOMContentLoaded  DOM树加载完成后触发，比load快，尽早与用户交互, 用于document或weindow
          （load会在页面的DOM/script/css/img等全部加载完成后触发，DOMContentLoaded只需要在DOM树加载完成即可触发，更快）
        - pageshow/pagehide  页面显示或卸载之前触发。这两个事件都有一个叫做persisted的属性，来标记页面加载、卸载时是否被保存在了bfcache里，如果有，那再次打开不会触发onload事件
          （bfcache 是往返缓存，保存页面数据和DOM、JS状态，使浏览器的“后退”“前进”更快。不过指定了onunload处理的页面不会被加入bfcache，onunload最常用于撤销在onload中执行的操作，而跳过onload后再次显示页面可能出现不正常）
        - hashchange    URL参数列表（及'#'后所有字符串）变化
        
    
    5、内存和性能      
        ① 事件委托 
          - 只需在DOM树中尽量高的层次添加事件处理程序
        ② 移除事件处理程序
          - 可能造成空事件处理程序的情况：
            a、从文档中移除带有事件处理程序的元素。使用innerHTML移除元素时，原有事件处理程序并没有被回收。在执行innerHTML前，先移除事件处理程序
            b、卸载页面时没有处理干净事件处理程序，那会残留在内存中。最好的做法是在页面卸载之前，先通过onunload事件处理程序移除所有事件处理程序
              在此，事件委托技术再次表现出它的优势——需要跟踪的事件处理程序越少，移除就越容易
              
    
    6、模拟事件      
          
#### 18、Ajax是什么？如何创建？
    ajax全称：Asynchronous JavaScript And XML
    （异步传输 + JS + XML）
    
    技术核心是XMLHttpRequest对象（又名XHR）
    
    注意：
    虽然名字中包含有XML，但Ajax通信与数据格式无关；
    这种技术就是无需刷新页面即可从服务器取得数据，但不一定是XML格式
    
    1、何为异步？
    
        所谓异步，即向服务器发送请求的时候，我们不必等待结果，而是可以做其他事情
        等到有结果了，它就会根据设定进行后续操作，与此同时，页面不会发生整页刷新，提高了用户体验。
    
    
    2、属性
        status          响应的HTTP状态
        statusText      HTTP状态的说明
        responseText    如果响应的内容是'text/xml'或'application/xml',这个属性中将保存包含响应数据的XML DOM文档
        
        readyState      请求/响应过程的当前活动阶段
        可取的值如下：
            0：未初始化。还没调用open方法
            1：启动。已经调用open，还没send
            2：发送。已经调用send，还没收到响应
            3：接收。已经接收到部分响应数据。
            4：完成。接收到全部响应数据。
        每当readyState的值有变化，就会触发一次readystatechange事件。通常我们只对readystate值为4感兴趣，因为此时数据已经准备就绪
        需在open之前指定onreadystatechange事件处理程序才能确保跨浏览器的兼容性
    
    
    3、原生方法
    
        open()  启动请求
        send()  发送请求
        abort() 收到响应前取消请求（取消后依然可以对xhr进行操作。由于内存原因，不建议重用xhr）
    
    
    4、进度事件 P580 
        loadstart → progress → error, abort, load → loadend
        
    
    5、Ajax流程：
    
    （1）创建XMLHttpRequest对象，也就是创建一个异步调用对象
        var xhr = new XMLHttpRequest(); //早期IE不可用，其方法见高程P572
        
    （2) 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL以及验证信息
        xhr.open('get', 'example.txt', false);
        
    （3）设置响应HTTP请求状态变化的函数(readystatechange事件)
        xhr.onreadystatechange = function(){
            if(xhr.readystate == 4){
                if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                    console.log(xhr.responseText);
                }else{
                    console.log('Request was unsuccessful: ' + xhr.status);
                }
            }
        }
        
    （4）发送HTTP请求
        xhr.send(null); 
        
    （5）获取异步调用返回的数据
    （6）使用JavaScript和DOM实现局部刷新
    
#### 19、跨域资源共享
    ajax通信的一个主要限制，就是跨域安全策略。默认情况下ajax只能访问当前域的资源，这种策略可以预防某些恶意行为。
    当然随着技术进步，非IE的原生XHR已经支持跨域了
    
    解决方式有CORS、jsonp、iframe、window.name、window.postMessage、服务器上设置代理页面等
    
    1、CORS（Cross-Origin Resource Sharing，跨域资源共享）
    
        CORS定义了浏览器跨域访问资源的规则。
        CORS的基本思想，是使用自定义的HTTP头部让服务器、浏览器进行沟通，从而决定响应的成败。
        
        发送HTTP请求时，CORS会添加一个Origin头部，其中包含请求页面的源信息（协议、域名和端口），如：
            Origin: http://www.nczonline.net
        
        如果服务器认为这个请求可以接受，就在Access-Control-Allow-Origin头部中回发相同的源信息（公共资源可以回发‘*’），如：
            Access-Control-Allow-Origin: http://www.nczonline.net
        
        驳回和限制：
        - 驳回：如果没有这个头部或者头部的源信息不匹配，浏览器就会驳回请求
        - 限制：请求和响应都不会包含cookie信息
        
        下面说一下CORS的实现
        ①IE：引入了XDR，与XHR类似，但可跨域。只支持异步，只支持get、post请求
        ②其他浏览器：XHR实现了对CORS的原生支持，厉害了。无需额外写代码。只是有一些限制：
            - 不能使用setRequestHeader()设置自定义头部
            - 调用getAllRequestHeaders()方法总会返回空字符串
            - 不能收发cookie（同CORS通用的限制）
    
    
    2、JSONP（JSON with padding，又名填充式JSON 或 参数式JSON）
    
        JSONP由两部分组成：回调函数和数据。
        回调函数一般在请求中指定，如下指定handleResponse()为回调函数：
        http://freegeoip.net/json/?callback=handleResponse
        
        使用方法：通过动态script元素来使用，使用时可以为src指定一个跨域url
            var script = document.createElement('script');
            script.src = 'http://freegeoip.net/json/?callback=handleResponse';
            document.body.insertBefore(script, document.body.firstchild);
        
        两点不足：
        1、安全问题：如果其他域不安全，很可能会在相应中夹带啊一些恶意代码，此时除了完全放弃JSONP调用之外，没有办法追究
        2、要确定请求失败不容易。虽然h5新增了一个onerror事件处理程序，但目前还没有得到浏览器支持。使用计时器检测也难尽人意，毕竟用户网速不一
    
    
    3、图像ping
    
        在JS中动态创建图像，指定跨域的src。响应可以是任何内容，通常是像素图或204
        常用于跟踪用户点击页面和统计动态广告曝光次数
        
        缺点：
        ① 只能发送get请求
        ② 无法访问服务器的响应文本
        
        因而图像ping只能用于单向通信。
        
        
    4、Comet 又称服务器推送
    
        comet是一种服务器向页面推送数据的技术，能够让信息几乎实时推送到页面，适合体育赛事直播和股票报价
        
        实现方式：长轮询 和 流。
        
        引申一下，说几个概念
        长轮询：浏览器向服务器发请求，浏览器收到请求后开始等待，直到有新数据才返回响应，关闭请求。然后浏览器再发一个新请求。
        短轮询：浏览器周期性发送请求。服务器一收到请求就返回响应，无论数据有没有更新，跟长轮询不同的地方在于有周期、没有等待的时间
        HTTP流：整个生命周期只使用一个HTTP连接。浏览器发送请求后，服务器连接一直把持打开，周期性向浏览器发送数据。通过侦听readystatechange和检测readyState,就可以利用XHR实现流。
    
    
    5、SSE 服务器发送事件
    
        属于comet的一种方式。创建到服务器的单向连接，服务器通过这个连接可以发送任意数量的数据，适用直播赛事
    
    
    6、Web Sockets（使用web socket协议)
    
        在单独的持久连接上进行全双工、双向通信。
        
        创建过程：
        - 浏览器发送http请求创建http连接，取得响应后，连接会升级成web socket连接（使用websocket协议，不再是http）
        
        优点：
        - 能够发送非常少量的数据，不用担心流量开销，非常适合移动应用。毕竟对于移动应用而言，网络延迟和带宽都是问题
        
        限制：
        - 现有服务器不一定支持websocket通信。在不能选择websocket的情况下，可以使用XHR和SSE的组合替代
    
    
    安全问题
        为防备CSRF攻击，需要验证发送请求者是否有权访问相应资源。有以下几种方式可以选择
        - 要求以SSL连接来访问可以通过XHR访问的资源
        - 每次请求都附带经过相应算法计算得到的验证码
        
        请注意，下面的防护措施无效：
        - 要求发送post请求而不是get ——很容易改变
        - 检查来源URL是否可信       ——容易伪造
        - 检查cookie进行验证        ——也很容易伪造
        
        XHR的open方法其实还可以附带用户名和密码的参数，但千万别这样做。
        把用户名和密码保存在JS中是极不安全的，因为任何会使用JS调试器的人，都可以看到相应的文本形式的密码
    
    
#### 20、？    
    
    
        
        









