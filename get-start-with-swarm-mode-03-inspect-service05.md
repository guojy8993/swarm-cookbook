# Inspect a service on the swarm

> 当成功部署服务到swarm之后,你可以使用Docker命令行查询运行中的服务的详情

登录到swarm manager节点使用"docker service <SERVICE-ID>"展示服务的详细信息,并使用"--pretty"选项美化输出,增加可读性:

```
[root@manager ~]# docker service inspect --pretty helloworld
ID:		6syg1lc6xfr8oegju7p0e6owf
Name:		helloworld
Mode:		Replicated
 Replicas:	1
Placement:
UpdateConfig:
 Parallelism:	1
 On failure:	pause
ContainerSpec:
 Image:		alpine
 Args:		ping 127.0.0.1
Resources:
```

如果要将输出结果转化成程序易处理的json字符串,那么可以不加"--pretty"选项:

```
[root@manager ~]# docker service inspect helloworld
[
    {
        "ID": "6syg1lc6xfr8oegju7p0e6owf",
        "Version": {
            "Index": 184
        },
        "CreatedAt": "2017-02-17T09:08:14.852694827Z",
        "UpdatedAt": "2017-02-17T09:08:14.852694827Z",
        "Spec": {
            "Name": "helloworld",
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "alpine",
                    "Args": [
                        "ping",
                        "127.0.0.1"
                    ]
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "RestartPolicy": {
                    "Condition": "any",
                    "MaxAttempts": 0
                },
                "Placement": {}
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause"
            },
            "EndpointSpec": {
                "Mode": "vip"
            }
        },
        "Endpoint": {
            "Spec": {}
        },
        "UpdateStatus": {
            "StartedAt": "0001-01-01T00:00:00Z",
            "CompletedAt": "0001-01-01T00:00:00Z"
        }
    }
]
```

运行"docker service ps <SERVICE-ID>"查看哪些节点运行着该服务:
```
[root@manager ~]# docker service ps helloworld
ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR
cge76a3iqjhx3b9s3zhn7aka5  helloworld.1  alpine  manager  Running        Running 13 minutes ago
```

在此例中,该服务运行于manager节点.而默认情况下,swarm中的manager节点可以像worker节点一样运行任务

Swarm同时也展示了预期状态以及最后状态,因此你可以检验服务是否按照服务定义来运行的.

而"docker ps"只能在服务调度到的节点上查看,才可以看到该任务.

```
[root@manager ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
191b30932ffc        alpine:latest   "ping 127.0.0.1"    20 minutes ago      Up 20 minutes  helloworld.1.cge76a3iqjhx3b9s3zhn7aka5
```

