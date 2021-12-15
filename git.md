git 使用规则
------------------> 1 git rebase <-----------------------
git rebase 步骤：

git checkout dev (master)
git pull
git checkout wan_fanhs （local）
git rebase -i HEAD~2 //合并提交 --- 2 表示合并两个
git rebase dev (master)---->解决冲突--->git rebase --continue
// git push -f
git checkout dev (master)
git merge wan_fanhs （local）
git push

------------------> 2 git 代码提交步骤 <-----------------------
git add .
git commit -m "说明"
git push

------------------> 3 git 撤销 commit <-----------------------

git commit -m "说明"
如果 commit 错误了想撤销，那么使用：（还没 push 之前这个是在本地的，git log 的信息也只是本地的）
git reset --soft SHA1 号 即可

------------------> 4 git stash 提交本地暂存 <-----------------------
git stash 提交到缓存区
git stash apply 这个指令会将指定缓存堆栈中的 stash 应用到工作目录中
git stash clear 这个指令是删除所有缓存的 stash
git stash list 这个指令查看现在 statsh 列表
git stash pop 这个指令会将缓存堆栈中的第一个 stash 删除，并将对应修改应用到当前的工作目录下

------------------> 5 git 合并多个 Commit <-----------------------
git rebase -i
合并多个 Commit
那么 git rebase -i 后面跟的参数应该是想要合并的最前面 commit id 的**\_上一个**
git log 查看 commit id
输入完成会弹出以下框，这里选择基于最早的 pick 合并，合并到最早的（ec6c077d9e one）合并，其他都合并到前面的 commit id 去
第一个 pick,其他 s 修改成以下然后保存（意思就是把下面的三个 commit 合并到 one）
esc:退出编辑 :wq 退出
添加提交信息 :wq

git rebase --abort 来撤销修改

------------------> 6 git 版本回退 <-----------------------
git 代码回退：
reset 还是 revert?
reset 和 revert 都可以用来回滚代码。但他们是有区别的，准确来说，reset 是用来"回退"版本，而 revert 是用来"还原"某次或者某几次提交

方法 1：
使用 git log 命令，查看分支提交历史，确认需要回退的版本
使用 git reset --hard commit_id 命令，进行版本回退
使用 git push origin 命令，推送至远程分支

快捷方法：回退上个版本：git reset --hard HEAD^
方法 2：
git revert -n 97ea0f9
git commit -m "恢复第三次修改"
