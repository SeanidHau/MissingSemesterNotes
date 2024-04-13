# Version Control（Git）

## Git的历史模型

```
type blob = array<byte>
type tree = map<string, tree | blob>
type commit = struct {
	parents  = array<commit>
	author  = string
	message = string
	snapshot = treee
}
```

## Git对数据的存储和寻址

```
type object = blob | tree | commit

objects = map<string, object>

def store(o) :
	id = sha1(o)
	object[id] = o
	
def load(id) :
	return object[id]
```

###  What is SHA-1 hash?

是一种密码散列函数，可以将任意长度的消息输入，并输出一个160位（20字节）的散列值（通常表示为40个十六进制字符）。这个散列值也称为消息摘要，它是输入数据的一个独特表示，意味着任何输入数据的微小变化都会导致输出散列值的显著变化。

## Reference

```
reference = map<string, string>
```

使用reference，可以使用人类可读的名称来引用历史中特定的快照，而不是十六进制字符

## Git Commands

```shell
git init
```

将Git仓库初始化，但它是隐藏文件夹，一般不可见，使用`ls -a`可以看见,git文件夹

```shell
git help <sub-command>
```

查看子命令的帮助，比如`git help init`

```shell
git status
```

查看当前仓库的情况

```shell
echo "Hello World" > hello.txt
git status
# On branch master
# No commits yet
# Untracked files:
# hello.txt
# nothing added to commit but untracked files present
```

虽然仓库有新文件，但是在下一次快照不会有它

```shell
git add hello.txt
git status
# On branch master
# No commits yet
# Changes to be committed:
# new file: hello.txt
```

文件只有在add后才会出现在下一次快照里

```shell
git commit
# 弹出文本编辑器，要求输入与提交此相关的信息
> Add hello.txt
# [master (root-commit) 42fb7a2] Add hello.txt
# 1 file changed, 1 insertion(+)
# create mode 100644 hello.txt
```

在提交信息中，42fb7a2就是刚才提交的哈希值

```shell
git cat-file -p 42fb7a2
# 这是提交的哈希值，内部还包含属的哈希值和一些其他信息
# tree  <treehash>
# author ...... ... ...
# committer ...... ... ...
git cat-file -p <treehash>
# ... blob <blobhash> hello.txt
git cat-file -p <blobhash>
# Hello World
```

```shell
git log
# commit ...... ( HEAD -> master)
# Author: ...
# Date: ...
# Add hello.txt
```

使用`git log`可以可视化提交历史，它是一个展平的线性历史记录，而`git log --all --graph --decorate`则可以以图的形式展示。

在可视化中，master通常代表项目的最新版本，可以视为指向这个提交的指针，当有新的提交时，master会指向那个新的提交。而HEAD是git里的一个特殊引用，类似于master引用，但有一些特殊的作用，基本上用于指向当前正在查看的提交。

```shell
git commit -a
#提交所有已跟踪的文件的修改
git add :/
#添加仓库顶部自上而下的所有内容
```

```shell
git checkout <hash | master>
```

改变当前工作目录的状态到指定的提交，这时使用`git log`查看可以发现HEAD转移到了指定的提交处，而master的位置不变。

需要注意的是，checkout可能会销毁你的更改。

```shell
git diff hello.txt
git diff <hash> <filename>
git diff <hash> <hash> <filename>
```

`git diff <filename>`可以显示当前目录与HEAD指向的快照相比发生了哪些变化，还可以接受参数对指定快照进行对比

## Branching and Merging

```shell
git checkout hello.txt
```

这是checkout的另一个用法，将会丢弃在工作目录中所做的修改，并将hello.txt的内容设置回HEAD指向的快照的状态

```shell
git branch
# * master
git branch -vv
# * master ... ......
#-vv可以打印一些更详细的信息
```

列出本地仓库中存在的所有分支

```shell
git branch <name>
# 创建一个新的分支
git checkout <branchname>
# 工作目录替换成指向的内容
或者
git checkout -b <brachname>
# 同样的效果
```

```shell
git merge <branchname>
```

合并分支，允许同时合并多个的分支，但在合并多个分支时，可能会出现冲突的情况，这时需要自行修改，或者使用mergetool

```shell
git mergetool
```

当问题解决后，执行

```shell
git merge --continue
```

可以继续合并

或者当你无法解决冲突时，使用

```shell
git merge --abort
```

来退回合并

## Git Remote

```shell
git remote
```

列出当前仓库所知道的所有远程仓库

```shell
git remote add <name> <url>
# 如果只使用一个仓库，通常名字为origin
# url类似于github url如果使用在线仓库托管服务
```

```shell
git push <remotename> <local branchname>:<remote branchname>
```

将更改从本地发送到远程仓库，创建一个新分支或更新上面的一个分支

```shell
git clone <url> <folder name>
```

克隆一个仓库到本地，创建一个本地副本

```shell
git push --set-upstream-to=origin/master
git push
```

简易化push

因为git在一般情况下不联网，所以需要在接受的仓库接收push

```shell
git fetch
# 只有一个远程仓库的情况下，不需要输入名字
git fetch <remote>
```

```shell
git pull
# 效果等同于git fetch + git merge
```

## Other Things Git Can Do

```shell
git config
vim ~/.gitconfig
```

自定义git

```shell
git clone --shallow
```

在复制极大的仓库时，`--shallow`可以更快的复制，但是不会在本地拥有完整的版本历史记录，只会有最新的快照

```shell
git add -p xxx.py
```

`-p`允许交互式地暂存文件的片段，在交互式的对话中可以将更改变成更小的片段，可以选择想保留和想丢弃的修改

```shell
git diff --cache
```

查看缓存区内的修改，不会显示想要丢弃的部分

```shell
git blame <filename>
```

用于确认谁编辑了文件的哪一行以及修改特定行的特定提交

```shell
git stash
git stash pop
```

`git stash`会将工作目录恢复到上一次提交的状态，但是所做的修改将会被保存，在执行命令`git stash pop`后将会重新出现

```shell
git bisect
```

一个可以用来解决手动搜索历史记录这个问题的工具，自动化寻找历史代码

__gitignore文件__：

用来帮助你的git忽略特定的文件，可以用文件名或用正则表达式
