## 框架 angular bootstrap
我们写好代码，框架帮我们调用,强约束按照人家的规范来写代码。

## 库 react vue jquery
我们调用库中的方法，我们是主动的

## angular MVC MVVM 简化代码，提高了开发效率
### MVC
- model(数据)
- view (视图)
- controller (控制器)

> 单向，用户改变视图(表单元素)，更改后会触发控制器，获取数据后，在刷新视图

## 双向数据绑定
- model
- view
- viewModel (视图模型)

> 页面变化（表单元素），会触发数据的变化,数据的变化，会更改页面的变化

## 安装angular
### 全局安装（在命令行使用的）
```
npm install babel -g 
npm install bower -g 
```
### 本地安装（在代码里使用的）
```
npm install jquery
npm install angular
```

> npm是基于node，安装node后会直接安装npm,安装模块到node_modules文件夹下，如果当前没有会像上一级找

## 安装前生成package.json文件
```
npm init -y
```

> 默认安装angular1.0,市面1.5.8




##angular
- ng 代表的是angular的自带指令
- 在想要执行范围的标签上增加ng-app,增加ng-app后会生成一个根作用域$rootScope，让angular启动,可以指定模块名,告诉angular以当前模块来启动

### ng-model
实现双向数据绑定,放在表单元素上
```
ng-model="name"
```
M->V:将name(作用域上定义的内容)代表的值放到输入框内
V->M:如果修改输入框中的内容，可以改变作用域上name的值

> 如果写的格式是ng-model="person.name",并且person不存在会声明一个对象
- 1.修改视图影响数据 ng-model表示双向数据绑定，而且只能加在表单元素上
    ng-model可以绑定一个变量名
    赋予默认值必须是字符串
- 2.数据变化，视图也要发生改变 {{}}可以将作用域上的变量取出来  
 {{}} 表达式 可以支持赋值运算，可以写三元运算符，可以运算变量+-*/
 
 {{}}闪烁问题 解决方法
    - 1.ng-bind等价于{{}}，{{}}是ng-bind的简写，解决某个变量闪烁问题
    - 2.解决成段代码的闪烁问题 先让带有ng-cloak的标签隐藏，当数据编译好后会自动移除掉ng-cloak属性-->
        <style>
            /*属性选择器*/
            [ng-cloak]{display: none}
        </style>
### 声明模块
一切从模块开始      
 1.模块的名字 2.模块的依赖（默认写[]） 3.配置函数
 var app = angular.module('模块名字',[]); app代表的是当前模块

### ng-controller
会产生一个作用域，作用范围和DOM平齐，可以嵌套控制器，子可以继承父亲。不要操作DOM
1.控制器的名字 2.控制器的一个定义,买一送一 送一个$scope = viewModel
 为了防止压缩 采用数组的形式 ，一一对应
 所有数据都是挂在在$scope上的属性，不会预解释
 app.controller("oneCtrl",["$scope",function($scope){
         $scope.name="zfpx";
         $scope.age = 9;
     }]);
     app.controller('twoCtrl',['$scope',function ($scope) {
         $scope.name = 'zf';
     }]);
     
  angular启动时会调用run方法，写了ng-app就开始执行   
     app.run(['$rootScope',function ($rootScope) {
         $rootScope.eat = '吃'
     }]);

    作用域，如果当前没有可以向上找,父子关系可以继承,控制器可以嵌套
    
### ng-事件 
事件，在angular可以通过ng-事件来执行作用域上的方法，如果需要事件源，需要手动传递$event对象
 要在视图上取出数据 必须将数据挂载$scope上 
 页面上这样写：<button ng-click="say($event,'你好')" id="btn">点我啊</button>
 
 $scope.say = function(e,who) {//在作用域上定义函数，在页面上用ng- + 事件名的方式执行
             console.log(e);
             //console.log(this==$scope);
             alert(who);
         };

数组
 ng-repeat要循环谁 就写在谁的身上 
ng-repeat="变量(数组中的value值) in 数据中" $index 索引（只要是数组 就通过索引来遍历）
遍历数组：ng-repeat="a in arr track by $index"
angular的核心是数据，数据更改视图会自动刷新
遍历对象:(key,value) in arrs 这个key是我们显示声明出来的 $index是angular提供给我们的


### find数组的方法
"查找"es6,找到后不会继续查找，返回找到的那一项
如果找到返回true即可，会将item返回回去,不会继续执行
var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
var obj = arr.find(function (item,index) {
        return item.name == 2;//如果找到返回true即可，会将item返回回去,不会继续执行
    });
    
### map数组的方法
"修改"映射,返回是一个新的数组
返回什么内容，就会将这一项变成什么内容
 var newArr = [1,2,3,2,2,2].map(function (item,index) {
        if(item == 2){
            return 100;
        }
        return item;
    });
   
remove    
$scope.remove = function (p) {
            $scope.products = $scope.products.filter(function (item) {
                return p!=item;
           });
返回false表示当前项移除掉    

过滤器
大小写
{{"asdasd" | uppercase}} 
{{"AASDASD"| lowercase}} 
货币过滤器 默认在最前面增加$符号
{{"123123.123"| currency:'￥':3}} 
限制过滤器
{{"你好吗" | limitTo:2}}
json过滤器
<pre>{{ {name:1,age:4} | json:4 }}
number过滤器
{{123123|number}}
日期过滤器
{{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}

orderBy排序 
根据的是当前遍历对象上拥有的属性 第三个参数代表的是 是否降序 true代表的是降序
    <tr ng-repeat="stu in students | orderBy:language:flag track by $index">
        <td>{{stu.name}}</td>
        <td>{{stu.english}}</td>
        <td>{{stu.chinese}}</td>
        <td>{{stu.math}}</td>
    </tr>

### filter数组的方法
"删除"过滤,返回是一个新的数组
push,pop,unshift,shift,slice,splice,concat,forEach,join,map,sort,indexof,reverse,lastIndexOf,every,some
```
var newArr = [].filter(function(item,index){
    return true保留这一项/false这项不要了
},this指向);
```

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

## 安装本地依赖时
--save  --save-dev

##bootstrap  css组件
默认颜色 default 灰色
    danger 红色
    warning 黄色
    primary 蓝色
    info 浅蓝色
    success 绿色


