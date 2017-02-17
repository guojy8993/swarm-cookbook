# Create a swarm

> 确保你已完成入门指导"SetUp"的步骤,各个节点都已经运行了1.2+ Docker Engine Daemon.

运行如下命令以创建一个新的swarm:
```
docker swarm init --advertise-addr <MANAGER-IP>
```
此处,我们使用如下命令初始化swarm manager:
```
[root@manager ~]# docker swarm init --advertise-addr 10.160.0.114
Swarm initialized: current node (204x40ldnxy31kbr6328rvdnc) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-00y2as6bixkaqm2k22ckd5xzpwdlyihtgl0x7u26qin2g4ga7k-7hx56vmbw57fg03p8lne64u0w \
    10.160.0.114:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
> ** NOTE: **

> 选项"--advertise-addr": 配置manager节点对外发布的ip地址. swarm中的其他节点必须能访问到该IP.

> 输出包含如何加入swarm集群的命令行. 节点究竟以何种角色加入集群取决于"--token".

使用"docker info"命令查看swarm状态:
```
[root@manager ~]# docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 3
Server Version: 1.12.5
...
Swarm: active
 NodeID: 204x40ldnxy31kbr6328rvdnc
 Is Manager: true
 ClusterID: a9nw5ng3sjgkt6c96cf68lppx
 Managers: 1
 Nodes: 1
 Orchestration:
  Task History Retention Limit: 5
 Raft:
  Snapshot Interval: 10000
  Heartbeat Tick: 1
  Election Tick: 3
 Dispatcher:
  Heartbeat Period: 5 seconds
 CA Configuration:
  Expiry Duration: 3 months
 Node Address: 10.160.0.114
```

使用"docker node ls"命令查看swarm集群节点信息:
```
[root@manager ~]# docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
204x40ldnxy31kbr6328rvdnc *  manager   Ready   Active        Leader
```
> ** NOTE: **

> 节点ID后的"*"表示你当前正连接于该节点

> Docker Engine swarm 模式自动以宿主节点的hostname命名swarm节点
