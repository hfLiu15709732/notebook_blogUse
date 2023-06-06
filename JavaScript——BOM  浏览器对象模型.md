## JavaScript——BOM  浏览器对象模型

### 0：Window对象：



#### 0.1window 对象中的方法

下表中列举了 window 对象中提供的方法及其描述：



| 方法               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| alert()            | 在浏览器窗口中弹出一个提示框，提示框中有一个确认按钮         |
| atob()             | 解码一个 base-64 编码的字符串                                |
| btoa()             | 创建一个 base-64 编码的字符串                                |
| blur()             | 把键盘焦点从顶层窗口移开                                     |
| clearInterval()    | 取消由 setInterval() 方法设置的定时器                        |
| clearTimeout()     | 取消由 setTimeout() 方法设置的定时器                         |
| close()            | 关闭某个浏览器窗口                                           |
| confirm()          | 在浏览器中弹出一个对话框，对话框带有一个确认按钮和一个取消按钮 |
| createPopup()      | 创建一个弹出窗口，注意：只有 IE 浏览器支持该方法             |
| focus()            | 使一个窗口获得焦点                                           |
| getSelection()     | 返回一个 Selection 对象，对象中包含用户选中的文本或光标当前的位置 |
| getComputedStyle() | 获取指定元素的 CSS 样式                                      |
| matchMedia()       | 返回一个 MediaQueryList 对象，表示指定的媒体查询解析后的结果 |
| moveBy()           | 将浏览器窗口移动指定的像素                                   |
| moveTo()           | 将浏览器窗口移动到一个指定的坐标                             |
| open()             | 打开一个新的浏览器窗口或查找一个已命名的窗口                 |
| print()            | 打印当前窗口的内容                                           |
| prompt()           | 显示一个可供用户输入的对话框                                 |
| resizeBy()         | 按照指定的像素调整窗口的大小，即将窗口的尺寸增加或减少指定的像素 |
| resizeTo()         | 将窗口的大小调整到指定的宽度和高度                           |
| scroll()           | 已废弃。您可以使用 scrollTo() 方法来替代                     |
| scrollBy()         | 将窗口的内容滚动指定的像素                                   |
| scrollTo()         | 将窗口的内容滚动到指定的坐标                                 |
| setInterval()      | 创建一个定时器，按照指定的时长（以毫秒计）来不断调用指定的函数或表达式 |
| setTimeout()       | 创建一个定时器，在经过指定的时长（以毫秒计）后调用指定函数或表达式，只执行一次 |
| stop()             | 停止页面载入                                                 |
| postMessage()      | 安全地实现跨源通信                                           |



#### 0.2window 对象中的属性

下表中列举了 window 对象中提供的属性及其描述：



| 属性           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| closed         | 返回窗口是否已被关闭                                         |
| defaultStatus  | 设置或返回窗口状态栏中的默认文本                             |
| document       | 对 Document 对象的只读引用                                   |
| frames         | 返回窗口中所有已经命名的框架集合，集合由 Window 对象组成，每个 Window 对象在窗口中含有一个 <frame> 或 <iframe> 标签 |
| history        | 对 History 对象的只读引用，该对象中包含了用户在浏览器中访问过的 URL |
| innerHeight    | 返回浏览器窗口的高度，不包含工具栏与滚动条                   |
| innerWidth     | 返回浏览器窗口的宽度，不包含工具栏与滚动条                   |
| localStorage   | 在浏览器中以键值对的形式保存某些数据，保存的数据没有过期时间，会永久保存在浏览器中，直至手动删除 |
| length         | 返回当前窗口中 <iframe> 框架的数量                           |
| location       | 引用窗口或框架的 Location 对象，该对象中包含当前 URL 的有关信息 |
| name           | 设置或返回窗口的名称                                         |
| navigator      | 对 Navigator 对象的只读引用，该对象中包含当前浏览器的有关信息 |
| opener         | 返回对创建此窗口的 window 对象的引用                         |
| outerHeight    | 返回浏览器窗口的完整高度，包含工具栏与滚动条                 |
| outerWidth     | 返回浏览器窗口的完整宽度，包含工具栏与滚动条                 |
| pageXOffset    | 设置或返回当前页面相对于浏览器窗口左上角沿水平方向滚动的距离 |
| pageYOffset    | 设置或返回当前页面相对于浏览器窗口左上角沿垂直方向滚动的距离 |
| parent         | 返回父窗口                                                   |
| screen         | 对 Screen 对象的只读引用，该对象中包含计算机屏幕的相关信息   |
| screenLeft     | 返回浏览器窗口相对于计算机屏幕的 X 坐标                      |
| screenTop      | 返回浏览器窗口相对于计算机屏幕的 Y 坐标                      |
| screenX        | 返回浏览器窗口相对于计算机屏幕的 X 坐标                      |
| sessionStorage | 在浏览器中以键值对的形式存储一些数据，数据会在关闭浏览器窗口或标签页之后删除 |
| screenY        | 返回浏览器窗口相对于计算机屏幕的 Y 坐标                      |
| self           | 返回对 window 对象的引用                                     |
| status         | 设置窗口状态栏的文本                                         |
| top            | 返回最顶层的父窗口                                           |





------

**javaScript有三种数据存储方式，分别是：**

- sessionStorage

- localStorage

- cookier

  

### 1.本地存储

------



#### 1.1-- sessionStorage

##### 基本使用



**sessionStorage仅在当前会话下有效，关闭页面或浏览器后被清除；**
setItem(key,value) 设置数据
getItem(key) 获取数据
removeItem(key) 移除数据
clear() 清除所有值

```javascript
<script>
    // 添加数据
   window.sessionStorage.setItem("name","李四")
   window.sessionStorage.setItem("age",18)
    // 获取数据
    console.log(window.sessionStorage.getItem("name")) // 李四
    // 清除某个数据
    window.sessionStorage.removeItem("gender")
    // 清空所有数据
    window.sessionStorage.clear()
</script>


```



------



#### 1.2--localStorage：



**localStorage 是 HTML5 标准中新加入的技术，用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除；**

**localStorage和sessionStorage最大一般为5MB，仅在客户端（即浏览器）中保存，不参与和服务器的通信；**

##### 基本使用：

- setItem(key,value) 设置数据

- getItem(key) 获取数据

- removeItem(key) 移除数据

- clear() 清除所有值

- 

  ```javascript
  <script>
      // 添加数据
      window.localStorage.setItem("name","张三")
      window.localStorage.setItem("age",20)
      window.localStorage.setItem("gender","男")
      // 获取数据
      console.log(window.localStorage.getItem("name")) // 张三
      // 清除某个数据
      window.localStorage.removeItem("gender")
      // 清空所有数据
      window.localStorage.clear()
  </script>
  
  
  ```



##### 扩展：

localStorage也可以手动设置失效时间,大概思路是存储的值加一个时间戳，下次取值时验证时间戳,localStorage只能存储字符，**存入时将对象转为json字符串,读取时也要解析.**



```javascript
Storage.prototype.setExpire = (key, value, expire) => {
	let obj = {
	data: value,
	time: Date.now(),
	expire: expire
	};
	//localStorage 设置的值不能为对象,转为json字符串
	localStorage.setItem(key, JSON.stringify(obj));
}

Storage.prototype.getExpire = key => {
    let val = localStorage.getItem(key);
    if (!val) {
        return val;
    }
    val = JSON.parse(val);
    if (Date.now() - val.time > val.expire) {
        localStorage.removeItem(key);
        return null;
    }
    return val.data;
}

```



------



#### 1.3--cookier



##### 简介：

Cookie 是一些数据, 存储于你电脑上的文本文件中，用于存储 web 页面的用户信息
Cookie 数据是以键值对的形式存在的，每个键值对都有过期时间。如果不设置时间，浏览器关闭，cookie就会消失，当然用户也可以手动清除cookie
Cookie每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题
Cookie内存大小受限，一般每个域名下是4K左右，每个域名大概能存储50个键值对

##### 基本操作：

通过访问document.cookie可以对cookie进行创建，修改与获取。
默认情况下，cookie 在浏览器关闭时删除，你还可以为 cookie的某个键值对 添加一个过期时间
如果设置新的cookie时，某个key已经存在，则会更新这个key对应的值，否则他们会同时存在cookie中



```javascript
<script>
    // 设置cookie
    document.cookie = "username=orochiz"
    document.cookie = "age=20"
 
    // 读取cookie
    var msg = document.cookie
    console.log(msg) // username=orochiz; age=20
 
    // 添加过期时间（单位：天）
    var d = new Date() // 当前时间 2019-9-25
    var days = 3       // 3天
    d.setDate(d.getDate() + days)
    document.cookie = "username=orochiz;"+"expires="+d
 
    // 删除cookie （给某个键值对设置过期的时间）
    d.setDate(d.getDate() - 1)
    console.log(document.cookie)
</script>


```



#### 1.4--对比：



##### ①传递方式不同

cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。

sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

##### ②数据大小不同

cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。

sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

##### ③数据有效期不同

sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；

localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；

cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

##### ④作用域不同

sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；

localStorage 在所有同源窗口中都是共享的；

cookie也是在所有同源窗口中都是共享的。

Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。

Web Storage 的 api 接口使用更方便。



