### git

-  初始准备

```bash
# 设置所有git仓库的提交者和邮箱, 必须
git config --global user.name "zzzzer"
git config --global user.email "zzzzer91@gmail.com"

# 生成密钥, 把.ssh中的id_rsa.pub的内容添加到github, 就可以上传了
ssh-keygen -t rsa -C "zzzzer91@gmail.com"
```

- 基本命令

```bash
# 把当前文件夹变为git可以管理的仓库
git init
# 把文件提交的暂存区, 可用"."代表当前目录下所有文件
git add <file_name>
# 撤销暂存区内容, 不会覆盖工作区
git reset HEAD <file_name>
# 把暂存区文件正式提交, 形成一个版本, "xxx" 是本次提交描述
git commit -m "xxx"
# 把工作区内容撤销
# 若暂存区有内容, 则回到暂存区, 暂存区内容还会存在
# 若暂存区无内容, 则回到版本库
git checkout -- <file_name>
# 查看仓库当前状态
git status
# 查看哪些内容被修改
git diff <file_name>
# 显示版本记录, --pretty=one 代表精简输出
git log --pretty=one
# 回到某一版本, 不建议使用
git reset --hard HEAD^ # 回到上一版本
git reset --hard HEAD^^ # 回到上上个版本
git reset --hard HEAD~100 # 回到上100个版本
git reset --hard <版本号> # 回到指定版本
# 添加标签
git tag <tag_name> # 默认打在当前分支上
git tag <tag_name> <commit_id> # 打在指定分支上
# 查看标签
git tag
# 查看指定标签信息
git show <tag_name>
```

- 分支命令

```bash
# 如当前分支有修改, 还没提交, 要把当前现场状态储存起来, 这样才能切换分支
git slash # 储存当前工作状态
git slash list # 查看所有存储起来到状态
git slash apply # 恢复之前状态, 但slash的内容不删除
git slash pip # 恢复之前状态,slash的内容删除

# 创建并切换到dev分支, 相当于 git brach dev;git checkout dev
git checkout -b dev
# 查看所有分支
git branch
# 删除dev分支
git branch -d dev
# 强制删除dev分支
git branch -D dev
# 切换回master分支
git checkout master
# 合并dev分支到当前分支
git merge dev
```

- 远程仓库

```bash
# 把本地仓库和远程仓库关联, origin是给远程仓库起的别名
git remote add origin git@github.com:zzzzer91/<远程仓库名.git>
# 第一次向远程仓库推送用这条, 以后可使用下面的
git push -u origin master
# 推送到远程仓库
git push
# 抓取远程仓库内容, 合并到本地
git pull
# 查看远程仓库
git remote -v
# 删除与远程仓库关联
git remote rm origin
```

