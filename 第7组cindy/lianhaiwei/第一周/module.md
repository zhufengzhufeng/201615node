# 模块

### 三种模块
    - 文件模块 自己写的模块 引用文件模块要使用相对路径
    - 核心模块、内置模块 node自带的本来就有的天生的
    - 第三方模块，需要安装下

### 模块导出
exports && module.exports 是什么
```javascript
//他们的初始对象都是 {}
console.log(exports); // {}
console.log(module.exports); // {}
```

exports && module.exports使用
```javascript
let util = { };

// exports 导出 这样导出只能一个一个的导出
exports.util = util;

//module.exports 导出可以导出一个对象
module.exports = {
    util: util
}
// module.exports 也可以直接导出
module.exports = util;
```

exports 和 module.exports 为什么使用不一样
```
//为什么？require的机制
// 其实 exports = module.exports 他们指向的是一个内存地址；
// 而require最后却返回时 module.exports 如果exports换内存地址了,exports就和返回值没关系,就获取不到值了;
// cnode的详细介绍:http://www.cnblogs.com/pigtail/archive/2013/01/14/2859555.html
```

### 模块引入
```javascript
//证实一下require的返回值确实是module.exports;
//2.js
let util = {
    eat:function () {
        console.log('eat');
    }
} ;
// exports 已经指向另一个地址了
exports = { out:function () { } };

module.exports = util;

//1.js
let a = require('./2');
console.log(a); // { eat: [Function: eat] }
```

### require的查询机制
- 文件模块是相对路径引入的
- 首先会查询当前目录下node_modules 文件夹 如果没有就会去上级的目录找node_modules ；直到根目录
