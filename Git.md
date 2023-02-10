# Git

## 02-git安装与配置

一些有用的配置:

```shell
#设置名字
git config --global  user.name "Boss"
#设置邮箱
git config --global user.emait "BoSS@mE.Com"
#设置开启彩色
git config --global COLOR.UI TRUE
#设置编辑器VIM
git config --global CoRE.Editor Vim
#忽略大小写（windows,Mac)
git config --global core.ignorecase true
#区分大小写（Linux）
git config --global core.ignorecase false
文件名大小写敏感：
hello.txt I HELLO.txtl
```

1.先给本地分支master改名

```shell
$ git branch -M main 
```

说明：“-M”对分支重命名

2.查看所有分支

```shell
$ git branch -a 
* main   
remotes/origin/main   
remotes/origin/master  
```

说明：“-M”对分支重命名

2.查看所有分支

```shell
$ git branch -a 
* main   
remotes/origin/main   
remotes/origin/master  
```

3.删除远程分支master

```shell
$ git push origin --delete master 
To https://github.com/***/learnOpenGL.git
  - [deleted]         master  
```

4.确认删除情况

```shell
$ git branch -a 
* main   
remotes/origin/main  
```

5.切换到当前分支main，也就要保留下来的分支

```shell
$ git checkout main 
Already on 'main'  
```

说明：“Already on 'main'”已经说明在当前分支

6.合并分支

```shell
$ git merge remotes/origin/main 
fatal: refusing to merge unrelated histories 
```

说明：拒绝合并，需要忽略这个限制，添加“--allow-unrelated-histories”

```shell
$ git merge remotes/origin/main --allow-unrelated-histories 
Merge made by the 'recursive' strategy. 
```

7.提交修改

```shell
$ git push origin main 
Enumerating objects: 34, done. 
Counting objects: 100% (34/34), done. 
Delta compression using up to 8 threads 
Compressing objects: 100% (28/28), done. 
Writing objects: 100% (32/32), 1.46 MiB | 1.03 MiB/s, done. 
Total 32 (delta 1), reused 0 (delta 0), pack-reused 0 
remote: Resolving deltas: 100% (1/1), done. 
To https://github.com/test/learnOpenGL.git 
   76a2889..d34cf75  main -> main  
```

8.再次查看分支情况

```shell
$ git branch -a 
* main   
remotes/origin/main 
```



注意：期间可能会弹出一些文本框，直接关闭。



总结：应该还有更简单的方式，有不足的请不吝赐教。
