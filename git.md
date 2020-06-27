# Git学习笔记

## 初始化 

1. 设置用户名

`git config --global user.name "用户名"`

2. 设置邮箱

`git config --global user.email "邮箱"`

3. 全局存储账号

`git config --global credential.helper store`

4. 建立git管理文件

`git init`

## 上传文件

### 查看版本库状态

`git status`

### 添加文件(add)

将没有放入版本库的文件（unstaged状态）添加进版本库（staged）

`git add 文件名`

如果想一次性添加文件夹中所有未被添加的文件，可以使用

`git add .`

### 提交改变(commit)

提交这次改变，并在`-m`自定义这次改变的信息

`git commit -m "create 文件名"`

### 流程图
   
   ![流程图](https://morvanzhou.github.io/static/results/git/2-1-1.png)

## 记录修改（log&diff）

在Git中，每次提交（commit）的修改，都会被单独的保存起来。每个commit记录了每次添加或删改的过程。

### 修改记录 log

查看修改记录

`git log`

被修改过的文件同样需要通过添加(add)和提交(commit)来完成上传。

```git
git add 文件名
git commit -m "change 操作代号"
```

同样，上传后，可以通过`git log`来查看这次修改操作的log

### 查看 unstaged

想查看还没`add`(unstaged状态)的修改部分和上个已经`commit`的文件有何不同，可以使用：

`git diff`

### 查看staged(--cached)

如果已经`add`了这次修改，文件变成了“可提交状态”(staged)，我们可以在diff中添加参数`--cached`来查看修改：

`git diff --cached`

### 查看 unstaged&staged(HEAD)

可以通过使用`git diff HEAD`同时查看`add`过(staged)和没`add`(unstaged)的修改。

`git diff HEAD`

## 回到从前reset

### 修改已commit的版本

想修改上一次的commit，比如添加另一个文件，并将这个修改也commit进上一次commit。可以使用`--amend`将这次改变合并到之前的commit中。

```git
$ git add 2.py
$ git commit --amend --no-edit   # "--no-edit": 不编辑, 直接合并到上一个 commit
$ git log --oneline    # "--oneline": 每个 commit 内容显示在一行

# 输出
904e1ba change 2    # 合并过的 change 2
c6762a1 change 1
13be9a7 create 1.py
```

### reset回到add之前

有时添加`add`了修改，但是又想补充一些内容再`add`，这时，可以通过`git reset 文件名`返回到reset之前。

```git
$ git add 1.py
$ git status -s # "-s": status 的缩写模式
# 输出
M  1.py     # staged
-----------------------
$ git reset 1.py
# 输出
Unstaged changes after reset:
M	1.py
-----------------------
$ git status -s
# 输出
 M 1.py     # unstaged
```

### reset回到commit之前

每个 `commit` 都有自己的 `id` 数字号, `HEAD` 是一个指针, 指引当前的状态是在哪个 `commit`. 我们如果要回到过去, 就是让 `HEAD` 回到过去并 `reset` 此时的 `HEAD` 到过去的位置.

```git
# 不管我们之前有没有做了一些 add 工作, 这一步让我们回到 上一次的 commit
$ git reset --hard HEAD    
# 输出
HEAD is now at 904e1ba change 2
-----------------------
# 看看所有的log
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
-----------------------
# 回到 c6762a1 change 1
# 方式1: "HEAD^"
$ git reset --hard HEAD^  

# 方式2: "commit id"
$ git reset --hard c6762a1
-----------------------
# 看看现在的 log
$ git log --oneline
# 输出
c6762a1 change 1
13be9a7 create 1.py
```

那如何回到reset之前的版本change 2呢？可以通过查看`git reflog`里面最近做的所以`HEAD`的改动，并选择想挽救的`commit id`:

```git
$ git reflog
# 输出
c6762a1 HEAD@{0}: reset: moving to c6762a1
904e1ba HEAD@{1}: commit (amend): change 2
0107760 HEAD@{2}: commit: change 2
c6762a1 HEAD@{3}: commit: change 1
13be9a7 HEAD@{4}: commit (initial): create 1.py
```

比如想回到`commit(amend):change 2`，可以通过使用`reset`：

```git
$ git reset --hard 904e1ba
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
```

