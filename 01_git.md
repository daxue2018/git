# 基本命令
git config --global 设置当前用户范围内配置，对机器上其他用户无效

git config --system 对当前机器上所有用户有效

git config 对当前所在git项目有效

git config --list 查看所有配置项

git config user.name 查看某个配置项

git init 使用git托管代码，执行后生成.git目录，默认创建master分支

git add --all .

git commit -m 'init'



git log --patch -2  （-patch 可以显示每次提交之间的diff，同时-n可以指定显示最近几个commit。可以查看最近两次commit的代码差异，进行code review比较方便）

git log --stat 显示每次commit的统计信息，包括修改了几个文件，插入多少行，删除多少行

git log --pretty=online 可以每个commit显示一行，就是一个commit SHA-1和一个提交说明。用git log --pretty=format:"%h -%an,%ar:%s" 可以显示短hash、作者、多久以前、提交说明

git log --oneline --abbrev-commit --graph 可以看到整个commit树结构，--oneline 显示一行，包括commit标识符、SHA-1hash值40位、提交备注、分支、HEAD指向哪个commit；


git reset --hard HEAD^ 回退上一个版本，将仓库、暂存区、工作区全部恢复到上一个commit。HEAD^表示commit的上一个commit

git reset --hard HEAD-5 回退到HEAD之前的倒数第五个commit的状态

git reset --hard hash 回退到指定commit的hash值

git reflog 






