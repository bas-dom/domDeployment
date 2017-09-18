# python3.4环境

pip3安装清单

mysql server高于5.1.6

# redis server

要求密码为config.py中相同

# nginx安装

配置文件内容：

# mainService python工程

部署到/home/mainService（使用svn co）

config都必须使用config-om.py（内部所有配置指向需用相应服务器）

运行步骤

source .runserver.sh

service nginx restart

