# Deploy a service to the swarm

> 在阅读该文档之前,请确保读者已经成功执行"add nodes"部分操作

打开终端,登录到swarm manager节点,运行如下命令创建服务:

```
[root@manager ~]# docker service create --replicas 1 --name helloworld alpine ping 127.0.0.1
52affsy2ta4498t4ikn937thx
```

> **NOTE:**

> 选项"--name"指定服务的名字

> 选项"--replicas"指定以1副本运行该服务

> 选项"ping 127.0.0.1"指定启动容器执行的命令

运行"docker service ls"查看运行中的服务的列表:

```
[root@manager ~]# docker service ls
ID            NAME        REPLICAS  IMAGE   COMMAND
6syg1lc6xfr8  helloworld  1/1       alpine  ping 127.0.0.1
```

运行"docker service ps <服务名>"查看该服务下的任务单元:

```
[root@manager ~]# docker service ps 6syg1lc6xfr8
ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE               ERROR
cge76a3iqjhx3b9s3zhn7aka5  helloworld.1  alpine  manager  Running        Running about a minute ago
```