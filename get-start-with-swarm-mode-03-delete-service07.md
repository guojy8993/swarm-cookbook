# Delete the service running on the swarm

运行"docker service rm <SERVICE-ID>"删除既存服务

```
[root@manager ~]# docker service ls
ID            NAME        REPLICAS  IMAGE   COMMAND
6syg1lc6xfr8  helloworld  2/2       alpine  ping 127.0.0.1

[root@manager ~]# docker service ps 6syg1lc6xfr8
ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE                ERROR
cge76a3iqjhx3b9s3zhn7aka5  helloworld.1  alpine  manager  Shutdown       Shutdown about a minute ago  
9gyfqk5q02lp3n7358zp2hymn  helloworld.2  alpine  worker2  Running        Running 11 minutes ago       
92qwnyiknjrslf3mz03531pu9  helloworld.3  alpine  worker1  Running        Running 11 minutes ago

[root@manager ~]# docker service rm 6syg1lc6xfr8
6syg1lc6xfr8
[root@manager ~]# docker service ls
ID  NAME  REPLICAS  IMAGE  COMMAND
[root@manager ~]# docker ps -a
CONTAINER ID   IMAGE  COMMAND   CREATED    STATUS  PORTS  NAMES
```           

