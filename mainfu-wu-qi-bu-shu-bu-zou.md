# python3.4环境

先将python默认指向到3.4而不是2.7 （有的系统默认在/usr/bin/python3.4，需用whereis python3.4确认）

```
sudo rm /usr/bin/python
sudo ln -s /usr/local/bin/python3.4 /usr/bin/python
```

```
apt-get update
apt-get install python3-pip
```

## pip3安装基础库清单

先将如下清单保存为requirement.txt，再用pip3 install -r requirement.txt

```
Flask==0.12.2
Flask-Assets==0.12
Flask-Compress==1.4.0
Flask-Cors==3.0.2
Flask-Mail==0.9.1
Flask-SocketIO==2.8.6
Jinja2==2.9.6
MarkupSafe==1.0
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
numpy==1.12.1
olefile==0.44
pdfkit==0.6.1
pymongo==3.4.0
pyparsing==2.2.0
python-dateutil==2.6.1
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

## matplotlib库安装

先安装freetype（下载后用make）,参考：[http://blog.csdn.net/caiyunfreedom/article/details/46545685](http://blog.csdn.net/caiyunfreedom/article/details/46545685)

```
sudo wget ftp://139.196.7.223/pkgconfig-0.17.2.tar.bz2
sudo tar jxvf pkgconfig-0.17.2.tar.bz2
cd pkgconfig-0.17.2
sudo ./configure
sudo make
sudo make install

wget ftp://139.196.7.223/freetype-2.4.0.tar.bz2
sudo tar jxvf freetype-2.4.0.tar.bz2
cd freetype-2.4.0
sudo ./configure
sudo make
sudo make install


wget ftp://139.196.7.223/matplotlib-1.4.3.tar.gz
sudo tar zxvf matplotlib-1.4.3.tar.gz
cd matplotlib-1.4.3
sudo python setup.py install
```

## MySQL connector \(1.2.2\)

```
pip3 install --egg http://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-1.2.2.zip
```

## lxml

```
apt-get install libxml2-dev libxslt-dev python-dev
sudo pip install lxml
```

## Python-docx

下载tar.gz后，解压，用python setup.py install安装，必须在lxml安装成功后做这一步

```

```

## pillow

由于依赖很多系统库，需要先安装系统库后再pip install pillow

```
sudo apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev \
libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk

sudo pip install pillow
```

# redis server安装

要求密码为config.py中相同

```
redis-server /etc/redis/redis.conf &
```

# MySQL Server安装

## 清除旧版本

删除mysql

```
1.sudo apt-get autoremove --purge mysql-server-5.5

2.sudo apt-get remove mysql-common
```

清理残留数据

```
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

重新安装mysql

```
1.sudo apt-get install mysql-server

2.sudo apt-get install mysql-client

3.sudo apt-get install php5-mysql
```

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

安装apache

端口设置到8080，避免与nginx冲突

安装 php5

安装phMyAdmin 网页管理终端

最后一步，初始化数据库结构，使用beopdoengine.sql和 beopdatabuffer.sql文件导入初始化。

创建用户

用户名自定义，建议用mainuser，密码自定义与config相匹配即可。

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
        }
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

测试成功的方法：

浏览器访问：[http://MAIN\_IP/api/updateProjectInfo，网页显示Tr](http://MAIN_IP/api/updateProjectInfo，网页显示True)ue则表示成功。

更新代码或补丁包后的重启方法是重启gunicorn对应的父进程（注意PPID是gunicorn对应的）

```
sudo kill -HUP [PPID]
```

重启后需确认PPID发生变化且进程数量正确表示成功。

