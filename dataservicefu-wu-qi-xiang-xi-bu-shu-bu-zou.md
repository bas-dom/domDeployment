python3.4

依赖库（可以用pip3 install  -r requirements.txt 完成安装，被中断后手动修复不能安装的库，便可继续）

```
beautifulsoup4==4.6.0
blinker==1.4
certifi==2017.4.17
chardet==3.0.4
click==6.7
et-xmlfile==1.0.1
Flask==0.12.2
Flask-Cors==3.0.2
Flask-Mail==0.9.1
idna==2.5
itsdangerous==0.24
jdcal==1.3
Jinja2==2.9.6
jpush==3.2.7
MarkupSafe==1.0
mysql-connector-python==1.2.2
numpy==1.9.1
odfpy==1.3.5
openpyxl==2.4.8
paho-mqtt==1.2.3
pdfkit==0.6.1
pika==0.10.0
pymongo==3.4.0
pywin32==220
PyYAML==3.12
redis==2.10.5
requests==2.18.1
selenium==3.4.3
six==1.10.0
tablib==0.11.5
unicodecsv==0.14.1
urllib3==1.21.1
Werkzeug==0.12.2
xlrd==1.0.0
xlwt==1.2.0
```

运行 DataService

也是利用nginx + gunicorn + flask 跑生产环境



# RabbitMQ



apt-get update 

apt-get install rabbitmq-server 

创建系统用户：

```
rabbitmqctl add_user 用户名 密码
```



 设置该用户为administrator角色：

```
rabbitmqctl set_user_tags 用户名 administrator
```

设置权限： 

```
rabbitmqctl set_permissions -p '/' 用户名 '.' '.' '.'
```



重启rabbitmq服务：

```
service rabbitmq-server restart 
```

如果需要进行防火墙设置（一般不建议在本机做防火墙，而利用云的自身防火墙设置为佳）：

```
iptables -A INPUT -p tcp --dport 5672 -j ACCEPT 
iptables -A INPUT -p tcp --dport 55672 -j ACCEPT 
iptables -A INPUT -p tcp --dport 15672 -j ACCEPT 
```





安装管理控制台插件

```
rabbitmq-plugins enable rabbitmq_management 
service rabbitmq-server restart
```

 

netstat -ntlp \|grep 5672 看端口是否正常 

rabbitmq配置文件没有，是默认的。 

最后一步：进入网页版的控制中心，将用户权限要设置访问目录/，否则没有权限



