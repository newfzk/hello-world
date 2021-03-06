# Git学习笔记

## 创建修改版本库

### 初始化 

1. 设置用户名

`git config --global user.name "用户名"`

2. 设置邮箱

`git config --global user.email "邮箱"`

3. 全局存储账号

`git config --global credential.helper store`

4. 建立git管理文件

`git init`

### 上传文件

#### 查看版本库状态

`git status`

#### 添加文件(add)

将没有放入版本库的文件（unstaged状态）添加进版本库（staged）

`git add 文件名`

如果想一次性添加文件夹中所有未被添加的文件，可以使用

`git add .`

#### 提交改变(commit)

提交这次改变，并在`-m`自定义这次改变的信息

`git commit -m "create 文件名"`

#### 流程图
   
   ![流程图](https://morvanzhou.github.io/static/results/git/2-1-1.png)

### 记录修改（log&diff）

在Git中，每次提交（commit）的修改，都会被单独的保存起来。每个commit记录了每次添加或删改的过程。

#### 修改记录 log

查看修改记录

`git log`

被修改过的文件同样需要通过添加(add)和提交(commit)来完成上传。

```git
git add 文件名
git commit -m "change 操作代号"
```

同样，上传后，可以通过`git log`来查看这次修改操作的log

#### 查看 unstaged

想查看还没`add`(unstaged状态)的修改部分和上个已经`commit`的文件有何不同，可以使用：

`git diff`

#### 查看staged(--cached)

如果已经`add`了这次修改，文件变成了“可提交状态”(staged)，我们可以在diff中添加参数`--cached`来查看修改：

`git diff --cached`

#### 查看 unstaged&staged(HEAD)

可以通过使用`git diff HEAD`同时查看`add`过(staged)和没`add`(unstaged)的修改。

`git diff HEAD`

## 回到从前

### 回到从前 reset

#### 修改已commit的版本

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

#### reset回到add之前

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

#### reset回到commit之前

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

### 回到从前checkout（针对单个文件）

`reset` 针对的是整个版本库，如果只想某个文件回到过去，要使用`checkout`

#### 改写文件checkout

我们仅仅要对 `1.py` 进行回到过去操作, 回到 `c6762a1 change 1` 这一个 `commit`. 使用 `checkout` + id `c6762a1` + `--` + 文件目录 ` 1.py`, 我们就能将 `1.py` 的指针 `HEAD` 放在这个时刻 `c6762a1`:

```git
$ git log --oneline
# 输出
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
---------------------
$ git checkout c6762a1 -- 1.py
# -- 后，文件名前需要用空格分开
```

这样，`1.py` 就回到了 `change 1` 版本，在此基础上，对 `1.py` 进行修改，然后 `add` 并 `commit` ：

```git
$ git add 1.py
$ git commit -m "back to change 1 and add comment for 1.py"
$ git log --oneline

# 输出
47f167e back to change 1 and add comment for 1.py
904e1ba change 2
c6762a1 change 1
13be9a7 create 1.py
```

可以看出，不想 `reset` 那样， `change 2` 并没有消失，但是 `1.py` 已经成功返回 `change 1` 版本并修改完成后提交 `commit`。

## 分支管理

### 分支branch

通常会将主分支`master`当做稳定版本，而开发新版本或新属性时，在另外一个分支上进行，这样就能开发使用互不干扰了。

#### graph
可以通过`--graph`来观看分支：

`git log --oneline --graph`

#### 使用branch创建dev分支

```git
git branch dev # 建立dev分支
git branch     # 查看当前分支
# 输出
  dev       
* master    # * 代表了当前的 HEAD 所在的分支
```

想把`HEAD`切换去`dev`分支时，可以用`checkout`：

```git
git checkout dev
# 输出
Switched to branch 'dev'
```

使用`checkout -b` + 分支名，就能直接创建和切换到新建的分支：

`git checkout -b dev`

#### 将dev中的修改推送到master

`dev`分支中的文件通master中是一样的，因为当前指针`HEAD`在`dev`分支上，所以现在对文件夹中的文件进行修改将不会影响到`master`分支。

在`1.py`上加入一行注释`#I was changed in dev branch`，然后再`commit`：

```git 
git commit -am "change 3 in dev"
# "-am": add所有改变，并直接commit
```

在开发版`dev`更新好后，就要将`dev`中的修改推送到`master`中了。

首先要切换到`master`，在将`dev`推送过来。

```git
git checkout master # 切换至master才能将其他分支合并过来

git merge dev # 将dev merge合并到master中
$ git log --oneline --graph

# 输出
* f9584f8 change 3 in dev
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

要注意，直接`git merge dev`，Git会默认采用`Fast forward`格式进行`merge`，这样`merge`的这次操作不会有`commit`信息。`log`中也不会有分支的图案，我们可以采用`--no-ff`这种方式保留`merge`的`commit`信息。

```git
$ git merge --no-ff -m "keep merge info" dev         # 保留 merge 信息
$ git log --oneline --graph

# 输出
*   c60668f keep merge info
|\  
| * f9584f8 change 3 in dev         # 这里就能看出, 我们建立过一个分支
|/  
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

### merge 分支冲突

假设有人在做开发版`dev`的更新，还有人在修改`master`中的一些bug。当我们再`merge dev`的时候，冲突就出现了。因为git不知道应该怎么处理`merge`时，在`master`和`dev`的不同修改。

现在在创建一个分支后，同时对两个分支都进行修改。比如：

- 在`master`中的`1.py`加上`# edited in master`
- 在`dev`中的`1.py`加上`# edited in dev`

如果直接`merge dev`到`master`的时候：

```git
$ git merge dev

# 输出
Auto-merging 1.py
CONFLICT (content): Merge conflict in 1.py
Automatic merge failed; fix conflicts and then commit the result.
```

git会发现`1.py`在`master`和`dev`上的版本不同，所以提示`merge`有冲突。具体的冲突，git会帮助我们标记出来，打开`1.py`就能看到。我们只需要手动合并一下两者的不同，然后再`commit`冲突就解决了。

再来看看`master`的`log`：

```git
$ git log --oneline --graph

# 输出
*   7810065 solve conflict
|\  
| * f7d2e3a change 3 in dev
* | 3d7796e change 4 in master
|/  
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

### rebase 分支冲突

初始版本库如同上节，在`master`和`dev`分支中都有自己的独立修改。

```git
# 这是 master 的 log
* 3d7796e change 4 in master # 这一条 commit 和 dev 的不一样
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
-----------------------------
# 这是 dev 的 log
* f7d2e3a change 3 in dev   # 这一条 commit 和 master 的不一样
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

使用`rebase`合并`dev`到`master`

```git
$ git branch

# 输出
  dev
* master
-------------------------
$ git rebase dev 

# 输出
First, rewinding head to replay your work on top of it...
Applying: change 3 in dev
Using index info to reconstruct a base tree...
M	1.py
Falling back to patching base and 3-way merge...
Auto-merging 1.py
CONFLICT (content): Merge conflict in 1.py
error: Failed to merge in the changes.
Patch failed at 0001 change 3 in dev
The copy of the patch that failed is found in: .git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

`git`提示，发现`1.py`在`master`和`dev`上版本不同，所以提示`merge`有冲突。具体的冲突，git已经帮我们标记出来，我们打开`1.py`就能看到：

```python
a = 1
# I went back to change 1
<<<<<<< f7d2e3a047be4624e83c1265a0946e2e8790f79c
# edited in dev
=======
# edited in master
>>>>>>> change 4 in master
```

这时，`HEAD`并没有指向`master`或者`dev`，而是停在了`rebase`模式上：

```git
$ git branch
* (no branch, rebasing master) # HEAD 在这
  dev
  master
```

所以我们打开`1.py`，手动合并一下两者的不同。然后执行`git add`和`git rebase --continue`就完成了`rebase`操作了。

```git
$ git add 1.py
$ git rebase --continue
```

最后在看一下`master`的`log`：

```git
$ git log --oneline --graph

# 输出
* c844cb1 change 4 in master    # 这条 commit 原本的id=3d7796e, 所以 master 的历史被修改
* f7d2e3a change 3 in dev       # rebase 过来的 dev commit
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

**!!注意!!** 这个例子也说明了使用`rebase`要万分小心，千万不要在共享的`branch`中`rebase`，不让就像上面一样，`master`的历史已经被`rebase`改变了。

如果真的不小心弄错了，可以使用`reset`这一节提到的`reflog`恢复。

### 临时修改stash

#### 暂存修改 stash

假设我们现在在`dev`分支上快乐的改代码：

`git checkout dev`

在`dev`中的`1.py`中加上一行`#feel happy`，然后老板的电话来了，可是我还没有改完这些代码，所以我就用`stash`将这些改变暂时放在一边。

```git
$ git status -s
# 输出
 M 1.py
------------------ 
$ git stash
# 输出
Saved working directory and index state WIP on dev: f7d2e3a change 3 in dev
HEAD is now at f7d2e3a change 3 in dev
-------------------
$ git status
# 输出
On branch dev
nothing to commit, working directory clean  # 干净得很
```

#### 做其他任务

然后我们建立另一个`branch`用来完成老板的任务：
```git
$ git checkout -b boss

# 输出
Switched to a new branch 'boss' # 创建并切换到 boss
```

苦逼的做完老板的任务，如添加`#lovely boss`去`1.py`然后`commit`，完成老板的任务。
```git
$ git commit -am "job from boss"
$ git checkout master
$ git merge --no-ff -m "merged boss job" boss
```

通过以下步骤完成老板的任务，并观看一下`master`的log：
```git
$ git commit -am "solve conflict"
$ git log --oneline --graph
*   1536bea solve conflict
|\  
| * 27ba884 job from boss
* | 2d1961f change 4 in master
|/  
* f7d2e3a change 3 in dev
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

#### 回复暂存

现在可以恢复暂存，继续开心的在`dev`上刷代码了。

```git
$ git checkout dev
$ git stash list    # 查看在 stash 中的缓存

# 输出
stash@{0}: WIP on dev: f7d2e3a change 3 in dev
```

上面说明在`dev`中，我们的确有`stash`的工作。现在可以通过`pop`来提取这个并继续工作了。
```git
$ git stash pop

# 输出
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   1.py

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (23332b7edc105a579b09b127336240a45756a91c)
----------------------
$ git status -s
# 输出
 M 1.py     # 和最开始一样了
```

## Github在线代码管理

#### 连接本地版本库

```git
$ git remote add origin repository的URL
```

#### 推送修改

如果在本地再进行修改，然后`commit`并`push`上去。

```git 
$ git commit -am "change 5"
$ git push -u origin master
```