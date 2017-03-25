##重定向配置
RewriteEngine On  
RewriteCond %{REQUEST_URI} ^/bbs/  
RewriteRule ^bbs/(.*) http://bbs.aaa.cn/$1 [R=permanent,L]  
RewriteCond %{REQUEST_URI} !^/bbs/  
RewriteRule ^(.*) http://www.111cn.net/$1 [R=permanent,L]

## 隐藏二级目录配置，例如https://www.bjlab.com/wenda/shuizi/205.html  跳转到 http://www.bjlab.com/wenda/205.html
RewriteCond %{REQUEST_URI} ^/wenda/  
RewriteRule ^wenda/(.*)/(.*)\.html /wenda/$2\.html [L,R=301,NC] 
RewriteCond %{REQUEST_URI} ^/biaozhun/  
RewriteRule ^biaozhun/(.*)/(.*)\.html /biaozhun/$2\.html [L,R=301,NC] 

