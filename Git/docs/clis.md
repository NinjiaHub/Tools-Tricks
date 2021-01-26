# clis

* log
	* `git log --online` - 简洁展示展示
	* `git log --all` - 展示所有分支的提交记录
	* `git log --all --decorate` - 展示所有分支的提交记录，并展示该提交来自哪个分支
	* `git log --all --decoreate --graph`  - 图形化展示
	* `git log -n 2` / `git log -2` - 展示最近的 2 条提交
	* `git log --after='2015-3-1' --before='2015-5-1'` - 设置筛选区间
	* `git log --follow index.php` - 跟踪单个文件的提交记录
	* `git shortlog` - 简洁展示每一个contributor&commit
	* `git log --author='Alexey'` - 只展示该 contributor 的提交记录
	* `git log --grep='redirect'` - 在提交记录中查找符合条件的提交记录
* add
	* `git add -p` - 交互式暂存同一个文件的部分修改
		*  y — stage the hunk
		* n — don t stage the hunk
		* e — edit the hunk
		* d — exit the process
		* s — split the hunk
* reset
	* `git reset --soft/--mixed/--hard HEAD~` 后退到指定的提交
* revert
	* `git revert commit-hash` - revert 指定的提交
* refs
	* `git reflog` - 展示所有 ref 指向修改相关的记录，可以根据该记录找回不小心删除掉的提交
* fsck - 列出所有丢失的 commits
	* `git fsck --lost-found` - 列出所有丢失的 commits
* rebase
	* `git rebase master` - 将 master 分支上的变更放在当前分支起点之前
	* `git pull --rebase origin master` - 将 pull 的远程 master 分支 rebase 到当前分支
* cherry-pick
	* `git cherry-pick commit-hash` - 挑选单独的 commit 放到当前分支的最后

## Tag
* lightweight - 只包含 tag name & commit hash
* annotated - 包含 tag name & tagger & tag message

### shortcut

* `git tag latest_commit` - lightweight
* `git tag -a latest_commit -m "this is the latest commit"` - annotated
* `git push origin --tags` - 推送本地的 tags 到远程库
* `git push origin Atutor_1.4.1` - 推送指定的 tag 到远程库