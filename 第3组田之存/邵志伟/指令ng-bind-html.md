###  指令ng-bind-html
- 结合使用 $sce.trustAsHtml([String])
###    指令
- 内置指令
- 自定义指令
	+   装饰指令 : 利用link函数赋予标签功能 
	+  组件: 利用template把自定义的组件封装成一个标签使用
	+  使用: 一般使用装饰指令给标签添加属性来增加功能 ，自定义标签使用模板 
-   指令的四种格式
	+ E：Element
	+ A：Attribute
	+ C:   Class
	+ M:  Comment ( 一般不使用)
	```
		app.directive("apple",function(){
				return {
					 // replace将原来标签替换掉,   模板只能这样写有一个根节点,  多个会报错   
					replace:true,  
					restrict:"EACM",       // 默认为EA
					template:`<div><div>world</div></div>`
			}
		});
		
		//link函数加载指令时，就执行了一遍
		app.directive("drag",function(){
			return {
				restrict:"A",
				link: function($scope,element,attrs){
					// element是jquery对象
				}
			}
		});
		
	```
- 组件
	+ 优点：复用性强
	+ link函数通过attrs来获取当前或者前辈属性的值并在$scope上定义一个属性
 ```
 <panel sty="warning" title="A1">1234</panel>

 <div class="panel-body" ng-transclude></div>
 <div class="panel panel-{{name}}">
 <button ng-click="say({abc:title,b:'hello'})">点击</button>
 
 app.directive("panel",function () {
        return {
            restrict:"E",
            link:function (scope,element,attrs) {
                scope.name = attrs["sty"];
                scope.title = attrs["title"];
            },
            templateUrl:`tmpl/panel.html`, //ajax请求获取
            scope:true,
             //给当前指令添加一个和 rootscope作用域平级 的作用域
              //{} 不能获取父级作用域得到属性  默认是false
            transclude:true //将内容1234添到文章中
        };
    });
    
 ```
	 + 通过scope中{}来获取当前元素属性的值并定义一个变量名
	```
	<panel sty="warning" title="t" s="say(abc,b)">
	
	app.directive("panel",function () {
	        return {
	            restrict:"E",
	            templateUrl:`tmpl/panel.html`,
	            scope:{
	                name:'@sty', //  @/= 当前属性 需要{{}}
	                title:'@' //名字相同可以去掉后面的
	                title:"=" //取得是变量 不需要{{}}
	                say:'&s'  //获取到函数
	            },
	            transclude:true
	        };
	    });
	```
-  指令交互
```
<girl eat train>beautiful</girl>

 app.directive('girl',function () {
        return {
            restrict:"E",
            controller:function ($scope) {
                 $scope.arr = [];
                this.collect = function (kinds) {
                    $scope.arr.push(kinds);
                }
            },
            link:function (scope,element) {
                element.on("click",function () {
                    alert(scope.arr);
                });
            }
        };
    });
    
app.directive('train',function () {
        return {
            restrict:"A",
            require:"^?girl", 
            //在当前指令上找girl指令，找到则会在link函数的第四个参数上增加girl控制器的实例,找不到则会向上一级寻找，如果找到，其中还必须有contorller函数，没有同样会报错，找不则会返回null
            link:function (scope,element,attrs,gCtrl) { //如果没有
                gCtrl.collect("train");
            }
        };
    });
```
-  $watch
> 作为作用域上的一个属性，可以监控变量（引号包起来）、函数（属性名）
```
<div ng-controller="myCtrl">
    {{product.name}}    <br>
    {{product.price}}   <br>
    {{product.count}}   <br>
    <input type="text" ng-model="product.count">    <br>
    {{sum()+product.free}}  <br>
</div>

app.controller("myCtrl",function ($scope) {
        $scope.product = { name:"电脑",price:20,count:2,free:30};
        $scope.sum = function () {
            return $scope.product.price * $scope.product.count;
        };
        $scope.$watch($scope.sum,function (newVal) {
                $scope.product.free = newVal >100?0:30;
        });
    });
```
-  $apply
> 当angular利用原生或者jq方法操作数据时，数据无法更新到model，需要手动刷新
> apply主要应用于手动刷新数据，服务自带刷新，就不用$apply,否则会报错
> 常用服务: sce interval timeout http 


-  $http服务
> http主要应用于和后台数据的交互的服务
>http服务中方法返回的都是一个promise对象
```
$http.jsonp($sce.trustAsResourceUrl("https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd="+$scope.query),{jsonpCallbackParam:'cb'})
     .then(function (res) {
         $scope.result = res.data.s;
     },function (err) {
         console.log(err);
     });
```

-  服务
> provider factory  service constant value
> 
> provider: 最大
> factory:基于provider  
> service :基于factory  
> value:基于 factory
> 
> provider运行顺序： new Provider > config > $get
> factory 不具有配置能力，因为get方法在我们的config方法后执行 配置后会被覆盖掉

- provider服务
```
app.config(function (StorageProvider) { //在要配置的服务后面 增加Provider后缀

    });
    
 app.provider("Storage",function () {
        this.$get = function () {
            return {
                setStorage:function (key,value) {
                    if(typeof value == "object"){
                        value = JSON.stringify(value);
                    }
                    window.localStorage.setItem(key,value);
                },
                getStorage:function (key) {
                    var value = window.localStorage.getItem(key);
                    if(value.startsWith('[') || value.startsWith('{')){
                        value = JSON.parse(value);
                    }
                    return value;
                }
            };
        }
    });

    app.controller("myCtrl",function ($scope,Storage) {
        Storage.setStorage("name",{name:1});
        Storage.getStorage("name").name;
    });
```
- factory服务
```
app.factory('Calc',function () { //factory是基于provider的 后面的函数就是provider的$get
        var currency = '$';
        return {
            "+" : (a,b)=>currency+(a+b)
        }; //这里返回的就是最后注入到控制的内容
    });
```
- 事件传播
> 自己监听事件：$scope.$on("[ 事件名称 ]",function(e,data){ });
> 向上发送事件：$scope.$emit("[ 事件名称 ]",val|fn|String);
> 向下广播事件：$scope.$broadcast("[ 事件名称 ]",val|fn|String);

- 控制器之间的通信
> $rootScope
> 事件
> 服务
> 事件传播
> 观察者模式