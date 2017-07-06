1 hehe xia yu le 2017-0706
2 hehe mei xia yu 
3 汉字行不行
###git 
236  git
  237  mkdir /git
  238  cd /git/
  240  git init --bare learngit.git
  242  cd learngit.git/
  246  chown git.git learngit.git/
  247  adduser git
  248  chown git.git learngit.git/
  251  passwd git
  253  git config --global user.name "xxlliu"
  254  git config --global user.email "liuxiaoxiangbenet@163.com"
  255  cd /git/
  257  cd learngit.git/
  263  history | grep git
  264  cd learngit.git/
  266  git add readme.txt
  268  git add readme.txt
  270  git add readme.txt 
  271  git init
  273  git add readme.txt 
  276  git commit -m "add readme.txt"
  277  git status
  278  git diff readme.txt 
  280  git add readme.txt 
  281  git commit -m "add 1"
  282  git diff readme.txt 
  284  git add readme.txt 
  285  git diff readme.txt 
  286  git status
  287  git commit -m "add 2"
  288  git status
  289  git diff readme.txt 
  291  git log
  292  git reset --hard HEAD^
  294  git log
  295  git reset --hard 376e736
  296  git log
  298  git reflog
  299  git diff HEAD -- readme.txt
  300  git diff HEAD -- readme.txt 
  301  git diff
  302  git diff --help
  303  git reflog
  311  cd /git/
  313  cd learngit.git/
  315  git remote add origin git@github.com:liuxiaoxiang/demo.git
  316  git push -u origin master
  318  git add readme.txt 
  319  git commit -m "add hanzi"
  321  git push -u origin master
  323  history | grep git
##分支
[root@localhost learngit.git]# git checkout -b dev
Switched to a new branch 'dev'
[root@localhost learngit.git]# git branch
* dev
  master
 vi readme.txt 
 git add readme.txt 
 git commit -m "branch dev"
 git branch
 git checkout master
 cat readme.txt 
 合并分支：
 git merge dev
 cat readme.txt 
 git branch
 删除分支
 git branch -d dev
 git branch
 git push -u origin master
###gitlab 
https://packages.gitlab.com/gitlab
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
yum install gitlab-ce-9.3.5-ce.0.el6.x86_64
