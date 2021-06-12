# git的简介

打不开git的方法：

​		windows C:\Windows\System32\drivers\etc\hosts
 		Mac 在Finder中同时按“Shift”“Command”“G”三个键，输入“/etc/hosts”
​		 linux /etc/hosts
​		 方法一直接修改hosts文件,添加下面IP到文件中
 		13.229.188.59 github.com
​		 140.82.112.4 github.com

​		集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。![image-20210601145934521](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210601145934521.png)

集中式版本控制系统，该版本需要在联网的模式下进行，如果是在局域网的情况下进行还好，如果是在打的互联网的情况下，网速很慢，提交一个10m的文件可能需要5分钟，速度很令人失望。

#### 二、分布式版本控制

​		首先，与集中式相比，分布式系统的优点则是每一个电脑都可以当成一台完整的服务器，说明每个人的机器都是有一台完整的版本库。如果进行多人的协作，你修改了文件A，同事修改了文件A，那么只需要给对方推送自己修改的部分，那么就可以知道修改的内容了。

​	其次，分布式系统的一个优点则是更加安全。如果是集中式系统中的服务器坏了，那么可能整个系统都陷入崩坏，而分布式系统中，如果是一个人的电脑出现了问题，那么我们可以从其他的电脑获取相应的数据。（不过我们，一般会把某一台电脑当成一个“服务器”来使用，因为它方便大家用于传送修改的数据）。

#### 三、创建一个版本库

​		版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

​		在cmd中进入到自己的刚刚创建的一个文件夹中去，使用git init 命令，可以将该文件夹变成git的仓库。

```
$ git init
Initialized empty Git repository in /E:/banben
```

​		使用git仓库，必须将文件放置到git仓库中去，这样git才能正确找到该文件。将文件放置到仓库中去后，那么，应该在命令行中输入git add，将文件暂时存入到缓存区（可以多次提交添加文件）。

```
$ git add readme.txt
```

​		在使用命令 git commit，将之前的提交在暂存区的文件提交到git仓库中去。

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

  		在git commit 添加-m "wrote a readme file"，是对提交该文件或者说每次修改之后的备注，在我们以后进行回滚的时候，我们可以选择到我们想要的版本。

**提示：**

​		**Q：**输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

​		**A：**Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。

​		**Q：**输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。

​		**A：**添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

# git详细介绍

#### 	一、查看比对

​			使用git status命令，我们可以看到git add命令后，还未提交的文件的情况。

```
E:\banben>git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.txt
        
```

​			可以看到readme。txt的文件还没有被提交。如果所有的文件都已经被提交后。则会显示

```
E:\banben>git status
On branch master
nothing to commit, working tree clean
```

​		

#### 	二、版本的回退

·	当我们使用git提交文件后，我们在使用过程中，肯定是需要修改文件，当我们使用git log命令，我们可以看到修改过的版本。

    E:\banben>git log
    commit 272c8976f60fa9c31fd146e785847a803cb496c0 (HEAD -> master)
    Author: 135user <a13546877605@163.com>
    Date:  Tue Jun 1 16:10:57 2021 +0800
    
    append one line
    
    commit 69408a2dcd9e488acf93bbc9073a67e68a4e4323
    Author: 135user <a13546877605@163.com>
    Date:   Tue Jun 1 16:08:40 2021 +0800
    
    append GPL
    
    commit a58ef9f31d0f9e9f785dc038fbf5282ac203f50a
    Author: 135user <a13546877605@163.com>
    Date:   Tue Jun 1 15:51:25 2021 +0800
    
    wrote a readme file

​		从上面我们可以看到每次提交后进行修改，head的指针是我们当前的版本

​		如果只是查看版本可以加上`--pretty=oneline`,这样,可以直接查看

```
E:\banben>git log --pretty=oneline
d1cb0d6f162f545de897d3d1e5eea398a1ebf2c0 (HEAD -> master) add life
866fca79daf726cffe16566f5d7c45efb2debc1f add one new line
272c8976f60fa9c31fd146e785847a803cb496c0 append one line
69408a2dcd9e488acf93bbc9073a67e68a4e4323 append GPL
a58ef9f31d0f9e9f785dc038fbf5282ac203f50a wrote a readme file
```

​		这乱码是提交的commit 的版本号，这段版本号是根据SHA1计算出来的一个非常大的数字，这个数字使用十六制表示的。每提交一次，实际上Git就会把它们自动串成一条时间线。

​		head 是当前的版本的位置，`head^`是上一个版本的位置，`head^^`是上上个版本的位置，如果是哪个具体的版本，如果是前3个版本，可以直接写成`head~3`。

​		使用`git reset`，可以进行回退到具体的版本。如：根据上面的版本号，跳到第一个版本。

		E:\banben>git reset --hard a58ef9f31d0f9e9f785dc038fbf5282ac203f50a
		HEAD is now at a58ef9f wrote a readme file

​		那么文件就跳到第一个版本

![image-20210606005115309](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210606005115309.png)

​		但是使用`git log`命令之后，就会发现其他的版本号或者提交记录已经丢失了

```
E:\banben>git log
commit a58ef9f31d0f9e9f785dc038fbf5282ac203f50a (HEAD -> master)
Author: 135user <a13546877605@163.com>
Date:   Tue Jun 1 15:51:25 2021 +0800

	wrote a readme file
```

​		我们可以直接使用`git reflog`标记，可以看到命令的历史，并且可以在重新调到我们想要的版本

```
E:\banben>git reflog
a58ef9f (HEAD -> master) HEAD@{0}: reset: moving to a58ef9f31d0f9e9f785dc038fbf5282ac203f50a
d1cb0d6 HEAD@{1}: commit: add life
866fca7 HEAD@{2}: commit: add one new line
272c897 HEAD@{3}: commit: append one line
69408a2 HEAD@{4}: commit: append GPL
a58ef9f (HEAD -> master) HEAD@{5}: commit (initial): wrote a readme file
```

#### 三、工作区和暂存区

##### 1、工作区

​		工作区，就是电脑中可以看到区域，如我们在E:\banben文件夹就是一个工作区

![image-20210606010237984](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210606010237984.png)

​		但是在这个文件夹中，有一个隐藏文件夹.git，![image-20210606010356789](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210606010356789.png)

##### 2、暂存区

​		里面有一个称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![image-20210607173851293](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210607173851293.png)

​		**把文件往Git版本库里添加的时候，是分两步执行的：**

​		第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

​		第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

​		当我们使用`git init`之后，git就会为我们创建第一个master分支。

#### 四、管理修改

​		git管理的不是文件，管理的是文件的修改。

​		例如：当我们修改一次文件的时候，然后将修改的文件进行上传git add，然后在修改第二次文件，然后在将文件进行提交，可以发现文件只有第一次修改。即：第一次修改 -> `git add` -> 第二次修改 -> `git commit`

​		为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

然后，添加：

```
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```

然后，再修改readme.txt：

```
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

所以 每一次进行修改后，我们都应该将文件的修改进行`git add`到暂存区，然后在将修改一次性提交到master分支。

#### 五、撤销修改

##### 		1.当修改已经提交到master后

​		当我们在将一些修改提交到工作区后，但是发现出现问题了，我们可以使用`git checkout -- file（文件名）`将文件从工作区恢复到暂存区里面，命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

​		一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

​		一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。	

​		使用`git checkout  -- file`其中“--”，不能省略，这个是用来切换到其他分支的意思，还有当使用这个命令之后，在实行的话就没有反应了，也就是说这个命令只能被执行一次。

##### 		2.当修改提交到暂存区

​		当我们将文件修改提交到暂存区，可以使用命令`git reset HEAD <file>`这个可以将暂存区里面的修改退回到工作区。`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

​			`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

#### 六、修改文件

​		如果要删除掉版本库中的文件的话，可以直接使用`git rm file`，并且使用`git commit -m “explain”`，这样就可以将文件删除掉

```
$ rm test.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

​		git会告诉我们将什么东西进行删除了

​		如果我们不小心将某个东西删除了，这是如果这个文件有被提交到版本库的话，我们可以直接使用`git checkout -- file`，这个命令就是将版本库与工作区中的文件不一致的话，可以将版本库重点对工作区进行替换。

# 远程仓库

#### 		一、添加远程仓库

​		1.首先先创建一个github账号，可以在主目录下看看有没有相关的.ssh文件夹（windows C:\Windows\System32\drivers\etc\hosts），如果有的话，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件。`id_rsa`是私钥，不能泄露；`id_rsa.pub`是公钥，可以将之公布出去。

​		如果没有这个文件夹的话，可以使用命令

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

​		"youremail@example.com"这个命令应该是github注册的具体的邮箱。（接下来一路Enter就行了）

​		2.登陆github账号，选择setting

![image-20210611164046642](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210611164046642.png)

​		选择ssh，然后在添加ssh keys

![image-20210611164152692](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210611164152692.png)

​			然后在title随便填，然后填写自己的公钥。`id_rsa.pub`里面的内容。这样就可以先连接好ssh。

​			在首页找到“Create a new repo”按钮创建自己新的仓库。![image-20210612115630000](C:\Users\ipfang\AppData\Roaming\Typora\typora-user-images\image-20210612115630000.png)

​		直接在填写name，然后直接创建即可，然后在cmd中回到自己本地的仓库。

