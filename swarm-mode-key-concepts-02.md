# Swarm 模式的关键概念
> 请使用**Docker Engine 1.2+**以支持swarm模式

# 什么是swarm
Docker Engine(1.2+)内置的集群管理与编排特性是使用SwarmKit达成的.Docker engines是以swarm模式加入集群.你可以通过初始化一个swarm集群或者加入一个既有swarm集群以enable一个docker engine的swarm模式.

一个Swarm就是一个Docker engine或docker 节点的集群,基于其上你可以部署各种服务.Docker Engine命令行与API包含管理swarm节点(添加/删除),以及部署编排服务的各种命令.

当你未使用swarm模式运行docker的时候,你是用docker命令运行容器.当你使用swarm模式运行docker的时候,你就可以编排服务了.
You can run swarm services and standalone containers on the same Docker instances.

# 什么是swarm节点
一个swarm节点就是一个加入swarm集群的docker engine实例.你也可以认为它其实就是一个docker节点.你可以在单个物理机或者多个云主机上运行一个或多个swarm节点, 但是在生产环境下,swarm的部署,应该是将swarm节点分布在多个物理机或云主机上.

为了在swarm上部署应用,你需要向swarm manager节点提交一份服务定义. Swarm manager节点分发任务(工作单元)到各个worker节点.

Swarm manager还会执行维护swarm预期状态所需的的编排与集群管理功能. 各Swarm Manager节点选举出唯一的leader以监督编排任务.

Swarm worker节点接受并执行swarm manager节点分布过来的任务. 默认情况下,manager节点也可以worker节点的角色运行服务, 但是你可以配置它们专一地运行swarm manager任务并成为专门的swarm manager节点. 一个代理程序运行于worker节点上并汇报运行于其上的任务.Swarm worker节点通知swarm manager节点分配个自己的任务的当前运行状态,所以swarm manager可以维护各个worker节点的预期状态.

# 服务与任务
服务是在worker节点上运行的一组任务的定义.服务是swarm系统的中心结构以及与swarm进行用户交互的主根.

当你创建一项服务的时,你可以指定使用哪个容器镜像以及在容器内执行哪些命令.

在"副本"服务模式下,swarm manager可以以你指定的副本数分布/运行任务于各个worker节点.

在"全局"服务模式下,swarm在个集群每一个可用节点上运行一个该服务的任务.

一个任务包括一个docker容器以及运行于容器内的命令.它是swarm的原子调度单元. Swarm manager按指定任务数运行服务于worker节点上. 一旦一项任务被分配到一个节点, 它就不允许移动到另一个节点. 任务要么运行于指定节点要么失败.

# 负载均衡

Swarm manager使用入向负载均衡暴露那些你想要对外可用的服务.Swarm manager可以自动为服务分配PublishedPort(范围30000-32767),或者你可以为服务指定PublishedPort,只要该端口尚未使用.

外部组件,例如云负载均衡器,可以通过集群中的任何节点的PublishedPort获取服务,而不管该节点当前是否运行着该服务的任务. 所有的swarm节点将入向连接路由到运行着的任务实例.

Swarm模式拥有内部DNS组件,它可以自动地为swarm内的服务分配DNS条目.Swarm manager使用内部负载均衡基于服务的DNS名称在集群内部分发请求.