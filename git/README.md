### 你要知道这些

- 分布存储系统-通常假设你在一个分布存储系统中工作，那真个存储都在你本地的开发框内。Git就是一种分布版本控制系统。如果你需要从存储中查询数据，你可以拉出数据。如果你需要添加或者检查文本到存储中，你可以推进数据。强烈建议，即使不需要用到SSH去和存储交互如果它在一个远程服务其中。
- 存储-存储是个普遍术语。版本控制系统管理一个存储，在Git中存储实际上就是一个数据库，在那里文本和历史都得到维护和保存。
- 工作集-工作集包括文件和文件夹。通常你会看到一个存储包含多个工作集，这些工作集组织在通常称作项目中。当你编辑一个工作集时，你顺带地就会升级存储。版本控制最重要的事情就是，当你升级你的存储时，你自动地创建了一个源代码支持，不仅是保存。关于工作集有一点需要记住的是，文件包含潜在的变化已不在存储中了。
- 添加和检入-正如你猜测的那样，添加是一个概念，即你添加新的文件或者提交和查找存在的文件。
- 返回/回滚-如果你需要返回到早前的文件版本，你将会返回或者回滚在存储中任何版本的工作集。
- 检出-如果你正在检出存储中的一个文件，通常你会将主干带入你当地的工作集中。在一些案例中，检出一个文件的活动或锁住这个文件，以防其他的开发者使用。
- 标签和标记-此概念用来说明一个具体存储的状态。你或许标签或者标记了一个你的Drupal模块版本，你认为可以稳定发布的。
- 分支和分叉-这是一个常见的概念，即创建一个完整的存储拷贝。通常，你会听到一个沙盒代码的术语，它的意思是一个开发者已经分支或者分叉了一个项目存储。
- 合并-如果你分支或者分叉了一个存储，那么你将不可避免地想要把你的源代码推送到存储的主干或者主要分支中。推送分支或者分叉代码返回主干的过程，就是指调用合并。

### 使用 git

#### 安装 git

[Install Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

#### 初始配置

这些是一些全局配置项, 请根据个人情况自行修改.

```shell
git config --global user.name "name"  
git config --global user.email xxx@email.com  
git config --global core.editor vim  
git Config --global push.default simple
```

获取某一命令的帮助信息:

```shell
git help <command>
```  

#### 仓库

创建一个本地空仓库:

```shell
cd mygit
git init
```

克隆一个已存在的仓库, 有多种协议:

```shell
cd mygit
git clone ssh://[user@]host.xz[:port]/path/to/repo.git/  
git clone git://host.xz[:port]/path/to/repo.git/  
git clone http[s]://host.xz[:port]/path/to/repo.git/  
git clone -l -s -n ./repo ./repocopy
```

#### git 工作流  

![git工作流](![](https:/img.pycoder.org/blog/20200202183708.png)

#### 添加与提交

```shell
cd mygit
touch newfile
git add newfile
git commit -m "add newfile"
```  

#### 版本回退

首先,要获取想要回退的版本的 **Commit**:

```shell
git log
commit 3eac1fbbde1af2bd61506d666e071f7824f8d2ec
Author: yotoobo <yotoobo@gmail.com>
Date:   Mon Jan 12 20:12:46 2015 +0800

    update git/README.md
```

只要知道了 **commit**, 那么我们就可以用 git 切换到对应版本：

```shell
git reset --hard 3eac1fbbde1a
```

还有一种方式表示版本,如HEAD表示当前版本,HEAD^1表示上一版本,HEAD^100表示上一百个版本:

```shell
git reset --hard HEAD^1
```  

#### 分支

分支是为了将修改记录的整体流程分叉保存。分叉后的分支不受其他分支的影响，所以在同一个数据库里可以同时进行多个修改。

![branch](https:/img.pycoder.org/blog/20200202190346.png)

```shell
# 创建一个叫 feature_x 的分支并切换到 feature_x
git checkout -b feature_x
# 切换回主分支
git checkout master
# 切换到分支
git checkout feature_x
# 更新分支
git checkout -B feature_x
# 删除分支
git branch -D feature_x
#推送分支到远程仓库
git push origin <branch>
```
  
如果使用 git clone 克隆，Git 会自动为你将此远程仓库命名为 origin，并下载其中所有的数据，建立一个指向它的 master 分支的指针，在本地命名为 origin/master，但你无法在本地更改其数据。接着，Git 建立一个属于你自己的本地 master 分支，始于 origin 上 master 分支相同的位置，你可以就此开始工作(基于本地 master 分支工作)

在分支上做开发后，还需要做合并操作:

```shell
# 切换到分支feature_x
git checkout feature_x
# 修改一个文件,add,commit
vim newfile
git add newfile
git commit -m "vim newfile on branch"
# 切换主分支
git checkout master
# 合并分支到当前
git merge feature_x
```
