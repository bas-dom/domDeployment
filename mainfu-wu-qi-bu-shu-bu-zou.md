# python3.4环境

```
apt-get update
apt-get install python3-pip
```

pip3安装清单\(用pip3 install -r requirement.txt\)

```
Flask==0.12.2
Flask-Assets==0.12
Flask-Compress==1.4.0
Flask-Cors==3.0.2
Flask-Mail==0.9.1
Flask-SocketIO==2.8.6
Jinja2==2.9.6
MarkupSafe==1.0
Pillow==4.1.1
Werkzeug==0.12.2
XlsxWriter==0.9.6
blinker==1.4
chardet==2.2.1
click==6.7
colorama==0.2.5
configobj==5.0.6
cycler==0.10.0
gunicorn==19.7.1
html5lib==0.999
imgkit==0.1.7
itsdangerous==0.24
lxml==3.3.3
matplotlib==2.0.2
mysql-connector-python==1.2.2
numpy==1.12.1
olefile==0.44
pdfkit==0.6.1
pymongo==3.4.0
pyparsing==2.2.0
python-dateutil==2.6.1
python-docx==0.8.6
python-engineio==1.5.4
python-socketio==1.7.5
pytz==2017.2
redis==2.10.5
requests==2.2.1
six==1.10.0
tablib==0.11.4
ufw==0.34-rc-0ubuntu2
urllib3==1.7.1
webassets==0.12.1
wheel==0.24.0
xlrd==1.0.0
xlwt==1.2.0
```

mysql server高于5.1.6

# redis server安装

要求密码为config.py中相同

# MySQL Server安装

5.1.4以上版本

安装后设置用户名密码为配置文件相同

开启防火墙的3306端口（使用iptables指令）

由于mysql将3306默认绑定到本地，需要更改my.cnf文件将其配置为监听所有客户端连接，也可以配置为监听指定的ip连接  
，需要在my.cnf找到bind-address=addr 这一行注释掉或者改为：

```
bind-address=0.0.0.0
```

> 参考[http://blog.csdn.net/ynnmnm/article/details/45097857](http://blog.csdn.net/ynnmnm/article/details/45097857)

配置文件修改后需要重启mysql server

最后一步，初始化数据库结构，使用beopdoengine.sql和 beopdatabuffer.sql文件导入初始化。

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

