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

