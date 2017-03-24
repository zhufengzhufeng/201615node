### angular

#### 安装angular
- 全局 在命令行使用的
```
npm install angular -g
```
- 本地安装 在代码里使用的
```
npm install abgular
npm install jquery
```

#### angular 中的自带命令 ng-

- 告诉angular开始执行,在想要执行范围的标签上增加ng-app  一般在html中添加 ng-app，增加ng-app后会生成一个根作用域$rootScope
- ng-model 表示双向数据的绑定，但是只能加在表单元素上

####双向数据的绑定

1 修改视图影响数据   ng-model只能放变量，支持赋值运算、三元运算、变量的加减乘除
```
<input type="text" ng-model="name">
{{name?name:'hello zfpx'}}
```
2 修改数据影响视图， {{}}可以将作用域上的变量取出来
  - {{}}的闪烁   ng-bind等价于{{}}，{{}}是ng-bind的简写，解决某个变量闪烁问题
```
{{name}}

<span ng-bind="name?name:'hello zfpx'"></span>
```
  - 解决成段代码的闪烁问题 先让带有ng-cloak的标签隐藏，当数据编译好后会自动移除掉ng-cloak属性
```
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
```
在window上声明一个angular
```
<script>
   console.log(angular);
</script>
```

#### Module
- 指定启动的模块
```
<html lang="en" ng-app="zfModule">
```
- 控制器的作用范围和标签的范围相同
```
<body ng-controller="myCtrl">
</body>
```
- 1.模块的名字 2.模块的依赖（默认写[]） 3.配置函数
- 一切从模块开始
```
var app = angular.module('zfModule',[]); //声明模块,app代表的是当前模块
1.控制器的名字 2.控制器的一个定义,买一送一 送一个$scope = viewModel
为了防止压缩 采用数组的形式 ，一一对应
```
- 根作用域下的变量
```
app.run(['$rootScope',function ($rootScope) {
        $rootScope.eat = '吃'
    }]);//angular启动时会调用run方法，写了ng-app就开始执行
```
- angular中的数据、变量一般都会挂在$scope上
```
$scope.name="zfpx";
$scope.age = 9;

var app = angular.module('appModule',[]);
    app.controller('myCtrl',['$scope',function ($scope) {
        $scope.foods = [
            {name:'馒头',type:["糯米的","苞米面","白面的"]},
            {name:'蔬菜类',type:["白菜","辣椒"]}
        ];
        $scope.obj = {name:'zfpx',age:8,address:'回龙观'}
    }]);
```
- 作用域，如果当前没有可以向上找,父子关系可以继承,控制器可以嵌套
```
<div>
        {{name}}
    <div ng-controller="twoCtrl">
        {{name}}
    </div>
</div>
```

ng-click
- angular事件  事件源需要传递参数 $event   '你好' 就是第二个参数who
```
<div ng-controller="myCtrl">

    <button ng-click="say($event,'你好')" id="btn">点我啊</button>
</div>

<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',['$scope',function ($scope) {
        //要在视图上取出数据 必须将数据挂载$scope上
        $scope.a = 100;
        //如果想获取事件源  需要出传递$event
        $scope.say = function(e,who) {//在作用域上定义函数，在页面上用ng- + 事件名的方式执行
            console.log(e);
            //console.log(this==$scope);
            alert(who);
        };
        //对象数据类型
    }]);
</script>
```

数组以及 ng-repeat
循环谁，就把ng-repeat放在谁里面
```
遍历时会产生作用域
(key,value) in arrs 这个key是我们显示声明出来的 $index是angular提供给我们的

<ul>
    <!--ng-repeat="变量(数组中的value值) in 数据中" $index 索引（只要是数组 就通过索引来遍历）-->
    <li ng-repeat="a in arr track by $index">{{a}}</li>
</ul>
```


#### angular 中的find、map、forEach
- forEach  遍历数组
- 兼容遍历数组
```
[1,2,3].forEach(function (item,index) {
        console.log(item,index);
    });

Array.prototype.forEach = function (callback) {
        for(var i = 0; i<this.length;i++){
            console.log(1);
            callback.call(window,this[i],i)
        }
    };
```
- find 如果找到返回true即可，会将item返回回去,不会继续执行
```
var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
    var obj = arr.find(function (item,index) {
        return item.name == 2;
    });
console.log(obj);
```
- map 将数组中的某项改变
```
var newArr = [1,2,3,2,2,2].map(function (item,index) {
    if(item == 2){
         return 100;
     }
      return item;
 });
console.log(newArr);
```
其中map、find都会返回一个新的数组

#### angular 过滤器
- 内置过滤器 
```
<!--大小写-->
{{"asdasd" | uppercase}} <br>
{{"AASDASD"| lowercase}} <br>
```
```
<!--货币过滤器 默认在最前面增加$符号-->
{{"123123.123"| currency:'￥':3}} <br>
```
```
<!--限制过滤器-->
{{"你好吗" | limitTo:2}} <br>
```
```
<!--json过滤器-->
<pre>{{ {name:1,age:4} | json:4 }}</pre>
```
```
<!--number过滤器-->
{{123123|number}}
```
```
<!--日期过滤器-->
{{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}
```
```
orderBy排序

filter过滤器

stu in students | orderBy:language:flag |filter:{english:query} track by $index

其中filter:orderBy:language:flag 是升降序
   {english:query}  依照查询条件，可以跟多个值
```
```
商品的购物车中，一般数量的加减 
 ng-disabled="product.productCount==1"  要求最少的数量是1
 ng-readonly="true"  是对于已经显示在输入框中的数量，只可阅读，补课更改
```
为了防止数据输入之后，浏览器放映过快：
1 ng-model-options="{debounce:500}"  延迟加载
2 ng-model-options="{updateOn:'blur'}  失去焦点刷新数据
- 自定义过滤器




