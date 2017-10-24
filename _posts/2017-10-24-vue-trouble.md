---
layout: post
title: 谷歌翻译引发的vue错误
---


 　　使用vue中，在更改数组中的数据发现视图不能随着数据更改，代码如下:

```html
<template>
  <div id="home2">
<button @click="change()">点我</button>
<ul id="ula">
	<li  v-for="(val,index) in lists"  >{{val}}</li>
</ul>
  </div> 
</template>
<script>
import Vue from 'Vue'
export default {
  name: 'home2',
  data(){
  	return {
  		msg:'Welcome to home2',

  		lists:['red','blue','yellow','orange','green'],
  	}
  },
   methods:{
  	change:function () {
  		//视野组件改写数据有时不成功，后来发现是谷歌翻译的问题
 		//this.li.shift()
		this.lists.splice(1,1)
		
  	}
  }  
}
</script>
```
　　发现总是会删除数据，视图总是删除最后一个数据。而本应该视图应删除第一个数据。给li元素添加：key也不行。后来又查了官网文档和相关资料后，使用了Vue.set()方法后还是不行。理论上使用这些方法可以触发视图重新渲染的啊。慢慢倒腾一段时间后，无意中发现视图能正常更新了。记住相关的设置后，刷新页面，视图又不能正常更新。因为用的是谷歌浏览器，有自动翻译的功能。在看到它自动翻译的时候，想会不会是谷歌翻译的问题。把翻译关掉后，视图更新和数据匹配。哈哈，看来有时候bug不一定是代码的问题。

2017.10.24　　

	


 　　

	

