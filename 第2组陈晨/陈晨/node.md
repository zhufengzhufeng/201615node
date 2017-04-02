---
title: Node初学者 （学习笔记）
---

#### Node （学习笔记）
#### Node 是什么
+ Node.js是一个让JS可以运行在浏览器之外的服务器端的平台 
+ 它实现了诸如文件系统、模块、包、操作系统API，网络通信等核心JS没有或不
完善的功能
+ 它摒弃传统平台依赖多线程来实现高并发的设计思路，而采用单线程，异步式
I/O，事件驱动式的程序设计模型
+ Node.js使用了来自于Google ChromeV8引擎。V8是目前世界上最快的
Javascript引擎
#### Node的优点
+ Nodejs基于Javascript语言 {:&.zoomIn}
+ 统一公共类库，代码标准化
+ Nodejs选择了目前最快的浏览器内核V8做为执行引擎
+ Nodejs的社区非常活跃
####Node 的概念
##### 一、非阻塞或异步I/O
> 由于Node.js是一个服务器端框架，因此它的主要工作之一就是处理来自浏览器的请求。在传统的I/O系统中，只有先前请求的响应返回来后，新的请求才能发出。这也就是为什么称之为阻塞I/O的通信。服务器阻塞了下一个到来的请求，然后处理当前的请求，直到请求处理完成，发出响应，再解除下一个到来的请求的阻塞。
> + 1、阻塞式I/O
>     - 针对内核来说，阻塞式是同步的前置条件
> + 2、非阻塞式I/0
>		- 针对内核来说，非阻塞式是异步的前置条件
#####二、原型
先来看下exports和module.exports的使用
**例1：原型**
 使用module.exports

person.js
```
function Person() {}
Person.prototype.eat = function () {
    console.log('吃');
};
module.exports = Person;
```
usePerson.js

```
//在usePerson使用eat方法
var Person = require('./person.js');
//new 类 调用原型上的方法
var p = new Person();
p.eat();
```


使用exports
person.js
```
function Person() {}
Person.prototype.eat = function () {
    console.log('吃');
};
exports.Person= Person; 
```
usePerson.js

```
//在usePerson使用eat方法
var Person = require('./person.js').person;
//new 类 调用原型上的方法
var p = new Person();
p.eat();
```
##### 例2：对象

使用 module.exports

calc.js
```
var calc = {
    "+":function (a,b) {
        return a+b;
    },
    "-":function (a,b) {
        return a-b
    }
};

module.exports=calc;
```
usecalc.js
```
var calc=require('./calc.js');
//使用+法和减法
console.log(calc["+"](1,2));
```
使用 exports

calc.js
```
var calc = {
    "+":function (a,b) {
        return a+b;
    },
    "-":function (a,b) {
        return a-b
    }
};

exports.calc=calc;
```
usecalc.js
```
var calc=require('./calc.js').calc;
//使用+法和减法
console.log(calc["+"](1,2));
```
#####三、模块
#####1、核心模块
> 即内置模块，常见的核心模块包括：HTTP模块、URL模块、EVENTS模块、文件系统模块等;导入模块用require导入只需传入模块名，require是同步方法
```
// 导入核心模块
var http = require('http);
```
#####2、用户自定义模块
>即 自己写的模块;用户定义的模块同样需要require导入。require导入还需要使用相对路径；require是同步方法
```
 // 导入用户定义的模块
var something = require('./something.js');
```
#####3、第三方模块
> 需要使用npm install 模块名称  来安装
##### 四、回调
```
function read(callback) {
    setTimeout(function () {
        var data = 'hello world';
    },2000);
}
//解决异步编程问题
read(function (data) {
    console.log(data);
});
```

