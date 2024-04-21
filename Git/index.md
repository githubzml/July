# Git 教程

- git init 初始化仓库

- git status 查看仓库状态

- git add 向暂存区中添加文件

- git commit 保存仓库的历史记录

## vim 编辑

退出命令是，按 ESC 键 跳到命令模式，然后输入:q（不保存）或者:wq（保存） 退出。

注意： 要有冒号(:q 获取 :wq)

i 是 INSERT 简写键

---

- git diff 查看更改前后的差别

工作树 暂存区的区别

- git diff head 查看工作树和最新提交的差别

这里的 head 是指 当前分支中最新一次提交的指针

- git branch -a 显示分支一览表

- git checkout -b 创建，切换分支

hash 值只要输入 4 位 以上就可以执行

- git checkout - 切换回上一分支

- git merge --no-ff 为了在历史记录中明确记录下本次分支合并 在合并时加上 --no-ff 参数

- git log --graph 以图表形式查看分支

- git reset --hard 目标时间点的哈希值

- git log 命令只能查看以当前状态为终点的历史日志。
- git relog 查看当前仓库的操作日志。

- git commit --amend 修改提交信息

更改历史

- git rebase -i HEAD~2 在历史记录中合并成一次完美的提交

- git branch -m main 将分支名称改为 main

1.  git push -u origin main 之后 git push
2.  git push origin main 之后 还是 git push origin main

    1 与 2 效果相同
    前提是，第一次提交需要加 -u 参数后，后面的提交就直接可以 git push

- git push -u origin feature-D 推送至 main 以外的分支

- git checkout -b xxx origin/xxx 将远程分支获取至本地仓库

- git pull origin feature-D 获取远端分支最新代码

# 查看分支之间的差别

- 版本差别

https://github.com/rails/rails/compare/4-0-stable...3-2-stable

- 几天前的差别
  https://github.com/rails/rails/compare/master@{7.day.ago}...master

- day week month year

- 查看与指定日期之间的差别

  https://github.com/rails/rails/compare/master@{2013-01-01}...master

# git fetch 和 git pull 区别

1. git fetch：Git git 会将数据拉取到本地仓库 - 它并不会自动合并或修改当前的工作。

2. git pull：git pull 是从远程获取最新版本并 merge 到本地，会自动合并或修改当前的工作。

3. 使用后 commitID 不同。

- fetch：使用 fetch 更新代码，本地的库中 master 的 commitID 不变，还是等于 1。

- pull：使用 pull 更新代码，本地的库中 master 的 commitID 发生改变，变成了 2。

4. git pull = git fetch + git merge

# Pull request 中 Merge pull request/Squash and merge/Rebase and merge 区别

1.  Merge pull request 直接合并
2.  Squash and merge 意思是压缩 提交 言外意思将一下小的改动 压缩为一个提交 防止记录过多 不好追溯
3.  Rebase and merge 产生日志记录的合并
    $ git checkout dev
    $ git rebase -i master
    $ git checkout master
    $ git merge dev

<!-- git remote add upstream git:https://github.com/ituring/first-pr -->

给原仓库设置名称

git remote add upstream git:XXX

获取原仓库最新数据

git fetch upstream

# git tag

```bash
# 直接给当前的提交版本创建一个 【附注标签】
$ git tag -a [标签名称] -m [附注信息]

# 给指定的提交版本创建一个【附注标签】
$ git tag -a [标签名称] [提交版本号] -m [附注信息]

```

- -a：理解为 annotated 的首字符，表示附注标签；

- -m：指定附注信息；

例如：

```bash
$ git tag -a v2.0 -m "v2.0版本正式发布"

$ git tag -a v2.0-release 4489bac3 -m "v2.0版本正式发布"

```

### 查看 tag 标签

1. 查看标签列表

```bash
# 查看所有标签列表
$ git tag
```

2. 查看标签提交信息

```bash
# 查看标签的信息，（轻量标签 和 附注标签 的信息是不一样的）
$ git show [标签名]
```

3. 删除 tag 标签

```bash
$ git tag -d [标签名称]
```

例如：

```bash
$ git tag -d test-2.0
```

## 远程仓库 tag 操作

1. 推送 tag 标签到远程仓库

```bash
# 将指定的标签上传到远程仓库
$ git push origin [标签名称]

# 将所有不在远程仓库中的标签上传到远程仓库
$ git push origin --tags

```

例如：

```bash
$ git push origin test-2.0
```

2. 删除远程仓库 tag 标签

```bash
$ git push origin --delete [标签名称]
```

例如：

```bash
$ git push origin --delete test-2.0
```

## 检出标签

检出标签的理解：是在这个标签的基础上进行其他的开发或操作。

检出标签的操作实质：是以标签指定的版本为基础版本，新建一个分支，继续其他的操作。

因此 ，就是新建分支的操作了。

```bash
$ git checkout -b [分支名称] [标签名称]
```

# Git Flow

## 版本号的分布规则

- 1.0.0：最初发布的版本
- 1.0.1：修正了轻微 BUG
- 1.0.2：修复漏洞
- 1.1.0：添加新功能
- 2.0.0：更新整体 UI 并添加新功能

# Git 分支

- master 分支（主分支） 用于发布生产环境的版本分支，在 master 分支中建立标签 Tags 版本号 将版本号部署到生产环境。

- release 分支 （测试分支）基于 master 分支建立的子分支，用于项目测试，测试通过达到上线标准后将该分支合并至 master 分支。

* develop 分支 （开发分支）基于 master 分支建立的子分支，用于合并开发人员的所有分支，完成所有需求开发后合并到 release 分支，提交测试服务器。

* feature 分支（需求分支）基于 develop 建立的子分支，根据不同的需求建立多个 feature 分支，完成功能后合并到 develop 分支

* bug 分支基于 release 分支创建的子分支，用于修复测试环境的问题，修复完成后合并到 release 和 develop 分支。

* hotfix 分支（紧急分支）基于 master 分支建立的子分支，用于生产环境问题的紧急修复，修复完成后合并到 master 分支 和 develop 分支
