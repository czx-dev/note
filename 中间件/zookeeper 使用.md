# zookeeper 使用

​	树形结构

## 	javaAPI使用

### 	1.建立连接

```java
 CuratorFramework zkCli = CuratorFrameworkFactory.builder()
     			//zookeeper server IP:prot
                .connectString("192.168.137.128:2181")
     			//会话超时
                .sessionTimeoutMs(9000)
     			//连接超时
                .connectionTimeoutMs(9000)
     			//重试策略
                .retryPolicy(new ExponentialBackoffRetry(300, 10))
     			//命名空间  该连接操作的根目录
                .namespace("test").build();
		//开启服务
        zkCli.start();
```

### 	2.添加节点

```java
zkCli.create()
    //创建级节点
	.creatingparentsIfNeeded()
    //节点类型
    .withMode()
   	//节点
    .forPath("路径",数据.getBytes(StandardCharsets.UTF_8))
```

### 3.修改节点

```java
zkCli().setData().foreach()
zkCli().
```









