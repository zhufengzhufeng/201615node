###Angular
#### npm官方源
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

#### 安装本地依赖时
--save  --save-dev


### angular
#### ng-app
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



### 更新老师代码
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

写esc :wq
```
###Angular
- 1.filter
````
{{"asdasd" | uppercase}} <br>
{{"AASDASD"| lowercase}} <br>
<!--货币过滤器 默认在最前面增加$符号-->
{{"123123.123"| currency:'￥':2}} <br>
<!--限制过滤器-->
{{"你好吗" | limitTo:2}} <br>
<!--json过滤器-->
<pre>{{ {name:1,age:4} | json:4}}</pre>
<!--number过滤器-->
{{123123|number}}
<!--日期过滤器-->
{{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}
````
- 2.input中指的数据实时刷新
    + 延迟刷新数据 数据确定下来过两秒刷新：ng-model-option="{debounce:2000}"
    + 失去焦点刷新数据：ng-model-options="{updateOn:'blur'}"
    + ng-bind-html 这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html)
- 3.angular指令的四种格式:
    + restrict 值可以是以下几种:
    ````
      E作为元素名使用<my-drag></my-drag>
      A作为属性使用...<div my-drag></div>
      C作为类名使用<div class="my-drag"></div>
      M作为注释使用前后必须用空格，不建议使用<!-- directive:myDrag -->
      restrict 默认值为 EA, 即可以通过元素名和属性名来调用指令。
    ````
- 4.指令分为两类 自带指令  自定义指令
    + 自定义指令
     装饰型指令 赋予标签一些功能  link函数
     组件式 把标签 替换成一个组件 template
     ````
    默认写法
     app.directive('myDrag',function () {
             return {
                 replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
                 restrict:'EACM', //限制使用范围 默认范围是EA,范围的标识必须大写
                 template:`<div>
                             <div>Hello</div>
                             <div>zfpx</div>
                           </div>` //模板的根节点只能有一个
             }
         });
         
         链接函数 链接视图和作用域的
                     link:function ($scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
                         // 1.代表的是当前指令所在的作用域
                         // 2.element代表当前指令所在的标签，是一个jquery对象
                     
                     }
     ...................................                     
     app.directive('panel',function () {
             return {
                 restrict:'E',
                 templateUrl:`tmpl/panel.html`,//ajax获取过来的
                 link:function (scope,element,attrs) {
                     //自己家没有 根作用域，如果自己家有 就是自己家的作用域
                     scope.name = attrs.st;
                     scope.title = attrs.title;
                 },
                 scope:{}, //给当前指令产生一个作用域 {}  默认值是false
                 // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
                 transclude:true,//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
             };
             
     ````
     + 还有指令配置项的：link controller等在项目运用：
     ````
       angular.module('myApp', []).directive('first', [ function(){
           return {
               // scope: false, // 默认值，共享父级作用域
               // controller: function($scope, $element, $attrs, $transclude) {},
               restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
               template: 'first name:{{name}}',
           };
       }])
       .............
       .directive('second', [ function(){
           return {
               scope: true, // 继承父级作用域并创建指令自己的作用域
               // controller: function($scope, $element, $attrs, $transclude) {},
               restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
               //当修改这里的name时，second会在自己的作用域中新建一个name变量，与父级作用域中的
               // name相对独立，所以再修改父级中的name对second中的name就不会有影响了
               template: 'second name:{{name}}',
           };
       }])
       ............
       .directive('third', [ function(){
           return {
               scope: {}, // 创建指令自己的独立作用域，与父级毫无关系
               // controller: function($scope, $element, $attrs, $transclude) {},
               restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
               template: 'third name:{{name}}',
           };
       }])
       .controller('DirectiveController', ['$scope', function($scope){
           $scope.name="mike";
       }]);
     ````
- 5.server
    + $location 服务，它可以返回当前页面的 URL 地址。
    + $http 是 AngularJS 应用中最常用的服务。 服务向服务器发送请求，应用响应服务器传送过来的数据。
    + AngularJS $timeout 服务对应了 JS window.setTimeout 函数。
    + AngularJS $interval 服务对应了 JS window.setInterval 函数。
    + 脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍
    + $apply:手动刷新视图
