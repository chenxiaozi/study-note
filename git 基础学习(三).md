#### 创建与合并分支

每次提交，Git 都会把他们传承一个时间线，这条时间线就是一个分支。到目前为止，只有一个时间线，在 Git 中，这个分支叫主分支（`master分支`）。`HEAD` 严格来说不是指向提交，而是指向 `master` , `master` 才是指向提交的，所以，`HEAD` 指向的就是当前分支。

1、开始，`master` 分支是一条线，Git 用 `master` 指向最新的提交，再用 `HEAD` 指向 `master` ，就能确定当前分支，以及当前分支的提交点。

每次提交，`master` 分支都会向前移动一步，随着不断的提交，`master` 分支的线也越来越长。

2、当创建新的分支，例如 `dev` 时，Git 新建了一个指针叫 `dev` ，指向 `master` 相同的提交，再把 `HEAD` 指向 `dev` ，就表示当前分支在 `dev` 上。因为只需要改变指针的指向，所以 Git 创建分支的速度很快。

从现在开始，对工作区的修改和提交就是针对 `dev` 分支了，比如新提交一次以后，`dev` 指针向前移动了一步，而 `master` 指针不变。

3、当 `dev` 上的工作完成了以后，就可以把 `dev` 合并到 `master` 上。（直接把 `master` 指向 `dev` 的当前提交）

4、合并完成后可以删除 `dev` 分支（把 `dev` 指针删掉）

**下面来实战一下**

根据上面说的步骤进行：

1、创建 `dev` 分支，然后切换到 `dev `分支

```shell
$ git checkout -b dev
Switched to a new branch 'dev'
```

 `git checkout` 命令加上 `-b` 参数表示创建并切换，相当于一下两条命令：

```shell
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

使用 `git branch` 命令查看当前分支：

```shell
$ git branch
* dev  
  master
```

2、现在就可以在 `dev` 分支上正常提交：

```shell
$ git add readme.txt  
$ git commit -m "branch test"
```

3、当 `dev` 分支上的工作完成，就可以切换回 `master` 分支：

```shell
$ git checkout master
Switched to branch 'master'
```

`master` 分支的提交点并没有改变，所以看不到 `dev` 分支上的修改。

将 `dev` 分支的工作合并到 `branch` 分支上：

```shell
$ git merge dev
```

4、删除 `dev` 分支：

```shell
$ git branch -d dev
```

#### switch

最新版本的 Git 提供了 `git switch` 吗，命令来切换分支：

* 创建并切换到新的 `dev` 分支

```shell
$ git switch -c dev
```

* 直接切换到已有的 `master` 分支

```shell
$ git switch master
```

#### **总结**

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>  `

切换分支：`git checkout <name> `或者`git switch <name> `

创建+切换分支：`git checkout -b <name> `或者`git switch -c <name> `

合并某分支到当前分支：`git merge <name> `

删除分支：`git branch -d <name>` 

#### 解决冲突

合并分支有时候也会遇见冲突。

创建一个新的分支：

```shell
$ git switch -c feature1
```

修改 `readme.txt` 上的内容，并在 `feature1` 分支上提交：

```shell
$ git add readme.txt

$ git commit -m "AND simple"
```

切换到 `master` 分支：

```shell
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

#### 解决冲突

创建新的 `featurel` 分支

修改 `readme.txt` ，并在 `featurel` 分支上提交，切换到 `master` 分支：

```shell
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。 

在 `master` 分支上继续对 `readme.txt` 文件进行修改，并提交。

此时，`master` 分支和 `featurel` 分支各自都分别有新的提交。（即在分支 `featurel` 上的修改与 `master` 还没有合并时，`master` 分支上又进行了修改）Git 的快速合并在这种情况下无法执行，可以试图将各自的修改进行合并，但有可能发生冲突。

```shell
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。 

Git会用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容 ：（readme.txt 中的内容）

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

此时，进行提交：

```shell
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

现在，`master` 分支和 `featurel` 分支变成了下图：

![1606381791562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1606381791562.png)

最后，删除 `featurel` 分支：

```shell
$ git branch -d feature1
```

**总结：** 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。 

用`git log --graph`命令可以看到分支合并图。 

#### 分支管理策略

