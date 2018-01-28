---
layout: post
title: vue循环输出图片
---


 经常写的代码中，有些会感觉到很重复，比如：
```
 	<div>
 		<div>
 			<img src="" alt="">
 			<p></p>
 		</div>
 		<div>
 			<img src="" alt="">
 			<p></p>
 		</div>
 		<div>
 			<img src="" alt="">
 			<p></p>
 		</div>
 	</div>
```
这时候一个一个移动光标再输入图片url很很烦，于是想到了循环输出URL。用了v-for以后，咦，发现图片怎么不显示？打开控制台一看原来是找不到地址。哦，用的是相对地址，如:./imags/icon.png.
在服务器下面找不到这些图片，而不用循环直接写在img元素里面的用相对地址可以显示。在控制台中可以看到图片在根目录下，图片名后面还有hash值。看来在webpack中用vue输出不能循环相对地址，改成绝对 地址来输出就好了，例如:/src/family/images/icon.png
图片地址统一配置是不是方便了很多。
2018.1.26