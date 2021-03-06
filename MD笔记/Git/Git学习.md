# Git学习笔记

## Git安装

git安装完成后，需要设置用户名和邮箱

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 创建版本库

在需要git管理的目录下输入

```bash
git init
```

就把git仓库建好了

把一个文件放到git仓库只需要两步

第一步，用命令`git add`告诉Git，把文件添加到仓库

```bash
git add filename
```

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```bash
git commit -m "xxx"
```

`-m`后面输入的是本次提交的说明，可以输入任意内容

commit一次可以提交多个文件，所以之前可以使用多个add文件

## 工作区状态与文件修改

```bash
git status
```

掌握工作区状态

```bash
git diff
```

如果有文件被修改过，可以查看修改内容

## 版本回退

`HEAD`指向的就是当前版本，因此，Git允许回退，使用命令

```bash
git reset --hard commit-id
```

上一个版本除了使用SHA-1的计算的id外，还可以使用`HEAD^`，也就是上一个版本，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

穿梭前，可以使用以下命令查看提交历史

```bash
git log
```

要重返未来，可以使用以下命令查看命令历史

```bash
git reflog
```

## 工作区和暂存区

### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`xxx`文件夹就是一个工作区：

### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的**版本库**。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![image](https://user-images.githubusercontent.com/88662273/128891784-f044410f-d0a9-4a17-a122-673fa85ec895.png)

把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

> 注意：`git commit`只是把暂存区的修改提交了，如果没有将后续修改`git add`到暂存区，则提交时不会提交后面的修改

## 撤销修改

1. 没有`git add`时，用`git restore filename`
2. 已经`git add`时，先`git restore --staged filename`
3. 已经`git commit`时，用`git reset`回退版本

## 删除文件

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 远程仓库

1. windows下打开Git bash，创建SSH Key：

```bash
ssh-keygen -t rsa -C "youremail@example.com"
```

一路回车，可以在用户主目录找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，`id_rsa`是私钥，`id_rsa.pub`是公钥。

2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：
3. 点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

### 添加远程库

1. 登录Github，然后右上角点击"+"找到"new repository"，创建一个新的仓库，在`epository name`填入自己的仓库名称，然后点击"Create repository"就成功创建了一个新的Git仓库

2. 根据Github的提示，在本地仓库下运行命令

   ```bash
   git remote add origin git@github.com:yourname/xxx.git
   ```

3. 添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

4. 下一步，就可以把本地库的所有内容推送到远程库

   ```bash
   git push -u origin master
   ```

   把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

   由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

   从现在起，只要本地作了提交，就可以通过命令：

   ```bash
   git push origin master
   ```

### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

然后，根据名字删除，比如删除`origin`：

```bash
git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

### 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

## 分支管理

### 创建与合并分支

创建分支

```bash
git branch <branchname>
```

查看本地分支

```bash
git branch
```

查看所有分支，包括本地和在线分支

```bash
git branch -a
```

切换分支

```bash
git switch <branchname>
或者
git checkout <branchname>
```

新建分支后切换分支可以用一个语句

```bash
git checkout -b <branchname>
# git checkout 命令加上 -b 参数表示创建并切换
```

合并某分支到当前分支

```bash
git merge <branchname>
```

删除分支

```bash
git branch -d <branchname>
```

### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

### bug分支

将工作现场“储藏”下来，相当于快照

```bash
git stash
```

查看暂存的工作空间

```bash
git stash list
```

恢复工作现场，恢复有两种办法

1. 一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

2. 另一种方式是用git stash pop，恢复的同时把stash内容也删了。

复制一个特定的提交到当前分支

```bash
git cherry-pick <commitid>
```

### feature分支

开发一个新feature，最好新建一个分支；

丢弃一个没有被合并的分支，可以强行删除

```bash
git branch -D <name>
```

## 多人协作

查看远程库的信息

```bash
git remote
```

或者，显示更详细的信息

```bash
git remote -v
```

### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```bash
git push origin <branchname>
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- master分支是主分支，因此要时刻与远程同步；

- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

在本地创建和远程分支对应的分支

```bash
git checkout -b branch-name origin/branch-name
```

建立本地分支和远程分支的关联

```bash
git branch --set-upstream branch-name origin/branch-name
```

### rebase

rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 标签管理

### 创建标签

切换到需要打标签的分支，然后输入

```bash
git tag <name>
```

查看所有标签

```bash
git tag
```

给历史提交打标签

```bash
git tag <name> <commit>
```

查看某个标签信息

```
git show <tagname>
```

### 操作标签

删除标签

```bash
git tag -d <tag-name>
```

推送某个标签到远程

```bash
git push origin <tagname>
```

一次性推送全部尚未推送到远程的本地标签

```bash
git push origin --tags
```

删除一个本地标签

```bash
git tag -d <tagname>
```

删除一个远程标签

```bash
git push origin :refs/tags/<tagname>
```

## 自定义Git

让Git显示颜色

```bash
git config --global color.ui true
```

### 忽略特殊文件

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

强制添加文件到Git,使用`-f`

```bash
git add -f App.class
```

检查规则错误，使用`git check-ignore`检查

```bash
git check-ignore -v App.class
```

编写规则排除部分文件

```bash
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class
```

把指定文件排除在.gitignore规则外的写法就是!+文件名，所以，只需把例外文件添加进去即可。

### 配置别名

比如使用`st`表示`status`

```bash
git config --global alias.st status
```

别名后面可以跟多个命令


