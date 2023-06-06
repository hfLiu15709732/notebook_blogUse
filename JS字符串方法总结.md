# JS字符串方法总结

## 1：整体概括：



1. | 序号 |     方法名     |              功能              |          最终返回值          | 是否会改变原字符串 |
   | :--: | :------------: | :----------------------------: | :--------------------------: | ------------------ |
   |  1   |   length属性   |         返回字符串长度         |        返回字符串长度        | 不会               |
   |  2   |    属性访问    |    访问字符串指定位置的字符    |   返回字符串指定位置的字符   | 不会               |
   |  3   |   indexOf()    |  返回字符串正向指定位置的字符  |   返回字符串指定位置的字符   | 不会               |
   |  4   | lastIndexOf()  |  返回字符串反向指定位置的字符  |   返回字符串指定位置的字符   | 不会               |
   |  5   |    search()    |            查找字符            |           所在位置           | 不会               |
   |  6   |    match()     |      匹配字符或正则表达式      |          数组或null          | 不会               |
   |  7   |    charAt()    |         指定位置的字符         |        指定位置的字符        | 不会               |
   |  8   |  charCodeAt()  | 获得指定位置字符的unicode 编码 | 指定位置的字符的unicode 编码 | 不会               |
   |  9   |    slice()     |           截取字符串           |        截取后的字符串        | 不会               |
   |  10  |  substring()   |           截取字符串           |        截取后的字符串        | 不会               |
   |  11  |    substr()    |           截取字符串           |        截取后的字符串        | 不会               |
   |  12  |   replace()    |            替换字符            |        替换后的字符串        | 不会               |
   |  13  | toUpperCase()  |        字符串全部转大写        |        改变后的字符串        | 不会               |
   |  14  | toLowerCase()  |        字符串全部转小写        |        改变后的字符串        | 不会               |
   |  15  |    conact()    |           字符串拼接           |       拼接之后的字符串       | 不会               |
   |  16  |     trim()     |      去除字符串两头的空格      |      去除空格后的字符串      | 不会               |
   |  17  |    split()     |    字符串根据划分转化为数组    |       根据划分后的数组       | 不会               |
   |  18  | localCompare() |         字符串比较大小         |            1/0/-1            | 不会               |
   |  19  |    链式调用    |                                |                              |                    |



**注意点：**

**JS中所有的字符串操作均不会影响原字符串；**

但这样的不变，只是表面数据不变化，**内存地址可能发生变化**





------



## 2.---length方法：

**`length`返回字符串的长度**

```javascript
let s = 'Hello World'

console.log(s.length) // 11
```



------

## 3.---属性访问：

**注意点：**

- 如果找不到字符，属相访问的形式会 返回 undefined，而 charAt() 返回空字符串。
- **它是只读的。str[0] = "z" 是没用的**

```javascript
   <script>

        let str="hello-world";
        console.log(str[3]);//l
        str[2]='z';
        console.log(str);//hello-world
        //注意该方式是只读的，str[2]='a'这样的代码不会报错，但也没用

    </script>
```



------

## 3----indexOf与lastIndexOf：

`indexOf()`方法返回字符串中指定文本***正向第一次出现的索引号**

`lastIndexOf()`方法返回指定文本在字符串中***反向第一次一次出现的索引**

[]()

```javascript
  <script>

        let str="hello-world";
        console.log(str.indexOf('l'));//2
        console.log(str.lastIndexOf('l'));//9

    </script>
```

如果未找到文本， indexOf() 和 lastIndexOf() 均返回 -1。 **两种方法都接受作为检索起始位置的第二个参数。**



------

## 4----search()与match方法：                                                     ------------->

**search()**方法**查找指定字符串**，并返回第一次匹配的位置

**注意**：

- **search()** 方法**的参数可以使用正则表达式**
- 和indexOf功能相似，但不能设置第二个参数（开始位置）

```javascript
        let str="abcdefgaacdcdcdcd";
        console.log(str.search("cd"));//2
```





**match()**方法用于查看原数据与指定数据**（字符串或正则表达式）**是否匹配

返回数组（第一个值为匹配到的数据，具有index属性与input属性），没有匹配到就返回null



**返回数组属性：**index（出现位置的索引号） input（原数据）

```javascript
        let str="abcdefgaacdcdcdcd";
        let arr=str.match("cdef");
        console.log(arr);//['cdef']
        console.log(arr.input);//abcdefgaacdcdcdcd
        console.log(arr.index);//2
```



------

## 5----charAt()方法：

`charAt()` 方法返回字符串中指定索引号的字符

```javascript
        let str="hello-world";
        console.log(str.charAt(4));//o
        console.log(str[4]);//o
```



------

## 5----charCodeAt()方法：

`charCodeAt()` 方法返回字符串中索引位置的字符 的unicode 编码

```javascript

        let str="hello-world";
        console.log(str.charCodeAt(4));//111
```



------

## 6----slice()方法：                                                              ------------->

slice() 截取字符串。

该方法设置两个参数：开始的索引号，**结束索引号（不包含该位置）**。

**左闭右开的区间**

```javascript
    let str="hello-world";
        let newStr=str.slice(3,7);
        console.log(newStr);//lo-w
        // 如果某个参数为负，则从字符串的结尾开始计数。
        console.log(str.slice(-3, -1)) // rl 即从倒数第三位开始，不包含倒数第一

        //如果省略第二个参数，则该方法将裁剪字符串的剩余部分
        console.log(str.slice(-3)) // rld

        //仍然是左闭右开
```



------

## 7----substring()方法：

`**substring()**` 与`**slice()**`方法很相像

但不同在于 **`substring()` 方法无法接受负的索引**。

```javascript
        let str="hello-world";

        console.log(str.substring(6,8)) //wo
```



------

## 8----substr()方法：                                             ------------->

`substr()` 仍然和slice()`**很相像。

不同之处在于**substr（）方法第二个参数是本方法的提取*长度*。**

```javascript
    let str="hello-world";

        console.log(str.substr(1, 3)) // ell

        //如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分。
        console.log(str.substr(1)) // ello World

       // 如果首个参数为负，则从字符串的结尾计算位置。
        console.log(str.substr(-5, 3)) // Wor
        console.log(str.substr(-5)) // World
```



------

## 9----replace()方法：                                                   ------------->

`replace()` 方法用另一个值替换在字符串中指定的值,**返回的是新字符串**。

**注意点：**

- 对大小写敏感
- 只替换首个匹配

```javascript
     let str="hello-world";

        console.log(str.replace('l', 'L')) // HeLlo World

        //如需替换所有匹配，请使用正则表达式的 g 标志（用于全局搜索）
        console.log(str.replace(/l/g, 'L')) // HeLLo WorLd

        //如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感）
        console.log(str.replace(/h/i, 'a')) // aello World
```



------

## 10----大小写转换方法：                                                    ------------->



**toUpperCase()** 把字符串转换为大写

**toLowerCase()** 把字符串转换为小写

```javascript
        let str="hello-world";
        console.log(str.toUpperCase());//HELLO-WORLD
        console.log(str.toLowerCase());//hello-world
```

------

## 11----concat（）方法：                                             ------------->

`concat()` 连接两个或多个字符串，会返回新字符串

与加法运算符功能相近

```javascript
        console.log(str.concat("///////",str2));//hello-world///////hhhhhhhh
        console.log(str.concat("///////",str2,str3));//hello-world///////hhhhhhhhaaaaaaaaaaaaa
        console.log(str.concat("///////",str2,"--------------",str3));//hello-world///////hhhhhhhh--------------aaaaaaaaaaaaa
```

------

## 12----trim（）方法：                                                     ------------->

`trim()` 方法可用来去除字符串两边的空白字符

```javascript
        let strNew="    ldfs   ysdfbsrdf    12536123    ";
        console.log(strNew.trim());//ldfs   ysdfbsrdf    12536123
```

------

## 13----split（）方法：                 ---------------->                              

通过 `split()`向数组的转化（分割为括号中的字符串参数）

```javascript

        let urlStr="id=123&pwd=456&email=789";
        let str1=urlStr.split("&");
        console.log(str1);//['id=123', 'pwd=456', 'email=789']


  		let s="Hello-World"
        console.log(s.split('')) //(11) ['H', 'e', 'l', 'l', 'o', '-', 'W', 'o', 'r', 'l', 'd']

        //如果省略分隔符或字符串中无分隔符，被返回的数组将包含 index [0] 中的整个字符串
        console.log(s.split()) //['Hello-World']

        console.log(s.split("************"));//['Hello-World']

```



------

## 14----localCompare ( )方法：



**根据unicode 编码值进行大小比较，**

返回1，说明调用API的字符串更大

返回0，说明两者相同

返回-1，说明调用API的字符串更小



```javascript
        let str="azzzzzzzz";
        let str1="baaaaaa";
        let str2="c"
        let strCopy="azzzzzzzz"
        
        console.log(str.localeCompare(str1));//-1
        console.log(str.localeCompare(strCopy));//0
        console.log(str2.localeCompare(str1));//1
```



------

## 15----链式调用：

若上述方法返回的是数组，就可以链式调用



**小案例：（将urlencoded编码转化为对象）**

```javascript
    <script>
        
        let urlStr="?id=123&pwd=456&email=789";
        let newObj={};


        urlStr
        .slice(1)//去掉urlencoded字符串中的？
        .split("&")//根据&分割成一次数组向
        .forEach((data)=>{
            let key=data.split("=")[0];
            let value=data.split("=")[1];//解析出对象的key和value
            newObj[key]=value;
        })
        console.log(newObj);//{id: '123', pwd: '456', email: '789'}

    </script>
```

