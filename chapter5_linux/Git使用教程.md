Git使用教程.md

# Git简介  

Git是一个分布式的版本控制系统(DVCS, Distributed Version Control Systems).

Git官网： https://git-scm.com/  
Git下载： https://git-scm.com/download/  

### Git常用命令  

git pull：取回远程主机某个分支的更新，再与本地的指定分支合并

格式：git pull <远程主机名> <远程分支名>:<本地分支名>

示例：
```
$ git pull origin dev
From code.XXX.com:wangyt/php-dev-docs
 * branch            dev        -> FETCH_HEAD
Already up-to-date.
```


git log

查看日志：git log  
参数--pretty=online显示成一行  
> git log  
> git log --pretty=online




mkdir git-test
cd git-test/
git init
vim readme.txt
git status
git add readme.txt
git commit -m 'add new file readme.txt'

vim readme.txt
git status
git diff readme.txt
git commit -m 'update readme.txt'

git add readme.txt
git commit -m 'update readme.txt'
git log
git status

vim readme.txt
git commit -m 'update readme.txt:TEST'
git add readme.txt
git commit -m 'update readme.txt:TEST'





回到上一个版本
git reset --hard HEAD^

回到某个指定的 commit id 版本：
git reset --hard 3628164

使用 git reflog 查看命令历史
$ git reflog
2630090 HEAD@{0}: reset: moving to 2630090
c585eb1 HEAD@{1}: reset: moving to HEAD^
2630090 HEAD@{2}: commit: update readme.txt:TEST
c585eb1 HEAD@{3}: commit: update readme.txt
6d0aee0 HEAD@{4}: commit (initial): add new file readme.txt


```
//编辑完文件，查看状态，会提示可能的操作
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   git.doc.md
no changes added to commit (use "git add" and/or "git commit -a")
```

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git branch
  dev
* master

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   git.doc.md
no changes added to commit (use "git add" and/or "git commit -a")

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git add git.doc.md

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git commit -m "UPDATE git.doc.md"
[master daeebd5] UPDATE git.doc.md
 1 file changed, 52 insertions(+), 10 deletions(-)

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.17 KiB | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To git@code.boqii.com:wangyt/php-dev-docs
   710954f..daeebd5  master -> master

Administrator@BQ-DN-BJB-022 MINGW64 /d/working/php-dev-docs (master)
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean




### 配置

列出当前的配置：

git config -l  
git config --global  
git config --local

设置邮箱和用户名：
> 如果没有配置邮箱与用户名，提交时会提示配置：
> ```
> $ git commit -m "Add configure.php.dev.env.md"
> *** Please tell me who you are.
> Run
>   git config --global user.email "you@example.com"
>   git config --global user.name "Your Name"
> ```

```
//全局的设置 
$ git config --global user.email "ahwyt2008@foxmail.com"
$ git config --global user.name "WangYongTao"
```

```
//局部的设置 
$ git config --local user.email "ahwyt2008@163.com"
$ git config --local user.name "WANGYongTao"
```




# Git教程 

官方教程：《Pro Git : 2nd Edition (2014)》  
https://git-scm.com/book/en/v2
https://git-scm.com/book/zh （中文版）

廖雪峰的官方网站：《Git教程》
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

易佰：《Git教程》
http://www.yiibai.com/git/


# 参考链接 
