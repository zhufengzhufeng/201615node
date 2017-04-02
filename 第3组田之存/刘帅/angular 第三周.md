## angular 第三周
### jsonp
jsonp 由于script标签可以跨路径请求数据
css img 返回是我内容是样式和图片
jsonp使用规则要求
- 前台
  1 、必须在全局下有一个函数
  > function aa(data) {
        console.log(data); //63342 -> 8080 jsonp接口
    }
    
  2、构建一个script标签
  >  var oScript = document.createElement('script');
  
  3、请求一个接口路径
  >  oScript.src = 'http://localhost:8080/jsonp?callback=aa';  //访问8080端口并且告诉他我的回调函数名字叫aa
 4、最后在插入body中
 >  document.body.appendChild(oScript);

- 后台
```
if(pathname == '/jsonp'){
        let arr = {s:['爱奇艺','阿里巴巴']};
        //1.取出全局函数的名字
        var fnName = query.callback; // 'aa'
        //2.返回函数执行
        res.setHeader('Content-Type','application/javascript;charset=utf8');
        res.end(fnName+'('+JSON.stringify(arr)+')'); // cb({s:['爱奇艺','阿里巴巴']})
```
- exists( ) 查询的方法
通过一个函数查询，文件的路径是否存在
- setHeader（"Content-Type",mime.lokup(pathname)+';charset=utf8'）设置响应投的文件类型         通过第三方模块查看路径，并设置返回格式
- createReadStream('.'+pathname).pipe(res);  创建一个可读流，并通过pipe返回客户端
 



> RESTFul
> 对用户操作接口   getAllUser  getOneUser  updateUser  deleteUser  deleteOneUser  addUser
> /user  /user (/id)  可以有可以没有
> 增  POST   /user  {name:1}  返回增加后的内容
> 删  DELETE  /user/1  /user   返回的是空对象
> 改  PUT  /user/1  {name:2}  返回修改后的那一项
> 查  GET  /uesr/1  /uesr    /uesr 返回数组    /user/1 返回的是对象
> angular 提供了一个模块  angular-resource 实现了这种风格


### bootstrap 简单样式
- navbar-inverse  黑色的导航条
- container-fluid  一个流式布局
```
<div class="navbar navbar-inverse">
     <div class="container-fluid">
          <div class="navbar-header">
               <a href="#!/home" class="navber-brand">书名 </a>
               // a 挑战不能直接写  否则会到根目录径下和配置的路径一一对应
          </div>
     </div>
</div>	
```


###书店
在主模块中引用控制器模块
- angular-resource: 在我们的模块中  要使用angular-resource这个模块
```
var app = angular.module('appModule',['ngResource','ngRoute','appModule.controller']);
```

- app.factory:在我们封装的服务中使用angular-resource中的服务
```
app.factory('User',['$resource',function($resource){
    路径可以是  /user  或者 /user/值
    这个函数执行后返回的是一个函数   函数上有get query delete remove save 方法
    return  $resource('/user/:id',null,{"update":{method:'PUT'}     //增加update方法
         })  
    }])
```

- 前端路由  基于配置的  配置$route这个服务
```
app.config(['$routeProvider',function($routeProvider){
    $routeProvider.when('/home',{
        templateUrl:'tmpl/home.html',
        controller:'homeCtrl'
    }).when('/add',{
        templateUrl:'tmpl/add.html',
        controller:'addCtrll'   
    }).when('/list',{
        templateUrl:'tmpl/list.html',
        controller:'listCtrll'
    }).when('/detail/:bid',{ //这个ID必须传入  但是可以是随机的
        templateUrl:'tmpl/detail.html',
        controller:'detailCtrl'
    }).otherwise('/home');//如果都匹配不到可以跳转到首页
}])
```

### controller 控制器
基于以上数据，将controller 单独写一个模块
```
angular.module('appModule.controller',[])
.controller('homeCtrl',function($scope){
   $scope.hello = '欢迎惠顾'；
})
.controller('addCtrl',function($scope,Book,$location){
   $scope.add=function(){
       //添加逻辑    Book.save($scope.book).$promise.then(function(){
       // 保存成功后跳转到列表页面  路由里有一个提供跳转的服务 $location
       $location.path('/list')

      });
   }
})
.controller('listCtrl',function($scop,Book){
   $scope.books=Book.query();
})
.controller('detailCtrl',function($scope,$touteParams,Book,$location){
   //当前传过来的id号
   let id=$routeParams.bid;
   //获取id的值去服务端查询
   $scope.book=Book.get({bid:id}); //获取单个

   //1.删除操作
   $scope.remove=function(){
       Book.delete({bid:id}).$promise.then(function(){
      $location.path('/list');     
   })
}

   //2.声明一个控制变量，切换按钮的显示
   $scope.flag=true;
   $scope.changeFlag=function(){
      //当点击修改时  给temp赋值  book克隆一份-->temp
      $scope=temp=JSON.parse(JSON.stringfy($sope.book));
      $scope.flag=false;
   }

//3.真实修改
$scope.sure=function(){
   Book.update({bid:id},$scope.temp).$promise.then(function(){
     $scope.flag=true;
     $scope.book=$scope.temp;
  })
 }
});
```



