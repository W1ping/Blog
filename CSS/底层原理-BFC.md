# BFC


相信大家在各类前端学习教程或者视频中都会看到这样一段代码

```html
.clearfix::after {
  content :'';
  display: block;
  clear: both;
}
```

我们也知道教程会说给要清除高度塌陷的元素加上这个属性就能解决问题，但是教程却没说为什么这样写，本文章将深入带你理解高度塌陷产生的原因和怎么解决，并解释这段代码的由来。

## BFC 定义

MDN 的解释
块格式化上下文（Block Formatting Context，BFC） 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

听起来云里雾里，这是文档的一个问题，用一堆不理解的概念名词来解释不理解的概念名词。
简单的来说 BFC 是一块独立的渲染区域，它规定在该区域内，**常规流**的  **块盒** 的布局，且创建 BFC 的元素，隔绝了它内部和外部的联系，内部的渲染不会影响到外部。

## BFC 的作用

创建 BFC 的元素，它的自动高度需要计算浮动元素
创建 BFC 的元素，它的边框盒不会与浮动元素重叠
创建 BFC 的元素，不会和它的子元素进行外边距合并
本文章暂时只讨论 浮动导致塌陷问题

## BFC 的创建
下列方式会创建块格式化上下文：

-   根元素（`<html>`）
-   浮动元素（[`float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float) 值不为 `none`）
-   绝对定位元素（[`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position) 值为 `absolute` 或 `fixed`）
-   行内块元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `inline-block`）
-   表格单元格（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `table-cell`，HTML 表格单元格默认值）
-   表格标题（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `table-caption`，HTML 表格标题默认值）
-   匿名表格单元格元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `table`、`table-row`、 `table-row-group`、`table-header-group`、`table-footer-group`（分别是 HTML table、tr、tbody、thead、tfoot 的默认值）或 `inline-table`）
-   [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 值不为 `visible`、`clip` 的块元素
-   [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `flow-root` 的元素
-   [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 值为 `layout`、`content` 或 `paint` 的元素
-   弹性元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `flex` 或 `inline-flex` 元素的直接子元素），如果它们本身既不是 [flex](https://developer.mozilla.org/zh-CN/docs/Glossary/Flex_Container)、[grid](https://developer.mozilla.org/zh-CN/docs/Glossary/Grid_Container) 也不是 [table](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_table) 容器
-   网格元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `grid` 或 `inline-grid` 元素的直接子元素），如果它们本身既不是 [flex](https://developer.mozilla.org/zh-CN/docs/Glossary/Flex_Container)、[grid](https://developer.mozilla.org/zh-CN/docs/Glossary/Grid_Container) 也不是 [table](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_table) 容器
-   多列容器（[`column-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count) 或 [`column-width` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/column-width "Currently only available in English (US)") 值不为 `auto`，包括`column-count` 为 `1`）
-   `column-span` 值为 `all` 的元素始终会创建一个新的 BFC，即使该元素没有包裹在一个多列容器中 ([规范变更](https://github.com/w3c/csswg-drafts/commit/a8634b96900279916bd6c505fda88dda71d8ec51), [Chrome bug](https://bugs.chromium.org/p/chromium/issues/detail?id=709362))
总结起来就是以下

1.  根元素html标签
2.  float属性不为none的时候
3.  overflow属性不为visible的时候
4.  display属性为inline-block、table-cell、table-caption、flex、inline-flex（其余不常见不需要记忆）的时候
5.  position属性为absolute或fixed的时候
6.  contain 值为 layout, content, paint 的时候

## 高度塌陷

高度塌陷是什么意思？
我们知道，如果一个父元素不设置固定高度值，那么他的高度将由子元素撑开来看下面代码
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1700dd285ca477c9da6cbd84012eeae~tplv-k3u1fbpfcp-zoom-1.image)
我们可以看到父元素的高度由子元素撑开也为 100px, 为什么宽度是一整个屏幕，这问题也很简单，因为 div 是块级元素，独占一行，这里不展开。
此时我们给 子元素 son1 设置浮动
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f1924169dc2b4383804f14af6e6dd1a7~tplv-k3u1fbpfcp-zoom-1.image)
此时我们可以看到父元素的高度变成了 0px
我们再看一个例子
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/837fb224e2464531a0ab0ad21fb1e9da~tplv-k3u1fbpfcp-zoom-1.image)
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0c970ac06baa4a45829841509796ce2c~tplv-k3u1fbpfcp-zoom-1.image)
看到这里我们得出一个结论，父元素的高度（如果没有给定一个固定的值）是由子元素撑开，但是  **父元素计算高度时不会加上设置了浮动属性的子元素**
**父元素高度计算不会加上设置了浮动属性的子元素**
**父元素高度计算不会加上设置了浮动属性的子元素**
**父元素高度计算不会加上设置了浮动属性的子元素**
这句话非常关键，这句话解释了高度塌陷的原因，也提供了解决高度塌陷的方法。
高度塌陷既然是因为没有计算浮动元素高度造成的，那么什么方法能让父元素计算高度时加上浮动元素的高度呢？
还记得上面说到 BFC 作用第一条说的就是  创建 BFC 的元素，它的自动高度需要计算浮动元素。

## BFC 解决高度塌陷的演化

解决高度塌陷，创建 BFC 块

1.  根元素html标签
    根标签在这里不符合，略过

2.  float属性不为none的时候\
    上代码：
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5761b3818bd94052983b75167f8d80f8~tplv-k3u1fbpfcp-zoom-1.image)
    我们可以看到给父元素设置了 float 属性后（float:right 同理），父元素的高度再次由子元素 撑开值为 100px.

3.  overflow属性不为visible的时候
    上代码：
    这里只测试其中两个属性，其他属性同理，自己可以动手试试。![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/221fb947c5124cdabba4f3f21f42a05a~tplv-k3u1fbpfcp-zoom-1.image)
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7862f26c470d445d86bd013316d17e1a~tplv-k3u1fbpfcp-zoom-1.image)
    当 overflow为visible 时高度再次塌陷，因为此时没有创建 BFC 块
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9dead6a0bca4ced9b098f3929c9da7b~tplv-k3u1fbpfcp-zoom-1.image)

4.  display属性为inline-block、table-cell、table-caption、flex、inline-flex（其余不常见不需要记忆）的时候
    上代码：
    其他情况同理，这里不多做测试![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c7e17d82d8144750803bf40be3c21744~tplv-k3u1fbpfcp-zoom-1.image)

5.  position属性为absolute或fixed的时候
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3a60adfd9d05433e9166e3fa37cf46d6~tplv-k3u1fbpfcp-zoom-1.image)
    当 position 为 relative 时
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f74040d4a6994cde8bf22fe296b927a8~tplv-k3u1fbpfcp-zoom-1.image)
    由此我们可以看到创建了 BFC 的元素确实会重新计算 浮动元素的高度。
    回到开头的那个问题，给父元素加上 clearfix 类
    ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c7f58be69d164906b22032ea1dfef1f7~tplv-k3u1fbpfcp-zoom-1.image)

6.  根元素html标签

7.  float属性不为none的时候

8.  overflow属性不为visible的时候

9.  display属性为inline-block、table-cell、table-caption、flex、inline-flex（其余不常见不需要记忆）的时候

10. position属性为absolute或fixed的时候

11. contain 值为 layout, content, paint 的时候

在上面演示的五种创建 BFC 块的方法中都可以解决高度塌陷问题，但是每种方法多多少少会有一些缺陷，或者说是会破坏我们希望的布局。我们细致列出每次情况可能会遇到的问题。

根元素 html 标签
这个没什么好说的，一个页面就一个根标签，我们不可能创建多个。

float 属性不为 none 的时候
float 属性会让父元素离开原有的流动模型，会破坏我们原先的理想效果，得不偿失

overflow 不为 visible
讲道理 overflow 属性设置为 hidden 时影响也挺小的，但如果设置为 scroll 的话，会出现滚动条边框，视觉效果还是挺明显的不推荐，当然你本来就像滚动条出现那没事了。

display 的多个属性
设置为 inline-block 影响其实也不大，因为设置为 行块盒不会对布局产生影响，但是影响大的是其他属性，比如设置为 flex 时，会变成 flex 布局模型。

position 属性为 absolute 或者 fixed 的时候
创建 BFC 块时这两个属性影响估计是最大的，设置这两个属性会使该 BFC 块真正的脱离文档流，用这两个属性创建 BFC 块极不推荐

最后有同学可能会说可以给父元素设置一个固定高度，这确实是个解决方法，但是也有个致命缺陷，我们知道在日常的开发中，我们很多情况一个元素的高度是不确定的，比如你刷评论区的时候有加载更多按钮，这时我们不可能给父元素设置固定高度，我们只能让高度由子元素撑开。

看回开头的代码

```html
.clearfix::after {
  content :'';
  display: block;
  clear: both;
}
```

给要清除浮动的元素添加一个伪元素，这样写的目的也是为了把未知的影响降到最低，我们知道这里最关键的一个属性是 clear:both ，（clear是用来清除浮动用到，可选值有 float,left,both）, 但是如果我们直接这样子写：
![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d1a0e2742b40473e88e0fe71ee01eee5~tplv-k3u1fbpfcp-zoom-1.image)
无疑改变了原有的 DOM  结构，所以我们使用 伪元素 ::after，因为[伪元素](%28https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements%29)是 CSS 渲染是才加上去的，而且不会改变原有的 DOM 结构，简单的说就是 伪元素不算 DOM元素。
