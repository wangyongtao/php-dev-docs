git.doc.md


## Git简介  

Git是一个分布式的版本控制系统(DVCS, Distributed Version Control Systems).

Git官网： https://git-scm.com/  
Git下载： https://git-scm.com/download/  


## Git基础命令 

获取Git的版本:  
> $ git --version  

查看当前分支状态:  
> $ git status
> $ git status -s 

查看变化: 
> $ git diff 查看当前分支的变化
> $ git diff master origin/master 查看本地master与远程master的变化

查看日志：
> git log -10
> git log --all --decorate --graph --pretty=oneline -10

查看分支: 当前分支前面会标一个(*)星号
> $ git branch  
> $ git branch -a  列出所有分支(包括远程分支)
> $ git branch -r  列出所有远程分支：  

从 origin/master 创建并切换到分支 fea_dev:
> $ git checkout -b fea_dev origin/master

列出当前的配置：
git config -l  (列出所有的配置)  
git config -l --global (列出所有全局的配置)  
git config -l --local (列出所有本地的配置)  


## Git常用命令  

(1) 分支操作：git branch

查看本地分支: 当前分支前面会标一个(*)星号
git branch  

列出所有分支(包括远程分支)
git branch -a  
git branch --all  

列出所有远程分支：  
git branch -r  


(2) 将本地更改加入版本追踪:  git add  

将当前更改或者新增的文件加入到Git的索引中(stage区)，即加入版本历史中。

$ git add git.doc.md  
$ git add *  

(3) 删除文件: git rm

删除文件：

```
// 先查看有哪些文件可以删除,但是不真执行删除
// 参数 -r:递归移除目录
$ git rm -r -n demo/*
rm 'demo/README.md'
rm 'demo/section1.md'
// 执行删除文件：
// 通过git status查看这次的删除操作已经可以提交了
$ git rm -r demo/*
rm 'demo/README.md'
rm 'demo/section1.md'
```

(4) 提交更新: git commit  
提交更新： 将索引内容添加到仓库中(HEAD)
$ git commit -m "UPDATE:message" 

(5) 推送更新: git push： 
将本地分支的更新,推送到远程主机
$ git push  

(6) 切换分支: git checkout 
切换分支: 检出branch分支
$ git checkout  
$ git checkout -b newBrach origin/master


创建并切换分支:
$ git checkout -b fea_20160506 origin/develope
git checkout -b develope origin/develope

git checkout dev_branch  
git checkout -b fea_526 origin/fea_526


git branch -l 
git checkout -b FEA_MALLSIMP-282 remotes/origin/FEA_MALLSIMP-282


(7) 合并分支: git merge  

说明: git-merge - Join two or more development histories together
格式: git merge [from_branch] [to_branch]  


说明: 合并分支，将 from_branch 分支的代码，合并到 to_branch 分支上  
示例： 
```
//将origin/develop分支的代码合并到当前分支上
$ git merge origin/develop

//将master分支的代码合并到dev_branch分支上
//可以看到：本次合并，提示有冲突，需要解决再提交
$ git merge master dev_branch
Auto-merging git.doc.md
CONFLICT (content): Merge conflict in git.doc.md
Automatic merge failed; fix conflicts and then commit the result.
```  

(8) 删除分支:   

git branch –d [BranchName]
git branch --delete [BranchName]

示例: 
// 删除本地分支:
$ git branch -d fea_develop

// 删除远程分支
$ git branch -r -d origin/fea_develop

操作示例：  
```
// 删除分支，需要checkout都别的分支，否则会报错
$ git branch -d fea_develop
error: Cannot delete the branch 'fea_develop' which you are currently on.

// 切换到master分支，再来删除分支 fea_develop : 
// 报错: 分支 fea_develop 没有被完全合并
// 如果不想合并，可以使用 git branch -D fea_develop 直接删除
$ git checkout master   
$ git branch -d fea_develop 
error: The branch 'fea_develop' is not fully merged.
If you are sure you want to delete it, run 'git branch -D fea_develop'.

// 现在不想直接删除，把他合并到master主干分支上
// 报错: 还是分支 fea_develop 没有被完全合并,还需要合并到远程分支才行
$ git merge fea_develop  
$ git branch -d fea_develop
warning: not deleting branch 'fea_develop' that is not yet merged to
         'refs/remotes/origin/master', even though it is merged to HEAD.
error: The branch 'fea_develop' is not fully merged.
If you are sure you want to delete it, run 'git branch -D fea_develop'.

// 现在不想直接删除，把他合并到master主干分支上，再来删除
$ git checkout fea_develop 
$ git push
$ git checkout master 
$ git merge fea_develop 
$ git branch -d fea_develop
Deleted branch fea_develop (was dcf9d19).

//删除远程分支：
$ git branch -r -d origin/fea_develop
Deleted remote-tracking branch origin/fea_develop (was dcf9d19).
```


(9) 重命名分支： git branch –m

git branch –m oldName newName

参数-m,不会覆盖已有分支名称，即如果名为 newName 的分支已经存在，则会提示已经存在了。
参数-M,就会覆盖已有分支名称，即会强制覆盖名为 newName 的分支


(10) 重命名文件: git mv

格式:  
git-mv - Move or rename a file, a directory, or a symlink

操作示例:   

创建一个文件，提交到版本库，重命名后再提交到版本库，查看日志

```
// 创建一个test.txt文件，写入'test.txt'内容
$ echo 'test.txt' > test.txt 

//查看状态:看到文件前有两个问号，说明还被加入版本控制中
$ git status -s 
?? test.txt

//将文件加入版本控制中
$ git add *  
$ git commit -m "add test.txt" 
[fea_develop 9b9a163] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt

// 移动文件test.tx，重命名为test_01.txt
$ git mv test.txt test_01.txt 
// 查看状态:看到文件前有个大写字母R(Rename),表示被重命名
$ git status -s 
 R  test.txt -> test_01.txt

// 提交更新
$ git add *
$ git commit -m "rename:test.txt->test02.txt"
 [fea_develop 80f3288] rename:test.txt->test02.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename test.txt => test_01.txt (100%)

// 查看日志
$ git log --pretty=oneline
80f32889999702899109f2e038e3f0a5b2b9efc6 rename:test.txt->test_01.txt
9b9a16365a6b7fc3ffb0e4c7090d6a85accd4f96 add test.txt
... ... 
```


远程管理：

> 远程管理命令： 
> （1） git clone 克隆操作  
> （2） git remote 远程操作  
> （3） git fetch 远程拉取  
> （4） git pull 远程拉取并合并  
> （5） git push 推送到远程仓库  


克隆操作：git clone  
使用: 
> git clone https://github.com/wangyongtao/php-dev-docs.git  

git remote  

$ git remote 列出所有远程主机  
$ git remote -v 列出所有远程主机,并带上网址  
$ git remote show <HOST_NAME> 列出远程主机的详细信息   
$ git remote add <HOST_NAME> <HOST_URL> 添加远程主机  
$ git remote rm <HOST_NAME> 删除远程主机  
$ git remote rename <HOST_NAME_OLD> <HOST_NAME_NEW> 远程主机的改名  


拉取远程分支：git fetch  

$ git fetch <远程主机名> 将所有分支数据拉取到本地
$ git fetch <远程主机名> <分支名> 将制定的分支数据拉取到本地
 
操作示例：
```  
//比如，取回origin主机的master分支。    
$ git fetch origin master   
$ git fetch origin master  
From code.test.com:wangyt/dev-docs  
 * branch   master  -> FETCH_HEAD   

//比如，取回origin主机的dev分支。
$ git fetch origin dev
From code.test.com:wangyt/dev-docs
 * branch   dev  -> FETCH_HEAD

//取回远程主机的更新以后，使用git checkout命令创建一个新的分支：
//比如，在origin/master的基础上，创建一个新分支newBrach：  
$ git checkout -b newBrach origin/master

```  



git pull：取回远程主机某个分支的更新，再与本地的指定分支合并

格式：git pull <远程主机名> <远程分支名>:<本地分支名>

示例：  
```
//拉取远程仓库dev分支的更新，并默认与本地当前分支合并
$ git pull origin dev
From code.test.com:wangyt/dev-docs
 * branch dev -> FETCH_HEAD
Already up-to-date.

//拉取远程仓库master分支的更新，并与指定的本地dev分支合并
$ git pull origin master:dev
From code.test.com:wangyt/dev-docs
   384f2e8..39995f8  master     -> dev
warning: fetch updated the current branch head.
fast-forwarding your working tree from
commit 384f2e848cb57af88c60e66965d9f7d1f6998b16.
Already up-to-date.
```

git push : 推送到远程仓库  

git push <远程仓库名> <分支名>  


$ git push
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'.
使用 matching 参数是 Git 1.x 的默认行为，即如果你执行 git push 但没有指定分支，它将 push 所有你本地的分支到远程仓库中对应匹配的分支。
而 Git 2.x 默认的是 simple，即执行 git push 没有指定分支时，只有当前分支会被 push 到远程分支。

git stash

git stash list 

git stash pop

代码比较： git diff

使用 git diff比较文件的变化，会进入Vi界面状态，输入“:q”退出比较。  
```
//比较文件git.doc.md的变化：  
$ git diff git.doc.md  
```

查看日志： git log 

说明: git-log - Show commit logs
示例: git log --all --decorate --graph --pretty=oneline -10

查看Git日志:  
$ git log   

查看所有日志，每条记录一行:  
> $ git log --pretty=oneline 

查看最近10条日志，每条记录一行:  
> $ git log --pretty=oneline -10

查看所有日志，并有图表展示  
> $ git log --all --decorate --graph 

查看所有日志，并用图表展示，每条记录一行  
> $ git log --all --decorate --graph --pretty=oneline 

恢复本地删除的文件:
直接从本地把文件checkout出来就可以了，用不着从远程服务器上pull下来，因为，所有的历史版本你的本地都有的。
具体做法 git checkout file 同时恢复多个被删除的文件：  
git ls-files -d | xargs -i git checkout {}  

git reflog 则列出了head曾经指向过的一系列commit  

使用 git reflog 查看命令历史
```
$ git reflog
2630090 HEAD@{0}: reset: moving to 2630090
c585eb1 HEAD@{1}: reset: moving to HEAD^
2630090 HEAD@{2}: commit: update readme.txt:TEST
c585eb1 HEAD@{3}: commit: update readme.txt
6d0aee0 HEAD@{4}: commit (initial): add new file readme.txt
```

## Git 高阶命令


作为merge的替代选择，你可以像下面这样将feature分支并入master分支：
git checkout feature
git rebase master

它会把整个feature分支移动到master分支的后面，有效地把所有master分支上新的提交并入过来。
但是，rebase为原分支上每一个提交创建一个新的提交，重写了项目历史，并且不会带来合并提交。

合并多个提交：

$ git rebase –i HEAD~3

(1) git rebase –i HEAD~3
(2) 将第二个及以后的 pick 修改为 s (squash)，然后输入“:x”(或“:wq”)保存并退出
(3) 这时git会自动第二个提交合并到第一个中去，并提示输入新的comments,修改后输入“:x”(或“:wq”)保存并退出
(4) 此时本地的（HEAD中）最后多次的提交已经被合并为一个提交。
如果需要提交到远程仓库，运行git push --force origin master即可。


## 配置

列出当前的配置：
 
git config -l  
git config --global  
git config --local

设置邮箱和用户名：
如果没有配置邮箱与用户名，提交时会提示配置：
 ```
 $ git commit -m "Add configure.php.dev.env.md"
 *** Please tell me who you are.
 Run
   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
```

```
//全局的设置 
$ git config --global user.email "ahwyt2008@foxmail.com"
$ git config --global user.name "WangYongTao"
```

```
//局部的设置 
$ git config --local user.email "ahwyt2008@163.com"
$ git config --local user.name "WANG-YongTao"
```

git config --global push.default simple


## 常用问题  

$ git push  
fatal: The current branch fea_20160506 has no upstream branch.
To push the current branch and set the remote as upstream, use
    git push --set-upstream origin fea_20160506

如何删除中文名的文件：
> 在使用git status的时候,会发现中文文件名无法正常显示：  
> deleted: "\345\246\202\344\275\225\345\256\211\350\243\205PHP.md"  
> 
> 正常显示中文，让Git不对中文文件名进行处理：  
> $ git config --global core.quotepath false  
>  
> 运行 git status，可以看到中文正常显示了。  
> deleted: "如何安装PHP.md"  
> 
> 但是，无法使用 git rm [name] 来删除该文件:  
> 可以运行 git add -u 将所有改动的文件提交到暂存区，制定删除文件名，直接提交就可以了。  
> $ git add -u  


## Git教程 

1. 官方教程：《Pro Git : 2nd Edition (2014)》    
https://git-scm.com/book/en/v2  
https://git-scm.com/book/zh （中文版）  

2. 廖雪峰的官方网站：《Git教程》  
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000  

3. 易佰：《Git教程》  
http://www.yiibai.com/git/  

Git远程操作详解  
http://www.ruanyifeng.com/blog/2014/06/git_remote.html  



## 参考链接   

1. https://git-scm.com/  
2. http://www.liaoxuefeng.com/  
3. http://www.yiibai.com/  
4. http://www.ruanyifeng.com/blog/2014/06/git_remote.html  
5. http://www.cnblogs.com/itech/p/5188932.html   
...    


## 更新记录

2016-05-17: 新增一些问题 
2016-05-30: 完善补充内容 

[END]