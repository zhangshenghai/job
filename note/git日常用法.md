#日志
```
推荐的 git 多人协同模式
前置约定
1. 以代码为主的项目,项目主负责人在项目初始化时,确保中心仓库有两个远程分支: master, dev.其中 master 是稳定的主线分支; dev 是从 master 上切出来的功能开发分支.
2. 建议项目负责人上 gitlab,在项目 Settings 里面将默认分支设定为 dev.
几点约定
尽可能的在自己的本地分支上开发新功能.
dev 分支功能开发分支,是可供运行与测试的 hot code 分支,请确保每个合并到 dev 分支的功能可运行,可测试.
master 分支为稳定主线分支,以在 master 分支打 tag 的静态版本来上线或回滚.
专人负责将 dev merge 到 master, 打 tag 和上线.
除非有必要,不建议将本地分支推送到远端
合并代码的基本原则: 先人后己
多人协作
开发排期较长(约定2天以上为长开发周期)
1. 参与项目,从中心仓库拉取代码
    $ git clone 工作项目的gitlab路径
2. 创建本地开发分支,以 feature 为例,具体的开发分支尽量取有意义的英文名
    $ git checkout master       # 从 master 分支创建最新开发分支,限定当前开发分支不依赖其他任何分支代码
    $ git pull origin master    # 容错操作
    $ git branch feature        # 创建本地开发分支
    $ git checkout feature      # 切到本地的开发分支
    $ git push origin feature   # 多人参与一个开发分支,所以需要将此分支推送到远端
  仅参与开发的同学,在执行完第1步后,执行以下命令:
  $ git fetch --all
  $ git checkout feature
3. 在本地分支开发新功能
4. 将新功能代码提交到本地分支中
    $ git add 新文件/修改的文件
    $ git commit            # 提交到本地
5. 将新功能推送到远端
    $ git push origin feature
6. 发起提测邮件进入测试流程.如果有问题,请重复 2-5 步;如果测试确认没有问题,发起上线.邮件形式通知相关人员
7. 项目负责人做 code review,并将 feature merge 到 master,完成上线
    $ git checkout master
    $ git pull origin master    # master 上有可能存在 hotfixes 代码
    $ git merge feature
    $ git push origin master
    打 tag,推送 tag 到远端,执行上线.
8. 完成上线后将新上线的功能 merge 到 dev 分支,并通知 dev 分支开发的同学拉取最新代码
    $ git checkout dev
    $ git pull origin dev   # 拉取远端最新代码
    $ git merge master      # 将 master 上 feature 功能 merge 到 dev 分支
    $ git push origin dev   # 将 feature 功能推到 dev 的远端
9. 为了防止以后误操作,做好善后
    $ git push origin :feature  # 删除已经完成上线的远端分支
    $ git branch -D feature     # 删除本地分支
dev 分支多人开发
适合短平快的开发模式,开发周期在2天以内

1. 参与项目,从中心仓库拉取代码
    $ git clone 工作项目的gitlab路径
2. 创建本地开发分支,以 feature 为例
    $ git branch feature    # 创建本地开发分支
    $ git checkout feature  # 切到本地的开发分支
3. 在本地分支开发新功能
4. 将新功能代码提交到本地分支中
    $ git add 新文件/修改的文件
    $ git commit            # 提交到本地
5. 切到 dev 分支,并更新 dev,将他人的成果 merge 到本地分支,确保代码与中心库一致
    $ git checkout dev
    $ git pull origin dev
    $ git checkout feature
    $ git merge dev
merge 如果有冲突,请解决冲突后,再执行第 4 步.如果没有冲突,刚可以将新功能代码 merge 到 dev 分支.
    $ git checkout dev
    $ git merge feature
6. 将新功能推送到远端
    $ git push origin dev
7. 发提测邮件,进入测试流程.如果有问题,请重复 2-6 步;如果测试确认没有问题,发起上线邮件申请.
8. 项目负责人 code reveiw,并将 dev merge 到 master,完成上线
    $ git checkout master
    $ git pull origin master
    $ git merge dev
    $ git push origin master
    打 tag,推送 tag 到远端,执行上线.
hotfixes
难免线上出现紧急问题,紧急修复流程如下:

1. 拉取最新线上代码,并创建紧急修复分支
    $ git checkout master
    $ git pull origin master
    $ git branch hotfixes
    $ git checkout hotfixes
2. 完成修复并提交代码
    $ git add 修复的代码文件
    $ git commit    # 提交到本地
    $ git checkout master
    $ git merge hotfixes
    $ git push origin master
3. 在线上预览机回归
4. 完成上线后的善后
    $ git checkout master
    $ git pull origin master
    $ git checkout dev
    $ git pull origin dev
    $ git merge master  # 把 hotfixes 的代码 merge 到 dev 分支
    $ git branch -D hotfixes    # 删除临时分支,防止以后误操作
申请测试的提测邮件模板
收件人: rd, qa, pm, op

标题: 『提测』xx功能

正文:

1、提测项目与分支

# 以 pandora 为例子
项目: pandora
分支: dev
2、功能点说明

1). 例行升级维护
  2). 其他
3、预计上线时间和其他说

# 请简单说明
申请上线的邮件模板
回复『提测』邮件,将标题换为『上线』

在正文件中贴上最后提交记录,形如:

07125f0..0ebf345  master -> master
以上内容作用有2点:

供 op 在上线后确认线上代码版本与代码库一致,确保代码上线正确性
回滚操作的重要依据.
git 交叉合并后出现的灵异现象
所谓的交叉合并类似场景为,从 master 上创建分支 dev和test,在 test 分支开发新功能,他人的新功能被合并到 dev 分支;这时将 dev 合并进 test,然后将 test 合并至 dev;之后将 test 合到 master 上线,完成上线后又将 master 合至 dev.
这样操作后出现的诡异现现象目前遇到两种:

1. 递归合并.合并两个分支时,没有任何文件需要合并,仅提示进行了递归合并. git diff 两个无端分支时没有差异,但人工查看代码时发现,有分支代码并没有合并进去.

提示语如下:
Already up-to-date!
Merge made by the 'recursive' strategy.

2. 每次合并都会出现几个与本分支功能无关的代码冲突,反复解决,反复冲突.

建议: 合并代码时一定要搞清楚合并方向,完成合并后在 push 之前,做好必要的 code review,利人利己.
git 多分支合并最佳实践
以 work-branch 为例,约定此分支为从最新的 master 分支创建的功能开发分支.

1. 创建功能开发分支
  $ git checkout master     # 切到 master 分支
  $ git pull origin master  # 拉取最新 master 分支代码,因为可能有新功能已经合并上线
  $ git checkout -b work-branch # 创建并切换新的分支
2. 在功能开发分支开发新功能,建议频繁按小功能完成点提交代码,完成自测
3. 将 master 最新功能合并进功能开发分支,并推送到远端
  $ git fetch --all     # 抓取所有远端分支的最新代码
  $ git merge origin/master     # 将远端 master 最新代码合并进入功能开发分支
如果有冲突,用 `git log -p 冲突文件` 查看改动作者,尽量和原作者共同裁决冲突.
  $ git push origin work-branch     # 将功能开发分支推送到远端
4. 将新功能分支 work-branch 合并到 develop
  $ git fetch --all     # 抓取所有远端分支的最新代码
  # 以下操作为容错操作,建议所有合并代码的同学按此指导操作,因为很多功能开分的同学会忘记合并 master 最新代码
  $ git checkout work-branch    # 切到待合并分支
  $ git pull origin work-branch # 拉取功能分支最新代码并在本地完成自动合并
  $ git fetch --all     # 抓取所有远端分支的最新代码
  $ git merge origin/master     # 将远端 master 最新代码合并进入功能开发分支
  $ git push origin work-branch # 将变化推送到远端
  # 正式开始合并操作
  $ git checkout develop    # 切换分支
  $ git pull origin develop # 拉取并在本地合并 develop 分支最新代码
  $ git fetch --all     # 抓取所有远端分支的最新代码,或者使用 git fetch -p,在抓取无端分支的同时,会清除无端已经删除而本地有记录的分支
  $ git merge origin/work-branch  # 将功能分支的远端 origin/work-branch 合并进 develop 本地分支
如果有冲突,用 `git log -p 冲突文件` 查看改动作者,尽量和原作者共同裁决冲突.
  $ git push origin develop # 将合并结果推送到远端,完成合并

按第 4 步如法炮制,可以完成 work-branch 到 master 的合并.将功能分支合并至 master,完成上线后一定要按第 4 步那样将 master 再合到 develop 中去,并删除远端的 work-branch.
版本控制之道
先人后己
小批量提交
走心的日志
It looks like git-am is in progress. Cannot rebase.
see

修复冲突
$ git apply PATCH --reject
$ edit edit edit
$ git add FIXED_FILES
$ git am --resolved
放弃更新
$ git am --abort
直接刪除 rebase-apply
$rm -rf .git/rebase-apply
rebase 还是 merge?
rebase和merge是代码观察者查看代码线性变化的不同维度,对最终合并的代码来讲,没有任何区别.merge是完全的以时间为线性轴,体现出源码在不同时间点上发生的变化;而rebase是提交源码作者为轴,将同一作者的提交在目标源码的最后基线上线性的合并,表现为分支功能的代码提交是线性的,而不是与协作者提交相穿插.

建议同分支合并使用rebase,跨分支合并使用merge,这样不会丢失关键的merge message.

将 svn 项目源码迁到 github/gitlab
$ svn log svn-pro-src | grep '^r' | awk '{print $3 " = " $3 " <" $3 "@UFO.com>"}' | sort -u > users.txt
$ git svn clone svn-pro-src --authors-file=users.txt --no-metadata
$ cd project
$ git remote add origin github/gitlab远程源
$ git push -u origin master
使用自己生成SSL证书时，Git报错的解决办法
git clone https://....

时报错，说证书校验有问题：

最简单的解决方法是加一个环境变量：

$ export GIT_SSL_NO_VERIFY=1
$ git config --global http.sslVerify false
OS X Yosemite(10.10)下无法使用git svn解决方法
自己在OS X下面没有找到好用的免费SVN客户端，又不想使用破解软件，更因为觉得SourceTree实在是太好，对它爱不离手，因此当自己遇到代码管理使用SVN的时候，还是习惯使用git svn做一个桥接，让代码在我本地是通过GIT来管理的。

近日电脑系统升级到了Yosemite后，使用git svn clone命令报错，错误信息如下： Can’t locate SVN/Core.pm in @INC (you may need to install the SVN::Core module)

添加Perl库链接 这个错误以前在10.9的时候也遇到过，Google一下就找到了解决方案。那个时候Perl库用的是5.16版本，现在系统升级到了5.18版本后，跟以前一样，添加两条软连接命令：

sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/SVN /System/Library/Perl/Extras/5.18/SVN
sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/auto/SVN/ /System/Library/Perl/Extras/5.18/auto/SVN
OS X El Capitan下git-svn无法使用
Unfortunately, the old solutions no longer work due to El Capitan’s System Integrity Protection:

$ sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/SVN /System/Library/Perl/Extras/5.18/SVN
ln: /System/Library/Perl/Extras/5.18/SVN: Operation not permitted
While you can disable SIP, that’s unnecessary in this case.

Here’s how you get git-svn working:

$ sudo mkdir /Library/Perl/5.18/auto
$ sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/SVN /Library/Perl/5.18/darwin-thread-multi-2level
$ sudo ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/auto/SVN /Library/Perl/5.18/auto/
You can’t write to /System, but you can still write to /Library.

git使用https方式记住密码
see git-scm see other

git使用https方式进行连接时，默认每次推送时都要输入用户名和密码。

git config credential.helper store

为当前仓库设置记住密码，设置后，只要在推送一次，以后就不需要用户名和密码了

Git 使用的简单汇总
1. 配置

 

git config --global user.name "your name"

git config --global user.email  mail@box.com

git config --global color.ui true

git config --global core.editor vi

git config --global alias.lol "log --graph --all"    设置alias，这样lol就是自己新的命令了。

 

2.基本使用

1.显示当前的配置信息

git config --list

 

2. 创建repo

从别的地方获取

git clone git://git.kernel.org/pub/scm/git/git.git



自己建立

mkdir test

cd test

git init

 

3. 显示状态

git status

 

4. commit

git add file.1 file.2 先增加文件，增加到index中。这样commit的时候才知道要commit哪些文件。

或者

git add -p   用来interactively选择哪些改变需要被commit

git commit -m "log message"

 

或者

git commit -a  自动检查应该commit什么文件。如果是新增的文件，仍然要使用git add来添加。

 

5. 显示以前的工作

git log 输出格式

git log

git log -p       显示patch

git log --stat   显示改动的一个总结

git log --graph  只显示当前branch的

git log --graph --all    显示所有branch的

git log --graph --all --decorate 显示branch的名字


git log --pretty=oneline, short, full, fuller  输出的log 形式不同

git log --pretty=format:"%h - %an, %ar : %s"   按照指定的格式输出。

关于--pretty的其他选项和具体的format格式，参考 git log --help中PRETTY FORMAT这部分。


git log --follow file.c

这个功能很有意思，尤其是当file.c被移动后。

通常我们会移动某个文件到某个目录下。如果这么做，git log是不能显示目录移动前的记录的。

那就加上 --follow吧。


git log的筛选

git log -2 -p   显示最近两次commit的log 和 diff

git log --author="Author Name" 筛选特定作者的log

git log --since="2012-2-23" --before="2012-2-24" 筛选时间段

git log --grep="key word" 在commit 的message中查找关键字

git log branch --not master 查看在branch上的，但不在master上的记录。


git log -S"func_name"  查找某个字符出现，或者移出的commit。 比如可以查找一个函数是什么时候添加，或者删除的。


git show sha1   这个sha1是每个commit的sha1，这样显示某个commit的完全信息，包括diff

 

6. 撤销改动

git checkout -- file.1

撤销了file.1的这次改动。只是撤销了没有staged的改动.

中间的 -- 表明了这是一个文件 而不是一个branch的名字


git reset --hard HEAD

撤销了所有没有commit的改动，包括了stage的和没有stage的。

这条命令的结果一样

git checkout HEAD file.1

包括了staged 和没有staged的都会清除。


有时候我们发现，之前做个一个commit有问题，不想要，想要去掉。

git revert HEAD      自动得重新做一个commit，将最后一次的commit返回回来。

git revert HEAD^     自动得重新做一个commit，将最后第二次的commit 返回回来。


7. 删除一个commit

git reset --hard HEAD~1

删除了最近的commit


8. 修改最近的一个commit

git commit --amend

 
7.显示所做的改动

git diff

显示所有的改动。 没有add到index中的。

 

git diff --staged或者 git diff --cached

显示staged改动，也就是add的东东，也就是将要commit的东东。

 

git diff commit1 commit2

显示这两个commit之间的变动， 从commit1到commit2的变动。


git diff commit1..commit2

两个点，效果跟上面的一样


git diff commit1...commit2

三个点，表示的是发生在commit2分支，一直到commit1和commit2共同父亲的变化。


git blame -C file1.c

显示文件具体的改动。。。。恩，好像是用来找是谁的错？


git blame -Ln,m file1.c

查看n,m两行间的改动。


git blame commit1~1 -Ln,m file1.c

查看commit1版本前的改动. 追查之前的log。


git blame commit1~1 -Ln,m -- old/file.c

如果这个文件被重命名过，或者移动过位置，就要输入旧的文件的名字。

而且一定要加上 -- ， 一定。

 

8. 删除某个文件

git rm file-name

从库和当前的working directory中删掉这个文件

git rm --cached file-name

只从库中删除，保留当前的本地文件。


9. 重命名一个文件

git mv file file-new


10. 应用patch

git apply patch-file

这样做从patch-file中应用这个patch。 效果和patch命令类似。

但是好处是，git apply要么成功，要么不成功。不想patch，有可能有部分的patch打上了，但是有的没有打上。

git apply后，并没有自动生成一个commit.


git apply --check  可以用来检测这个patch 是不是会产生冲突或者失败。


git am patch-file

这是专门为git 设计的命令。 patch-file是通过git format-patch 生成的。

其中包含了作者信息和简单描述。

git am后，会自动的生成一个commit.


git am --resolved

git am 过程中可能会有conflict. 如过遇到conflict， 那就需要手动修改code， git add 后

用git am --resolved


11. git 制作patch

具体步骤写在了 http://blog.csdn.net/richardysteven/article/details/6701156


3. commit range

在git中，我们经常需要制定一个commit的范围，比如git log中，可以显示某范围内的改动。

除了man gitrevisions, 在这个网站上也有不过的描述，尤其是对 double dot 和 triple dot

http://git-scm.com/book/ch6-1.html

http://stackoverflow.com/questions/462974/whats-the-difference-between-and-in-git-commit-ranges

而且这种语法，在git log和git diff两种情况下，有不同的意义.


在git log中，

git log ^r1 r2 表示显示从r2到root，但是去掉r1到root中和r2到root有重复的。

这个也可以表示为 git log r1..r2.


git log r1...r2 表示 显示从r1到root, r2到root，但是去掉他们共有的部分。


我这样理解， 前一种显示的是树上的一个分支。而后一种显示了两个分支。


在git diff中

git diff目的是比较两个commit之间的区别。


git diff A B 和 git diff A..B 是一样的，就是显示这两个之间的区别。


git diff A...B 和 git diff $(git-merge-base A B) B一样。 就是显示 在B这个分支上，做了什么改动。

                      有时候这个命令是，git merge-base A B


在一个branch上，但不在另一个branch上

git log local_copy ^kernel 

这样可以再merge前，看看都有哪些东西要commit.

这个命令用来看，在local_copy branch上，但是不在kernel branch上的。


```