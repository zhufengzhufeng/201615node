###angular指令Directive
####$sce控制代码安全检查
为什么要要$sce？因为AngularJS里很多地方，比如路径默认是个字符串，不会认为是路径，从而访问不到我们需要的东西，那么我们就可以通过$sce告诉angualr这个路径，这样是很安全的。它有以下几种方法：
```
$sce.trustAs(type,name);
$sce.trustAsUrl(value);
$sce.trustAsHtml(value);
$sce.trustAsResourceUrl(value);
$sce.trustAsJs(value);
```
- ng-bind-html  这里只能放字符串类型的,需要将数据html编译成$sce.trustAsHtml(html)


#### directive指令
>directive(指令)是AngularJ非常强大功能之一。它就相当于为我们写了公共的自定义DOM元素或CLASS属性或ATTR属性，并且还可以在它的基础上来操作scope、绑定事件、更改样式等。通过这个Directive，我们可以封装很多公共指令，比如分页指令、自动补全指令等等。然后在HTML页面里只需要简单的写一行代码就可以实现很多强大的功能。
指令分为两类: 自带指令  自定义指令
自定义指令:
    装饰型指令 赋予标签一些功能  link函数
    组件式指令 把对应标签 替换成一个组件 template

##### directive定义与用法
```    
app.directive('panel',function () {
    return {
        restrict:'EA',   
        replace:false,
        template:`<div><div>Hello</div><div>zfpx</div></div>`,
        templateUrl:`tmpl/panel.html`,   
        link:function (scope,element,attrs) {//element即对应的元素，attrs为该元素上的自定义属性的集合
            //
            scope.name = attrs.st;
            scope.title = attrs.title;
        },
        scope:{}, 
        transclude:true
    }
});
```

##### directive指令详解：
- restrict:（字符串）限制使用范围,取值有：E(元素),A(属性),C(类),M(注释)，默认范围是'EA',范围的标识必须大写
```
<my-drag></my-drag>         <!--Element E-->
<div my-drag></div>         <!--Attributes A-->
<div class="my-drag"></div> <!--Class C-->
<!-- directive:myDrag -->   <!--Comment M 前后必须用空格，不建议使用-->
```
- replace:(布尔值)默认值为false，设置为true时候，将指令对应的标签元素替换掉，要求模板只能有一个根节点
- template：(字符串)，一段HTML文本，用于替换或添加对用元素的模板
- templateUrl：(字符串)，一个代表HTML文件路径的字符串，通过ajax获取过来的
- scope：默认值是false。
>true表示继承父作用域，并创建自己的作用域（子作用域）
{}表示创建一个全新的隔离作用域，不能继承父级作用域
- link:function link(scope, element, attrs, controller) { ... }
>链接函数负责注册DOM事件和更新DOM。它是在模板被克隆之后执行的，它也是大部分指令逻辑代码编写的地方。
  scope - 指令需要监听的作用域,自己家没有作用域 scope为根作用域，如果自己家有 scope就是自己家的作用域
  element - instance element - 指令所在的元素。只有在postLink函数中对元素的子元素进行操作才是安全的，因为那时它们才已经全部链接好。
  attrs - instance attributes - 实例属性，一个标准化的、所有声明在当前元素上的属性列表，这些属性在所有链接函数间是共享的。
  controller - 控制器实例，也就是当前指令通过require请求的指令direct2内部的controller。比如：direct2指令中的controller:function(){this.addStrength = function(){}}，那么，在当前指令的link函数中，你就可以通过controller.addStrength进行调用了。
- transclude:(布尔)
>如果不想让指令元素内部的内容被模板替换，可以设置这个值为true。会产生一个作用域，一般情况下需要和ng-transclude指令一起使用。 比如：template:"<div>hello every <div ng-transclude></div></div>"，这时，指令元素内部的内容会嵌入到ng-transclude这个div中。也就是变成了<div>hello every <div>这是指令内部的内容</div></div>。默认值为false；这个配置选项可以让我们提取包含在指令那个元素里面的内容，再将它放置在指令模板的特定位置。当你开启transclude后，你就可以使用ng-transclude来指明了应该在什么地方放置transcluded内容
- controller
可以是一个字符串或者函数,若是为字符串，则将字符串当做是控制器的名字，来查找注册在应用中的控制器的构造函数,可以注入一些特殊的服务（参数）:
>$scope，与指令元素相关联的作用域
>$element，当前指令对应的 元素
>$attrs，由当前元素的属性组成的对象
>$transclude，嵌入链接函数，实际被执行用来克隆元素和操作DOM的函数

除非是用来定义一些可复用的行为，一般不推荐在这使用。指令的控制器和link函数可以进行互换。区别在于，控制器主要是用来提供可在指令间复用的行为但link链接函数只能在当前内部指令中定义行为，且无法再指令间复用。

#####@、=、&局部 scope 属性
scope值为{}时产生一个隔离作用域，隔离作用域可以通过绑定策略来访问父作用域的属性，directive 在使用隔离 scope 的时候，提供了三种方法同隔离之外的地方交互
- @：绑定到当前元素的属性值到scope作用域中。属性值是一个字符串。
- =：绑定到当前元素的属性值到scope作用域中。属性值是父作用域中的一个变量名。
- &：提供一种方式在父作用域执行一个表达式（函数等）。如果没有指定attr名称，则属性名称为相同的本地名称。