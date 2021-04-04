### 1. 连接远程仓库

 #### 1.1. 创建仓库

在连接远程仓库之前，得先要确定你有一个远程仓库，到<a href="github.com">GitHub官网</a>搞一个账户。

点右上角的加号然后“New repository”输入一个仓库名字然后其余全部默认就行。

#### 1.2. 创建SSH Key

在用户主目录下找一个`.ssh`的文件，如果没有的话就需要创建一个，打开命令行输入<font color=red>`ssh-keygen -t rsa -C "你的邮箱"`</font>，把它替换成你的邮箱就行，然后也不需要设置密码，一直默认回车。

#### 1.3. 将SSH Key导入仓库

于你的本地Git仓库和GitHub远程仓库之间的传输是通过SSH加密的，远程仓库为了识别身份就需要识别SSH密钥，上一步创建的`.ssh`文件目录下就两个文件，分别是`id_rsa.pub`和`id_rsa`，其中前者是公钥，后者是私钥，你把公钥给GitHub远程仓库，自己留着私钥，就能跟GitHub远程仓库加密通话了，把公钥交给GitHub账户呢？

在GitHub中打开设置，点"SSH and GPG keys"，然后就会看到"New SSH key"，点一下，随便取一个标题，再下面的框框中填上`id_rsa.pub`的内容（右键打开方式选记事本就行），点了确定就行。

这样就成功的把SSH Key导入到Git账户里面了，就可以跟远程账户加密通话了。

#### 1.4.把远程仓库和本地仓库连接起来

再自己仓库的主目录下输入命令

<font color=red>`git remote add origin git@github.com:<用户名>/<仓库名>.git`</font>

以上命令`origin`是远程库名，也可以改成别的，但是这个是默认的，一看就知道是远程库，学习阶段没有改的必要。

`<用户名>`就是注册用户的时候输入的用户名。

`<仓库名>`就是1.1中建立仓库的时候输入的仓库名。

如果地址写错了，关联错了，或者干脆想姐出关联，可以用

<font color=red>`git remote rm origin`</font>来解除关联。



不过在此之前建议看看远程库信息，确认一遍，输入命令

<font color = red>`git remote -v`</font>

### 2. 与远程仓库的简单交互

当本地库向远程库传输数据关联后，就能向远程库传输数据和同步数据。

输入命令

```
git push -u origin master
```

`-u`是向一个空的仓库传输数据时可以使用的参数，加上后Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来。

那以后向远程仓库传输时就可以直接输入

```
git push origin master
```

从远程仓库同步数据的命令为

```
git pull origin master
```

这是同步，如果是这届把远程仓库克隆到当前目录下命令为

```
git clone git@github.com:<用户名>/<仓库名>.git
```



