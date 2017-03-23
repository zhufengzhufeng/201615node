#### angular过滤器补充
> \$sce 服务，\$sce.trustAsHtml('字符串');可以将页面的数据转换成html内容.同时需要配合ng-bind-html来使用将数据编译成html.
> 注意：ng-bind-html 这里只能放字符串类型的.

#### angular 指令
> 指令分为两类 自带指令  自定义指令
> 自定义指令：
> 装饰型指令 赋予标签一些功能  link函数
    组件式 把标签 替换成一个组件 template

指令分为四种格式(E(Element)、A(Attribute)、C(Class)、M(Comment) 。
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

自定义指令
```
//组件式
app.directive('myDrag',function () {
    return {
        replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
        restrict:'EA', //限制使用范围 默认范围是EA,范围的标识必须大写
        template:`<div>
                    <div>Hello</div>
                    <div>world</div>
                  </div>` //模板的根节点只能有一个
    }
});
```
```
//装饰式 (指令默认没有作用域)
app.directive('myBorder',function () {
    return {
        replace:true,
        restrict:'EA',
        //链接函数 链接视图和作用域的
        link:function ($scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
            // 1.代表的是当前指令所在的作用域
            // 2.element代表当前指令所在的标签，是一个jquery对象
            //angular.element == $
            angular.element(document)//$(document)
            // 3.代表当前指令上所有的属性集合
        }
    }
});
```
```
//添加属性drag 当前元素则可以拖拽
app.directive('drag', function ($document) {
    return {
        restrict: 'A',
        link: function (scope, element, attrs)  {
            element.on('mousedown', function (e) {
                var disX = e.pageX - this.offsetLeft;
                var disY = e.pageY - this.offsetTop;
                angular.element(document).on('mousemove', function (e) {
                    element.css({
                        top: e.pageY - disY + 'px',
                        left: e.pageX - disX + 'px'
                    });
                });
                angular.element(document).on('mouseup', function (e) {
                    angular.element(document).off('mousemove');
                    angular.element(document).off('mousedown');
                });
                e.preventDefault();
            });
        }
    }
});
```

#### 组件
组件化开发的好处，复用，管理方便统一管理
```
app.directive('panel',function () {
    return {
        restrict:'E',
        templateUrl:`tmpl/panel.html`,//ajax获取过来的
        link:function (scope,element,attrs) {
            //自己家没有 根作用域，如果自己家有 就是自己家的作用域
            scope.name = attrs.st;
            scope.title = attrs.title;//取不到属性对应的值的变量
        },
        //scope:{}, //给当前指令产生一个作用域 {}  默认值是false
        // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
        scope: {
        //通过属性传递的值 可以在scope:{} 里快速获取到
        //@符引用的是字符串
        //= 取的是变量
        //& 取的是方法
        //名字和key相同，值可以去掉符号后面的。
        name: '@st',
        title: '=article',
        say: '&s'
        },
        transclude:true,//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
    };
});
```

> 补充:
ng-class (动态的添加类样式) 和class不冲突(静态)
语法: {true:'block',false:'none'}[flag] 或者 {block:flag,none:!flag}

> ng-show/ng-hide ng-if
频繁切换用show/hide  操作的是样式
一次就确定下来的内容 如果值为false内部代码不执行 操作的dom, repeat经常和if连用  if会产生一个作用域

#### 指令和指令交互
```
app.directive('girl',function () {
    return {
        restrict:'E',
        controller:function ($scope) { //当前指令所在的作用域,不会产生作用域
        }
    }
});
app.directive('train',function () {
    return {
        restrict:'A',
        //^自己身上找找不到向上找
        require:'^?girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
        link:function (scope,element,attrs,gCtrl) { //如果没有依赖到使用的是? 则得到的值是null，否则报错
        }
    }
});
```

#### $watch
> 脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍
```
//实时监控
$rootScope.$watch(function () {
    return $rootScope.name;
},function (newVal,oldVal) {
    console.log(newVal,oldVal);
});
//放入的是作用域上的一个变量字符串
$rootScope.$watch('name',function (newVal,oldVal) {
    console.log(newVal,oldVal);
})
```

#### $apply
> 当不是用angular自带方法改变了数据模型的时候, 数据更新不会影响视图, 需要调用scope.$apply()方法使视图更新.而$interval, $timeout服务则不用使用$apply.

#### $http
> $http 用法同jquery 返回都是promise对象, 对象中有一个then方法, 方法中有两个参数, 第一个是成功的回调,第二个是失败的回调。
\$sce.trustAsResourceUrl()//作为一个可信任的路径$sce.

#### 服务
> - 服务是单例的, 方法都是公用的
- 实现复用代码 $rootScope, localStorage
- angular提供了五种服务 "provider factory" service constant value.
- provider是最大的服务, 可以进行配置, 其他4种不能配置.
```
//运行顺序 new Provider > config > $get
app.config(function (CalcProvider) { //在要配置的服务后面 增加Provider后缀
//console.log(CalcProvider); //是provider的一个实例
});
app.provider('Calc', function () {
//当我们将Calc放到控制器中,会自动调用$get方法
this.$get = function () {
    return {};
}//provider默认就会被实例化
});
```
>factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
```
app.factory('Calc',function () { //factory是基于provider的 后面的函数就是provider的$get
    var currency = '$';
    return {
    }; //这里返回的就是最后注入到控制的内容
});
```
>最大的是provider factory是基于provider的  service 是基于factory的  value是基于 factory的

#### 控制器间的交互
>控制器间的交互 $rootScope, 服务， 事件(观察者模式) $on  $emit  $broadcast
```
$scope.$broadcast('event', '要传的');//向下传播
$scope.$emit('event', '要传的');//向上传播
//二者都是通过$on()来接受
$scope.$on('event', function(e,data) {
//e为事件对象
//data为传过来的值
//event 要和$broadcast 和 $emit的第一个参数值相同.
});
```