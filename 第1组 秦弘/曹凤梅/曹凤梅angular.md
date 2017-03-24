##angular
#### 框架 angular bootstrap
我们写好代码，框架帮我们调用,强约束按照人家的规范来写代码。

#### 库 react vue jquery
我们调用库中的方法，我们是主动的
#### 单向绑定数据
用户改变视图(表单元素)，更改后会触发控制器，获取数据后，在刷新视图
#### 双向数据绑定（表单元素）
- model
- view
- viewModel (视图模型)
####angular基础知识
```
>1、angular开始执行,在想要执行范围的标签上增加ng-app，增加ng-app后会生成一个根作用域$rootScope
>2、修改视图影响数据 ng-model表示双向数据绑定，而且只能加在表单元素上(《div contenteditable="true">1111《/div>可编辑div也不能增加ng-model) , ng-model可以绑定一个变量名，而且只能绑定变量；
{{name?name:'hello zfpx'}}
{{}}闪烁问题 ng-bind等价于{{}}，{{}}是ng-bind的简写，解决某个变量闪烁问题
>3、数据变化，视图也要发生改变 {{}}可以将作用域上的变量取出来；
>4、{{}}有闪烁问题 ，ng-bind等价于{{}}，{{}}是ng-bind的简写，解决某个变量闪烁问题；ng-cloak是解决成段代码的闪烁问题 ，原理是先让带有ng-cloak的标签隐藏，当数据编译好后会自动移除掉ng-cloak属性；
>5、模块化：
       - ng-app="模块名"
       - var app = angular.module('模块名',[]); //声明模块,app代表的是当前模块（参数代表1.模块的名字 2.模块的依赖（默认写[]） 3.配置函数）
       - app.controller("控制器名字",["$scope",function($scope){
        $scope.name="zfpx";
        $scope.age = 9;
    }]);
    PS：1）为了防止压缩 采用数组的形式 ，一一对应
    2）要在视图上取出数据 必须将数据挂载$scope上
    3）参数代表：1.控制器的名字 2.控制器的一个定义,默认有一个参数$scope = viewModel
>6、如果想获取事件源  需要出传递$event
>7、ng-repeat="变量(数组中的value值)  in  数据  track by $index" （只要是数组 就通过索引来遍历）
>8、(key,value) in arrs 这个key（属性名）是我们显示声明出来的 $index是angular提供给我们的；
>9、bootstrap.css：
   默认颜色 default 灰色
    danger 红色
    warning 黄色
    primary 蓝色
    info 浅蓝色
    success 绿色
>10、app.run(['$rootScope',function ($rootScope) {
        $rootScope.eat = '吃'
    }]);//angular启动时会调用run方法，写了ng-app就开始执行，在全局作用域下添加属性； 
      
```
## npm官方源
切换到国内源下载
- 安装nrm切换源
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

### ng-bind
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



## 更新老师代码
第一步
```
git clone 地址 文件夹的名字
```
第二步
```
cd 文件夹的名字
```
第三步，拉取代码
```
git pull 如果更改先提交你的更改 
git add .
git commit -m ''如果报错
git config --global user.name 'youname' 
git config --global user.email 'youremail'
```
写esc :wq  不修改保存；
####angular内置过滤器（9个）
```
1、<!--大小写-->
{{"asdasd" | uppercase}} 
{{"AASDASD"| lowercase}} 
2、<!--货币过滤器 默认在最前面增加$符号-->
{{"123123.123"| currency:'￥':3}} 
3、<!--限制过滤器-->
{{"你好吗" | limitTo:2}} //限制两个字
4、<!--json过滤器-->
<pre>{{ {name:1,age:4} | json:4 }}</pre>/
5、<!--number过滤器-->
{{123123|number}}
6、<!--日期过滤器-->
{{1489568089171 | date:"yyyy年MM月dd日  hh:mm:ss"}}
7、<!--orderBy排序 
1）<!--延迟刷新数据 数据确定下来 过两秒刷新  ng-model-options="{debounce:500}"-->
2）<!--失去焦点刷新数据 -->
<input type="text" ng-model="query" ng-model-options="{updateOn:'blur'}">
3）<!--根据的是当前遍历对象上拥有的属性 第三个参数代表的是 是否降序 true代表的是降序-->
tr ng-repeat="stu in students | orderBy:language:flag=true |filter:{english:query} track by $index">
        <td>{{stu.name}}</td>
        <td>{{stu.english}}</td>
        <td>{{stu.chinese}}</td>
        <td>{{stu.math}}</td>
8、filter过滤器 -->
filter可以指定查询{字段:查询条件} ，默认是全部-->
```
####自定义过滤器
```
<body>
{{"hello" | capitalized:1}}
<body>
var app = angular.module('appModule',[]);
    app.filter('capitalized',function () {
       return function (input) {
                //1.param1代表的是要过滤的数据
           //取出除了第一个的所有的参数的集合[](面试题)
           //方法1、console.log([].slice.call(arguments,1));
           //方法2、类数组转换成数组console.log(Array.from(arguments).slice(1));
           //方法3、扩张运算符console.log(o);//...o 
 return ;//最终显示到页面上的数据
 }
    });
```
###面试题：类的继承有哪些？
```
function Parent() {
    this.smoke = '抽烟';
}
Parent.prototype.eat = function () {
    console.log('吃');
};
function Child() {
    //Parent.call(this); // child.smoke = '抽烟'
}
var child = new Child();
模拟Object.create,了解原理：
function create(pro) { //Object.create的原理
    var Fn = function () {};//这个函数是一个空函数
    Fn.prototype = pro;//函数的公有方法指向父亲的共有方法
    return new Fn();//实例只有共有方法
}
Child.prototype = create(Parent.prototype);
Object.setPrototypeOf(Child.prototype,Parent.prototype);
1.会继承私有属性和公有属性
Child.prototype = new Parent();
2.call继承 只继承私有（父方法在子方法中执行时call）
Parent.call(this);（只有子的实例可以调用父的私有方法，不能用子方法调用）
3.只继承父亲公有的方法
Child.prototype.__proto__ = Parent.prototype;
Child.prototype = Object.create(Parent.prototype); es5
Object.setPrototypeOf(Child.prototype,Parent.prototype); es6
extends es6
```
