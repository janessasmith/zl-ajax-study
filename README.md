##怎样实现Ajax技术
* 利用html+css来实现页面，表达信息；
* 用XMLHttpRequest和web服务器进行数据的异步交换；
* 运营js操作DOM，实现动态局部刷新；

##如何使用XMLHttpRequest
* 实例化一个XMLHttpRequest对象(创建XMLHttpRequest对象)
```python
 // 不兼容IE6
var request = new XMLHttpRequest();
```
```python
// 兼容IE6
var request;
if(window.XMLHttpRequest) {
    request = new XMLHttpRequest(); // IE7+及其他现代浏览器
}
else {
    request = new ActiveXObjext("Microsoft.XMLHTTP"); // IE5,IE6
}
```
* 请求XMLHttpRequest

HTTP是一种**无状态的协议**，不建立持久的连接（也就是说服务端不保留连接的相关信息）。
**一个完整的HTTP请求过程**，通常有下面7个步骤：
* 建立TCP连接；
* Web浏览器向Web服务器发送请求命令；
* Web浏览器发送请求头信息；
* Web服务器应答；
* Web服务器发送应答头信息；
* Web服务器向浏览器发送数据；
* Web服务器关闭TCP连接；

一个**HTTP请求**一般由4部分组成：
* HTTP请求的方法或动作，比如是GET还是POST请求 ；
* 正在请求的URL，总得知道请求的地址是什么吧；
* 请求头，包含一些客户端环境信息，身份验证信息等；
* 请求体，也就是请求正文，请求正文中可以包含客户提交的查询字符串信息，表单信息等；

**GET和POST请求**:
* GET: 一般用于**信息获取**；使用URL传递参数；对所发送信息的数量也是有限制，一般在2000个字符；（一般还是安全的，因为GET请求都是用来获取信息而不是修改信息，换句话说都是用来**查询数据**，不会影响数据本身，所以一般不用GET请求来做新建和修改操作；GET发送的信息对任何人都是可见的，所有的变量名和值都显示在URL中，也就是说GET请求是用URL传递参数的）GET请求是**幂等**的，一个GET请求执行一次和一万次的效果都是一样的
* POST：一般用于修改服务器上的资源；对所发送信息的数量无限制；（比GET请求更安全，向服务器发送信息，用于修改服务器资源，一般用于**新建修改和删除存表单和发送数据**，而这个数据并不在URL中显示，对其他人不可见，所有的值都会被嵌入HTTP请求体中，对发送信息的数量没有限制）

一个**HTTP响应**一般由3个部分组成：
* 一个数字和文字组成的状态码，用来显示请求是成功还是失败；
* 响应头，响应头也和请求头一样包含许多有用的信息，例如服务器类型、日期时间、内容类型和长度等；
* 响应体，也就是响应正文；

HTTP状态码由3位数字构成，其中首位数字定义了状态码的类型：
* 1xx：信息类，表示收到Web浏览器请求，正在进一步的处理中；
* 2xx: 成功，表示用户请求被正确接收，理解和处理，例如： 200 OK；
* 3xx: 重定向，表示请求没有成功，客户必须采取进一步的动作；
* 4xx: 客户端错误，表示客户端提交的请求有错误，例如： 404 NOT FOUND，意味着请求中所引用的文档不存在；
* 5xx: 服务器错误，表示服务器不能完成对请求的处理，例如：500；

##XMLHttpRequest如何发送请求
* open(method, url, async): method(发送请求方法)、url(请求地址)、async(请求同步/异步，默认是异步，即async:true)
* send(string)

**注意**.setRequestHeader()是设置HTTP的头信息，告诉Web服务器发送的是什么东西，例如：request.setRequestHeader("Content-type", "application/x-www-form-urlencoded");发送表单信息，.setReuquestHeader()必须放在.open()和.send()方法之间。一般是在POST才会用。
##XMLHttpRequest如何获取服务器响应
获取响应值的方法：
* responseText: 获取字符串形式的响应数据；
* responseXML: 获取XML形式的响应数据(用的比较少，一般都用json)；
* status和statusText: 以数字和文本形式返回HTTP状态码；
* getAllResponseHeader(): 获取所有的响应报头；
* getResponseHeader(): 查询响应中的某个字段的值；

##如何知道服务器是否响应
响应返回和成功的时候得到通知，在实际操作中需要监听XMLHttpRequest上的readyState属性的变化，这个属性的变化代表着服务器响应的变化。
**readyState属性**
* 0：请求未初始化，open还没有调用；
* 1：服务器连接已建立，open已经调用；
* 2：请求已接收，也就是接收到头信息了；
* 3：请求处理中，也就是接收到响应主体了；
* 4：请求已完成，且响应已就绪，也就是响应完成了；
```python
var request = new XMLHttpRequest();
request.open("GET"， “get.php”, "true");
request.send();
request.onreadystatechange = function() {
    // 如果响应成功或响应完成
    if(request.readyState === 4 && request.status === 200) {
        // 做一些事情request.responseText    
    }
}
```
XMLHttpRequest对象的出现分割了同步和异步。XMLHttpRequest出现之前是同步的，出现之后是异步的。
同步：页面请求实时传给服务器，导致必填数据没有填的时候，要回到页面上重新从头填写，耗时长、客户体验差。
异步：在页面必填项写上必填选项，不用通过传给服务器判断必填内容是否已经填写完整，耗时短、用户体验强。
##什么是json
* json: JavaScript对象表示法(JavaScript Object Notation)；
* json是**存储和交换文本信息的语法**，类似XML。它采用**键值对**的方式来组织，易于人们阅读和编写，同时也易于机器解析和生成；
* json是独立于语言的，也就是说不管什么语言，都可以解析json，只需要按照json的规则来就行；

##json和XML比较
* json的长度比xml格式更短小；
* json读写的速度更快；
* json可以使用JavaScript内建的方法直接进行解析，转换成JavaScript对象，非常方便；

##json语法规则
* json数据的**书写格式**是：名称/值对；名称/值对组合中的名称写在前面(在双引号中)，值对写在后面(同样在双引号中)，中间用冒号隔开，如："name" : "janessasmith"
* json的值可以是下面这些类型：
    * 数字(整数或浮点数)，比如：123，1.23
    * 字符串(在双引号中)
    * 逻辑值(true或false)
    * 数组(在方括号中)
    * 对象(在花括号中)
    * null
    
    ```python
    // json字符串，大括号括起来表示是一个json对象，里面有一个值对名称为staff，它的值是一个数组
    {
        "staff": [
            {"name": "janessa", "age": 22},
            {"name": "smith", "age": 23},
            {"name": "janessasmith", "age": 24}        
        ]
    }
    ```
##json格式如何在JS中解析
json解析有2种方式：
* eval和JSON.parse；
* **在代码中使用eval是很危险的！**特别是用它执行第三方的json数据(其中可能包含恶意代码)时，尽可能使用JSON.parse()方法解析字符串本身，该方法还可以捕捉json中的语法错误；

第一种方式eval:
```python
// 定义一个json字符串
var jsondata = '{"staff": [{"name": "janessa", "age": 22}, {"name": "smith", "age": 23}, {"name": "janessasmith", "age": 24}]}';
// json解析-使用eval()
var jsonobj = eval('(' + jsondata + ')');
alert(jsonobj.staff[0].name);
```
第二种方式JSON.parse:
```python
var jsondata = '{"staff": [{"name": "janessa", "age": 22}, {"name": "smith", "age": 23}, {"name": "janessasmith", "age": 24}]}';
// json解析-使用JSON.parse()
var jsonobj = JSON.parse(jsondata);
alert(jsonobj.staff[0].name);
```
eval()和JSON.parse()对比
```python
var jsondata = '{"staff": [{"name": "janessa", "age": alert(22)}, {"name": "smith", "age": 23}, {"name": "janessasmith", "age": 24}]}';
var jsonobj = eval('(' + jsondata + ')');
alert(jsonobj.staff[0].name);  // 会先弹出alert(22)的弹窗再弹出json的弹窗
```
```python
var jsondata = '{"staff": [{"name": "janessa", "age": alert(22)}, {"name": "smith", "age": 23}, {"name": "janessasmith", "age": 24}]}';
var jsonobj = JSON.parse(jsondata);
alert(jsonobj.staff[0].name);  // 报错，不会弹出alert(22)的弹窗
```
eval内部：加上圆括号的目的是迫使eval函数在处理JavaScript代码的时候强制将括号内的表达式（expression）转化为对象，而不是作为语句（statement）来执行
**总结：**用eval执行不会去看json字符串是否合法，而且js中的方法会直接执行，这是非常的危险
在线json校验工具：**JSONLint**

##用jQuery实现Ajax
* jQuery.ajax([settings])
    * type: 类型，“POST”或"GET"，默认为"GET"；
    * url: 发送请求的地址；
    * data: 是一个对象，连同请求发送到服务器的数据，主要是POST使用；
    * dataType: 一般都采用json格式，可以设置为"json"；
    * success: 是一个方法，请求成功后的**回调函数**；传入返回后的数据，以及包含成功代码的字符串；
    * error: 是一个方法，请求失败时调用此函数；传入XMLHttpRequest对象；
    
##跨域
一个域名地址的组成：
* http://(协议)www(子域名).abc.com(主域名):8080(端口号)/scripts/jquery.js(请求资源地址)；
* **当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域**；
* 不同域之间相互请求资源，就算作“跨域”；

**注意**：默认端口号是80，所有端口号可以省略；默认是http协议，所以http也可以省略；子域名可以有多级；
JavaScript处于安全方面的考虑，不允许跨域调用其他页面的对象；
**跨域**：简单理解就是因为JavaScript同源策略的限制，a.com域名下的js无法操作b.com或者c.a.com域名下的对象；
##处理跨域的方法
* 通过在同域名的web服务器端创建一个**代理**（后台处理）；
* **JSONP**可用于解决主流浏览器的跨域数据访问的问题；
```python
// a域名去声明，b域名去调用
<script type="text/javascript">
    function jsonp(json) {
        alert(json["name"]);   
    }
</script>
<script src="http://www.bbb.com/jsonp.js"></script>
// 在www.bbb.com页面中：
// 注意：jsonp写法是jsonp({参数})
jsonp({"name": "janessasmith", "age": 24});
```
**注意**：jsonp只对GET请求做改造，没有对POST做处理，也就是说只能对GET请求起作用，jsonp是不支持POST请求；这也是jsonp的局限性；
* HTML5提供的XMLHttpRequest Level2(**XHR2**)已经实现了跨域访问以及其他的一些新功能；
    * IE10以下的版本都不支持；
    * 在服务器端做一些小小的改造即可：
        * `` header("Access-Control-Allow-Origin: *");`` // *就是所有域都可以访问
        * ``header("Acess-Control-Allow-Methods: POST, GET");``