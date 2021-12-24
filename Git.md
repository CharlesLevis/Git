# Git 教程

## 1 开始

### 1.1 安装 Git

可以在 Linux，Windows 和 Mac OS 上安装 Git。

##### ① `git config` 设置用户

安装好之后，还需要最后一步设置，在命令行输入：

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

### 1.2 创建版本库

版本库又名仓库，英文名**repository**，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```shell
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

##### ② `git init` 初始化仓库

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```shell
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

### 1.3 将文件添加到版本库

**所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。**版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

在 Git 仓库中，也就是`learngit`目录下（子目录也行）创建并编写一个`readme.txt`文件，内容如下：

```markdown
Git is a version control system.
Git is free software.
```

把一个文件放到Git仓库只需要两步。

##### ③ `git add` 添加文件到仓库

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```shell
$ git add readme.txt
```

执行上面的命令，没有任何显示，说明添加成功。（Unix的哲学是“没有消息就是好消息”）

##### ④ `git commit` 提交文件到仓库

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```shell
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt中的两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```shell
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

### 1.4 疑难解答

Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

A：Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。

Q：输入`git add readme.txt`，得到错误：`fatal: pathspec 'readme.txt' did not match any files`。

A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

## 2 时光穿梭

### 2.1 时空定位

**要随时掌控工作区状态，使用`git status`命令。**

继续修改readme.txt文件，改成如下内容：

```
Git is a distributed version control system.
Git is free software.
```

##### ⑤ `git status` 查看工作区状态

现在，运行`git status`命令看看结果：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

**`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt` 被修改过了，但还没有准备提交的修改。**

虽然Git告诉我们`readme.txt`被修改了，但如果能看看具体修改了什么内容，自然是很好的。

##### ⑥ `git diff` 查看修改内容

可以用`git diff`这个命令看看：

```shell
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
\ No newline at end of file
```

`git diff`顾名思义就是查看 difference，显示的格式正是 Unix 通用的 diff 格式，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：

```shell
$ git add readme.txt
```

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```shell
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

**`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：**

```shell
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```shell
$ git status
On branch master
nothing to commit, working tree clean
```

**这次，`git status`告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。**

### 2.2 版本回溯

再次修改 readme.txt 文件如下：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

然后提交：

```
$ git add readme.txt
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

在 Git 中，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在 Git 中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

##### ⑦ `git log` 查看历史记录

在Git中，我们**用`git log`命令查看历史记录**：

```shell
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```shell
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

需要友情提示的是，你看到的一大串类似`1094adb...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

+++

现在启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

##### ⑧ `git reset` 版本退回

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

`--hard`参数有啥意义？这个后面再讲，现在你先放心使用。

再看`readme.txt`的内容，已经还原到了版本`add distributed`：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

再用`git log`再看看现在版本库的状态：

```shell
$ git log --pretty=oneline
e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master) add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

之前最新的那个版本 `append GPL`已经看不到了。

现在，想要再次重现未来的版本，可以在命令行窗口（未关闭前）中找到那个`append GPL`的`commit id`是`1094adb...`，

于是，再次使用`git reset`命令：

```shell
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

版本号没必要写全，前几位就可以了，Git 会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再次查看`readme.txt`的内容：

```shell
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

成功重现之前的最新版本 `append GPL`。

Git 的版本回退速度非常快，因为 Git 在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git 仅仅是把 HEAD 从指向 `append GPL`

```
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

 改为指向 `add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

然后顺便把工作区的文件更新了。所以，让`HEAD`指向哪个版本号，时光机就定位在哪个版本。

如果回退到了某个版本之后，关掉了电脑，想恢复到新版本，却找不到新版本的`commit id`怎么办？

在 Git 中，总是有后悔药可以吃。当使用 `$ git reset --hard HEAD^` 回退到 `add distributed` 版本时，再想恢复到 `append GPL`，就必须找到 `append GPL`的 commit id。Git 提供了一个命令`git reflog`用来记录你的每一次命令：

```shell
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

### 2.3 工作区和版本库

+ **工作区**：就是在电脑里能看到的目录，比如之前创建的 `learngit` 文件夹就是一个工作区：

+ **版本库（Repository）**：工作区有一个隐藏目录`.git`，这个不算工作区，而是 Git 的版本库。

  Git 的版本库里存了很多东西，其中最重要的就是**称为 stage（或者叫 index）的暂存区**，还有 Git 为我们自动创建的第一个**分支 `master`**，以及指向`master`的一个指针叫 `HEAD`。

**Git 的工作流程以及文件在工作区和版本库（缓存区和分支）的映射关系。**

![git-repo](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0.jpeg) 

实际上，`git add`命令就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的。此时，暂存区就没有任何内容了。

### 2.4 管理修改

——为什么 Git 比其他版本控制系统设计得优秀，因为 Git 跟踪并管理的是修改，而非文件。

用 `git diff HEAD -- readme.txt` 命令可以查看工作区和版本库里面最新版本的区别：

### 2.5 撤销修改

如果工作时不小心在`readme.txt`中添加了错误的一行：

```shell
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
My stupid boss still prefers SVN.
```

**场景1：错误发现得很及时（没有 `git add`，也没有 `git commit`），就可以很容易地纠正它。**

可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

可以发现，Git会告诉你，`git restore file`可以丢弃工作区的修改：

##### ⑨ `git restore` 撤销修改

```shell
$ git restore readme.txt
```

命令`git restore readme.txt`意思就是，把 `readme.txt` 文件在工作区的修改全部撤销，这里有两种情况：

+ 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

+ 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

现在，看看`readme.txt`的文件内容：

```shell
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

文件内容果然复原了。

**场景2：如果错误在 `git add` 到暂存区之后才被发现，问题也不大。**

在 `commit` 之前，用命令 `git reset HEAD <file>` 可以把暂存区的修改撤销掉（unstage），重新放回工作区。（`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。）

再用`git status`查看一下，会发现暂存区是干净的，工作区有修改。

最后再用 `git restore file` 撤销工作区的修改。

**场景3：假设不但改错了东西，还从暂存区提交到了版本库，怎么办呢？**

还记得版本回退吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，就真的惨了……

+++

**小结**

+ 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git restore file`。

+ 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

+ 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 2.6 删除文件

##### ⑩ `git rm` 删除文件

一般情况下，直接在文件管理器中把没用的文件手动删除了，或者用 `rm` 命令删了：

```shell
$ rm test.txt
```

此时，工作区和版本库就不一致了：在工作区，test.txt 文件已被删除，而在版本库分支中仍然存在。

现在有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```shell
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

这样，文件就从版本库中被删除了。

*! 小提示：先手动删除文件，然后使用 `git rm <file>` 和 `git add<file>` 效果是一样的。*

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git restore test.txt
```

`git restore` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

*! 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！*

## 3 远程仓库

使用 Github 充当远程仓库，在 Github 的设置中上传 SSH 公钥。

### 3.1 添加远程库

登陆 GitHub，然后，在右上角 “+” 找到 “New repository” 按钮，创建一个新的仓库：

根据 GitHub 的提示，在本地的`learngit`仓库下运行命令，将本地仓库与远程仓库关联：

##### @ `git remote add` 添加远程库

```shell
$ git remote add origin https://github.com/CharlesLevis/Git.git
```

下一步，就可以把本地库的所有内容推送到远程库上：

##### @ `git push` 远程推送

```shell
$ git push -u origin master
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送到远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

```shell
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

