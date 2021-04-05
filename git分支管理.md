### 1. 分支的的创建与合并删除

版本提交是一条直线，有一条主分支`master`指向的分支就是一条主分支，`HEAD`指向的就是`master`，每次提交，`master`都会向前移动一个，但是当我们不想在主分支上工作，就可以创立一个新的分支，然后工作完成再合并到主分支上面，那`master`直接指向执行了合并命令的分支，就完成了分支的合并。

#### 1.1. 分支的创建切换

<font color=red>`git branch 分支名`</font>可以创建新的分支。

<font color=red>`git checkout 分支名`</font>可以切换分支。

<font color=red>`git checkout -b 分支名`</font>可以创建新的分支并且切换到新建的分支。

创建一个分支"dev"并切换

```
xza@xzalinux:~/work/notegit$ git branch dev
```

```
xza@xzalinux:~/work/notegit$ git checkout dev
```

或者

```
xza@xzalinux:~/work/notegit$ git checkout -b dev
```

在新分支里面新建一个文件dev.txt并写入内容

```
xza@xzalinux:~/work/notegit$ touch dev.txt
xza@xzalinux:~/work/notegit$ vi dev.txt
xza@xzalinux:~/work/notegit$ cat dev.txt
This is a text only in "dev" branch.
```

然后再把这个分支提交

```
xza@xzalinux:~/work/notegit$ git add dev.txt
xza@xzalinux:~/work/notegit$ git commit -m "Linux@dev.txt"
[dev e0813bc] Linux@dev.txt
 1 file changed, 1 insertion(+)
 create mode 100644 dev.txt
```

用ls命令看看当前目录下的文件

```
xza@xzalinux:~/work/notegit$ ls
dev.txt  gits.txt  ok.txt
```

再切换回`master`分支

```
xza@xzalinux:~/work/notegit$ git checkout master
切换到分支 'master'
您的分支与上游分支 'origin/master' 一致。
```

再看看目录下的文件

```
xza@xzalinux:~/work/notegit$ ls
gits.txt  ok.txt
```

发现没有dev.txt文件，分支提交后不影响主分支。

#### 1.2. 分支的合并

合并分支的命令是<font color=red>`git merge 分支名`</font>，把命令中的分支合并到当前分支。

把上面那个例子的dev分支合并到master分支下

```
xza@xzalinux:~/work/notegit$ git merge dev
更新 d99e481..e0813bc
Fast-forward
 dev.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 dev.txt
```

再用ls命令看当前目录下的文件

```
xza@xzalinux:~/work/notegit$ ls
dev.txt  gits.txt  ok.txt
```

dev.txt就出来了

#### 1.3. 分支的删除

当一个分支提交后，这个分支就可以删除了

命令<font color=red>`git branch -d 分支名`</font>

查看当前所有分支的命令<font color=red>`git branch`</font>

查看当前分支信息

```
xza@xzalinux:~/work/notegit$ git branch
  dev
* master
```

删除dev分支

```
xza@xzalinux:~/work/notegit$ git branch -d dev
已删除分支 dev（曾为 e0813bc）。
```

再看分支信息

```
xza@xzalinux:~/work/notegit$ git branch
* master
```

dev分支已经成功删除了

### 2. 解决分支的冲突

假如现在出现了这种情况：

建立一个新分支dev然后切换回master分支再修改dev.txt内容为

```
master!
```

然后再提交，再切换回dev分支修改dev.txt内容为

```
dev!
```

然后再提交

再把dev分支合并上master分支上面去

```
xza@xzalinux:~/work/notegit$ git branch dev
xza@xzalinux:~/work/notegit$ vi dev.txt
xza@xzalinux:~/work/notegit$ cat dev.txt
master!
xza@xzalinux:~/work/notegit$ git add .
xza@xzalinux:~/work/notegit$ git commit -m "linux-master@change dev.txt"
[master 84c4d7a] linux-master@change dev.txt
 1 file changed, 1 insertion(+), 1 deletion(-)
xza@xzalinux:~/work/notegit$ git checkout dev
切换到分支 'dev'
xza@xzalinux:~/work/notegit$ vi dev.txt
xza@xzalinux:~/work/notegit$ cat dev.txt
dev!
xza@xzalinux:~/work/notegit$ git add .
xza@xzalinux:~/work/notegit$ git commit -m "linux-dev@change dev.txt"
[dev 256af04] linux-dev@change dev.txt
 1 file changed, 1 insertion(+), 1 deletion(-)
xza@xzalinux:~/work/notegit$ git checkout master
切换到分支 'master'
您的分支领先 'origin/master' 共 2 个提交。
  （使用 "git push" 来发布您的本地提交）
xza@xzalinux:~/work/notegit$ git merge dev
自动合并 dev.txt
冲突（内容）：合并冲突于 dev.txt
自动合并失败，修正冲突然后提交修正的结果。

```

可以看到合并失败了，这是因为两方都修改了同一个文件

我们可以先看看文件内容

xza@xzalinux:~/work/notegit$ cat dev.txt
```
xza@xzalinux:~/work/notegit$ cat dev.txt
<<<<<<< HEAD
master!
=======
dev!
>>>>>>> dev
```

git用\<\<\<\<和\>\>\>\>来标识了两种不同的文件

这个时候只需要修改这个文件再提交合并就行了。

```
xza@xzalinux:~/work/notegit$ vi dev.txt
	add
xza@xzalinux:~/work/notegit$ git add .
xza@xzalinux:~/work/notegit$ git commit -m "Linux_master@sameFile"
[master fd72f64] Linux_master@sameFile
xza@xzalinux:~/work/notegit$ git merge dev
已经是最新的。
xza@xzalinux:~/work/notegit$ cat dev.txt
同样的内容
```

### 3. 分支的提交和更新

每次从远程仓库克隆或者下载信息的的时候，除了master分支以外的分支是不会被克隆和上传的。

如果要上传的话可以用命令<font color=red>`git push origin 分支名`</font>

或者切换到想要上传的分支然后用命令<font color=red>`git push`</font>

继续上面的例子，我要上传dev分支

```
xza@xzalinux:~/work/notegit$ git checkout dev
切换到分支 'dev'
xza@xzalinux:~/work/notegit$ git push
fatal: 当前分支 dev 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

    git push --set-upstream origin dev
```

由于是第一次上传，我的dev没有和远程仓库的任何一个分支对应起来，这时候我们可以使用命令

<font color=red>`git push --set-upstream origin 远程仓库分支名`</font>

来将本地仓库分支和远程仓库分支关联起来（如果远程仓库没有这个分支的话会先建立一个远程分支）

现在我们将这个添加上去

```
xza@xzalinux:~/work/notegit$ git push --set-upstream origin dev
Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'dev' on GitHub by visiting:
remote:      https://github.com/Find-ing/X_X/pull/new/dev
remote: 
To github.com:Find-ing/X_X.git
 * [new branch]      dev -> dev
分支 'dev' 设置为跟踪来自 'origin' 的远程分支 'dev'。
```

现在就成功在远端建立了一个dev分支并且和本地的dev分支关联起来。

现在就能push了

```
xza@xzalinux:~/work/notegit$ git push
对象计数中: 3, 完成.
Delta compression using up to 2 threads.
压缩对象中: 100% (2/2), 完成.
写入对象中: 100% (3/3), 375 bytes | 375.00 KiB/s, 完成.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:Find-ing/X_X.git
   fd72f64..ad51f7d  master -> master
```

在本地创建和远程分支对应的分支

<font color=red>`git checkout -b branch-name origin/branch-name`</font>

建立本地分支和远程分支的关联

<font color=red>`git branch --set-upstream branch-name origin/branch-name`</font>

例如我现在换一台电脑，我刚刚上传了dev分支，我想把这个分支拷贝到本地mster分支上面，由于更新不更新分支内容，我就可以用到上述命令