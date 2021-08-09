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

![image-20210809164710648](C:\Users\李大胖\AppData\Roaming\Typora\typora-user-images\image-20210809164710648.png)

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



