
# 查看状态

### 查看状态

```
git status
```

### 列出所有追踪的文件

可以使用以下几个命令来查看Git当前已经追踪的文件:

```
git ls-files
```

这个命令将显示Git当前正在追踪的所有文件,也就是已经加入到仓库中的文件。

```
git ls-tree -r master --name-onl
```

这个命令显示在master分支下所有被追踪的文件,去掉其他提交信息只显示文件名。

```
git ls-tree -r HEAD --name-only
```

显示在当前提交(HEAD)所包含的所有追踪文件。

```
git diff --name-status
```

显示与上次提交相比被修改或者新增的文件。

此外,可以通过查看.git文件夹内的index文件来看被追踪的文件列表。

使用这些命令可以方便地查看当前仓库中都有哪些文件被Git所管理和追踪,从而确认.gitignore的设置是否正确。

如果发现有不需要追踪的文件,可以用git rm --cached \<file\>停止追踪指定的文件,然后添加到.gitignore中。

# 远程仓库(github)

### 删除远程仓库的标签

```
git push origin --delete <tagname>
```

列出远程仓库

```
git remote -v
```
# .gitignore

### 编写规则

1. 每行指定一个需要忽略的文件路径或模式,路径区分大小写。
2. 可以使用通配符,例如 * 表示任意多个字符,? 表示一个字符。
3. 以/开始表示要忽略的是一个目录。
4. 以/结束表示要忽略的是这个目录下的内容,不包括本身。
5. 以/!开始表示不忽略这个规则匹配到的文件或目录。
6. 注释行以 # 开头。
7. 可以使用 ! 取消前面的忽略模式,让Git再次跟踪匹配的文件。

```
# 忽略所有 .开头的隐藏文件
.*

# 忽略所有编译生成的文件
*.o
*.exe

# 忽略所有日志文件
*.log

# 忽略 node_modules 目录
node_modules/

# 不忽略 lib 目录下的 README 文件
!lib/README
```

**需要注意的是,被追踪的文件即使后来在 .gitignore 中被忽略了,也不会从仓库中消失,需要用 git rm 删除。**

### 查看已追踪的文件

可以使用以下几个命令来查看Git当前已经追踪的文件:

1. git ls-files

这个命令将显示Git当前正在追踪的所有文件,也就是已经加入到仓库中的文件。

2. git ls-tree -r master --name-only

这个命令显示在master分支下所有被追踪的文件,去掉其他提交信息只显示文件名。

3. git ls-tree -r HEAD --name-only

显示在当前提交(HEAD)所包含的所有追踪文件。

4. git diff --name-status

显示与上次提交相比被修改或者新增的文件。

此外,可以通过查看.git文件夹内的index文件来看被追踪的文件列表。

使用这些命令可以方便地查看当前仓库中都有哪些文件被Git所管理和追踪,从而确认.gitignore的设置是否正确。如果发现有不需要追踪的文件,可以用git rm --cached \<file\>停止追踪指定的文件,然后添加到.gitignore中。

# 标签(tag) 

参见 https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE

## 常用操作

### 新建标签

在当前分支上新建标签

```
git tag <tagname>
```

### 轻量标签

另一种给提交打标签的方式是使用轻量标签。 轻量标签本质上是将提交校验和存储到一个文件中——没有保存任何其他信息。 创建轻量标签，不需要使用 `-a`、`-s` 或 `-m` 选项，只需要提供标签名字：

```console
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```

这时，如果在标签上运行 `git show`，你不会看到额外的标签信息。 命令只会显示出提交信息：

```console
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```

### 删除标签

要删除掉你本地仓库上的标签，可以使用命令 `git tag -d <tagname>`。 例如，可以使用以下命令删除一个轻量标签：

```console
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```

注意上述命令并不会从任何远程仓库中移除这个标签，你必须用 `git push <remote> :refs/tags/<tagname>` 来更新你的远程仓库：

第一种变体是 `git push <remote> :refs/tags/<tagname>` ：

```console
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
```

上面这种操作的含义是，将冒号前面的空值推送到远程标签名，从而高效地删除它。

第二种更直观的删除远程标签的方式是：

```console
$ git push origin --delete <tagname>
```