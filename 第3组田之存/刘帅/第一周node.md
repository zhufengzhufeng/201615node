## git
### 简谈git安装
#### 安装brew在mac上 mac http://brew.sh
```
brew install git 
```
+ 设置git ,告诉git你是谁
```
git config --global user.name xxx
 git config --global user.email xxx
 git config --list
```
#### 初始化git
```
git init 
git init -y
```
+ 创建并进入目录
```
mkdir gittest 
cd gittest
```
+ ls显示所有文件
```
ls -al 显示隐藏的文件
```
+ 创建文件
```
touch index.txt
cat index.txt 查看内容
```
- 查看git 状态
```
git status
```
- 添加到暂存区
```
git add .  或者 git add -A
```
- 提交到历史库
```
git commit -m 'name'
```
####对比代码
- 工作区和暂存区
```
git diff
```
- 暂存区和历史去
```
git diff --cached
```
- 工作区和历史区
```
git diff master(分之名)
```

## 查看日志
```
git log 过多的话可以使用上下键查看，不想看的可以q退出
```
- 查看说有日志
```
git reflog
```
- 回滚
```
git reset --hard 版本号
```
- vi 编辑
```
vi index.txt
i 编辑模式
esc + :wq 保存并退出  强制退出q!
```

####分支管理
- 查看分支
```
git branch
```
- 创建分支
```
git branch <branchName>
```
- 切换分支
```
git checkout <branchName>
```
- 删除分支（自己不能删除自己）
```
git branch -D <branchName>
```

- 创建并切换（相当于将当前内容克隆一份）
```
git checkout -b <branchName>
```
- 在master上合并dev分支
```
git marge dev
```
- 产生冲突（手动解决提交新的版本）
 
 ### 创建仓库
- 创建仓库，写一个仓库名
```
new  repository
``` 
- 本地要创建一个README.md  .gittignore
```
echo '.idea' >> .gitignore
echo 'welcome' >> README.md
```
- 提交并关联仓库
```
git add .  或者 git add -A
git commit -m ''
git remote add origin 地址
git remote -v 查看所有的关联
git remote rm origin 删除
```
- 推送到远程仓库上
```
git push origin master (可以推送其他分支) -u （upstream 下次提交不必再输入origin master）
```

### 发布静态页
- 发布的静态页必须在gh-pages 分支
```
git checkout -b gh-pages
git add . 或者 gitadd-A
git commit -m
git push origin gh-pages
```
- 讲师有一个仓库
   + 组长fork讲师的仓库
   + 将信息放到自己的文件夹下提交代码
   + 推送到GitHub上
   + 发送pull request 请求合并
   
- 组长给组员开通权限
   + 组员clone最新代码
   + 放个如自己的文件提交
   + 拉取组长的仓库的最新代码
   ```
   git pull
   ```
   + 再次推送
- 组长推送给讲师
   + 先拉取自己组的最新代码
    ```
   git pull
   ```
 + 拉取老师最新的代码
 ```
 git remote add tacher htts://github.com/zhunfengzhunfeng/201615node.git
 ```
 + 推送到自己的仓库上
```
git push origin master
```
+ pull request



##node
###单线程和多线程
- node主线程是单线程的，进程中包含线程，正常Java一个进程中包含多个线程，node中一个进程只能包含一个线程，允许开子进程。

### 同步和异步
- 代码从上到下执行，先走同步在走异步，异步不会阻塞主线程
- 例如：五个人一块吃饭

### 阻塞和非阻塞
- 针对内核来说的，非阻塞是异步的前置条件

### 回调（回头在 调）
- 用回调来解决异步编程问题

### 事件环

###异步的文件读写，callback，定时器，能用异步不用同步

### node全局对象
- 在任意地点可以直接访问的
- 在global上挂载的都是全局对象


##module
###js中模块
- (seajs cmd,requirejs amd,node commonjs)
- cmd 就近依赖，amd依赖前置
- 单例（不能保证完全解决冲突，调用时调用名字过程）
- 闭包，node中实现模块化 采用的是读写

### commonjs(提高了可维护性，有利于分工协作，高内聚低耦合)
- 如何定义模块
创建一个js文件，每个文件就是一个模块，多个模块可以组成一个包
- 如何导出一个模块
 exports/module.exports
- 如何使用一个模块
 require


## npm工具 （npm node-package-manager）
### 全局下载（在命令下使用） -g
### 安装nrm源切换工具
```
npm i nrm -g  或者 npm install nrm -g
```
### 增加珠峰源
```
nrm ls
nrm add zf http://172.18.1.139
nrm use zf
```

###安装全局
https://github.com/ksky521/nodePPT
```
npm install nodeppt -g
```
#### http-server
```
npm install http-server -g
```
### 本地安装（在我们代码里使用的）
#### 初始化依赖文件（packge.json）
在指定目录下生成
```
npm init -y
```
#### 1、开发依赖 gulp
```
npm install gulp (--save--dev)或者（-D）
````
#### 2、项目依赖 jquery
查看版本
```
npm info jquery
npm install jquery@2.2.4(--save) 或者（-S）
```
#### 3、发布自己的包
- package.json
   + name 不能喝自己发布的包重名
   + main 里对应的主文件写一个

- 发布yap切换到npm
- 添加用户 有的话可以登录
```
npm addUser
```
- 发布
```
npm publish 
```





     
    