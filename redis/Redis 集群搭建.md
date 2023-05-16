## 下载官方工具(redis-trib)
### 1. redis-trib 需要ruby环境 可能需要镜像安装
``` shell
yum install ruby rubygems -y
```
### 2. 创建集群
```shell
# 进入redis安装目录
cd reids安装目录/src
# 创建集群 # --replicas 每个主节点都有从节点
redis-trib create --replicas ip:prot ip:port
```
说明:  wyin