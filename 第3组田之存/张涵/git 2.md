##  git 2
####Git更新错误解决方法
> 在执行git pull origin master时，出现本地文件已经修改,    提交修改即可
```
	git add .
	git commit -m ""
	esc :wq // 进入文本模式时进行退出
	ESC :!q //强制退出
```
####   使用nrm替换源
```
// 安装nrm，用来代替npm 指令
	npm install nrm -g
// 查看所有镜像源
	nrm ls
// 测试镜像源网络状况
	nrm test
// 添加镜像源
	nrm add xxx http://172.18.1.139
// 删除 镜像源
	nrm del xxx
//  切换 镜像源
	nrm use xxx
// 查看所有全局
	npm root -g
```
####Angular 安装
> 安装在本地，没有安装在开发和生产环境之中，会自动寻找node_modules文件夹,没有就会向上一级寻找，直到根目录中没有找到，则在本地建立
```
	npm init -y
	npm install angular    //把依赖下载到本地文件
	npm install xxx --save //把依赖放入到生产依赖清单（上线需要）
	npm install xxx --save-dev //把依赖放入到开发依赖清单（开发需要）
```
