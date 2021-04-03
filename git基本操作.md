### git的基本操作

#### 1. 创建版本库

git创建版本库要先在一个选择好的工作目录下输入命令<font color='red'>`git init`</font>，（如果是第一次使用git，还需要设置用户名和邮箱命令是<font color='red'>`  git config --global user.email "you@example.com"`</font>和<font color='red'>`git config --global user.name "Your Name"`</font>）该命令会在当前目录下创建一个`.git`的文件夹，这是个隐藏文件，建议不要去碰这个文件夹。

#### 2. 生成新版本

git生成一个文件的新版本有两个步骤，第一个是`git add <fliename>`，将文件存入缓冲区（暂存区），第二个步骤是`git commit -m "备注"`将文件提交至分支，生成新版本。

#### 3. 获取修改内容

git管理下文件被修改后会有记录（被add过在暂存区的文件才会有）

我们随时随刻可以用`git status`来查看git的状态

例如新建一个文件`gittext.txt`，然后生成新版本

```
xyc@click-machine:~/work/notegit$ touch gits.txt
xyc@click-machine:~/work/notegit$ vi gits.txt
xyc@click-machine:~/work/notegit$ cat gits.txt
This is a File
xyc@click-machine:~/work/notegit$ git status
位于分支 master

尚无提交

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	gits.txt

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）

```

建好文件后保存然后查看git状态，发现提示“提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）”这说明要获取修改内容的前提是这个文件被add过，所以下面add该文件。

```
xyc@click-machine:~/work/notegit$ git add gits.txt
```

此时虽然此时没有提示，但是这也说明他添加从成功了因为` No news is the best news.`

如果我们这时修改这个文件为

```
xyc@click-machine:~/work/notegit$ cat gits.txt
This is a Fil
a new line
```

然后再查看git状态

```
xyc@click-machine:~/work/notegit$ git status
位于分支 master

尚无提交

要提交的变更：
  （使用 "git rm --cached <文件>..." 以取消暂存）

	新文件：   gits.txt

尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     gits.txt
```

已经检测到变更，但是提示我的变更并未放入暂存（没有add）

然后我们可以用 <font color='red'>`git diff <filename>`</font>来获取变更内容

```
xyc@click-machine:~/work/notegit$ git diff gits.txt
diff --git a/gits.txt b/gits.txt
index 2c3b38e..917f566 100644
--- a/gits.txt
+++ b/gits.txt
@@ -1 +1,2 @@
-This is a File
+This is a Fil
+a new line 
```

上面详细地给出了变更内容

#### 4. 提交版本&暂存区&工作区

上面多次提到暂存区，那么什么是暂存区呢，暂存区存在于`.git`文件里面，工作区就是我们选择的用`git init` 的根目录，就拿刚刚的例子，我修改了那个文件，那个文件修改前的内容存在暂存区，修改后的存在工作区，提交版本就是提交暂存区的内容到分支。

提交版本的命令为<font color='red'>`git commit -m "备注"`</font>，假如我现在提交版本的话，就是旧版本被提交，而不是修改后的版本。

我们可以通过<font color='red'>`git log`</font>命令来获取提交的版本，还可以通过<font color='red'>`git reset  --hard  版本号的一部分`</font>来选择版本，例如我现在提交版本

```
xyc@click-machine:~/work/notegit$ git commit -m "commit old version"
[master （根提交） da220bd] commit old version
 1 file changed, 1 insertion(+)
 create mode 100644 gits.txt
xyc@click-machine:~/work/notegit$ git log
commit da220bd118e908ae848d9e23b77dad833b1235e7 (HEAD -> master)
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 19:58:24 2021 +0800

    commit old version
xyc@click-machine:~/work/notegit$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     gits.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

可以发现工作区的文件还在提示没有提交到暂存区，此时我们将工作区修改后的内容add到暂存区再查看git状态。

```
xyc@click-machine:~/work/notegit$ git add gits.txt
xyc@click-machine:~/work/notegit$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     gits.txt

xyc@click-machine:~/work/notegit$ 
```

现在的状态又在提示已经到了暂存区，但是还没有提交，正准备提交。

现在我们提交它。

```
xyc@click-machine:~/work/notegit$ git commit -m "The Second Commit"
xyc@click-machine:~/work/notegit$ git log
commit 50d8af4d3609b7ab28d5c7941a2862b7bfdb3ab6 (HEAD -> master)
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 20:08:54 2021 +0800

    The Second Commit

commit da220bd118e908ae848d9e23b77dad833b1235e7
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 19:58:24 2021 +0800

    commit old version
```

现在我们成功把新版本提交到了分支上。

这时我们再看git的状态。

```
xyc@click-machine:~/work/notegit$ git status
位于分支 master
无文件要提交，干净的工作区
```

#### 5. 版本回退，找到以前的版本

命令<font color='red'>`git reset  --hard 版本号的一部分`</font>

嫌麻烦也可以用<font color='red'>`git reset  --hard HEAD^`</font>

其中`^`号一个就是回退一个版本，`^^`就是回退两个版本以此类推

当我们提交了多个版本，有想要回退或者前进到某一个版本时我们可以用上述命令

```
xyc@click-machine:~/work/notegit$ git log
commit 50d8af4d3609b7ab28d5c7941a2862b7bfdb3ab6 (HEAD -> master)
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 20:08:54 2021 +0800

    The Second Commit

commit da220bd118e908ae848d9e23b77dad833b1235e7
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 19:58:24 2021 +0800

    commit old version
```

上面`commit`后面跟的奇奇怪怪的就是`commit ID`也就是版本号。版本号后面跟了一个括号，那个就是 表示当前的版本。

分支版本回退以后工作区的文件也会自动版本回退。

那么又出现了一个新的问题，我今天晚上把版本回退到以前了，然后我把电脑关了，第二天觉得不应该退，还是不退好，但是我又不知道版本号了怎么办。

在git中给我们提供了一个指令<font color=red>`git reflog`</font>用来查询每一次的命令。

```html
xyc@xzalinux:~/work/notegit$ git log
commit 50d8af4d3609b7ab28d5c7941a2862b7bfdb3ab6 (HEAD -> master)
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 20:08:54 2021 +0800

    The Second Commit

commit da220bd118e908ae848d9e23b77dad833b1235e7
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 19:58:24 2021 +0800

    commit old version
xyc@xzalinux:~/work/notegit$ git reset --hard da22
HEAD 现在位于 da220bd commit old version
xyc@xzalinux:~/work/notegit$ git log
commit da220bd118e908ae848d9e23b77dad833b1235e7 (HEAD -> master)
Author: X_X <750133671@qq.com>
Date:   Fri Apr 2 19:58:24 2021 +0800

    commit old version
    
    
    
xyc@xzalinux:~/work/notegit$ git reflog
da220bd (HEAD -> master) HEAD@{0}: reset: moving to da22
50d8af4 HEAD@{1}: reset: moving to 50d8a
50d8af4 HEAD@{2}: commit: The Second Commit
da220bd (HEAD -> master) HEAD@{3}: commit (initial): commit old version
xyc@xzalinux:~/work/notegit$ 
```

可以看到版本更迭。

#### 6. 撤销修改

每次当你修改了工作区文件，但是你还没有add到缓存区，你直接用`git status`命令查看git状态的时候，你仔细看看下面的提示。

```html
xyc@xzalinux:~/work/notegit$ ls
gits.txt
xyc@xzalinux:~/work/notegit$ vi gits.txt
xyc@xzalinux:~/work/notegit$ cat gits.txt
This is a Fil
a new line
撤销修改
xyc@xzalinux:~/work/notegit$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     gits.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

可以看到**（使用 "git checkout -- <文件>..." 丢弃工作区的改动）**这一行，丢弃改动，相当于是手动撤回。

我在文档中加了一行写上了"撤销修改"四个字。

当我使用<font color=red>`git checkout -- filename`</font>这个命令时，就会撤销工作区的改动。

```html
xyc@xzalinux:~/work/notegit$ git checkout -- gits.txt
xyc@xzalinux:~/work/notegit$ git status
位于分支 master
无文件要提交，干净的工作区
xyc@xzalinux:~/work/notegit$ cat gits.txt
This is a Fil
a new line
```

还有一种就是你改错了并且已经add到了缓存区例如这样

```html
xyc@xzalinux:~/work/notegit$ cat gits.txt
This is a Fil
a new line
撤销更改2
xyc@xzalinux:~/work/notegit$ git add gits.txt
xyc@xzalinux:~/work/notegit$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     gits.txt
```

我们看到他又给出了提示**（使用 "git reset HEAD <文件>..." 以取消暂存）**可以取消暂存，当我们用这个命令取消暂存之后再看git状态会是什么样子的呢？

```html
xyc@xzalinux:~/work/notegit$ git reset HEAD gits.txt
重置后取消暂存的变更：
M	gits.txt


xyc@xzalinux:~/work/notegit$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     gits.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）

    
```

就变得跟刚刚说的第一种情况一样了，就可以按照上面的方法继续撤回修改。



#### 7. 关于删除文件

准备工作（新建一个待删文件然后提交）：

```
xyc@xzalinux:~/work/notegit$ touch del.txt
xyc@xzalinux:~/work/notegit$ vi del.txt
xyc@xzalinux:~/work/notegit$ cat del.txt
This is a file ready to be delete

xyc@xzalinux:~/work/notegit$ git add del.txt
xyc@xzalinux:~/work/notegit$ git commit -m "delFile"
[master 967925c] delFile
 1 file changed, 2 insertions(+)
 create mode 100644 del.txt
```

现在我们删除它

```
xyc@xzalinux:~/work/notegit$ rm del.txt
xyc@xzalinux:~/work/notegit$ ls
gits.txt
```

现在它已经被删除了，然后我们用`git status`来查看git现阶段的状态。

```
xyc@xzalinux:~/work/notegit$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     del.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

有没有发现很熟悉，其实删除本身就是对文件的一种修改