#### 创建标签

标签的作用就是标记一个版本，它指向某个 commit ，但是不可以移动。

1、切换到需要打标签的分支上

2、敲命令 `git tag <name>` 就可以打一个新标签：

```shell
$ git tag v1.0
```

可以使用 `git tag` 查看所有标签：

```shell
$ git tag
v1.0
```

默认标签是打在最新提交的 commit 上的。

也可以找到历史中的 `commit id` ，打上标签：

```shell
$ git log --pretty=oneline --abbrev-commit

$ git tag v0.9 f52c633
```

*注意标签不是按时间顺序输出的，是按字母顺序排序的。可以用 `git show <tagname>` 查看标签信息。*

标签总是和某个 commit 挂钩。如果这个 commit 既出现在master分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。

#### 操作标签

删除标签：`$ git tag -d v0.1`

创建的标签都只存储在本地，不会推送到远程。

推送标签到远程：`git push origin <tagname>`

一次性推送全部尚未推送到远程的本地标签: `$ git push origin --tags`

删除远程标签：

* 先本地删除 `$ git tag -d v0.9`
* 远程删除 `$ git push origin :refs/tags/v0.9`

#### 使用 GitHub

参与一个开源项目的步骤（以 bootstrap 为例）：

1、点 “Fork” 就在自己的账号下克隆了一个 bootstrap 仓库

2、从自己的账号下clone：

```shell
git clone git@github.com:michaelliao/bootstrap.git
```

一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git ` 克隆，因为没有权限，你将不能推送修改。

#### 自定义 Git

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

如果你确实想添加该文件，可以用`-f`强制添加到Git：

```shell
$ git add -f App.class
```

### 小结

- 忽略某些文件时，需要编写 `.gitignore`；
- `.gitignore `文件本身要放到版本库里，并且可以对 `.gitignore` 做版本管理！