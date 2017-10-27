---
layout: post
title: vue-cli中引入jQuery
---


 　自行用bower下载的jQuery引入到项目中，一种方法可在script标签中直接通过import引用，亲测可用，缺点是不能使用别名，例如$符号。
 另一种在webpack.config.js文件中设置，如下

```javascript 
      resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      'jquery': path.resolve(__dirname, 'bower_components/jquery/dist/jquery.min.js'),
    }
  },
```
resolve:模块的处理方案
 
resolve.alias：设置模块别名，便于我们更方便引用
通过在resolve.alias对引用的文件设置别名，对引用的模块名称进行简写和地址重定向。
接下来在module.exports的最后加入，不加入不能用别名
```javascript 
		plugins: [
    		new webpack.ProvidePlugin({
    			$: "jquery",
      			jQuery: "jquery"
   			 })
  		],
```
于是就完成了，引入css文件可在style标签内@import 'path/to/xx'