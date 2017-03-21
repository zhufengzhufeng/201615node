###Git

本文不对git具体是什么做过多的赘述，想详细了解，可以去[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)，里面真的是非常详细啊，本文意在引导初次使用并希望了解git基本操作的同学学习；(本环境为mac，与windows下基本一致);

#### git下载/安装
mac下建议采用[homebrew](https://brew.sh/index_zh-cn.html)安装，方便省事也省力；

![获取Homebrew](http://upload-images.jianshu.io/upload_images/4596776-7342599f0369f740.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
直接在终端窗口复制首页第一条命令安装Homebrew，安装完成后，在终端直接运行
```
brew install git
```
安装完成后运行以下命令检测是否安装成功；
```
git version
```
运行完成后有类似`git version 2.10.1` 字样，则说明安装成功，brew默认安装最新版本；
windows下去此网站自行下载：https://git-for-windows.github.io/index.html

### 使用git
####  告诉git你是谁
```
git config --global user.name xxxxx
git config --global user.email xxxxx
git config --list
```
#### 初始化git
指定路径下新建一个版本库（文件夹），建议使用一个新建一个干净的文件夹学习； 可用`mkdir xxx`; 
```
git init
```
执行初始化后，空项目中出现了.git 文件（默认是隐藏的，并且里面的文件不要乱改，改了会影响使用），此时说明初始化成功，这个文件夹此时就变成Git可以管理的仓库了；
![.git](http://upload-images.jianshu.io/upload_images/4596776-0cf4b54b3388b473.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### ls显示所有文件
```
cd xxx  进入指定路径目录
ls -al 显示隐藏的，这个时候你也可以看到.git的文件
```

#### 创建文件，并把文件添加至仓库中
```
touch index.html
cat index.txt 查看内容
```

#### 查看git状态
```
git status 
```

![git status](http://upload-images.jianshu.io/upload_images/4596776-8e4558290081920c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时出现一个红色的index.html 说明是我们新添加（做了修改）的一个文件； 只要有变动的都是红色显示；


git 有三个区域：
- 工作区
- 暂存区
- 版本库

#### 添加到暂存区
```
git add index.html  // 添加指定到暂存区
git add . // 添加所有至暂存区
```

#### 提交到版本库
```
git commit -m'index.html'   //-m'index.html' 可以理解为给此次上传贴了一个标签
```

![提交](http://upload-images.jianshu.io/upload_images/4596776-97417fbee30e6b58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---------
#### 对比代码
以上已经把文件上传至版本库了，但是细心的同学应该记得我们没有往index.html 中添加任何代码；现在我们需要往index中写入一些内容；

![写入内容](http://upload-images.jianshu.io/upload_images/4596776-3cbb85cf28df76b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 工作区和暂存区对比
此时我们已经成功添加了内容，但注意，此时的内容我们只是在工作区进行了修改，暂存区还是之前空的index文件，我们执行以下命令可以对比工作区和暂存区；
```
git diff 
```
发现显示了我们添加的内容：
![对比1](http://upload-images.jianshu.io/upload_images/4596776-ae7c7b08bb09a3d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 暂存区和版本库
没有对比就没有伤害，我们还可以用以下命令对比暂存区和版本库
```
git diff --cached
```
此时你应该什么都没有得到，那么恭喜你没有报错，这是正常的，因为暂存区和版本库现在没有什么不同，都是空的index；

- 工作区和版本库
```
git diff master  //(master为分支名，默认都是这个)
```
因为版本库也是空的，结果也很明显：
![对比2](http://upload-images.jianshu.io/upload_images/4596776-c6e07ad34ab2be23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上比对可以看出我们已经将想要添加的内容写入了工作区，但是这是不够的，我们需要将我们修改的文件上传至git的版本库中，同样需要两步：
![对比三](http://upload-images.jianshu.io/upload_images/4596776-5d58c0e146259bae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
执行后三个区的文件就相等了，没有任何区别。

---------


#### 查看日志 
可以通过以下命令查看版本库中现在存在几个版本了，这个版本可以理解为游戏中的存档；
```
git log  //过多可以使用上下键查看，不想看可以q退出
```
![版本库log](http://upload-images.jianshu.io/upload_images/4596776-95e1ab92eb52247f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最上面是最近一次修改（看日期理解）；
此时index.html 为游戏刚开始的第一个存档；
'add hello world' 是游戏第二关的存档；

#### 查看所有日志
```
git reflog
```
#### 回滚
这里假设我们在打游戏，第一关怪太厉害，好不容易打过去，技能加错了，第二关各种被虐，怎么办呢，想回起点重新开始；此时我们就需要通过刚才`git log` 查到的版本库日志取得存档号（版本号），黄色的就是；我靠，这么长一堆，放心，仅需复制前七位即可，多了也行；
```
git reset --hard 版本号
```
![回到过去](http://upload-images.jianshu.io/upload_images/4596776-a272f192994453f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行完，告诉了你'now at xxxxx'，便回到了你想回到的地方，为了验证，我们现在进到我们的项目中，看一眼index.html，哇，是不是好神奇，什么都没了，恭喜你，可以重新开始打怪了；

------

#### vi编辑
还要用编辑器打开写hello world 太累了，能不能直接在命令行里面写呢，可以，就是有这样的大神，在终端中敲代码（反正我不会）；
```
vi index.html
i 编辑模式
esc + :wq 保存并退出  强制退出q!
```
![vi控制台编辑文本](http://upload-images.jianshu.io/upload_images/4596776-11bb2be3cbdca8eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编辑完成后可以复习下上面学过的命令
```
上传版本库前对比：
git diff  
git diff --cached   //两个 -
git diff master 
想想会是什么结果？
提示：此时仅仅在工作区修改了文本，还没有上传，工作区，暂存区和版本库....

上传：
git add index.html
git commit -m"add hello"   // 再说明，-m是个标签，写什么都可以，只是方便我们知道每次都做了什么修改，必须要写；

上传版本库后对比：
git diff  
git diff --cached   //两个 -
git diff master 

最后查看版本库：
一共有多少版本呢？
git log   // git log 只显示回到过去后的版本，中间的过程不显示
git reflog  // git reflog 显示过去所有操作过的版本；
```
![版本号](http://upload-images.jianshu.io/upload_images/4596776-ac3ebe8d83fdec6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-----
#### github
到现在为止，你在本地再也不用担心文件备份或者丢失的问题了。
但是真实工作中一个项目是多人协作的一个过程，每个人负责的区域不同就会造成几乎每个人都会有一个版本，而最终我们定下来的肯定是只有一个版本；
- 实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

当然搭建一个服务器的代价太大了，此时就出现了一个叫**[GitHub](https://github.com/)**的神奇网站；这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。


本地仓库有了，现在在github上也需要新建一个仓库（因为我已经建好了，所以有红色警告）；
![新建仓库](http://upload-images.jianshu.io/upload_images/4596776-3c393566c30c9a00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

建好仓库如下：
![](http://upload-images.jianshu.io/upload_images/4596776-4928196195d283d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**https://github.com/a307420929/git_du.git** 就是你的仓库地址；

现在回到你的本地仓库下在控制台中执行：
```
git remote add origin https://github.com/a307420929/git_du.git
//origin 后写入你自己的地址；
```
上传本地仓库文件至GitHub
```
git push origin master
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-d0f04d4bbf24967f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
刷新Github上的仓库，出现本地仓库中新建的html文件，说明上传成功~！
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-a8a55dc3950364ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在，你就拥有了真正的分布式版本库！
分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！


从远程库克隆
第一步在新建一个仓库；
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-3f9b5ffb02bc01a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第四步勾选的意思是在GitHub上创建的仓库中自带一个 README.md文件，此时这里就是一个只有在GitHub上有，本地仓库没有的文件，现在需求想把此仓库拉取到本地；

点击右边的Clone or download，将地址复制；
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-96cfb76ae362bd40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在控制台输入
```
git clone https://github.com/a307420929/git_clone.git
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-88e8906dd50b210a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时进入到git_du目录下：
会发现已经成功拉取到本地了
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4596776-8aff0fd36d13e96b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
--------

--------

#### 分支管理
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
- 删除分支（不能自己删除自己）
```

git branch -D <branchName>
```
- 创建并切换(相当于将当前内容克隆一份)
```
git checkout -b <branchName>
```
- 在master上合并dev分支
```
git merge dev
```
- 产生冲突(手动解决提交新的版本)

## 创建仓库
- 创建空仓库，写一个仓库名
```
new repository
```

- 本地要建一个README.md .gitignore
```
echo '.idea' >> .gitignore
echo 'welcome' >> README.md
```

- 提交并关联仓库
```
git add .
git commit -m ''
git remote add origin 地址
git remote -v 查看所有关联
git remote rm origin 删除
```
- 推送到远程仓库上
```
git push origin master(可以推送其他分支) -u(upstream 下次提交不必再输入origin master)
```

## 发布静态页
- 发布的静态页必须在gh-pages分支
```
git checkout -b gh-pages
git add .
git commit -m 
git push origin gh-pages
```