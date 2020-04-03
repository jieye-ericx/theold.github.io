## 1. 怎么让一个 div 水平垂直居中

```html
<div class="parent">
  <div class="child"></div>
</div>
```

+ 简单粗暴

  ```css
  div.parent {
    background-color:red;
    height:300px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  div.child {
    background-color:blue;
    height:50px;
    width:20%;
  }
  ```

+ ```css
  div.parent {
      background-color:red;
    	height:300px;
      position: relative; 
  }
  div.child {
      background-color:blue;
    	height:50px;
    	width:20%;
      position: absolute; 
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);  
      //主要原理是先top后child位于父亲的150-200高度，left后位于50%-70%宽度，再自身transform50%后,就能向左上平移半个自己的身位，正好居中
  }
  ```

+ ```csharp
  div.parent {
      display: grid;
  }
  div.child {
      justify-self: center;
      align-self: center;
  }
  ```

## 2. 分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景。

- `display: none;`

1. **DOM 结构**：浏览器不会渲染 `display` 属性为 `none` 的元素，不占据空间；
2. **事件监听**：无法进行 DOM 事件监听；
3. **性能**：动态改变此属性时会引起重排，性能较差；
4. **继承**：不会被子元素继承，毕竟子类也不会被渲染；
5. **transition**：`transition` 不支持 `display`。

- `visibility: hidden;`

1. **DOM 结构**：元素被隐藏，但是会被渲染不会消失，占据空间；
2. **事件监听**：无法进行 DOM 事件监听；
3. **性 能**：动态改变此属性时会引起重绘，性能较高；
4. **继 承**：会被子元素继承，子元素可以通过设置 `visibility: visible;` 来取消隐藏；
5. **transition**：`transition` 不支持 `display`。

- opacity: 0;

1. **DOM 结构**：透明度为 100%，元素隐藏，占据空间；
2. **事件监听**：可以进行 DOM 事件监听；
3. **性 能**：提升为合成层，不会触发重绘，性能较高；
4. **继 承**：会被子元素继承,且，子元素并不能通过 `opacity: 1` 来取消隐藏；
5. **transition**：`transition` 不支持 `opacity`。

## 3. 如何修改才能让图片宽度为 300px ？注意下面代码不可修改。

`<img src="1.jpg" style="width:480px!important;">`

<hr>


增加`max-width:300px`或`transform: scale(0.625)`,但后者元素本身大小还是480。

## 4. BFC、IFC、GFC 和 FFC

**BFC（Block formatting contexts）：块级格式上下文** 页面上的一个隔离的渲染区域，那么他是如何产生的呢？可以触发BFC的元素有float、position、overflow、display：table-cell/ inline-block/table-caption ；BFC有什么作用呢？比如说实现多栏布局’

**IFC（Inline formatting contexts）：内联格式上下文** IFC的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的padding/margin影响)IFC中的line box一般左右都贴紧整个IFC，但是会因为float元素而扰乱。float元素会位于IFC与与line box之间，使得line box宽度缩短。 同个ifc下的多个line box高度会不同 IFC中时不可能有块级元素的，当插入块级元素时（如p中插入div）会产生两个匿名块与div分隔开，即产生两个IFC，每个IFC对外表现为块级元素，与div垂直排列。 那么IFC一般有什么用呢？ 水平居中：当一个块要在环境中水平居中时，设置其为inline-block则会在外层产生IFC，通过text-align则可以使其水平居中。 垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他行内元素则可以在此父元素下垂直居中。

**GFC（GrideLayout formatting contexts）：网格布局格式化上下文** 当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。那么GFC有什么用呢，和table又有什么区别呢？首先同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。

**FFC（Flex formatting contexts）:自适应格式上下文** display值为flex或者inline-flex的元素将会生成自适应容器（flex container），可惜这个牛逼的属性只有谷歌和火狐支持，不过在移动端也足够了，至少safari和chrome还是OK的，毕竟这俩在移动端才是王道。Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局。

## 5. 如何用 css 或 js 实现多行文本溢出省略效果，考虑兼容性

+ 单行： overflow: hidden; text-overflow:ellipsis; white-space: nowrap; 
+ 多行： display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 3; //行数 overflow: hidden; 
+ 兼容： p{position: relative; line-height: 20px; max-height: 40px;overflow: hidden;} p::after{content: "..."; position: absolute; bottom: 0; right: 0; padding-left: 40px; background: -webkit-linear-gradient(left, transparent, #fff 55%); background: -o-linear-gradient(right, transparent, #fff 55%); background: -moz-linear-gradient(right, transparent, #fff 55%); background: linear-gradient(to right, transparent, #fff 55%); }

## 6.介绍下BFC及其应用

BFC特性：

1. 内部box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定，在一个BFC中，两个相邻的块级盒子的垂直外边距会产生折叠。
3. 在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）
4. 形成了BFC的区域不会与float box重叠
5. 计算BFC高度时，浮动元素也参与计算

生成BFC除了 @webproblem 童鞋所说的还有：行内块元素、网格布局、contain值为layout、content或 strict的元素等。 更多生成BFC的方法：[传送门](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

BFC作用：

1. 利用特性4可实现左图右文之类的效果：

```html
<img src='image.png'>
<p>我是超长的文字<p>
img {
    float:left
}
p {
    overflow:hidden
}
```

1. 利用特性5可以解决浮动元素造成的父元素高度塌陷问题：

```html
<div class='parent'>
    <div class='float'>浮动元素</div>
</div>
.parent {
    overflow:hidden;
}
.float {
    float:left;
}
```
