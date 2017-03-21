# git总结

### svn和git的区别
svn是集中式   git是分布式

### 配置用户信息
```bash
git config --global user.name  your name
git config --global user.email your email
git config --list //查看git的配置
```

### 初始化git
```bash
git init
```

### 查看文件状态
```bash
git status
```

### 添加到暂存区
```bash
git add <filename> || .
```
### 把暂存区的文件撤回工作区
```bash
git reset <filename> || .
```
### 撤销工作区的修改
```bash
git checkout -- <filename>
```

### 添加到历史区
```bash
git commit -m'commit info'
```
### 删除历史区的文件
```bash
git rm <filename>
git commit -m'rm ' 重新提交
```

### 对比代码
工作区和暂存区
```bash
git diff
```
暂存区和历史区
```bash
git diff --cached
```
工作区和历史区
```bash
git diff master(分支名)
```

### 日志
查看日志
```bash
git log
```
查看所有日志
```bash
git reflog
```

### 回滚
回到上次第几个版本
```bash
git reset --hard HEAD^   //回退上一个版本
git reset --hard HEAD~10 //回退上一个版本
```
回到指定版本
```bash
git reset --hard 版本号
```

### 分支管理
查看分支
```bash
git branch
```
创建分支
```bash
git branch <branchName>
```
切换分支
```bash
git checkout <branchName>
```
创建并切换分支
```bash
git checkout -b <branchName>
```
删除分支(不能删除自己现在所在的分支)
```bash
git branch -D <branchName>
```
合并分支(合并到当前分支)
```bash
git merge <branchName>
```

### 远程仓库
创建ssh钥
```bash
ssh-keygen -t rsa -C "your email"
```
建立远程关联
```bash
git remote add origin <url>
```
查看远程关联
```bash
git remote -v
```
删除远程关联
```bash
git remote origin
```
推送到远程仓库
```bash
git push origin master -u (upstream 下次提交不必再输入origin master)
```
更新远程到代码到本地
```bash
git pull
```

### 冲突解决
代码冲突的时候git会保留两者 ，此时打开文件删除一项保留一项
