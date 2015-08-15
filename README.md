# js-
网上js总结
以下为Web前端开发笔试题集锦之Javascript篇，移步HTML/CSS篇
1, 判断字符串是否是这样组成的，第一个必须是字母，后面可以是字母、数字、下划线，总长度为5-20
var reg = /^[a-zA-Z][a-zA-Z_0-9]{4,19}$/;
reg.test("a1a__a1a__a1a__a1a__");
2，截取字符串abcdefg的efg
var str = "abcdefg";
if (/efg/.test(str)) {
  var efg = str.substr(str.indexOf("efg"), 3);
  alert(efg);
}
3，判断一个字符串中出现次数最多的字符，统计这个次数

//将字符串的字符保存在一个hash table中，key是字符，value是这个字符出现的次数
var str = "abcdefgaddda";
var obj = {};
for (var i = 0, l = str.length; i &lt; l; i++) {
   var key = str[i];
   if (!obj[key]) {
      obj[key] = 1;
   } else {
      obj[key]++;
   }
}

/*遍历这个hash table，获取value最大的key和value*/
var max = -1;
var max_key = "";
var key;
for (key in obj) {
   if (max < obj[key]) {
      max = obj[key];
      max_key = key;
   }
}

alert("max:"+max+" max_key:"+max_key);
4，IE与FF脚本兼容性问题
(1) window.event：
表示当前的事件对象，IE有这个对象，FF没有，FF通过给事件处理函数传递事件对象
(2) 获取事件源
IE用srcElement获取事件源，而FF用target获取事件源
(3) 添加，去除事件
IE：element.attachEvent(“onclick”, function) element.detachEvent(“onclick”, function)
FF：element.addEventListener(“click”, function, true) element.removeEventListener(“click”, function, true)
(4) 获取标签的自定义属性
IE：div1.value或div1[“value”]
FF：可用div1.getAttribute(“value”)
(5) document.getElementByName()和document.all[name]
IE；document.getElementByName()和document.all[name]均不能获取div元素
FF：可以
(6) input.type的属性
IE：input.type只读
FF：input.type可读写
(7) innerText textContent outerHTML
IE：支持innerText, outerHTML
FF：支持textContent
(8) 是否可用id代替HTML元素
IE：可以用id来代替HTML元素
FF：不可以
这里只列出了常见的，还有不少，更多的介绍可以参看JavaScript在IE浏览器和Firefox浏览器中的差异总结
5，规避javascript多人开发函数重名问题
(1) 可以开发前规定命名规范，根据不同开发人员开发的功能在函数前加前缀
(2) 将每个开发人员的函数封装到类中，调用的时候就调用类的函数，即使函数重名只要类名不重复就ok
6，javascript面向对象中继承实现
javascript面向对象中的继承实现一般都使用到了构造函数和Prototype原型链，简单的代码如下：
function Animal(name) {
   this.name = name;
}

Animal.prototype.getName = function() {alert(this.name)}
function Dog() {};
Dog.prototype = new Animal("Buddy");
Dog.prototype.constructor = Dog;
var dog = new Dog();
7，FF下面实现outerHTML
FF不支持outerHTML，要实现outerHTML还需要特殊处理
思路如下：
在页面中添加一个新的元素A，克隆一份需要获取outerHTML的元素，将这个元素append到新的A中，然后获取A的innerHTML就可以了。
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml"> 
<head> 
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" /> 
<title>获取outerHMTL</title> 
<style> 
div{ background:#0000FF;width:100px;height:100px;}  
span{ background:#00FF00;width:100px;height:100px;}  
p{ background:#FF0000;width:100px;height:100px;}  
</style> 
</head> 
<body> 
<div id="a"><span>SPAN</span>DIV</div> 
<span>SPAN</span>
<p>P</p> 
<script type="text/javascript">
function getOuterHTML(id){
 var el = document.getElementById(id);
 var newNode = document.createElement("div");
 document.appendChild(newNode);
 var clone = el.cloneNode(true);
 newNode.appendChild(clone);
 alert(newNode.innerHTML);
 document.removeChild(newNode);
}
getOuterHTML("a");
</script>
</body> 
</html>
8，编写一个方法 求一个字符串的字节长度
假设：
一个英文字符占用一个字节，一个中文字符占用两个字节
function GetBytes(str){
    var len = str.length;
    var bytes = len;
    for(var i=0; i<len; i++){
        if (str.charCodeAt(i) > 255) bytes++;
    }
    return bytes;
}
alert(GetBytes("你好,as"));
9，编写一个方法 去掉一个数组的重复元素
var arr = [1 ,1 ,2, 3, 3, 2, 1];
Array.prototype.unique = function(){
    var ret = [];
    var o = {};
    var len = this.length;
    for (var i=0; i<len; i++){
        var v = this[i];
        if (!o[v]){
            o[v] = 1;
            ret.push(v);
        }
    }
    return ret;
};
alert(arr.unique());
10，写出3个使用this的典型应用
（1）在html元素事件属性中使用，如
<input type=”button” onclick=”showInfo(this);” value=”点击一下”/>
（2）构造函数
function Animal(name, color) {
   this.name = name;
   this.color = color;
}
（3）
<input type="button" id="text" value="点击一下" />
<script type="text/javascript">
var btn = document.getElementById("text");
btn.onclick = function() {
alert(this.value); //此处的this是按钮元素
}
</script>
（4）CSS expression表达式中使用this关键字
<table width="100px" height="100px">
  <tr>
    <td>
    <div style="width:expression(this.parentNode.width);">div element</div>
    </td>
  </tr>
</table>
12，如何显示/隐藏一个DOM元素？
el.style.display = "";
el.style.display = "none";
el是要操作的DOM元素
13，JavaScript中如何检测一个变量是一个String类型？请写出函数实现
String类型有两种生成方式：
(1)Var str = “hello world”;
(2)Var str2 = new String(“hello world”);
function IsString(str){
    return (typeof str == "string" || str.constructor == String);
}
var str = "";
alert(IsString(1));
alert(IsString(str));
alert(IsString(new String(str)));
14，网页中实现一个计算当年还剩多少时间的倒数计时程序，要求网页上实时动态显示“××年还剩××天××时××分××秒”
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
   <title>倒计时</title>
</head>
<body>
<input type="text" value="" id="input" size="1000"/>
<script type="text/javascript">
   function counter() {
      var date = new Date();
      var year = date.getFullYear();
      var date2 = new Date(year, 12, 31, 23, 59, 59);
      var time = (date2 - date)/1000;
      var day =Math.floor(time/(24*60*60))
      var hour = Math.floor(time%(24*60*60)/(60*60))
      var minute = Math.floor(time%(24*60*60)%(60*60)/60);
      var second = Math.floor(time%(24*60*60)%(60*60)%60);
      var str = year + "年还剩"+day+"天"+hour+"时"+minute+"分"+second+"秒";
      document.getElementById("input").value = str;
   }
   window.setInterval("counter()", 1000);
</script>
</body>
</html>
15，补充代码，鼠标单击Button1后将Button1移动到Button2的后面<div> <input type=”button” id =”button1″ value=”1″ onclick=”???”> <input type=”button” id =”button2″ value=”2″ /”> </div>
<div>
   <input type="button" id ="button1" value="1" onclick="moveBtn(this);">
   <input type="button" id ="button2" value="2" />
</div>
<script type="text/javascript">
function moveBtn(obj) {
   var clone = obj.cloneNode(true);
   var parent = obj.parentNode;
   parent.appendChild(clone);
   parent.removeChild(obj);
}
</script>
16，JavaScript有哪几种数据类型
简单：Number，Boolean，String，Null，Undefined
复合：Object，Array，Function
17，下面css标签在JavaScript中调用应如何拼写，border-left-color，-moz-viewport
borderLeftColor
mozViewport
18，JavaScript中如何对一个对象进行深度clone
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
19，如何控制alert中的换行
\n alert(“p\np”);
20，请实现，鼠标点击页面中的任意标签，alert该标签的名称．（注意兼容性）
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml"> 
<head> 
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" /> 
<title>鼠标点击页面中的任意标签，alert该标签的名称</title> 
<style> 
div{ background:#0000FF;width:100px;height:100px;}  
span{ background:#00FF00;width:100px;height:100px;}  
p{ background:#FF0000;width:100px;height:100px;}  
</style> 
<script type="text/javascript">
document.onclick = function(evt){
 var e = window.event || evt;
 var tag = e["target"] || e["srcElement"];
 alert(tag.tagName);
};
</script>
</head> 
<body> 
<div id="div"><span>SPAN</span>DIV</div> 
<span>SPAN</span>
<p>P</p> 
</body> 
</html> 
21，请编写一个JavaScript函数 parseQueryString，它的用途是把URL参数解析为一个对象，如：
var url = “http://witmax.cn/index.php?key0=0&key1=1&key2=2″;
function parseQueryString(url){
   var params = {};
   var arr = url.split("?");
   if (arr.length <= 1)
      return params;
   arr = arr[1].split("&");
   for(var i=0, l=arr.length; i<l; i++){
      var a = arr[i].split("=");
      params[a[0]] = a[1];
   }
   return params;
}

var url = "http://witmax.cn/index.php?key0=0&key1=1&key2=2";
var ps = parseQueryString(url);
alert(ps["key1"]);
22，ajax是什么? ajax的交互模型? 同步和异步的区别? 如何解决跨域问题?
Ajax是多种技术组合起来的一种浏览器和服务器交互技术，基本思想是允许一个互联网浏览器向一个远程页面/服务做异步的http调用，并且用收到的数据来更新一个当前web页面而不必刷新整个页面。该技术能够改进客户端的体验。包含的技术：
XHTML：对应W3C的XHTML规范，目前是XHTML1.0。
CSS：对应W3C的CSS规范，目前是CSS2.0
DOM：这里的DOM主要是指HTML DOM，XML DOM包括在下面的XML中
JavaScript：对应于ECMA的ECMAScript规范
XML：对应W3C的XML DOM、XSLT、XPath等等规范
XMLHttpRequest：对应WhatWG的Web Applications1.0规范（http://whatwg.org/specs/web-apps/current-work/）
AJAX交互模型
 
同步：脚本会停留并等待服务器发送回复然后再继续
异步：脚本允许页面继续其进程并处理可能的回复
跨域问题简单的理解就是因为JS同源策略的限制，a.com域名下的JS无法操作b.com或c.a.com下的对象，具体场景如下：
 
PS：（1）如果是端口或者协议造成的跨域问题前端是无能为力的
(2) 在跨域问题上，域仅仅通过URL的首部来识别而不会尝试判断相同的IP地址对应的域或者两个域是否对应一个IP
前端对于跨域的解决办法：
(1) document.domain+iframe
(2) 动态创建script标签
23，什么是闭包？下面这个ul，如何点击每一列的时候alert其index?
<ul id=”test”>
<li>这是第一条</li>
<li>这是第二条</li>
<li>这是第三条</li>
</ul>
内部函数被定义它的函数的外部区域调用的时候就产生了闭包。
(function A() {
   var index = 0;
   var ul = document.getElementById("test");
   var obj = {};
   for (var i = 0, l = ul.childNodes.length; i < l; i++) {
      if (ul.childNodes[i].nodeName.toLowerCase() == "li") {
         var li = ul.childNodes[i];
         li.onclick = function() {
            index++;
            alert(index);
         }
      }
   }
})();
24，请给出异步加载js方案，不少于两种
默认情况javascript是同步加载的，也就是javascript的加载时阻塞的，后面的元素要等待javascript加载完毕后才能进行再加载，对于一些意义不是很大的javascript，如果放在页头会导致加载很慢的话，是会严重影响用户体验的。
异步加载方式：
(1) defer，只支持IE
(2) async：
(3) 创建script，插入到DOM中，加载完毕后callBack，见代码：
function loadScript(url, callback){
   var script = document.createElement("script")
   script.type = "text/javascript";
   if (script.readyState){ //IE
      script.onreadystatechange = function(){
         if (script.readyState == "loaded" ||
            script.readyState == "complete"){
            script.onreadystatechange = null;
            callback();
         }
      };
   } else { //Others: Firefox, Safari, Chrome, and Opera
      script.onload = function(){
          callback();
      };
   }
   script.src = url;
   document.body.appendChild(script);
}
25，请设计一套方案，用于确保页面中JS加载完全。
var n = document.createElement("script");
n.type = "text/javascript";
//以上省略部分代码
//ie支持script的readystatechange属性(IE support the readystatechange event for script and css nodes)
if(ua.ie){
   n.onreadystatechange = function(){
       var rs = this.readyState;
       if('loaded' === rs || 'complete'===rs){
           n.onreadystatechange = null;
           f(id,url); //回调函数
       }
};
//省略部分代码
//safari 3.x supports the load event for script nodes(DOM2)
   n.addEventListener('load',function(){
       f(id,url);
   });
//firefox and opera support onload(but not dom2 in ff) handlers for
//script nodes. opera, but no ff, support the onload event for link
//nodes.
}else{
   n.onload = function(){
       f(id,url);
   };
}
26，js中如何定义class,如何扩展prototype?
Ele.className = “***”; //***在css中定义，形式如下：.*** {…}
A.prototype.B = C;
A是某个构造函数的名字
B是这个构造函数的属性
C是想要定义的属性的值
27，如何添加html元素的事件,有几种方法.
（1） 为HTML元素的事件属性赋值
（2） 在JS中使用ele.on*** = function() {…}
（3） 使用DOM2的添加事件的方法 addEventListener或attachEvent
28，documen.write和 innerHTML的区别
document.write只能重绘整个页面
innerHTML可以重绘页面的一部分
29，多浏览器检测通过什么？
（1） navigator.userAgent
（2） 不同浏览器的特性，如addEventListener
30，js的基础对象有那些, window和document的常用的方法和属性列出来
String,Number,Boolean
Window:
方法：setInterval,setTimeout,clearInterval,clearTimeout,alert,confirm,open
属性：name,parent,screenLeft,screenTop,self,top,status
Document
方法：createElement,execCommand,getElementById,getElementsByName,getElementByTagName,write,writeln
属性：cookie,doctype,domain,documentElement,readyState,URL,
31，前端开发的优化问题
（1） 减少http请求次数：css spirit,data uri
（2） JS，CSS源码压缩
（3） 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
（4） 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能
（5） 用setTimeout来避免页面失去响应
（6） 用hash-table来优化查找
（7） 当需要设置的样式很多时设置className而不是直接操作style
（8） 少用全局变量
（9） 缓存DOM节点查找的结果
（10） 避免使用CSS Expression
（11） 图片预载
（12） 避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢
32，如何控制网页在网络传输过程中的数据量
启用GZIP压缩
保持良好的编程习惯，避免重复的CSS，JavaScript代码，多余的HTML标签和属性
33，Flash、Ajax各自的优缺点，在使用中如何取舍？
Ajax的优势
（1） 可搜索型
（2） 开放性
（3） 费用
（4） 易用性
（5） 易于开发
Flash的优势
（1） 多媒体处理
（2） 兼容性
（3） 矢量图形 比SVG，Canvas优势大很多
（4） 客户端资源调度，比如麦克风，摄像头

