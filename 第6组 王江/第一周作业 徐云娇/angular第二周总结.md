##angular  第二周 总结笔记


```
<!--bootstrap栅格化布局 一共 12 列-->
<div class="container">
    <div class="row">
        <div class="col-md-12">
        
        </div>
    </div>
</div>
```
```
<!--ng-bind-html 这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html)-->
```
```
return input;//不需要过滤时将原值返回即可
```
```
return $sce.trustAsHtml(input.toString()); //将页面的数据转换成了html内容
```
```
<!--指令分为四种格式-->
<!--Element E-->
<my-drag></my-drag>
<!--Attributes A-->
<div my-drag></div>
<!--Class C-->
<div class="my-drag"></div>
<!--Comment M 前后必须用空格，不建议使用-->
<!-- directive:myDrag -->
```

```
<!--指令分为两类 自带指令  自定义指令-->
<!--自定义指令
    装饰型指令 赋予标签一些功能  link函数
    组件式 把标签 替换成一个组件 template
```
```
//param1 指令的名字
    app.directive('myDrag',function () {
        return {
            replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
            restrict:'EA', //限制使用范围 默认范围是EA,范围的标识必须大写
            template:`<div>
                        <div>Hello</div>
                        <div>zfpx</div>
                      </div>` //模板的根节点只能有一个
        }
    });
```

```
//param1 指令的名字
    //指令默认没有作用域
    app.directive('myBorder',function () {
        return {
            replace:true,
            restrict:'EA',
            //链接函数 链接视图和作用域的
            link:function ($scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
                // 1.代表的是当前指令所在的作用域
                // 2.element代表当前指令所在的标签，是一个jquery对象
                element.css('outline','5px dashed yellowgreen');
                //angular.element == $
                angular.element(document).on('click',function () {
                    alert('click')
                });
                // 3.代表当前指令上所有的属性集合
                console.log(attrs);
            }
        }
    });


```

```
<!--angular会判断jquery是否存在，存在的话不加载自己的js-->
```
```
组件化开发的好处，复用，管理方便统一管理
```
```
 app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,//ajax获取过来的
            link:function (scope,element,attrs) {
                //自己家没有 根作用域，如果自己家有 就是自己家的作用域
                scope.name = attrs.st;
                scope.title = attrs.title;
            },
            scope:{}, //给当前指令产生一个作用域 {}  默认值是false
            // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
            transclude:true,//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
        };
    });
```

```
app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,
            /*link:function (scope,element,attrs) {
                scope.name = attrs.st;
                scope.title = attrs.title; //取不到属性对应的值的变量
            },*/
            scope:{ //@符引用的是字符串
                name:'@st',
                //title:'@' //名字相同可以去掉后面的
                // = 取的是变量
                title:'=article' //scope.title = $scope.t
            }, //如果通过属性传递的值 可以再scope:{} 里快速获取到
            transclude:true,
        };
    });
```
```
 app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,
            //无法取到父级的内容，只能通过属性传递
            scope:{
                name:'@st',
                title:'=article',
                say:'&s' //取的是方法，可以直接用&符号取到
            },
            transclude:true,
        };
    });

```
```
<!--控制div的显示和隐藏，学指令和指令之间的交互-->

<!--ng-class（动态）和class不冲突（静态）
{true:'block',false:'none'}[flag]
{block:flag,none:!flag}

    ng-show/ng-hide ng-if
    频繁切换用show/hide  操作的是样式
    一次就确定下来的内容 如果值为false内部代码不执行 操作的dom, repeat经常和if连用  if会产生一个作用域

```

```
 // 脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍
    // vue 自带的defineProperty set get + 观察者
```
```
 /*app.controller('myCtrl',function ($scope,$interval,$timeout,$sce) { //服务
        var timer = $interval(function () { //不是angular自带的方法，数据更新不会影响视图
            //通知视图刷新
            $scope.d = Date.now();
            //$scope.$apply(); //手动刷新视图
        },1000);
        $interval.cancel(timer); //取消定时器
```

```
app.directive('zfModel',function () {
        return {
            restrict:'A',
            link:function (scope,element,attrs) {
                //监控作用域上值得变化 ，将值赋予给输入框
                scope.$watch(attrs["zfModel"],function (newVal) {
                    element.val(newVal); //将新值赋予给输入框
                });
                //监控输入框输入的内容,将监控的内容 绑定到作用域上 scope[attrs["zfModel"]] = 1111
                element.on('input',function () { //手动apply
                    scope[attrs["zfModel"]] = element.val(); //改变数据 不会通知视图刷新
                    scope.$apply();
                });
            }
        }
    });// $interval $timeout $sce $http.jsonp  自定义服务
```
```
  $scope.search = function () { //输入时触发
            //作为一个可信任的路径$sce.trustAsResourceUrl()
            //百度jsonp接口 callback名字得是cb
            //$http 返回都是promise对象 对象中有一个then方法，方法中有两个参数，第一个是成功的回调第二个是失败的回掉
            $http.jsonp(
                    $sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query)
                    ,{jsonpCallbackParam: 'cb'}
            ).then(function (res) { //成功
                $scope.arrs = res.data.s;
            },function (err) { //失败
                console.log(err);
            });
        }
    });
```
```
 //1.服务是单例的 ，方法都是公用的
    var app = angular.module('appModule',[]);
    //2.实现复用代码 $rootScope, localStorage
    //3.angular提供了五种服务  "provider factory" service constant value
    //4.provider是最大的服务，可以进行配置,其他4中不能配置
    app.config(function (CalcProvider) { //在要配置的服务后面 增加Provider后缀
        //console.log(CalcProvider); //是provider的一个实例
        CalcProvider.currency = '£'
    });
    //运行顺序 new Provider > config > $get
    app.provider('Calc',function () { //当我们将Storage放到控制器中，会自动调用$get方法
        this.currency = '$'; //在实例上增加了属性
        this.$get = function () {
            //var that = this; //谁调用的$get(实例)
            return {
                sum: (a,b) => this.currency +(a+b)
            }
        };
    });//provider默认就会被实例化
    app.controller('oneCtrl',function ($scope,$http,Calc) {  //sum()
        console.log(Calc.sum(1,2));
    });
    app.controller('twoCtrl',function ($scope,$http) {
    });
    //$http $sce
```
```
//1.执行的一瞬间 可以叫闭包
//2.函数执行后 返回引用数据类型，并且这个引用类型被外变量接收,那么这个函数就不会销毁 称之为闭包
//3.箭头函数中没有this 这个this就是上一级的函数中的this
```