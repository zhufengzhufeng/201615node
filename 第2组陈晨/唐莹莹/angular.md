##angular
#### 框架 angular bootstrap
我们写好代码，框架帮我们调用,强约束按照人家的规范来写代码。

####库 react vue jquery
我们调用库中的方法，我们是主动的
>angular MVC MVVM 简化代码，提高了开发效率
#### MVC
- model(数据)
- view (视图)
- controller (控制器)

> 单向，用户改变视图(表单元素)，更改后会触发控制器，获取数据后，在刷新视图

#### 双向数据绑定
- model
- view
- viewModel (视图模型)

> 页面变化（表单元素），会触发数据的变化,数据的变化，会更改页面的变化

####安装angular
#####全局安装（在命令行使用的）
```
npm install babel -g 
npm install bower -g 
```
##### 本地安装（在代码里使用的）
```
npm install jquery
npm install angular
```

> npm是基于node，安装node后会直接安装npm,安装模块到node_modules文件夹下，如果当前没有会像上一级找

#### 安装前生成package.json文件
```
npm init -y
```

> 默认安装angular1.0,市面1.5.8

####双向绑定
```
<!DOCTYPE html>
<!--告诉angular开始执行,在想要执行范围的标签上增加ng-app-->
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
    <title></title>
</head>

<body>

<!--1.修改视图影响数据 ng-model表示双向数据绑定，而且只能加在表单元素上.ng-model只能放变量-->

<input type="text" ng-model="name">
<!--表达式 可以支持赋值运算，可以写三元运算符，可以运算变量+-*/-->
<!--{{name?name:'hello zfpx'}}-->
<!--{{}}闪烁问题 ng-bind等价于{{}}，{{}}是ng-bind的简写，解决某个变量闪烁问题  数据变化，视图也要发生改变 {{}}可以将作用域上的变量取出来-->
<span ng-bind="name?name:'hello zfpx'"></span>
<!--解决成段代码的闪烁问题 先让带有ng-cloak的标签隐藏，当数据编译好后会自动移除掉ng-cloak属性-->
<style>
    /*属性选择器*/
    [ng-cloak]{display: none}
</style>
<ul ng-cloak>
    <li>
        {{name}} {{name}}
        <ul>
            <li>{{name}} {{name}}</li>
        </ul>
    </li>
</ul>
<!--模块化 自动给你实现了模块化-->
<script src="node_modules/angular/angular.js"></script>
</body>
</html>
```
####module
```
<script>
    //1.模块的名字 2.模块的依赖（默认写[] 不写是获取） 3.配置函数
    //一切从模块开始
    
    var app = angular.module('zfModule',[]); //声明模块,app代表的是当前模块
    
    //1.控制器的名字 2.控制器的一个定义,买一送一 送一个$scope = viewModel
    //为了防止压缩 采用数组的形式 ，一一对应
    
    app.run(['$rootScope',function ($rootScope) {
        $rootScope.eat = '吃'
    }]);//angular启动时会调用run方法，写了ng-app就开始执行
    
    app.controller("oneCtrl",["$scope",function($scope){
        $scope.name="zfpx";
        $scope.age = 9;
    }]);
    app.controller('twoCtrl',['$scope',function ($scope) {
        $scope.name = 'zf';
    }]);
    //作用域，如果当前没有可以向上找,父子关系可以继承,控制器可以嵌套
</script>
```
####filter
```
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


```

####自定义
```
{{"hello" | capitalized}}
<!--实现首字母大写-->
{{"welcome, to bei-jing" | multiFirstAlterUppercase}}
<!--实现多个单词首字母写-->
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.filter('capitalized',function () {
       return function (input) {//1.param1代表的是要过滤的数据
           //取出除了第一个的所有的参数的集合[]
           //console.log([].slice.call(arguments,1));
           //console.log(Array.from(arguments).slice(1));//类数组转换成数组
           //console.log(o);//...o
           return ;//最终显示到页面上的数据
       }
    });

```