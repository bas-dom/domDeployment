# python3.4环境

与dataService基本相同，需要numpy库。

注意要有：

```
paho-mqtt
selenium
redis
numpy
```

# 注意：容易遗漏不影响启动但影响策略api的安装库

html5lib：这个如果不装的话，很多api实时运行时就会有error

lxml：容易遗漏，实时会报出意外

phantomjs：这个如果不装的话，很多api实时运行时就会有error

# 下载和运行DataJob工程

# Supervisord

需要用supervisord来运行，运行配置文件：

```
[unix_http_server]
file = /tmp/supervisor.sock
;chmod = 0777
;chown= root:felinx

;[inet_http_server]
# Web管理界面设定
;port=9001
;username = admin
;password = password

[supervisorctl]
; 必须和'unix_http_server'里面的设定匹配
serverurl = unix:///tmp/supervisor.sock



[supervisord]
logfile=/tmp/supervisord.log ; (main log file;default $CWD/supervisord.log)
logfile_maxbytes=50MB       ; (max main logfile bytes b4 rotation;default 50MB)
logfile_backups=10          ; (num of main logfile rotation backups;default 10)
loglevel=info               ; (log level;default info; others: debug,warn,trace)
pidfile=/tmp/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
nodaemon=false              ; (start in foreground if true;default false)
minfds=1024                 ; (min. avail startup file descriptors;default 1024)
minprocs=200                ; (min. avail process descriptors;default 200)
user=root                 ; (default is current user, required if root)
;childlogdir=/var/log/supervisord/            ; ('AUTO' child log dir, default $TEMP)

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

; 管理的单个进程的配置，可以添加多个program


[program:alarm]
command=python3.4 /home/DataJob/AlarmSendNoticeQueue.py AlarmDataQueue_1 &
autorstart=true
autorestart=true


[program:expert1]
command=python3.4 /home/DataJob/RealCalcQueue.py RealCalcQueue_1 &
autorstart=true
autorestart=true


[program:expert2]
command=python3.4 /home/DataJob/RealCalcQueue.py RealCalcQueue_2 &
autorstart=true
autorestart=true

[program:expert3]
command=python3.4 /home/DataJob/RealCalcQueue.py RealCalcQueue_3 &
autorstart=true
autorestart=true


[program:expert4]
command=python3.4 /home/DataJob/RealCalcQueue.py RealCalcQueue_4 &
autorstart=true
autorestart=true

[program:expert5]
command=python3.4 /home/DataJob/RealCalcQueue.py RealCalcQueue_5 &
autorstart=true
autorestart=true


[program:diag1]
command=python3.4 /home/DataJob/DiagnosisCalcQueue.py DiagnosisCalcQueue_1 &
autorstart=true
autorestart=true


[program:diag2]
command=python3.4 /home/DataJob/DiagnosisCalcQueue.py DiagnosisCalcQueue_2 &
autorstart=true
autorestart=true


[program:diag3]
command=python3.4 /home/DataJob/DiagnosisCalcQueue.py DiagnosisCalcQueue_3 &
autorstart=true
autorestart=true



[program:force1]
command=python3.4 /home/DataJob/ForceCalcQueue.py ForceCalcQueue_1 &
autorstart=true
autorestart=true

[program:fixtime]
command=python3.4 /home/DataJob/FixedTimeCalc.py FixedTimeCalc.py &
autorstart=true
autorestart=true

[program:patchnull1]
command=python3.4 /home/DataJob/PatchQueryNull.py PatchQueryNull_1 &
autorstart=true
autorestart=true

[program:patchnull2]
command=python3.4 /home/DataJob/PatchQueryNull.py PatchQueryNull_2 &
autorstart=true
autorestart=true

[program:repair]
command=python3.4 /home/DataJob/RepairCalcQueue.py Repair_1 &
autorstart=true
autorestart=true


[program:thirddata]
command=python3.4 /home/DataJob/updateThirdDataRealAndHistory.py updateThirdDataRealAndHistory.py &
autorstart=true
autorestart=true

```

# Python3环境下安装Python2.7虚拟环境

由于supervisor依赖于2.7环境，而本服务器运行切至python3环境，因此需要用虚拟环境来切换。

pip3 install virtualenv

virtualenv /usr/bin/python2.7 /home/venvpy27

source /home/venvpy27/bin/activate



pip install supervisor



vim /etc/supervisord.conf





