
---
title: Dokcer 配置相关
date: 2022-11-11
---
# Docker使用

windwos 容器位于:\\wsl$\docker-desktop-data\version-pack-data\community\docker\containers

## 端口映射:                

### 1. 创建容器时映射

```shell
docker run -d -p 50001:22 -p 53306:3306 --privileged --name centos8 chenzhongxian/learn:v0.1
```
              
```shell
参数说明:
-p 宿主机端口:容器端口
-d  后台运行
--name 容器名 
--privileged  解决挂载问题 权限  container内的root拥有真正的root权限。
chenzhongxian/learn:v0.1 远程仓库:tag
```



### 2. 后续添加---已创建容器

#### 	1. docker container update --restart=always 容器名字

#### 	2.关闭docker 

#### 	3.修改容器下 config.v2.json

​			 K:容器端口

```json
"Config"{
        "ExposedPorts":{
            "22/tcp":{

            },
            "3306/tcp":{

            }
        }
	}
```

#### 	4.修改hostconfig.json

​			K: 容器端口  V:{ "HostIp":"", "HostPort":"宿主机映射端口"}

```json
"PortBindings":{
        "22/tcp":[
            {
                "HostIp":"",
                "HostPort":"50001"
            }
        ],
        "3306/tcp":[
            {
                "HostIp":"",
                "HostPort":"53306"
            }
        ]
    }
```

##### 	5.docker 重启



## 挂载卷:

### 1. 创建容器时进行挂载
```shell
dockcer run -v 主机:容器
```
### 2. 后续添加已创建容器

#### 1. 修改 hostconfig.json 文件  Binds 列表中增加 "主机目录:容器目录" 
### 2. 修改 config.v2.json 文件
```shell
"MountPoints": {
 
"/import（容器）": {
            "Source": "/data（主机）",
            "Destination": "/import（容器）",
            "RW": true,
            "Name": "",
            "Driver": "",
            "Type": "bind",
            "Propagation": "rprivate",
            "Spec": {
                "Type": "bind",
                "Source": "/data（主机）",
                "Target": "/import（容器）"
            },
            "SkipMountpointCreation": false
        }

```

# Docker-compose
## 简介
docker-compose是基于docker的开源项目，利用yaml配置文件对容器快速编排，只能对本机的docker进行处理