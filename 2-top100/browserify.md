# 利用Node.js实现浏览器端的 JS模块化加载

这是前端模块打包管理工具之一，与require.js，webpack等具备相同的目的，虽然是后来者，但发展很快。

Browserify 可以让你使用类似于 node 的 require() 的方式来组织浏览器端的 Javascript 代码，通过预编译让前端 Javascript 可以直接使用 Node NPM 安装的一些库。

安装：

```
npm install -g browserify
```

示例

这是 main.js 的内容，像普通的 nodejs 程序那样使用 require() 加载库和文件：

```
var foo = require('./foo.js');
var bar = require('../lib/bar.js');
var gamma = require('gamma');

var elem = document.getElementById('result');
var x = foo(100) + bar('baz');
elem.textContent = gamma(x);
```

导出的方法：

```
module.exports = function (n) { return n * 111 }
```

使用 browserify 编译：

```
$ browserify main.js > bundle.js
```

现在 main.js 需要的所有其它文件都会被编译进 bundle.js 中，包括很多层 require() 的情况也会一起被递归式的编译过来。

编译好的 js 可以直接拿到浏览器使用

```
<script src="bundle.js"></script>
```

参考：


https://github.com/substack/node-browserify

https://github.com/webpack/webpack

http://www.tuicool.com/articles/QZVzeuU

[Browserify vs. Webpack 这篇文章推崇使用Browserify](http://www.oschina.net/translate/browserify-vs-webpack)

[前端模块打包管理工具](http://www.oschina.net/question/1035386_226887)


