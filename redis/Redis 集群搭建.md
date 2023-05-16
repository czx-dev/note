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
说明:  一主一从 是为了防止集群挂掉  Redis 最多 16384个哈希槽  在创建集群时分配 在存入数据时进行CRC16(key) % 16384计算放入哈希槽 主节点挂掉从节点会顶替