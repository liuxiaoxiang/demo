打开php.ini，首先找到
file_uploads = on ;是否允许通过HTTP上传文件的开关。默认为ON即是开
upload_tmp_dir ;文件上传至服务器上存储临时文件的地方，如果没指定就会用系统默认的临时文件夹
upload_max_filesize = 8m ;望文生意，即允许上传文件大小的最大值。默认为2M
post_max_size = 8m ;指通过表单POST给PHP的所能接收的最大值，包括表单里的所有值。默认为8M
一般地，设置好上述四个参数后，上传<=8M的文件是不成问题，在网络正常的情况下。
但如果要上传>8M的大体积文件，只设置上述四项还一定能行的通。

###php -m | grep mcryp 不显示问题,php -m 调用的是cli里的文件，如果没有，需要手动创建一个软链接
root@logs:/etc/php5/cli/conf.d# ln -s ../../mods-available/mcrypt.ini 20-mcrypt.ini
###
