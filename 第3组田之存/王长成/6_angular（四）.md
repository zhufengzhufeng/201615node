### 服务
angular提供了五种自定义服务方式  provider factory" service constant value
#### 内置$http
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

#### provider
可配置,是唯一一种你可以传进 .config() 函数的服务:
```
app.provider('Calc',function () { 
        this.currency = '$'; //在实例上增加了属性
        this.$get = function () {
            return {
                sum: (a,b) => this.currency +(a+b)
            }
        };
    });
```
Calc为服务的名字,第二个参数接受一个构造函数,实例对象上要有一个￥get方法，$get方法必须要返回一个对象obj,这个对象就是真正被注入的服务.
- 构造函数本质是要造一个对象，所以provider的第二个函数参数也可以直接返回一个对象，这个对象上要有一个$get方法,$get方法必须要返回一个对象obj
```
app.provider('name',function(){
　　....
　　return {
　　　　...
　　　　$get:function(){
　　　　　　...
　　　　　　return obj
　　　　}
     }
})
```
- 在要配置的服务后面 增加Provider后缀
app.config(function (CalcProvider) { 
        //console.log(CalcProvider); //是provider的一个实例
        CalcProvider.currency = '£'
    });
- 执行顺序:先执行provider第二个参数函数中$get函数之前的语句，再执行config配置，再执行$get函数，返回被注入的服务

#### factory
app.factory('name',function(){return obj})
factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
```
serviceApp.factory('myConfig',function(){
    var myname = 'code_bunny';
    var age = 12;
    var id = 1;
    return {
        name: myname,
        age: age,
        getId: function(){
            return id
        }
    }
});
```
name为服务的名字,第二个参数传入一个函数,函数需要有一个返回值obj,返回一个对象.实际被注入的服务就是这个对象.

#### 其他自定义服务
最大的是provider  factory是基于provider的  service 是基于factory的  value是基于factory的

