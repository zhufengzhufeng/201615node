##Angular
###AngularJS简介
AngularJS是一个JavaScript框架，它通过指令扩展了HTML，通过表达式绑定数据到HTML，由此使得开发现代的单一页面应用程序变得更加容易
- 可以把应用程序数据绑定到HTML元素。
- 可以克隆和重复HTML元素。
- 可以隐藏和显示HTML元素。
- 可以在HTML元素"背后"添加代码。
- 支持输入验证。

###MVC与MVVM
####MVC(Model-View-Controller)
> MVC是将Model, View和Controller分离，让彼此的职责明确的分开，这样不论是改M, V还是C，都可以确保另外两层可不用做任何修改，同时这样的分层也可以加强程式的可测试性，View和Model基本上是相关的，但它们并不会有直接的相依关系，而是由Controller去决定Model产生的资料，然后丢给View去做呈现，也就是说，Controller是Model和View之间的协调者，View和Model不能直接沟通，以确保责任的分离。

单向数据绑定，用户改变视图(表单元素)，更改后会触发控制器，获取数据后，在刷新视图：

![enter image description here](http://upload-images.jianshu.io/upload_images/4886613-eb564b66a8a4646e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
单向绑定的优点是相应的可以带来单向数据流，这样做的好处是所有状态变化都可以被记录、跟踪，状态变化通过手动调用通知，源头易追溯。同时组件数据只有唯一的入口和出口，使得程序更直观更容易理解，有利于应用的可维护性。缺点则是代码量会相应的上升，数据的流转过程变长。


####MVVM(Model-View-ViewModel)
> MVVM的架构一样是M, V分离，但中间是以VM (ViewModel)来串接，ViewModel负责直接对Model做沟通，而View可以透过一些机制来和ViewModel沟通以取得资料或将资料抛给Model做存取等工作。

双向数据绑定，页面变化（表单元素），会触发数据的变化,数据的变化，会更改页面的变化：

![enter image description here](http://upload-images.jianshu.io/upload_images/4886613-0eaf328feaa044f8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
双向绑定优点是在表单交互较多的场景下，会简化大量业务无关的代码。缺点就是我们无法追踪局部状态的变化(虽然大部分情况下我们并不关心)，潜在的行为太多也增加了出错时 debug 的难度。同时由于组件数据变化来源入口变得可能不止一个，新手玩家很容易将数据流转方向弄得紊乱，如果再缺乏一些“管制”手段，最后就很容易因为一处错误操作造成应用雪崩。

###Angular安装与nrm切换源
- 全局安装：
```
npm install babel -g 
npm install bower -g 
```
- 本地安装：
```
npm install jquery
npm install angular
```

- 安装前项目需初始化生成package.json文件
```
npm init -y
```
> npm是基于node，安装node后会直接安装npm,安装模块到node_modules文件夹下，如果当前没有会像上一级找
> 默认安装angular1.0,市面1.5.8

nrm切换到国内源下载
- 全局安装nrm：
```
npm install nrm -g
```
- nrm切换源：
```
nrm use <源名称>
```
- 查看所有源：
```
nrm ls
```
- 测试源的速度：
```
nrm test
```
- 添加珠峰源：
```
nrm add zhufeng http://172.18.1.139
```
- 删除源：
```
nrm del zhufeng
```
- 查看全局安装目录 ：
```
npm root -g
```


###AngularJS 指令
AngularJS 指令是扩展的 HTML 属性，带有前缀 ng-

- ng-app 指令初始化一个 AngularJS 应用程序，定义了 AngularJS 应用程序的根元素。
- ng-bind 绑定 HTML 元素到应用程序数据,和{{ }}作用一致,可以解决闪烁单行问题。
- ng-model 指令把元素值（比如输入域的值）绑定到应用程序，实现双向数据绑定,放在表单元素上。
  - ng-model 指令也可以：
  - 为应用程序数据提供类型验证（number、email、required）。
  - 为应用程序数据提供状态（invalid、dirty、touched、error）。
  - 为 HTML 元素提供 CSS 类。
  - 绑定 HTML 元素到 HTML 表单。

- ng-repeat 可以遍历数组或对象,遍历在谁，写在谁的身上，指令对于集合中（数组中）的每个项会 克隆一次 HTML 元素。
- ng-controller 会产生一个作用域，作用范围和DOM平齐，可以嵌套控制器，子可以继承父亲。
- ng-cloak 解决多行闪烁问题，需加上样式：[ng-cloak]{display:none}
- ng-事件 （-ng-click...）通过ng-事件来执行作用域上的方法，如果需要事件源，需要手动传递$event对象

Angular简单购物车功能示例代码：
```
<!doctype html>
<html lang="en" ng-app="appModule">
<!--初始化一个 AngularJS 应用程序，在整个html元素为angular根元素-->
<head>
    <meta charset="UTF-8">
    <title>简单购物车功能示例</title>
</head>
<body>
<div ng-controller="myCtrl"><!--产生一个作用域，在当前div下有效-->
    <ul>
        <li ng-repeat="(key,product) in products track by $index">
           <!--key为索引，product为索引对应项，可以只写一项：product in products-->
           <!--track by $index表示通过索引遍历，性能更好，同时不会因值相同产生错误-->

            {{product.name}}<!--双大括号绑定数据-->
            <button ng-click="remove(product)">删除</button><!--绑定remove函数，必须加括号，需要传事件源则加$event-->
        </li>
    </ul>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);//声明模块,app代表的是当前模块
    
    app.controller('myCtrl',['$scope',function ($scope) {
        //1.控制器的名字 2.控制器的一个定义 $scope 即 viewModel
        //因传参规定必须为$scope,为了防止压缩改变 采用数组的形式
        
        //运用localStorage本地存储数据
        if(localStorage.getItem('products')){
            //先获取本地内容，如果有
            $scope.products = JSON.parse(localStorage.getItem('products'));
        }else{
            $scope.products = [
                {name:'冰箱'},
                {name:'电脑'},
                {name:'电视'},
            ];
            localStorage.setItem('products',JSON.stringify($scope.products));
        }
        $scope.remove = function (p) { //filter过滤，如果遇到不想要的返回false即可,返回新数组
            $scope.products = $scope.products.filter(function (item) {
                return  item != p;//返回false表示当前项移除掉
            });
            localStorage.setItem('products',JSON.stringify($scope.products));
        };
    }]);
</script>
</body>
</html>
```

###Angular过滤器
####内置过滤器：
大小写:
```
{{"asdasd" | uppercase}} 
{{"AASDASD"| lowercase}}
```

货币过滤器 默认在最前面增加$符号:
```
{{"123123.123"| currency:'￥':3}}
```

限制过滤器:
```
{{"你好吗" | limitTo:2}}
```

json过滤器:
```
<pre>{{ {name:1,age:4} | json:4 }}
```

number过滤器:
```
{{123123|number}}
```

日期过滤器:
```
{{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}
```
####自定义过滤器：
```
<body>
{{"hello" | capitalized}}
<!--实现首字母大写-->
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.filter('capitalized',function () {
       return function (input,param1) {//1.param1代表的是要过滤的数据
           //取出除了第一个的所有的参数的集合[]
           //console.log([].slice.call(arguments,1));
           //console.log(Array.from(arguments).slice(1));//类数组转换成数组
           //console.log(o);//...o
           return input;//最终显示到页面上的数据
       }
    });
</script>
</body>
```










###es5,es6方法
filter数组的方法
"删除"过滤,返回是一个新的数组
push,pop,unshift,shift,slice,splice,concat,forEach,join,map,sort,indexof,reverse,lastIndexOf,every,some
```
var newArr = [].filter(function(item,index){
    return true保留这一项/false这项不要了
},this指向);
```

 map数组的方法
"修改"映射,返回是一个新的数组
```
var newArr = [1,2,3,2,2,2].map(function (item,index) {
    return `<li>${item}</li>`;//返回什么内容，就会将这一项变成什么内容
});
console.log(newArr);
``` 

find数组的方法
"查找"es6,找到后不会继续查找，返回找到的那一项
```
var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
var obj = arr.find(function (item,index) {
    return item.name == 2;//如果找到返回true即可，会将item返回回去,不会继续执行
});
``` 
