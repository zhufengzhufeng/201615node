##11周第3 和 4天

```
// 1.Buffer是global上的属性 new/Buffer.  全局对象
//setTimeout setInterval process setImmediate console Buffer 形参(exports require module __dirname __filename)
//console.log(global);

//1.二进制 逢二进一 16进制 逢16进1  Buffer展现给我们的是16进制
//2.utf8一个汉字 代表3个字节
//3.一个字节由八个位组成Bit

//
```

```
//转换公式 当前位的值* 进制^(当前所在位-1) 累加
var sum = 0;
for(var i = 1; i<=8;i++){
    // sum+= 1* 2^(i-1);
    sum+= 1*Math.pow(2,i-1)
}
console.log(sum); //255 最大255 -> 转换成16进制是多少 ff
//15*16^1+15*16^0
```

### 
/1.创建buffer 三种方式(固定大小) 非常像数组 会将buffer转换成字符串
//1） 长度创建 字符串创建 数组创建


```
var buffer =  new Buffer(106); //6个字节
buffer.fill(0); //对256取模 0-255是256个数
//给的10进制 0x开头默认是16进制
```
```
 //targetBuffer,目标buffer targetStart,目标的开始 sourceStart,源的开始 sourceEnd源的结束
```

```
//1.myConcat list totalLength
//2.判断长度是否传递，如果给了长度就构建一个buffer,将小buffer依次拷贝到大buffer上，过长则将多余的部分 截取掉slice()截取有效长度
//3.手动维护长度 在构建buffer，将小buffer依次拷贝到大buffer上 copy
```

```
/ base64 不是一个加密算法， 加密 md5 sha1 sha256

//将汉字转换成base64 '珠' => 54+g  3*8 = 6*4
//2进制装换成10进制不得大于64,得到的结果在可见编码中取值
```


```
const fs = require('fs');
//1.如果写的文件不存在会创建文件
//2.默认写入格式是utf8格式
//3.文件有内容，清空文件内容
//fs.writeFileSync('./a.txt',new Buffer('珠峰'));
//如果放入的是buffer格式 会默认toString('utf8');

```
```

//拷贝的方法  同步 readFileSync+writeFileSync
//          异步 readFile + writeFile
```

```
// copySync('./name.txt','./name1.txt');
// readFileSync readFile
// writeFile  writeFileSync
// appendFile appendFileSync
// path.resolve() path.join() 给一个相对路径 解析一个绝对路径
// mkdirSync mkdir 创建目录
// rmdirSync rmdir 删除目录
// unlinkSync unlink 删除文件
// readdirSync readdir 读取目录
// existsSync exists 是否存在
// statSync  stat 判断文件状态


```


```
const fs = require('fs');
//1.读取保证文件必须存在
//2.默认返回的是可读流对象
//3.highWaterMark: 65536, 64k 一次读多少
//4.encoding 是null 默认读出的内容是buffer

```



```

const fs = require('fs');
//1.写的默认编码 utf8
//2.文件不存在会创建文件，会清空原有内容
//3.ws代表的是可写流
//4.highWaterMark  16384 16*1024
var ws = fs.createWriteStream('./2.txt',{highWaterMark:4});
//写入 write() 写完 end()
var flag = ws.write('abcd1',function () { //buffer or string
    console.log('成功');
});
//flag表示是否可以继续写入
ws.end('在吃一口');// 表示合上嘴，强制将没有写完的内容写完，在关闭
//end可以传递参数，表示在合上嘴之前 在写点东西
console.log(flag);

```


```
//drain 可写流的方法 抽干，可写流空了
```

```
//1.10G 先读一次 64k ，开始写入 16k，如果写不进去了,暂停读取，等待写完后在读取，读取成功后，关闭写入
```

```
//1.先监听读取事件rs.on('data');
    rs.on('data',function (data) {
        //2.先写入ws.write，如果返回值为false，rs.pause();
        var flag = ws.write(data);
        if(!flag){
            rs.pause();
        }
    });
    //3.ws.on('drain')可写流干了,在进行读取rs.resume();
    ws.on('drain',function () {
       rs.resume();
    });
    //4.rs.on('end') 读完了 ws.end()关闭写的文件
    rs.on('end',function () {
        ws.end();
    });
}
pipe('1.txt','3.txt');

```


###promise

```
//Promise本意是承诺，在程序中的意思就是承诺我过一段时间后会给你一个结果。 什么时候会用到过一段时间？答案是异步操作，异步是指可能比较长时间才有结果的才做，例如网络请求、读取本地文件等
//3.Promise的三种状态
//例如媳妇说想买个包，这时候他就要"等待"我的回复，我可以过两天买，如果买了表示"成功"，如果我最后拒绝表示"失败"，当然我也有可能一直拖一辈子
//Pending Promise对象实例创建时候的初始状态
//Fulfilled 可以理解为成功的状态
//Rejected 可以理解为失败的状态
//then 方法就是用来指定Promise 对象的状态改变时确定执行的操作，resolve 时执行第一个//函数（onFulfilled），reject 时执行第二个函数（onRejected）
```