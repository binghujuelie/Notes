[toc]

## 初始化版本库

```
git init 
```

## 添加文件

把一个文件放到Git仓库只需要两步
readme.txt内容如下：

> Git is a version control system.
> Git is free software.
> 放在上文git init的目录下.
> 第一步，用命令git add告诉Git，可反复多次使用，添加多个文件,把文件添加到仓库：

```
$ git add readme.txt
```

第二步，用命令git commit告诉Git，把文件提交到仓库,commit可以一次提交很多文件：

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的readme.txt文件）；2 insertions：插入了两行内容（readme.txt有两行）

## 时空穿梭

### 修改历史查询

修改readme.txt文件

改成如下内容：

`Git is a distributed version control system. `

`Git is free software.`

运行git status命令看看结果

```
E:\test>git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

运行 `git diff`查看当前仓库状态。

```
E:\test>git diff
diff --git a/readme.txt b/readme.txt
index 07fc819..b18bfcc 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system
+Git is a distributed version control system.^M
 Git is free software.
\ No newline at end of file
```

可以从上面的命令输出看到，我们在第一行添加了一个distributed单词。

提交修改和提交新文件是一样的两步.

### 版本回退

用 `git log`命令查看历史记录
注：
和SVN不一样，Git的commit id不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示。

Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

现在，我们要把当前版本append GPL回退到上一个版本add distributed，就可以使用 `git reset`命令,版本号没必要写全，前几位就可以了，Git会自动去找：

```
E:\test>git reset --hard 4439d
HEAD is now at 4439dd9 add distributed
```

想恢复到新版本怎么办？找不到新版本的commit id怎么办？
Git提供了一个命令git reflog用来记录你的每一次命令：

```
E:\test>git reflog
4439dd9 (HEAD -> master) HEAD@{0}: reset: moving to 4439d
daffbfd HEAD@{1}: commit: append GPL
4439dd9 (HEAD -> master) HEAD@{2}: commit: add distributed
0c1e964 HEAD@{3}: commit (initial): write a readme file
```

我们可以知道append GPL的commit id是daffbfd

### 工作区和暂存区

当前我的 `E:\test`就是工作区

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

`git status`:

作两次对比，工作区 vs 暂存区 以及 暂存区 vs 仓库，并将两次对比的结果显示在输出中。哪个对比没差别，就不显示。两个对比都没差别，就显示working tree clean。

`git diff`：

1. git diff：是查看working tree与index的差别的。
2. git diff --cached：是查看index与repository的差别的。
3. git diff HEAD：是查看working tree和repository的差别的。其中：HEAD代表的是最近的一次commit的信息。

### 管理修改

Git跟踪并管理的是修改，而非文件
git add放在暂存区之后，如果修改相应文件，应该重新git add 再git commit -m。否则修改的内容不会被提交。

### 撤销修改

`git checkout --file`可以丢弃工作区的修改
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

命令git reset HEAD `<file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

### 删除文件

一、确实要从版本库中删除该文件，那就用命令 `git rm`删掉，并且 `git commit`：

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

二、删错了，用git checkout恢复

## 远程仓库

github创建SSH KEY

### 添加远程库

先在github上创建一个Git仓库，然后根据github上的提示，在本地仓库运行命令

```
E:\test>git remote add origin git@github.com:winfredliu/learngit.git
```

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
下一步，就可以把本地库的所有内容推送到远程库上：

```
E:\test>git push -u origin main
```

把本地库的内容推送到远程，用 `git push`命令，实际上是把当前分支main推送到远程。

由于远程库是空的，我们第一次推送main分支时，加上了-u参数，Git不但会把本地的main分支内容推送的远程新的main分支，还会把本地的main分支和远程的main分支关联起来，在以后的推送或者拉取时就可以简化命令。
从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin main
```

把本地main分支的最新修改推送至GitHub.

#### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm `<name>`命令。使用前，建议先用git remote -v查看远程库信息，然后，根据名字删除，比如删除origin：

```
git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

### 从远程库克隆

用 `git clone`把远程库克隆到本地库

```
git clone git@github.com:winfredliu/learngit.git
```

GitHub给出的地址不止一个，还可以用https://github.com/winfredliu/learngit.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。

使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https

## 分支管理

首先，我们创建dev分支，然后切换到dev分支：

```
E:\test>git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上-b参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
```

然后，用 `git branch`命令查看当前分支：

```
E:\test>git branch
* dev
  main
```

git branch命令会列出所有分支，当前分支前面会标一个*号。
现在，我们把dev分支的工作成果合并到main分支

```
E:\test>git merge dev
Updating f92c8e5..a7abaa2
Fast-forward
 readme.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```

`git merge`命令用于合并指定分支到当前分支.
注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把main指向dev的当前提交，所以合并速度非常快。

#### 删除本地分支

合并完成后，就可以放心地删除dev分支了，`git branch -d dev`是删除指令：

```
E:\test>git branch -d dev
Deleted branch dev (was a7abaa2).
```

我们注意到切换分支使用git checkout `<branch>`.实际上，切换分支这个动作，用switch更科学。因此，最新版本的Git提供了新的 `git switch`命令来切换分支：

#### 删除远程分支

指令：`git push <远程主机名> --detete <删除分支名>`
删除出现如下错误：

```
E:\test>git push origin --delete main
To github.com:winfredliu/learngit.git
 ! [remote rejected] main (refusing to delete the current branch: refs/heads/main)
error: failed to push some refs to 'github.com:winfredliu/learngit.git'
```

这是因为在我们的远程仓库中main是默认分支，所以无法删除。所以我们需要将远程仓库中的默认分支改为另外的分支。

1. 进入github的远程仓库，点击如下界面中的setting
2. 进入如下界面，再点击branch
3. 设置branch中的default branch，更改为main之外的分支，这里我设置的master，然后点击一下update后确认。
   然后再删除。

创建并切换到新的dev分支，可以使用：

```
git switch -c dev
```

直接切换到已有的master分支，可以使用

```
git switch master
```

### 解决冲突

参考链接：[解决冲突](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344 "廖雪峰的教程-解决分支冲突问题")

简要来说，合并分支时，每个分支工作区的文件如果不一致的话就会产生冲突。这个时候要使工作区文件相同。

### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下--no-ff方式的git merge：

首先，仍然创建并切换dev分支,修改readme.txt文件，并提交一个新的commit,现在，我们切换回main.

准备合并dev分支，请注意 `--no-ff`参数，表示禁用 `Fast forward`：

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

#### 分支历史变迁

合并后，我们用git log看看分支历史

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
```

![merge图](https://s3.bmp.ovh/imgs/2022/01/b7954a837df7139d.png "分支可视图")

### BUG分支

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支 `issue-101`来修复它，但是，等等，当前正在 `dev`上进行的工作还没有提交：
git提供了一个 `git stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
git stash
```

现在，用 `git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在main分支上修复，就从main创建临时分支 `issue-101`。

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交

```
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到main分支，并完成合并，最后删除 `issue-101`分支.

修复完BUG后，是时候接着回到 `dev`分支干活了！

工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

```
E:\test>git stash list
stash@{0}: WIP on main: bd67996 AND simple
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用 `git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用 `git stash drop`来删除；

另一种方式是用 `git stash pop`，恢复的同时把stash内容也删了：

举例：

```
E:\test>git stash pop
Removing test.txt
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        img/

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (e4765582cca89fa0c52a11ccb8a254d504b3643b)
```

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

```
git stash apply stash@{0}
```

在main分支上修复了bug后，我们要想一想，dev分支是早期从main分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在dev上修复，我们只需要把 `4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制 `4c805e2 fix bug 101`这个提交所做的修改，并不是把整个main分支merge过来。

为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支

```
git cherry-pick 4c805e2
```

当然要确保当前分支在dev上。

### Feature分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

```
git switch -c feature-1
git add 某个文件
git commit -m ""
git switch 起始分支
```

就在此时，接到上级命令，因经费不足，新功能必须取消！

Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。。

```
git branch -D feature-1
```

### 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的分支和远程的m分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用 `git remote`:

```
E:\test>git remote
origin
```

或者，用 `git remote -v`显示更详细的信息：

```
E:\test>git remote -v
origin  git@github.com:winfredliu/learngit.git (fetch)
origin  git@github.com:winfredliu/learngit.git (push)
```

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
git push origin main
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- master分支是主分支，因此要时刻与远程同步；
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

#### 抓取分支

人协作时，大家都会往master和dev分支上推送各自的修改。
当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。
现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

```
git checkout -b dev origin/dev
```

![这是图片](https://i.bmp.ovh/imgs/2022/01/fc36a49ada8f0283.png)
注：如上图所示，你test目录下clone，但管理该项目应该在learngit目录下

现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程。你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送。

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用 `git pull`把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：
git pull前，应该指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

```git
git pull --rebase origin master
```

```
git branch --set-upstream-to=origin/dev dev
```

再pull.

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push.

```
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

### Rebase

Git有一种称为rebase的操作，有人把它翻译成“变基”。

我们输入命令 `git rebase`,原本分叉的提交现在变成一条直线了.这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观

## 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

commit号不如tag直观。

### 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上,然后，敲命令 `git tag <name>`就可以打一个新标签：

```
E:\test>git tag v1.0
```

可以用命令 `git tag`查看所有标签：

```
E:\test>git tag
v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```
E:\test>git log --pretty=oneline --abbrev-commit
bd67996 (HEAD -> master, tag: v1.0, origin/master, origin/dev, feature1, dev) AND simple
a7abaa2 branch test
f92c8e5 add test.txt
4439dd9 add distributed
0c1e964 write a readme file
```

比方说要对 `add distributed`这次提交打标签，它对应的 `commit id`是 `4439dd9`，敲入命令：

```
E:\test>git tag v0.9 4439dd9
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用 `git show <tagname>`查看标签信息：

```
E:\test>git show v0.9
commit 4439dd984e2d83ff7c7a70df6d0519eef2600669 (tag: v0.9)
Author: winfredliu <3194769362@qq.com>
Date:   Thu Jan 13 18:31:47 2022 +0800

    add distributed

diff --git a/readme.txt b/readme.txt
index 07fc819..b18bfcc 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system
+Git is a distributed version control system.^M
 Git is free software.
\ No newline at end of file
```

还可以创建带有说明的标签，用 `-a`指定标签名，`-m`指定说明文字：

```
E:\test>git tag -a v0.1 -m "version 0.1 released" 0c1e964
```

用命令 `git show <tagname>`可以看到说明文字:

```
tag v0.1
Tagger: winfredliu <3194769362@qq.com>
Date:   Tue Jan 18 20:11:52 2022 +0800

version 0.1 released

commit 0c1e9644bed1817dc3671747fd8da4825387bd9a (tag: v0.1)
Author: winfredliu <3194769362@qq.com>
Date:   Thu Jan 13 18:07:40 2022 +0800

    write a readme file
.......
```

注：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

### 操作标签

如果标签打错了，也可以删除：

```
E:\test>git tag -d v0.1
Deleted tag 'v0.1' (was 2acd0d6)
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

#### 推送标签到远程

如果要推送某个标签到远程，使用命令 `git push origin <tagname>`：

```
E:\test>git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:winfredliu/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```
E:\test>git push origin --tags
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:winfredliu/learngit.git
 * [new tag]         v0.9 -> v0.9
```

#### 删除远程标签

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```
E:\test>git tag -d v0.9
Deleted tag 'v0.9' (was 4439dd9)
```

然后，从远程删除。删除命令也是push，但是格式如下：

```
E:\test> git push origin :refs/tags/v0.9
To github.com:winfredliu/learngit.git
 - [deleted]         v0.9
```

## Github删除仓库

如果我们想要删除Github中没有用的仓库，应该如何去做呢？

进入到我们需要删除的仓库里面，找到“settings”即仓库设置,
然后，在仓库设置里拉到最底部，找到“Danger Zone”即危险区域,
点击“Delete this repository”这样就可以删除该仓库了。

## 重命名某个文件

比如将`readme.md`改为`README.md`要先`git add`,`git commit` `readme.md`；再`git add`,`git commit` `README.md`

## git add 命令

我们可以使用命令git add将所加入的文件添加进来

`git add .`将仓库文件夹中的内容都添加进来

`git add *.`文件类型//添加指定文件类型的文件

## Git报错“*（no branch, rebasing master）”

本次出现这个错误是因为本地提交了`commit`但是未`push`成功，所以使用`git pull --rebase`，由于远程仓库和本地的`commit`有冲突，Git无法自动解决冲突时，会切换到一个匿名分支，然后使用`git branch`发现报错`“no branch, rebasing master”`。

解决办法：

在当前匿名分支下，解决完冲突，然后使用命令`git rebase --continue`，可以将代码合并到之前操作时的分支（我当时是在master分支所以后面都说master分支），执行完`git rebase --continue`后就自动回到了`master`分支，但是之前提交的commit没有在本地仓库区(`Repository`)了，回到了本地暂存区(Index / Stage)，这时候需要重新`commit`，之后再执行`git push origin master`就可以将变更推到远程仓库了。
