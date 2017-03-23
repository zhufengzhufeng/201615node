---
title: Node.js模块导出exports和module.exports的区别
---

#### Node.js模块导出exports和module.exports的区别（学习笔记）
每一个Node.js执行文件，都自动创建一个module对象，同时，module对象会创建一个叫exports的属性，初始化的值是 {}
```
(function(exports,require,module,__filename,__dirname){
       module.exports = exports = {}
       return module.exports
  })

```
```
1 、 module.exports = {};
```
exports和module.exports指向同一块内存，但require()返回的是module.exports而不是exports

```
var str = "difference"
exports.a = str;
exports.b = function () {

}

```

给 exports 赋值其实是给 module.exports 这个空对象添加了两个属性而已，上面的代码相当于：
```
var str = "difference"
module.exports.a = str;
module.exports.b = function () {

}

```

先来看下exports和module.exports的使用

使用exports

app.js

```
var s = require("./log");
s.log("hello");

```
log.js
```
exports.log =function (str) {
    console.log(str);
}
```

使用module.exports

app.js
```
var s = require("./log");
s.log("hello");

```
log.js

```
module.exports =function (str) {
    console.log(str);
}

```
上述两种用法都没问题，但如果这样使用
```
  exports = function (str) {
    console.log(str);
}
```
运行程序就会报错。

前面的例子中给 exports 添加属性，只是对 exports 指向的内存做了修改。而上例则是对exports指向的内存进行了覆盖，使exports指向了一块新的内存，这样，exports和module.exports指向的内存并不是同一块，exports和module.exports并无任何关系。exports指向的内存有了变化，而module.exports指向的内存并无变化，仍为空对象{}。

require得到的会是一个空对象，则会有
```
TypeError: s.log is not a function

```
报错信息。

再看下面的例子

app.js

```
 var x = require('./init');

 console.log(x.a)
```
init.js

```
 module.exports = {a: 2}
 exports.a = 1 
```
运行app.js会有输出
```
2
```

这也就是module.exports对象不为空的时候exports对象就自动忽略，因为module.exports通过赋值方式已经和exports对象指向的变量不同了，exports对象怎么改和module.exports对象没关系了。

```
 exports = module.exports = somethings
```
等价于
```
  module.exports = somethings
  exports = module.exports
```


原因也很简单， module.exports = somethings 是对 module.exports 进行了覆盖，此时 module.exports 和 exports 的关系断裂，module.exports 指向了新的内存块，而 exports 还是指向原来的内存块，为了让 module.exports 和 exports 还是指向同一块内存或者说指向同一个 “对象”，所以我们就 exports = module.exports 。

最后，关于exports和module.exports的关系可以总结为

```
1 、module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {}
2 、exports 是指向的 module.exports 的引用
3 、require() 返回的是 module.exports 而不是 exports

```