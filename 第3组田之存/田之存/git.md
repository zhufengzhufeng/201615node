###GIT

- cd e:改变根目录
- ls -l 查看目录下的文件
- ls -al 查看目录下所有文件 显示隐藏的
- git init 初始化
- touch xxx.txt 创建文件，文件，文件
- cat xxx.txt  查看文件内容
- rm xxx.txt 删除文件
- vi xxx.txt进入编辑模式 -> i 进入insert模式 -> esc 退出insert模式 -> :wq 保存并退出
- git status 查看git状态
> 添加到暂存区
- git add .
- git add -A
- git add xxx.txt
> 提交到版本库
- git commit -m 'write hello'   -m（message）的作用写上注释内容
>对比代码
- git diff 对比代码 （工作区和暂存区）
- git diff --cached  （暂存区和版本库）
- git diff 分支名(例：master)     （工作区和版本库）
> 查看日志
- git log 查看日志，不想看q退出
- git reflog 查看所有的 
- git log --grep=     搜索文件

- git reset HEAD .  删除暂存区所有文件
- git reset HEAD xxx.txt 删除暂存区某个文件
- git reset --hard 版本  回滚

- git checkout xxx.index  从暂存区把代码拉到工作区

> 分支管理
- git branch  查看所有分支、
- git branch 分支名字  创建分支
- git checkout 分支名字  进入到分支（切换分支）
- git checkout -b 分支名字   创建分支并且切换分支 相当于将当前内容克隆一份
- git branch -D 分支名字   删除分支 （不能在自己的分支内删除自己）
- git merge 分支名字   （在主干上合并分支）例如“在master上合并dev”

###创建仓库
- 创建空仓库 github中
```
//GitHub中选择
new repository
```
- 本地要建一个README.md  .gitignore
```
echo ".idea" >> .gitignore
echo "welcome" >> README.md
```

- 关联
 ```
 git add .
 git commit -m ""
 git remote add origin https://......  //origin 代表后面地址
 git remote -v 查看所有关联
 git remote  rem origin 删除
 
 ```
 
 -  推送到远程
 ```
 git push origin master(可以推送其他分支) -u
 ```
 
 - 发布静态页
   发布静态页必须在gh-pages分支上
   ```
   git checkout -b gh-pages
   git add .
   git commit -m ''
   git push origin gh-pages
   ```
 


###讲师有一个仓库，组长Fork



老师的
https://github.com/zhufengzhufeng/201615node.git