<img src="https://www.danilovesovic.com/images/course_images/FJZI8uJf76hpXGsnwwzmlvlwmzAaCJuG7nwR2Ac9.jpeg" style="zoom: 25%;" />

## 1. 什么是[Flex](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3DFlex%26spm%3D1001.2101.3001.7020)布局

`Flex`是[Flexible](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3DFlexible%26spm%3D1001.2101.3001.7020) Box的缩写，`flex`布局表示弹性布局，可以为盒状模型提供最大的灵活性。

任何一种元素都可以指定为 `flex布局`

```css
.wrap{
    display:flex;
}
复制代码
```

行内元素也可以使用`flex`布局

```arduino
.box{
  display: inline-flex;
}
复制代码
```

`Webkit`内核的浏览器，必须加上`-webkit`前缀

```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
复制代码
```

## 2. Flex布局中的基本概念

采用 `flex` 布局的元素被称作容器。

在 `flex` 布局中的子元素被称作项目。

```css
即父级元素采用flex布局，则父级元素为容器，全部子元素自动成为项目
复制代码
```

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/607c0e333f9d46ef822a8bac07dcfc64~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

## 3. 容器的一些属性

有六个常用属性设置在容器上，分别为：

- flex-direction
- flex-wrap
- flew-flow
- justify-content
- align-items
- align-content

### 3.1 flex-direction

**设置容器主轴的方向**

```sql
.wrap{
    flex-direction:row | row-reverse | column | column=reverse;
}
复制代码
```

包含四个属性值：

`row`: 默认值，表示沿水平方向，由左到右。

`row-reverse`：表示沿水平方向，由右到左

`column`：表示垂直方向，由上到下

`column-reverse`:表示垂直方向，由下到上

### 3.2 flex-wrap

**设置当项目在容器中一行无法显示的时候如何处理**

```lua
.wrap{
    flex-wrap:nowrap | wrap | wrap-reverse;
}
复制代码
```

包含三个属性值：

`nowrap`：表示不换行（设置的项目的宽度就失效了，强行在一行显示）

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/158d1a9b38884c4abbd02d40ceff2e3b~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`wrap`：正常换行，第一个位于第一行的第一个

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ded797e58154725acf2f3d45fa06471~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`wrap-reverse`：向上换行，第一行位于下方

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/25824fed52d74beea22cb66cde9c2828~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

### 3.3 flex-flow

**flex-deriction和flex-wrap属性的简写，默认值为[row nowrap]**

第一个属性值为`flex-direction`的属性值

第二个属性值为`flex-wrap`的属性值

### 3.4 justify-content

**设置项目在容器中的对齐方式。**

```sql
.wrap{
    justify-content: flex-start | flex-end | center |space-between | space-around
}
复制代码
```

该属性主要要五个属性值：

`flex-start`：默认值，左对齐
 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d247687fdd7c44729a7e9544c0dfb6c7~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`flex-end`：右对齐
 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/24ac929375684fedba11c96fd7cfc70f~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`center`：居中对齐
 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4faac3ff64834084ba851cfdf20c9398~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`space-between`：两端对齐
 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5917249083124428b57a392ff5ee67c4~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`space-around`：每个项目两侧的间距相等
 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7c0ee7da10814496bfdc22a7369ea317~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

### 3.5 align-items

**定义项目在交叉轴上是如何对齐显示的**

```css
.wrap{
    align-items:flex-start | flex-end | center | baseline | stretch
}
复制代码
```

该属性主要有五个属性值：（以交叉轴从上向下为例）

`flex-start`: 交叉轴的起点对齐

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1ade3da03c62413c85831006f6f1d6f2~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`flex-end`：交叉轴的终点对齐

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4abdffe1780345c89c153dde056adb6a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`center`：交叉轴居中对齐

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a8797e9d03b4a3d98b8c5cfa0f00936~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

`baseline`：项目的第一行文字的基线对齐

`stretch`：默认值，如果项目未设置高度或者高度为auto，将占满整个容器的高度

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88171dd8142d46ef870149faa9e8ff63~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

### 3.6 align-content

**定义多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。**

```sql
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
复制代码
```

该属性主要要六个属性值：

`flex-start`：与交叉轴的起点对齐。
 `flex-end`：与交叉轴的终点对齐。
 `center`：与交叉轴的中点对齐。
 `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
 `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
 `stretch`：默认值，轴线占满整个交叉轴。

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b53ccd3a74a74dfba99d45269d69b38c~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

## 4. 项目的一些属性

有五个常用属性设置在项目上，分别为：

- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

### 4.1 flex-grow

**该属性用来控制当前项目是否放大显示。 默认值为0，表示即使容器有剩余空间也不放大显示。 如果设置为1，则平均分摊后放大显示。**

```css
.green-item{
    flex-grow:2;
}
复制代码
```

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b7c2191ce5e45a98e47421899841a96~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

### 4.2 flex-shrink

**该属性表示元素的缩小比例。 默认值为1，如果空间不够用时所有的项目同比缩小。 如果一个项目的该属性设置为0，则空间不足时该项目也不缩小。**

### 4.3 flex-basis

**该属性表示表示项目占据主轴空间的值。 默认为auto，表示项目当前默认的大小。 如果设置为一个固定的值，则该项目在容器中占据固定的大小。**

### 4.4 flex

**该属性是 flex-grow属性、flex-shrink属性、flex-basis属性的简写。 默认值为：0 1 auto；**

```c
.item{
    flex:(0 1 auto) | auto(1 1 auto) | none (0 0 auto)
}
复制代码
```

### 4.5 align-self

**该属性表示当前项目可以和其他项目拥有不一样的对齐方式**

该属性主要要六个属性值：

`auto`：默认值，和父元素`align-self`的值一致

`flex-start`：顶端对齐

`flex-end`：底部对齐

`center`：竖直方向上居中对齐

`baseline`：`item`第一行文字的底部对齐

`stretch`：当`item`未设置高度时，`item`将和容器等高对齐

```css
.item{
    align-self: flex-start | flex-end | center | baseline | stretch 
}
复制代码
```

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/041ecff3a3f44fbebb14d5a74baa0261~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)


链接：https://juejin.cn/post/7192179524514086971