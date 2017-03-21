## 框架 angular
我们写好的代码，框架帮我们调用，强约束按照人家的规范来写代码。
## 库 react  vue  jquery
  我们调用库中的方法，我们是主动

###angular MVC　MVVM --简化代码提高开发效率
 - MVC  
  + model(数据)
  + view(视图)
  + controller (控制器)
  > 单向用户改变视图（表单元素），更改后会触发控制器，获取数据后，再刷新视图
  
- MVVM
  + model(数据)
  + view(视图)
  + viewModel (视图模型）
   > 页面变化，会触发数据的变化，数据的变化，会更改页面的变化
##  安装  angular
### 全局安装（在I命令行使用的）

```
npm install babel -g
npm  install bower -g
```
###本地安装（在代码里使用）

```
npm install Jquery
npm install angular
```
npm 是基于NOde，安装node 后会直接安装npm,安装模块到node-modules文件件下，如果当前没有会向上一级查找

### 安装前生成package.json文件

```
npm init -y

npm inof 查看版本
```
> 默认安装  angular 1.0 市面多数1.5.8

 组件化开发
 ---------------------------------------------------------------


C1、安装需要使用的第三方模块

**文件夹路径不能有中文**

bower：从gitHub上下载资源文件，一般应用于客户端想要调取某一个类库或者框架等
  安装bower到全局：npm install bower -g

  bower init =>在本地项目中生成一个bower.json文件

  bower install xxx --save-dev
bower install less --save-dev
            --save 保存到发布产品时候的依赖清单中(产品上线需要)
            --save-dev 保存到开发产品时候的依赖清单中(上线可能不需要，但是开发中需要)

使用bower下载安装，需要保证在DOS命令中集成了git命令，因为bower是基于git下载的，如何保证git命令能够集成到DOS中，我们需要配置环境变量：


########每个项目开始的第一件事情cmp init -y
  1、安装到全局(global)

    npm install xxx(模块名字) -g

    npm uninstall xxx -g 卸载已经安装在全局的模块

npm install less -y  本文件夹





  2、安装在当前的项目下

    npm install xxx(模块名字)

    npm uninstall xxx
3、
npm install xxx --save-dev 放在开发依赖模块清单中

     npm install xxx --save 放在生产依赖的模块清单中
我们平时在做项目开发的时候，首先会在当前项目的根目录下执行：

     npm init -y 

   这样执行完成后，会在当前项目的根目录下生成一个文件：package.json

     下一个B项目如果还是需要这些模块，我们就没有必要一个个的安装了，只需要把package.json文件拷贝到B项目中，然后执行 npm install   即可，这样的系统就会默认按照我们的开发依赖清单一个个的下载 =>"跑环境"

  4 把模块安装在当前的项目中也有问题：

   - 不能像安装在全局一样使用命令了，但是可以使用JS导入了

     解决：在package.json 文件的scripts这个属性中进行配置

      "scripts": {//->配置NPM脚本命令

         "myFirst": "lessc ceshi.less ceshi.min.css -x"

       }

      以后再DOS中只需要执行npm run myFirst，就可以执行lessc -v这个命令了


---------------


npm init -y   =>package.json

npm install xxx --save-dev (--save) 安装到本地项目中
npm install less --save-dev (--save)
如果需要使用命令的话，我们还需要配置“命令”操作(package.json)

npm install less -y



npm install bootstrap
npm install angular
npm install angular --save-dev
npm install bootstrap --save-dev



 npm install  nrm -g    //安装到本地
npm root -g     ///查看安装位置
nrm ls        //nrm ls 查看资源     
nrm test    //测试
## npm官方源
切换到国内源下载
- nrm切换源
```
npm install nrm -g
```
- 查看所有源
```
nrm ls
```
- 测试源的速度
```
nrm test
```
- 添加珠峰源
```
nrm add zhufeng http://172.18.1.139
```
- 删除源
```
nrm del zhufeng
```
- 查看全局安装目录 
```
npm root -g
```

---------------------------------------------------------
LESS / SASS ...

=>CSS预编译语言，CSS不是编程语言，它只是标记语言，在CSS中没有变量、函数、判断、继承、作用域等等这些概念；LESS/SASS等就是按照新的语法(面向对象编程的语法)去写样式，但是写好的样式代码，浏览器不能识别，还需要把他们编译成正常的CSS代码才可以

=>lessc css/index.less css/index.min.css -x 这个命令就是把某一个LESS代码编译为正常能识别的CSS代码，并且进行压缩
  <head>
    <link rel='stylesheet' href='css/index.min.css'/>
  </head>

  [产品上线]


=>在开发的时候，我们可以引入一个less.min.js文件，这样我们开发的时候导入less文件，这个js可以帮我们随时编译成CSS
  <head>
    <link rel='stylesheet/less' href='css/index.less'/>
    <script src='js/less.min.js'></script><!--这个JS首先找到所有导入的LESS文件，然后进行编译，把他们编译成CSS，然后让浏览器渲染的是CSS-->
  </head>

  [产品开发]



less.模块命令编辑
可以设置变量方便以后调整
&是并列关系
.父级（继承父级所有样式）


------------------------]
###腾讯体育视频直播/点播官网首页

####真实的工作流程
1、当产品和UI确定好需求和设计稿之后,我们做项目之前需要召开“需求碰头会”：开发者们(前端开发、后台开发、DB、测试等)了解产品具体的需求和功能，定下开发计划(技术方案、时间方案、风险预估等)

需要详细研讨
  UI -> 样式上的一些细节问题和具体详细的功能探讨，确保我们了解了真正的需求，如果有漏掉的，后期开发中在去问，保持随时沟通
  “先听一遍，在重述一遍”->需求沟通模式

前端开发工程师
  技术方案：HTML采用HTML5处理、CSS/CSS3采用LESS处理、JS采用传统类库JQ、JS代码按照ES6标准来写(把ES6转换为ES5我们需要使用的是BABEL)、数据采用JSONP技术从服务器端获取...

  时间方案：
     A ->xxx ->需要在哪天完成
     B ->xxx ->需要在哪天完成
     ...
     各位开发人员回去后需要定制自己开发任务的日计划和周计划

  风险预估

2、和后台开发人员沟通配合 ->数据
3、和测试人员沟通配合 ->功能是否实现是否存在问题

------------------
[babel]
npm install babel-cli babel-preset-es2015 babel-preset-es2016 --save-dev

在当前项目的根目录下建立一个文件 .babelrc (没有文件名只有后缀名)
```
{
  "presets": [
    "es2015",
    "es2016"
  ],
  "plugins": []
}
```

-x 压缩
-o 编译某一个文件
-d 编译一个文件中的所有文件
-w 监听
...




- 查看全局安装目录 
```
npm root -g
```

## 安装本地依赖时
--save  --save-dev


## angular
### ng-app
```
html ng-app="appModule"
```
会产生一个根作用域$rootScope,让angular启动,可以指定模块名,告诉angular以当前模块来启动
### 声明模块
一切从模块开始
```
var app = angular.module("appModule",[]);
```
### ng-model
实现双向数据绑定,放在表单元素上
```
ng-model="name"
```
M->V:将name(作用域上定义的内容)代表的值放到输入框内
V->M:如果修改输入框中的内容，可以改变作用域上name的值

> 如果写的格式是ng-model="person.name",并且person不存在会声明一个对象

取值和{{}}作用一致,可以解决闪烁单行问题

- 赋值，三元，运算

### ng-cloak
解决多行闪烁问题
```
<style>
    [ng-cloak]{display:none}
</style>
<div ng-cloak></div>
```

### ng-事件 
事件，在angular可以通过ng-事件来执行作用域上的方法，如果需要事件源，需要手动传递$event对象

### ng-controller
会产生一个作用域，作用范围和DOM平齐，可以嵌套控制器，子可以继承父亲。不要操作DOM
```
<div ng-controller="myCtrl"></div>
app.controller('myCtrl',['$scope',function($scope){}])
```

### run方法
写了ng-app会默认调用run方法,一般定义全局共有的变量
```
app.run(['$rootScope',function($rootScope){}]);
```

### ng-repeat
可以遍历数组或对象,遍历谁，写在谁的身上
```
<div ng-repeat="(key,value) in arrs track by key"></div>
$scope.arrs = [{name:1},{name:2}]
```

### filter数组的方法
"删除"过滤,返回是一个新的数组
push,pop,unshift,shift,slice,splice,concat,forEach,join,map,sort,indexof,reverse,lastIndexOf,every,some
```
var newArr = [].filter(function(item,index){
    return true保留这一项/false这项不要了
},this指向);
```

### map数组的方法
"修改"映射,返回是一个新的数组
```
var newArr = [1,2,3,2,2,2].map(function (item,index) {
    return `<li>${item}</li>`;//返回什么内容，就会将这一项变成什么内容
});
console.log(newArr);
``` 

### find数组的方法
"查找"es6,找到后不会继续查找，返回找到的那一项
```
var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
var obj = arr.find(function (item,index) {
    return item.name == 2;//如果找到返回true即可，会将item返回回去,不会继续执行
});
``` 

###AngularJS ng-model-options 指令
定义和用法
ng-model-options 指令绑定了 HTML 表单元素到 scope 变量中
你可以指定绑定数据触发的时间，或者指定等待多少毫秒，参数设置可以参考以下说明。

```
语法
<element ng-model-options="option"></element>
<input>, <select>, <textarea>, 元素支持该指令。
```

参数值
值
描述
option
指定了绑定数据的规则，规则如下:
**
{updateOn: 'event'}规则指定事件发生后绑定数据
{debounce : 1000} 规定等待多少毫秒后绑定数据
{allowInvalid : true|false} 规定是否需要验证后绑定数据
{getterSetter : true|false} 规定是否作为 getters/setters 绑定到模型
{timezone : '0100'} 规则是否使用时区
**

	
 - 过滤器
 ```
 <body>
 <!--大小写-->
 {{"asdasd" | uppercase}} <br>
 {{"AASDASD"| lowercase}} <br>
 <!--货币过滤器 默认在最前面增加$符号-->
 {{"123123.123"| currency:'￥':3}} <br>
 <!--限制过滤器-->
 {{"你好吗" | limitTo:2}} <br>
 <!--json过滤器-->
 <pre>{{ {name:1,age:4} | json:4 }}</pre>
 <!--number过滤器-->
 {{123123|number}}
 <!--日期过滤器-->
 {{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}
 <!--orderBy排序 filter过滤器 -->
 <script src="node_modules/angular/angular.js"></script>
 <script>
     //内置过滤器 9个  自定义过滤器
     var app = angular.module('appModule',[]);
 </script>
 </body>
 ```
 
 #### 数组方法拓展
 ##### filter数组的方法
 "删除"过滤,返回是一个新的数组
 push,pop,unshift,shift,slice,splice,concat,forEach,join,map,sort,indexof,reverse,lastIndexOf,every,some
 ```
 var newArr = [].filter(function(item,index){
     return true保留这一项/false这项不要了
 },this指向);
 ```
 
 ##### map数组的方法
 "修改"映射,返回是一个新的数组
 ```
 var newArr = [1,2,3,2,2,2].map(function (item,index) {
     return `<li>${item}</li>`;//返回什么内容，就会将这一项变成什么内容
 });
 console.log(newArr);
 ``` 
 
 ##### find数组的方法
 "查找"es6,找到后不会继续查找，返回找到的那一项
 ```
 var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
 var obj = arr.find(function (item,index) {
     return item.name == 2;//如果找到返回true即可，会将item返回回去,不会继续执行
 });
 ``` 




