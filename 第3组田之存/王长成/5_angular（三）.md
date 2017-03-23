### directive指令间的交互
#### directive指令依赖
- require:'^girl'自己身上找找不到向上找，找到则会在link函数的第四个参数增加girl控制器的实例
- require:'^?girl'如果没有依赖到使用的是? 则得到的值是null，否则报错
```
app.directive('loveMoney',function () {
    return {
        require:'^?girl',
        link: function (scope,element,attrs,girlCtrl) {
            girlCtrl.add('loveMoney');
        }
    }
});
```
#### opener组demo
样式：
```
.title{
    width: 100px;
    height: 30px;
    line-height: 30px;
    background: yellow;
}
.content{
    width: 100px;
    height: 100px;
    background: red;
}
```

增加指令组：
```
<group>
    <opener title="标题1">这是内容1</opener>
    <opener title="标题2">这是内容2</opener>
</group>
```

增加引用模板：
```
<div class="title" ng-click="show()">{{title}}</div>
<div class="content" ng-show="flag" ng-transclude></div>
```

增加指令
```
app.directive('group', function () {
    return {
        controller: function ($scope) {
            var arr = [];
            this.add = function (scope) {
                arr.push(scope);
            }
            this.close = function (scope) {
                for(var i = 0; i<arr.length;i++){
                    if(arr[i]!=scope){
                        arr[i].flag = false;
                    }
                }
            }
        }
    }
});
app.directive('opener',function () {
    return {
        templateUrl:'open.html',
        transclude:true,
        require:'^group',
        scope:{
            title:'@'
        },
        link:function(scope,element,attrs,groupCtrl){
            scope.flag = false;
            scope.show = function () {
                scope.flag = !scope.flag;
                groupCtrl.close(scope);
            };
            groupCtrl.add(scope);
        }
    }
});
```

### angular方法
#### $watch(watchExpression, listener, objectEquality)
监听模型变化
```
$scope.$watch(function() {
    return $scope.foo;
}, function(newVal, oldVal) {
    console.log(newVal, oldVal);
});
```
- watchExpression：监听的对象，它可以是一个angular表达式如'name',或函数如function(){return $scope.name}。

- listener:当watchExpression变化时会被调用的函数或者表达式,它接收3个参数：newValue(新值), oldValue(旧值), scope(作用域的引用)

- objectEquality：是否深度监听，如果设置为true,它告诉Angular检查所监控的对象中每一个属性的变化. 如果你希望监控数组的个别元素或者对象的属性而不是一个普通的值, 那么你应该使用它

#### $apply
AngularJS外部的控制器（DOM 事件、外部的回调函数如 jQuery UI 空间等）调用了AngularJS 函数之后，必须调用$apply。即不是angular自带的方法，数据更新不会影响视图在这种情况下，你需要命令 AngularJS刷新自已。

#### $http
我们可以使用内置的$http服务直接同外部进行通信。$http服务只是简单的封装了浏览器原生的XMLHttpRequest对象,用法同jquery
>$http服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成HTTP请求的
 配置内容。这个函数返回一个promise对象，具有success和error两个方法。返回一个promise对象
 ```
 var promise=$http({
 method:'GET',
 url:"data.json"
 });
 ```
 
 由于$http方法返回一个promise对象，对象中有一个then方法来处理回调，方法中有两个参数，第一个是成功的回调第二个是失败的回调
 ```
 $http.jsonp(
       $sce.trustAsResourceUrl(
       'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query),
       {jsonpCallbackParam: 'cb'})
       .then(
       function (res) { //成功
                 $scope.arrs = res.data.s;
             },
       function (err) { //失败
                 console.log(err);
             });
```
