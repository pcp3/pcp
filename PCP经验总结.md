# PCP经验总结

## 1 mongdb问题

在《问题：监控问题一览.docx》文档中大体列举了监控常见的问题，以及导致的原因，在后续的文档中，会分别针对每种情况出一个详细的解决文档。现在介绍一个总体的排查解决问题的流程。

### 1.1 具体步骤如下：

\1.     查看磁盘空间情况，可以使用命令：

| df -h         | 查看总体使用情况                                             |
| ------------- | ------------------------------------------------------------ |
| du –sh /var/* | 查看哪个目录比较大，通过灵活的修改路径（/var/*）来检查不同的路径下文件夹的大小，通常用来解决log日志太大，mq数据文件太大等问题 |

 

### 1.2.     查看mongodb的运行状态，使用以下命令：

mongo

异常图示如下：

![img](file:////Users/zhanglch/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

如果此数据库运行异常，重启mongodb的命令如下：

mongod –f /mongod.conf

启动成功图示如下：

![img](file:////Users/zhanglch/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

\3.     查看ceilometer服务的状态，使用一下命令：

openstack-service status
 ceilometer的服务都为active表示正常，如果出现下图中的failed或者（inactive），表示ceilometer服务异常了，异常图示如下：

![img](file:////Users/zhanglch/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.png)

重启ceilometer的命令如下：

Openstack-service stop ceilometer

Openstack-service start ceilometer

\4.     查看rabbitmq中的metering 以及 cloud.monitor.statistic.metering队列的运行情况