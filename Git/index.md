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
