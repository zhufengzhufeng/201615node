在之前做web时也经常用到js对象转json和json转js对象.既然是Node.js处理I/O数据，也把这个记下来。

Json转Js对象:JSON.parse(jsonstr); //可以将json字符串转换成json对象
Js对象转Json:JSON.stringify(jsonobj); //可以将json对象转换成json对符串

复制代码
var jsonStr='{"name":"cuiyanwei","sex":"男","blog":"http://www.cnblogs.com/5ishare/"}';
//json转js对象
var jsObj=JSON.parse(jsonStr);
console.log(jsObj);
//js对象转Json
var newJsonStr=JSON.stringify(jsObj);
console.log(newJsonStr);
复制代码
复制代码
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe Buffer.js
{ name: 'cuiyanwei',
  sex: '男',
  blog: 'http://www.cnblogs.com/5ishare/' }
{"name":"cuiyanwei","sex":"男","blog":"http://www.cnblogs.com/5ishare/"}

Process finished with exit code 0
复制代码
二、Buffer模块缓冲数据

1.介绍

缓冲数据是由一系列大端或小端(这个要百度)格式字节组成,比文本数据占用较小空间.Buffer模块是全局的,不需要使用require()来访问它。

缓冲数据被存储在类似数组的结构中,但被存储在正常V8堆内存之外的原始内存分配区中,因此不能调整大小.

2.使用

在介绍中说缓冲数据被存储在类似数组的结构中,基本和数组的操作差不多:创建对象、读、写、分割、拷贝、合并等。

1).创建

Buffer对象是在原始的内存分配区，在创建的时候确定大小。

    //    1.字节大小 2.八位字节缓冲区 3.UTF8字符串
var  buffer256=new  Buffer(256);
var  bufferOctets=new Buffer([0x6f,0x63,0x74,0x65,0x74,0x73]);
var bufferUTF8=new  Buffer("Some UTF8 Text",'utf8');
2).写(方法好多)

复制代码
var buf256=new Buffer(25);
//fill(value,[offset],[end]) 将value写到缓冲区从offset索引(默认0)处开始,并在end索引处结束的每个字节(默认buffer长度)
buf256.fill("1",0);
console.log(buf256.toString());
//wirte(string,[offset],[length],[encoding]) 使用encoding编码从缓冲区offset索引开始写入string中length数量的字节
buf256.write("abc",1,2,'utf8');
console.log(buf256.toString());
buf256[6]=56;//将索引处的数据替换为指定的value值
console.log(buf256.toString());
复制代码
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe Buffer.js
1111111111111111111111111
1ab1111111111111111111111
1ab1118111111111111111111

Process finished with exit code 0
3).读(可逆，有写就有读)

复制代码
var bufUTF8=new  Buffer("Some UTF8 Text",'utf8');
console.log(bufUTF8.toString());
//toString([encoding],[start],[end]) 使用encoding编码,读取从start(0)到end(尾)
console.log(bufUTF8.toString('utf8',0,4));
//返回缓冲区在指定offset字节的八进制值
console.log(bufUTF8[8].toString(16));
//Node.js提供StringDecoder对象,有一个write(buffer)来进行解码,并使用指定的编码写入缓冲区数据
var StringDecoder=require('string_decoder').StringDecoder;
var decoder=new StringDecoder('utf8');
console.log(decoder.write(bufUTF8));
复制代码
复制代码
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe Buffer.js
Some UTF8 Text
Some
38
Some UTF8 Text

Process finished with exit code 0
复制代码
4.Buffer长度

Buffer的长度是计算的字节长度,字符串的长度是字符的长度.

var s='UTF8 text \u00b6';
console.log(s.length);
console.log(Buffer.byteLength(s,'utf8'));
console.log(Buffer(s).length)
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe Buffer.js
11
12
12

Process finished with exit code 0
5.复制缓冲区 主要是copy方法

var bufSource=new  Buffer("1234567890",'utf8');
var bufTarget=new  Buffer("abcedfghijklmn",'utf8');
//copy.(targetBuffer, targetStart, sourceStart, sourceEnd)
//bufSource.copy(bufTarget,10,3,9); //abcedfghij4567
bufSource.copy(bufTarget,0,3,9);//456789ghijklmn
console.log(bufTarget.toString());
6.对缓冲区切片

这个有点类似字符串截取.slice([start],[end])，返回一个Buffer对象.如果编辑一个切片也会改变原来的缓冲区。

复制代码
var  number=new Buffer('123456789');
console.log(number.toString());
var slice=number.slice(3.9);
console.log(slice.toString());
slice[0]='#'.charCodeAt(0);
slice[slice.length-1]='#'.charCodeAt(0);
console.log(slice.toString());
console.log(number.toString());
复制代码
复制代码
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe Buffer.js
123456789
456789
#5678#
123#5678#

Process finished with exit code 0
复制代码
7.拼接缓冲区

这个有点类似C中的字符串拼接.concat(list,[totalLength])可以把多个Buffer对象拼接在一起形成一个新的缓冲区.

var buff1 = new Buffer("123");
var buff2 = new Buffer("abc");
console.log(Buffer.concat([buff1, buff2,buff1]).toString());
"C:\Program Files (x86)\JetBrains\WebStorm 11.0.3\bin\runnerw.exe" F:\nodejs\node.exe buffer_concat.js
123abc123

Process finished with exit code 0