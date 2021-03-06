<p align='center'>
    <img src=images/logo@2x.png>
</p>

<h1 align='center'>GitNotes</h1>
<h4 align='right'>——Sean</h4>

学习廖雪峰老师的[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)的简单记录，主要是命令的汇总。


## 1 Git简介
[Git](https://git-scm.com/) 是目前世界上最先进的分布式版本控制系统（没有之一）。自动记录每次文件改动，还可以让同事协作编辑。

### 1.1  Git的诞生
- 1991年，Linus 创建了全世界志愿者共同参与的开源系统软件 Linux，这些志愿者将源代码通过 diff 的方式发送给 Linus，然后由 linus 本人手工方式合并代码，Linus 坚定反对使用速度慢且必须联网的 CVS 和 SVN，以及违背开源精神的其他付费商用版本控制系统；
- 2002年，鉴于随着 Linux 发展其代码库太大，同时社区弟兄的不满，Linus 选择了一个商业的版本控制系统 Bitkeeper，其东家 BitMover 出于人道主义授权 Linux 社区免费使用；
- 2005年，随着 Liunx 社区的牛人聚集，开发 Samba 的 Andrew 试图破解 BitKeeper 的协议被 BitMover 发现，收回免费授权；
- 于是 Linus 花了两周时间自己用 C 写了一个分布式的版本控制系统，这就是 Git，一个月之内，Linux 系统的源代码已经由 Git 管理了；
- 之后 Git 迅速成为最流行的分布式版本控制系统，尤其是2008年随着 GitHub 的上线，它为开源项目免费提供 Git 储存，无数项目开始迁移至 GitHub，如 jQuery、PHP、Ruby等。

### 1.2 集中式 vs 分布式
- 集中式，版本库存放在中央服务器，每次工作先要用自己电脑从中央服务器获取最新版本，集中式最大毛病是必须联网才能工作，如 CVS、SVN；
<p align='center'>
    <img src=images/集中式.jpg>
</p>

- 分布式，不存在“中央服务器”（虽然有一个充当的，但是作用仅是方便大家“交换”修改，没有它也可以工作），每个电脑都是一个完整版本库，工作不需联网，比集中式安全，如 Git；
<p align='center'>
    <img src=images/分布式.jpg>
</p>

Git 较于 SVN 等还有其他优势，如极其强大的分支管理等；其他集中式，如 IBM 的 ClearCase，微软的 VSS；其他分布式，如 Mercurial 和 Bazaar 等。但 Git 是最快、最简单也是最流行。


## 2 安装Git
- Linux，部分系统自带，否则通过 `$ sudo apt-get install git` 即可安装；
- Mac：
   - 先安装 homebrew，再通过其安装 Git;
   - 推荐方法，直接从 AppStore 安装 Xcode，其集成了 Git（不过还需在其 Preference 中 Downloads，并 Install）。
- Windows，[Git 官网](https://git-scm.com/)下载，按默认安装即可。

**设置 username**：`$ git config --global user.name "Your name"`

**设置 useremail**：`$ git config --global user.email "email@example.com"`

上述二者冒充可查，`--global` 表示在这台机器上的所有仓库都会使用这个配置，当然也可不同仓库指定不同用户名和邮箱地址。


## 3 创建版本库
版本库，简单理解为一个目录，里面所有文件可以被 Git 管理，每个文件的修改、删除都能被跟踪，可以追踪历史、还原等，又叫仓库、repository。

**初始化仓库**：`$ git init`

**将文件添加到仓库**，分两步（第一步可多次使用）：
   - `$ git add <file>`

   - `$ git commit -m <message>`
   

## 4 时光机穿梭
**查看仓库当前状态**：`$ git status`

### 4.1 版本回退
**查看提交日志**：`$ git log( --pretty=oneline)`，由近及远

**回退到上一个版本**：`$ git reset --hard HEAD^`，HEAD指当前版本，^个数表示回退个数，或 `$ git reset --hard ~n` 回退到倒数第n个

**回退到某一个版本**：`$ git reset --hard <commit_id>`，向前向后都行

**查看记录的每一次命令**：`$ git reflog`

### 4.2 工作区和暂存区
- 工作区，Working Directory，电脑能看到的文件夹；
- 版本库，Repository，隐藏的 *.git* 文件夹，包括暂存区（又称 stage 或 index）和 Git 自动为我们创建的第一个分支 master，以及指向 master 的指针 HEAD，前文“将文件添加到仓库”的两步分别如图所示。
<p align='center'>
    <img src=images/repo.jpg>
</p>

**查看修改内容**：
- `$ git diff`，比较工作区和stage;
- `$ git diff --cached`，比较stage和index;
- `$ git diff HEAD`，比较工作区和index;

### 4.3 管理修改
Git 比其他版本控制系统设计得优秀的一个原因，是因为 Git 跟踪并管理的是修改，而非文件。

如果“第一次修改 -> git add -> 第二次修改 -> git commit”，则第二次修改不会被提交，即每次修改，如果不用 git add 到暂存区，那就不会加入到 commit 中。

### 4.4 撤销修改
**撤销工作区全部修改**：`$ git checkout -- <file>`，让 file 回到最近一次 add/commit 时状态：
- 如果 file 修改后没有 add，则撤销到和版本库一样；
- 如果 file 已经 add 后又修改，则撤销到和 add 后的状态。

**撤销暂存区全部修改**：`$ git reset HEAD <file>`，HEAD 表示最新的版本，详见 “git reset --help”

### 4.5 删除文件
**删除文件**：`$ git rm <file>`

- “git add -> git commit -> rm -> git rm -> git commit”，如果 “rm” 是误删，可及时通过 “git checkout -- file” 从 stage 恢复；
- “git add -> git commit -> git rm -> git commit”，如果 “git rm” 是误删，则 stage 中也被删，可及时先通过 “git reset HEAD file” 撤销暂存区修改，再通过 “git checkout -- file” 从 stage 恢复；


## 5 远程仓库
注册 [GitHub](https://github.com/)，并进行两步设置：
- **创建SSH keys**：`$ ssh-keys -t rsa -C "youremail@example.com"`，会在主目录下生成一对密钥对：*id_rsa* 和 *id_rsa_pub*，前者是私钥，不能泄露，后者是公钥，可以告诉他人；
- 登录 GitHub 添加公钥，“ setting -> SSH and GPG keys”。

### 5.1 添加远程库
步骤：
- GitHub 网站 “create a new repo”，设置仓库名 repo_name；
- `$ git remote add origin git@github.com:<github_name>/<repo_name>.git`；
- **将本地提交推到远程**：`$ git push origin master`，第一次需要使用 `$ git push -u origin master` 将本地 master 分支和远程 master 分支关联。

### 5.2 从远程库克隆
Git 支持多种协议，默认的 "git://" 使用 ssh，也可以使用 https（速度慢，且每次推送需要输入口令）等其他协议：
- `git clone git@github.com:<github_name>/<repo_name>.git`
- `git clone https://github.com/<github_name>/<repo_name>.git`
- `git clone git://github.com/<github_name>/<repo_name>.git`


## 6 分支管理
<p align='center'>
    <img src=images/分支.png>
</p>

### 6.1 创建与合并分支
分支：Git 将每次提交串成的一条时间线。目前为止只有默认的 master 主分支。

**查看当前分支**：`$ git branch`

**创建分支**：`$ git branch <branch_name>`

**切换分支**：`$ git checkout <branch_name>`

**创建同时切换至新分支**：`$ git checkout -b <branch_name>`，等同上述二者

**合并某分支到当前分支**：`$ git merge <branch_name>`

**删除分支**：`$ git branch -d <branch_name>`

### 6.2 解决冲突
<p align='center'>
    <img src=images/冲突.png>
</p>

如上图，如果 master 分支和 feature1 分支都有更新，则 merge 二者可能会报冲突，这时可以通过 git status 查看冲突，并手动解决冲突，再提交，最后合并。可以通过 “git log --graph( --pretty=oneline --abbrev-commit)” 查看分支合并图。

### 6.3 分支管理策略
**使用 no-ff 方式合并分支**：`git merge --no-ff -m <message> <branch_name>"`，可以保留分支信息，“git merge” 默认的 fast froward 模式不会保留分支信息。

实际开发分支管理原则：
- master 分支：非常稳定的，仅用来发布新版本；
- dev 分支：不稳定的，日常干活的分支；
- 每个人有各自分支，不时往 dev 分支合并。

<p align='center'>
    <img src=images/分支策略.png>
</p>

### 6.4 bug分支
修复 bug 时，我们会通过创建新的分支进行修复，修复好后进行合并，并删除。如果手头工作还没有完成，可以在手头分支把工作现场储存起来。

**储存工作现场**：`$ git stash`

**查看 stash 列表**：`$ git stash list`

**恢复到工作现场，方式1**：`$ git stash pop`，恢复同时也删除该 stash

**恢复到工作现场，方式2**：
- **恢复到指定 stash**：`$ git stash apply stash@{n}`；
- **删除 stash 内容**：`$ git stash drop`。

### 6.5 feature分支
实际开发中，每添加一个新功能最好新建一个 feature 分支。
- 顺利的话，和 bug 分支类似，合并然后删除；
- 需要放弃 feature，则使用 “git branch -d branch_name” 删除，对于未合并过的分支需要使用 D 替代 d，且删后找不回。

### 6.6 多人协作
**查看远程库信息**：`$ git remote( -v)`，括号内表示查看详细信息

**从远程抓取分支**：`$ git pull`，有冲突需先解决

多人协作通常工作模式：
- 首先，试图 “git push origin bransh_name” 推送自己修改；
- 若推送失败，则是因为远程分支比你的本地更新，需要先通过 “git pull” 试图合并；
- 若合并有冲突，则先解决冲突，并在本地提交；
- 不存在冲突后，再利用 “git push origin branch_name”推送。

**建立本地分支和远程分支链接**：`$ git branch --set-upstream-to=origin/<branch_name> <branch_name>`，解决 “git pull” 时可能会出现的 “no tracking infromation”。

### 6.7 rebase
**将本地未 push 的分支提交历史整理成直线**：`$ git rebase`，便于查看历史提交的变化，缺点是本地的分支提交已经被修改过了。


## 7 标签管理
标签：tag，版本库快照，实际是指向某个 commit 的指针（和分支很像，但是标签不能移动），唯一确定打标签时刻的版本。

标签总是与某个 commit 挂钩，如果这个 commit 出现在多个分支，则这些分支上都会看到这个 tag。

### 7.1 创建标签
**新建默认标签**：`$ git tag <tag_name>`，默认 HEAD

**新建指定标签**：`$ git tag -a <tag-name> -m <message> <commit_id>`，指定 commit_id 打标签，并附 message 说明文字

**查看指定名称标签**：`$ git show <tag_name>`

**查看所有标签**：`$ git tag`

### 7.2 操作标签

**推送本地标签到远程**：`$ git push origin <tag_name>`

**一次性推送所有标签**：`$ git push origin --tags`

**删除标签**：`$ git tag -d <tag_name>`

**删除远程标签**，分两步：
- 先删除本地，同上；
- **删除远程**：`git push origin :refs/tags/<tag_name>`


## 8 使用GitHub
- 在 GitHub 上，可以任意 Fork 开源仓库；

- 自己拥有 Fork 后的仓库的读写权限；

- 可以推送 pull request 给官方仓库来贡献代码。


## 9 使用码云
[码云](https://gitee.com)：国内 Git 托管服务，较 GitHub 网速快很多。

和GitHub相比，码云也提供免费的Git仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。

**删除关联的远程库**：`git remote rm origin`

通过 “git remote add gitee git@gitee.com:<gitee_name>/<repo_name>.git”、“git remote add github git@github.com:<github_name>/<repo_name>.git”，Git 可以同时同步到多个远程库:
<p align='center'>
    <img src=images/gitee.JPG>
</p>


## 10 自定义Git
在安装 Git 一节中，我们已经配置了 user.name 和 user.email，实际上，Git还有很多可配置项。

**显示颜色**：`$ git config --global color.ui true`

### 10.1 忽略特殊文件
在 Git 工作区的根目录下创建一个特殊的 *.gitignore* 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。Windows 下，不能直接以这个文件名创建文件，可以通过文本编辑器中“保存”或“另存为”来解决。

我们不需要从头写 *.gitignore* 文件，GitHub 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)。

忽略文件的原则是：
- 忽略操作系统自动生成的文件，比如缩略图等；
- 忽略编译生成的中间文件、可执行文件等；
- 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

**强制添加忽略文件到 Git**：`$ git add -f <file>`

**查找限制文件的规则**：`$ git check_ignore -v <file>`

### 10.2 配置别名
**配置命令别名**：`$ git config --global alias.<command_alias> command`，如:
- “ git config --global alias.st status”，则以后 “git st” 等价于 “git status”；
- “ git config --global alias.unstage “reset HEAD””，则以后 “git unstage file” 等价于 “git reset HEAD file”。

上述设置的配置文件是位于工作区目录的 *.git/config* 文件；如果不设置 global 参数，则设置仅对当前仓库有效，配置文件是位于主目录的 *.gitconfig* 文件。对于二者，均是删除对应行即删除别名。

### 10.3 搭建Git服务器
步骤：
- **安装 Git**：`$ sudo apt-get install git`
- **创建运行用户**：`$ sudo adduser git`
- **创建证书登陆**：收集所有需要登录的用户的公钥，就是他们自己的 *id_rsa.pub* 文件，把所有公钥导入到 */home/git/.ssh/authorized_keys* 文件里，一行一个
- **初始化仓库**：
   - 先选定目录，如 */srv/sample.git*，进行初始化，建立裸仓库（纯粹为共享），`$ sudo git init --bare sample.git`
   - 更改权限，将 owner 改为 git，不让用户直接登陆到服务器上改工作区（服务器上仓库通常以 *.git* 结尾）：`$ sudo chown -R git:git sample.git`
- **禁用 shell 登陆**，出于安全考虑，第二步创建的 git 用户不允许登录 shell ，这可以通过编辑 */etc/passwd* 文件的以下行完成：“git:x:1001:1001:,,,:/home/git:/bin/bash” -> “git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell”
- **克隆远程仓库**：`$ git clone git@server:/srv/sample.git`

[Gitosis](https://github.com/sitaramc/gitolite)是一个可以方便管理密钥和管理权限的工具。


## 11 期末总结
- [Git 官网](http://git-scm.com)
- [Git Cheat Sheet](./git-cheatsheet.pdf)
