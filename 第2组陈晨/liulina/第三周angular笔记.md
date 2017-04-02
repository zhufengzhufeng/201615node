#### 跨域的两种实现方式
##### jsonp 方式
>原理：由于script标签可以跨路径请求数据.(只能发送get请求)
优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据.
```
//前台模拟
<script>
    //要求 1.必须在全局下有一个函数
    function cb(data) {
        console.log(data); //63342 -> 8080 jsonp接口
    }
    //2.构建一个script标签
    var oScript = document.createElement('script');
    //3.请求一个接口路径
    oScript.src = 'http://localhost:8080/jsonp?callback=cb';//访问8080端口并且告诉他我的回调函数名字叫cb
    document.body.appendChild(oScript);
</script>
//后台模拟
if(pathname == '/jsonp'){
        let arr = {s:['爱奇艺','阿里巴巴']};
        //1.取出全局函数的名字
        var fnName = query.callback; // 'cb'
        //2.返回函数执行
        res.setHeader('Content-Type','application/javascript;charset=utf8');
        res.end(fnName+'('+JSON.stringify(arr)+')'); // cb({s:['爱奇艺','阿里巴巴']})
}
```
##### 跨域资源共享 CORS （Cross-origin resource sharing）支持所有类型的http请求
同源策略：域名，协议，端口相同
> CORS需要浏览器和服务器同时支持, ie10以下不支持。
> 浏览器端一般自动完成， 关键是服务器端支持.
> 一般分为两种请求（简单请求和非简单请求）
```
//设置跨域头，只能在服务端设置，app是没有跨域问题的，在chrome浏览器会有跨域问题
//设置那个域名可以访问我  http://localhost:8080
res.setHeader('Access-Control-Allow-Origin','*');
res.setHeader('Access-Control-Allow-Methods','GET,DELETE,POST,PUT,OPTIONS');
res.setHeader('Access-Control-Allow-Headers','Content-Type');
//先发送一个 options 请求 ，如果能响应到 ，再去发送真正的请求(针对复杂请求)
if(req.method == 'OPTIONS'){ //方法名必须是大写
    res.end();
}
```

#### 模块的依赖
> 我们将使用angular-resource这个模块, 这个模块会提供符合restful规范的一些功能。
```
//RESTFul 规范 风格 规定了返回值
// /user   /user（/id）可以有可以没有
// 增 POST /user   {name:1}    返回增加后的那一项内容 返回对象
// 删 DELETE /user/1   /user   返回的是空对象 
// 改 PUT   /user/1  {name:2}  返回修改后的那一项 返回对象
// 查 GET  /user/1   /user     /user返回数组  /user/1 返回的是对象
```
```
//在我们的模块中 要使用angular-resource这个模块
 var app = angular.module('appModule',['ngResource']);
 //在我们封装的服务中使用angular-resource中的服务
 app.factory('User',['$resource',function ($resource) {
     //路径可以是  /user 或者 /user/值
     //这个函数执行后返回的是一个函数 函数上有get query delete remove save方法
     return $resource('/user/:id',null,{
         "update":{method:'PUT'}  //增加update方法
     });
 }]);
 app.controller('myCtrl',['$http','$scope','User',function ($http,$scope,User) {
//User 为$resource函数执行后返回的函数
     $scope.ary = User.query(); //GET /user 同步方式
     /*User.query().$promise.then(function (data) { //直接使用data即可
         $scope.ary = data;
     });*/
     //还有一些其他我们会用到的方法
     //User.update({id: 对应id值});
     //User.save(data);
     //User.delete({id: 对应id值});
 }]);
```
补充：
 reduce 示例
```
//所有的数据操作 都是map reduce
//reduce(callback, 可选参数). 这个可选参数如果设置了。则为callback的第一个参数的值，若不传，第一个参数则为数组的第一项。下一次第一个参数就变为上一次的累计结果.
var arr1 = [{name:'zfpx',age:1},{name:'zfppx',age:2}];
var result = arr1.reduce((currentState,item)=>{
    currentState.push({name:item.name});
    return currentState;
},[]);
console.log(result);
```
对象克隆的方式
```
//第一种
var obj1 = {age: 1};
var obj = JSON.parse(JSON.stringify(obj1)); //常用对象的克隆方法
//第二种
//2.es6 Object.assign
var obj = {};
Object.assign(obj, obj1); //得到obj
//第三种 循环对象
```

#### angular-route
> 首先需要按照angular-route.用来做路由的。使用示例代码如下:
```
//注册所需的服务，控制器，指令等...
var app = angular.module('appModule', ['ngResource', 'ngRoute', 'appModule.controller']);
//ngRoute的使用
app.config(['$routeProvider', function ($routeProvider) {
        $routeProvider.when('/home', {
            templateUrl: 'tmpl/home.html',
            controller: 'homeCtrl'
        }).when('/add', {
            templateUrl: 'tmpl/add.html',
            controller: 'addCtrl'
        }).when('/list', {
            templateUrl: 'tmpl/list.html',
            controller: 'listCtrl'
        }).when('/detail/:bid', {
            templateUrl: 'tmpl/detail.html',
            controller: 'detailCtrl'
        }).otherwise('/home');
    }]);
```
> ngRoute 还提供了我们所需的\$location, $routeParams 服务.
> - $routeParams 是一个对象，会取到url中query部分的键值对.
> - \$location用来实现跳转的.$location.path('/list');
> ng-view 指令 用来在页面中显示templateUrl或者template所代表的dom结构。

#### angular 中的form表单
> 在angular中，给表单元素提供了验证机制，要求是在form上添加name属性，在input等要验证的元素上也要填相应的name属性。这样才能进行验证。
```
//假如form标签的name="myform", 则myform就是一个对象。可以做验证相关。例如bookName 对象里面属性就可以对表单元素name="bookName"的元素进行校验。
"bookName": {
    "$validators": {},
    "$asyncValidators": {},
    "$parsers": [],
    "$formatters": [
      null
    ],
    "$viewChangeListeners": [],
    "$untouched": true,
    "$touched": false,
    "$pristine": true,
    "$dirty": false,//是否变脏
    "$valid": false,//是否有效
    "$invalid": true,
    "$error": {
      "required": true//是否触发了必填错误等.
    },
    "$name": "bookName",
    "$options": {}
  },
```

补充: 
> ng-src : 等数据准备好，再请求路径.
> ng-pattern: 正则校验 
