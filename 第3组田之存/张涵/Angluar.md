###   Angluar

####框架和库区别
> 框架：主动调用开发人员的文件
> 库：开发人员调用库的文件和方法
####框架
> 1. MVC : 单向数据绑定，例如：原生js中的事件 
   > model view controller
> 2. MVVM(ViewModel) ：双向数据绑定
   > model view viewmodel 
#### Angular 语法
```
	var app = angular.module("appModule",[]);// 声明一个模块
	app.run(["$rootScope",function($rootScope){	
	//$rootScope根作用域
	}]);
	app.controller("控制器名",["$scope",function($scope){
	//$scope当前作用域
	}]);
	ng-xxx:   //支持赋值、运算、三元
	ng-app    //启动模块名称
	ng-model  //双向绑定数据
	ng-model-options//设置数据的属性：延时。。。	
	ng-bind   //解决单行闪烁问题
	ng-cloak  //解决多行闪烁问题
	ng-repeat //遍历数组或对象
	ng-controller //控制器会产生一个作用域
	ng-click="fn($event)"
```

###   Angluar过滤器
#####内置过滤器
- 货币过滤
```
{{111111|currency}}  /\/默认美元符
{{2222.2222|currency:"¥":3}} /\/设定RMB符，保留小数
```
- 字符限制过滤
```
{{"aaaaaaa"|limitTo:4}}//限制字符个数
```
-  日期过滤
```
{{Date.now()|date:"yyyy-MM-dd hh:mm:ss EEEE"}}
{{Date.now()|date:"yyyy年MM月dd日 hh时mm分ss秒"}}
```
- orderBy过滤
```
stu in students | orderBy:[变量] :[]...
//orderBy过滤器可以将一个数组中的元素进行排序，接收一个参数来指定排序规则，参数可以是一个字符串，表示以该属性名称进行排序。可以是 一个函数，定义排序属性。还可以是一个数组，表示依次按数组中的属性值进行排序（若按第一项比较的值相等，再按第二项比较）
```
- Filter过滤器
filter可以指定查询{字段:查询条件} ，默认是全部
```
 stu in students | orderBy : chinese( 条件) : flag(升降(true)序)...  | filter : { english : query }
``` 
-  数字过滤器
```
{{"1235" | number}}
```
- json 过滤器
``` 
//pre 是预格式文本标签，会保留空格和换行符
<pre>{{ {name:1,age:6} | json}}</pre>
```
- 大小写过滤器
```
{{"abcd" | uppercase}} 
{{"ABCD" | lowercase}}
```

####自定义过滤器
```
{{"hello" | firstAlphaUpper}}
app.filter("firstAlphaUpper",function (){
	return function(data) {
	 return data[0].toUpperCase();
  }；
});
{{"hello world" | firstAlphaUpper}}
app.filter("firstAlphaUpper",function (){
	return function(data) {
		var reg = /^\w|\s\w/;
		dadta = data.replace(reg,function(){
			return arguments[0].toUppercase();
		});
	return data;
  }；
});
```
####  过滤器查询以及解决方法
>缺点：查询导致数据更新太快，导致性能和体验不佳
>解决方法: 
>- 在输入表单中加入延时500ms属性 ng-model-options="{debounce:500}
>- 失去焦点再刷新数据  ng-model-options="{updateOn:'blur'}
####    类数组转换为数组的方法
```
 1. [].prototype.slice.call(likeArray,0);
 2. Array.from(likeArray).slice(0);//ES6  不兼容

function (data,...o){ // 将参数中从索引1-end组成一个数组
	[].prototype.slice.call(arguments,1);
	Array.from(likeArray).slice(1);//ES6  不兼容
	o;//ES6 不兼容
}

```
####    数组中forEach的兼容方法
```
Array.prototype.myForEach = function(callback){
	for(var i = 0,len = arguments.length;i<len;i++){
		callBack.call(window,arguments[i],i);
	}
}
```
#### 数组中的方法
- filter
 ```
	 [1,2,3,5,7].filter(function(item,index){
		 return (item>2); //返回值为布尔值
	 });          //返回一个新数组[3,5,7]
	 
 ```
- forEach：没有返回值，只是简单的遍历
- map
```
	[1,3,5,7,9].map(function(item,index){
		return `<li>${item}</li>`;
	});//返回一个新数组
```
- find
 找到对应的项之后，就停止查找,并返回查到的项。
```
	[1,3,5,7,9].find(function(item,index){
		return item==7;
	});//7
```
####  获取下拉列表 选项中值的方法
>  利用select标签的onchange 事件，通过获取select标签中的value值，即可获取option中的value值
```
<select id="sel"> //默认选中第一个利用checked可以选择默认状态
	<option value="0">北京</option>
	<option value="0">上海</option>
	<option value="0">深圳</option>
</select>

sel.onchange = function(){
	alert(this.value);
};
```
> angular 中提供了 as for in 专用语法
```
1.<select ng-model="sel"> 
		<options value='{{dis.value}}' ng-repeat="dis in discounts track by $index">{{dis.name}}</options>//sel必须为字符串类型
  </select>
2.<select ng-model="sel" ng-options="dis.value as dis.name for dis in discounts"> </select>//不用关注sel的值是字符串类型还是数字类型
```