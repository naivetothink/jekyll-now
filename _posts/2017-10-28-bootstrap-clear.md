---
layout: post
title: 使用bootstrap栅格空一格会怎样？
---



　在使用bootstrap栅格时如果空一格会怎么样呢？答案是后面的元素会跑到缺口那里去。无论是在container容器内部的元素还是外部的元素。这时候就需要浮动了。但发现无论是给元素本身加还是col元素或者是row元素加会发现浮动带来的影响一直存在。经发现如果目标元素在container元素内，怎么折腾都没用。而目标元素在container容器外的可以给container容器加个清浮动的class。目前认为，如果非要不凑够12格，那么就再添加一个container容器吧，别再有缺口的容器上瞎折腾了。
　以下是清浮动的代码：
```css
	.cl:after,
	.clearfix:after{
		content:".";display:block;height:0;clear:both;visibility:hidden
	}
	.cl,.clearfix{zoom:1}
```
2017.10.28