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

1. js的原型继承与类继承&emsp;(原型继承比较好,类继承只继承了实例属性，没有原型属性。原型链继承可以继承所有。)

    ```javascript
    //类继承
    var father = function () {
        this.age = 52;
        this.say = function () {
            alert('hello i am ' + this.name + ' and i am ' + this.age + 'years old');
        }
    }

    var child = function () {
        this.name = 'bill';
        father.call(this);
    }

    var man = new child();
    man.say();

    //原型继承
    var father = function () {}

    father.prototype.a = function () {}
    var child = function () {}

    child.prototype = new father();

    var man = new child();

    man.a();
    ```

1. 用apply和call怎么继承原型链上的共享属性？通过空函数传值。新建一个空函数C。C实例化后C的实例属性就是空，然后用B的apply/call去继承C，相当于继承了C的实例属性。[详细说明](https://segmentfault.com/a/1190000007801452)

    ```javascript
    function inherits(Child, Parent) {
        var F = function () {};
        F.prototype = Parent.prototype;
        Child.prototype = new F();
        Child.prototype.constructor = Child;
    }
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

1. 如何创建一个ajax？(总共有8种callback , `onSuccess` , `onFailure` , `onUninitialized` , `onLoading` , `onLoaded` , `onInteractive` , `onComplete` , `onException`)实例化，open，send,onreadystatechange，然后是req,readyState和status。原生ajax的四个过程。实例化，open，send,onreadystatechange，然后是req,readyState和status。那么问题是通过哪个属性得到data？jquery里是success回调里面的形参。responseText和responseXML。后者是XML解析了的。
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
    用promise手写ajax. [详细说明](https://johanzhu.github.io/2016/10/23/19/)

    ```javascript
    var getJSON = function (url) {
        var promise = new Promise(function (resolve, reject) {
            var client = new XMLHttpRequest();
            client.open("GET", url);
            client.onreadystatechange = handler;
            client.responseType = "json";
            client.setRequestHeader("Accept", "application/json");
            client.send();

            function handler() {
                if (this.readyState !== 4) {
                    return;
                }
                if (this.status === 200) {
                    resolve(this.response);
                } else {
                    reject(new Error(this.statusText));
                }
            };
        });
        return promise;
    };

    getJSON("/posts.json").then(function (json) {
        console.log('Contents: ' + json);
    }, function (error) {
        console.error('出错了', error);
    });
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

1. 异步加载JS的方式有哪些？&emsp;[defer与async的区别](https://segmentfault.com/q/1010000000640869)
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
    //格式规范 : JSON.parse('{"name":[1,2,3]}')
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

1. navigator对象

    navigator对象包含有关浏览器的信息. [更多信息请点击](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/navigator)

    | 属性 | 描述 |
    |:-- |:-- |
    |appCodeName|返回浏览器的代码名|
    |appMinorVersion|返回浏览器的次级版本|
    |appName|返回浏览器的名称|
    |appVersion|返回浏览器的平台和版本信息|

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
    （3）IE下，event对象有x,y属性，但是没有pageX,pageY属性；
    （4）Firefox下，event对象有pageX,pageY属性，但是没有x,y属性。解决方法是条件注释，缺点是在IE浏览器下可能会增加额外的HTTP请求数。
    （5）Chrome 中文界面下默认会将小于12px的文本强制按照12px显示，可通过加入 CSS属性-webkit-text-size-adjust: none;来解决。
    （6）超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不再具有hover和active了，解决方法是改变CSS属性的排列顺序：
    L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}
    ```

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
    (2)变量符不一样，less是@，而Scss是$，而且变量的作用域也不一样。
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

    * 标准浏览器盒子模型的宽度 = width + padding-left + padding-right + border-left + border-right + margin-left + margin-right
    * IE浏览器(6-8)盒子模型的宽度 = width + margin-left + margin-right

    方法 : `display:border-box;`

1. Array自带的排序Sort是怎么实现的?

    基于一种更复杂的快排实现的(`sort`会破坏原数组内部的顺序)

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

1. document.domain+iframe的设置

    对于主域相同而子域不同的例子，可以通过设置document.domain的办法来解决。具体的做法是可以在`http://www.a.com/a.html`和`http://script.a.com/b.html`两个文件中分别加上document.domain = ‘a.com’；然后通过a.html文件中创建一个iframe，去控制iframe的contentDocument，这样两个js文件之间就可以“交互”了。当然这种办法只能解决主域相同而二级域名不同的情况，如果你异想天开的把script.a.com的domian设为alibaba.com那显然是会报错地！代码如下：

    `www.a.com`上的`a.html`
    ```javascript
    document.domain = 'a.com';
    var ifr = document.createElement('iframe');
    ifr.src = 'http://script.a.com/b.html';
    ifr.style.display = 'none';
    document.body.appendChild(ifr);
    ifr.onload = function(){
        var doc = ifr.contentDocument || ifr.contentWindow.document;
        // 在这里操纵b.html
        alert(doc.getElementsByTagName("h1")[0].childNodes[0].nodeValue);
    };
    ```

    script.a.com上的b.html
    ```javscript
    document.domain = 'a.com';
    ```

    这种方式适用于{www.kuqin.com, kuqin.com, script.kuqin.com, css.kuqin.com}中的任何页面相互通信。

    备注：某一页面的domain默认等于window.location.hostname。主域名是不带www的域名，例如a.com，主域名前面带前缀的通常都为二级域名或多级域名，例如www.a.com其实是二级域名。 domain只能设置为主域名，不可以在b.a.com中将domain设置为c.a.com。

    问题：

    1、安全性，当一个站点（b.a.com）被攻击后，另一个站点（c.a.com）会引起安全漏洞。

    2、如果一个页面中引入多个iframe，要想能够操作所有iframe，必须都得设置相同domain。

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

1. OSI七层模型 && TCP/IP五层模型的协议

    | OSI中的层 | 功能 | TCP/IP协议族 | 相应措施 |
    |:--------- |:---- |:----------- |:---- |
    |应用层| 文件传输，电子邮件，文件服务，虚拟终端 |TFTP，HTTP，SNMP，FTP，SMTP，DNS，Telnet|  |
    表示层| 数据格式化，代码转换，数据加密| 没有协议 | Linux应用命令测试 |
    会话层| 解除或建立与别的接点的联系| 没有协议 |   |
    传输层| 提供端对端的接口 | TCP，UDP| TCP,UDP协议分析 |
    网络层| 为数据包选择路由 | IP，ICMP，RIP，OSPF，BGP，IGMP| 检查IP地址,路由设置 |
    数据链路层| 传输有地址的帧以及错误检测功能 | SLIP，CSLIP，PPP，ARP，RARP，MTU| ARP地址检测,物理连接测试 |
    物理层| 以二进制数据形式在物理媒体上传输数据| ISO2110，IEEE802，IEEE802.2|  |

    | TCP/IP层 | 网络设备 |
    |:-------- |:------- |
    | 应用层 |   |
    | 传输层 | 四层交换机、也有工作在四层的路由器 |
    | 网络层 | 路由器、三层交换机 |
    | 物理层 | 中继器、集线器、还有我们通常说的双绞线也工作在物理层 |
    | 数据链路层 | 网桥（现已很少使用）、以太网交换机（二层交换机）、网卡（其实网卡是一半工作在物理层、一半工作在数据链路层） |

    ```css
    TCP (Transmission Control Protocol)和UDP(User Datagram Protocol)协议属于传输层协议。其中TCP提供IP环境下的数据可靠传输，它提供的服务包括数据流传送、可靠性、有效流控、全双工操作和多路复 用。通过面向连接、端到端和可靠的数据包发送。通俗说，它是事先为所发送的数据开辟出连接好的通道，然后再进行数据发送；而UDP则不为IP提供可靠性、 流控或差错恢复功能。一般来说，TCP对应的是可靠性要求高的应用，而UDP对应的则是可靠性要求低、传输经济的应用。TCP支持的应用协议主要 有：Telnet、FTP、SMTP等；UDP支持的应用层协议主要有：NFS（网络文件系统）、SNMP（简单网络管理协议）、DNS（主域名称系 统）、TFTP（通用文件传输协议）等.
    TCP/IP协议与低层的数据链路层和物理层无关，这也是TCP/IP的重要特点
    OSI是Open System Interconnect的缩写，意为开放式系统互联。
    OSI七层参考模型的各个层次的划分遵循下列原则：
    1、同一层中的各网络节点都有相同的层次结构，具有同样的功能。
    2、同一节点内相邻层之间通过接口（可以是逻辑接口）进行通信。
    3、七层结构中的每一层使用下一层提供的服务，并且向其上层提供服务。
    4、不同节点的同等层按照协议实现对等层之间的通信。

    第一层：物理层（PhysicalLayer)，
    规定通信设备的机械的、电气的、功能的和过程的特性，用以建立、维护和拆除物理链路连接。具体地讲，机械 特性规定了网络连接时所需接插件的规格尺寸、引脚数量和排列情况等；电气特性规定了在物理连接上传输bit流时线路上信号电平的大小、阻抗匹配、传输速率 距离限制等；功能特性是指对各个信号先分配确切的信号含义，即定义了DTE和DCE之间各个线路的功能；规程特性定义了利用信号线进行bit流传输的一组 操作规程，是指在物理连接的建立、维护、交换信息是，DTE和DCE双放在各电路上的动作系列。在这一层，数据的单位称为比特（bit）。属于物理层定义的典型规范代表包括：EIA/TIA RS-232、EIA/TIA RS-449、V.35、RJ-45等。

    第二层：数据链路层（DataLinkLayer):
    在物理层提供比特流服务的基础上，建立相邻结点之间的数据链路，通过差错控制提供数据帧（Frame）在信道上无差错的传输，并进行各电路上的动作系列。数据链路层在不可靠的物理介质上提供可靠的传输。该层的作用包括：物理地址寻址、数据的成帧、流量控制、数据的检错、重发等。在这一层，数据的单位称为帧（frame）。数据链路层协议的代表包括：SDLC、HDLC、PPP、STP、帧中继等。

    第三层是网络层
    在 计算机网络中进行通信的两个计算机之间可能会经过很多个数据链路，也可能还要经过很多通信子网。网络层的任务就是选择合适的网间路由和交换结点， 确保数据及时传送。网络层将数据链路层提供的帧组成数据包，包中封装有网络层包头，其中含有逻辑地址信息- -源站点和目的站点地址的网络地址。如 果你在谈论一个IP地址，那么你是在处理第3层的问题，这是“数据包”问题，而不是第2层的“帧”。IP是第3层问题的一部分，此外还有一些路由协议和地 址解析协议（ARP）。有关路由的一切事情都在这第3层处理。地址解析和路由是3层的重要目的。网络层还可以实现拥塞控制、网际互连等功能。在这一层，数据的单位称为数据包（packet）。网络层协议的代表包括：IP、IPX、RIP、OSPF等。

    第 四层是处理信息的传输层
    第4层的数据单元也称作数据包（packets）。但是，当你谈论TCP等具体的协议时又有特殊的叫法，TCP的数据单元称为段 （segments）而UDP协议的数据单元称为“数据报（datagrams）”。这个层负责获取全部信息，因此，它必须跟踪数据单元碎片、乱序到达的 数据包和其它在传输过程中可能发生的危险。第4层为上层提供端到端（最终用户到最终用户）的透明的、可靠的数据传输服务。所为透明的传输是指在通信过程中 传输层对上层屏蔽了通信传输系统的具体细节。传输层协议的代表包括：TCP、UDP、SPX等。

    第五层是会话层
    这一层也可以称为会晤层或对话层，在会话层及以上的高层次中，数据传送的单位不再另外命名，而是统称为报文。会话层不参与具体的传输，它提供包括访问验证和会话管理在内的建立和维护应用之间通信的机制。如服务器验证用户登录便是由会话层完成的。

    第六层是表示层
    这一层主要解决拥护信息的语法表示问题。它将欲交换的数据从适合于某一用户的抽象语法，转换为适合于OSI系统内部使用的传送语法。即提供格式化的表示和转换数据服务。数据的压缩和解压缩， 加密和解密等工作都由表示层负责。

    第七层应用层
    应用层为操作系统或网络应用程序提供访问网络服务的接口。应用层协议的代表包括：Telnet、FTP、HTTP、SNMP等。

    除了层的数量之外，开放式系统互联（OSI）模型与TCP/IP协议有什么区别？

    开放式系统互联模型是一个参考标准，解释协议相互之间应该如何相互作用。TCP/IP协议是美国国防部发明的，是让互联网成为了目前这个样子的标准之一。开放式系统互联模型中没有清楚地描绘TCP/IP协议，但是在解释TCP/IP协议时很容易想到开放式系统互联模型。两者的主要区别如下：

    TCP/IP协议中的应用层处理开放式系统互联模型中的第五层、第六层和第七层的功能。

    TCP/IP协议中的传输层并不能总是保证在传输层可靠地传输数据包，而开放式系统互联模型可以做到。TCP/IP协议还提供一项名为UDP（用户数据报协议）的选择。UDP不能保证可靠的数据包传输。

    TCP/UDP协议

    TCP(Transmission Control Protocol)和UDP(User Datagram Protocol)协议属于传输层协议。其中TCP提供IP环境下的数据可靠传输，它提供的服务包括数据流传送、可靠性、有效流控、全双工操作和多路复用。通过面向连接、端到端和可靠的数据包发送。通俗说，它是事先为所发送的数据开辟出连接好的通道，然后再进行数据发送；而UDP则不为IP提供可靠性、流控或差错恢复功能。一般来说，TCP对应的是可靠性要求高的应用，而UDP对应的则是可靠性要求低、传输经济的应用。

    TCP支持的应用协议主要有：Telnet、FTP、SMTP等；UDP支持的应用层协议主要有：NFS（网络文件系统）、SNMP（简单网络管理协议）、DNS（主域名称系统）、TFTP（通用文件传输协议）等。

    TCP/IP协议与低层的数据链路层和物理层无关，这也是TCP/IP的重要特点。

    OSI是Open System Interconnect的缩写，意为开放式系统互联。
    ```

    网络分层结构。4层，应用层，传输层，网络层和数据链路层。依次是http等应用，TCP/UDP，IP和物理连接。然后又追问了一下ssl在哪一层。ssl是socket，是单独的一层。如果要算应该算传输层。

1. 解释平衡二叉树，以及在数据结构中的应用（红黑树）

    平衡二叉树（Balanced Binary Tree）又被称为AVL树（有别于AVL算法），且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

    红黑树（英语：Red–black tree）是一种自平衡二叉查找树，是在计算机科学中用到的一种数据结构，典型的用途是实现关联数组。

    数据结构中的应用:
    * 红黑树和AVL树一样都对插入时间、删除时间和查找时间提供了最好可能的最坏情况担保。这不只是使它们在时间敏感的应用如实时应用（real time application）中有价值，而且使它们有在提供最坏情况担保的其他数据结构中作为建造板块的价值；例如，在计算几何中使用的很多数据结构都可以基于红黑树。
    * 红黑树在函数式编程中也特别有用，在这里它们是最常用的持久数据结构（persistent data structure）之一，它们用来构造关联数组和集合，每次插入、删除之后它们能保持为以前的版本。除了O(log n)的时间之外，红黑树的持久版本对每次插入或删除需要O(log n)的空间。
    * 红黑树是2-3-4树的一种等同。换句话说，对于每个2-3-4树，都存在至少一个数据元素是同样次序的红黑树。在2-3-4树上的插入和删除操作也等同于在红黑树中颜色翻转和旋转。这使得2-3-4树成为理解红黑树背后的逻辑的重要工具，这也是很多介绍算法的教科书在红黑树之前介绍2-3-4树的原因，尽管2-3-4树在实践中不经常使用。
    * 红黑树相对于AVL树来说，牺牲了部分平衡性以换取插入/删除操作时少量的旋转操作，整体来说性能要优于AVL树。
    性质[编辑]

1. css3旋转,动画

    ```css
    div.carset{
        width:0;
        height:0;
        border-width:20px;
        border-style:solid;
        border-color:transparent;
        border-top-color:#666;
        animation: animationName 5s infinite;
    }
    @keyframes animationName{
        from{
            transform:rotate(0deg)
        }to{
            transform:rotate(360deg)
        }
    }
    ```

1. 移动端适配问题

    ```css
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scable=no">
    ```
    * initial-scale设置页面的初始缩放程度 和布局视口的宽度
    * minimum-scale允许用户的最小缩放程度，为一个数字，可以带小数
    * maximum-scale允许用户的最大缩放值，为一个数字，可以带小数
    * user-scalable是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许
    * width=device-width : 在safari中，当设置initial-scale= 1 时，理想视口的尺寸会随着屏幕的旋转改变。在竖屏时，布局视口的宽度是320px ,横屏下，是480px 或者568px.但在ie10中却有完全相反的问题.initial-scale = 1  时，在横屏模式下宽度也保持为320px ,但width = device-width 时，它会从320px变为480px.

1. Http/2

    （超文本传输协议第2版，最初命名为HTTP 2.0），是HTTP协议的的第二个主要版本，使用于万维网。HTTP/2的目标包括异步连接复用，头压缩和请求反馈管线化并保留与HTTP 1.1的完全语义兼容。

1. 使用原生JS来操作Cookie

    ```javscript
    //写cookies

    function setCookie(name,value)
    {
        var Days = 30;
        var exp = new Date();
        exp.setTime(exp.getTime() + Days*24*60*60*1000);
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
    }

    //读取cookies
    function getCookie(name)
    {
        var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
        if(arr=document.cookie.match(reg))
            return unescape(arr[2]);
        else
            return null;
    }

    //删除cookies
    function delCookie(name)
    {
        var exp = new Date();
        exp.setTime(exp.getTime() - 1);
        var cval=getCookie(name);
        if(cval!=null)
            document.cookie= name + "="+cval+";expires="+exp.toGMTString();
    }
    //使用示例
    setCookie("name","hayden");
    alert(getCookie("name"));

    //如果需要设定自定义过期时间
    //那么把上面的setCookie　函数换成下面两个函数就ok;


    //程序代码
    function setCookie(name,value,time)
    {
        var strsec = getsec(time);
        var exp = new Date();
        exp.setTime(exp.getTime() + strsec*1);
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
    }
    function getsec(str)
    {
        alert(str);
        var str1=str.substring(1,str.length)*1;
        var str2=str.substring(0,1);
        if (str2=="s"){
            return str1*1000;
        }else if (str2=="h"){
            return str1*60*60*1000;
        }else if (str2=="d"){
            return str1*24*60*60*1000;
        }
    }
    //这是有设定过期时间的使用示例：
    //s20是代表20秒
    //h是指小时，如12小时则是：h12
    //d是天数，30天则：d30

    setCookie("name","hayden","s20");
    ```

1. DOM事件流的三个阶段 ( 事件捕获阶段 , 处于目标阶段 , 事件冒泡阶段 )

    [理解DOM事件流的三个阶段](https://segmentfault.com/a/1190000004463384)

    注意: 在 target phase，event handler 被调用的顺序不再遵循先捕获，后冒泡的原则，而是严格按照 event handler 注册的顺序,即给btn同时绑定冒泡和捕获事件,谁先执行,谁就先触发,不一定是先捕获在冒泡

1. 前端攻防(主要是CSRF和XSS)&emsp;[了解更多](https://zhuanlan.zhihu.com/p/25486768?group_id=820705780520079360)
    * CSRF : 中文名,跨站请求伪造。可以简单理解为,攻击者盗用了你的身份，以你的名义发送恶意请求，容易造成个人隐私泄露以及财产安全。要完成一次 CSRF 攻击，受害者必须完成：
        * 登录受信任网站，并在本地生成 cookie
        * 在不登出 A 的情况下，访问危险网站 B

        防御手段 : 现在一般都在服务端进行防御
        * 关键操作只接受POST请求
        * 验证码
        * 检测Referer
        * Token(目前主流做法)
            * Token 要足够随机，使攻击者无法准确预测
            * Token 是一次性的，即每次请求成功后要更新 Token，增加预测难度
            * Token 要主要保密性，敏感操作使用 POST，防止 Token 出现在 URL 中

    * XSS : 全称为 Cross-site script，跨站脚本攻击，为了和 CSS 层叠样式表区分所以取名为 XSS，是 Web 程序中常见的漏洞。其原理是攻击者向有 XSS 漏洞的网站中输入恶意的 HTML 代码，当其它用户浏览该网站时候，该段 HTML 代码会自动执行，从而达到攻击的目的，如盗取用户的 Cookie，破坏页面结构，重定向到其它网站等。
        * 持久型 XSS 就是对客户端攻击的脚本植入到服务器上，从而导致每个正常访问到的用户都会遭到这段 XSS 脚本的攻击。(如上述的留言评论功能)
        * 非持久型 XSS 是对一个页面的 URL 中的某个参数做文章，把精心构造好的恶意脚本包装在 URL 参数重，再将这个 URL 发布到网上，骗取用户访问，从而进行攻击

        防御 XSS 攻击最简单直接的方法就是过滤用户的输入.
        * 如果不需要用户输入 HTML，可以直接对用户的输入进行 HTML 转义
        * 当用户需要输入 HTML 代码时：将用户的输入使用 HTML 解析库进行解析，获取其中的数据。然后根据用户原有的标签属性，重新构建 HTML 元素树。构建的过程中，所有的标签、属性都只从白名单中拿取。

1. 实现事件代理

    实现原理是利用了浏览器的事件冒泡和事件源（target）

    例如给一个`table`的`td`加一个单击事件,但是如果表格1000行,就得绑定1000次,所以,可以使用`bind`等函数奖`click`绑定到了`table`上,从而实现事件代理.

    ```javascript
    $("#tab td").click(function () {
        $(this).css("background", "red");
    });
    //事件代理后
    $("#tab").bind("click", function (ev) {
        var $obj = $(ev.target);
        $obj.css("background", "red");
    })
    ```

    原生JS实现,注意子元素传递上来的应该是event.target或者e.srcElement。这个强调下IE和W3C的区别，建议写一个封装。

    ```javascript
    // ============ 简单的事件委托
    function delegateEvent(interfaceEle, selector, type, fn) {
        if (interfaceEle.addEventListener) {
            interfaceEle.addEventListener(type, eventfn);
        } else {
            interfaceEle.attachEvent("on" + type, eventfn);
        }

        function eventfn(e) {
            var e = e || window.event;
            var target = e.target || e.srcElement;
            if (matchSelector(target, selector)) {
                if (fn) {
                    fn.call(target, e);
                }
            }
        }
    }
    /**
    * only support #id, tagName, .className
    * and it's simple single, no combination
    */
    function matchSelector(ele, selector) {
        // if use id
        if (selector.charAt(0) === "#") {
            return ele.id === selector.slice(1);
        }
        // if use class
        if (selector.charAt(0) === ".") {
            return (" " + ele.className + " ").indexOf(" " + selector.slice(1) + " ") != -1;
        }
        // if use tagName
        return ele.tagName.toLowerCase() === selector.toLowerCase();
    }
    //调用
    var odiv = document.getElementById("oDiv");
    delegateEvent(odiv, "a", "click", function () {
        alert("1");
    })
    ```

1. 前端路由?前后端路由的区别?

    区别在于express是服务器端的路由，也就是说需要向后台服务器发送请求，然后服务器来决定来render那个.html，这也就是最早的mvc架构模式，而前端的路由是将这一过程放在浏览器端，也就是前台写js代码控制，不在请求服务器，前台一般利用histroy和hash来控制，达到不刷新页面可以使显示内容发生变化，这样好处是js代码不发生变化(浏览器端可以维护一个稳定的model)；一般单页应用就是前台来控制路由，这样速度更快，用户体验更好。单页应用还将模板拿到了浏览器端，从而解放了服务端，服务端趋于服务化。

1. 如何获取光标的水平位置?

    ```JavaScript
    function getX(e){
        e = e || window.event;
        //先检查非IE浏览器，再检查IE的位置
        return e.pageX || e.clentX + document.body.scrollLeft;
    }
    ```

1. 原生JS实现鼠标拖拽

    ```javascript
    function drag(elementId) {
        var element = document.getElementById(elementId);
        var position = {
            offsetX: 0, //点击处偏移元素的X
            offsetY: 0, //偏移Y值
            state: 0 //是否正处于拖拽状态，1表示正在拖拽，0表示释放
        };

        //获得兼容的event对象
        function getEvent(event) {
            return event || window.event;
        }

        //元素被鼠标拖住
        element.addEventListener('mousedown', function (event) {

            //获得偏移的位置以及更改状态
            var e = getEvent(event);
            position.offsetX = e.offsetX;
            position.offsetY = e.offsetY;
            position.state = 1;
        }, false);

        //元素移动过程中
        document.addEventListener('mousemove', function (event) {

            var e = getEvent(event);
            if (position.state) {
                position.endX = e.clientX;
                position.endY = e.clientY;
                //设置绝对位置在文档中，鼠标当前位置-开始拖拽时的偏移位置
                element.style.position = 'absolute';
                element.style.top = position.endY - position.offsetY + 'px';
                element.style.left = position.endX - position.offsetX + 'px';
            }
        }, false);

        //释放拖拽状态
        element.addEventListener('mouseup', function (event) {
            position.state = 0;
        }, false);
    }

    drag('box');
    ```

1. 自己项目中写的比较好的代码

    ```javascript
    //搜索框等待用户停止输入后再发送请求
    input.on("input propertychange", function (event) {
        if (input.val().trim() == "") {
            input_icon.attr("xlink:href", "#icon-sousuo-sousuo");
            search_list.hide();
        } else {
            input_icon.attr("xlink:href", "#icon-cha");
            search_list.show();
            /!*初始化当前页数*!/
            search_list_footer.attr("data-current-page", "1");
            /!*用户停止输入一段时间后进行事件触发*!/
            last = event.timeStamp;
            clearTimeout();
            setTimeout(function () {
                if (last - event.timeStamp == 0) {
                    //进行请求
                    searchWxAccountList(search_url);
                }
            }, 800);
        }
    })
    //点击div以外的区域gaidiv消失
    $(document).on("click", function (e) {
        var target = $(e.target);
        if (target.closest(".search_list").length == 0) {
            input.val("");
            search_list.hide();
            search_list_footer.hide();
            search_list.children(".search_list_body").empty();
            input_icon.attr("xlink:href", "#icon-sousuo-sousuo");
        }
    })
    document.addEventListener('click', function (e) {
        var target = e.target;
        if (!target.closest('.search_list')) {
            input.value = '';
            search_list.style.display = 'none';
            search_list_footer.style.display = 'none';
            search_list.querySelector('.search_list_body').innerHTML = '';
            input_icon.setAttribute('xlink:href', '#icon-sousuo-sousuo');
        }
    })
    ```

1. 一边定宽一边自适应flex布局

    ```html
    <div class="parent">
        <div class="left">左列</div>
        <div class="right">右列</div>
    </div>

    <style>
        .parent {
            display: flex;
        }
        .left {
            width: 100px;
        }
        .right {
            flex: 1;
        }
    </style>
    ```

