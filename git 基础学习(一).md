#### Git简介

分布式版本控制系统，用 C 语言编写

**创建版本库** 

版本库又名仓库（repository），可以简单的理解为一个目录， 这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

1、创建空目录 

```shell
$ mkdir learngit
$ cd learngit
$ chdir
/Users/michael/learngit
chdir命令用于显示当前目录，在window 中要确保目录名（包括父目录）不包含中文。
```

2、通过 `git init`命令把这个目录变成Git可以管理的仓库

```shell
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

会多出来一个 `.git`目录，是 Git来管理版本库的，此目录中的文件最好不要修改。`.git`目录默认是隐藏的，用 `ls -ah` 命令就可以看见。

**把文件添加到版本库**

所有的版本控制系统，只能跟踪文本文件的改动，比如 TXT 文件，网页，所有的程序代码等等 。 图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。 



*示例*

编写一个`readme.txt` 文件，放在 `learngit` 目录（子目录也可以）

**注意：** 不要使用Windows自带的**记事本**编辑任何文本文件 ， 建议你下载[Notepad++](http://notepad-plus-plus.org/)代替记事本 。

1、用命令 `git add` 告诉Git，把文件添加到仓库：

```shell
$ git add readme.txt
```

2、用命令`git commit` 告诉Git，把文件提交到仓库：

```shell
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`git commit` 命令中 `-m ` 后面输入的是本次提交的说明，可以输入任意内容，最好是有意义的。

命令执行成功后告诉我们 `1 file changed` ：1个文件被改动（新添加了一个 readme.txt文件）；`2 insertions`  :插入了两行内容。

**说明：** Git添加文件需要 `add` 和 `commit` 两步，多次 `add` 不同的文件，`commit` 可以一次提交很多文件。

```shell
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

#### 时光机穿梭

修改`readme.txt` 文件，运行 `git status`

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

 `git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。 

` git diff ` 可以查看具体修改了什么内容。

 提交修改和提交新文件是一样的两步，第一步是`git add`： 

```shell
$ git add readme.txt
```

 第二步`git commit` ：

```shell
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

**版本回退**

在对`readme.txt` 文件进行一次修改

此时的文件共有三个版本：

* wrote a readme file 
* add distributed 
* append GPL 

在 Git 中用 `git log` 命令查看历史版本

```shell
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数

在 Git 中，用 `HEAD` 表示当前版本，上一个版本就是 `HEAD^` 上上个版本就是 `HEAD^^`，往上100个版本写成 `HEAD~100`

使用 `git reset` 命令回退到上一个版本

```shell
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

只要上面的命令行窗口没有关闭，就可以找到 那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本： 

```shell
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```



当找不到新版本的`commit id` ,可以使用 `git reflog` 查看每一次命令。
