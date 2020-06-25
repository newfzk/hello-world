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
git commit -m "change 文件名"
```



