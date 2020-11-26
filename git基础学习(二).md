#### 工作区和暂存区

 Git和其他版本控制系统如 SVN 的一个不同之处就是有暂存区的概念。 

**工作区**

就是在电脑中可以看见的目录

**版本库 （Repository）**

工作区中有一个隐藏的目录 `.git` ，这个不算工作区，而是 git 的版本库。

 Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的**暂存区**，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。 

![1606293520289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1606293520289.png)

 把文件往Git版本库里添加的时候，是分两步执行的： 

*  用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区； 
*  用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。 

在 创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。 

#### 管理修改

Git管理的是修改（新增、删除了一行，更改了某个字符等），而非文件。

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。 

#### 撤销修改

`git checkout -- file`可以丢弃工作区的修改： 

```shell
$ git checkout -- readme.txt
```

 把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况： 

* `readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态； 
* `readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。 

 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。 

当将错误的修改已经 `add` 到暂存区，但还没有 `commit` 

`git reset HEAD `可以把暂存区的修改撤销掉，重新放回工作区： 

```shell
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD `，就回到了场景1，第二步按场景1操作。

#### 删除文件

添加一个新的 test.txt 文件到 Git 并提交：

```shell
$ git add test.txt

$ git commit -m "add test.txt"
[master b84166e] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

此时，在工作区中删除 test.txt 文件。工作区和版本库就不一致了，`git status` 命令会告诉你哪些文件被删除了。此时有两种选择：

1、从版本库中删除该文件，使用 `git rm` 删掉，并且 `git commit`

```shell
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

此时，文件就从版本库中被删除

2、误删，想要恢复  因为版本库中还存在工作区中误删的文件

```shell
$ git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。 

#### 远程仓库

Git 是分布式版本控制系统，同一个 Git 仓库可以分布到不同的机器上。

本地 Git 仓库和 github 仓库之间的传输是通过 SSH 加密的，需要以下设置：

1、 创建 `SSH Key`。 

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key： 

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

设置成功后在用户主目录里可以找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。 

2、 登陆GitHub，打开“Account settings”，“SSH Keys”页面： 

点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容 。 点“Add Key”，你就应该看到已经添加的Key 。

**为什么GitHub需要SSH Key**

因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。 GitHub也允许你添加多个Key ，需要把每台电脑的 Key 都添加到 github。

#### 添加远程仓库

在本地创建一个本地仓库后，继续在 github 创建一个 Git 仓库，并且让两个仓库进行远程同步。这样，github 上的仓库可以作为备份，又可以让其他人通过仓库来协作。

1、登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库（learngit）。

*新建的仓库是空的，我们可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到 github 仓库。*

2、在本地的 `leargit` 仓库下运行命令：

```shell
$ git remote add origin git@github.com:michaelliao/learngit.git
```

上面的 `michaelliao` 就是你的 github 账户名，添加成功后，远程库的名字就是 `orgin` ，也可以自行修改。

3、将本地库的所有内容推送到远程库上：

```shell
$ git push -u origin master
```

把本地库的内容推送到远程，用 `git push` 命令，实际上就是把当前分支 `master` 推送到远程。

因为远程库是空的，所以第一次推送的时候，加上了 `-u` 参数，此时，Git 不但会把本地的 `master` 分支内容推送到远程新的 `master` 分支，还会把本地的 `master` 分支和远程的 `master` 分支关联起来，在以后的推送和拉取时就可以简化命令。

从现在开始，只要本地做了提交，就可以通过命令：

```shell
$ git push origin master
```

把本地的 `master` 分支的最新修改推送到 github。

**SSH 警告**

当第一次使用 Git 的 `clone` 或 `push` 命令连接 github 时，会有一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。 

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了： 

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。 

#### 从远程库克隆

创建远程库，然后，从远程库克隆

1、在 github 中创建一个新的库（gitskills）

2、用命令 `git clone` 克隆一个本地库：

```shell
$ git clone git@github.com:michaelliao/gitskills.git
```

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。 

Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。