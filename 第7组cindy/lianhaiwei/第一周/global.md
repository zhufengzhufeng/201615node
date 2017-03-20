## 全局变量
```javascript
// 每当node执行文件的时候就会在整个文件的外层包一层自执行函数,相当于全局的了
(function(exports,require,module,__dirname,__filename){

})()
exports //导出模块
require //引入模块
module  //导出模块
__dirname  //当前文件的绝对路径 /Users/lianhaiwei/Desktop/home/201612nodedemo/module/person/useperson.js
__filename //当前文件所在文件夹的绝对路径 /Users/lianhaiwei/Desktop/home/201612nodedemo/module/person
```

## 注意点
- 用`var`声明的变量并不是全局变量
```javascript
var a = 1;
console.log(global.a); //undefined
```
- 声明全局变量的
```javascript
global.a = 1;
console.log(global.a); // 1
```


## 全局对象

node中的全局对象是`global`

### global对象
```javascript
console.log(global);
{ ...
  global:
  process:
  Buffer:
  clearImmediate: [Function],
  clearInterval: [Function],
  clearTimeout: [Function],
  setImmediate: [Function],
  setInterval: [Function],
  setTimeout: [Function],
  console: [Getter]
}
```

### process
```javascript
process.pid     //当前的进程id
process.kill()  //杀死某个进程
process.exit()  //退出自己
process.cwd()   //current working directory 可以更改
process.chdir() //改变目录
process.nextTick(callback) //[详解] http://www.oschina.net/translate/understanding-process-next-tick?print
```

### Buffer
```javascript
// Buffer 存放二进制数据的缓存区。
```

### setImmediate
```javascript
//setImmediate 相当于
setImmediate = setTimeout(function(){},0);
```

### console
```javascript
console log/dir/error/warn/info/time/timeEnd
```
