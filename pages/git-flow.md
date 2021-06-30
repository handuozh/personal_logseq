- source
	- https://zhuanlan.zhihu.com/p/140553749
- `fork` and `pull request`
	- `merge request`
- Git flow
  collapsed:: true
	- too complicated
	- a main branch and a develop branch
	- ![gitlab_flow_gitdashflow.png](../assets/gitlab_flow_gitdashflow_1624693170826_0.png)
- Github flow  (fork flow)
  collapsed:: true
	- **use fork and pull request**
	- clean and straightforward
	- merging everything into `main` branch, too simple
	- ![gitlab_flow_github_flow.png](../assets/gitlab_flow_github_flow_1624693244273_0.png)
- gitlab-flow  (branch flow)
	- Need a pre-production stage
	- use branches
	- ![gitlab_flow_environment_branches.png](../assets/gitlab_flow_environment_branches_1624693357664_0.png){:height 281, :width 250}
	- `pre-production` branch as a buffer
		- we can name it beta-{version}
	- `production` branch
		- release-{version}
	- ? hotfix
		- Do we hot-fix based on `master` branch or `production` branch?
			- Normally `production` fix
		- If based on `production` branch, how to merge back to `master` branch
	- Steps
		-
		  1. 新的迭代开始，所有开发人员从主干master拉个人分支开发特性
		-
		  2. 开发完成后，在迭代结束前，合入主干
		-
		  3. 从主干拉取要发布的分支，我们是这么命名的：release/(version)，将这个分支部署到测试环境进行测试
		-
		  4. 测出的bug，通过从release/(version)拉出分支进行修复，修复完成后，再合入release/(version)
		-
		  5. 正式发布版本，如果上线后，又有bug，根据4的方式处理6. 等发布版本稳定后，将release/(version)反合入主干7. 上述过程，不影响下个迭代任务的开发，可以继续往主干合入代码
- Principles
	- 迭代任务都以issue方式跟踪，版本规划都以milestone跟踪
	- 设置commit规范检查
	- 设定release/(version)分支为保护分支，不允许直接推送，只能通过merge
	- 创建每个merge都必须关联issue，都设置了merge模版描述
	- 合入需要CI通过，检视闭环，多人看护审核通过 ^^这个未来进行^^
-