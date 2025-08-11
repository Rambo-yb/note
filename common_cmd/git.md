```shell
git branch 查看当前分支
git branch -r 查看所有分支
git chackout [分支名] 切换分支
git checkout -b feature_HWP-2012 remotes/origin/feature_HWP-2012

分支创建：
git checkout -b feature_NGD 创建分支
git push origin feature_NGD 推送远端
git branch --set-upstream-to=origin/feature_NGD feature_NGD 本地关联远端分支

查看用户名 git config user.name
查看邮箱 git config user.email
查看配置信息 git config --list

修改git用户名和邮箱，使用新的镜像需要先修改用户名和邮箱
git config --global --replace-all user.name "yuanbo05115"
git config --global --replace-all user.email "yuanbo05115@klicen.com"

git命令忽略文件权限修改
git config core.fileMode false
git --global config core.fileMode false

不忽略
git config core.fileMode true

git代码提交：
git add 修改文件
git reset HEAD + 文件名 撤销add文件
git rm -r 文件名 删除已提交文件
git checkout HEAD -- 文件名 撤销rm文件
git commit -m "[#jira子任务storyID]1.修改描述；2. ....." //story任务一次只能提交一个子任务修改
git commit -m "[#jira子任务bugID#jira子任务bugID]1.修改描述" //bug可以一次提交多个子任务修改
git push origin 父任务分支

git log 查找日志获取本次commit的ID信息
git reset --soft commitId^ 回退commit
git reset --hard commitId 回退到某个commit

git rm -r -n --cached 文件/文件夹
git rm -r --cached 文件/文件夹

git chackout ./ 还原本分支所有修改

git stash 将当前未提交修改暂存到缓存中
git stash list 缓存列表
git stash pop 移出缓存，回到stash状态

打补丁
git am patch文件

撤销回退，返回未来版本
git reflog 查看命令历史，以确定回到未来的哪个版本
git reset --hard 版本号

打tag：
git tag -a v1.0.0 -m "version v1.0.0" 创建带有注释的标签
git tag 查看所有标签

git push origin v1.0.0 推送标签到远程仓库
git push --tags 推送本地所有标签
git tag -d v1.0.2 删除本地tag
git push origin :refs/tags/v1.0.2 删除远端tag
git show v1.0.2 查看tag标签

分支合并：
git checkout master 切到接收改动的分支
git merge develop 将指定分支合并到当前分支
```