# git

## 底层

1. git ls-files -s  查看暂存区
2. git cat-file -p 哈希值           查看对应哈希值的内容
3. git cat-file -t 哈希值            查看对对应哈希值的类型





### 配置用户信息

git config --global user.name 'xxx'		(同时也可以用来查看)

git config --global user.email 'xxx@xxx'

**查看已经配置的信息**:git config --list

检查版本：git --version





### 区域

工作区  --> 暂存区 -->  版本库

* 工作区

  就是自己本地的代码(vscode等等)

* 暂存区

* 版本库







### 对象

* git对象
* 树对象
* 提交对象





### 初始化仓库

在目录下面右键，git bash here

然后 git init

然后目录下面就出现了.git目录



**.git目录下面包含：**

![image-20220507103100887](git学习.assets/image-20220507103100887.png)



objects是版本库，里面的文件只会新增，不会减少



## 高层

0. **git status**        **查看文件跟踪状态**      
1. **git init 			初始化仓库**
2. **git add ./         将修改添加到暂存区**
3. **git commit -m "注释"       将暂存区提交到版本库**
4. git diff               查看哪些修改还没被暂存
5. git diff --staged (git diff --cached)     查看哪些修改暂存了，但是还没提交(commit)
6. git commit -a -m "注释"          直接提交，跳过暂存
7. git log          查看提交日志     简介输出：git log --oneline![image-20220509214744658](git学习.assets/image-20220509214744658.png)
8. 



![image-20220507151529584](git学习.assets/image-20220507151529584.png)

git init     生成.git

git ls-files -s     查看暂存区   看到是空的

find .git/objects/       查看.git目录下的objects目录里面有什么，看到有info文件夹和pack文件夹

echo "i am leaning git" -> learning.txt       把这段字符串写进learning.txt文件中

find .git/objects/ -type f  在objects下面查找类型为**文件**，结果为空



![image-20220507151739641](git学习.assets/image-20220507151739641.png)

git add 之后再查看缓存区，发现有东西了。



![image-20220507152317281](git学习.assets/image-20220507152317281.png)

发现objects(版本库)下面也有文件类型的文件了，

git cat-file -p hash(加hash值)  就可以看到文件里面的内容了，哈希值不必写全

git cat-file -t hash    可以看到类型

### **重点：git add真实步骤**

git add 是先到**版本库**，再到**暂存区**

git ad  ->  版本库  ->  暂存区

每次git add ，有多少个文件修改了(就生成多少个git对象添加到版本库中)，就会在objects(版本库)里面添加多少个目录，每个目录里面有一个文件名为哈希值的文件。



这时候暂存区里面就存放了修改的



### git commit

新增一个.txt文件后git add ./ 然后 git commit -m  之后，发现版本库objects下面多了三个目录，每个目录里面是一个hash，对应**git对象，树对象，提交对象**



![image-20220508210920091](git学习.assets/image-20220508210920091.png)

高层命令git add 相当于底层命令git hash-object -w

高层命令git commit 相当于底层命令git wtire-tree + git commit-tree







## 分支

1. git branch 分支名         在当前提交对象上创建新分支
2. git branch 分支名 哈希值            在指定的提交对象(哈希值对应)上面创建新的分支
3. git checkout 分支名       切换分支
4. git checkout -b 分支名       新建分支并且切换过去
5. git branch (git branch -l)         查看分支
6. git branch -d 分支名       删除分支    -D 为强制删除
7. git branch -v                查看每一个分支的最后一次提交
8. git log --oneline --decorate --graph --all          查看整个项目的分支图



**分支本质就是一个提交对象**

**HEAD：是一个指针，指向当前活动的分支，默认指向master分支，可以通过checkout切换HEAD的指向，也就是切换分支，每次有新的提交时，HEAD都会带着当前指向的分支一起向前移动**



git branch :为你创建了一个可以移动的新的指针。 比如，创建一个 testing 分 

支：git branch testing。这会在当前所在的提交对象上创建一个指针

![image-20220509220958076](git学习.assets/image-20220509220958076.png)

此时HEAD还是指向master分支，

git checkout dev 之后就指向了dev分支

![image-20220509221305273](git学习.assets/image-20220509221305273.png)

然后我们在dev分支上面去修改东西，不会影响master，master分支就留在了last commit这个版本。

![image-20220509221744900](git学习.assets/image-20220509221744900.png)

![image-20220509221817107](git学习.assets/image-20220509221817107.png)

dev分支上面有a.txt和c.txt，切换到master看，只有c.txt；



![image-20220509234802727](git学习.assets/image-20220509234802727.png)

看到创建出来的新分支dev，比master多了两个提交

用**git log --onelline --decorate --graph --all** 查看完整的日志：

![image-20220509235109525](git学习.assets/image-20220509235109525.png)

接下来想删除dev分支：首先要切换回master分支，然后才能删除dev分支![image-20220510000148451](git学习.assets/image-20220510000148451.png)





**新建分支，让这个分支指向任何版本**

![image-20220510001155906](git学习.assets/image-20220510001155906.png)

使用git branch 分支名 哈希值  就可以让这个分支指向对应的哈希值对应的版本。

![image-20220510001321195](git学习.assets/image-20220510001321195.png)

本来master只有c的，现在有a.txt,b.txt。可见指向了之前的版本啦。

使用场景：想看会之气那的代码怎么写的，就用这种方法，看到、拿到需要的内容了，就切回别的分支，把这个分支删了就行。





<u>git每次存的都是项目的完整快照，需要的磁盘空间会比svn稍微大一点，但是大不了很多，(git团队对代码做了极致的压缩)；</u>



#### 切换分支

切换分支会动三个地方：

1. HEAD
2. 暂存区
3. 工作目录



![image-20220510145433457](git学习.assets/image-20220510145433457.png)

在test分支，暂存区两个文件，工作目录也是；

![image-20220510145524357](git学习.assets/image-20220510145524357.png)

切换到master，暂存区一个a，工作目录也是；



**每次切换分支前，当前分支一定要是干净的(已提交状态)**

如果在test分支**新增**了c.txt但是又提交(不add  不commit)，就切换到master分支：![image-20220510150110154](git学习.assets/image-20220510150110154.png)

发现master的工作目录也会有c,但是暂存区没有c(本来是只有a的);



我们如果在test的时候把c给add到暂存区了再切换到master，master上面的暂存区也就有c了：![image-20220510150655424](git学习.assets/image-20220510150655424.png)





如果是**修改了当前分支已经提交过的文件**(而不是新增文件)，再想切换分支，就会被**拒绝**![image-20220510152502317](git学习.assets/image-20220510152502317.png)







## 实际案例

![image-20220510152628840](git学习.assets/image-20220510152628840.png)

本来在有分支issue53分支在工作，然后在master基础上面创建了hotbug分支，解决了问题，提交了：![image-20220510170510123](git学习.assets/image-20220510170510123.png)

这时，我们要把解决好的hotbug合并：![image-20220510170605753](git学习.assets/image-20220510170605753.png)

其实就是直接把master提上去，这时候就可以删除hotbug分支了；

然而，这时候我们本来工作的iss53分支就落后于master了，master基于原来的已经解决了一个bug，而这个bug在iss53还是存在的；

![image-20220510195515323](git学习.assets/image-20220510195515323.png)



然后模拟冲突的情况：iss53分支也修改了a.txt的第三行(因为master也提交了修改了第三行的),当切换到master合并iss53的时候就会产生冲突：

![image-20220510222820628](git学习.assets/image-20220510222820628.png)

会让你处理冲突之后再commit（这时候还没合并成功）;

我们进入a.txt处理好冲突：

![image-20220510223059991](git学习.assets/image-20220510223059991.png)

![image-20220510223119928](git学习.assets/image-20220510223119928.png)

处理好冲突后提交，就合并成功了：

![image-20220510223332954](git学习.assets/image-20220510223332954.png)

然后就可以把iss53分支给删掉了



## 存储

1. git stash               把当前的修改压栈
2. git stash list          看看当前分支stash里面有没有东西
3. git stash apply       运用栈里面的修改，但是不会把东西出栈
4. git stash drop stash@{0}           删除栈顶东西
5. git pop               出栈，且应用







## git后悔药

* 工作区

  * 如何撤回自己在工作区目录中的修改：git restore 文件名

* 暂存区

  * 如何撤回自己的暂存区：git restore --staged 文件名

* 版本库

  * 如何撤回自己的提交：1.要是内容错了，直接改过来再提交一次就好了

    ​                                       2.要是提交的时候提交注释错了：git commit --amend



#### 回滚到上一次提交

git reset --soft HEAD~ (波浪号)          让当前HEAD指向的分支回滚

![image-20220511212918400](git学习.assets/image-20220511212918400.png)







## 远程仓库

新建一个远程仓库：![image-20220512164216517](git学习.assets/image-20220512164216517.png)

然后建立连接：

![image-20220512164250736](git学习.assets/image-20220512164250736.png)

learning_git是给仓库的别名

配置远程仓库的用户信息：

![image-20220512164741013](git学习.assets/image-20220512164741013.png)

![image-20220512164747338](git学习.assets/image-20220512164747338.png)

