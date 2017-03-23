### github
安装brew在mac上
mac http://brew.sh
```
brew install git 
```
### 告诉git你是谁
```
git config --global user.name xxxxx
git config --global user.email xxxxx
git config --list
```
### 创建版本库
```
git clone <url>          克隆远程版本库

git init                 初始化
```
### 文件操作
```
mkdir gitTest  	          创建并进入文件夹

ls                        显示所有文件不含隐藏）

ls -al                    显示所有文件 （包括隐藏）

touch index.txt           创建文件

cat index.txt             查看内容
```


### 修改与提交
```
git status                查看git状态

git add .                 添加所有到暂存区

git commit -m 'message'   提交到历史库
```


### 对比代码
```
git diff  工作区和暂存区

git diff --cached  暂存区和历史区 

git diff <branch>  工作区和历史区 
```

### 查看日志
```
git log              可用上下键查看，q键退出

git reflog           查看所有日志
```
### 回滚
```
git checkout --<file>         暂存区中file来覆盖工作区中的file

git reset --hard 版本号        该版本号版本库内容覆盖工作区与暂存区
```

## 分支
```
git branch                     查看所有分支

git branch <branchName>        创建分支

git checkout <branchName>      切换分支

git checkout -b <branchName>   创建并切换到分支（基于当前分支克隆一个分支）

git branch -d <branchName>     删除分支（不能在当前分支上删除当前分支）

git merge <branchName>         合并分支 产生冲突只能手动解决再重新提交修改

git log --graph                图表显示分支过程
```
### 远程操作
```
git romote -v                     查看远程版本内裤信息

git remote add <romote> <branch>  添加远程版本库

git remote rm <romote>            删除远程版本库

git pull <romote> <branch>        下载代码并快速合并

git push <romote> <branch>        上传代码并快速合并

```
### 初始化本地仓库并推送到远程版本库
> 第一次提交本地代码资源到远程（github、conding等）时，需要在远程先创建好仓库（repository），在本地建好README.md文件（不要在远程建这个初始化文件），同时也可建一个上传忽略名单的文件.gitingore
```
git init
git add .
git commit -m "message"
git remote add origin <address>
git push origin master
```

### 发布静态页
> 现在github建立一个仓库，github上静态页分支名为gh-pages（以前用master分支不能生成静态页，现在可以了），本地建立gh-pages分支，推送到此仓库，在进入github该仓库页面，点击settings，找到github Pages一栏，source中选择分支，点击save即可生成。在coding中，将代码上传到远程仓库后，在项目页点击‘代码’、点击‘pages服务’，部署来源分支即可生成
```
git checkout -b gh-pages   建立并切换到gh-pages分支
git add .
git commit -m ''
git push origin gh-pages
```




