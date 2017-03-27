### 第一周angular相关笔记整理(刘丽娜)

#### 基本介绍
框架 (angular、bootstrap...) 我们写好代码,框架帮我们调用,强制约束按照人家的规范来写代码。
库 (react vue jquery) 我们调用库中的方法, 我们是主动的

mvc (model view controller)
>单向,用户改变视图(表单元素), 更改后会触发控制器, 获取数据后, 再刷新视图

mvvm
>双向数据绑定: 页面变化 (表单元素), 会触发数据的变化, 数据的变化, 会更改页面的变化

#### 安装angular
   全局安装 (在命令行使用的)
   本地安装 (在代码里使用的)
   
   > npm 是基于node, 安装node后会直接安装npm, 安装模块到node_modules文件夹下,如果当前没有会像上一级找

#### angular 基本介绍
使用时将angular.js文件引入进来即可使用
>ng- 代表的是angular的自带指令
>ng-app 会生成一个根作用域$rootScope
>ng-model 实现数据双向绑定（可以绑定一个变量名），只能添加到表单元素上

```
{{exp|filter}} exp代表表达式，可以将作用域上的变量取出来，支持赋值运算，可以写三元运算符，可以运算变量+-*/,也可以调用函数(这里要用自定义函数,不能使用原生函数,若想使用,则在自定义函数里面封,也可以利用filter格式化数据)等

{{}}闪烁问题的解决
{{}}具有闪烁问题，ng-bind可以解决某个变量闪烁问题， 等价于{{}}, {{}}是ng-bind的简写
ng-cloak 给元素添加这个属性，让带有这个属性的元素隐藏， 当数据编译好后会自动移除掉ng-cloak属性，解决了成段代码的闪烁问题.
```

#### 模块与控制器的简介
在html元素处，指定启动模块，即np-app='模块名', 若要使用控制器，也要用ng-controller指定作用范围，其与标签的范围相同

```
//1.模块的名字 2.模块的依赖（默认写[]） 3.配置函数
    //一切从模块开始
    var app = angular.module('zfModule',[]); //声明模块,app代表的是当前模块
    //1.控制器的名字 2.控制器的一个定义,买一送一 送一个$scope = viewModel
    //为了防止压缩将构造函数的参数重命名为很短的名字， 比如a, 采用数组的形式 ，一一对应,因为字符串不会被js代码压缩器重命名
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
```

#### 页面上事件的绑定
>ng- 与事件名的方式绑定，如 ng-click, ng-mouseover等
在作用域上定义相关函数即可。

#### 数组与对象绑定到模板
>$event 事件源 显示传给绑定函数
>ng-repeat相当于循环
eg：
```
<!--要循环谁 就写在谁的身上-->
<!--ng-repeat="变量(数组中的value值) in 数据中" $index 索引（只要是数组 就通过索引来遍历）-->
    <li ng-repeat="a in arr track by $index">{{a}}</li>
```

遍历对象的另一种方式
```
<!--遍历时会产生作用域-->
<!--(key,value) in arrs 这个key是我们显示声明出来的 $index是angular提供给我们的-->
<li ng-repeat="(key,o) in obj">{{key}}:{{o}}</li>
```

#### nrm (npm resource manager) npm源管理器
```
npm install -g nrm x //安装
nrm ls //列出可用的源

nrm add <registry> <url> [home] //增加源
nrm del <registry> //删除
nrm test //测试源的速度 
nrm use cnpm //switch to cnpm
nrm home //go to a registry home page
npm root -g //查看全局安装目录
```

### angular 过滤器
分为内置过滤器与自定义过滤器,过滤器可以应用在视图模板中的表达式中,也可以在控制器和服务中使用过滤器

过滤器的格式如下:

```
{{ 表达式 | 过滤器名 }}
//过滤器可以应用在另外一个过滤器的结果之上, 这种叫做链式反应
{{ 表达式 | 过滤器1 | 过滤器2 | ... }}
//过滤器可以拥有（多个）参数
{{ 表达式 | 过滤器:参数1:参数2:... }}
```
内置过滤器如下:

```
<!--大小写-->
{{"asdasd" | uppercase}} <br>
{{"AASDASD"| lowercase}} <br>
<!--货币过滤器 默认在最前面增加$符号 3代表小数个数-->
{{"123123.123"| currency:'￥':3}} <br>
<!--限制过滤器-->
{{"你好吗" | limitTo:2}} <br>
<!--json过滤器 4代表子元素前面会有4个空格-->
<pre>{{ {name:1,age:4} | json:4 }}</pre>
<!--number过滤器-->
{{123123|number}}
<!--日期过滤器-->
{{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}

//这两个一般应用于控制器与服务中
<!--orderBy排序 filter过滤器 -->
<!--根据的是当前遍历对象上拥有的属性 第三个参数代表的是 是否降序 true代表的是降序-->
<!--filter可以指定查询{字段:查询条件} ，默认是全部-->
<tr ng-repeat="stu in students | orderBy:language:flag |filter:{english:query} track by $index">
```

自定义过滤器

使用和内置过滤器一样
实现:
```
//定义两个自定义过滤器 capitalized 和 multiFirstAlphaUppercase
var app = angular.module('appModule',[]);
    app.filter('capitalized', function () {
       return function (input) {
           return input.replace(input[0], input[0].toUpperCase());
       }
    });
    app.filter('multiFirstAlphaUppercase', function () {
        return function (input) {
            input = input.replace(/^\w|\s\w/g, function () {
                return arguments[0].toUpperCase();
            });
            return input;
        }
    }
```

#### ng-model-options 
指令绑定了 HTML 表单元素到 scope 变量中,input, select, textarea, 元素支持该指令.
```
<element ng-model-options="option"></element>
option: 指定了绑定规则,如下:
{updateOn: 'event'}规则指定事件发生后绑定数据
{debounce : 1000} 规定等待多少毫秒后绑定数据
{allowInvalid : true|false} 规定是否需要验证后绑定数据
```

补充:

```
ng-disabled
ng-readonly
//这个规定在select中使用 用来循环option的另一种方式
ng-options="dis.value as dis.name for dis in discount"
```