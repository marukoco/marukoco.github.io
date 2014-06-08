---
layout:post
title:Github basic operation record
---

##一.拷贝github的项目到本地

###1.把原有账号的repository克隆到本地

  git clone git@github.com:Chibi-Maruko/**chibi-maruko.github.com**.git 

###2.假如本地已经存在了这个项目，而仓库中又有一新的更新，如何把更的合并到本地的项目中？

$ git fetch origin    //取得远程更新，这里可以看做是准备要取了

$ git merge origin/master  //把更新的内容合并到本地分支/master

##二. 创建密钥
我们如何让本地git项目与远程的github建立联系呢？之里就用的密钥。通俗点叫口令吧！

$ cd ~/. ssh 检查本机的ssh密钥

$ssh-keygen –t rsa –C “defnngj@gmai.com”  //生产新的密钥

注意: 此处的邮箱地址，你可以输入自己的邮箱地址。在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

打开本地C:\Documents and Settings\Administrator\.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。

登陆github系统。点击右上角的 Account Settings--->SSH Public keys ---> add another public keys

把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了

$ ssh –T git@github.com //查看配置结果

如果提示：Hi defnngj You've successfully authenticated, but GitHub does not provide shell access. 说明你连接成功了。

##三.提交本地修改到github
切换到要提交的目录下：

$ git status   //查看当前项目下所有文的状态，如果第一次，你会发现都红颜色的，因为它还没有交给git/github管理。

$ git add .   //（.）点表示当前目录下的所有内容，交给git管理，也就是提交到了git的本地仓库。
  
$ git commit –m”new natter ”  //“”中内容为对你更新或修改了哪些内容做一个描述。

$ git remote add origin git@github.com:defnngj/hibernate-demo.git

//如果你是第一次提交项目，这一句非常重要，这是你本地的当前的项目与远程的哪个仓库建立连接。

$ git remote -v  //查看你当前项目远程连接的是哪个仓库地址。

$ git push -u origin master  //将本地的项目提交到远程仓库中。

**git push -u origin master** //执行后可以立刻在页面上看到本地的修正

--uppdates remote refs using local refs, while sending objects necessary to complete the given refs.

##四.项目中删除了一些文件，如何提交？
假如远程仓库中已经存了aaa这个文件，我fetch了下来，并删除了aaa这个文件，想再push上到远程仓库中，并使远程仓库中的项目被新的修改覆盖（也是是远程仓库中的aaa也被删除）

$ git status   //可以看到我们删除的哪些文件

$ git add .   //删除之后的文件提交git管理。

$ git rm   src/com/hzh/hibernate/dao/aaa.java    //移除我们删除的那个文件，不然git不允许我们往远程仓库提交。

Ps: 如果你想删除的是某个目录（java包），这里想移除整个目录的内容。

$ git rm  src/com/hzh/hibernate/bbb/ -r   // -r 会把bbb/目录下的所有内容一次性移动。