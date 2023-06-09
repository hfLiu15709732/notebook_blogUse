## 一、关于ES6理论：

### 1、ES6是什么？

ES 全称是ECAMScript，它是JavaScript基础构建的一种语言，JavaScript正是建立在ECAMScript语言的基础规范中建立使用的，那么，ECAMScript的使用，对于JavaScript至关重要！

在我的理解中，ECAMScript是一种语言层面的东西，它只是定义了JavaScript以及在它基础之上建立的其他语言的语法规范，而JavaScript的语言，更关于一种平台性质在其中。

正如图1-1所示，JavaScript包括 ECAMScript、DOM、BOM三个组成部分，DOM和BOM是web API提供的接口或者是JavaScript和浏览器之间进行交互的部分，实质就是操纵文档元素，进行展示布局，而ECAMScript在JavaScript中其中语法的作用，它不会去跟文档有直接的关系，但是他的数据处理完成后会通过web API展示在文档中。

![image-20210809114226980.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0dc3b7980649493ea9fa44551477d162~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

下面就关于ES2015、ES2016、ES2017三个版本的更新内容，即我们通常讲的ES6来展开讲讲它都有哪些新内容

### 2、ES6新特性的分类

我们将讲解的内容主要归为四大类：

1、解决原有语法上的一些不足

let 和 const 的块级作用域

2、对原有语法进行增强

解构、展开、参数默认值、模板字符串

3、全新的对象、全新的方法、全新的功能

promise、proxy、object的assign、is

4、全新的数据类型和数据结构

symbol、set、map

## 二、ES6的使用

### 1、let 和 const 的块级作用域

在之前的ECMAScript中，对于作用域的划分只有**全局作用域**和**函数作用域**，在ES6中又新增了块级作用域，通常的来说就是**一对{}** 包裹着的代码，在{}中的作用域就是块级作用域，因此新定义了两个let和const关键词，对于var这个老的关键词，是用于对老代码的维护所保留。

1> let 关键字禁止变量提升，要先定义后使用，而且只作用于它所定义的块级作用域中，常用于for 或 if 之类的结构中。

```css
for ( var i = 0;i < 3;i++) {
        for ( var i = 0; i < 3; i++) {
            console.log(i);
        }
    } // 0 1 2
    
    -----------------------------------
    for ( let i = 0;i < 3;i++) {
        for (let i = 0; i < 3; i++) {
            console.log(i);
        }
    } // 0 1 2 0 1 2 0 1 2
复制代码
```

2> 在for循环中，let效果实际上是利用了闭包的机制，并且，对于循环块中的let 和 执行块中的let是两个块中分别定义的变量，互不影响。

```ini
for (let i = 0; i < 2; i++) {
        let i = 'foo'
        console.log(i);
    } // foo  foo
复制代码
```

3> const 是在let 的基础上添加了一个**只读**属性，即变量一旦声明过后就不允许在修改，这里是不允许修改定义的地址，而不是不可以修改他内部的属性值。

```ini
const obj = {name: 'aaa'}
obj.name = 'bbb'
复制代码
```

4>在使用上，不要使用var，主要使用 const 并配合着 let 使用

### 2、数组的解构

数组的解构是根据位置进行获取的。

```arduino
// 
const arr = [100, 200, 300]
  // 都获取
  const [a, b, c] = arr // a = 100 b = 200 c = 300
  // 只获取特定的值
  const [, , c] = arr  // c = 300
  // 对于的数组长度超过，会返回undefined
  const [a,b,c,d,e] = arr // a = 100, b = 200 c = 300  d = undefined e = undefined
  // 还可以设置默认值
  const [a,b,c,d = 400,e = 500] = arr // a = 100, b = 200 c = 300  d = 400 e = 500
复制代码
```

### 3、对象的解构

对象的解构和数组类似，只不过不是根据位置下标获取，而是根据属性名进行获取。

```javascript
const obj = {name: 'bob', age: 23}

 // 首先是根据属性获取值
 const {name, age} = obj
 console.log(name)
 console.log(age)
 // 如果外面有一个相同的属性名，可以使用重命名的方式获取，并且可以设置默认值
 const {name:setName = 'sale', age} = obj
 let name = 'reset'
 console.log(name)
 console.log(age)
复制代码
```

### 4、模板自变量

在ES6的语法中，新增了模板自变量的新语法格式，使用破折号按键那个点做双引号的格式 ·· ，这个模板自变量的特性如下：

1、支持换行;

2、采用${}的方式进行插值表达式，{}中的内容就是js代码;

3、在模板自变量之前，我们可以加一个标签函数用来对模板自变量的值进行处理。

```javascript
const name = 'df'
	const age == 18
	const mrg = `I am df, ${name}的年龄是 ${age}` // 定义一个模板自变量
	
	const fn = (strs) => { // strs 就是接收到的模板自变量的数组
		console.log(strs) // 这里会打印一个数组出来，是通过${}进行分割后的字符串数组，不包含分割符中的值
	}
	function tag ( strs,name,name1) { // 在str后面，会依次接收表达式的值
		console.log(strs,name1,name1) // [ 'I am df, ', '的年龄是 ', '' ] 18 18
		console.log(strs,name,name1) // [ 'I am df, ', '的年龄是 ', '' ] df 18
		return strs[0] + name + strs[1] + name1 + strs[2] // 
	}
    const mrg1 = tag`I am df, ${name}的年龄是 ${age}` // 对于模板标签函数，我们可以看成是模板自变量的一个过滤器，对模板进行数据处理
     console.log(mrg1); // 这里标签函数tag的返回值就是模板自变量的值（即mggr1的值）
复制代码
```

### 5、默认参数和剩余参数

默认参数

默认参数的设定在之前是通过在函数内部进行条件判断进行判断。

```less
function add (a ,b) {
a = a === undefined ? 0 : a
b = b === undefined ? 1 : b
retrun a + b
}
add (0) // 1
复制代码
```

在ES6中新增的给参数设置默认值的特性，将大大简化我们的代码

```css
function add (a = 0 ,b = 1) { // 这里要说明的是，一定要将有默认值的参数放到没有默认值参数的后面位置（即出现在参数列表的最后），因为我们的参数是按照次序传递的，如果没有放到最后，我们的默认值将无法正常工作
console.log(' foo is shu', a + b)
retrun a + b
}
add (0) // 1
复制代码
```

剩余参数

之前我们是使用arguments对象，通过伪数组方式来接收参数，包括我们在未知参数个数的情况下。现在，在ES6的语法中，新增了...的操作符，它有两个作用：

1、剩余操作符，用来接收从当前位置剩余参数的操作。

```lua
function redece (...arg) {
	console.log(arg) // 这里的arg是一个伪数组，是传递进来的实参数组，并且这个操作只能放到最后一个参数的位置上进行（只能使用一次），否则影响前面参数
}
复制代码
```

2、spread用法：（展开），将数组或者对象中内容或者属性进行依次展开。

```javascript
const arr = ['foo', 'fn', 'functions']

console.log(arr[0], arr[1], arr[2]) // 方案一 
console.log.apply(console, arr) // 方案二   apply方法是（参数一： 用来从新定向this指向，参数二，向函数中放入参数的数组）
console.log(...arr) // ES6（spread）写法

复制代码
```

### 6、箭头函数

箭头函数 （参数列表）=> {函数体} ，在函数体中只有返回值的情况下， 大括号可以省略。

```dart
const fn = n => n+1
const fn1 = (n = 3, m = 2) => {
	return n + m
}
复制代码
```

箭头函数的特点：

1、简化的函数的书写格式，使函数看起来更加容读和书写

2、它没有this的机制，不会改变this的指向，任何情况下都不会改变

```javascript
name = 1
const fn = (n,m) => {
n = this.name 
return n + m
}

// 非箭头函数的写法，this会指向函数调用者
say : function (n, m) {
const _this = this
setTimeout( ()=> {
	console.log(this.name) // undefined
}, 1000)
}
复制代码
```

打扰一下，这里想到了apply、call、bind 三个方法是用来改变this执行的，记录一下：

```kotlin
call： 两个参数，第一个是this值，不设置或null指向全局，第二个是一个形参对象；立即执行

apply： 两个参数，第一个是this值，不设置或null指向全局，第二个是一个形参伪数组；立即执行

bind: 是call的封装；懒执行


复制代码
```

### 7、对象字面量的增强

对于对象用法的增强，主要表现在：

1、如果想给对象的属性赋值一个变量值，并且该变量名和属性名一样，则可以省略写法。

2、在对象中的方法也可以省略写，当方法名和属性名相同时，省略冒号,这里的this是指向调用对象方法的对象

3、对于属性名的命名，必须使用明确的值，在es2015中，可以使用[]的方式对对象属性名进行命名

```javascript
const name = 'df'
// 1、
const obj = {
 foo: 'foo',
 name   //这里的name名字相同，可以省略写
 
 // 2、
 // fn : function fn () {console.log('eeeee...')}
 // es2015中可以用
 fn () {console.log('eeeee...')}
 }
 
//  3、
// obj.Math.random():123  //写法是错误的
// obj[Math.random()]:123
// 在es2015中，新增了写法
obj[Math.random()] = 123
复制代码
```

在说之前，现补充一个遗漏的点，

1、对于字符串，新增加了三个方法，includes 和 startsWith endsWith，入参是字符串，返回一个是否包含该字符串的boolean值.

2、assign方法：将源对象合并到目标对象中

```css
const obj = {
name: 'df'
age: 12
}
const objDog = {
	type: 'dog'
}
const action = { say : 'www'}
const animal = Object.assign(objDog, obj, action) // 后面也可以跟多个参数，都会赋值到第一个目标对象中
console.log(animal) // {name: 'df',age: 12,type: 'dog',say: 'www'}
复制代码
```

具体的应用场景是：

1、如果一个对象（o1）的属性引用了另一个对象（o2）的值，在修改o1的时候，也会修改o2的值，这里用到assign方法，将o2的值赋值给o1的话，两个就相互不影响了。

```javascript
const obj1 = function func () {
 	const name = 'df'
        ret
 }
 obj1.name = 'jxl'
 console,log(obj.name) // df
 console.log(obj1.name) // jxl
```


链接：https://juejin.cn/post/7010696844153978887
