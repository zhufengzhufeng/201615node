###Angular 

####框架（framework）
angular bootstrap ...
我们写好代码，框架帮我们调用，但是我们的代码必须强行按照所用框架约束的规则和规范来编写
####库（library）
react vue jQuery....
我们调用库里面的方法帮助我们解决想实现的功能，怎么写我们是主动决定的；
> 库和框架的不同点：
> 引stackoverflow某大牛回答的很精辟的一句话
> “it means that when you call a library, you are in control. But with a framework, the control is inverted: the framework calls you. (This is called the Hollywood Principle: Don't call Us, We'll call You.) ”
> 大概意思就是说当我需要调用库的时候，写什么或者怎么写都是我来决定的，而用框架的时候，控制发生反转，框架告诉你怎么用，框架让你必须这么用（有点霸道）；

具体优缺点，需要根据具体项目需求对比，库有库的优缺点，框架有框架的优缺点，没有绝对性...

#### MVC / MVVM
MV*的思想中心很一致：UI和逻辑分离，提取数据模型。
**MVC**  -Model View Controller
- model（ 模型 ） -> 数据 
- view （ 视图 ） -> UI
- controller （ 控制器 ）-> 处理/加工Model

工作模型：Controller=>Model=>View
单向，用户改变视图（ 表单元素 ），更改后会触发控制器，获取数据后，再刷新视图

**MVVM**  -Model View ViewModel
- model （ 数据 ）
- view （ 视图 ）
- viewModel （ 视图模型 ）

工作模型：Model<=>ViewModel<=>View
页面变化（表单元素），会触发数据的变化，数据的变化，会更改页面的变化

####angular安装
##### 全局安装 （命令行使用）
```
npm install babel -g
```
##### 本地安装（在代码里使用）
```
npm install jquery
npm install angular
```
> npm 是基于node的，安装node后自带,安装模块到node_modules文件夹下，如果当前没有会向上一层找, 所以安装前生成package.json 文件，执行
> ```
> npm -init -y
> ```
>默认安装angular1.0 市面上一般用1.5.8

#### angular 基本语法：

- 生成根作用域：
```
ng-app="app-Module"    
//增加ng-app后会生成一个根作用域$rootScope,告诉angular开始执行，在想要执行的范围的标签上增加ng-app
```
- 双向数据绑定：
```
ng-model="age"  
//ng-model表示双向数据绑定，而且只能加载表单元素上
//ng-model可以绑定一个变量名。
//M->V:将name(作用域上定义的内容)代表的值放到输入框内
//V->M:如果修改输入框中的内容，可以改变作用域上name的值
// 如果写的格式是ng-model="person.name",并且person不存在会声明一个对象
```
- 取值表达式：
```
{{age}} 
//数据变化，视图也要发生改变 {{}}可以将作用域上的变量取出来
```
- 解决多行闪烁问题
```
<style>
    [ng-cloak]{display:none}
</style>
<div ng-cloak></div>
```

```
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" ng-model="age">
{{age}}
<div ng-bind='age'></div>
<!-- -->
<script src="node_modules/angular/angular.js"></script>
<script>
    console.log(angular)
</script>
</body>
</html>
```

- 控制器：
```
<!--控制器的作用范围和标签的范围相同-->
<div ng-controller="oneCtrl">{{name}}</div>
<div ng-controller="twoCtrl">{{name}}</div>
<script src="../../node_modules/angular/angular.min.js"></script>
<script>
    //1.模块的名字 2.模块的依赖（默认写[]） 3.配置函数
    //一切从模块开始
    var app = angular.module('zfModule',[]); //声明模块,app代表的是当前模块
    //1.控制器的名字 2.控制器的一个定义,买一送一 送一个$scope
    app.controller('oneCtrl',function ($scope) {
        $scope.name = 'zfpx';
    });
    app.controller('twoCtrl',function ($scope) {
        $scope.name = 'zf';
    });
</script>

```

- Angular 中添加事件：

```
<body ng-controller="myCtrl">
<!--$event 是事件源，相当于DOM2中的e -->
<input type="text" ng-keyup="addNumber($event)" ng-model="n">
<input type="button" value="删除" ng-click="removeNumber($event)">
<ul>
    <li ng-repeat="a in arrs track by $index">{{a}}</li>
</ul>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',["$scope",function ($scope) {
        $scope.arrs = [1,2,3,4];
        $scope.addNumber = function (e) {
            var keyCode = e.keyCode;
            if(keyCode==13){
                $scope.arrs.unshift($scope.n);
                $scope.n = '';
            }
        }
        $scope.removeNumber =function (e) {
            $scope.arrs.shift($scope.n);
        }
    }]);
</script>
```

Angular 中的遍历：（ng-repeat）
可以遍历数组或对象,遍历谁，写在谁的身上
```
<body ng-controller="myCtrl">
<div>
    <ul class="list-group">
        <li class="list-group-item" ng-repeat="food in foods track by $index">
            <div class="text-danger h2">{{food.name}}</div>
            <ul class="list-group">
                <li class="list-group-item text-center" ng-repeat="a in food.type track by $index">{{a}}</li>
            </ul>
        </li>
    </ul>
    <ul>
        <li ng-repeat="(key,a) in obj">{{key}} : {{a}}</li>
    </ul>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',['$scope',function ($scope) {
        $scope.foods = [
            {name:'馒头',type:["糯米的","苞米面","白面的"]},
            {name:'蔬菜',type:["白菜","豆角","辣椒"]}
        ];
        $scope.obj = {
            name:'zfpx',
            age:8,
            address:'回龙观'
        }
    }])
</script>
</body>
```
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
