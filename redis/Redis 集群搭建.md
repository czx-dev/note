## 下载官方工具(redis-trib)
### 1. redis-trib 需要ruby环境 可能需要镜像安装
``` shell
yum install ruby rubygems -y
```
### 2. 创建集群
```shell
# 进入redis安装目录
cd reids安装目录/src
# 创建集群 # --replicas 每个主节点都有从节点 -a 密码认证
redis-trib create -a 123456 --replicas ip:prot ip:port
```
说明:  一主一从 是为了防止集群挂掉  Redis 最多 16384个哈希槽  在创建集群时分配 在存入数据时进行CRC16(key) % 16384计算放入哈希槽 主节点挂掉从节点会顶替
### 3. 增加主节点
```shell
# 使用如下命令即可添加节点将一个新的节点添加到集群中
# -a 密码认证(没有密码不用带此参数)
# --cluster add-node 添加节点 新节点IP:新节点端口 任意存活节点IP:任意存活节点端口
./bin/redis-cli -a 123456 --cluster add-node 192.168.100.104:8007 192.168.100.101:8001
# 重新分配哈希槽  all 表示所有
./bin/redis-cli --cluster reshard 192.168.100.101:8002

```
### 4. 增加从节点
```shell
# 使用如下命令即可添加节点将一个新的节点添加到集群中
# -a 密码认证(没有密码不用带此参数)
# --cluster add-node 添加节点 新节点IP:新节点端口 任意存活节点IP:任意存活节点端口
./bin/redis-cli -a 123456 --cluster add-node 192.168.100.104:8007 192.168.100.101:8001
# 连接redis
./bin/redis-cli
# 绑定主节点
CLUSTER REPLICATE 主节点ID
```
### 5.删除节点
```shell 
# 执行如下命令删除节点
# -a 密码认证(没有密码不用带此参数)
# --cluster del-node 连接任意一个存活的节点IP:连接任意一个存活的节点端口 要删除节点ID 
./bin/redis-cli -a 123456 --cluster del-node 192.168.100.101:8002 8cf44439390dc9412813ad27c43858a6bb53365c

```