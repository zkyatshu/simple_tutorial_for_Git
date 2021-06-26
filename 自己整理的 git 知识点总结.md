# 自己整理的 git 知识点总结

## git clone 指令：





## git reset 指令：

在 commit 水平上执行 reset 指令主要有3种调用形式：

- `git reset --soft <commmit id>`

  > 行为1：改变 HEAD 指针所指的分支指针的指向，让其指向目标版本，然后改变 HEAD 指针重新指向上述分支指针。
  >
  > 不改变 working tree 和 index 中的内容。

- `git reset --mixed <commmit id>`

  >行为1：改变 HEAD 指针所指的分支指针的指向，让其指向目标版本，然后改变 HEAD 指针重新指向上述分支指针。
  >
  >行为2：改变 index，使其和 HEAD 指针所指版本内容一致。
  >
  >不改变 working tree 中的内容。

  > `git reset` 其实是 `git reset --mixed HEAD` 的省略写法。

- `git reset --hard <commmit id>`

  > 行为1：改变 HEAD 指针所指的分支指针的指向，让其指向目标版本，然后改变 HEAD 指针重新指向上述分支指针。
  >
  > 行为2：改变 index，使其和 HEAD 指针所指版本内容一致。
  >
  > 行为3：改变 working tree 使其看起来像 index。

  > 请慎重使用该命令，应为该命令会改变你的工作目录，未被提交的改动都会被覆盖！

在 作用路径 水平上执行 reset 命令的主要调用形式为：

- `git reset <tree-ish> -- <pathspec>`

  > 行为1：使用版本库中某一个版本的 \<pathspec\> 的内容替换 index 中相应 \<pathspec\> 的内容（官方文档：These forms reset the index entries for all paths that match the `<pathspec>` to their state at `<tree-ish>`. (It does not affect the working tree or the current branch.)）。
  >
  > 不改变 HEAD 指针以及 HEAD 指针所指向的分支指针。
  >
  > 不改变 working tree 中的内容。
  >
  > 如果不写 \<tree-ish\> 参数，则默认为 HEAD。
  >
  > 可以省略 `--`，但是对于我这种菜鸟，在使用的时候还是加上吧，省略形式能看懂就可以了，但自己别那么写！

  > 调用实例：`git reset -- .`



git reset 指令还可以用来压缩提交，详见[Git Pro中文文档](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)中压缩一节中的内容。



### 参考资料：

官方英文文档：https://git-scm.com/docs/git-reset

Git Pro中文文档：https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86





## git checkout 指令：

这里只介绍该指令的一部分功能！

指令总结：根据 index 或指定的版本来更新工作目录（官方文档：Updates files in the working tree to match the version in the index or the specified tree. ）。

`git checkout <tree-ish> -- <pathspec>`

官方文档：Overwrite the contents of the files that match the pathspec. When the `<tree-ish>` (most often a commit) is not given, overwrite working tree with the contents in the index. When the `<tree-ish>` is given, overwrite both the index and the working tree with the contents at the `<tree-ish>`.



git checkout 指令主要有2种调用形式：

- `git checkout -- <pathspec>`

  > 根据 index 中 \<pathspec\> 所对应的文件，改写 working tree 中 \<pathspec\> 所对应的文件的内容。
  >
  > 不改变 HEAD 指针以及 HEAD 指针所指向的分支指针。
  >
  > 不改变 index 中的内容。

- `git checkout <tree-ish> -- <pathspec>`

  > 根据版本库中 \<tree-ish\> 所对应版本中 \<pathspec\> 所对应的文件，改写 working tree 和 index 中 \<pathspec\> 所对应的文件的内容。
  >
  > 不改变 HEAD 指针以及 HEAD 指针所指向的分支指针。



> 请慎重使用 git checkout 命令，应为该命令会改变你的工作目录，未被提交的改动都会被覆盖！



下面介绍 git checkout 指令有关分支方面的功能：

`git checkout <branch>`：使 HEAD 指向 \<branch\> 分支指针，并根据所指分支指针所指向的版本，更新 working tree 和 index。

`git checkout --detach <branch>`：使 HEAD 指向 \<branch\> 分支指针所指的版本，此时 HEAD 指针处于 detched 状态，然后根据所指的版本，更新 working tree 和 index。

`git checkout [--detach] <commit>`：使 HEAD 指向 \<commit\> 这个提交（版本），此时 HEAD 指针处于 detched 状态，然后根据所指的版本，更新 working tree 和 index。



### detched HEAD：

关于 HEAD 的 detched 状态详见[官方文档](https://git-scm.com/docs/git-checkout#_detached_head)。



### 参考资料：

官方英文文档：https://git-scm.com/docs/git-checkout







## git rm 指令：

指令总结：删除 index 和 working tree 中相应的 pathspec，或仅删除 index 中相应的 pathspec。

> git 中没有指令可以实现只删除 working tree 中相应的 pathspec，而不删除 index 中相应的 pathspec。
>
> 可以使用 `rm <file/path>` 删除 working tree 中文件或目录，或者直接在资源管理器中手动删除文件或目录，但是这并不会影响 index 中相应的内容。

`git rm <pathspec>`：检查后，删除 index 和 working tree 中相应的 pathspec。

> 该指令只有 working tree、index 和 HEAD 所指向的版本三者中 pathspec 的内容完全一致才能删除成功，否则就会报错。

`git rm -f <pathspec>`：不检查，强制删除 index 和 working tree 中相应的 pathspec。

> 该指令会强制删除 index 和 working tree 中相应的 pathspec，不会检查上面所提到的三者是否一致。

`git rm --cached <pathspec>`：检查后，仅删除 index 中相应的 pathspec。

> 该指令只有 index 中的 pathspec 和 working tree 中 pathspec 的内容完全一致，**或** index 中的 pathspec 和 HEAD 所指向的版本中 pathspec 的内容完全一致，才能删除成功，否则就会报错。该指令不会对 working tree 中 pathspec 做任何改动。



### 参考资料：

官方英文文档：https://git-scm.com/docs/git-rm



<span style='color:green'>**已跟踪文件清单好像就是指暂存区中的内容**，所以使用 `git rm` 指令删除某一 pathspec 之后，pathspec 就不在已跟踪文件清单中了，即使之后再创建一个同名的文件，该文件也不在已跟踪文件清单中。</span>

> 上述理解是由[官方文档](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)中“移除文件”一节的内容得出的。



<span style='color:red'>git rm 指令在很多情况下还未被实验过，所以应该谨慎使用。</span>





## git mv 指令：

指令总结：Move or rename a file, directory or symlink.

下面只介绍该指令给一个文件改名的功能，该功能可以由下面的指令完成：

`git mv <source> <destination>`

运行上面的指令其实相当于运行了三个子指令，其实相当于给 index 和 working tree 中相应文件都改了名字，详细介绍见[官方文档](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)中“移动文件”一节的内容。





## git ls-files 指令：

`git ls-files` ：显示 index 中的所有文件。

`git ls-files -u` ：显示 index 中的所有 unmerged 文件。

该指令的完整功能请查看[官方文档](https://git-scm.com/docs/git-ls-files)。





## git branch 命令：

指令总结：列出/创建/删除/重命名/复制 分支。

`git branch`：列出所有分支。

`git branch <branchname> [<start-point>]`：创建一个名为 \<branchname\> 的分支，使其指向 \<start-point\>，如果不给出 \<start-point\>，则默认指向 HEAD 所指的提交。

> `<start-point>` 可以是一个分支的名字，一个 commit 的 id，或一个 tag。

> 该指令仅仅创建了一个分支（即指针），该指令并不会使 HEAD 指向该新创建的分支，需要使用 checkout 或 switch 指令来切换分支。

`git branch -d <branchname>`：删除 \<branchname\> 分支。

`git branch -m <oldbranch> <newbranch>`：将 \<oldbranch\> 分支重命名为 \<newbranch\>。

> 分支改名以后，其跟踪信息中相应的名字也会被更改，所以不需要重新建立其与上游分支的联系。



>  关于以上指令的所有选项及注意事项详见[官方文档](https://git-scm.com/docs/git-branch)。



> <span style='color:red'>有个问题就是：如果删除额一个分支，那么这个分支上的提交也被删除了吗，还是可以通过 reflog 查看 commit id 之后进行恢复？</span>



### 参考资料：

[官方文档](https://git-scm.com/docs/git-branch)





## git branch 指令（有关远程分支部分）：

`git branch -vv`：列出本地所有分支指针、这些分支指向的 commit 的 id 和提交信息、这些分支所跟踪的上游分支以及和跟踪分支的关系。



`git branch <branchname> [<start-point>]`：创建一个分支指针 `<branchname>`。当 `<start-point>` 是一个远程分支指针的时候，那么 `<branchname>` 分支会被设置为跟踪该远程分支（就是建立“联系”，让远程分支变为本地分支的上游分支），也就是说会更改 .git/config 配置文件中的 `branch.<name>.remote` 和 `branch.<name>.merge` 参数的值。

> 假如运行指令 `git branch branch2 github_test2/branch1`，.git/config 配置文件中会自动添加一下内容：
>
> ```
> [branch "branch2"]
> 	remote = github_test2
> 	merge = refs/heads/branch1
> ```



`git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]`：设置 `<branchname>` 的跟踪信息，将 `<upstream>` 设置为 `<branchname>` 的上游分支。如果没有指定 `<branchname>`，则默认为当前分支。该指令也可以用于更改 `<branchname>` 的跟踪信息，将其更改为跟踪其他分支。

> 注意：这 `<upstream>` 上游分支的指针的“影子指针”存在于本地仓库中时，用 `-u` 选项才有用。如果是远程仓库有远程分支`<upstream>`，但是本地仓库并没有远程分支`<upstream>`的“影子指针”的话，该指令是会报错说该上游指针不存在！

> 假如此时本地的 branch1 分支跟踪的是远程的 github_test/branch1 分支（即 github_test 仓库的 branch1 分支），那么 .git/config 文件中的内容为：
>
> ```
> [branch "branch1"]
> 	remote = github_test
> 	merge = refs/heads/branch1
> ```
>
> 下面使用 `git branch -u github_test2/master branch1` 指令让本地的 branch1 分支跟踪远程的 github_test2/master 分支（即 github_test2 仓库的 master 分支），那么 .git/config 文件中的内容将变为：
>
> ```
> [branch "branch1"]
> 	remote = github_test2
> 	merge = refs/heads/master
> ```



`git branch --unset-upstream [<branchname>]`：删除 `<branchname>` 关于远程分支的跟踪信息。如果没有指定 `<branchname>`，则默认为当前分支。

> 假如此时本地分支 `branch2` 跟踪的是 `github_test2/branch1` 远程分支，运行指令`git branch --unset-upstream branch2`之后，.git/config 配置文件中一下内容将会被删除：
>
> ```
> [branch "branch2"]
> 	remote = github_test2
> 	merge = refs/heads/branch1
> ```
>
> 注意：本地分支指针 `branch2` 还是存在于本地的，远程仓库`github_test2` 的`branch1` 分支也还存在于远程仓库中，只是两者之间的“联系”被切断了，即关于本地分支指针 `branch2` 的远程跟踪信息被删除了。



`git branch -d|-D -r <branchname>`：`-d` 和 `-D`两个选项是选且仅选其中一个选项在指令中的，删除远程仓库的远程分支 `<branchname>` 在本地仓库的快照中的“影子分支（指针）”。

> 1. 问：可以删除在本地仓库的远程分支指针吗？
>
>    答：只能使用 -r 选项才能删除远程仓库的远程分支在本地仓库中的“影子指针”（不加`-r`只有`-D`，指令会报错说找不到指针），远程仓库的远程分支其实是还在的，下次 fetch 还是依然会在本地仓库中创建远程仓库中远程分支的“影子指针”。
>
> 2. 问：删除远程分支之后，与之建立联系的本地分支的跟踪信息会被删除吗？
>
>    答：不会被删除，即 .git/config 文件中关于本地分支和该仓库的远程分支的跟踪信息并没有被删除。删除远程分支并不是真的删除了远程仓库中的远程分支指针，只是删除了本地仓库中对远程仓库上一次快照中远程分支的“影子指针”，其实不会对远程仓库造成任何影响！但是使用 `git remote remove <repository>`  之后，远程仓库的远程分支与本地分支的“联系”将被全部切断，也就是 .git/config 文件中关于这个仓库的信息，以及该仓库的远程分支和本地分支之间的跟踪信息都会被删除。





## git tag 指令：

### 标签的类型

Git 支持两种标签：轻量标签（lightweight）与附注标签（annotated）。

1. 轻量标签：它只是某个特定提交的引用。

2. 附注标签：是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。



### 创建标签（给提交打标签）

打（创建）标签的本质其实是在 `.git/refs/tags/`目录下添加一个 标签的引用(reference)，即在该目录下创建一个以标签为文件名的没有后缀的文件。对于轻量标签，其文件中的内容是 所指向的提交的 SHA-1 校验和的值；<span style='color:red'>对于附注标签，其文件中内容好像也是一个 SHA-1 校验和，但是不知道所指的这个对象是什么。</span>

`git tag <tagname> [<commit>]`：给指定提交创建轻量标签。如果不给 `<commit>` 参数值，则默认为 HEAD 所指向的提交。

`git tag -a <tagname> [<commit>] -m <msg>`：给指定提交创建附注标签。如果不给 `<commit>` 参数值，则默认为 HEAD 所指向的提交。



### 删除标签

`git tag -d <tagname>`：删除本地标签 `<tagname>`。



### 查询标签

`git tag`：以字母顺序列出所有的标签。

`git tag -l <pattern>`：列出所有与标签名的通配模式 `<pattern>` 匹配的标签。

> 举个栗子，`git tag -l 'v1*'` 将列出所有以 `v1` 开头的标签。

`git rev-list <tagname>`：列出标签 `<tagname>` 所指向的提交的 SHA-1 校验和的值。

`git show <tagname>`：显示标签 `<tagname>` 的信息（对于附注标签）以及它所引用的对象。如果引用的对象是一个提交，还会显示一些文本差异，该文本差异的显示其实相当于调用了底层指令`git diff-tree --cc <tagname> `。

> `git diff-tree --cc <tree-ish>`：比较通过两个树对象找到的 blob 对象的内容和模式，如果只给定了一个`<tree ish>`，则将提交与其父级进行比较。



### 使用标签

`git checkout <tagname> `：相当于`git checkout [--detach] <commit>` 指令，指令功能详见 [git checkout 指令部分](#git checkout 指令：)。



### git 是如何管理标签的

这部分属于自己查看资源管理器相关文件的猜测，详见[这里](#关于 .git/refs 文件夹：)。



### 与远程仓库相关内容

#### 推送标签到远程仓库：

默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。

`git push <repository> <tagname> `：将一个标签 `<tagname>` 推送至远程仓库 `<repository>`。

``git push --tags <repository>`：推送 refs/tags 目录下的所有引用到远程仓库 `<repository>`。该指令不会把本地的新提交也一并提交到远程仓库，仅仅提交了所有的标签。



#### 从远程仓库抓取标签

当使用 `git fetch` 指令时，默认情况下，指向将要从远程仓库被获取的历史记录的任何标记也会被获取。

可以在 `git fetch` 指令中使用 `-t` 或 `--tags` 选项来获取远程仓库的所有标签，即将远程仓库的 refs/tags/* 以相同的名字抓取至本地目录。



#### 删除远程仓库的标签

`git push <remote> :refs/tags/<tagname>`：删除远程仓库 `<remote>` 中的标签 `<tagname>`。

>  该指令的含义是，将冒号前面的空值推送到远程标签名，从而高效地删除它。

`git push <remote> --delete <tagname>`：删除远程仓库 `<remote>` 中的标签 `<tagname>`。



### 参考资料：

https://git-scm.com/docs/git-tag

https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE





## git remote 指令：

`git remote`：显示所有远程仓库的别名。

`git remote -v`：显示所有远程仓库的别名，并在其后面显示相应的 url。

`git remote show <name>`：显示某一个远程仓库的相关信息，包括：Fetch URL、Push URL、HEAD branch、Remote branches、Local branches configured for 'git pull'、Local refs configured for 'git push' 等。

`git remote add <name> <url>`：添加一个远程仓库，并给它的 `<url>` 起一个别名 `<name>`。

> `git remote add github_test git@github.com:zkyatshu/test.git` 指令会在 .git/config 文件中自动添加一下内容：
>
> ```
> [remote "github_test"]
> 	url = git@github.com:zkyatshu/test.git
> 	fetch = +refs/heads/*:refs/remotes/github_test/*
> ```



`git remote rename <old> <new>`：对一个远程仓库的别名进行修改，并对该远程仓库的所有远程跟踪分支以及配置设置进行更新。

> 假如 .git/config 文件中关于远程仓库 github_test 的内容为：
>
> ```
> [remote "github_test"]
> 	url = git@github.com:zkyatshu/test.git
> 	fetch = +refs/heads/*:refs/remotes/github_test/*
> [branch "master"]
> 	remote = github_test
> 	merge = refs/heads/master
> [branch "branch1"]
> 	remote = github_test
> 	merge = refs/heads/branch1
> ```
>
> 那么，在执行指令 `git remote rename github_test GITHUB_TEST` 之后， .git/config 文件中关于远程仓库 github_test 的内容会被修改为：
>
> ```
> [remote "GITHUB_TEST"]
> 	url = git@github.com:zkyatshu/test.git
> 	fetch = +refs/heads/*:refs/remotes/GITHUB_TEST/*
> [branch "master"]
> 	remote = GITHUB_TEST
> 	merge = refs/heads/master
> [branch "branch1"]
> 	remote = GITHUB_TEST
> 	merge = refs/heads/branch1
> ```
>
> <span style='color:red'>注意该指令会改变 fetch 一项的值，感觉对于远程仓库的推送和抓取会造成影响，所以请谨慎使用该命令！</span>



`git remote remove <name>`：在本地删除远程仓库的别名，并且删除关于该仓库的远程跟踪分支以及配置设置。

> 假如 .git/config 文件中关于远程仓库 github_test2 的内容为：
>
> ```
> [remote "github_test2"]
> 	url = git@github.com:zkyatshu/test2.git
> 	fetch = +refs/heads/*:refs/remotes/github_test2/*
> [branch "master2"]
> 	remote = github_test2
> 	merge = refs/heads/master
> [branch "branch2"]
> 	remote = github_test2
> 	merge = refs/heads/branch1
> ```
>
> 那么，在执行指令 `git remote remove github_test2` 之后， .git/config 文件中上面的内容都将被删除。
>
> 但是本地的 master2、branch2 两个分支指针还是存在于本地的！远程仓库也不会受到影响。





## git ls-remote 指令：

`git ls-remote <repository>`：展示远程仓库 `<repository>` 可用的引用(references)，以及与之相关的 commit 的 ID。`<repository>` 可以是仓库的别名。

><span style='color:red'>好像有关于 tag 的选项，但是没看。</span>





## git fetch 指令：

`git fetch <repository> <refspec>`：从 `<repository>` 仓库中抓取分支（和标签）以及完成其历史记录所需的对象，对本地仓库中的远程跟踪分支进行更新。

> 该指令不会对本地的 index 和 working tree 产生任何影响。

><span style='color:red'>官方文档的第二大段讲了关于tag的问题，此处先不介绍。</span>
>
><span style='color:red'>此指令会对 .git/FETCH_HEAD 文件产生影响，该文件可能会被其他指令（比如 `git push` 指令）使用到，但这里不做介绍。</span>



### [CONFIGURED REMOTE-TRACKING BRANCHES](https://git-scm.com/docs/git-fetch#_configured_remote_tracking_branches)

假设 .git/config 配置文件中有：

```
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
```

当 `git fetch` 指令没有指定 fetch 哪个分支和/或标签时，即指令为 `git fetch origin` 或 `git fetch`，`remote.<repository>.fetch` 的值将被用作 `<refspec>` 的值，它将指定 fetch 哪一个远程分支以及更新哪一个本地分支，上面的例子中，将抓取 `origin` 所有的分支，并更新相应的 remote-tracking 分支（抓取左边，更新右边）。

当 `git fetch` 指令指定了 fetch 哪个分支和/或标签时，即指令为 `git fetch origin master`，指令中的`<refspec>s` 决定了应该 fetch 哪些分支，在该指令中将只抓取 master 分支。`remote.<repository>.fetch ` 决定哪一个 remote-tracking 将被更新。当使用这种指令方式的时候，`remote.<repository>.fetch` 不会对抓取什么产生任何影响，但是该值会决定 fetch 到的引用应该保存在哪里。

> 对于自己这种新手就不要省略指令中的参数了，老老实实、踏踏实实地使用包括完整参数的指令！



关于引用规范（即 `<refspec>`）的介绍，详见以下两个官方参考资料：

https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E5%BC%95%E7%94%A8%E8%A7%84%E8%8C%83

https://git-scm.com/docs/git-fetch





## git push 指令：

`git push <repository> <refspec>`：使用本地的引用，即 `<refspec>`，更新远程仓库 `<repository>` 的引用，同时发送完成相应的引用所需的对象。

<span style='color:green'>使用自己的话来说，就是将分支 `<refspec>`推送到远程仓库 `<repository>`，并在远程仓库的相同位置创建和本地分支同名的分支，但是本地分支和远程仓库的同名分支并不会建立“联系”。</span>

当指令没有给出 `<refspec>` 的时候：

1. 指令会到配置文件中查询 `remote.*.push` 参数的值作为 `<refspec>` 的默认值。
2. 如果在配置文件中找不到该参数，将根据配置中的 `push.default` 参数来决定推送什么。
3. 当指令和配置文件都没有指定要推送什么时，默认的行为是（该行为即为 `push.default` 配置参数的值为 `simple` 时所对应的行为）：将当前分支推送到它所跟踪的上游分支，但作为保护措施，如果上游分支与当前本地分支的名称不相同，推送将被中止。

> 关于 `push.default` 配置参数，详见[官方手册](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pushdefault)。



`git push -u <repository> <refspec>`：`-u` 选项的作用是在推送的同时，让本地分支和远程仓库中的同名分支建立“联系”，使本地分支开始跟踪远程仓库中的同名分支。



 <span style='color:red'>`git push origin --delete branchname`</span>





## git log 常用指令：

git log --oneline --graph --decorate --all

git log --oneline --graph --all





## git 配置：

### 关于分之合并的配置：

`git config merge.conflictStyle diff3`：详细说明见[说明文档1](https://git-scm.com/docs/git-config#Documentation/git-config.txt-mergeconflictStyle)和[说明文档2](https://git-scm.com/docs/git-merge#_how_conflicts_are_presented)。

> 该参数的默认值是 `merge`。



### 关于指令别名的配置：

`git config --global alias.tree 'log --oneline --graph --all'`：以后指令 `git log --oneline --graph --all` 就可以直接简写成 `git tree` 就好了。

>  该指令的实质应该就是在全局配置文件中加上以下内容，本机的全局配置文件为C:\Users\zky\.gitconfig 文件：
>
> ```
> [alias]
> 	tree = log --oneline --graph --all
> ```

`git config --global --unset alias.tree`：该指令可以删除前一个命令设置的别名，本质也是删除 C:\Users\zky\.gitconfig 文件中的上述内容。

`git config --global --remove-section alias`：删除全局配置文件 .gitconfig 文件中的 [alias] 小节、以及该小节底下的内容，从而删除所有命令别名。





## 关于分支合并的知识：

此处只是简单介绍一部分，很多没看懂也没办法介绍，够用就好。

### [FAST-FORWARD MERGE](https://git-scm.com/docs/git-merge#_fast_forward_merge)：

此种合并不会产生额外的提交，仅仅是移动了 HEAD 所指的分支指针以及 HEAD 指针本身，并更新 index 和 working tree。

### [TRUE MERGE](https://git-scm.com/docs/git-merge#_true_merge)：

当进行合并，且不能协调两个分支之间的改动时，会执行以下步骤：

1. HEAD 指针的位置不发生变化；

2. 将 MERGE_HEAD 设置为指向另一个分支（被合并的分支）；

3. 能够合并的 paths，进行合并之后会更新 index 和 working tree；

4. 对于有冲突的 paths，在 index 中会保存三个版本：

   1. stage 1：保存了共同祖先的版本；
   2. stage 2：保存了 HEAD 所指的版本；
   3. stage 3：保存了 MERGE_HEAD 所指的版本；

   > 上述三个版本可以指令 `git ls-files -u` 进行查看。

   在 working tree 中则会有将上述三方“合并”后的版本（其中会标识出冲突的地方以及不同版本的内容）。

   > 在使用 `git config --global merge.conflictstyle diff3` 命令进行配置之后，上述合并后的版本会同时展示三个版本的内容（即多了共同祖先的版本的内容），而不是只包含两个分支版本的内容。
   >
   > 关于此配置的相关资料：
   > https://git-scm.com/docs/git-config#Documentation/git-config.txt-mergeconflictStyle
   >
   > https://git-scm.com/docs/git-merge#_how_conflicts_are_presented
   >
   > https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%AB%98%E7%BA%A7%E5%90%88%E5%B9%B6 中的“检出冲突”部分。

5. 其他的部分不会被改动。

### [HOW TO RESOLVE CONFLICTS](https://git-scm.com/docs/git-merge#_how_to_resolve_conflicts)：

1. 选择撤销此次合并：

   执行指令 `git merge --abort`，回到合并之前的状态。

2. 进行手动合并：

   `git show :1:filename > ancestor`

   `git show :2:filename > ours`

   `git show :3:filename > theirs`

   使用上面的三个指令可以将三个版本的文件输出到 working tree，分别命名为`ancestor`、`ours` 和 `theirs`。

   其中，`:1:filename` 对应于共同祖先的版本；`:2:filename` 对应于 HEAD 所指的版本；`:3:filename` 对应于 MERGE_HEAD 所指的版本。

   

   将 working tree 中有冲突的文件手动修改好后，执行 `git add` 指令将其添加到 index，此时 index 中原来的三个版本的文件就没有了，只有你刚添加到 index 的这个文件。

   将所有冲突的文件都修改好并添加到 index 之后，就可以提交到版本库了。

   提交以后，HEAD 所指的分支指针会指向该合并后的提交，然后 HEAD 会指向该分支的指针。



### 参考资料：

https://git-scm.com/docs/git-merge#_how_to_resolve_conflicts

https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%AB%98%E7%BA%A7%E5%90%88%E5%B9%B6

https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6





## 关于远程分支的知识：

### 远程分支部分常用指令：

1. [git remote 指令](#git remote 指令：)
2. [git branch 指令](#git branch 指令（有关远程分支部分）：)
3. [git fetch 指令](#git fetch 指令：)
4. [git push 指令](#git push 指令：)



### 两个本地和远程仓库的互动过程演示：

1. 在Github上创建仓库，创建并配置 SSH key；

   

2. 在本地（A）添加远程仓库信息：

   `git remote add <name> <url>`：我理解这条指令的功能是给远程仓库地址 `<url>` 起一个便于使用的别名 `<name>`。

   `git remote add github_test https://github.com/zkyatshu/test.git`

   `git remote add github_test git@github.com:zkyatshu/test.git`

   > `git remote remove github_test`：删除 https://github.com/zkyatshu/test.git 这个仓库在本地（A）的别称 github_test。实际上是更改了 .git/config 文件中的内容。

   

3. 将本地（A）仓库推送至远程仓库，顺便建立远程分支和本地（A）分支的“联系”：

   `git push -u github_test master`：将本地（A）仓库的 master 分支推送到远程仓库，并在远程仓库的相应位置建立 master 分支（指针），然后使本地（A）的 master 分支与远程的 master 分支建立联系。

   > 但是从另一个本地（B）抓取到远程仓库的结果看，本地的另一个分支 branch1 分支的提交记录也推送到了远程仓库，但是远程仓库的相应位置并没有与该分支对应的分支指针！
   >
   > <span style='color:red'>创建远程分支和本地分支的联系时，是怎么改变配置文件的？</span>

   `git push -u github_test branch1`：将本地（A）仓库的 branch1 分支推送到远程仓库，并在远程仓库的相应位置建立 branch1 分支（指针），然后使本地（A）的 branch1 分支与远程的 branch1 分支建立联系。

   

4. 在本地（B）添加远程仓库，抓取远程仓库到本地（B）：

   `git remote add github_test git@github.com:zkyatshu/test.git`

   `git fetch github_test`：抓取远程仓库到本地版本库，并不会对本地 index 和 working tree 带来影响。

   > 注意：简单来说，`git pull` 指令相当于 `git fetch` 指令再加上 `git merge` 指令，所以该指令可能会影响到 working tree，所以不建议自己使用该指令！老老实实、踏踏实实地使用 `git fetch` 指令！

   > 这里抓取到的包括两个分支，分别为 github_test/master 分支和 github_test/branch1 分支，但是本地（B）并不能操作这两个分支指针，它们是“只读”的，使用 `git log --oneline --graph --all` 指令查看版本历史“树”，可以看见两个指针是红色的，且没有本地指针，甚至没有 HEAD 指针。所以要自己创建本地分支指针并使之与远程分支建立“联系”。

   

5. 建立本地（B）分支和远程分支的“联系”：

   `git branch master github_test/master`：在本地（B）创建 master 分支指针，使之指向远程分支 github_test/master 所指的 commit。

   > 此时已经有了 HEAD 指针，指向的是本地的 master 分支指针。这里只是创建了指针，对 index 和 working tree 并没有影响。
   >
   > 需要在运行 `git checkout master` 才能改变 index 和 working tree。
   >
   > 关于该指令怎样更改了配置文件，见[这里](#git branch 指令（有关远程分支部分）：)

   

   也可以让本地（B）已经存在的分支指针和远程分支建立联系：

   - 先创建本地分支指针：

     `git branch branch1 b1b01cd`：其中 `b1b01cd` 为远程分支 github_test/branch1 分支指针所指的那个 commit 的 id。

   - 在分支上进行一次提交：

     `git add .`

     `git commit -m '...'`

   - 创建本地 branch1 分支和远程 github_test/branch1 分支之间的联系：

     `git branch -u github_test/branch1 branch1`

     > 更改一个分支的所追踪的上游分支也可以使用该指令，详见[这里](#git branch 指令（有关远程分支部分）：)！

   

6. 推送本地分支（B）到远程分支：

   `git push github_test`：<span style='color:red'>不知道该指令是推送当前分支还是推送所有本地（B）分支。</span>

   



## 关于 .git/refs 文件夹：

### 文件结构：

--.git

​    --refs

​        --heads

​            master

​            branch1

​            branch2

​        --remotes

​            --github_test

​                master

​                branch1

​            --github_test2

​                branch2

​        --tags

​            v1

​            v1_



### 末端文件内容：

1. .git/refs/heads、.git/refs/remotes 目录下的文件中的内容只是 所指向的提交的 SHA-1 校验和的值！

2. .git/refs/tags 目录下的文件中，如果文件名对应的是轻量标签，则其内容是 所指向的提交的 SHA-1 校验和的值；如果文件名对应的是附注标签，则其中的内容也是一个对象的 SHA-1 校验和，但是不知道这个对象是什么。





