git clone git@122.200.94.17:xxliu/demo.git
##实体机ubuntu lnmp网站环境搭建脚本：
#!/bin/bash
apt-get update
apt-get -y install php-soap
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
apt-get -y install curl libcurl3 libcurl3-dev php5-curl php5-dev 
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
apt-get -y install libapache2-mod-php5
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
apt-get -y install cronolog
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2

a2enmod rewrite 
a2enmod expires
a2enmod include
sed -i '$a\ServerName localhost' /etc/apache2/apache2.conf
sed  -i "s/ServerTokens OS/ServerTokens Prod/g" /etc/apache2/conf-available/security.conf
sed  -i "s/ServerSignature On/ServerSignature Off/g"  /etc/apache2/conf-available/security.conf
apt-get -y install apf-firewall
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
sed -i "s/IFACE_IN=\"eth0\"/IFACE_IN=\"em1\"/g" /etc/apf-firewall/conf.apf 
sed -i "s/IFACE_OUT=\"eth0\"/IFACE_OUT=\"em1\"/g" /etc/apf-firewall/conf.apf 
sed -i "s/IFACE_TRUSTED=\"\"/IFACE_TRUSTED=\"em2\"/g" /etc/apf-firewall/conf.apf 
sed -i "s/DEVEL_MODE=\"1\"/DEVEL_MODE=\"0\"/g" /etc/apf-firewall/conf.apf 
sed -i "s/SET_MONOKERN=\"0\"/SET_MONOKERN=\"1\"/g" /etc/apf-firewall/conf.apf
sed -i "s/IG_TCP_CPORTS=\"22\"/IG_TCP_CPORTS=\"80\"/g" /etc/apf-firewall/conf.apf
sed -i "s/DLIST_DSHIELD=\"0\"/DLIST_DSHIELD=\"1\"/g" /etc/apf-firewall/conf.apf
sed -i "s/no/yes/g" /etc/default/apf-firewall
apf -a 122.200.94.22
apf -a 122.200.94.25
apf -a 122.200.94.26
apf -r
apt-get -y install  pure-ftpd
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
groupadd ftpgroup
useradd ftpuser -g ftpgroup -s /bin/false -d /dev/null
mkdir -p /data/web/
mkdir /data/logs
chown -R ftpuser.ftpgroup /data/web/
ln -s /etc/pure-ftpd/conf/PureDB /etc/pure-ftpd/auth/60puredb
(echo ftptest;echo ftptest) | pure-pw useradd test -u ftpuser -d /data/web/
pure-pw mkdb
/etc/init.d/pure-ftpd restart
apt-get -y install zabbix-agent
if [$? -ne 0 ];then
    echo "ok"
else
   echo "not ok"
fi
sleep 2
sed -i "s/Server=127.0.0.1/Server=192.168.161.216/g"  /etc/zabbix/zabbix_agentd.conf
/etc/init.d/zabbix-agent restart
###icp备案查询地址
http://www.miitbeian.gov.cn/publish/query/indexFirst.action
###
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
##同步自动输入密码
root@logs:~# cat logs104.sh 
#!/bin/bash
expect -c "
spawn rsync -avz --delete /data/www root@ip地址:/data
expect {
    \"*assword\" {set timeout 300; send \"密码\r\";}
#    \"yes/no\" {send \"yes\r\"; exp_continue;}
      }
expect eof"


####walle 瓦力Walle 一个web部署系统工具，配置简单、功能完善、界面流畅、开箱即用！支持git、svn版本管理，支持各种web代码发布，PHP，Python，JAVA等代码的发布、回滚，可以通过web来一键完成。
http://www.walle-web.io/docs/index.html
