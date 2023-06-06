# 使用cheerio配合Node.js实现爬虫

![](https://s3.amazonaws.com/oodles-blogs/blog-images/9cf44211-88ce-48f9-805f-e624334a67ab.jpeg)

## 1.简介：

> **cheerio是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方**



## 2.特点：

1. **简而言之**：让你在==服务器端和html==愉快的玩耍
2. **熟悉的语法**：cheerio实现了jQuery的一个子集，去掉了jQuery中所有与DOM不一致或者是用来填浏览器的坑的东西，重现了jQuery最美妙的API
3. **快到没朋友**：cheerio使用了及其简洁而又标准的DOM模型， 因此对文档的转换，操作，渲染都极其的高效。基本的端到端测试显示它的速度至少是JSDOM的8倍
4. **极其灵活**：cheerio使用了`@FB55`编写的非常兼容的`htmlparser2`，因此它可以解析几乎所有的HTML和XML



## 3.具体相关API与实现

> **相关功能太多，之后开一个新的文章具体说一说**



## 4.主要应用场景：爬虫



### 主代码



**基础够的话，直接看代码即可（注释写的很ok了）**



```js
var express = require('express');//建立建议的接收处理服务器
var cheerio = require('cheerio');//分析爬取出的数据，主要使用api可借鉴jQuery
const axios = require('axios');//发送请求要用

var app = express();//创建服务器实例

app.get('/r23',async function (req, res, next) {
// 建立路由

let items = [];
let ide=1;



for(let i=1;i<19;i++){


    await axios.get(`https://cpu-compare.com/zh-CN/benchmark/cinebenchr23?page=${i}`)
    .then(function (val) {

    // val.data里面存储着网页的 html 内容
    //将它传给 cheerio.load 之后
    // 就可以得到一个实现了 jquery 接口的变量
    // 之后直接使用jQuery的相关API即可了
    let $ = cheerio.load(val.data);//创建接口实例
    $('.rating-table-content').each((idx, element)=>{
    //idx是本次爬取循环的index
    //分析将要爬取数据的所在网页位置


    var $element = $(element);//创建解析所用到的实例


    let perArr=$element.find('.t4').text();
    perArr=perArr.split("\n");
    //t4类名的数据存在同类名多个标签，需要先行处理后存储

    items.push({
    id:ide++,
    href: $element.find(".t1-flex").find('a').attr('href'),
    title: $element.find(".t1-flex").find('a').find('span').text(),
    R23多核: perArr[2],
    R23单核: perArr[4],
    CPU核心数: perArr[6],
    CPU主频:$element.find(".t5").text().split("\n")[2],
    CPU_TDP:$element.find(".t6").text().split("\n")[2],
    发布时间:$element.find(".t7 span").text(),
    });//数据解析并存储到数组中去




    });
    })
    .catch(()=>{
        console.log("出错啦！！！");
    })
    //爬虫部分代码


}//由于该网页数据为分页设计，所以采取循环爬取的策略


res.send(items);//发送爬取出的数据发给前台

});




app.get('/r20',async function (req, res, next) {
    // 建立路由
    
    let items = [];
    let ide=1;
    
    
    
    for(let i=1;i<28;i++){
    
    
        await axios.get(`https://cpu-compare.com/zh-CN/benchmark/cinebenchr20?page=${i}`)
        .then(function (val) {
    
        // val.data里面存储着网页的 html 内容
        //将它传给 cheerio.load 之后
        // 就可以得到一个实现了 jquery 接口的变量
        // 之后直接使用jQuery的相关API即可了
        let $ = cheerio.load(val.data);//创建接口实例
        $('.rating-table-content').each((idx, element)=>{
        //idx是本次爬取循环的index
        //分析将要爬取数据的所在网页位置
    
    
        var $element = $(element);//创建解析所用到的实例
    
    
        let perArr=$element.find('.t4').text();
        perArr=perArr.split("\n");
        //t4类名的数据存在同类名多个标签，需要先行处理后存储
    
        items.push({
        id:ide++,
        href: $element.find(".t1-flex").find('a').attr('href'),
        title: $element.find(".t1-flex").find('a').find('span').text(),
        R23多核: perArr[2],
        R23单核: perArr[4],
        CPU核心数: perArr[6],
        CPU主频:$element.find(".t5").text().split("\n")[2],
        CPU_TDP:$element.find(".t6").text().split("\n")[2],
        发布时间:$element.find(".t7 span").text(),
        });//数据解析并存储到数组中去
    
    
    
    
        });
        })
        .catch(()=>{
            console.log("出错啦！！！");
        })
        //爬虫部分代码
    
    
    }//由于该网页数据为分页设计，所以采取循环爬取的策略
    
    
    res.send(items);//发送爬取出的数据发给前台
    
    });



app.listen(3000,function(){

console.log('app is listening at port 3000');

});//启动监听服务区
```





### 步骤1:启动与建立服务器：

> **可用的框架很多（Express，Koa，Egg）,本文章简便起见，使用express**

```js
var express = require('express');//建立建议的接收处理服务器


var app = express();//创建服务器实例

app.get('/',async function (req, res, next) {
// 建立路由
let items = [];
res.send(items);//发送爬取出的数据发给前台
});



app.listen(3000,function(){

console.log('app is listening at port 3000');

});//启动监听服务区
```





### 步骤2:发送爬虫请求

> **可用的形式依旧很多,可自行选择，axios对于爬虫确实不是最优的选择**

```js
    await axios.get(`https://cpu-compare.com/zh-CN/benchmark/cinebenchr23?page=${i}`)
    .then(function (val) {
    //解析数据的代码
    })
    .catch(()=>{
        console.log("出错啦！！！");//错误检验
    })
    //爬虫部分代码




```





### 步骤3:解析HTML标签



```js
//解析数据的代码


    // val.data里面存储着网页的 html 内容
    //将它传给 cheerio.load 之后
    // 就可以得到一个实现了 jquery 接口的变量
    // 之后直接使用jQuery的相关API即可了
    let $ = cheerio.load(val.data);//创建接口实例
    $('.rating-table-content').each((idx, element)=>{
    //idx是本次爬取循环的index
    //分析将要爬取数据的所在网页位置


    var $element = $(element);//创建解析所用到的实例


    let perArr=$element.find('.t4').text();
    perArr=perArr.split("\n");
    //t4类名的数据存在同类名多个标签，需要先行处理后存储

    items.push({
    id:ide,
    href: $element.find(".t1-flex").find('a').attr('href'),
    title: $element.find(".t1-flex").find('a').find('span').text(),
    R23多核: perArr[2],
    R23单核: perArr[4],
    CPU核心数: perArr[6],
    CPU主频:$element.find(".t5").text().split("\n")[2],
    CPU_TDP:$element.find(".t6").text().split("\n")[2],
    发布时间:$element.find(".t7 span").text(),
    });//数据解析并存储到数组中去
        
        
 
 //解析标签的部分其实就是对HTML标签逐层的解析，采用了jquery的api
 //在目标网站中启动调试模式，对着目标信息所处的标签层级逐层解开
```





### 步骤4:完善（存入数据库/分页处理）

```js
//根据网址不同，处理不同的页面URL;逐渐爬取出所有网站的信息
//使用Async和await进行异步等待,爬完后在res.send（）响应给前台数据



for(let i=1;i<19;i++){


    await axios.get(`https://cpu-compare.com/zh-CN/benchmark/cinebenchr23?page=${i}`)
    
    .then(function (val) {
    })
    .catch(()=>{
        console.log("出错啦！！！");
    })
    //爬虫部分代码


}//由于该网页数据为分页设计，所以采取循环爬取的策略
    
    
    res.send(items);//发送爬取出的数据发给前台

```









> **连接数据库存入的部分还没写，后期补档**







### 步骤5:爬出数据的一部分展示：



```json
{
"id": 1,
"href": "/zh-CN/cpu/amd_ryzen_threadripper_3990x",
"title": "AMD Ryzen Threadripper 3990X",
"R23多核": "74422 ",
"R23单核": "1254 ",
"CPU核心数": "64 ",
"CPU主频": "2.90 GHz ",
"CPU_TDP": "280 W ",
"发布时间": "Q1/2020"
},
{
"id": 2,
"href": "/zh-CN/cpu/amd_ryzen_threadripper_pro_3995wx",
"title": "AMD Ryzen Threadripper Pro 3995WX",
"R23多核": "73220 ",
"R23单核": "1231 ",
"CPU核心数": "64 ",
"CPU主频": "2.70 GHz ",
"CPU_TDP": "280 W ",
"发布时间": "Q1/2021"
},
{
"id": 3,
"href": "/zh-CN/cpu/intel_xeon_platinum_8280l",
"title": "Intel Xeon Platinum 8280L",
"R23多核": "49876 ",
"R23单核": "1037 ",
"CPU核心数": "28 ",
"CPU主频": " 2.70 GHz ",
"CPU_TDP": "205 W ",
"发布时间": "Q3/2019"
},
{
"id": 4,
"href": "/zh-CN/cpu/amd_epyc_7702",
"title": "AMD Epyc 7702",
"R23多核": "49035 ",
"R23单核": "996 ",
"CPU核心数": "64 ",
"CPU主频": "2.00 GHz ",
"CPU_TDP": "200 W ",
"发布时间": "Q3/2019"
},
{
"id": 5,
"href": "/zh-CN/cpu/amd_epyc_7702",
"title": "AMD Epyc 7702",
"R23多核": "49035 ",
"R23单核": "996 ",
"CPU核心数": "64 ",
"CPU主频": "2.00 GHz ",
"CPU_TDP": "200 W ",
"发布时间": "Q3/2019"
},
{
"id": 6,
"href": "/zh-CN/cpu/amd_epyc_7702p",
"title": "AMD Epyc 7702P",
"R23多核": "49035 ",
"R23单核": "996 ",
"CPU核心数": "64 ",
"CPU主频": "2.00 GHz ",
"CPU_TDP": "200 W ",
"发布时间": "Q3/2019"
},
{
"id": 7,
"href": "/zh-CN/cpu/amd_ryzen_threadripper_3970x",
"title": "AMD Ryzen Threadripper 3970X",
"R23多核": "44502 ",
"R23单核": "1270 ",
"CPU核心数": "32 ",
"CPU主频": "3.70 GHz ",
"CPU_TDP": "280 W ",
"发布时间": "Q4/2019"
},
{
"id": 8,
"href": "/zh-CN/cpu/amd_ryzen_threadripper_pro_3975wx",
"title": "AMD Ryzen Threadripper PRO 3975WX",
"R23多核": "42986 ",
"R23单核": "1244 ",
"CPU核心数": "32 ",
"CPU主频": "3.50 GHz ",
"CPU_TDP": "280 W ",
"发布时间": "Q3/2020"
},
```



## 5.可参考相关文档:

[cheerio官方文档](https://cheerio.js.org/)

[cheerio个人翻译的中文文档](https://www.jianshu.com/p/629a81b4e013)

[jQuery文档](https://jquery.cuishifeng.cn/)

[Axios文档](https://www.axios-http.cn/)

[@FB55](https://link.jianshu.com?t=https://github.com/FB55)

[htmlparser2](https://link.jianshu.com?t=https://github.com/fb55/htmlparser2)





























```js
var express = require('express');//建立建议的接收处理服务器
var cheerio = require('cheerio');//分析爬取出的数据，主要使用api可借鉴jQuery
const axios = require('axios');//发送请求要用

var app = express();//创建服务器实例

app.get('/ios',async function (req, res, next) {
// 建立路由

let items = [];
let ide=1;


    await axios.get(`https://browser.geekbench.com/ios-benchmarks/`)
    .then(function (val) {

    // val.data里面存储着网页的 html 内容
    //将它传给 cheerio.load 之后
    // 就可以得到一个实现了 jquery 接口的变量
    // 之后直接使用jQuery的相关API即可了
    let $ = cheerio.load(val.data);//创建接口实例
    $('.benchmark-chart-table tbody tr').each((idx, element)=>{
    //idx是本次爬取循环的index
    //分析将要爬取数据的所在网页位置


    var $element = $(element);//创建解析所用到的实例


    let perArr=$element.find('.t4').text();
    perArr=perArr.split("\n");
    //t4类名的数据存在同类名多个标签，需要先行处理后存储

    items.push({
    id:idx,

    title: $element.find(".name").find('a').text(),
    detailPageHref:$element.find(".name").find("a").attr("href"),
    computer:$element.find(".description").text(),
    score:$element.find(".score").text(),

    });//数据解析并存储到数组中去




    });
    })
    .catch(()=>{
        console.log("出错啦！！！");
    })
    //爬虫部分代码


//由于该网页数据为分页设计，所以采取循环爬取的策略


res.send(items);//发送爬取出的数据发给前台

});




app.get('/r20',async function (req, res, next) {
    // 建立路由
    
    let items = [];
    let ide=1;
    
    
    
    for(let i=1;i<28;i++){
    
    
        await axios.get(`https://cpu-compare.com/zh-CN/benchmark/cinebenchr20?page=${i}`)
        .then(function (val) {
    
        // val.data里面存储着网页的 html 内容
        //将它传给 cheerio.load 之后
        // 就可以得到一个实现了 jquery 接口的变量
        // 之后直接使用jQuery的相关API即可了
        let $ = cheerio.load(val.data);//创建接口实例
        $('.rating-table-content').each((idx, element)=>{
        //idx是本次爬取循环的index
        //分析将要爬取数据的所在网页位置
    
    
        var $element = $(element);//创建解析所用到的实例
    
    
        let perArr=$element.find('.t4').text();
        perArr=perArr.split("\n");
        //t4类名的数据存在同类名多个标签，需要先行处理后存储
    
        items.push({
        id:ide++,
        href: $element.find(".t1-flex").find('a').attr('href'),
        title: $element.find(".t1-flex").find('a').find('span').text(),
        R23多核: perArr[2],
        R23单核: perArr[4],
        CPU核心数: perArr[6],
        CPU主频:$element.find(".t5").text().split("\n")[2],
        CPU_TDP:$element.find(".t6").text().split("\n")[2],
        发布时间:$element.find(".t7 span").text(),
        });//数据解析并存储到数组中去
    
    
    
    
        });
        })
        .catch(()=>{
            console.log("出错啦！！！");
        })
        //爬虫部分代码
    
    
    }//由于该网页数据为分页设计，所以采取循环爬取的策略
    
    
    res.send(items);//发送爬取出的数据发给前台
    
    });



app.listen(3000,function(){

console.log('app is listening at port 3000');

});//启动监听服务区
```









```sql
CREATE TABLE `crawl_db`.`cpu_r23` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `href` VARCHAR(200) NOT NULL,
  `title` VARCHAR(200) NOT NULL,
  `r20_sigal` VARCHAR(70) NOT NULL,
  `r20_muti` VARCHAR(70) NOT NULL,
  `cores` VARCHAR(30) NOT NULL,
  `cpu_speed` VARCHAR(45) NOT NULL,
  `cpu_tdp` VARCHAR(45) NOT NULL,
  `time` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE);

```

