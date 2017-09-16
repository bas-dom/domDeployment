|  | **代号** | 所需软件环境及配置 | 运行代码或工程 |
| :--- | :--- | :--- | :--- |
| 计算与诊断运算服务器 | **Job** | python 3.4 | JobService\(py工程\) |
|  |  | supervisord | SystemJob\(py工程\) |
| 历史数据服务器 | **Mongo** | mongodb\(≥3.2\) | 无 |
| 消息队列服务器 | **MQ** | RabbitMQ\(≥3.2\) | 无 |
| 数据接入服务器 | **Driver** |  | DTUServer\(exe工程\) |
| 后台Restful API服务器 | **Service** | ubuntu14 | dataService\(py工程\) |
|  |  |  | JobService\(py工程\) |
| Web服务器 | **Main** | ubuntu14 | MainService\(py工程\) |



