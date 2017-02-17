# Add nodes to the swarm

> 请确保阅读本文档前你已经成功完成"create swarm"的操作,建立起了一个manager节点的swarm

ssh登录到其中一个work节点,例如 worker1.

运行"docker swarm init"执行结果提供的加入集群命令,以添加当前节点到swarm集群.

```
[root@worker1 ~]# docker swarm join \
>     --token SWMTKN-1-00y2as6bixkaqm2k22ckd5xzpwdlyihtgl0x7u26qin2g4ga7k-7hx56vmbw57fg03p8lne64u0w \
>     10.160.0.114:2377
This node joined a swarm as a worker.
```

如果你已经遗忘了该命令行,可以去manager节点重新查询:

```
[root@manager ~]# docker swarm join-token worker
To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-00y2as6bixkaqm2k22ckd5xzpwdlyihtgl0x7u26qin2g4ga7k-7hx56vmbw57fg03p8lne64u0w \
    10.160.0.114:2377

[root@manager ~]#
```

同理添加worker2节点.

登录到swarm manager节点查询集群节点信息

```
[root@manager ~]# docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
204x40ldnxy31kbr6328rvdnc *  manager1  Ready   Active        Leader
26dh3iu6kfbvcd1yfzfi3s3cz    worker1   Ready   Active        
dpxci62cgiapq5w5ivrvnaeb0    worker2   Ready   Active
```
> **NOTE:**

> MANAGER列的Leader表示该节点是swarm manager节点,空白表示该节点是worker节点

> Swarm管理命令(类似"docker node ls")只允许在manager节点执行