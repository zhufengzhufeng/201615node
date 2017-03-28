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