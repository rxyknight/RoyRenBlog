---
layout: post
title: 'Useful Git Commands'
date: '2015-09-15'
header-img: "img/post-bg-web.jpg"
tags:
     - git
author: 'Roy Ren'
---

Git is a version control system (VCS) that is used for software development and other version control tasks. As a distributed revision control system it is aimed at speed, data integrity, and support for distributed, non-linear workflows.

## Clone Project

	git clone http://xxxxxx.xxx/xxxxx
	
## Checkout from remote branch to local branch

	git checkout -b [my_local_branch] [my_remote_branch]
	
e.g. Checkout from remote branch dev/1.0.0 to local branch feature/1.0.0_my.

	git checkout -b feature/1.0.0_my dev/1.0.0
	
## Track branch

	git branch --set-upstream-to=origin/[my_remote_branch] [my_local_branch]

or

	git push -u origin [my_remote_branch]   //Note: will push

e.g. The local branch feature/1.0.0_my is managed to the remote branch feature/1.0.0_my，

	git branch --set-upstream-to=origin/feature/1.0.0_my feature/1.0.0_my

Since the previously created branch was fetched from dev/1.0.0, the local feature / 1.0.0_my is associated with the remote dev / 1.0.0 branch, and the git push (without other) is executed, and the feature/1.0.0_my branch is new The code will be uploaded to the dev/1.0.0 branch, so we need to re-associate the branch, of course, when you can git push designated remote branch.

## Submit code

	git add .

git commit -am '注释说明'
增加tag
1
2
3
4
5
git tag -a [tag_name] -m '注释说明'   //在需要的时候打tag

git push origin [tag_name]  //上传tag到远程仓库

git push origin --tags  //推送所有本地没有提交的tag
推送本地代码到远端
1
2
3
git push  //推送到默认关联分支

git push origin [remote_branch]  //推送到指定分支
切换分支
1
2
3
git checkout [branch_name]

git checkout -b [branch_name] //没有分支的话新建改分支
合并分支
1
git merge [branch_name]   //将branch_name分支合到当前分支
获取远端更新
1
2
3
git pull  //获取更新自动合并到本地分支

git fetch //获取更新单不自动合到本地分支
查看本地分支
1
git branch
查看远程分支
1
git branch -r
删除分支
1
git branch -d [branch_name]  //-d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项
合并分支
1
git merge [branch_name]  //将名称为[brand_name]的分支与当前分支合并
删除远程分支
1
git push origin :[branch_name]
恢复某个文件
1
git checkout -- [filename]  //注意,两个短横线左右都有空格
撤销本地上一次提交
1
git reset --soft HEAD^ 
撤销远程最后两次提交
1
git reset --soft HEAD~2 