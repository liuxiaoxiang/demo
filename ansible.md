##磁盘空间查看
ansible all -m shell -a 'df -h | grep -vE "^Filesystem|tmpfs|cdrom|shm|lock|run|cgroup|udev"'

##nginx 域名统计

ansible 10.47.137.192  -m shell -a "cat /etc/nginx/conf.d/* | grep -v '#' |  grep server_name | awk '{print \$2}' | awk -F';' '{print \$1}'"
ansible 10.51.101.210 -m shell -a "cat /usr/local/nginx/conf/vhost/* | grep -v '#' | grep server_name | awk '{print \$2}' | awk -F';' '{print \$1}'"

####apache 域名统计
ansible all -m shell -a "cat /etc/apache2/sites-enabled/* | grep Server | grep -v '#' | grep -v '@'| awk '{print \$2}' | sort | uniq"

####添加任务计划
ansible webservs -m cron -a 'minute="*/10" job="/bin/echo hell" name="test cron job"


ansible webservs -m cron -a 'minute="*/10" job="/bin/echo hell" name="test cron job" state=absent'

ansible all -m copy -a 'src=/etc/fstab dest=/tmp/fstab.ansible owner=root mode=640'

ansible all -m file -a 'owner=mysql group=mysql mode=644 path=/tmp/fstab.ansible'

ansible webservs -m service -a 'enabled=true name=httpd state=started'

ansible all -m copy -a 'src=/shell/restartapache.sh dest=/shell/restartapache.sh owner=root mode=641'

ansible all -m cron -a 'minute="*/5" job="/bin/sh /shell/restartapache.sh > /dev/null" name="restart apache2 and mysql"'

###ip
ansible all -m raw -a "ifconfig eth1 |grep 'inet addr' |awk -F: '{print \$2}'"
##top 
ansible all -m shell -a 'top -bn 1 | grep Tasks'


nsible帮助：
ansible-doc -l   列出所有模块
ansible-doc -s cron  (模块名称)
2.ansible命令应用基础：
语法：
ansible <host-pattern> [-f forks] [-m module_name] [-a args] [options]
-f forks:启动的并发线程数
-m module_name: 要使用的模块名
-a args:模块特有的参数

3.常见的模块：
    command :命令模块，默认模块，用于远程执行命令
           ansible all -a 'date'
    cron :
          state:
               present:安装
               absent：移除
    */5 * * * *  /bin/sh /shell/restartapache.sh > /dev/null
   ansible all -m cron -a 'minute="*/5" job="/bin/sh /shell/restartapache.sh > /dev/null" name="restart apache2 and mysql"'  默认是安装
   ansible all -m cron -a 'minute="*/5" job="/bin/sh /shell/restartapache.sh > /dev/null" name="restart apache2 and mysql" state=absent'  移除

    user : ansible all -m user -a 'name="user1"'
           ansible all -m user -a 'name="user1" state=absent'

    group : ansible all -m group -a 'name=xxliu gid=306 system=yes'
            ansible all -m shell -a 'cat /etc/group'
    copy  : ansible all -m copy -a 'src=/shell/restartapache.sh dest=/shell/restartapache.sh owner=root mode=641'
            src可以是相对路径也可以是绝对路径，但是dest必须是绝对路径
            content: 取代src，用此处指定的内容直接生产文件
            ansible all -m copy -a 'content="hello word\nnihao\n" dest=/shell/test.txt owner=root mode=641'
            查看文件内容：
             root@ansible:~# ansible all -m shell -a 'cat /shell/test.txt'
             122.200.94.45 | SUCCESS | rc=0 >>
             hello word
             nihao
    file ： 设定文件属性
            path:文件路径   可以使用name= 或者dest 替换
            创建符号链接：
            src： 源文件
            path：符号链接文件路径
            ansible all -m file -a 'path=/shell/test.link src=/shell/test.txt state=link'
            test.link -> /shell/test.txt
    
    setup : 收集远程主机的facts
    script : ansible all -m script -a "test.sh"

    synchronize模块：
    目的：将主控方/root/a目录推送到指定节点的/tmp目录下
    命令：ansible 10.1.1.113 -m synchronize -a 'src=/root/a dest=/tmp/ compress=yes'
    执行效果：
    delete=yes   使两边的内容一样（即以推送方为主）
    compress=yes  开启压缩，默认为开启
    --exclude=.Git  忽略同步.git结尾的文件

    由于模块，默认都是推送push。因此，如果你在使用拉取pull功能的时候，可以参考如下来实现
    mode=pull   更改推送模式为拉取模式
    目的：将10.1.1.113节点的/tmp/a目录拉取到主控节点的/root目录下
    命令：ansible 10.1.1.113 -m synchronize -a 'mode=pull src=/tmp/a dest=/root/'
    get_url模块：
    目的：将http://10.1.1.116/favicon.ico文件下载到指定节点的/tmp目录下
    命令：ansible 10.1.1.113 -m get_url -a 'url=http://10.1.1.116/favicon.ico dest=/tmp'
