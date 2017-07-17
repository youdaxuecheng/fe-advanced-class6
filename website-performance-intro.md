### 一、浏览器解析页面

浏览器在收到 HTML 文档之后会对文档进行解析开始构建 DOM (Document Object Model) 树，进而在文档中发现样式表，开始解析 CSS 来构建 CSSOM（CSS Object Model）树，这两者都构建完成后，开始构建渲染树。整个过程如下：

![Render Tree](https://user-images.githubusercontent.com/966009/28268853-6fc5a562-6b32-11e7-87fe-08f87baebdc1.png)


### 二、性能优化

#### 1. 减少DOM的操作

DOM查找，插入等操作都是比较消耗性能的，所以性能优化的一个方法就是减少DOM相关的操作。

#### 2. 避免强制性同步布局
在 JavaScript 中读取到的布局信息都是上一帧的信息，如果在 JavaScript 中修改了页面的布局，比如给某个元素添加了一个类，然后再读取布局信息。这个时候为了获得真实的布局信息，浏览器需要强制性对页面进行布局。因此应该避免这样做。

#### 3.优化渲染性能
浏览器通常每秒更新页面 60 次，每一帧的时间就是 16.6ms，为了能让浏览器保持 60帧 的帧率，为了让动画看起来流畅，需要保证帧率达到 60fps，因此每一帧的逻辑需要在 16.6ms 内完成。


>每一帧实际上都包含下列步骤：

  ● JavaScript：改变元素样式，添加元素到 DOM 中等等

  ● Style：元素的类或者style改变了，这个时候需要重新计算元素的样式

  ● Layout：需要重新计算元素的具体尺寸

  ● Paint：将元素的绘制的图层上

  ● Composite：合并多个图层

**示例**
比如：修改了元素的width还height，或top。浏览器会重新计算布局，并对整个页面进行重排。


比如：修改lbackground-color，仅仅修改了页面重绘的属性，不会影响页面布局，浏览器会跳过计算布局(layout)过程，只进行重绘。(Paint)
举例：

更多内容可以参考[CSS trigggers](https://csstriggers.com/)




#### 4. 使用 transform 和 opacity 来完成动画

在以往开发过程中，前端开发习惯使用操作DOM+CSS来制作动画。而如今期望开发者都是用`transform`和`opacity`来制作简单的动画效果，对这两个属性的修改不需要经历 layout 和 paint 过程。

### 三、注意事项
#### 1. CSS阻塞渲染
通常情况下 CSS 被认为是阻塞渲染的资源，在CSSOM 构建完成之前，页面不会被渲染，放在顶部让样式表能够尽早开始加载。但如果把引入样式表的 link 放在文档底部，页面虽然能立刻呈现出来，但是页面加载出来的时候会是没有样式的，是混乱的。当后来样式表加载进来后，页面会立即进行重绘，这也就是通常所说的闪烁了。


#### 2 JavaScript 阻塞文档解析
当在 HTML 文档中遇到 script 标签后控制权将交给 JavaScript，在 JavaScript 下载并执行完成之前，都不会解析 HTML。因此如果将 JavaScript 放在文档顶部，恰好这个时候 JavaScript 脚本加载的特别慢，用户将会等待很长一段时间，这段个时候 HTML 文档还没有解析到 body 部分，页面会是空白的。

**小结**

默认情况下，CSS样式放在head标签内部，JavaScript文件放在body标签闭合前面


### 参考链接

1. [Chrome开发者工具](https://developers.google.com/web/tools/chrome-devtools/)

2. [前端性能优化的三个维度](http://www.jianshu.com/p/a5d9938ed60f)

3. [关于前端性能优化的 23 条建议](https://juejin.im/entry/5822f1840ce4630058a8d837)
