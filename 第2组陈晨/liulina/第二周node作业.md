#### Buffer
> NodeJs提供了一个Buffer全局对象来提供对二进制数据的操作， 固定内存分配。Buffer好比由一个多位字节元素组成的数组， 在javascript中表示二进制数据

##### 定义buffer的三种方式
```
//通过长度定义buffer
new Buffer(size);//size为字节数
//通过数组定义buffer
new Buffer（array）；//array中的值为0-255之间.
//字符串创建
new Buffer(str, [encoding]);
```

##### buffer常用方法
fill() 用对256取模的结果 替换原来的值
```
fill(0)
```
write 方法  用于向buffer里面写内容 
```
//write(string, offset, length, encoding)//
buffer.write('hello');
buffer.write('world', 5);//写多次必须制定offset
```
toString方法 将buffer转换成字符串类型 start end 是截取的buffer的长度
```
toString('utf8', start, end)
```
数组的方法，如slice 方法, forEach 也可以使用
```
buffer.forEach(function (item) {
	console.log(item);//得到的是10进制
});
``` 
copy 方法 复制Buffer， 把多个buffer拷贝到一个大buffer上
```
//sourceBuffer.copy(targetBuffer, targetstart， sourcestart, sourceend);

var buffer = new Buffer(12);
var buf1 = new Buffer('天天');
var buf2 = new Buffer('向上');
buf1.copy(buffer, 0);
buf2.copy(buffer, 6);
console.log(buffer.toString());//天天向上
```

concat 方法 将多个小的连接为一个大的buffer
```
//Buffer.concat([buf1, buf2], length);

var buf1 = new Buffer('天天');
var buf2 = new Buffer('向上');
console.log(Buffer.concat([buf1,buf2]).toString());//天天向上

//concat实现原理
Buffer.myConcat = function (list,totalLength) {
    //1.判断长度是否传递
    if(typeof totalLength == "undefined"){
        totalLength = 0;
        list.forEach(function (item) {
            totalLength += item.length;
        });
    }
    var buffer = new Buffer(totalLength);//1000
    var index = 0;
    list.forEach(function (item) {
        item.copy(buffer,index);
        index+= item.length;
    });
    return buffer.slice(0,index);
};
```
isBuffer 方法 判断是否为buffer
```
Buffer.isBuffer
```
length 获取字节长度(显示是字符串所代表buffer的长度)
```
Buffer.byteLength('姓名');
buffer.length;
```
##### 进制转换
```
//将任意进制字符串转换为十进制
parseInt("11", 2);//3 二进制转为十进制
parseInt("77", 8);//63 八转十
parseInt("e7", 16);//175 十六转十
//将十进制转换为其它进制字符串
(3).toString(2)//"11" 转2
(17).toString(16)//"11" 转16
(33).toString(32)//"11" 转32
```
>base64的转换 ， 不是加密算法,  加密的有 md5, sha1, sha256。举例说明：将汉字转换成base64 '珠' => 54+g 将得到的3*8个二进制数 按照 6*4 方式再进行转换.因为2进制装换成10进制不得大于64， 最后得到的结果在可见编码中取值
```
//转换过程如下:
//将汉字转换成2进制
var buffer = new Buffer('珠');//0xe7 0x8f 0xa0
console.log((0xe7).toString(2)); //11100111
console.log((0x8f).toString(2)); //10001111
console.log((0xa0).toString(2)); //10100000
将2进制转换成10进制 (6位的二进制前面补两个0)
console.log(parseInt('00111001',2)); //57
console.log(parseInt('00111000',2)); //56
console.log(parseInt('00111110',2)); //62
console.log(parseInt('00100000',2)); //32
var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
str+= 'abcdefghijklmnopqrstuvwxyz';
str+='0123456789';
str+='+/';
console.log(str[57]+str[56]+str[62]+str[32]);//54+g
```

#### FS核心模块
Node.js的文件系统的Api,同步方法和异步方法同时出现
>1. 读取文件 (readFile,readFileSync )
```
//能用异步 绝不用同步
//1.读取文件没有会报错
//2.读取默认格式是buffer类型  encoding:null
const fs = require('fs'); 
//同步方式
var school = {};
var name = fs.readFileSync('./name.txt','utf8');
school.name = name;
var age = fs.readFileSync('./age.txt','utf8');
school.age = age;
console.log(school);
//异步方式
var school = {}; //计数器的原理
fs.readFile('./name.txt','utf8',function (e,data) {
    if(e)console.log(e);
    school.name = data;
    out();
});
fs.readFile('./age.txt','utf8',function (e,data) {
    if(e)console.log(e);
    school.age = data;
    out();
});
function out() {
    if(Object.keys(school).length == 2){
        console.log(school); //{name:'nana',age:26}
    }
}
```
>2.写文件(writeFile, writeFileSync, appendFile, appendFileSync)
```
//1.如果写的文件不存在会创建文件
//2.默认写入格式是utf8格式
//3.文件有内容，清空文件内容
//如果放入的是buffer格式 会默认toString('utf8');
const fs = require('fs');
fs.writeFileSync('./a.txt',new Buffer('hello')); //同步
//异步
fs.writeFile('./a.txt',new Buffer('hello'),function (err) {
    console.log(err);
});
```
>3.拷贝的实现 
>- 同步 readFileSync+writeFileSync
>- 异步 readFile + writeFile
```
//异步方式
function copy(source,target) {//异步
    //先读在写
    fs.readFile(source,function (err,data) {
        if(err)console.log(err);
        //appendFile可以进行累加，不清空内容写入
        fs.writeFile(target,data,{encoding:'utf8',flag:'a'},function (err) {
            if(err)console.log(err);
            console.log('拷贝成功');
        });
    });
};
```

##### 目录操作
>创建目录 mkdirSync mkdir （要求父目录必须存在）
>读取目录下所有文件 readdirSync readdir
>查看文件目录信息 statSync stat
>文件是否存在 existsSync exists

##### 路径出路模块 path 
path是node中专门处理路径的一个核心模块
> path.join 将多个参数值字符串结合为一个路径字符串
```
path.resolve('1.js');
path.join(__dirname,'1.js');//相当于resolve
```
> path.basename 文件名
> path.extname 扩展名
> path.sep 文件分隔符
> path.delimiter 环境变量路径分隔符
> path.normalize 将非标准的路径字符串转化为标准路径字符串 特点:
> -  可以解析.和..
> - 多个杠可以转换成一个杠
> - 在window下反杠会转化成正杠
> - 如结尾以杠结尾的，则保留斜杠
> 
resolve 取得绝对路径特点:
> - 以应用程序根目录为起点
> - 如果参数是普通字符串，则意思是当前目录的下级目录
> - 如果参数是.. 回到上一级目录
> - 如果是/开头 表示一个绝对的根路径

创建多级目录
```
//同步
function makep(p) {
    var arr = p.split('/'); //a   a/b  a/b/c
    for(var i = 0;i<arr.length;i++){
        var path = arr.slice(0,i+1).join('/');
        if(!fs.existsSync(path)){
            fs.mkdirSync(path)
        }
    }
}
makep('a/b/c/d/e/f');
//异步(递归)
function makep(p) {
    var paths = p.split('/');
    var index = 1;
    function makeOne(subpath) {
        if(paths.length == index-1){
            return;
        }
        fs.exists(subpath,function (flag) {
            var temp = paths.slice(0,++index).join('/');
            if(!flag){ //不存在的时候创建
                fs.mkdir(subpath,function () {
                    makeOne(temp);// a/b
                });
            }else{ //存在的时候继续下一次
                makeOne(temp);
            }
        });
    }
    makeOne(paths[index-1]);
}
makep('a/b/c/d/e/f');
```

#### 事件
> 发布订阅模式， 定义对象间的一种 一对多的依赖关系，一个发布者可以对应多个 订阅者,当发布者 发生变化 的时候,他可以将消息一一通知给所有的订阅者,当一 个对象的状态发生改变时,所有依赖于它的对象都 得到通知并被自动更新 。
##### events模块
>node中很多模块都继承自events

1.绑定事件 
on方法和addListener方法绑定
on(event, listener);
addListener(event, listener);
```
var EventEmmiter = require('events'); var util = require('util');
function Girl() {} util.inherits(Girl,EventEmmiter); var girl = new Girl(); girl.on('帅哥来了',function () {
console.log('喜欢帅哥'); });
```
2.发射事件
> emit(event, args); 

3.绑定一次事件 多次触发只执行一次
> once(event, listener);

4.解除监听
> removeListener(event, listener);

5.解除所有监听
> removeAllListeners(event);
removeAllListeners();

6.设置最大监听数(默认最大为10， 超过会产生警告)
> setMaxListeners(count);

7.获取监听函数
> listeners(event);

8.获得监听个数
> listenerCount(event); 

订阅发布模拟
```
//订阅
Observable.prototype.on = Observable.prototype.addListener = function (
     if (this._events[event]) {
         this._events[event].push(listener);
     } else {
         this._events[event] = [listener];
     }
     if(this._events[event].length>this._maxListeners){
         console.error('maxListener reached!');
} }
//发布
Observable.prototype.emit = function(event){
     var args = Array.prototype.slice.call(arguments,1);
     if(this._events[event]){
         var listeners = this._events[event];
         listeners.forEach(function(listener){
             listener.apply(null,args);
         });
      }
 }
```
#### 流
> 流是一组有序的，有起点和终点的字节数据传输手段 它不关心文件的整体内容，只关注是否从文件中读到了数据，以及读到数据之后的处理 流是一个抽象接口，被 Node 中的很多对象所实现。比如HTTP 服务器request和response对象都是流。

1.可读流createReadStream
> 实现了stream.Readable接口的对象,将对象数据读取为流数据,当监听data事件后,开始发射数据
```
fs.createReadStream = function(path, options) {
  return new ReadStream(path, options);
};
util.inherits(ReadStream, Readable);
```
1.1 创建可读流
> var rs = fs.createReadStream(path,[options]); //返回可读流对象, 默认读出的内容是buffer.
path读取文件的路径(必须保证存在)
options
- flags打开文件要做的操作,默认为'r'
- encoding默认为null
- start开始读取的索引位置
- end结束读取的索引位置
- highWaterMark读取缓存区默认的大小64kb, 如果指定utf8编码highWaterMark要大于3个字节

1.1.1 监听data事件
流切换到流动模式,数据会被尽可能快的读出(默认是非流动模式)
```
rs.on('data', function (data) {
    console.log(data);
});
```
1.1.2 监听end事件
> 该事件会在读完数据后被触发
```
rs.on('end', function () {//rs.emit('end');如果没读完,不会触发.
    console.log('读取完成');
});
```
1.1.3 监听error事件
```
rs.on('error', function (err) {
    console.log(err);
});
```
1.1.4 设置编码
> 与指定{encoding:'utf8'}效果相同，设置编码
rs.setEncoding('utf8');

1.1.5暂停触发data,  恢复触发data
>通过pause()方法和resume()方法
```
rs.on('data', function (data) {
    rs.pause();
    console.log(data);
});
setTimeout(function () {
    rs.resume();
},2000);
```

2.可写流createWriteStream
>实现了stream.Writable接口的对象来将流数据写入到对象中
```
fs.createWriteStream = function(path, options) {
  return new WriteStream(path, options);
};

util.inherits(WriteStream, Writable);
```

2.1 创建可写流
>var ws = fs.createWriteStream(path,[options]);//文件不存在，会创建，会清空原有内容
path写入的文件路径
options
flags打开文件要做的操作,默认为'w'
encoding默认为utf8
highWaterMark写入缓存区的默认大小16kb

2.1.1 write方法
>ws.write(chunk,[encoding],[callback]);
chunk写入的数据buffer/string
encoding编码格式chunk为字符串时有用，可选
callback 写入成功后的回调
返回值为布尔值，系统缓存区满时为false,未满时为true

2.1.2 end方法
> ws.end(chunk,[encoding],[callback]);
调用该方法关闭文件,迫使系统缓存区的数据立即写入文件中。不能再次写入

2.1.3 drain方法
```
var fs = require('fs');
var ws = fs.createWriteStream('./2.txt',{highWaterMark:5});
var i = 0;
function write(){
    var flag = true;
    while (flag&&i<10){
        flag = ws.write(''+i++);
    }
}
write();
ws.on('drain', function () {
    write();
});
```
3.pipe方法
3.1 pipe方法的原理
```
var fs = require('fs');
var ws = fs.createWriteStream('./2.txt');
var rs = fs.createReadStream('./1.txt');
rs.on('data', function (data) {
    var flag = ws.write(data);
    if(!flag)
    rs.pause();
});
ws.on('drain', function () {
    rs.resume();
});
rs.on('end', function () {
    ws.end();
});
```
3.2 pipe用法
```
readStream.pipe(writeStream);
var from = fs.createReadStream('./1.txt');
var to = fs.createWriteStream('./2.txt');
from.pipe(to);
```
>将数据的滞留量限制到一个可接受的水平，以使得不同速度的来源和目标不会淹没可用内存。

#### 其他模块
url模块， http模块，mime模块的相关使用.
>http 模块
```
http.createServer(function (req, res) {
	//请求来时的回调
	//req.url
	//req.on();
	//res.setHeader('Content-Type','text/html;charset=utf8');
	//res.statusCode
	//res.write()
	//res.writeHead(status, {});
	//require('_http_server').STATUS_CODES[code] //状态码对应信息
	//res.end()
}).listen(9999, function () {
//监听成功后的回调
});
```
>url模块
```
var { pathname ,query } = url.parse(req.url,true);
```
> fs模块
```
//利用流来进行文件的读写
fs.createReadStream('./index.html').pipe(res);
```
>mime模块
```
//lookup根据路径，返回相应的mime类型
res.setHeader('Content-Type',mime.lookup(pathname)+';charset=utf8');
```

##### Promise 
解决异步回调问题
1.编程中遇到的问题
>1.1 如何同步异步请求
如果几个异步操作之间并没有前后顺序之分,但需要等多个异步操作都完成后才能执行后续的任务，无法实现并行节约时间
1.2 如何解决回调地狱
在需要多个操作的时候，会导致多个回调函数嵌套，导致代码不够直观，就是常说的回调地狱

2.Promise
> Promise本意是承诺，在程序中的意思就是承诺我过一段时间后会给你一个结果。 什么时候会用到过一段时间？答案是异步操作，异步是指可能比较长时间才有结果的才做，例如网络请求、读取本地文件等

3.Promise的三种状态
>Pending Promise对象实例创建时候的初始状态
Fulfilled 可以理解为成功的状态
Rejected 可以理解为失败的状态
then 方法就是用来指定Promise 对象的状态改变时确定执行的操作，resolve 时执行第一个函数（onFulfilled），reject 时执行第二个函数（onRejected）

4.构造一个Promise
4.1 promise的方法会立刻执行
```
let promise = new Promise(()=>{
    console.log('hello');
});
console.log('world');
```

4.2 promise也可以代表一个未来的值

4.3 代表一个用于不会返回的值

4.4 应用状态实现抛硬币
```
function flip_coin() {
    return new Promise((resolve,reject)=>{
        setTimeout(function () {
            var random =  Math.random();
            if(random > 0.5){
                resolve('正');
            }else{
                resolve('反');
            }
        },2000)
    })
}
flip_coin().then(data=>{
    console.log(data);
},data=>{
    console.log(data);
});
```
5.实现简单的Promise
```
function Promise(fn) {
    fn((data)=>{
        this.resolve(data)
    },(data)=>{
        this.reject(data);
    })
}
Promise.prototype.resolve = function (data) {
    this._success(data)
};
Promise.prototype.reject = function (data) {
    this._error(data);
};
Promise.prototype.then = function (success,error) {
    this._success = success;
    this._error = error;
};
```
6.Error会导致触发Reject
可以采用then的第二个参数捕获失败，也可以通过catch函数进行捕获
```
function flip_coin() {
    return new Promise((resolve,reject)=>{
        throw Error('没有硬币')
    })
}
flip_coin().then(data=>{
    console.log(data);
}).catch((e)=>{
    console.log(e);
})
```
7.Promise.all实现并行
> 接受一个数组，数组内都是Promise实例，返回一个Promise实例，这个Promise实例的状态转移取决于参数的Promise实例的状态变化。当参数中所有的实例都处于resolve状态时，返回的Promise实例会变为resolve状态。如果参数中任意一个实例处于reject状态，返回的Promise实例变为reject状态
```
const fs = require('fs');
let p1 =  new Promise((resolve,reject)=>{
    fs.readFile('./name.txt','utf8',function (err,data) {
        resolve(data);
    });
})
let p2 = new Promise((resolve,reject)=>{
    fs.readFile('./age.txt','utf8',function (err,data) {
        resolve(data);
    });
})
Promise.all([p1,p2]).then(([res1,res2])=>{
    console.log(res1);
})
```

>不管两个promise谁先完成，Promise.all 方法会按照数组里面的顺序将结果返回

8.Promise.race实现选择
接受一个数组，数组内都是Promise实例,返回一个Promise实例，这个Promise实例的状态转移取决于参数的Promise实例的状态变化。当参数中任何一个实例处于resolve状态时，返回的Promise实例会变为resolve状态。如果参数中任意一个实例处于reject状态，返回的Promise实例变为reject状态。
```
var p = Promise.race([p1, p2]);
p.then(([res1, res2])=>{
    console.log(res1);
});
```
9.Promise.resolve
返回一个Promise实例，这个实例处于resolve状态。
```
Promise.resolve('成功').then(data=>{
    console.log(data);
})
```
10.Promise.reject
返回一个Promise实例，这个实例处于reject状态
```
Promise.reject('失败').then(data=>{
    console.log(data);
},re=>{
    console.log(re);
})
```
11.封装ajax
```
function ajax({url=new Error('url必须提供'),method='GET',async=true,dataType='json'}){
  return new Promise(function(resolve,reject){
     var xhr = new XMLHttpRequest();
     xhr.open(method,url,async);
     xhr.responseType = dataType;
     xhr.onreadystatechange = function(){
         if(xhr.readyState == 4){
             if(/^2\d{2}/.test(xhr.status)){
                resolve(xhr.response);
             }else{
                 reject(xhr.response);
             }
         }
     }
      xhr.send();
  });
}
```
12.chain中返回结果
```
//chain then后返回的结果可以被下一个then使用 then里面也可以返回一个Promise实例
Promise.resolve([1,2,3])
.then(arr=>{
    return [...arr,4]
}).then(arr=>{
    return [...arr,5]
}).then(arr=>{
    console.log(arr);
})
//then里面返回一个Promise实例
Promise.resolve('user').then(data=>{
    return new Promise(function (resolve,reject) {
        fetch('/'+data).then(res=>res.json().then((json)=>{
            resolve(json)
        }))
    })
}).then(data=>{
    console.log(data);
});
```
13.async/await
本质是语法糖，await与async要连用，await后只能跟promise
```
async function getHello() {
    return new Promise((resolve,reject) => {
        setTimeout(function () {
            resolve('hello');
        },2000);
    })
}
async function getData () {
   var result = await getHello();
   console.log(result);
} ;
getData();
```
