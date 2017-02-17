# Scale the service in the swarm

> 在阅读实践操作该部分之前,请确保你已经成功完成"deploy a service"部分操作,然后你可以进行弹性操作.

运行"docker service scale <SERVICE-ID>=N"命令改变运行在swarm中的服务的预期状态:

```
[root@manager ~]# docker service ls
ID            NAME        REPLICAS  IMAGE   COMMAND
6syg1lc6xfr8  helloworld  1/1       alpine  ping 127.0.0.1
[root@manager ~]# docker service scale 6syg1lc6xfr8=3
6syg1lc6xfr8 scaled to 3
```

运行"docker service ps <SERVICE-ID>"查看更新过的任务列表:
```
[root@manager ~]# docker service ps 6syg1lc6xfr8
ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR
cge76a3iqjhx3b9s3zhn7aka5  helloworld.1  alpine  manager  Running        Running 36 minutes ago  
9gyfqk5q02lp3n7358zp2hymn  helloworld.2  alpine  worker2  Running        Running 2 minutes ago   
92qwnyiknjrslf3mz03531pu9  helloworld.3  alpine  worker1  Running        Running 2 minutes ago
```

可以观察到swarm已经创建两个新的task以弹性扩展到三个运行实例.这些任务被分配到swarm的三个节点上去:

```
[root@worker1 ~]# docker ps -a
CONTAINER ID   IMAGE    COMMAND    CREATED  STATUS  PORTS  NAMES
c743608a776c  alpine:latest   "ping 127.0.0.1"   6 minutes ago  Up 6 minutes  helloworld.3.92qwnyiknjrslf3mz03531pu9

[root@worker2 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6eeca285a8d9 alpine:latest "ping 127.0.0.1" 6 minutes ago Up 6 minutes  helloworld.2.9gyfqk5q02lp3n7358zp2hymn
```

反向弹性缩小集群:
```
[root@manager ~]# docker service scale 6syg1lc6xfr8=2
6syg1lc6xfr8 scaled to 2
[root@manager ~]# docker service ps 6syg1lc6xfr8
ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE        ERROR
cge76a3iqjhx3b9s3zhn7aka5  helloworld.1  alpine  manager  Shutdown   Shutdown about a minute ago  
9gyfqk5q02lp3n7358zp2hymn  helloworld.2  alpine  worker2  Running    Running 11 minutes ago       
92qwnyiknjrslf3mz03531pu9  helloworld.3  alpine  worker1  Running    Running 11 minutes ago
```
