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

配置文件一般用/etc/nginx/site-available/default

内容：注意其中的dataServiceIP要换为dataService的IP地址

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

注意：本文件在mainService工程中，原则上不需要再建立，这里列出来，注意其配置。

配置文件内容：

```
import os
from  datetime import datetime

import logging

bind='127.0.0.1:9001'
workers=11
backlog=2048
timeout=600
worker_class="sync" #sync, gevent,meinheld
debug=True
proc_name='gunicorn.pid'
pidfile='/var/log/gunicorn/debug.log'
loglevel='debug'
errorlog='/var/log/gunicorn/gunicorn_error_%s.log'%(datetime.now().strftime('%Y-%m-%d-%H-%M'))
accesslog = '/var/log/gunicorn/gunicorn_access_%s.log'%(datetime.now().strftime('%Y-%m-%d-%H-%M'))
access_log_format = '%(h)s --%(p)s-- %(u)s %(t)s "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"'

logFileName = './log/serverlog%s.txt' % datetime.now().strftime('%Y-%m-%d-%H-%M')
logging.basicConfig(filename=logFileName, level=logging.ERROR,
                    format='%(asctime)s --- levelname:%(levelname)s filename: %(filename)s funcName:%(funcName)s '
                           'outputNumber: [%(lineno)d]  thread: %(threadName)s output msg: %(message)s',
                    datefmt='[%Y-%m-%d %H:%M:%S]')

```

# mainService python工程

部署到/home/mainService（使用svn co）

config都必须使用config-om.py（内部所有配置指向需用相应服务器）

运行步骤

source .runserver.sh

service nginx restart

