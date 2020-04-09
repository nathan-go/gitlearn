# git

## git 的诞生

1. git 是由 Linus 使用 c 编写的一个分布式版本控制系统

## 集中式 vs 分布式

1. 集中式的版本控制系统就是集中存放在中央服务其中，然后我们在使用的时候，我们 fetch it from the remote。
2. 集中式版本控制系统最大的毛病就是必须联网才能工作。

### 分布式

1. 分布式版本控制系统根本没有中央服务器，每一个人的电脑上都有一个完整的版本库，我们在共同修改的时候，只需要把各自的修改推送给对方，就可以互相看到对方的修改了。
2. 分布式的方式鲁棒性高，因为没有中央服务器，每个人都有一个版本。
3. 当然 git 的优势不紧紧在联网方面的优势，更有强大的分支管理。

## 安装完了，开始使用

### 1. 创建版本库

1. `git init`: 初始化管理的仓库， 而且 git 会提示我们这个是一个空的仓库。repository。然后我们可以看到当前的目录下面多了一个.git 目录，这个目录是 git 用来跟踪管理版本库的，这个目录注意不能随便修改的哈。我们可以使用`ls -ah`来看到
2. 所有的版本控制系统，其实只能跟踪文本文件的改动，比如 txt，网页，等程序代码，git 也是。而对于图片和视频这些个二进制文件，虽然也能由版本控制系统管理，但是没法跟踪文件的变化，只能知道文件的大小发生了变化，但是对于改了什么内容，版本控制系统是没法知道的。
3. 而且，针对 ms 的 word 文档也是二进制格式，所以版本控制系统是没法跟踪 word 文件的改动的。
4. 针对使用 window 同学，要注意不要用 window 自带的记事本编写任何文本文件。
5. git add \*(readme.txt), 输入后不会有东西输出，unix 哲学： 没有消息就是好消息
6. git commit -m 'write a file': 输入后会告诉我们那些文件修改了，什么内容被插入了, file changes, 25 insertions
7. 我们的逻辑在这里在于我们可以添加很多次文件，然后总结成一个 commit
8. **summary**:

```bash
$ git init
$ git add <file>
$ git add <another file>
$ git commit -m "message"
```

### 2. 时光机穿梭

1. 然后我们文件先修改一下，然后使用`git status`命令看一下，我们会发现，git status 命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，有文件被修改过了，但是还没有准备好提交的修改。
2. git status 只能告诉我们那些文件被修改了，而如果我们想看到那些地方修改了的话，我们需要使用`git diff <filename>`
3. 然后我们在知道那些文件做了修改之后，我们就继续使用 git add, git commit

```bash
git status
git diff <filename>
```

### 3. 版本退回

1. 每一次 commit 都会有一个 git 中的快照，一旦我们误删了文件, 我们都可以从最近的一个 commit 返回。
2. 使用`git log`我们可以可以查看历史的记录
3. `git log --pretty=oneline`
4. 类似 1094ab 等等一大串是 commit id(版本号), 需要注意的是： git 中的这个数字是一个 sha1 计算出来的一个非常大的数字，用 16 进制表示。
5. 在 git 中，我们使用了`HEAD`表示当前的版本，也就是最新的提交的版本，而上一个版本是`HEAD^`。上上一个版本就是`HEAD^^`, 当然往上 100 个可以是`HEAD-100`.
6. 那么我们回退到上一个版本是用的命令是:
   `git reset --hard HEAD^`
7. 如果我们需要回退到一个指定的版本的话，我们可以取 commit id 的前几个位数，然后使用 reset
   `git reset --hard 1234d`
8. 比如我们回退到了之前的版本，我们就失去了之前 head 版本的 commit id，那么我们可以使用`git reflog`来查看我们的每一次命令和 commit id。

**summary**：

1. git reset --hard commit-id
2. git log
3. git reflog

### 4. 工作区和暂缓区

1. git 多了一个所谓的暂缓区的概念。

#### 1. 工作区(working dir)

1. 我们在文件夹中的目录就是一个工作区。

#### 2. 版本库(repository)

1. 工作区有一个隐藏的目录.git，这个不算工作区，而是 git 的版本库。
2. git 的版本库里面有很多东西，其中最重要的就是 stage（或者叫做 index)的暂存区。还有 git 为我们自动创建的第一个分支 master, 以及指向 master 的一个指针叫做 HEAD。

3. 下面是重新考虑一下：
    1. git add: 把文件添加进去，实际上就是文件修改添加到暂存区里面。
    2. git commit: 提交更改，实际上就是把暂存区里面的所有内容提交到当前的分支。
4. ![avatar](/1-工作区暂缓区.png)

### 3. 管理修改

1. git 相比较其他的优秀在于，git 跟踪的并管理的是修改，而不是文件。
2. git diff HEAD -- readme.txt， 命令可以查看工作区和版本库里面最新版本的区别。

### 4. 撤销修改

1. `git checkout -- filename`: 把 readme.txt 文件在**工作区**的修改全部撤销：
    1. 一种是 readme.md 自修改后还没有放到暂存区，现在撤销修改就回到了和版本库中一样的状态。
    2. 一种是 readme.md 已经添加到了暂存区后，又做了修改，现在撤销修改之后就回到了添加到暂存区后的状态。
2. 简而言之就是 git checkout 可以让文件回到最近一次的 git commit 或者 git add 时的状态。
3. --的意思就是我们在当前分支，所以这个东西非常的重要。
4. 针对我们 git add 之后操作的撤销，我们需要使用到`git reset HEAD <file>`, 可以把暂存区的修改撤销掉（unstage）， 重新放回到工作区中去，但是我们要注意的是，只是内容被 unstage 了，并没有被删除掉。**总结**： `git reset HEAD readme.md`, 把暂存区的修改撤销到工作区中。

### 5. 删除文件

1. 在 git 中，删除也是一个修改的操作，我们实战一下。
2. rm delete.md
3. 注意删除也是一个操作
4. 我们在使用了 rm 之后就表示工作区和版本库就回避一致了。 我们可以通过 git status 查看到这个情况。
5. 我们查看到之后发现有两个选择：
    1. 确实我们需要从版本中删除掉，然后我们需要使用`git rm`,然后`git commit`
    2. 我们确实是删除错了，因为版本库中还有，所以我们可以很轻松的把删除的文件恢复到最新的版本。 但是这个删除指定是使用 rm filename，或者直接删除文件，`git checkout -- text.txt`
       notice: 这里的 git checkout 其实就是使用版本库里面的版本替换到工作区的版本，无论工作区是修改了还是删除了，都可以一键还原。

## 远程仓库

1. git remote add origin https://github.com/nathan-go/gitlearn.git
   git push -u origin master
2. origin: 远程库的 git 的默认叫法。
3. 如果我们第一次把本地库的内容推送到远程上面，我们使用`git push -u(把本地master分支和远程的master分支关联起来) origin master`
4. 以后的话我们只需要把 git push origin master 完成就行

### 远程克隆

1. git clone： ssh 协议速度，但是如果我们服务器端只支持 http 的话，那么就只能使用 https 协议了

## 分支管理

1. 类似平行宇宙

### 1. 创建和合并分支

1. 在版本控制的里面，我们每次提交都会有一个时间线，而这个时间线会串成一个分支。
2. 当下为止，我们 git 只有一个分支，这个分支叫做主分支，也就是 master 分支。而 HEAD 严格来讲并不是指向提交，而是指向了 master，master 才是指向提交的，所以 HEAD 指向的是当前的分支。
3. 新创建的 dev 分支，其实 master 也好，dev 也好，只是一个指针。

#### 创建过程

1. git checkout -b dev(switch to a new branch `dev`)
   解释： 1. checkout 加上-b 的参数的意思是创建并切换到这个分支 2. 相当于: git branch dev, git checkout dev
2. git branch: 可以查看分支和当前的分支
3. git checkout branch/master: 切换分支

#### 合并分支

1. git merge dev: 命令的意思是合并指定分支到当前分支。
2. 合并完之后我们就可以删除这个分支了，git branch -d dev
3. 这种方式其实很快的，而且更加安全，所以我们推荐使用这种方式。

#### switch

1. 我们注意到 git checkout <branch>, 和前面的撤销修改： git checkout -- filename, 是同一个命令有些困惑，所以 git 新版本里面提供了一个新的：
2. git switch -c dev: 创建并切换到新的 dev 分支。change
3. git switch master: 切换到新的 master 分支。

#### summary：

1. Git 鼓励大量使用分支：
    1. 查看分支：git branch
    2. 创建分支：git branch <name>
    3. 切换分支：git checkout <name>或者 git switch <name>
    4. 创建+切换分支：git checkout -b <name>或者 git switch -c <name>
    5. 合并某分支到当前分支：git merge <name>
    6. 删除分支：git branch -d <name>

### 2. 解决冲突

1.
