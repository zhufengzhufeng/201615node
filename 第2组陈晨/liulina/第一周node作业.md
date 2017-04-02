#### git介绍
#### git基本介绍
Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git。
>- git: 分布式版本控制 (每个人的电脑是一个完整的版本库,本地操作时不需要联网)
- svn: 集中式版本控制(必须联网才能工作,有一台中央服务器)


#### 创建版本库
```
cd directory
git init //初始化git
git add <file>/.
git commit -m(提交说明)
git status //掌握工作区的状态
git diff //可以查看修改内容 工作区和暂存区
git diff --cached/--staged 暂存区和版本库
git diff HEAD 工作区和版本库
git diff topic master/git diff topic..master //将两个分支上的最新提交做diff
git diff HEAD^ HEAD //比较上次提交commit和上上次提交
```

#### 版本回退
```
git log --pretty=oneline//提交日志 (回退到哪个版本)
HEAD//用来表示当前版本
git reset --hard HEAD^(HEAD~100)
git reflog //查看命令历史,确定要回到未来哪个版本
```

#### 工作区和暂存区
> 工作区(working directory) .git 的相对目录下。
版本库(repository): .git 目录下(版本库)。
暂存区(stage).

#### 管理修改
```
git checkout --file //丢掉工作区的修改
git reset HEAD file //添加到了暂存区,想丢弃修改
//提交到了版本库。可以利用版本回退
git rm //用于删除一个文件 可以恢复 git checkout 使用版本库的版本替换工作区的版本。(无论修改还是删除都可以复原)
```

#### 远程仓库
```
//git仓库与github之间利用ssh的方式连接
//第一步
ssh-keygen -t rsa -C "" //一路回车 在用户主目录的.ssh里面有id_rsa 与id_rsa.pub (秘钥对) 公钥可以告诉别人
//第二步
登录github, “Account settings”->"SSH Keys"->"Add SSH Key"->填上任意Title，粘贴id_rsa.pub文件的内容->点“Add Key”
//之后可以进行传输
```

```
//在github创建仓库
//添加远程库
git remote add origin git@github.com:michaelliao/learngit.git
//由于是空的,第一次推送加-u ,下一次就不用了
git push origin master
//克隆远程库
git clone 仓库
git 支持多种协议
//默认的git://使用ssh 也可以使用https 但每次提交需要输入用户名和密码。
```

#### 分支管理
>查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

>当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用git log --graph命令可以看到分支合并图

```
git branch -d feature//删除分支
git branch -D feature //强制删除没有被合并的分支
git remote //查看远程库信息
git remote -v //显示更详细信息
git remote add origin 地址
git remote rm origin 删除远程关联
//推送分支
git push origin master
git push origin dev
git checkout -b dev origin/dev //创建远程origin的dev分支到本地
git branch --set-upstream branch-name origin/branch-name//建立本地分支和远程分支的关联
//从远程抓取分支，使用git pull，如果有冲突，要先处理冲突
```

#### 发布静态页
发布的静态页必须在gh-pages分支
```
git checkout -b gh-pages
git add .
git commit -m 
git push origin gh-pages
```
#### 使用github
- fork 放到自己的仓库
- clone  放到自己的本地文件夹
- 推送到github上
- 发送pull request请求合并

#### 配置git
```
git config --global user.name xxxxx
git config --global user.email xxxxx
git config --list
git config --global color.ui true //让git现实颜色
```

#### 忽略特殊文件
在工作区根目录下创建.gitignore, 填写忽略文件(可以是目录, 可以用*号匹配).

#### hexo简单使用介绍（用于搭建博客）
```
npm install hexo-cli -g //发布hexo
hexo init //生成blog
hexo server/s //启动服务
//文字放在_posts下
//github上对应的仓库名必须为
github名.github.io
//发布到git上需要下载一个插件
npm install hexo-deployer-git --save
```

#### 配置_config.yml
```
deploy:
  type: git
  repo: https://username:password@github.com/zuyuan/zuyuan.github.io.git
  branch: master
  message: push
```

#### 使用
```
hexo new "" //生成新博文
hexo g //生成静态页
hexo deploy //发布到github上
github名.github.io //网址
hexo s//本地测试地址
```

#### node初识
#### 一些概念
单线程和多线程
> node主线程是单线程的，进程中包含线程, 一个进程中包含多个线程，node中一个进程只能包含一个线程，允许开子进程。

同步和异步
> 代码从上到下执行, 先走同步再走异步, 异步不会阻塞主线程

阻塞和非阻塞
> 针对内核来说的, 非阻塞是异步的前置条件

回调 (回头再调)
> 用回调来解决异步编程问题

事件环

异步的文件读写, callback, 定时器, 能用异步 不用同步

node全局对象
> 在任意地点可以直接访问的,在global上挂载的都是全局对象

#### node一些细节介绍
> node的每一个js文件, 在文件执行前, 会套用一个闭包函数, 在这个函数中this指向发生了改变
```
(function () {
   console.log(this);//自执行函数中的this = global
})();
```
```
//用var声明的不会挂载在global属性上
var a = 1;
console.log(global.a);//undefined
```
>console.time 与 console.timeEnd();
```
//用来计算客户端访问服务器端所用的时间、代码所用的时间
console.time('count');
for(var i = 0; i<1000000000;i++){}
console.timeEnd('count');
```

> setTimeout(fn, interval, param1, param2), 后面参数为fn的参数

```
//this指的是setTimeout自己,改变this指向使用bind方法
var a = 1;
setTimeout(function () {
    console.log(this);//this === Timeout
}.bind(a));
```

> setImmediate = setTimeout(function () {}, 0);
默认process.nextTick>setImmediate>=setTimeout

```
(function(exports,require,module,__dirname,__filename){
   //这五个参数是形参，可以在任何文件中使用，我们也叫他全局变量
})()
```

>process有一些方法和属性
```
//process.pid 当前的进程id
//process.kill()杀死某个进程
//process.exit()退出自己
//process.cwd() current working directory 可以更改
//process.chdir() 改变目录
```

#### js中的模块
>- (seajs cmd,requirejs amd,node commonjs)
 - cmd 就近依赖,amd 依赖前置
 - 单例(不能保证完全解决冲突,调用时调用名字过程)
 - 闭包，node中实现模块化 采用的是读写

> commonjs(提高了可维护性，有利于分工协作，高内聚低耦合)
- 如何定义模块
创建一个js文件, 每个文件就是一个模块, 多个模块可以组成一个包
- 如何导出一个模块
exports/module.exports
- 如何使用一个模块
require

> 使用某个模块中的变量, 可以挂载在全局下, 让所有模块来使用

```
//一个模块内部执行原理
(function(exports,require,module,__filename,__dirname){
      module.exports = exports = {}
      return module.exports
})
```
```
var a = 100;
exports = a; //改变了exports的指向 不会导致module.exports的变化，另一个文件require的返回值是module.exports
exports.a = a;//改变了module.exports中的属性

//可以得出二者一致
exports.a = {};
module.exports.a = {};

require() 看其是否同步, 看有没有回调函数, 看是否有返回值.
```

#### npm 相关
```
//安装http-server 模块 用于启动一个服务
npm install http-server -g
//nodeppt 模块
```
> 发布自己的包
- package.json
    - name不能和已发布的包重名
    - main里对应的主文件写一个
- 发布要切换到npm
- 添加用户 有的话可以登录

```
npm addUser
npm publish //发布
npm unpublish --force
```
> 直接使用require可以将第三方模块进行引用
```
//先找自己的node_module,没有向上找，找到根目录结束,没有报错
console.log(module.paths);
//在node_modules中查找文件，并且名字是lina-very-handsome,再找package.json文件，找main对应的名字 进行执行
```

#### 模块的查找机制

> 为什么-g安装的可以在命令行下使用,npm可以在命令行下使用，所有的全局包 都安装在npm上，会在npm下创建一个脚本文件，可以映射到真实的文件上，所以通过全局安装的可以直接在命令行下使用


#### bower(管理前端文件的)
> npm(管理node模块的) ->安装的文件放到node_modules下 不能指定安装目录
  bower(管理前端文件的) ->指定目录下载 只管理前端文件(在git中下载)

```
npm install bower -g
bower init
bower install bootstrap --save
//指定目录(默认安装到bower_component下)
.bowerrc
{"directory":"src/public/lic"}
```