```js
var express = require('express');
var cheerio = require('cheerio');
var superagent = require('superagent');

var app = express();

app.get('/', function (req, res, next) {
// 用 superagent 去抓取 https://cnodejs.org/ 的内容
superagent.get('https://www.gpu-monkey.com/en/gpu_benchmark-3dmark_time_spy_and_fire_strike-5')
.end(function (err, sres) {
// 常规的错误处理
if (err) {
return next(err);
}
// sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
// 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
// 剩下就都是 jquery 的内容了
var $ = cheerio.load(sres.text);
var items = [];
$('.data tbody tr').each(function (idx, element) {
var $element = $(element);
items.push({
title: $element.children(1).find("a").text(),
memory: $element.children(1).find("span").text(),
hrefPIC: $element.children(1).find("a").attr("href"),
score: $element.children(2).find("div").text(),
});
console.log(items);
});

res.send(items);
});
});



app.listen(3000,function(){

console.log('app is listening at port 3000');

});
```

```js
var express = require('express');
var cheerio = require('cheerio');
var superagent = require('superagent');

var app = express();

app.get('/', function (req, res, next) {
// 用 superagent 去抓取 https://cnodejs.org/ 的内容
superagent.get('https://benchmarks.ul.com/zh-hans/compare/best-gpus')
.end(function (err, sres) {
// 常规的错误处理
if (err) {
return next(err);
}
// sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
// 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
// 剩下就都是 jquery 的内容了
var $ = cheerio.load(sres.text);
var items = [];
$('.navigationtable tbody tr').each(function (idx, element) {
var $element = $(element);
items.push({
id:$element.find(".order-cell").text(),
title: $element.find(".OneLinkNoTx").text(),
hrefPIC: $element.find(".OneLinkNoTx").attr('href'),


// memory: $element.children(1).find("span").text(),
score: $element.find(".bar-score").text(),
price:$element.find(".price-large-list").find("div").text(),
});
console.log(items);
});

res.send(items);
});
});



app.listen(3000,function(){

console.log('app is listening at port 3000');

});
```







```js
var express = require('express');
var cheerio = require('cheerio');
var superagent = require('superagent');

var app = express();

app.get('/',function (req, res, next) {
// 用 superagent 去抓取 https://cnodejs.org/ 的内容

let items = [];
let ide=0;

superagent.get('https://cpu-compare.com/zh-CN/benchmark/cinebenchr23?page=2')
.end(function (err, sres) {
// 常规的错误处理
if (err) {
return next(err);
}
// sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
// 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
// 剩下就都是 jquery 的内容了
var $ = cheerio.load(sres.text);
$('.rating-table-content').each(function (idx, element) {

ide++;
var $element = $(element);
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




});


});



res.send(items);


});



app.listen(3000,function(){

console.log('app is listening at port 3000');

});
```

