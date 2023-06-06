# CSS遗失部分总结

# 1.伪类选择器

## 1.1用户行为伪类：

------



### 1.1.1  -----hover选择器：

**应用场景：**

**鼠标经过元素时的样式变化**，Tips提示，下拉列表和过渡动画等

**细节优化：**

- 如果在鼠标移动到目标元素过程中触发了附近其他元素的:hover效果导致遮盖了目标元素，可通过设置目标元素效果hover的延时出现（通过visibility控制显隐和transition配合使用）
- 通过:hover实现类似**下拉列表**这种重要功能时，需考虑用户交互环境无鼠标的情景（如触屏设备，智能电视），可通过增加focus伪类来优化（使用Tab键来聚焦目标元素触发之前的hover对应的效果）

**注意事项：**

:**hover不适用于移动端**，虽然也能触发，但消失并不敏捷，体验反而奇怪。

------

### 1.1.2  -----active选择器：

`:active` 伪类一般被用在 **a元素和button元素**中。这个伪类的一些其他适用对象包括包含激活元素的元素，以及可以通过他们关联的[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label)标签被激活的表格元素。

```html
<p>This paragraph contains a link:
  <a href="#">This link will turn red while you click on it.</a>
  The paragraph will get a gray background while you click on it or the link.
</p>
```



```css
a:link { color: blue; }          /* 未访问链接 */
a:visited { color: purple; }     /* 已访问链接 */
a:hover { background: yellow; }  /* 用户鼠标悬停 */
a:active { color: red; }         /* 激活链接 */

p:active { background: #eee; }   /* 激活段落 */
```

------



1.1.3 -----focus选择器：

**有效范围：**

- 非disabled状态的表单元素（**包括input，select和button元素）**
- 含有href属性的a元素
- area和summary元素

**应用场景：**

常用于表单元素聚焦时的边框高亮显示

**注意事项：**

使用:focus来修改按钮的样式时，建议按钮就用button元素，而非span或div标签模拟的按钮。因为考虑到纯键盘无鼠标的场景,button元素可以通过键盘Tab触发聚焦，而span或div元素不行

------

### 1.1.3-----focus-within：

**这也就意味着，它或它的后代获得焦点，都可以触发 `:focus-within`。**

该选择器非常实用。举个通俗的例子：**表单中的某个input字段触发focus，整个form父元素都可被高亮。**

**focus-within是添加个最高层元素/父元素**



```html
<p>试试在这个表单中输入点什么。</p>

<form>
  <label for="given_name">Given Name:</label>
  <input id="given_name" type="text">
  <br>
  <label for="family_name">Family Name:</label>
  <input id="family_name" type="text">
</form>

```

```css
form {
  border: 1px solid;
  color: gray;
  padding: 4px;
}

form:focus-within {
  background: #ff8;
  color: black;
}

input {
  margin: 4px;
}

```







### **1.1.4 其他相关：**

- :link。元素被定义了超链接但并未被访问过
- :visited。元素被定义了超链接并已被访问过
- :active。元素被激活
- :hover。鼠标悬停
- :focus。元素获取焦点

------

1.1.4-----focus-visible：

当元素匹配[`:focus`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus)伪类并且客户端 (UA) 的启发式引擎决定焦点应当可见 (在这种情况下很多浏览器默认显示“焦点框”。) 时，**`:focus-visible`** 伪类将生效。

这个选择器可以有效地根据用户的输入方式 **(鼠标 vs 键盘)** 展示不同形式的焦点。

（看案例能看出来）（浏览器自己去判断）（不考虑键盘这个问题的话，实际意义不明显）



案例：

在这个例子中，`:focus-visible` 选择器利用客户端 (UA) 的行为决定是否匹配。比较一下，当你用鼠**标点击控件和用键盘 tab 切换**控件有何不同。请注意元素的表现与具有 `:focus` 样式的元素的区别。

```html
<input value="Default styles"><br>
<button>Default styles</button><br>
<input class="focus-only" value=":focus only"><br>
<button class="focus-only">:focus only</button><br>
<input class="focus-visible-only" value=":focus-visible only"><br>
<button class="focus-visible-only">:focus-visible only</button>

```

```css
input, button {
  margin: 10px;
}

.focus-only:focus {
  outline: 2px solid black;
}

.focus-visible-only:focus-visible {
  outline: 4px dashed darkorange;
}

```



------

## 1.2 输入伪类：

### 1.2.1-----enabled和:disabled：

在Web表单中，有些**表单元素（如输入框、密码框、复选框等）**有“**可用”和“不可用”**这2种状态。默认情况下，这些表单元素都处在可用状态。

在CSS3中，我们可以使用：enabled选择器和：disabled选择器来分别设置表单元素的可用与不可用这两种状态的CSS样式。举例：

```html
<!DOCTYPE html>
<html>
<head>
    <title>伪类选择器</title>
    <meta charset="utf-8">
    <style type="text/css">
        /* 设置禁用与可用的样式 */
        :enabled {
            outline: 1px solid green;
        }

        :disabled {
            background-color: #dddddd;
        }
    </style>
</head>
<body>
    <form>
        <p>
            <label for="enabled">可用：</label>
            <input type="text" name="enabled">
        </p> 
        <p>     
            <label for="disabled">禁用：</label>
            <input type="text" name="disabled" disabled>
        </p> 
        <button>可用按钮</button>
        <button disabled>不可用按钮</button>
    </form>
</body>
</html>

```

------

### 1.2.2-----read-only和:read-write

**:read-write**
定义：当一个元素（例如可输入文本的 input元素）**可以被用户编辑时，修改样式。**

触发条件：当一个元素是可编辑元素时，可以修改其样式

兼容：IE不支持

**:read-only**
定义：当一个可编辑元素（例如可输入文本的 input元素）不可被用户编辑，**即只读时（具有readonly属性时），修改样式。**

触发条件：只针对当一个可编辑元素被赋予readonly（只读）属性时，可以修改其样式

兼容：IE不支持

**注意：**
因为:read-write和:read-only均针对可编辑元素，而HTML5新增了一个特性**contenteditable**属性，当任意一个元素拥有contenteditable属性，并且**它的属性值为true时，该元素也属于可编辑元**素。

------

1.2.3.其他相关：

- **:checked。选中的复选按钮或者单选按钮表单元素**
- :enabled。所有启用的表单元素
- :disabled。所有禁用的表单元素

UI 元素状态伪类选择器主要是针对 Form 表单元素进行操作，最常见的使用方式如设置 **"type="text"** 有 enable 和 disabled 两种状态，enable 为可写状态，disabled 为不可状态。







------

1.3结构伪类选择器：



- **:fisrt-child。**父元素的第一个子元素
- **:last-child。**父元素的最后一个子元素的元素
- **:root：**元素所在文档的根元素。在 HTML 文档中，根元素始终是 html，此时该选择器与 html 类型选择器匹配的内容相同
- **:nth-child(n)。**父元素的第 n 个子元素。其中 **n 可以是整数（1，2，3）**、**关键字（even，odd）**、也可以**是公式（2n+1）**,而且 **n 值起始值为 1，而不是 0。**
- **:nth-last-child(n)：**父元素的倒数第 n 个子元素。此选择器与 :nth-child(n) 选择器计算顺序刚好相反，但使用方法都是一样的，其中 :nth-last-child(1) 始终匹配最后一个元素，与 last-child 等同。



**注意：**

使用结构伪类选择器的好处是可以根据元素在文档中所处的位置，**来动态地选择元素，从而减少 HTML 文档对 id 或类的依赖**，有助于保持代码干净整洁。
另外需要注意的是，**在结构伪类选择器中，子元素的序号是从 1 开始的。**

------



## 1.43其他伪类：



### 1.3.1-----visited选择器：

**`:visited`** CSS[伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)表示用户已访问过的链接。出于隐私原因，可以使用此选择器修改的样式**非常有限**。

- 允许使用的 CSS 属性为[`color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color), [`background-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-color), [`border-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-color), [`border-bottom-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-bottom-color), [`border-left-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-left-color), [`border-right-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-right-color), [`border-top-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-top-color), [`column-rule-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-rule-color), 和[`outline-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-color)。

```html
<a href="#test-visited-link">你是否访问过此链接？</a>
<a href="">你已经访问过此链接。</a>

```

```css
a {
  /* 指定某些属性的默认值，允许他们使用：visited 状态进行样式设置 */
  background-color: white;
  border: 1px solid white;
}

a:visited {
  background-color: yellow;
  border-color: hotpink;
  color: hotpink;
}

```



------

## 2.伪元素：

### 2.1概括与总结对比：

| 伪元素         | 例子               | 例子描述                                              |
| -------------- | ------------------ | ----------------------------------------------------- |
| ::after        | p::after           | 在每个 <p> 元素之后插入内容                           |
| ::before       | p::before          | 在每个 <p> 元素之前插入内容                           |
| ::first-letter | p::first-letter    | 匹配每个 <p> 元素中内容的首字母                       |
| ::first-line   | p::first-line      | 匹配每个 <p> 元素中内容的首行                         |
| ::selection    | p::selection       | 匹配用户选择的元素部分                                |
| ::placeholder  | input::placeholder | 匹配每个表单输入框（例如 <input>）的 placeholder 属性 |





### 2.2 ::after伪元素:

伪元素 ::after 能够在指定元素的后面插入一些内容，在 ::after 中需要使用 content 属性来定义要追加的内容，而且在 **::after 中必须定义 content 属性才会生效**（没有需要插入的内容时可以将 **content 属性的值定义为空`""`**）。



#### **小案例：**

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p.one::after {
            content:"";
            display: inline-block;
            width: 50px;
            height: 10px;
            background: blue;
        }
        p.two::after {
            content:"要插入的内容";
            color: red;
            font-size: 6px;
        }
        p.three::after {
            content: url('./smiley.gif');
            position: relative;
            top: 8px;
        }
    </style>
</head>
<body>
    <p class="one">伪元素 ::after</p>
    <p class="two">伪元素 ::after</p>
    <p class="three">伪元素 ::after</p>
</body>
</html>
```

------

2.3 ::before伪元素:

伪元素 ::before 能够在指定元素的前面插入一些内容。与 ::after 相似，::before 中也需要使用 content 属性来定义要追加的内容，而且在 ::before 中必须定义 content 属性才会生效（没有需要插入的内容时可以将 content 属性的值定义为空`""`）。



```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p.one::before {
            content:"";
            display: inline-block;
            width: 50px;
            height: 10px;
            background: blue;
        }
        p.two::before {
            content:"要插入的内容";
            color: red;
            font-size: 6px;
        }
        p.three::before {
            content: url('./smiley.gif');
            position: relative;
            top: 8px;
        }
    </style>
</head>
<body>
    <p class="one">伪元素 ::before</p>
    <p class="two">伪元素 ::before</p>
    <p class="three">伪元素 ::before</p>
</body>
</html>
```

------



### 2.4 ::first-letter伪元素:

伪元素 ::first-letter 用来设置指定元素中内容**第一个字符的样式，**通常用来**配合 font-size 和 float 属性制作首字下沉效果**。需要注意的是，伪元素 ::first-letter **仅可以用于块级元素**，行内元素想要使用该伪元素，则需要先将其转换为块级元素



```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p::first-letter{
            font-size: 2em;
            color: blue;
        }
    </style>
</head>
<body>
    <p>伪元素 ::first-letter</p>
</body>
</html>
```



------

2.5 ::first-line伪元素:

伪元素 **::first-line 用来设置指定元素中内容第一行的样式，**与 ::first-letter 类似，**伪元素 ::first-line 也仅可以用于块级元素**，行内元素想要使用该伪元素，则需要先将其转换为块级元素。

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p::first-line{
            font-size: 1.5em;
            color: blue;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <p>伪元素 ::first-line 用来设置指定元素中内容第一行的样式，与 ::first-letter 类似，伪元素 ::first-line 也仅可以用于块级元素，行内元素想要使用该伪元素，则需要先将其转换为块级元素。</p>
</body>
</html>
```





------

### 2.5 ::selection伪元素:

伪元素 ::selection 用来设置对象被选中时的样式，需要注意的是，伪元素 ::selection 中只能定义元素被**选中时的 color、background、cursor、outline 以及 text-shadow（IE11 尚不支持定义该属性）等属性。**

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        p::selection{
            color: red;
            background-color: #CCC;
        }
    </style>
</head>
<body>
    <p>伪元素 ::selection 用来设置对象被选中时的样式，需要注意的是，伪元素 ::selection 中只能定义元素被选中时的 color、background、cursor、outline 以及 text-shadow（IE11 尚不支持定义该属性）等属性。 </p>
</body>
</html>
```



------

### 2.6 ::placeholder伪元素:

伪元素 ::placeholder 用来设置表单元素（<input>、<textarea> 元素）的占位文本（通过 HTML 的 placeholder 属性设置的文本）

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        input.text::placeholder{
            color: red;
            background-color: #CCC;
        }
    </style>
</head>
<body>
    <input placeholder="请输入一段文本">未使用伪元素 ::placeholder<br>
    <input placeholder="请输入一段文本" class="text">使用伪元素 ::placeholder 的效果
</body>
</html>
```



------

3.
