###Git  
git config --list ：查看git配置
git config --global user.name xxx ：配置用户名
git config --global user.email xxx ：配置邮箱 

###初始化git
git init
mkdir xxx（目录名）：创建目录

###ls：显示所有文件 // [ls -al]

### touch xxx.xx 创建一个文件
rm xxx.xx 删除
cat xxx.xx 查看

###vi xxx.xx 编辑 -> i 键 编辑模式  -> esc :wq 保存并退出

###git status ：查看git状态
git add . / git add -A 放入暂存区（过渡区）
    git reset HEAD . 取消暂存
    git checkout xxx.xx 从暂存区拉回工作区
git commit -m'message' 放入版本库（历史区）

git log : 查看git日志

###对比代码
git diff 工作区和暂存区

git diff --cached 暂存区和历史区

git diff master(分支名) 工作区和历史区

###git reset --hard 版本号（7~8位） ：从历史区拉到工作区
  通过查看日志 git log 粘贴版本号

##分支管理
git branch 查看分支

git branch <name> 创建分支

git checkout <name> 切换分支

git checkout -b dev 创建并切换分支 dev（分支名字）  // 相当于克隆当前那一份给新的分支

git branch -D <name> 删除分支，不能自己删除自己

git merge dev 在master上合并dev分支

··············································································

## 创建仓库 
- 创建一个空仓库 new repository
- 本地要建一个README.MD  .gitignore

-提交并关联仓库
git add .
git commit -m'message'
git remote add origin(可改变) xxx(仓库地址)
git remote -v 查看所有关联
git remote rm origin 删除

- 推送到远程仓库上
git push origin master -u

##发布静态页面
- 发布的静态页必须在gh-pages分支












