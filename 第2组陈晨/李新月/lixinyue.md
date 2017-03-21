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


### filter数组的方法
"删除"过滤,返回是一个新的数组
push,pop,unshift,shift,slice,splice,concat,forEach,join,map,sort,indexof,reverse,lastIndexOf,every,some

### map数组的方法
"修改"映射,返回是一个新的数组

### find数组的方法
"查找"es6,找到后不会继续查找，返回找到的那一

### 提交并关联仓库
```
git add .
git commit -m ''
git remote add origin 地址
git remote -v 查看所有关联
git remote rm origin 删除
```


更新代码

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