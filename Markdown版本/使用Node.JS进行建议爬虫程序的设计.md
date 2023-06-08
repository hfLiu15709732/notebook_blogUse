# 使用Node.JS进行建议爬虫程序的设计

## 工具

爬虫必备工具：cheerio cheerio简单介绍：cheerio是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方。 大家可以简单的理解为用来解析html非常方便的工具。 使用之前只需要在终端安装即可 **npm install cheerio  --save**

## node爬虫步骤解析

### 一、选取网页url，使用http协议get到网页数据

彼岸图网链接地址：'[pic.netbian.com](https://link.juejin.cn?target=https%3A%2F%2Fpic.netbian.com)'

首先我们请求http协议，通过http来拿到网页的所有数据

```javascript
const https = require('https');
const url ='https://pic.netbian.com'
https.get(url,function(res){
    // 分段返回的 自己拼接
    let html = '';
    // 有数据产生的时候 拼接
    res.on('data',function(chunk){
        html += chunk;
    })
    // 拼接完成
    res.on('end',function(){
        console.log(html);
    })
})

复制代码
```

上面代码呢，大家一定要注意我们请求数据时，拿到的数据是分段拿到的,我们需要通过自己把**数据拼接**起来

```javascript
res.on('data',function(chunk){
        html += chunk;
    })

复制代码
```

拼接完成时 我们可以输出一下，看一下我们是否拿到了完整数据

```javascript
res.on('end',function(){
        console.log(html);
    })

复制代码
```

### 二、使用cheerio工具解析需要的内容

```scss
const cheerio = require('cheerio');
res.on('end',function(){
        console.log(html);
        const $ = cheerio.load(html);
        let allFilms = [];
        $('.slist ul li a span img).each(function(){
            // this 循环时 指向当前这个电影
            // 当前这个电影下面的title
            const pic = $(this).attr('src');
            // 存 数据库
            // 没有数据库存成一个json文件 fs
            allFilms.push({
                pic
            })
        })

复制代码
```

可以通过检查网页源代码查看需要的内容在哪个标签下面，然后通过$符号来拿到需要的内容，这里我就拿了图片的url

到了这时候，你会发现，node爬虫实现是非常简单的，我们只需要认真分析一下我们拿到的html数据，将需要的内容拿出来保存在本地就基本完成了

### 三、保存数据

下面就是保存数据了，我将数据保存在films.json文件中 将数据保存到文件中，我们引入一个fs模块，将数据写入文件中去

```javascript
const fs = require('fs');
fs.writeFile('./films.json'), JSON.stringify(allFilms),function(err){
            if(!err){
                console.log('文件写入完毕');
            }
        })
复制代码
```

文件写入代码需要写在res.on('end')里面，数据读完->写入 写入完成，可以查看一下films.json，里面是有爬取的数据的。

### 四、下载图片

我们爬取的图片数据是图片地址，如果我们要将图片保存到本地呢？ 这时候只需要跟前面请求网页数据一样，把图片地址url请求回来，每一张图片写入到本地即可

```javascript
function downloadImage(allFilms) {
    for(let i=0; i<allFilms.length; i++){
        const picUrl = allFilms[i].pic;
        // 请求 -> 拿到内容
        // fs.writeFile('./xx.png','内容')
        https.get(picUrl,function(res){
            res.setEncoding('binary');
            let str = '';
            res.on('data',function(chunk){
                str += chunk;
            })
            res.on('end',function(){
                fs.writeFile(`./images/${i}.png`,str,'binary',function(err){
                    if(!err){
                        console.log(`第${i}张图片下载成功`);
                    }
                })
            })
        })
    }
}
复制代码
复制代码
```

下载图片的步骤跟爬取网页数据的步骤是一模一样的，我们将图片的格式保存为.png 写好了下载图片的函数，我们在res.on('end')里面调用一下函数就大功告成了

## 源码

### app.js

```javascript
const express = require('express')

const app = express()
const cheerio = require('cheerio');

const https = require('https');
const url = 'https://pic.netbian.com';
const { downloadImage } = require('./utils');
https.get(url, function (res) {
    // 分段返回的 自己拼接
    let html = '';
    // 有数据产生的时候 拼接
    res.on('data', function (chunk) {
        html += chunk;
    })
    // 拼接完成
    res.on('end', function () {
        const $ = cheerio.load(html);
        $('.slist ul li a span img').each(function (index, element) {
            downloadImage(url + $(this).attr('src'), index)
        })
    })
})
app.listen(3000, () => {
    console.log('Server is running on port 3000')
})
复制代码
```

### utils->index.js

```javascript
const https = require('https');
const fs = require('fs');
function downloadImage(picUrl, index) {
    https.get(picUrl, function (res) {
        res.setEncoding('binary');
        let str = '';
        res.on('data', function (chunk) {
            str += chunk;
        })
        res.on('end', function () {
            fs.writeFile(`./images/${index}.png`, str, 'binary', function (err) {
                if (!err) {
                    console.log(`第${index}张图片下载成功`);
                } else {
                    console.log(err);
                }

            })
        })
    })
}

module.exports = {
    downloadImage
}
```




链接：https://juejin.cn/post/7057035666944688135