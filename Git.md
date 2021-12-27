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

#### ## 小结 ##

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

### 3.2 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

##### @ `git remote -v` 查看远程库

```shell
$ git remote -v
origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
```

##### @ `git remote rm` 删除远程库

然后，根据名字删除，比如删除`origin`：

```shell
$ git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

+++

#### ## 小结 ##

+ 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

+ 关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

+ 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

+ 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 3.3 从远程库克隆

##### @ `git clone` 克隆远程库

克隆到本地的库自动与原远程库关联。

## 4 分支管理

——平行宇宙

### 4.1 创建与合并分支

在版本回退里，已经知道，每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在 Git 里，这个分支叫主分支，即 `master` 分支。`HEAD` 严格来说不是指向提交，而是指向 `master`，`master` 才是指向提交的，所以，`HEAD` 指向的就是当前分支。

#### 4.1.1 原理

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

 ![git-br-initial](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0.png)

每次提交，`master`分支都会向前移动一步，这样，随着不断提交，`master`分支的线也越来越长。

当创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](C:\Users\Lab1119\OneDrive\Markdown Notes\img\l.png) 

Git 创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](C:\Users\Lab1119\OneDrive\Markdown Notes\img\l-16403363675223.png) 

假如在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？**最简单的方法，就是直接把`master`指向`dev`的当前提交**，就完成了合并：

![git-br-ff-merge](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16403363675224.png) 

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，就剩下了一条`master`分支：

![git-br-rm](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16403363675225.png) 

#### 4.1.2 实战

首先，创建`dev`分支，然后切换到`dev`分支：

##### @ `git check` 创建/切换分支

```shell
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout` 命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```shell
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

实际上，切换分支这个动作，用`switch`更科学。因此，新版本的Git提供了新的`git switch`命令来切换分支：

##### @ `git switch` 创建/切换分支

创建并切换到新的`dev`分支，可以使用：

```shell
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```shell
$ git switch master
```

然后，用`git branch`命令查看当前分支：

##### @ `git branch` 查看分支

```shell
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```
Creating a new branch is quick.
```

然后提交：

```shell
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```shell
$ git checkout master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16403369147759.png) 

现在，我们把`dev`分支的工作成果合并到`master`分支上：

##### @ `git merge` 合并分支

```shell
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git 告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

##### @ `git branch -d` 删除分支

```shell
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```shell
$ git branch
* master
```

因为创建、合并和删除分支非常快，所以 Git 鼓励使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

+++

#### ## 小结 ##

Git 鼓励使用分支：

+ 查看分支：`git branch`

+ 创建分支：`git branch <name>`

+ 切换分支：`git checkout <name>`或者`git switch <name>`

+ 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

+ 合并某分支到当前分支：`git merge <name>`

+ 删除分支：`git branch -d <name>`

### 4.2 解决冲突

合并分支并不会一直一帆风顺，也可能会产生冲突。

准备新的`feature1`分支，继续我们的新分支开发：

```shell
$ git switch -c feature1
Switched to a new branch 'feature1'
```

修改`readme.txt`最后一行，改为：

```
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交：

```shell
$ git add readme.txt

$ git commit -m "AND simple"
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支：

```shell
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。

在`master`分支上把`readme.txt`文件的最后一行改为：

```
Creating a new branch is quick & simple.
```

提交：

```shell
$ git add readme.txt 
$ git commit -m "& simple"
[master 5dc6824] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![git-br-feature1](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0.png) 

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```shell
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

果然冲突了！Git 告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。**`git status`也可以告诉我们冲突的文件**：

```shell
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看 readme.txt 的内容：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Creating a new branch is quick and simple.
```

再提交：

```shell
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16405892526312.png) 

用`git log --graph`命令可以看到分支合并图：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
```

看完后，**按 `q` 键退出**。

最后，删除`feature1`分支：

```shell
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

工作完成。

***！注意：**同一个文件，只要多个分支不在同一个位置改动，就不会产生冲突，可以成功合并。*

---

#### ## Untracked files 解决

随时可能出现的意想不到的问题——**Untracked files（未监控）的文件**

在编译git库拉下来的代码时，往往会产生一些中间文件，这些文件我们根本不需要，尤其是在成产环节做预编译，检查代码提交是否能编译通过这种case时，我们往往需要编译完成后不管正确与否，还原现场，以方便下次sync代码时不受上一次的编译影响。

+ **删除 untracked files**

```shell
git clean -f
```

+ **连 untracked 的目录也一起删掉**

```shell
git clean -fd
```

+ **连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）**

```shell
git clean -xfd
```

+ **在用上述 git clean 前，强烈建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删**

```shell
git clean -nxfd
git clean -nf
git clean -nfd
```

### 4.3 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用 `Fast forward` 模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```shell
$ git switch -c dev
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```shell
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```shell
$ git switch master
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```shell
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16405922990734.png) 

#### ## 分支管理原则

在实际开发中，我们应该按照几个基本原则进行分支管理：

+ 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

+ 干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

+ 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](C:\Users\Lab1119\OneDrive\Markdown Notes\img\0-16405924834976.png) 

### 4.4 Bug 分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```shell
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git 还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

##### @ `git stash` 备份工作区

```shell
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```shell
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，需要把 “Git is free software ...” 改为 “Git is a free software ...”，然后提交：

```shell
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```shell
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```shell
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

##### @ `git stash list` 工作区备份列表

```shell
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

##### @ `git stash apply` 恢复工作区

##### @ `git stash drop` 删除工作区备份

##### @ `git stash pop` 恢复工作区并删除备份

```shell
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```shell
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```shell
$ git stash apply stash@{0}
```

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```shell
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

#### ## 小结

+ 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

+ 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

+ 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

### 4.5 Feature 分支

开发一个新 feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 4.6 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```shell
$ git remote
origin
```

或者，用`git remote -v`显示更详细的信息：

```shell
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

上面显示了可以抓取和推送的 `origin` 的地址。如果没有推送权限，就看不到 push 的地址。

#### 4.6.1 推送分支

多人协作时，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- `bug` 分支只用于在本地修复 bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个 bug；
- `feature` 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

#### 4.6.2 抓取分支

##### @ `git pull` 抓取远程分支

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

#### ## 小结

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

### 4.7 Rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 5 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签是版本库的一个快照，也是指向某个 commit 的指针。

标签 tag 需要取一个让人容易记住的有意义的名字，它跟某个 commit 绑在一起，方便开发人员快速查找。

### 5.1 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```shell
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

然后，敲命令`git tag <name>`就可以打一个新标签：

##### @ `git tag <name>` 创建标签

```shell
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

##### @ `git tag` 查看标签

```shell
$ git tag
v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```shell
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```shell
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```shell
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

##### @ `git show <tagname>` 查看标签信息

```shell
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，**用`-a`指定标签名，`-m`指定说明文字**：

```shell
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

用命令`git show <tagname>` 可以看到说明文字：

```shell
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
...
```

*！ 注意：标签总是和某个 commit 挂钩。如果这个 commit 既出现在 master 分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。*

#### ## 小结

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个 commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

### 5.2 操作标签

如果标签打错了，也可以删除：

##### @ `git tag -g` 删除标签

```shell
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```shell
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```shell
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```shell
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```

然后，从远程删除。删除命令也是push，但是格式如下：

```shell
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

#### ## 小结

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

