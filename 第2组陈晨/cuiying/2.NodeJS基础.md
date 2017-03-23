
###NodeJS基础
- NODE不是后台开发语言,我们了解的JAVA、PHP、C#、.NET(dot net)...才是后台开发语言；
- NODE仅仅是一个基于V8引擎来渲染和解析JS的平台和工具(有点类似浏览器)；
- 真实的开发中，我们一般都把NODE安装在服务器端，这样我们就可以在服务器端编写一些JS代码来处理服务器端的业务逻辑了。

####1.NODE如何去执行JS代码
- 直接利用WB：在JS的空白处，右键，找到 Run xxx.js 就是在WB中调取NODE环境执行我们的JS代码
- 在当前JS所在的文件目录中打开DOS窗口，在DOS中执行 node xxx.js 也相当于把JS代码在NODE环境下执行了
- NODE的REPL命令

####2.NODE的优势
- 快：基于V8引擎渲染和解析的
- NODE是单线程基于事件驱动的异步操作

>单线程和多线程：
>node主线程是单线程的，进程中包含线程，正常Java一个子进程中包含多个线程，node中一个进程只能包含一个线程，允许开子进程。
>同步和异步：
>代码从上到下执行，先同步再异步，异步不会阻塞主线程
>阻塞和非阻塞：
>针对内核来说的，非阻塞是异步的前置条件
>阻塞是按顺序执行的，而非阻塞是不需要按顺序的，所以如果需要处理回调函数的参数，我们就需要写在回调函数内。   

- NODE提供了无阻塞的I/O操作

>I/O：input/output 对文件的增删改查操作
>- JS在客户端浏览器中运行的时候是否可以对客户端本地的文件进行I/O操作?
>  + 不可以，因为我们要保证客户端的安全，JS做为前端开发语言是不可以操作
>  + 客户端本地文件的，但是浏览器也提供了部分需要用户自主操作的功能，例如：上传图片
>- JS在服务器端NODE环境下运行的时候是否可以对服务器上的文件进行I/O操作？可以的

####3.学习NODE从模块开始学起
- 核心模块（内置模块）：自身带的  http、url、fs，引用文件要使用相对路径
- 文件模块（自定义模块）：自己写的
- 第三方模块：别人写好的，我们来调取使用 
    + 安装NODE之后自带npm命令，用来管理第三方模块的，所有我们需要调取的第三方模块都在https://www.npmjs.com/这个网站中
    
#####第三方模块
```
使用npm安装第三方模块：
  
  1.安装到全局（只能在命令行里使用）
  
    安装：npm install xxx -g
    卸载：npm uninstall xxx -g
    
    - 把模块安装到全局，不管在哪个项目目录下，我们都可以通过使用命令操作的方式来进行操作，但是不能通过代码导入模块使用
    
    - 安装到全局很多时候会导致版本号冲突
    
    - 我们在开发时，一般把第三方模块安装在当前项目中
    npm install nodeapp -g
    
    npm install http-server -g
    http-server start
    
    
  2.安装在当前项目中（在代码里使用）
  
    安装：npm install xxx
    卸载：npm uninstall xxx
    
    - 把模块安装在当前项目中，不能使用命令，但是可以使用Js导入了
      解决方法：在package.json文件中的scripts这个属性中进行配置
    
    - 在当前的项目中会有一个node_modiles文件夹，里面存储的都是我们当前项目所需要使用的模块
      解决方法：上传GitHub的时候在项目目录中增加一个文件：.gitignore，这个文件可以配置上传时忽略那些文件，被忽略的文件内容不被上传
    
    - 安装在A项目中的模块不能在B项目中使用，B项目需要自己安装模块
      解决方法：需要在安装第三方模块时，把当前项目所需要的模块清单写入到package.json的开发依赖项中
      npm install xxx --save-dev 放在开发依赖模块清单中
      npm install xxx --save 放在生产依赖模块清单中
      作用：如果下一个项目还需要这些模块，我们只需把package.json文件copy到下一个项目中，然后执行npm install即可，
    
    -平时在做项目开发时，首先会在当前项目的根目录下执行 npm init -y 命令，然后会在当前项目的根目录下生成一个文件：package.json
       {
         "name": "20170222", //项目名称
         "version": "1.0.0", //项目的版本号
         "description": "", //当前项目的描述
         "main": "temp.js", //当前项目的入口
         "dependencies": {  //当前项目依赖的第三方模块清单(生产:项目发布上线)
           "less": "^2.7.2"
         },
         "devDependencies": {}, //当前项目依赖的第三方模块清单(开发:项目正在研发)
         "scripts": {//命令脚本
           "test": "echo \"Error: no test specified\" && exit 1"
         },
         "keywords": [],
         "author": "",
         "license": "ISC"
       }
       
```
>在NODE环境下，它认为每一个JS都可以理解为一个单独的模块，所以我们创建模块只需要创建JS文件即可；并且每一个模块和模块之间是互不干扰的；


**模块之间的相互调用**
  需求：
  A ：sum：任意数求和
  B ：avg：求任意几个数的平均数->调取A中的sum
  C ：调取B中的avg实现获取 12,23,34,45,56,67,78,89 这几个数的平均数
```
A文件： 
  function sum() {
      var total = null;
      //周氏继承法,让arg指向Array.prototype,这样ARG就可以调取数组中的方法了(__proto__在IE下不兼容,但是在NODE环境下可以放心使用)
      arguments.__proto__ = Array.prototype;
      arguments.forEach(function (item, index) {
          item = Number(item);
          !isNaN(item) ? total += item : null;
      });
      return total;
  }
  module.exports = {
      sum: sum
  };

B文件：
  var a = require('./A');
  function avg() {
      arguments.__proto__ = Array.prototype;
      arguments.sort(function (a, b) {
          return a - b;
      });
      arguments.shift();
      arguments.pop();
  
      //a.sum(arguments);//->a.sum([...]) 我们想要的把数组中的每一项一个个的传递给sum
      var total = a.sum.apply(null, arguments);
      return (total / arguments.length).toFixed(2);
  }
  module.exports = {
      avg: avg
  };
  
C文件：
  var b = require('./B');
  var result = b.avg(1, 2, 3, 4, 5, 6, 7);
  console.log(result);

```

#####内置模块
- 服务器端需要处理的事情
  + 在服务器上创建一个服务(监听一个端口)
  + 接收(解析)客户端的请求信息
  + 找到客户端需要的资源文件中的源代码
  + 把源代码返回给客户端

######(1)HTTP
- 这个模块主要用于创建服务、监听端口、接收请求、返回内容的管理
  
  http.createServer：创建一个服务

######(2)URL
- 主要用来解析URL地址的
  
  url.parse：解析一个完整的URL(也可以不完整)中的每一项的

######(3)FS
- 提供一些方法供开发者进行I/O的操作

  + fs.readFileSync([pathname])：同步读取指定文件中的内容，读取出来的内容是字符串格式的
  + fs.readFile([pathname],[callback])：异步读取文件中的内容

  + fs.writeFileSync([pathname],[content])：同步把内容写入到文件中，写入的内容需要是字符串格式的，而且写入属于覆盖式写入
fs.writeFile...


>- sync:同步
>- async:异步

```
var http = require('http'),
    url = require('url'),
    fs = require('fs');
var server = http.createServer(function (req, res) {
    var urlObj = url.parse(req.url, true),
        pathname = urlObj.pathname,
        query = urlObj.query;

    //->客户端静态资源文件请求的处理
    var reg = /\.([0-9a-zA-Z]+)/i;
    if (reg.test(pathname)) {
        var conFile = null,
            status = 200;
        try {
            conFile = fs.readFileSync('.' + pathname);
        } catch (e) {
            conFile = 'not found~';
            status = 404;
        }
        var suffix = reg.exec(pathname)[1].toUpperCase(),
            suffixMIME = 'text/plain';
        switch (suffix) {
            case 'HTML':
                suffixMIME = 'text/html';
                break;
            case 'CSS':
                suffixMIME = 'text/css';
                break;
            case 'JS':
                suffixMIME = 'text/javascript';
                break;
        }
        res.writeHead(status, {
            'content-type': suffixMIME + ';charset=utf-8;'
        });
        res.end(conFile);
    }
});
server.listen(8080);
```


