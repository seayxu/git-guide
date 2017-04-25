# 如何为项目贡献

如何操作，分两种情况：
- 一是源仓库有读写权限，可直接操作。
- 二是没有权限直接操作源仓库，采用 `合并请求[PR]` 的方式。

源仓库地址为：`http://git-repo.com/origin/project.git`。
派生仓库地址为：`http://git-repo.com/fork/project.git`。

# 方式一

**源仓库有读写权限。**

执行的顺序：

```
Clone ---> Modified ---> Commit ----> Pull -------------------------> Push
                                           \                       /
                                            \ Conflict            /
                                             \ ___ ___ Merged __ /
```

1. 克隆

从源仓库克隆到本地。

```
git clone http://git-repo.com/origin/project.git
```

2. 修改

修改本地仓库内容。

3. 提交

将工作区修改的内容提交到本地版本库。

```
git add .
git commit -m "message"
```

4. 拉取

从远程仓库拉取最新内容到本地。

```
git pull remote-host-name remote-branch-name
```

*注：如果有冲突，修改冲突，合并提交。*

5. 推送

将本地最新修改内容推送至远程源仓库中。

```
git push remote-host-name local-branch-name
```

# 方式二

**源仓库没有读写权限。**

执行的顺序：

```
     ___ ___ ___ ___ Origin Remote Repo① ___ ___ ___ ___
   /                                                     \
  |                                                       |
Fork[①] ---> Clone ---> Modified[③] ---> Commit[③] ---> Pull[①] -------------------> Push[②]
  |            |                              |           \                       /     |
  |            |                              |            \ Conflict            /      |
  |            |                              |             \ ___ Merged[③] ___ /       |
  |            |                              |                                         |
  |             \ ___ Local Remote Repo③ ___ /                                          |
  |                                                                                     |
   \ ___ ___ ___ ___ ___ ___ ___ ___ ___ Fork Remote Repo② ___ ___ ___ ___ ___ ___ ___ / 

```

1. 派生

将源仓库①派生到自己的远程仓库②中。

2. 克隆

从源仓库或派生仓库克隆到本地③。

```
# from origin remote repo
git clone http://git-repo.com/origin/project.git
# add fork remote host
git remote add parent http://git-repo.com/fork/project.git
```

或者

```
# from fork remote repo
git clone http://git-repo.com/fork/project.git
# add origin remote host
git remote add parent http://git-repo.com/origin/project.git
```

*注：以下以派生仓库克隆介绍。*

3. 修改

修改本地仓库内容。

4. 提交

将工作区修改的内容提交到本地版本库③。

```
git add .
git commit -m "message"
```

5. 拉取

从远程仓库①拉取最新内容到本地。

```
git pull parent remote-branch-name
```

*注：如果有冲突，修改冲突，合并提交。*

6. 推送

将本地最新修改内容推送至派生远程源仓库②中。

```
git push origin local-branch-name
```

7. 合并请求

在 Web 或者 客户端新建一个 `PR` ，等待管理员处理。