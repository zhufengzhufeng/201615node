##  angular 第二周笔记
### bootstrap 栅格化布局  
    Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。它包含了易于使用的预定义类，还有强大的mixin 用于生成更具语义的布局。

```
<div class="container">
    <div class="row">
        <div class="col-md-12">
            、、、中间放内容
            <input type="text" ng-model="query">
            <table class="table table-bordered">
                <tr>
                    <th>名字</th>
                    <th>英语</th>
                    <th>语文</th>
                    <th>数学</th>
                </tr>
                <tr ng-repeat="score in scores track by $index">
                    <!--130=><font color="red">13</font>0-->
                    <td>{{score.name}}</td>
                    <td ng-bind-html="score.english | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.chinese | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.math | tableRed:query | asHtml"></td>
                </tr>
            </table>
        </div>
    </div>
</div>	

ng-bind-html 这里只能放字符串类型的，将数据html编译成$sce.trustAsHtml(html)
```

### 指令
- 自带指令
 + Element  E   <my-drag>标签</my-drag>
 + Attributes  A   <div my-drag>属性</div>
 + Class  C  <div class="my-drag"></div>
 + Comment  M   <!-- directive:myDrag -->

```
<script>
    var app = angular.module('appModule',[]);
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
</script>
``` 

- 自定义指令
  + 装饰型指令：赋予标签一些功能  link函数
  + 组件式  把标签替换成一个组件  template
	   +  link
```
<script>
    var app = angular.module('appModule',[]);
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
</script>
```
### 简单指令
- restrict  (E A C M) 默认值为 EA, 即可以通过元素名和属性名来调用指令。  你可以限制你的指令只能通过特定的方式来调用。
- templateUrl :`文件夹/文件名.后缀名`  无法取到父级的内容，只能通过属性传递  ajax获取过来的；
- link：function(){
     自己家没有 根作用域，如果自己家有 就是自己家的作用域
      }
- scope:{}   //给当前指令产生一个作用域 {}  默认值是false   
     {}不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
- transclude:true; 会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
```
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope) {
        $scope.t = '文章1';
        $scope.say = function (title,c) {
            alert(title);
            alert(c)
        }
    });
    app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,
            //无法取到父级的内容，只能通过属性传递  ajax获取过来的
            link:function (scope,element,attrs) {
                //自己家没有 根作用域，如果自己家有 就是自己家的作用域
                scope.name = attrs.st;
                scope.title = attrs.title;
            },
            scope:{
                name:'@st',
                title:'=article',
                say:'&s' //取的是方法，可以直接用&符号取到
            },
            transclude:true,
        };
    });
</script>
```
scope:{  //@符引用的是字符串，接到的都是属性
    name:'@属性名' 
    title：'@'  //名字相同可以去掉后面的
    = 取得是变量
    title:'=article' //scope.title = $scope.t
    say:'&方法名' //取的是方法，可以直接用&符号取到

}；//如果通过属性传递的值 可以再scope:{} 里快速获取到


### 事件
- scope.$watch() 监控作用域上值的变化，将值赋予给输入框
- scope.$on() 监控输入框输入的内容，将监控的内容绑定到作用域上
- scope.$broadcast()  
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
    });

```

### 服务
- 1.服务是单例的 ，方法都是公用的
- 2、实现复用代码 $rootScope, localStorage
- 3、 angular提供了五种服务  "provider factory" service constant value
    + factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
    +  factory是基于provider的 后面的函数就是provider的$get
- 4、provider是最大的服务，可以进行配置,其他4中不能配置
  + 在要配置的服务后面 增加Provider后缀
  + 运行顺序 new Provider > config > $get
  + /当我们将Storage放到控制器中，会自动调用$get方法
  + 最大的是provider factory是基于provider的  service 是基于factory的  value是基于 factory的
  + 控制器间的交互 1.$rootScope, 服务， 事件(观察者模式) $on  $emit  $broadcast