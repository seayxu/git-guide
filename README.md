# Git 简易指南
日常常用 Git 命令。 

# 什么是 Git
> Git 是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。

# 常见的 Git 代码托管平台

- [Gitlab](https://gitlab.com/)
- [Github](https://github.com/)
- [Git@OSC](http://git.oschina.net/)
- [Coding](https://coding.net/)

# 名词解释

- **仓库（Repository）**：通常就是一个项目，也可以是多个项目在一起。
- **版本库**：就是仓库根目录下的`.git`文件夹（是隐藏文件夹），用于跟踪版本修改，可退回。
- **分支（Branch）**：是基于仓库的平行空间，Git 一般有默认的分支`master`。
- **工作区（Working Directory）**：按照文件系统就是一个文件夹，项目的根目录，所有的文件内容修改都是在工作区完成。
- **暂存区（Stage）**：就是即将提交到某分支中的临时存储区域。


# 本地初始化（init）
```
cd <dir>
git init
```

# 仓库克隆（clone）
```
git clone <remote_repo_url>
# 指定克隆的目录
git clone <remote_repo_url> <dir>
```

# 远程仓库（remote）
一般的默认 `remote_name` 为 `origin`。

- 查看

```
# 查看远程主机
git remote

# 查看远程主机的地址
git remote -v

# 查看指定远程主机
git remote show <remote_name>
```

- 添加远程主机

```
git remote add <remote_name> <remote_repo_url>
```

- 删除远程主机

```
git remote rm <remote_name>
```

- 重命名远程主机名称

```
git remote rename <remote_name> <remote_name_new>
```

# 添加至暂存区（add）

```
git add .
git add -A
git add --all
git add . -f
# -f是强制操作
```

# 查看状态（status）

```
git status
```

# 提交修改（commit）

```
git commit -m "提交的描述文字"
```

# 查看历史提交记录（log）

```
git log
```

# 查看命令历史（reflog）

```
git reflog
```

# 版本回退（reset）
在 Git 中 用 `HEAD` 表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上一个版本，依次类推。由于当退回版本过早这种表示方法不方法，于是就有了简便的写法：`HEAD~回退版本值`。例如：倒数第10次版本就是`HEAD~10`。

```
# 回退到上次版本
git reset --hard HEAD^
# 回退到上上次版本
git reset --hard HEAD^^
# 回退到倒数第10版本
git reset --hard HEAD~10

# 回退到指定提交id的版本
git reset --hard commit_id
```

# 分支操作（branch）

- 创建分支

```
git branch <branch-name>
```

- 重命名本地分支

```
git branch -m <old-branch-name> <new-branch-name>
```

- 分支切换

```
git checkout <branch-name>
```

- 创建分支并切换到该分支

```
git checkout -d <branch-name>
```

- 删除本地分支：

```
git branch -d/-D <branch-name>
# -d：删除
# -D：强制删除
```

- 查看所有分支

```
git branch -a
```

# 标签管理（tag）

- 查看本地所有标签

```
git tag
```

- 创建标签

给当前分支添加tag
```
git tag <tag-name>
```

给当指定提交sha-1值添加tag
```
git tag <tag-name> [sha-1]
```

- 删除本地tag

```
git tag -d <tag-name>
```

# 取回（fetch）

```
# 将更新全部取回本地
git fetch [<remote_repo> | <remote_repo_url>]

# 将指定分支更新取回本地
git fetch [<remote_repo> | <remote_repo_url>] <remote-branch-name>
```

# 拉取（pull）
相当于 *pull & merge* 两个操作。

```
# 拉取远程分支到当前分支
git pull origin <remote-branch-name>

# 拉取远程分支到一个新的分支
git pull origin <remote-branch-name>:<local-branch-name>
```

# 推送（push）

## 分支（branch）
- 推送本地分支：

```
# 推送本地分支至远程仓库
git push <remote> <local-branch-name>

# 推送本地分支至远程仓库指定分支名称
git push <remote> <local-branch-name>:<remote-branch-name>
```

- 查看远程分支

```
git branch -r
```

- 删除远程分支：

```
git push origin --delete <branch-name>

# 推送一个空分支到远程分支，其实就相当于删除远程分支
git push origin :<branch-name>
```

- 删除不存在对应远程分支的本地分支：

```
git remote prune origin
git fetch -p
```

## 标签（tag）
- 推送本地所有标签到远程仓库

```
git push --tags
```

- 获取远程标签

```
git fetch origin tag <tag-name>
```

- 删除远程标签

```
git push origin --delete tag <tag-name>

# 推送一个空标签到远程
git push origin :refs/tags/<tag-name>
```

# 提取内容

从其它分支提取文件
```
git checkout <branch-name> -- <file name>
```

从历史版本提取文件
```
git reset commit_id <file name>
git checkout -- <file name>
```

# 合并(merge)

## 1.本仓库内

```
git checkout <base-on-branch>
git merge <be-merge-branch-name>
# git add .
# git commit -m ""
git push origin <base-on-branch>
```

## 2.非本仓库内

**第一步: 创建并切换一个新分支，然后拉取要合并的分支。**

```
git checkout -b <new-branch-name> <base-on-branch>
git pull <[remote-http[s] | ssh-url]> <merge-branch-name>
```

**第二步: 本地执行合并操作，然后提交合并[推送至远程仓库]。**

```
git checkout <base-on-branch>
git merge --no-ff <new-branch-name>
# git add .
# git commit -m ""
# git push origin <base-on-branch>
```