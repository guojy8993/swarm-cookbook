# Docker Swarm模式入门

> 开始前请确保你已经熟悉swarm模式下的关键概念(key concepts)

该文档从以下四步指引你入门:
```
1. 在swarm模式下初始化一个Docker Engine集群 
2. 添加节点到swarm
3. 部署应用服务到swarm
4. swarm集群的管理
```

# 配置

运行入门操作前,请进行如下准备与配置:
```
1. 三台网络互连的宿主机
2. Docker Engine 1.12+
3. Swarm manager宿主机的IP配置
4. 开放节点间的特殊协议与端口
```
## 三台网络互连的宿主机
该入门指导在swarm中使用3台网络互连的宿主机.机器名与ip分配如下:
```
manager1      10.160.0.114
worker1       10.160.0.115
worker2       10.160.0.117
```

## Docker Engine 1.12+
该入门指导手册使用Docker Engine 1.12+

## Swarm manager宿主机的IP配置
```
1. 该IP必须配置到宿主操作系统可以访问到的网卡设备.且该IP可以被swarm的各个节点访问到.
2. 该IP应该是一个固定ip.
```

## 开放节点间的特殊协议与端口

如下端口必须设置可用:
```
1. TCP/2377: 集合群管理通信
2. TCP/UDP/7946: 节点间通信
3. UDP/4789: overlay网络通信
```

另外,如果使用加密的overlay网络(--opt encrypted),那么你需要额外放开50端口以允许ESP协议通信.