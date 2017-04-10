#12周1天和2天总结 

## 跨域头
```
res.setHeader('Access-Control-Allow-Origin','*');
res.setHeader('Access-Control-Allow-Methods','GET,DELETE,POST,PUT,OPTIONS');
res.setHeader('Access-Control-Allow-Headers','Content-Type');
//先发送一个 options 请求 ，如果能响应到 ，再去发送真正的请求
if(req.method == 'OPTIONS'){ //方法名必须是大写
    res.end();
}
```
## jsonp
``` 
 var fnName = query.callback; // 'aa'
//2.返回函数执行
res.setHeader('Content-Type','application/javascript;charset=utf8');
res.end(fnName+'('+JSON.stringify(arr)+')');
```

## postMessage(iframe)



####第二天

一、 常用类
1、container
居中的内容展示
2、row  行内容显示
3、col 列内容显示， 列必须在row 中
xs 宽度小于768 ，sm宽度小于990 大于768 ,md 宽度大于990，小于1200， lg宽度大于1200
col-xs-4 代表 小屏下显示为4列，
col-xs-4 col-md-2  代表小屏下占4列，大屏下占2列

4、h1-H6  排版
5、pageHeader 
6、small 小一号的深灰色 ，用作副标题。
7、lead  处理一个段落的文字，让其显示效果显著。

8、文字class
9、list-unstyled  无样式的列表

10、list-inline 横向水平列表 

11、table 基本使用一致。table-hover 某行鼠标变色；table-condensed 表格紧缩，实际padding减半。
9、list-unstyled  无样式的列表

10、list-inline 横向水平列表 

11、table 基本使用一致。table-hover 某行鼠标变色；table-condensed 表格紧缩，实际padding减半。




.table包裹在.table-responsive中即可创建响应式表格，其会在小屏幕设备上（小于768px）水平滚动。当屏幕大于768px宽度时，水平滚动条消失。


12、form 
13、按钮

btn-lg、.btn-sm、.btn-xs   大中小三种按钮尺寸
active  激活状态
14 img 
img src="..." class="img-responsive" alt="Responsive image"
为图片赋予了max-width: 100%;和height: auto;属性，可以让图片按比例缩放，不超过其父元素的尺寸

通过单独或联合使用以下列出的class，可以针对不同屏幕尺寸隐藏或显示页面内容。





打印class
和常规的响应式class一样，使用下面的class可以针对打印机隐藏或显示某些内容。


15  下拉菜单  dropdown

改为右对齐 
class="dropdown-menu text-right" role="menu" aria-labelledby="dropdownMenu1">

16 、  导航  nav
Bootstrap中可用的导航有相似的标记，用基类.nav开头，这是相似的部分。改变修饰类可以改变样式。
