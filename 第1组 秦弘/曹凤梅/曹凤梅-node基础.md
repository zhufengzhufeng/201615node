##node基础
#### 单线程和多线程
- node主线程是单线程的，进程中包含线程，正常java 一个进程中包含多个线程，node中一个进程只能包含一个线程，但允许开子进程。

####同步和异步
- 代码从上到下执行，先走同步再走异步，异步不会阻塞主线程

#### 阻塞和非阻塞
- 针对内核来说的，非阻塞是异步的前置条件

#### 回调（回头再调）
- 用回调来解决异步编程问题

#### 事件环

####异步的有：文件读写，callback,定时器，PS：能用异步 不用同步

#### node全局对象
- 在任意地点可以直接访问的
- 在global上挂载的都是全局对象
####node中的回调函数
```
function read(callback) {
    setTimeout(function () {
        var data = 'hello world';
        callback(data);
    },2000);
}
//解决异步编程问题
read(function (data) {
    console.log(data);
});
```
####node的global
```
1、在全局下console.log(this); 这是this是{} ；因为每个文件执行前 会套用一个闭包函数，在这个函数中this指向发生了改变；
2、;(function () {
   console.log(this);
})();
// 自执行函数中的this = global
3、var a = 100; //用操作符声明的变量不会挂载在global属性上，也就是global.a是undefined；
但是挂载在global对象上的属性，可以在任何模块中使用，比如：global.a = 100;这是挂载在global下的属性；
4、globa是一个对象；它的常用属性：
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
5、global的console属性：有log/dir/error/warn/info   time/timeEnd
比如：console.time('count');
for(var i = 0; i<1000000000;i++){}
//用来计算客户端访问服务器端所用的时间、代码所用的时间
console.timeEnd('count');*/
PS：time和timeEnd的参数必须相同才能自动计算出时间；
6、global的setTimeout属性：
//第三个参数及以后的参数可以传递参数
function eatrace(who,who1) {
    console.log(who+'吃米饭'+who1);
};
setTimeout(eatrace,2000,'我','你');
node中的定时器中的this指的是setTimeout自己,改变this指向只能使用bind方法
var a = 1;
setTimeout(function () {
    console.log(this);
}.bind(a));
7、global的setImmediate属性：setImmediate（function(){}） = setTimeout(function(){},0);
PS：setImmediate一般不写时间；
小例子：
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
console.log(102);
ps: 默认执行顺序：先同步输出100,101,102，再异步执行nextTick>setImmediate>= setTimeout；
```
###node中的npm
***
npm : node-package-manager
安装nrm源切换工具
```
npm install nrm -g
```
#### 增加珠峰源
```
nrm ls 查看网源
nrm add zf http://172.18.1.139 增加网源
nrm use zf 使用网源
```
#### 安装全局(在命令行下使用) -g
https://github.com/ksky521/nodePPT ：制作PPT的插架的网址介绍；
```
npm install nodeppt -g
```
#### http-server查看一个项目的局域网和外网
```
npm install http-server -g
```
启动服务
```
http-server -p 3000 更改端口
```
#### 卸载
```
npm uninstall http-server -g
```

#### 本地安装(在我们代码里使用的)
 初始化依赖文件(package.json)
在指定目录下生成
```
npm init -y
```
#### 1.开发依赖 gulp

```
npm install gulp (--save-dev)或者(-D)
```

#### 2.项目依赖 jquery

```
npm info jquery  查看版本
npm install jquery@2.2.4 (--save)或者(-S)  安装指定版本；
```

#### 3.发布自己的包
- package.json 
    - name不能和已发布的包重名
    - main里对应的主文件件写一个
- 发布要切换到npm   即：nrm  use  npm
- 添加用户 有的话可以登录
```
npm addUser 添加用户或登录（添加时设置一个用户名和用户密码）
```
- 发布
```
npm publish 
```
- 卸载包
```
npm unpublish  --force
```

#### 模块的查找机制

> 为什么-g安装的可以在命令行下使用,npm可以在命令行下使用，所有的全局包 都安装在npm上，会在npm下创建一个脚本文件，可以映射到真实的文件上，所以通过全局安装的可以直接在命令行下使用

### bower(管理前端文件的) 
npm(管理node模块的) ->安装的文件放到node_modules下 不能指定安装目录
bower(管理前端文件的) ->制定目录下载 只管理前端文件(在git中下载)
```
npm install bower -g
```
#### 1.1初始化 bower.json记录依赖
```
bower init
```
#### 1.2下载文件
```
bower install bootstrap --save
```
#### #1.3 指定目录
```
.bowerrc
{"directory":"src/public/lic"}
```
> 默认安装到bower_component下

####发布hexo
```
npm install hexo-cli -g
```

#### 生成blog
```
hexo init
```

#### 启动服务
```
hexo server
```

### #文章放在_posts下
md格式文件
#### 仓库名
```
github名.github.io
```
####发布到git上需要下载一个插件 
```
npm install hexo-deployer-git --save
```
####配置_config.yml
```
deploy:
  type: git
  repo: https://username:password@github.com/zuyuan/zuyuan.github.io.git
  branch: master
  message: push
```

#### 如果重新提交
#####生成
```
hexo g
```
#### 发布
```
hexo deploy
```

#### 访问网址
```
zuyuan.github.io
```
***
