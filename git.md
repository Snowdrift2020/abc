# Git 命令详解

这份文档用于快速理解和查询 Git 常用命令，适合本地项目上传 GitHub、日常修改提交、分支管理和常见问题排查。

## 1. Git 是什么

Git 是一个版本控制工具，用来记录文件的修改历史。

GitHub 是一个代码托管平台，可以把本地 Git 仓库上传到网络上，方便备份、协作和分享。

简单理解：

- Git：本地版本管理工具
- GitHub：远程仓库平台
- commit：一次本地版本记录
- push：把本地提交上传到远程仓库
- pull：从远程仓库拉取最新内容

## 2. 查看 Git 信息

### 查看 Git 版本

```powershell
git --version
```

### 查看当前 Git 配置

```powershell
git config --list
```

### 查看用户名

```powershell
git config --global user.name
```

### 查看邮箱

```powershell
git config --global user.email
```

### 设置用户名

```powershell
git config --global user.name "你的名字"
```

### 设置邮箱

```powershell
git config --global user.email "你的邮箱"
```

## 3. 创建本地仓库

进入项目目录：

```powershell
cd C:\Users\29834\Desktop\abc
```

初始化 Git 仓库：

```powershell
git init
```

执行后，当前目录下会出现一个隐藏目录：

```text
.git
```

这个目录保存 Git 的版本记录，不要手动删除。

## 4. 查看文件状态

```powershell
git status
```

常见状态：

```text
Untracked files
```

表示新文件还没有被 Git 跟踪。

```text
Changes not staged for commit
```

表示文件修改了，但还没有加入暂存区。

```text
Changes to be committed
```

表示文件已经加入暂存区，等待提交。

```text
nothing to commit, working tree clean
```

表示当前没有需要提交的修改。

## 5. 添加文件到暂存区

添加单个文件：

```powershell
git add 文件名
```

例如：

```powershell
git add server-setup-guide.md
```

添加当前目录下所有修改：

```powershell
git add .
```

添加所有 Markdown 文件：

```powershell
git add *.md
```

## 6. 提交修改

提交到本地仓库：

```powershell
git commit -m "提交说明"
```

例如：

```powershell
git commit -m "update docs"
```

提交说明建议写清楚这次改了什么，比如：

```powershell
git commit -m "add git command guide"
git commit -m "fix server setup steps"
git commit -m "update ssh login notes"
```

## 7. 查看提交记录

查看完整提交记录：

```powershell
git log
```

查看简洁提交记录：

```powershell
git log --oneline
```

查看最近 3 条提交：

```powershell
git log --oneline -3
```

## 8. 分支命令

### 查看当前分支

```powershell
git branch
```

当前分支前面会有一个星号：

```text
* main
```

### 创建分支

```powershell
git branch 分支名
```

例如：

```powershell
git branch dev
```

### 切换分支

```powershell
git switch 分支名
```

例如：

```powershell
git switch dev
```

### 创建并切换分支

```powershell
git switch -c 分支名
```

例如：

```powershell
git switch -c feature-docs
```

### 合并分支

先切回 main：

```powershell
git switch main
```

再合并 dev：

```powershell
git merge dev
```

### 删除本地分支

```powershell
git branch -d 分支名
```

如果分支没有合并，Git 会阻止删除。强制删除可以用：

```powershell
git branch -D 分支名
```

## 9. 设置主分支为 main

有些 Git 默认创建 `master` 分支。现在 GitHub 常用 `main`。

把当前分支改名为 main：

```powershell
git branch -M main
```

## 10. 绑定 GitHub 远程仓库

使用 HTTPS 地址：

```powershell
git remote add origin https://github.com/用户名/仓库名.git
```

例如：

```powershell
git remote add origin https://github.com/Snowdrift2020/abc.git
```

使用 SSH 地址：

```powershell
git remote add origin git@github.com:用户名/仓库名.git
```

例如：

```powershell
git remote add origin git@github.com:Snowdrift2020/abc.git
```

查看远程仓库：

```powershell
git remote -v
```

正常输出类似：

```text
origin  git@github.com:Snowdrift2020/abc.git (fetch)
origin  git@github.com:Snowdrift2020/abc.git (push)
```

## 11. 修改远程仓库地址

如果远程地址写错了，可以修改：

```powershell
git remote set-url origin 新地址
```

例如改成 SSH：

```powershell
git remote set-url origin git@github.com:Snowdrift2020/abc.git
```

例如改成 HTTPS：

```powershell
git remote set-url origin https://github.com/Snowdrift2020/abc.git
```

## 12. 删除远程仓库别名

如果不小心写错远程名，比如写成了 `orgin`：

```powershell
git remote remove orgin
```

然后保留正确的 `origin`。

## 13. 推送到 GitHub

第一次推送：

```powershell
git push -u origin main
```

`-u` 的作用是把本地 `main` 分支和远程 `origin/main` 分支关联起来。

第一次成功后，以后只需要：

```powershell
git push
```

## 14. 从 GitHub 拉取更新

如果远程仓库有别人提交的内容，或者你在 GitHub 网页上修改过文件，可以拉取：

```powershell
git pull
```

完整写法：

```powershell
git pull origin main
```

## 15. 日常更新文档的标准流程

修改文件后，执行：

```powershell
git status
git add .
git commit -m "update docs"
git push
```

如果只想提交某一个文件：

```powershell
git status
git add server-setup-guide.md
git commit -m "update server setup guide"
git push
```

## 16. 查看文件修改内容

查看还没有暂存的修改：

```powershell
git diff
```

查看已经暂存、准备提交的修改：

```powershell
git diff --staged
```

查看某次提交改了什么：

```powershell
git show 提交ID
```

例如：

```powershell
git show 88822b8
```

## 17. 撤销修改

### 撤销某个文件的工作区修改

注意：这个命令会丢弃文件当前未提交的修改。

```powershell
git restore 文件名
```

例如：

```powershell
git restore server-setup-guide.md
```

### 取消暂存

如果已经执行了 `git add`，但还没有 commit，可以取消暂存：

```powershell
git restore --staged 文件名
```

例如：

```powershell
git restore --staged server-setup-guide.md
```

### 修改最后一次提交说明

```powershell
git commit --amend -m "新的提交说明"
```

如果最后一次提交已经推送到 GitHub，修改提交历史要谨慎。

## 18. 克隆远程仓库

从 GitHub 下载仓库到本地：

```powershell
git clone 仓库地址
```

例如：

```powershell
git clone git@github.com:Snowdrift2020/abc.git
```

或者：

```powershell
git clone https://github.com/Snowdrift2020/abc.git
```

## 19. SSH 登录 GitHub

测试 SSH 是否配置成功：

```powershell
ssh -T git@github.com
```

成功时会看到类似：

```text
Hi Snowdrift2020! You've successfully authenticated, but GitHub does not provide shell access.
```

后半句不是错误，表示 GitHub 不提供 shell 登录，但 Git 权限已经正常。

## 20. 生成 SSH Key

生成 ED25519 SSH key：

```powershell
ssh-keygen -t ed25519 -C "你的GitHub邮箱"
```

出现保存路径提示时，直接回车：

```text
Enter file in which to save the key (.../.ssh/id_ed25519):
```

出现密码提示时，也可以直接回车：

```text
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

复制公钥：

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

然后打开 GitHub SSH keys 页面：

```text
https://github.com/settings/keys
```

点击 `New SSH key`，粘贴保存。

## 21. .gitignore 文件

`.gitignore` 用来告诉 Git 哪些文件不要提交。

常见内容：

```gitignore
node_modules/
.env
*.log
dist/
build/
.DS_Store
Thumbs.db
```

创建 `.gitignore` 后，提交它：

```powershell
git add .gitignore
git commit -m "add gitignore"
git push
```

注意：如果某个文件已经被 Git 跟踪，再写进 `.gitignore` 不会自动取消跟踪。

取消跟踪但保留本地文件：

```powershell
git rm --cached 文件名
```

例如：

```powershell
git rm --cached .env
```

然后提交：

```powershell
git commit -m "stop tracking env file"
```

## 22. 常见报错

### fatal: 'origin' does not appear to be a git repository

原因：远程仓库名可能写错，或者没有添加 `origin`。

查看：

```powershell
git remote -v
```

如果看到 `orgin`，说明拼错了。可以改名：

```powershell
git remote rename orgin origin
```

或者删除错的：

```powershell
git remote remove orgin
```

### fatal: remote origin already exists

原因：已经存在名为 `origin` 的远程仓库。

解决：

```powershell
git remote set-url origin 新地址
```

### Permission denied (publickey)

原因：SSH key 没有配置好，或者没有添加到 GitHub。

检查：

```powershell
ssh -T git@github.com
```

如果失败，重新生成 SSH key，并添加到 GitHub。

### Authentication failed

原因：HTTPS 登录失败。

GitHub 不再支持用账号密码直接推送。HTTPS 推送时，密码位置要使用 Personal Access Token。

更推荐使用 SSH：

```powershell
git remote set-url origin git@github.com:用户名/仓库名.git
```

### Recv failure: Connection was reset

原因：网络连接被重置，可能是网络、代理、VPN 或 GitHub 连接不稳定。

可以尝试：

```powershell
git push
```

如果多次失败，换网络、换代理节点，或者使用 SSH 地址。

### LF will be replaced by CRLF

这是 Windows 换行符提示，不是错误。

Git 发现文件使用 LF 换行，而 Windows 可能会转换成 CRLF。

通常可以忽略，不影响提交和推送。

## 23. 推荐命令速查

初始化仓库：

```powershell
git init
```

查看状态：

```powershell
git status
```

添加全部修改：

```powershell
git add .
```

提交：

```powershell
git commit -m "update"
```

推送：

```powershell
git push
```

拉取：

```powershell
git pull
```

查看远程仓库：

```powershell
git remote -v
```

查看提交记录：

```powershell
git log --oneline
```

查看分支：

```powershell
git branch
```

切换分支：

```powershell
git switch 分支名
```

## 24. 最常用工作流

### 第一次上传项目

```powershell
cd C:\Users\29834\Desktop\abc
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Snowdrift2020/abc.git
git push -u origin main
```

### 后续更新项目

```powershell
cd C:\Users\29834\Desktop\abc
git status
git add .
git commit -m "update docs"
git push
```

### 查看是否上传成功

```powershell
git status
git log --oneline -3
git remote -v
```

如果 `git status` 显示：

```text
nothing to commit, working tree clean
```

并且 GitHub 页面能看到最新文件，就说明上传成功。
