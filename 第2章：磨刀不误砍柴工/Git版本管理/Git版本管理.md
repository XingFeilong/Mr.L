# Git 版本管理

* 生成develop开发分支：git checkout -b develop origin\/develop\(从远程origin\/develop checkout出一个分支\)或git checkout -b develop\(从本地checkout出一个分支\)

* 查看远程路径: git remote -v

* 将本地分支推送到远程形成分支: git push origin lianghuiyuan\(分支名\)

* git提交发布步骤\(形成稳定版本后，在master分支上打tag并递交tags\)

> 1、git checkout master
> 
> 2、git merge develop
> 
> 3、git push
> 
> 4、git tag -a v2.1.0
> 
> 5、git push --tags

