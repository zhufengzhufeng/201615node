## node 1
####单线程和多线程
- node主线程是单线程的，进程中包含线程，正常java一个进程包含多个线程，node中一个进程只能包含一个线程，允许开子进程

####同步 异步
- 代码从上到下执行，先走同步再走异步，异步不会阻塞主线程
```

for(var i=0;i<100;i++){
 console.log(i)
 }
```

####阻塞  非阻塞
		- 是对于内核来说的，非阻塞是异步的前置条件

#### callback  回调函数
- 要回调来解决异步编程问题

#### 异步的文件读写、callback 、定时器  能用异步不用同步
```
function read(callback){
  setTimeout(function(){
    var data='hello world'
    callback(data);
  },2000)
}
// 解决异步编程问题
read(function(data){
  console.log(data);
 })
```

####事件环

### node全局对象
- 在任意地点可以直接访问的
- 在global上挂载的都是全局对象


#### node 中常用的几个方法：
```
     process
     Buffer
     global
     clearImmediate: [Function],
     clearInterval: [Function],
     clearTimeout: [Function],
     setImmediate: [Function],
     setInterval: [Function],
     setTimeout: [Function],
     console: [Getter]
```

- node的this是指 this = global
- node中的变量挂载在global对象上的属性 可以在任何模块中使用
```
var a = 100;
console.log(global.a)
```
- node中用来计算访问服务器端所用的时差：
```
time/timeEnd
```
- node中改变this的只能用  bind
```
 var a = 1;
setTimeout(function () {
    console.log(this);
}.bind(a));
```
- setImmediate setTimeout process.nextTick三者的区别：
```
console.log(100);
setImmediate(function () {
    console.log('setImmediate');
});
console.log(101);
setTimeout(function () {
    console.log('setTimeout');
},20);
process.nextTick(function () {
    console.log('nextTick');
});

但是其中：
- setImmediate = setTimeout(function(){},0);
- 默认nextTick>setImmediate>= setTimeout
```
####node中函数中的参数：
1 五个参数是形参，可以在任何文件中使用，我们也叫他全局变量
```
   (function(exports,require,module,__dirname,__filename){
      __dirname
   })()

其中console.log(__dirname); //路径是不可变的
   console.log(__filename);//当前文件所在文件夹的绝对路径
```
2 global
```
  process.pid 当前的进程id
  process.kill()杀死某个进程
  process.exit()退出自己
  process.cwd() current working directory 可以更改
  process.chdir() 改变目录
console.log(process.cwd());
console.log(__dirname); //当前绝对路径
```

