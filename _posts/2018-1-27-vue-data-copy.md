---
layout: post
title: vue中数据的复制
---


在vue中数据是双向绑定的，有时候需要一份单独的数据或者说是默认数据，这时候可以用数据的深复制，浅复制对象的话只是复制了引用。当然也可以用笨方法，比如再添加一份相同的数据，但那毕竟不好看不是吗？
可以用以下方法来复制
```
      copyData: function () {  
         var obj={};  
         obj=JSON.parse(JSON.stringify(this.urls)); 
         this.copyUrls=obj;  
         // return obj  
      },

```
再在mounted中运行一下this.copyData()就可以了，这样就有了一份相同的数据并且不会改变。网上有把这段代码放到computed里的，但不知为何数据还是会改变。
2018.1.27