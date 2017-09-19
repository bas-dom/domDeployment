# RabbitMQ安装

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



最后一步：进入网页版的控制中心

> 默认登录网址为http://MQ服务器:15672
>
> 登陆用户名密码为前面所设置的

将用户权限要设置访问目录/，否则没有权限

