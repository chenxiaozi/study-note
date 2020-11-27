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

#### **switch**

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

通常，合并分支时，如果可能，Git 会用 `Fast forward` 模式，这种模式下删除分支后，回丢掉分支信息。如果强制禁用 `Fast forward` 模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。

**--on-ff 方式的 git merge**

1、创建并切换到 `dev` 分支

2、修改 `readme.txt` 文件，并提交一个新的 commit

3、切换到 `master` 

4、准备合并 `dev` 分支，注意 `--on-ff` 参数，表示禁用 `Fast forward`:

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
  1 file changed, 1 insertion(+)
```

 因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。 

然后可以使用 `git log` 查看分支历史：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
```

#### Bug 分支

在 Git 中，可以为每个 Bug 创建一个临时分支，修复后在进行合并。

当遇到 Bug 时，想要创建一个 `issue-bug` 分支来修复它，但是 `dev` 分支上的工作还没有提交。Git 提供了一个 `stash` 功能，可以把当前工作现场“储藏”起来，等恢复现场后继续工作：

```shell
$ git stash
```

修复 Bug 步骤：

1、确定要在那个分支上修复 bug，假定要在 `master` 分支上修复，就从 `master` 创建临时分支：

```shell
$ git checkout master

$ git checkout -b issue-bug
```

2、修改好 Bug 后提交（`git add` 命令和 `git commit`命令）

```shell
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-bug 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

3、 切换到 `master` 分支，并完成合并，最后删除 `issue-bug`  分支：

```shell
$ git switch master

$ git merge --no-ff -m "merged bug fix 101" issue-101
```

4、继续回到 `dev` 继续开发，此时的 `dev` 分支工作区是干净的：

```shell
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

使用 `git stash list` 命令可以查看保存工作现场的地方：

```shell
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

5、恢复 `git stash` 隐藏的工作现场，有两个方法

* 用 `git stash apply` 恢复，用 `git stash drop` 删除 stash 内容
* 用 `git stash pop` ，恢复的同时把 stash 内容也删除

**思考**：因为 dev 分支是从 master 分支分出来的，所以在 master 分支上修改的 bug 也存在于 dev 分支上。现在就需要修复 dev 分支上的 bug。

需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到 dev 分支 。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。 Git 提供了一个`cherry-pick`命令 。

```shell
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

#### Feature 分支

每添加一个新功能，最好新建一个 feature 分支，在上面开发后再进行合并，最后删除该 feature 分支。

1、创建并切换至 feature 分支

```shell
$ git switch -c feature
```

2、开发完毕后，提交

3、切回 dev 分支，准备合并，与 bug 分支类似，合并后删除。

4、如果想要删除新功能

```shell
$ git branch -D feature
```

`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。

#### 多人协作

当从远程仓库克隆时，实际上 Git 自动把本地的 `master` 分支和远程 `master` 分支对应起来，远程仓库的默认名称是 `origin` 。

查看远程库的信息，用 `git remote` :

```shell
$ git remote
origin
```

或者 `git remote -v` 显示更详细的信息 :

```shell
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。 

**推送分支**

把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```shell
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```shell
$ git push origin dev
```

 但是，并不是一定要把本地分支往远程推送 ：

- `master` 分支是主分支，因此要时刻与远程同步；
- `dev` 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug 分支只用于在本地修复 bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

**抓取分支**

当在另一台电脑上克隆时：

```shell
$ git clone git@github.com:michaelliao/learngit.git
```

默认只能看见本地的 `master` 分支。

创建远程 `origin` 的 `dev `分支到本地（本地和远程分支名称最好一致） ：

```shell
$ git checkout -b dev origin/dev
```

此时，就可以在 `dev `上继续修改，然后，时不时地把 `dev` 分支 `push` 到远程。但你也对同样的文件做了修改，并试图推送，会推送失败，因为产生了冲突。

先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

1、制定本地 `dev `分支与远程 `origin/dev` 分支的链接 

```shell
$ git branch --set-upstream-to=origin/dev dev
```

2、pull

```shell
$ git pull
```

3、此时合并有冲突，需手动解决，解决方法与分支管理中的解决冲突完全一样。解决后，提交，再push

####  Rebase 

多人在同一支分支上协作，容易产生冲突。使用 `git rebase` 可以解决提交分叉的问题。

**总结**

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。





