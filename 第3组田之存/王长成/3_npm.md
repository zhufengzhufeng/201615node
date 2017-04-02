## npm： node-package-manager
nrm切换到国内源下载
- 全局安装nrm：
```
npm install nrm -g
```
- nrm切换源：
```
nrm use <源名称>
```
- 查看所有源：
```
nrm ls
```
- 测试源的速度：
```
nrm test
```
- 添加珠峰源：
```
nrm add zhufeng http://172.18.1.139
```
- 删除源：
```
nrm del zhufeng
```
- 查看全局安装目录 ：
```
npm root -g
```



### 方便有趣的第三方工具：http-server与nodeppt
nodeppt是使用nodejs写的网络幻灯片，基于GFM的markdown语法编写，可方便的将md文件生成网页ppt，让分项更加轻松有趣~
#### demo演示
- http://qdemo.sinaapp.com/

- http://qdemo.sinaapp.com/?_multiscreen=1  双屏控制（要允许弹窗）

#### 安装与启用
```
npm install nodeppt -g   安装

nodeppt start -h         获取帮助 

nodeppt start -p <port>  绑定端口 
```



#### http-server
它是一个简单的服务部署,可方便的使用局域网测试项目
```
npm install http-server -g

http-server -p 3000 更改端口
```



#### 初始化依赖文件(package.json)
- 在指定目录下初始化
```
npm init -y
```
- 开发依赖 gulp

```
npm install gulp (--save-dev)或者(-D)
```

- 项目依赖 jquery
查看版本
```
npm info jquery
npm install jquery@2.2.4 (--save)或者(-S)
```

#### 发布自己的包
> package.json 里，
> name不能和已发布的包重名，
> main里写一个对应的主文件，
> 发布要切换到npm源  

- 添加用户 有账户话是登录，没有的话是注册
```
npm addUser
```
- 发布
```
npm publish 
```
- 卸载包
```
npm unpublish  --force
```

#### 模块的查找机制

> 为什么-g安装的可以在命令行下使用,npm可以在命令行下使用，所有的全局包 都安装在npm上，会在npm下创建一个脚本文件(快捷方式)，可以映射到真实的文件上，所以通过全局安装的可以直接在命令行下使用

#### bower(管理前端文件的) 
npm(管理node模块的) ->安装的文件放到node_modules下 不能指定安装目录
bower(管理前端文件的) ->可以指定目录下载 只管理前端文件(在git中下载)
```
npm install bower -g             安装

bower init  初始化 bower.json     记录依赖

bower install bootstrap --save   下载bootstrap

.bowerrc
{"directory":"src/public/lic"}   指定目录，默认安装到bower_component下
```


### 强大的博客框架hexo
- 安装
```
npm install hexo-cli -g
```
- hexo初始化
```
hexo init
```

- 启动服务
```
hexo server
```

- 文章放在_posts下（md格式文件）

- 仓库名
```
sheepmiee.coding.me
```

- 发布到git上需要下载一个插件 
```
npm install hexo-deployer-git --save
```

- 配置_config.yml
```
deploy:
  type: git
  repo: https://username:password@git.coding.net/sheepmiee/sheepmiee.coding.me.git
  branch: master
  message: push
```
- 生成
```
hexo g
```
- 发布
```
hexo d
```
- 访问网址
```
http://sheepmiee.coding.me
```
