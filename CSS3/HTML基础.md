# 一、块级格式化上下文

#### 1、什么是块级格式化上下文

  浮动、绝对定位的元素、块容器(例如inline块、表格单元格和表标题)等非块盒，和“overflow”不为“visible”的块容器(除非这个值被传播到viewport中)会它们的内容建立新的块级格式化上下文。 具有 BFC 的元素与普通的容器没有什么区别，但是从功能上，具有 BFC 的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器没有的一些特性，例如可以包含浮动元素，清除浮动的方法（如 overflow 方法）就是触发了浮动元素的父元素的 BFC ，使到它可以包含浮动元素，从而防止出现高度塌陷的问题。 

#### 2、如何触发BFC

- float 的值不为 none
- position 的值不为 static 或 relative
- display 的值为 table-cell、table-caption、inline-block、flex 或 inline-flex
- overflow 的值不为 visiable
- 对于 display:table 的元素，产生 block formatting contexts 的是匿名框而不是 display:table。

#### 3、BFC的作用

1、利用BFC可以消除Margin Collapse

2、利用BFC去容纳浮动元素

overFlow能够清除浮动的原因，floatw为right的时候会建立一个块级格式化上下文，而父元素此时还在html那个块级格式化上文中中，由于没有了子元素，因此高度自然就塌陷了，此时父元素overflow:hidden等于父元素与子元素又在一个块级格式化上下文当中了因此，高度自然的就被撑开了。