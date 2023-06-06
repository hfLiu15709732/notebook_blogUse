# Ajax与promise方面总结

## 1.ajax方面：

### 1.0: ajax概述：



Ajax用于浏览器与服务器通信而无需刷新整个页面，服务器将不再返回整个页面，而是返回少量数据，通过JavaScript DOM更新一部分节点。期间数据传输可采用xml，json等格式，Ajax最早用于谷歌的搜索提示。

其实不刷新整个页面便可与服务器通信的方法有很多，比如Flash，Java applet，iframe等，但Ajax是目前最为常见的一种。

我们可以使用JavaScript扩展对象XMLHttpRequest实现Ajax，对于这种方法在这里不做介绍，下面直接了解jQuery实现Ajax的几种方法。


### 1.1：xhr理论概括：

`XMLHttpRequest`（简称 `xhr`）是浏览器提供的 `Javascript` 对象，通过它，可以**请求服务器上的数据资源**。之前所学的 `jQuery` 中的 Ajax 函数，就是基于 `xhr` 对象封装出来的



### 1.2：原生XHR发起GET请求：

步骤：

- 创建 `xhr` 对象
- 调用 `xhr.open()` 函数
- 调用 `xhr.send()` 函数
- 监听 `xhr.onreadystatechange` 事件



```javascript
// 1. 创建 XHR 对象
var xhr = new XMLHttpRequest()
// 2. 调用 open 函数
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks')
// 3. 调用 send 函数
xhr.send()
// 4. 监听 onreadystatechange 事件
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
     // 获取服务器响应的数据
     console.log(xhr.responseText)
   }
}
```



**`readyState` 属性：**

`XMLHttpRequest` 对象的 `readyState` 属性，用来表示**当前 `Ajax` 请求所处的状态**。每个 `Ajax` 请求必然处于以

下状态中的一个：

| 值   | 状态             | 描述                                               |
| ---- | ---------------- | -------------------------------------------------- |
| 0    | UNSET            | 对象被创建，但没有调用open方法                     |
| 1    | OPEND            | open（）函数被调用                                 |
| 2    | HEADERS-REVEIVED | send()函数被调用，响应头被接收                     |
| 3    | LOADING          | 数据接受中，此时response已包含部分数据             |
| 4    | DONE             | Ajax请求完成，可表明该程序是否彻底成功还是彻底失败 |



**携带参数问题：**

这种在 URL 地址后面拼接的参数，叫做**查询字符串**。

例如：

```ABAP
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1')
```

**定义：**查询字符串（URL 参数）是指在 URL 的末尾加上用于向服务器发送信息的字符串（变量）。

**格式：**将英文的 ? 放在URL 的末尾，然后再加上 参数＝值 ，想加上多个参数的话，使用 & 符号进行分隔。以

这个形式，可以将想要发送给服务器的数据添加到 URL 中。

**带多个URL参数例子：**

```ABAP
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1&name="lhf"&password="lhf345"')
```

**URL编码与解码**

理论：

操作使用：



### 1.3：原生XHR发起POST请求：



**步骤：**

- 创建 `xhr` 对象
- 调用 `xhr.open()` 函数
- **设置 Content-Type 属性**（固定写法分为URL参数与JSON数据）
- 调用 `xhr.send()` 函数，**同时指定要发送的数据**
- 监听 `xhr.onreadystatechange` 事件



```javascript
// 1. 创建 xhr 对象
var xhr = new XMLHttpRequest()
// 2. 调用 open 函数
xhr.open('POST', 'http://www.liulongbin.top:3006/api/addbook')
// 3. 设置 Content-Type 属性（固定写法）
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
// 4. 调用 send 函数
xhr.send('bookname=水浒传&author=施耐庵&publisher=上海图书出版社')
// 5. 监听事件
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.responseText)
  }
}

//下面是传输JSON数据的
var xhr = new XMLHttpRequest();
xhr.open("post","http://127.0.0.1:8000/post");
xhr.setRequestHeader("content-Type","application/json");
xhr.send(JSON.stringify({name:"js",age: 18}));
xhr.onreadystatechange = function() {
    if(xhr.readyState === 4 && xhr.status ===200){
        console.log(xhr.responseText);
    }
}
xhr.onerror = function (error) {
    console.log(error)
}
输出：{"name":"js","age":"18"}

```



**post请求头的常见数据格式**

**1、application/json（JSON数据格式）**

```javascript
xhr.setRequestHeader("Content-type","application/json; charset=utf-8");
```

这种类型是我们现在最常用的，越来越多的人把它作为请求头，用来告诉服务端消息主体是序列化后的 JSON 字符串。由于 JSON 规范的流行，除了低版本 IE 之外的各大浏览器都原生支持 JSON.stringify，服务端语言也都有处理 JSON 的函数，使用 JSON 不会遇上什么麻烦。
**2、application/x-www-form-urlencoded**

```javascript
xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded; charset=utf-8");
```

这应该是最常见的 POST 提交数据的方式了。浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据

**3、multipart/form-data **

```javascript
xhr.setRequestHeader("Content-type", "multipart/form-data; charset=utf-8");
```

这又是一个常见的 POST 数据提交的方式。我们使用表单上传文件时，必须让 form 的 enctyped 等于这个值

**4、text/xml**

```javascript
xhr.setRequestHeader("Content-type", "text/xml; charset=utf-8");
```

它是一种使用 HTTP 作为传输协议，XML 作为编码方式的远程调用规范，这种方式现在不常用







数据交换格式，就是**服务器端**与**客户端**之间进行**数据传输与交换的格式**

前端领域，经常提及的两种数据交换格式分别是 `XML` 和 `JSON`。其中 `XML` 用的非常少，所以，我们重点要学

习的数据交换格式就是 `JSON`



JSON语法注意事项
① 属性名必须使用双引号包裹

② 字符串类型的值必须使用双引号包裹

③ JSON 中不允许使用单引号表示字符串

④ JSON 中不能写注释

⑤ JSON 的最外层必须是对象或数组格式

⑥ 不能使用 undefined 或函数作为 JSON 的值

**JSON 的作用：**在计算机与网络之间存储和传输数据。

**JSON 的本质：**用字符串来表示 Javascript 对象数据或数组数据



**get的JSON应用场景：**

```javascript
var xhr = new XMLHttpRequest()
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks')
xhr.send()
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.responseText)
    console.log(typeof xhr.responseText)
    var result = JSON.parse(xhr.responseText)
    console.log(result)
  }
}

```

### 1.4 原生发送get post总结：

```javascript
/*

两种请求方式（get/post）相比：

1、get请求方式中不用设置请求头信息

     post请求方式必须要设置：（普通类型  or  json类型）

     aj.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

     aj.setRequestHeader('Content-Type', 'application/json');

2、get请求方式中将请求参数挂于 【请求地址?】 之后，多个参数用&连接；send()无参数

      post请求方式将请求参数置于send()中传递（遇到json类型需要转为字符串再传递）

      因为send()方法只能传递字符串类型的数据---借助JSON.stringify()方法转换类型

3、服务器端对get的响应无需配置用什么方法去解析什么类型的参数，req.query直接获取

      对post的响应需要配置：（当然，用于解析post的body-parser第三方模块不能少）

                对于普通类型：app.use(bodyParser.urlencoded({ extended: false }));

                对于json类型：app.use(bodyParser.json());

*/
```





### 1.5 jquery发起请求：



#### 1.5.1 数据概述：

当确定数据传输采用json格式后，下面就需要考虑序列化问题了。

网络中传输的都是文本字符串（其实是二进制比特流，这里方便理解），因此在向网络通道中写入数据时，都需要先序列化json对象为文本字符串。而从网络通道中读取数据时，都需要反序列化文本字符串为json对象。在Python中json.dumps用于序列化，json.loads用于反序列化。

如果确定数据格式是json，JS也需对服务器返回的数据进行反序列化，即把json样式的字符串变成json对象。

```javascript
var json_str = '{"result": "hello, world!"}';
var json_object = eval("(" + json_str + ")");  // 法一，使用eval函数，注意括号
var json_object = jQuery.parseJSON(json_str);  // 法二，使用jQuery的parseJSON函数
 
alert(json_object.result);                     // 反序列化成功，输出结果
```





#### 1.5.2 $.ajax()方法：

形式：$.ajax({name:val, name:val,...});
可选字段：
1）url：链接地址，字符串表示
2）data：需发送到服务器的数据，GET与POST都可以，格式为{A: '...', B: '...'}
3）type："POST" 或 "GET"，请求类型
4）timeout：请求超时时间，单位为毫秒，数值表示
5）cache：是否缓存请求结果，bool表示
6）contentType：内容类型，默认为"application/x-www-form-urlencoded"
7）dataType：服务器响应的数据类型，字符串表示；当填写为json时，回调函数中无需再对数据反序列化为json
8）success：请求成功后，服务器回调的函数
9）error：请求失败后，服务器回调的函数
10）complete：请求完成后调用的函数，无论请求是成功还是失败，都会调用该函数；如果设置了success与error函数，则该函数在它们之后被调用
11）async：是否异步处理，bool表示，默认为true；设置该值为false后，JS不会向下执行，而是原地等待服务器返回数据，并完成相应的回调函数后，再向下执行
12）username：访问认证请求中携带的用户名，字符串表示
13）password：返回认证请求中携带的密码，字符串表示
不知道将最后两个放到data中去，是不是密码会以明文展示，因没有尝试过，这里不敢下结论。

```javascript
$.ajax({
    url: "/greet",
    data: {name: 'jenny'},
    type: "POST",
    dataType: "json",
    success: function(data) {
        // data = jQuery.parseJSON(data);  //dataType指明了返回数据为json类型，故不需要再反序列化
        ...
    }
});
    
     <script type="text/javascript">
        function login1(){
            $.ajax({
                //${pageContext.request.contextPath}用于取后端方法的绝对路径的项目名
                url: "${pageContext.request.contextPath}/user/returnJson",
                type: "GET",
                data:'{name: 'James'}', //必须是字符串格式
                contentType:"application/json", //指定内容格式
                dataType:json,
                success: function(data) {  //括号里的data是服务器返回的数据
                    console.log(data);
                    document.getElementById("myDiv").innerText=data["name"];
                }
            });
        }
    </script>

```



#### 1.5.3 $.post()方法：



该方法使用POST方式执行Ajax请求，从服务器加载数据。

形式：$.post(url, data, func, dataType);
可选参数：
1）url：链接地址，字符串表示
2）data：需要发送到服务器的数据，格式为{A: '...', B: '...'}
3）func：请求成功后，服务器回调的函数；function(data, status, xhr)，其中data为服务器回传的数据，status为响应状态，xhr为XMLHttpRequest对象，个人感觉关注data参数即可
4）dataType：服务器返回数据的格式

```javascript
$.post(
    "/greet",
    {name: 'Brad'},
    function(data) {
        ...
    },
    "json"
);
```



#### 1.5.4 $.get()方法：

该方法使用GET方式执行Ajax请求，从服务器加载数据。

形式：*$.get(url, data, func, dataType);*
其各个参数所示意义与$.post()一致，在此不再列出，唯一区别就是请求类型是GET。

```javascript
$.get(
    "/greet",
    {name: 'Brad'},
    function(data) {
        ...
    },
    "json"
);
```



### 1.6 axios发起请求：

### 1.7async 与awit在其中的应用：
