# 前端相关面试题

1. JS的内置对象
    * Object 是Javascript中所有的对象的父对象
    * 数据封装对象：Object、Array、Boolean、Number、String
    * 其他对象（FAMDRE）：Function、Argument、Math、Date、RegExp、Error

1. 介绍一下javascript原型，原型链，他们有何特点
    * 每个对象都会在内部初始化一个属性，就是prototype（原型），当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。
    * 特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本，当我们修改原型时，与之相关的对象也会继承这一改变。当我们需要一个属性时，JavaScript引擎会先看当前对象中是否有这个属性，如果没有的话，就会查找它的prototype对象是否有这个属性，如此递推下去，一致检索到Object内建对象。

1. javascript有几种类型的值？画下他们的内存图。
    * 栈：原始数据类型（Udefined,Null,Boolean,Number,String）
    * 堆：引用数据类型（对象，数组，函数）。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

1. javascript如何实现继承？
    * 构造继承、原型继承、实例继承、拷贝继承

    ```javascript
    //原型prototype机制或apply和call方法去实现较简单，建议使用构造函数与原型混合方式。
    function Parent() {
        this.name = 'song';
    }
    function Child() {
        this.age = 28;
    }
    Child.prototype = new Parent(); //通过原型,继承了Parent
    var demo = new Child();
    alert(demo.age);
    alert(demo.name); //得到被继承的属性
    ```

1. javascript有哪几种创建对象的方式？

    ```javascript
    //javascript创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用JSON；但写法有很多种，也能混合使用。
    (1)对象字面量的方式
    person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
    (2)用function来模拟无参的构造函数
    function Person(){}
    var person = new Person(); //定义一个function，如果使用new"实例化",该function可以看作是一个Class
        person.name = "Xiaosong";
        person.age = "23";
        person.work = function() {
        alert("Hello " + person.name);
    }
    person.work();
    (3)用function来模拟参构造函数来实现（用this关键字定义构造的上下文属性）
    function Person(name,age,hobby) {
        this.name = name; //this作用域：当前对象
        this.age = age;
        this.work = work;
        this.info = function() {
            alert("我叫" + this.name + "，今年" + this.age + "岁，是个" + this.work);
        }
    }
    var Xiaosong = new Person("WooKong",23,"程序猿"); //实例化、创建对象
    Xiaosong.info(); //调用info()方法
    (4)用工厂方式来创建（内置对象）
    var jsCreater = new Object();
    jsCreater.name = "Brendan Eich"; //JavaScript的发明者
    jsCreater.work = "JavaScript";
    jsCreater.info = function() {
    alert("我是"+this.work+"的发明者"+this.name);
    }
    jsCreater.info();
    (5)用原型方式来创建
    function Standard(){}
    Standard.prototype.name = "ECMAScript";
    Standard.prototype.event = function() {
        alert(this.name+"是脚本语言标准规范");
    }
    var jiaoben = new Standard();
    jiaoben.event();
    (6)用混合方式来创建
    function iPhone(name,event) {
        this.name = name;
        this.event = event;
    }
    iPhone.prototype.sell = function() {
        alert("我是"+this.name+"，我是iPhone5s的"+this.event+"~ haha!");
    }
    var SE = new iPhone("iPhone SE","官方翻新机");
    SE.sell();
    ```

1. 什么是闭包，为什么要用它？
    * 闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。
    * 特性：
        * 函数类再嵌套函数
        * 内部函数可以引用外层的参数与变量
        * 参数与变量不会被垃圾回收机制回收

1. new操作符具体干了什么？
    1. 创建一个空对象，并且this变量引用该对象，同时还集成了该函数的原型
    1. 属性和方法被加入到this引用的对象中
    1. 新创建的对象由this所引用，并且最后隐式的返回this

1. 如何创建一个ajax？(总共有8种callback , `onSuccess` , `onFailure` , `onUninitialized` , `onLoading` , `onLoaded` , `onInteractive` , `onComplete` , `onException`)
    1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象
    1. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
    1. 设置响应HTTP请求状态变化的函数
    1. 发送HTTP请求
    1. 获取异步调用返回的数据
    1. 使用JavaScript和DOM实现局部刷新

    ```javascript
    // 1.获得ajax
    if (window.XMLHttpRequest) { //查看当前浏览器XMLHttpRequest是否是全局变量
        var oAjax = new XMLHttpResquest();
    } else {
        var oAjax = new ActiveXObject('Microsoft.XMLHTTP'); //IE6,传入微软参数
    }

    // 2.打开地址
    switch (json.type.toLowerCase()) {
        case 'get':
            oAjax.open('GET', json.url + '?' + jsonToURL(json.data), true); // 提交方式(大写)，url，是否异步
            oAjax.send(); // 3.发送数据
            break;
        case 'post':
            oAjax.open('POST', json.url, true);
            oAjax.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
            oAjax.send(jsonToURL(json.data)); // 3.发送数据
            break;
    }

    // 4.接收数据
    oAjax.onreadystatechange = function() { //监控状态
        if (oAjax.readyState == 4) {
            json.complete && json.complete();
            if (oAjax.status >= 200 && oAjax.status < 300 ||
                oAjax.status == 304) {
                json.success && json.success(oAjax.responseText); //执行成功的回调函数, responseText为响应内容
            } else {
                json.error && json.error(oAjax.status); //执行失败的回调函数
            }
        }
    };
    ```

1. ES6怎么写class，为何会出现class？
    * ES6的class可以看作是一个语法糖，他的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰·更加面向对象编程的语法而已

    ```javascript
    class Point {
        constructor(x,y) {
            //构造方法
            this.x = x; //this关键字代表实例对象
            this.y = y;
        } toString() {
            return '(' + this.x + ',' + this.y + ')';
        }
    }
    ```

1. 异步加载JS的方式有哪些？
    1. defer，只支持 IE
    1. async
    1. 创建 script，插入到 DOM 中，加载完毕后 callBack

    ```javascript
    function loadScript(url, callback){
        var script = document.createElement("script")
        script.type = "text/javascript";
        if(script.readyState){ //IE
            script.onreadystatechange = function(){
                if (script.readyState == "loaded" ||script.readyState == "complete"){
                        script.onreadystatechange = null;
                        callback();
                }
            }
        } else {
        //Others: Firefox, Safari, Chrome, and Opera
            script.onload = function(){
                callback();
            };
        }
        script.src = url;
        document.body.appendChild(script);
    }
    ```

1. DOM操作——怎么添加、移除、移动、复制、创建和查找结点？
    * 创建新节点
        * createDocumentFragment() //创建一个DOM片段
        * createElement() //创建一个具体的元素
        * createTextNode() //创建一个文本节点
    * 添加、移除、替换、插入
        * appendChild()
        * removeChild()
        * replaceChild()
        * insertBefore() //在已有的子节点前插入一个新的子节点
    * 查找
        * getElementsByTagName() //通过标签名称
        * getElementsByName() //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
        * getElementById() //通过元素Id，唯一性

1. 哪些操作会造成内存泄漏
    * 内存泄漏是指任何对象在您不再拥有或需要它之后依然存在
    * 例如：setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏

1. jQuery中如何将数组转化为json字符串，然后再转化回来
    ```javascript
    //jQuery 中没有提供这个功能，所以需要先编写两个 jQuery 的扩展：
    $.fn.stringifyArray = function(array) {
        return JSON.stringify(array)
    }
    $.fn.parseArray = function(array) {
        return JSON.parse(array)
    }
    //然后调用:
    $("").stringifyArray(array)
    ```

1. jQuery.extend 与 jQuery.fn.extend 有何区别？

    ```javascript
    jQuery.extend(object);　//为jQuery类添加类方法，可以理解为添加静态方法
    jQuery.extend({
        min: function(a, b) { return a < b ? a : b; },
        max: function(a, b) { return a > b ? a : b; }
    });
    jQuery.min(2,3); //  2
    jQuery.max(4,5); //  5
    jQuery.extend( target, object1, [objectN])//用一个或多个其他对象来扩展一个对象，返回被扩展的对象
    jQuery.fn.extend(object); //对jQuery.prototype进行的扩展，就是为jQuery类添加“成员函数”。jQuery类的实例可以使用这个“成员函数”。
    //比如我们要开发一个插件，做一个特殊的编辑框，当它被点击时，便alert 当前编辑框里的内容。可以这么做：
    $.fn.extend({
        alertWhileClick:function() {
            $(this).click(function(){
                    alert($(this).val());
            });
        }
    });
    $("#input1").alertWhileClick(); // 页面上为$("#input1")为一个jQuery实例，当它调用成员方法 alertWhileClick后，便实现了扩展，每次被点击时它会先弹出目前编辑里的内容。
    ```

1. 如何判断当前脚本运行在浏览器还是node环境中？
    * 通过判断 Global 对象是否为 window ，如果不为 window ，当前脚本没有运行在浏览器中

1. 检测浏览器版本有哪些方式？
    * 功能检测、userAgent 特征检测
    * 不同浏览器的特性，如addEventListener
        * 比如：navigator.userAgent
            * `//"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36"`

1. Canvas和SVG的比较

    | Canvas | SVG |
    |:---- |:---- |
    |位图技术，可以保存为.png | 矢量图技术，不能保存为位图 |
    |善于表现颜色和线条细节 | 可以缩放，不善于表现细节 |
    |网页游戏，统计图|图标，统计图，地图|
    |一个标签(canvas)+一个对象(getcontext)所有图形图像都靠ctx绘制 | 几十个标签---每个图形对应一个标签 |
    |不能被搜索引擎爬虫所访问|可以|
    |只能为整个 Canvas绑定监听函数|每个图形（标签）可以绑定事件监听函数|

1. 谈谈你对 JavaScript 中的模块规范 CommonJS、AMD、CMD 的了解？

    | CommonJS |   AMD    | CMD     |
    |:----------|:----------|:---------|
    |Node.js|RequireJS|SeaJS|

1. 前端MVC，MVVM;
    * MVC----模型(Model):数据保存; 视图(View):用户界面;控制器(Controller):业务逻辑;
        1. `View` 传送指令到 `Controller`
        1. `Controller` 完成业务逻辑后，要求 `Model` 改变状态
        1. `Model` 将新的数据发送到 `View` ，用户得到反馈所有通信都是单向的。
    * MVVM----模型(Model)，视图(View)，视图模型(ViewModel)
        1. 各部分间都是双向通信
        1. `View` 与 `Model` 不发生联系，都通过 `ViewModel` 传递
        1. `View` 非常薄，不部署任何业务逻辑，称为“被动视图”（Passive View），即没有任何主动性；而 `ViewModel` 非常厚，所有逻辑都部署在那里。采用双向绑定(`data-binding`):`View` 的变动，自动反映在 `ViewModel` ，反之亦然。

1. 如何阻止事件的冒泡?

    ```javascript
    //阻止冒泡的方法
    function stopPP(e)
    {
        var evt = e|| window.event;
        //IE用cancelBubble=true来阻止而FF下需要用stopPropagation方法
        evt.stopPropagation ? evt.stopPropagation() : (evt.cancelBubble=true);
    }
    ```

1. JavaScript中如何对一个对象进行深度clone?

    ```javascript
    function cloneObject(o) {
        if(!o || 'object' !== typeof o) {
            return o;
        }
        var c = 'function' === typeof o.pop ? [] : {};
        var p, v;
        for(p in o) {
            if(o.hasOwnProperty(p)) {
                v = o[p];
                if(v && 'object' === typeof v) {
                    c[p] = Ext.ux.clone(v);
                }
                else {
                    c[p] = v;
                }
            }
        }
        return c;
    };
    ```

1. 如何将arguments转为数组?

    `Object.prototype.slice.call(arguments);`

1. 如何获取光标的水平位置?

    ```JavaScript
    function getX(e){
        e = e || window.event;
        //先检查非IE浏览器，再检查IE的位置
        return e.pageX || e.clentX + document.body.scrollLeft;
    }
    ```
1. 函数柯里化

    ```javascript
    var getN;
    function add(n){
        getN=function(){console.log(n);}
        return function(m){
            n+=m;
            arguments.callee.toString=function(){
                return n;
            }
            return arguments.callee;
        }
    }
    add(1)(2)(3); getN();//6
    add(1)(2)(3)(4); getN();//10
    alert(add(1)(2)(3));//6
    alert(add(1)(2)(3)(4));//10
    ```
1. 递归

    ```javascript
    var emp={
        work:function(){//3,2,1
                var  sum=0;//+3+2+1 +2+1  +1
                for(vari=0; i<arguments.length&&arguments[0]>0;i++){
                    sum+=arguments[i]
                        +arguments.callee(
                            --arguments[i]
                        );
                }
            return sum;
            }
        }
    console.log(emp.work(3,2,1));//10
    ```

1. OOP--面向对象编程(object-oriented programming)

    (1)
    ```javascript
    window.a=300;
    function fn1(){
        this.a=100;
        this.b=200;
        return function(){
                alert(this.a)
                }.call(arguments[0])
    }
    function fn2(){ this.a=new fn1(); }
    var a=new fn1().b;//300
    var v=new fn1(fn2());//[object Object]
    ```

    (2)
    ```javascript
    var number=2;//4  8
    var obj={
        number:4,//8
        fn1:(function(){
        //var number;
        this.number*=2;
        number*=2; //声明提前  undefined
        var number=3;
        return function(){
            this.number*=2;
            number*=3;
            alert(number);
        }
        })()
    }
    var fn1=obj.fn1;
    alert(number); fn1(); obj.fn1();
    //4           9      27
    alert(window.number);//8
    alert(obj.number);//8
    ```

    (3)
    ```javascript
    function Foo(){
    getName=function(){alert(1);};
        return this;
    }
    Foo.getName=function(){alert(2);};
    Foo.prototype.getName=function(){
        alert(3);
    };
    var getName=function(){alert(4);};
    function getName(){ alert(5); };
    Foo.getName();//2
    getName();//4
    Foo().getName();//1
    getName();//1
    new Foo.getName();//2
    new Foo().getName();//3
    new new Foo().getName();//3
    ```

    (4)
    ```javascript
    var a=1;
    var b={
        a:2,
        b:function(){
            console.log(this.a);//1
        }(),
        f:this.f=function(){
            console.log(this.a);
        }
    };
    function f(){ console.log(3); }
    f();//1
    b.f();//2
    (b.f)();//2
    (0,b.f)();//1
    ```

    (5)
    ```javascript
    var foo=function(){
        console.log(this.a);
    }
    var obj={a:2,foo:foo};
    var a=10;
    var bar=obj.foo;
    var bar2=foo.bind(obj);
    bar();//10
    bar2();//2
    foo();//10
    obj.foo();//2
    setTimeout(bar,0);//10
    ```

    (6)
    ```javascript
    function MyObj(){ 
        this.p.pid++; 
    }
    MyObj.prototype.p={"pid":0}//2
    MyObj.prototype.getNum=function(num){
        return this.p.pid+num;
    }
    var _obj1=new MyObj(); //创建新对象，继承原型pid+1
    var _obj2=new MyObj(); //创建新对象，继承原型pid+2
    console.log( _obj1.getNum(1)+_obj2.getNum(2));
                //7      2+1   +    2+2
    ```

1. HTML5的离线存储怎么使用？能否解释一下工作原理？

    ```css
    在用户没有连接英特网时，可以正常访问站点和应用；在用户连接英特网时，更新用户机器上的缓存文件。
    原理：HTML5的离线存储是基于一个新建的 .appcache 文件的缓存机制（并非存储技术），通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储下来。之后当网络处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
    使用方法：
    (1) 在页面头部像下面一样加入一个 manifest 的属性；
    (2) 在 cache.manifest 文件里编写离线存储资源；
        CACHE MANIFEST
        #v0.11
        CACHE：
            js/app.js
            css/style.css
        NETWORK:
            resource/logo.png
        FALLBACK：
            / /offline.html
    (3) 在离线状态时，操作 window.applicationCache 进行需求实现；
    ```

1. 常见的浏览器内核有哪些?

    ```css
    Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
    Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等。
    Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
    Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
    EdgeHTML内核：Microsoft Edge。  [此内核其实是从MSHTML fork而来，删掉了几乎所有的IE私有特性]
    ```

1. 请描述一下 cookies，sessionStorage 和 localStorage 的区别？

    ```css
    Web Storage有两种形式：LocalStorage（本地存储）和sessionStorage（会话存储）。这两种方式都允许开发者使用js设置的键值对进行操作，在在重新加载不同的页面的时候读出它们。这一点与cookie类似。
    （1）与cookie不同的是：Web Storage数据完全存储在客户端，不需要通过浏览器的请求将数据传给服务器，因此x相比cookie来说能够存储更多的数据，大概5M左右。
    （2）LocalStorage和sessionStorage功能上是一样的，但是存储持久时间不一样。
        LocalStorage：浏览器关闭了数据仍然可以保存下来，并可用于所有同源（相同的域名、协议和端口）窗口（或标签页）；
        sessionStorage：数据存储在窗口对象中，窗口关闭后对应的窗口对象消失，存储的数据也会丢失。
        注意：sessionStorage 都可以用localStorage 来代替，但需要记住的是，在窗口或者标签页关闭时，使用sessionStorage 存储的数据会丢失。
    （3）使用 local storage和session storage主要通过在js中操作这两个对象来实现，分别为window.localStorage和window.sessionStorage. 这两个对象均是Storage类的两个实例，自然也具有Storage类的属性和方法。
    ```
1. ifame有哪些缺点

    ```css
    （1）iframe会阻塞主页面的Onload事件；
    （2）搜索引擎的检索程序无法解读这种页面，不利于SEO；
    （3）iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
    （4）使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好通过JavaScript动态给iframe添加src属性值，这样可以绕开以上两个问题。
    ```

1. `link`和`@import`的区别?

    ```css
    （1）link属于XHTML标签，而@import是CSS提供的。
    （2）页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载。
    （3）import只在IE 5以上才能识别，而link是XHTML标签，无兼容问题。
    （4）link方式的样式权重高于@import的权重。
    （5）使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的。
    ```

1. 经常遇到的浏览器的兼容性有哪些?原因,解决方法?

    ```css
    （1）png24为的图片在IE6浏览器上出现背景，解决方案是做成PNG8。
    （2）浏览器默认的margin和padding不同，解决方案是加一个全局的*{margin:0;padding:0;}来统一。
    （3）IE6双边距bug:块属性标签float后，又有横行的margin情况下，在IE 6显示margin比设置的大。
    （4）浮动ie产生的双边距问题：块级元素就加display：inline；行内元素转块级元素display：inline后面再加display：table。
            .bb{
                background-color:#f1ee18;        /*所有识别*/
                .background-color:#00deff\9;        /*IE6、7、8识别*/
                +background-color:#a200ff;        /*IE6、7识别*/
                _background-color:#1e0bd1;        /*IE6识别*/
            }
    ```

1. 常见Hack技巧?

    ```css
    （1）IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用getAttribute()获取自定义属性；
    （2）Firefox下，只能使用getAttribute()获取自定义属性。解决方法：统一通过getAttribute()获取自定义属性。
    （3）IE下，even对象有x,y属性，但是没有pageX,pageY属性；
    （4）Firefox下，event对象有pageX,pageY属性，但是没有x,y属性。解决方法是条件注释，缺点是在IE浏览器下可能会增加额外的HTTP请求数。
    （5）Chrome 中文界面下默认会将小于12px的文本强制按照12px显示，可通过加入 CSS属性-webkit-text-size-adjust: none;来解决。
    （6）超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不再具有hover和active了，解决方法是改变CSS属性的排列顺序：
    L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}
    ```

1. 让Chrome支持小于12px的文字

    `body{-webkit-text-size-adjust:none}`

1. 现在HTML5中css3可以写出一个旋转的立方体，请写出要用到的CSS属性。

    ```css
    -webkit-transform-style: preserve-3d;
    -webkit-transform: rotateY(30deg) rotateX(10deg);
    -webkit-animation:  rot 4s linear infinite;
    ```

1. Sass && Less

    ```css
    Sass (Syntactically Awesome Stylesheets)是一种动态样式语言，语法跟css一样(但多了些功能)，比css好写，而且更容易阅读。Sass语法类似与Haml，属于缩排语法（makeup），用意就是为了快速写Html和Css。
    Less一种动态样式语言. 将CSS赋予了动态语言的特性，如变量，继承，运算， 函数. LESS 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可一在服务端运行 (借助 Node.js)。
    区别：
    (1))Sass是基于Ruby的，是在服务端处理的，而Less是需要引入less.js来处理Less代码输出Css到浏览器，也可以在开发环节使用Less，然后编译成Css文件，直接放到项目中，也有Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。
    (2)变量符不一样，less是@，而Scss是$，而且变量的作用域也不一样，后面会讲到。
    (3)输出设置，Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。
    (4)Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。
    ```

1. 写出大于1024并小于1360屏幕的媒体查询关键代码

    ```css
    @Media screen and min-width(1024px) and max-width(1360px){
        //doSomething
    }
    ```

1. 如何让chrome盒模型(w3c标准盒模型)显示的和IE的一致?

    1. 标准浏览器盒子模型的宽度 = width + padding-left + padding-right + border-left + border-right + margin-left + margin-right
    1. IE浏览器(6-8)盒子模型的宽度 = width + margin-left + margin-right

    方法 : `display:border-box;`

1. Array自带的排序Sort是怎么实现的?

    基于一种更复杂的快排实现的(`sort`会破坏原数组内部的顺序)

1. navigator对象

    navigator对象包含有关浏览器的信息. [更多信息请点击](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/navigator)

    | 属性 | 描述 |
    |:-- |:-- |
    |appCodeName|返回浏览器的代码名|
    |appMinorVersion|返回浏览器的次级版本|
    |appName|返回浏览器的名称|
    |appVersion|返回浏览器的平台和版本信息|

1. 手写jsonp的实现  [参考自github上charleylla的部分代码](https://gist.github.com/charleylla/e44a0ce59accea062ebf357e55489f02)

    ```javascript
    (function (window, document) {
        'use strict';
        var jsonp = function (url, data, callback) {

            //1 挂载回调函数
            var fnSuffix = Math.random().toString().replace('.', '');
            var cbFuncName = 'my_json_cb_' + fnSuffix;
            //将函数挂载在全局环境的方式不推荐  使用cbs.my_json_cb_
            window[cbFuncName] = callback;

            //2 将data转化成url字符串的形式
            // {id:1,name:'zhangsan'} =>id=1&name=zhangsan
            var querystring = url.indexOf('?') == -1 ? '?' : '&';
            for (var key in data) {
                querystring += key + '=' + data[key] + '&';
                //            id=          1   &
            }
            //querystring=?id=1&name=zhangsan&
            //3 处理url地址中的回调参数
            //url+=callback=sdfsfdg

            querystring += 'callback=' + cbFuncName;
            //querystring=?id=1&name=zhangsan&cb=my_json_cb_0231241
            //4 创建一个script的标签
            var scriptElement = document.createElement('script');
            scriptElement.src = url + querystring;
            // 此时还不能将其append到页面上

            //5 将script标签放到页面中
            document.body.appendChild(scriptElement);
            //append过后页面会自动对这个地址发送请求，请求完成以后自动执行脚本

        };

        /*把jsonp放到全局*/
        window.$jsonp = jsonp;

    })(window, document);

    /* 参考用例
    (function(){
        $jsonp(//地址
                'http://api.douban.com/v2/movie/in_theaters',
                //传递的参数
                {
                    count:10,start:5
                },
                //回调函数
                function(data){
                    console.log(data);
                }
        );
    })();
    */
    ```

1. http请求头，请求体，cookie在哪个里面？url在哪里面？

    [相关介绍](https://github.com/seed-fe/blog/wiki/%E4%B8%83%E3%80%81%E7%BB%93%E5%90%88%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E5%AD%A6%E4%B9%A0http%E5%8D%8F%E8%AE%AE)

    * HTTP的请求报文分为三个部分--请求行,请求头和请求体
        1. 请求行分为三个部分:请求方法,请求地址和协议及版本,以CRLF(rn)结束

1. GET与POST的区别
    1. GET请求是为了从服务器获取内容，在需要提交数据的情况下，提交的数据会作为querystring放在URL之后，以?分割URL和querystring，querystring也只是为了获取内容所发送的参数；POST请求是为了向服务器发送内容（比如表单），会把提交的数据放在HTTP包的request body中。
    1. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制。
    1. 在express框架中，对于GET请求的参数'?xxxx=',使用req.query.xxxxx方法获得；对于POST请求的参数，使用req.body.xxxxx方法获得
    1. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码。


1. HTTP协议的状态消息都有哪些?(如200、302对应的描述)

    状态码主要用来表示响应的状态，根据状态码可以判断响应是否如预期。

    HTTP/1.1中定义了5类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别

    * 1XX 提示信息类 - 表示请求已被成功接收，继续处理

    * 2XX 响应成功类 - 表示请求已被成功接收，理解，接受

    * 3XX 重定向类 - 要完成请求必须进行更进一步的处理

    * 4XX 客户端错误类 - 请求有语法错误或请求无法实现

    * 5XX 服务器端错误类 - 服务器未能实现合法的请求

    状态码有很多，不过一开始只需要掌握一些比较常见的：

    * 200 ok： 最常见的就是成功响应状态码200了， 这表明该请求被成功地完成，所请求的资源发送回客户端。上面打开项目主页的实例中就是200
    * 304 not modified： 假如我们打开主页后在浏览器中刷新，就会看到响应的状态码变成了304，这代表之前响应的html文档已经被缓存了，服务器端相同的文档没有变化，可以继续使用缓存的文档，因此304响应没有response body部分
    * 302 found： 重定向，新的URL会在response header中的Location中返回，浏览器将会自动使用新的URL发出新的Request，假如我们在登录页提交登录表单发送一个POST请求进行登录，就会得到一个302响应并且重定向到/index路径下
    * 404 not found: 请求资源不存在（输错了URL，或者服务器端现在没有这个页面了）
    * 500 Internal Server Error： 服务器发生了不可预期的错误，这个一般在会在服务器的程序码出错时发生

1. OSI七层模型

    | OSI中的层 | 功能 | TCP/IP协议族 | 相应措施 |
    |:--------- |:---- |:----------- |:---- |
    |应用层| 文件传输，电子邮件，文件服务，虚拟终端 |TFTP，HTTP，SNMP，FTP，SMTP，DNS，Telnet|  |
    表示层| 数据格式化，代码转换，数据加密| 没有协议 | Linux应用命令测试 |
    会话层| 解除或建立与别的接点的联系| 没有协议 |   |
    传输层| 提供端对端的接口 | TCP，UDP| TCP,UDP协议分析 |
    网络层| 为数据包选择路由 | IP，ICMP，RIP，OSPF，BGP，IGMP| 检查IP地址,路由设置 |
    数据链路层| 传输有地址的帧以及错误检测功能 | SLIP，CSLIP，PPP，ARP，RARP，MTU| ARP地址检测,物理连接测试 |
    物理层| 以二进制数据形式在物理媒体上传输数据| ISO2110，IEEE802，IEEE802.2|  |
