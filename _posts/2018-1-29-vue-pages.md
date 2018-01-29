---
layout: post
title: vue 多页面的配置
---


1、vue init webpack
2、npm install glob -D ,glob是nodeJs读取本地文件名称的工具
3、修改build下的文件
	（1）修改webpack.base.conf.js
		添加以下代码：
```
var glob = require('glob')；
var entries = getEntry('./src/pages/**/*.js')
```
		将module.exports中的
```
entry: {
   app: './src/main.js'
  },
```
		注释掉，然后添加这一行代码：
```
entry: entries,
```
		entries的数据需要添加一个方法获得
```
		//获取入口js文件
		function getEntry(globPath) {
		 var entries = {},
		  basename, tmp, pathname;
		 
		 glob.sync(globPath).forEach(function (entry) {
		  //获取js文件名称 如entry为'./src/pages/**/hehe.js'，则过滤为hehe
		  basename = path.basename(entry, path.extname(entry));
		  //用/分割取后3个，如entry为'./src/pages/**/hehe.js'，则结果为[ 'pages', '**', 'hehe.js' ]
		  tmp = entry.split('/').splice(-3);
		  // splice(1,1) 从第二个开始删除一个，如结果为‘**’
		  // pathname = tmp.splice(1, 1) + '/' + basename;
		  pathname = tmp.splice(1, 1);
		  entries[pathname] = entry;
		  // entries[basename] = entry;
		 });
		 return entries;
		}
```
		这里把入口的名称设置为了文件夹的名称，不然还需要把文件夹里的文件包括HTML文件和js文件都改成相同的名字。这样太麻烦了，还是直接用文件夹名称方便，方便以后直接复制粘贴文件夹。
		这个文件改到这里就可以了，只是把单入口改为了多入口。文件的名称要和HtmlWebpackPlugin里的chunks对应上。chunks不填写会引入全部的js文件（入口文件），填写一个则只引入这一个的js文件。
		（2）修改webpack.dev.conf.js
		添加以下代码：
```
		//引入
		var glob = require('glob')
		var path = require('path')
```
		将module.exports中的plugins里的
```
		new HtmlWebpackPlugin({
		  filename: 'index.html',
		  template: 'index.html',
		  inject: true
		}),
```
		注释掉，然后添加以下代码：
```
		function getEntry(globPath) {
		  
		 var entries = {},tmp,basename, pathname;
		 
		 glob.sync(globPath).forEach(function (entry) {
		  basename = path.basename(entry, path.extname(entry));
		  tmp = entry.split('/').splice(-3);
		  // splice(1,1) 从第二个开始删除一个，如结果为‘**’
		  // pathname = tmp.splice(1, 1) + '/' + basename;
		  pathname = tmp.splice(1, 1);
		  //pathname为文件夹的名称
		  entries[pathname] = entry;
		 });
		 return entries;
		}
		 
		var pages = getEntry('./src/modules/**/*.html');
```	
		这个函数写在var glob = require('glob')后面即可。
		然后就要用for in把pages的属性名遍历出来了，该方法不要用来遍历数组，遍历数组结果是0,1,2等等，哈哈。此时的属性名已经是文件夹的名称了。
		在这段代码
```
		module.exports = new Promise((resolve, reject) => {
		  portfinder.basePort = process.env.PORT || config.dev.port
		  portfinder.getPort((err, port) => {
		    if (err) {
		      reject(err)
		    } else {
```	
		后边添加以下代码（就是else语句里）
```
		for (var pathname in pages) {
		 // 配置生成的html文件，定义路径等
		 // 此时的pathname已经为文件夹的名称

		 var conf = {
		  filename: pathname + '.html',
		    // filename: "haha" + '.html',
		  template: pages[pathname],  // 模板路径
		  inject: true,       // js插入位置
		  chunks:[pathname]
		  // chunks:['index'] 
		 };
		 devWebpackConfig.plugins.push(new HtmlWebpackPlugin(conf));
		}
```
		看别的教程里把for in 语句写在外面，然后最后一段话是这样的，如下
```
		module.exports.plugins.push(new HtmlWebpackPlugin(conf));
```
		我用的时候不知为何老是出错，提示找不到push这个方法，只有退而求其次写在语句里面了。
		这样开发环境就差不多了，在src目录下新建个modules文件夹，在该文件夹了放入相关文件。webpack会自动生成对应文件夹名称的js文件和html文件，至于原本文件夹里的文件名称已经不必在意了。只是只能放一个js文件和html文件，但那又何妨，想有多个页面可以用vue-router呀。
		（3）webpack.prod.conf.js
		这个文件修改的套路与上一个文件类似
		将webpackConfig中的plugins里的
```
new HtmlWebpackPlugin({
  filename: config.build.index,
  template: 'index.html',
  inject: true,
  minify: {
   removeComments: true,
   collapseWhitespace: true,
   removeAttributeQuotes: true
  },
  chunksSortMode: 'dependency'
}),
```
		注释掉，在声明定义webpackConfig的后面添加以下代码：
```javascript
function getEntry(globPath) {
  
 var entries = {},tmp,basename, pathname;
 
 glob.sync(globPath).forEach(function (entry) {
  basename = path.basename(entry, path.extname(entry));
  tmp = entry.split('/').splice(-3);
  // splice(1,1) 从第二个开始删除一个，如结果为‘**’
  // pathname = tmp.splice(1, 1) + '/' + basename;
  pathname = tmp.splice(1, 1);
  //pathname为文件夹的名称
  entries[pathname] = entry;
 });
 return entries;
}
var glob = require('glob')
var pages = getEntry('./src/modules/**/*.html');
for (var pathname in pages) {
 var conf = {
   filename: process.env.NODE_ENV === 'testing'
    ? pathname + '.html'
    : config.build[pathname],
   template: pages[pathname],
   inject: true,
   minify: {
    removeComments: true,
    collapseWhitespace: true,
    removeAttributeQuotes: true
   },
    chunksSortMode: 'dependency', // dependency 页面中引入的js按照依赖关系排序；manual 页面中引入的js按照下面的chunks的数组中的顺序排序；
     
    chunks: ['manifest', 'vender', pathname] // 生成的页面中引入的js，'manifest', 'vender'这两个js是webpack在打包过程中抽取出的一些公共方法依赖，其中，'manifest'又是从'vender'中抽取得到的，所以这三个js文件的依赖关系是 pathname依赖 'vender'，'vender'依赖'manifest'.

  }
 webpackConfig.plugins.push(new HtmlWebpackPlugin(conf));
}
```
		此时，这个文件也修改好了。
		3、修改config下的文件
		这个文件夹下，只需要修改一个文件：index.js 这个文件的作用是，寻找文件路径，然后根据这个文件设置的目录层级，生成打包后的文件以及相应的层级文件结构。 添加以下代码：
```
var build= {
    // Template for index.html
    // index: path.resolve(__dirname, '../dist/index.html'),
    // family:path.resolve(__dirname, '../dist/family.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '../dist',

    /**
     * Source Maps
     */

    productionSourceMap: true,
    // https://webpack.js.org/configuration/devtool/#production
    devtool: '#source-map',

    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],

    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  }
var pages = getEntry('./src/modules/**/*.html');
//入口 index: path.resolve(__dirname, '../dist/index.html')
for (var pathname in pages) {
 build[pathname] = path.resolve(__dirname, '../dist/' + pathname + '.html')
}

```
		然后将module.exports中的build的值换成我们刚刚添加声明的变量build。 如果希望修改打包后的目层级结构，可以在build中修改；还可以在build中增加我们需要定义的变量，比如我们需要将fabfile.py和favicon.ico拷贝到dist目录下的a目录下，就可以在build中定义一个属性，
```
distA:path.resolve(__dirname, '../dist/a),
```
		然后因为在webpack.prod.conf.js中已经引入了'copy-webpack-plugin'（var CopyWebpackPlugin = require('copy-webpack-plugin')），我们就可以在 webpackConfig.plugins下添加如下代码：
```
new CopyWebpackPlugin([
  {
   from: path.resolve(__dirname, '../fabfile.py'),
   to: config.build.distA,
   template: 'fabfile.py'
  }
 ])
new CopyWebpackPlugin([
  {
   from: path.resolve(__dirname, '../favicon.ico'),
   to: config.build.distA,
   template: 'favicon.ico'
  }
 ])
```
		5、打包
		做完以上修改，本地运行没有问题，如果打包后有问题，出现报错：webpackJsonp is not defined
		解决方式如下： 在webpack.prod.conf.js文件下的for (var pathname in pages)循环中定义的conf里，添加两行代码：

		chunksSortMode: 'dependency', // dependency 页面中引入的js按照依赖关系排序；manual 页面中引入的js按照下面的chunks的数组中的顺序排序；
		 
		chunks: ['manifest', 'vender', pathname] // 生成的页面中引入的js，'manifest', 'vender'这两个js是webpack在打包过程中抽取出的一些公共方法依赖，其中，'manifest'又是从'vender'中抽取得到的，所以这三个js文件的依赖关系是 pathname依赖 'vender'，'vender'依赖'manifest'.
		引入这两个公共chunks，就不会出现js代码缺少公共依赖不运行了。
		综上，就是本次项目从单页面到多页面项目的转变历程。
2018.1.29