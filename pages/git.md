- #related  [[git-flow]]
- Basics
  heading:: true
  background_color:: rgb(73, 125, 70)
	- 基本功能图解
	  heading:: true
		- ![图解1](http://marklodato.github.io/visual-git-guide/basic-usage.svg.png)
		- git add files 把当前文件放入暂存区域。 stage 注意： git add 同时具有添加到跟踪列表的功能
		- git commit 给暂存区域生成快照并提交history区
		- git checkout – files 把文件从暂存区域复制到工作目录，用来丢弃本地修改.(注意将会丢弃本地修改.)
	- 合并图解
	  heading:: true
		- ![合并](http://marklodato.github.io/visual-git-guide/basic-usage-2.svg.png)
		- git commit -a 相当于运行 git add 把所有当前目录下的文件加入暂存区域再运行 git commit.
			-
			  #+BEGIN_NOTE
			  如果文件从没有被跟踪过（untracked）仍然需要用 git add . 先添加
			  #+END_NOTE
		- git checkout HEAD – files 回滚到复制最后一次提交.
	- 删除文件
		- git rm files 从暂存区 和 工作目录同时删除，然后可以 commit 到版本库.
		- rm files 从工作 目录删除 因为暂存区没有删除，因此提示：修改没有暂存.
			- 如果需要提交到版本需要 git add . 暂存 然后在 commit
		- git rm –cached f3.py 从版本库 和暂存区删除（不再跟踪） 文件仍然存在于工作目录（untracked状态）
- lazygit
- rebase
	- 提取我们在A分支上的改动，然后应用在B分支的代码上，完成类似于补丁的功能
	-