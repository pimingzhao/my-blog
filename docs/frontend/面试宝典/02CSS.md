## 盒模型

基本构成部分：`margin` 、`border` 、`padding` 、`content` 

在 ie 浏览器中盒子的 `width` 等于 `border + padding + contentW` ；在谷歌浏览器中 `width` 等于 `contentW` 。

为了解决两者的差异性，可以设置 `box-sizing` 属性，它有以下三个属性值：

- content-box （默认值，cs2.1标准，`width === content width`）
- border-box （`width === border + padding + content width`）
- inherit（规定应从父元素继承 box-sizing 属性的值）

## flex 布局

阮一峰的教程：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

### 常见的问题

- flex 快捷设置（等同于同时设置了 `flex-grow` 、`flex-shrink` 、`flex-basis` ）
  - flex: auto;  指 flex: 1 1 auto;
  - flex: none; 指 flex: 0 0 auto;
  - flex: 1; 指 flex: 1 1 0%;
  - `flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大
  - `flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
  - `flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小

## Grid 布局

阮一峰的教程：[http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html) 