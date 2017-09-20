# python3.4环境

pip3安装清单

mysql server高于5.1.6

# redis server安装

要求密码为config.py中相同

# MySQL Server安装

5.1.4以上版本

安装后设置用户名密码为配置文件相同

初始化数据库结构，使用beopdoengine.sql和 beopdatabuffer.sql文件导入初始化。

# nginx安装

配置文件内容：注意其中的dataServiceIP要换为dataService的IP地址

```
server {
        send_timeout 600;
        #server_name ***.com;
        listen 80 default_server;


        gzip on;
        gzip_min_length 1k;
        gzip_buffers 4 16k;
        #gzip_http_version 1.0;
        gzip_comp_level 2;
        gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        gzip_vary on;
        gzip_disable "MSIE [1-6]\.";

        location ^~ /dist {
                alias /home/eic/mainService/beopWeb/static/dist/;
        }

        location ^~ /static {
                alias /home/eic/mainService/beopWeb/static/;
        }

        location / {
                try_files $uri $uri/ /index.html;
        }

        location /api/ {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_pass http://127.0.0.1:9001/;

                client_max_body_size 10m;
                proxy_connect_timeout 600;
                proxy_send_timeout 600;
                proxy_read_timeout 600;
        }

        location = /api/cloudPoint/onlinetest {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_pass http://dataServiceIP:4000;
        }

        location ^~ /api/v2/diagnosis/ {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;

                rewrite ^/api/(.*) /$1 break;
                proxy_pass http://dataServiceIP:4000;
        }

        location ^~ /api/diagnosis/ {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                rewrite ^/api/(.*) /$1 break;
                proxy_pass http://dataServiceIP:4000;
        }

        root /home/eic/mainService/beopWeb/static;
        index index.html index.htm;
        error_page 404 /404.html;

```

# 

# gunicorn安装和配置

配置文件内容：



# mainService python工程

部署到/home/mainService（使用svn co）

config都必须使用config-om.py（内部所有配置指向需用相应服务器）

运行步骤

source .runserver.sh

service nginx restart

