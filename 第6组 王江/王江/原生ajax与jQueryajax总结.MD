Ajax及其工作原理
AJAX 是一种与服务器交换数据无需刷新网页的技术，最早由Google公司在谷歌地图里使用，并迅速风靡。

AJAX是不能跨域的，如需跨域，可以使用document.domain='a.com'；或者使用服务器代理，代理XMLHttpRequest文件

AJAX是基于现有的Internet标准，并且联合使用它们：

XMLHttpRequest 对象 (异步的与服务器交换数据)
JavaScript/DOM (信息显示/交互)
CSS (给数据定义样式)
XML (作为转换数据的格式)

创建 XMLHttpRequest 对象
所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。

创建Ajax对象：

//IE6以上
var oAjax = new XMLHttpRequest();

//IE6
var oAjax =new ActiveXObject("Microsoft.XMLHTTP")
连接服务器
oAjax.open(方法,url,是否异步)
我们都知道，Ajax即“Asynchronous Javascript And XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。所以，Ajax天生就是工作在异步模式的（异步为true，同步false）

同步和异步
同步是指：发送方发出数据后，等接收方发回响应以后才发下一个数据包的通讯方式。
异步是指：发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式。
（简单点说：同步就是只能一件一件事的来做，而异步就是可以多件事同时进行）

发送请求send()
```
<script type="text/javascript">
function getDoc(){
    var xmlhttp;
    if(window.xmlhttpRequest){
        xmlhttp=new XMLHttpRequest();
    }
    else{
        xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");//for IE6
    }
    xmlhttp.onreadystatechange = function(){
        if(xmlhttp.readyState==4&&xmlhttp.status==200){
            document.getElementById("￼myId").innerHTML=xmlhttp.responseText;
        }
    }
    xmlhttp.open("￼GET", index.php,true);
    xmlhttp.send();
}
</script>
</head>
<body>
    <button type="button" onclick="getDoc()">请求数据</button>
</body>
```


GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

无法使用缓存文件（更新服务器上的文件或数据库）
向服务器发送大量数据（POST 没有数据量限制）
发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠
接收返回信息
oAjax.onreadystatechange = function(){   //当请求状态改变时要调用的事件处理器

    alert(oAjax.readystate);

}
只要readyState属性的值发生变化时，便会触发一次readyStatechange事件。可以利用这个事件来检测每次状态变化后readyState的值。通常，我们只对readyState值为4的阶段感兴趣，因为这时所有数据都已经就绪，不过，必须在调用open()之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。下面来看一个例子：
```
var xhr = createXHR();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
            alert(xhr.statusText);
        } else {
            alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("get", "example.txt", true);
xhr.send(null);
```

XHR对象
XMLHttpRequest这个对象的属性：

它的属性有：

onreadystatechange  每次状态改变所触发事件的事件处理程序。

responseText     从服务器进程返回数据的字符串形式。
responseXML    从服务器进程返回的DOM兼容的文档数据对象。

status           从服务器返回的数字代码，比如常见的404（未找到）和200（已就绪）

status Text       伴随状态码的字符串信息
当XHR对象把一个HTTP请求发送到服务器的过程中会经历几个状态，直到请求被处理，然后才接收一个回应。readyState就是XHR请求的状态属性，它本身有5个属性值：

0（未初始化）还没有调用open()方法
1（载入）已调用send()方法，正在发送请求
2(载入完成)send()方法完成，已收到全部响应内容
3(解析)正在解析响应内容
4（完成）响应内容解析完成，可以再客户端使用了
http状态码
1字头：消息。这一类型的状态码，代表请求已被接受，需要继续处理。
2字头：成功。这一类型的状态码，代表请求已成功被服务器接收、理解、并接受。
3字头：重定向。这类状态码代表需要客户端采取进一步的操作才能完成请求。
4字头：客户端错误。这类状态码代表了客户端看起来可能发生错误，妨碍了服务器的处理。
5字头：服务器错误。这类状态码代表了服务器在处理请求的过程中有错误或者异常状态发生。

status和statusText
　　statusText是响应返回的文本信息，仅当readyState值为3或4的时候才能使用。当readyState为其它值时视图存取statusText属性将引发异常。

XHR的方法
方法	描述
abort()	导致当前正在执行的请求被取消
getAllResponseHeaders()	返回包含所有响应头的名称和值的单个字符
getResponseHeader(name)	返回响应头中指定的名称和值
open(method,url,async,username,pwd)	设置HTTP方法（get或post）等
send(content)	发出带有指定主体内容的请求
setRequestHeader(name,value)	使用指定的名称和值设置请求头
```
<script type="text/javascript">
        var oAjax =oAjax();
        alert(oAjax.readyState);//弹出"0"
        oAjax.open("get","index.html",true);
        alert(oAjax.readyState);//弹出"1"
        oAjax.send(null);
        alert(oAjax.readyState);//IE下弹出4,而firefox是2
        //可以通过readystatechange事件监听
        oAjax = XHR();
        oAjax.onreadystatechange = function () {
            alert(oAjax.readyState);//Firefox下依次是1,2,3,4但最后还会再来个1
            //IE下则是1,1,3,4
        };
        oAjax.open("get","index.txt",true);
        oAjax.send(null);
</script>
```

$.ajax()方法对象参数列表
参数	类型	说明
url	string	发送请求的地址
type	string	请求方法，默认GET
timeout	Number	设置请求超时的时间
data	Object或String	发送到服务器的对象，键值对字符串或对象
datatype	String	返回的数据类型，比如html，XML，json等
success	Function	请求成功后调用的回调函数
complete	Function	请求完成后调用的回调函数
error	Function	请求失败后调用的回调函数
```
$(function () {
    $("input").click(function () {
        $.ajax({
            type : "POST",
            url: 'test.php',
            data:{
                url: 'baidu.com'
            },
            success : function(response,status,xhr){
                $('#box').html(response);
            }
        })
    })
});
```

表单序列化
Ajax用的最多的地方莫过于表单操作，而传统的表单操作是通过submit提交将数据传输到服务器，如果使用Ajax异步处理
的话，我们需要将每个表单元素获取才能提交，这样工作效率就大大降低
```
<body>
<form action="">
    用户名: <input type="text" name="user" />
    邮件: <input type="text" name="email" />
    <input type="button" value="提交"/>
</form>
<div id="box"></div>
<script type="text/javascript">
    $(function () {
        $("form input[type=button]").click(function () {
            $.ajax({
                type: 'POST',
                url: 'test.php',
                data: {
                    user: $('form input[name=user]').val(),
                    email: $("form input[name=email]").val(),
                },
                success: function(response){
                    $("#box").html(response);
                }
            })
        })
    });
</script>
</body>
```

表单元素特别多的情况下应采用表单序列化，即data:$(“form”).serialize();//得到的是字符串键值对，并且对url进行编码

serialize()方法不但可以序列化表单内的元素，还可以直接获取单选框，复选框和下拉列表框等内容


```
<body>
<form action="">
    用户名: <input type="text" name="user" />
    邮件: <input type="text" name="email" />
    <input type="button" value="提交"/>
    <input type="radio" name="sex" value="男"/>男
    <input type="radio" name="sex" value="女"/>女
    <input type="button" value="submit"/>
</form>
<div id="box"></div>
<script>
    $(function () {
        $("form input[name=sex]").click(function () {
            $('#box').html(decodeURIComponent($(this).serialize()));
        })
    })
</script>
</body>
```
