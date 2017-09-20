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

配置文件内容：

# mainService python工程

部署到/home/mainService（使用svn co）

config都必须使用config-om.py（内部所有配置指向需用相应服务器）

运行步骤

source .runserver.sh

service nginx restart

