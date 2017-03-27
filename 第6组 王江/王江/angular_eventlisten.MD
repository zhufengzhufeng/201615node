AngularJS中自带有一系列与鼠标相关的指令例如“ngClick”，“ngMousedown”，以及“ngMouseleave”来进行DOM互动。不幸的是，所有的这些指令都需要运用到一个HTML元素上。但是，如果你想要在文档节点上监听鼠标事件怎么办？由于文档节点并不通过一个HTML元素来表示，AngularJS中自带的指令似乎就没有什么帮助了。

为了让我的控制器能够对文档层面上的鼠标事件有所反应，我必须创建一个自定义AngularJS指令，它将会把DOM节点和控制器的$scope方法“粘合”起来。在下面的例子中，我创建了“bnDocumentClick”属性指令：

bn-docuemnt-click="handleClick($event)"
这个指令将会在当前控制器的$scope对象的上下文中处理这个给定的表达式。在下面的例子中，$event对象其实就是潜在的jQuery事件对象。

现在，我们来看看例子 – 当用户点击页面时，鼠标的X/Y坐标将会输出到视图中：

<!doctype html>
<html ng-app="Demo">
<head>
    <meta charset="utf-8" />
    <title>Capturing Document-Clicks In AngularJS</title>
</head>
<body
    ng-controller="DemoController"
    bn-document-click="handleClick( $event )">

    <h1>
        Capturing Document-Clicks In AngularJS
    </h1>

    <p>
        Click anywhere to trigger an event.
    </p>

    <p>
        <strong>Click X</strong>: {{ mouseX }}
    </p>

    <p>
        <strong>Click Y</strong>: {{ mouseY }}
    </p>



    <!--
        从CDN中载入jQUery和AngularJS。为了能够使用“完整的”jQuery（与之对应的是不完整的jQLite），我们需要首先载入jQuery
    -->
    <script
        type="text/javascript"
        src="//ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js">
    </script>
    <script
        type="text/javascript"
        src="//ajax.googleapis.com/ajax/libs/angularjs/1.0.2/angular.min.js">
    </script>
    <script type="text/javascript">


        //为我们的应用创建一个module
        var Demo = angular.module( "Demo", [] );


        // -------------------------------------------------- //
        // -------------------------------------------------- //



        //定义document-click指令，当点击事件被触发时，给定的表达式将被处理
        Demo.directive(
            "bnDocumentClick",
            function( $document, $parse ){

                //将Angular的上下文链接到DOM事件
                var linkFunction = function( $scope, $element, $attributes ){


                    //获得表达式
                    var scopeExpression = $attributes.bnDocumentClick;

                                        //使用$parse来编译表达式
                    var invoker = $parse( scopeExpression );


                    //绑定click事件
                    $document.on(
                        "click",
                        function( event ){


                            //当点击事件被触发时，我们需要再次调用AngularJS的上下文。再次，我们使用$apply()来确保$digest()方法在幕后被调用
                            $scope.$apply(
                                function(){


                                    //在scope中调用处理函数，将jQuery时间映射到$event对象上
                                    invoker(
                                        $scope,
                                        {
                                            $event: event
                                        }
                                    );

                                }
                            );

                        }
                    );


                    //当父控制器被从渲染文档中移除时监听"$destory"事件

                };

                                  //返回linking函数
                return( linkFunction );

            }
        );


        // -------------------------------------------------- //
        // -------------------------------------------------- //


        //定义控制器
        Demo.controller(
            "DemoController",
            function( $scope ) {

                //设置初始的X/Y值
                $scope.mouseX = "N/A";
                $scope.mouseY = "N/A";


                //当文档被点击时，它将调用该方法，并为它传递jQuery事件
                $scope.handleClick = function( event ){

                    $scope.mouseX = event.pageX;
                    $scope.mouseY = event.pageY;

                };

            }
        );


        // -------------------------------------------------- //
        // -------------------------------------------------- //


    </script>

</body>
</html>
在这里的例子中，控制器非常的简单。它仅仅使用了$scope的mouseX和mouseY属性来渲染视图。复杂的部分完全位于bnDocumentClick指令中。

“link”函数被用来粘合DOM以及运行控制器的$scope。在这个例子中，link函数绑定了文档的节点并在鼠标被点击时触发给定的处理器。由于jQuery时间被绑定在AngularJS上下文之外，我们需要使用$apply()函数来确保在点击处理器被执行之后，所有的digest循环都会被处理过一遍。

最有趣的部分在于我们使用$parse()方法来创建一个调用函数，它允许我们将jQuery事件传递给$scope中的handleClick()函数。$parse方法将会编译AngularJS表达式：

var invoker = $parse( scopeExpression );
将它编译成为一个函数，这个函数会接受一个给定的上下文以及一系列本地变量：

invoker(context,valueMap);
它有点类似于JavaScript中的apply()函数：

apply(context,arguments);
它也允许我们将潜在的jQUery时间传递给handleClick()函数，这个函数用于帮助我们更新$scope的属性 – mouseX和mouseY。

在本文中，我们主要使用$event和$parse，关于它们的具体信息请参看文档。