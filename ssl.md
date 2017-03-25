
#生成证书申请请求的命令，其中生成的server.key是私钥，需要在apache中配置，SSLCertificateKeyFile 这个选项写私钥文件

openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr

#查看证书文件的内容

openssl req -noout -text -in server.csr 

#apache 中配置 例如：
<IfModule mod_ssl.c>
	<VirtualHost _default_:443>
        DocumentRoot "/test"
        ServerName  imonline.28.com
        <Directory /data/web/imonline_online>
        Options Indexes FollowSymLinks MultiViews Includes
        AllowOverride All
        Order allow,deny
        Allow from all
        AddType text/html .shtml .html .htm
        AddOutputFilter INCLUDES .shtml .html .htm
        </Directory>
        DirectoryIndex index.php index.html index.htm
        CustomLog "|/usr/bin/cronolog /data/logs/443-imonline.28.com_%Y%m%d_access.log" combined
        SSLEngine on
        SSLCertificateFile    /data/ssl/__28_com.crt
        SSLCertificateKeyFile /data/ssl/server.key
        SSLCertificateChainFile /data/ssl/__28_com.ca-bundle
	</VirtualHost>
</IfModule>

#nginx 配置：
server
        {
        listen  80;
        server_name  fuwu.28.com;
	rewrite ^(.*$) https://$host$1 permanent;

    }

server
        {
        listen  443 ssl;
        server_name  fuwu.28.com;
        ssl_certificate /usr/local/nginx/ssl/31253367.new.crt;
        ssl_certificate_key /usr/local/nginx/ssl/server.key;

        location / {
            proxy_pass        http://fuwu.28.com;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
	    proxy_set_header   X-Forwarded-Proto $scheme;
            proxy_next_upstream error timeout invalid_header http_500 http_503;
        }
    }




