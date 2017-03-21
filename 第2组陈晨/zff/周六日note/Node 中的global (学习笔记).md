---
title: Node 中的global (学习笔记)
---

#### Node 中的global (学习笔记)
在Node每个文件执行前都会套用一个自执行函数，在这个函数中this指向发生了改变
```
console.log(this);
等价于
;(function(){
console.log(this);
})()
得出{}
```
**在js中执行函数中的this是window，而在Node中自执行函数是global**
####Node.js 全局对象与全局变量
**1 、全局变量**
 + __filename 
 表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。
```
console.log( __filename );
```
+ __dirname
表示当前执行脚本所在的目录。
```
console.log( __dirname );
```
+ process
 是一个全局变量，即 global 对象的属性。
```
process.pid 当前的进程id
process.kill()杀死某个进程
process.exit()退出自己
process.cwd() current working directory 可以更改
process.chdir() 改变目录
```
+ global
是一个全局变量
```
global.a=100;
挂载在global对象上的属性 可以在任何模块中使用

var a = 100; //用var声明的不会挂载在global属性上
```

**2 、全局函数**
+ setTimeout(fn, ms)
全局函数在指定的毫秒(ms)数后执行指定函数(fn)。：setTimeout() 只执行一次指定函数
```
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setTimeout(printHello, 2000);
```
+ clearTimeout(t)
全局函数用于停止一个之前通过 setTimeout() 创建的定时器。 参数 t 是通过 setTimeout() 函数创建的定时器。
```
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setTimeout(printHello, 2000);

// 清除定时器
clearTimeout(t);
```

+ setInterval(fn, ms)
全局函数在指定的毫秒(ms)数后执行指定函数(fn)
```
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setInterval(printHello, 2000);
```
+ clearInterval(t)
全局函数用于停止一个之前通过 setInterval() 创建的定时器。 参数 t 是通过 clearInterval() 函数创建的定时器。
```
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setInterval(printHello, 2000);

// 清除定时器
clearInterval(t);
```
```
function eatrace(who,who1) {
    console.log(who+'吃米饭'+who1);
};
setTimeout(eatrace,2000,'我','你');

//this指的是setTimeout自己,改变this指向使用bind方法
/*var a = 1;
setTimeout(function () {
    console.log(this);
}.bind(a));*/
```
+ setImmediate 
```
setImmediate = setTimeout(function(){},0);
```
```
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

console.log(102);
默认nextTick>setImmediate>= setTimeout
```
+ console
控制台标准输出
	- console.log()
	- console.info()
	- console.error([data][, ...])
	- console.warn([data][, ...])
	- console.dir(obj[, options])
	- console.time(label)    输出时间，表示计时开始。
	- console.timeEnd(label)    结束时间，表示计时结束。
```
console.time('count');
for(var i = 0; i<1000000000;i++){}
//用来计算客户端访问服务器端所用的时间、代码所用的时间
console.timeEnd('count');*/
```
