Docker官网 ： https://www.docker.com/

Docker源码 ：https://github.com/moby/moby

Docker团队Git： https://github.com/docker

[深入浅出作者NigelPoulton的Github](https://github.com/nigelpoulton)

#### 1 、Docker能干什么

​		Docker的主要目标是**“Build,Ship and Run Any App,Anywhere”**.

​		就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，是用户的APP及其运行环境能够做到“一次封装，到处运行”。

​		其核心是有Docker image（Docker 镜像）来支撑的。Docker通过**把应用的运行时环境和应用打包在一起，解决了部署环境依赖的问题**；通过引入**分层文件系统**的概念，**解决了空间利用的问题**。它彻底消除了编译、打包与部署、运维之间的鸿沟，与现在互联网企业推崇的DevOps理念不谋而合，大大提高了应用开发部署的效率。



![1604494124563](C:\Users\15761\AppData\Roaming\Typora\typora-user-images\1604494124563.png)

#### 2、Docker的基本组成

​		image：镜像

​		container：容器

​		repository：仓库



#### 3、关于容器技术

​		容器技术，又称为容器虚拟化，是一种操作系统虚拟化，是一种轻量级的虚拟化技术 。

​		**容器=cgroup+namespace+rootfs+容器引擎（用户态工具）**

​					其中各项的功能分别为：		

​					·Cgroup：资源控制。
​					·Namespace：访问隔离。
​					·rootfs：文件系统隔离。
​					·容器引擎：生命周期控制



​		容器的核心技术主要包括**Namespace**和**Cgroup**这2个内核特性。

##### 		3.1	**Namespace（命名空间**）: 

​				主要做**访问隔离**。其原理是针对一类资源进行抽象，并将其封装在一起提供给一个容器使用，对于这类资源，因为每个容器都有自己的抽象，而它们彼此之间是不可见的，所以就可以做到访问隔离。

​				3.1.1	Namespace主要是通过clone、setns和unshare这3个系统调用来完成的。

​				**clone**可以用来创建新的Namespace。

​				**unshare**为已有的进程创建新的Namespace。

​				**setns**可以将进程放到已有的Namespace。

```
docker exec 命令的实现原理就是setns。
```

​				3.1.2	 目前Linux内核总共实现了6种Namespace：

```
·IPC：隔离System V IPC和POSIX消息队列。
·Network：隔离网络资源。
·Mount：隔离文件系统挂载点。
·PID：隔离进程ID。
·UTS：隔离主机名和域名。
·User：隔离用户ID和组ID
```

###### 					3.1.2.1	**UTS Namespace 隔离主机名和域名**

​											为什么要使用UTS Namespace做隔离？

​											这是因为主机名可以用来代替IP地址，因此，也就可以使用主机名在网络上访问某台									机器了，如果不做隔离，这个机制在容器里就会出问题。

###### 					3.1.2.2 	**IPC是Inter-Process Communication的简写，也就是进程间通信**

​											IPC机制会用到**标识符**，两个进程通过标识符找到对应的消息队列进行通信等。

​											IPC Namespace 能做到的事情是，使相同标识符在2个Namespace中代表不通的消									息队列，这样也就使得2个Namespace中的进程不能通过IPC进程通信了。

###### 					3.1.2.3	**PID Namespace用于进程隔离PID号**

​											这样不同Namespace里的进程PID就可以是一样的。

###### 					3.1.2.4	**Mount Namespace用来隔离文件系统挂载点**

​											在创建了一个新的Mount Namespace后，进程系统对文件系统挂载/卸载的动作就不会									影响到其他Namespace。

###### 					**3.1.2.5	Network Namespace 对网络相关的系统资源进行隔离**

​											每个Network Namespace都有自己的网络设备、IP地址、路由表、/proc/net目录、端									口号等。

​											新建的Network Namespace会有一个lookback设备，用户需要在这里做自己的网络配									置。

###### 					3.1.2.6	**User Namespace用来隔离用户和组ID**

​											一个进程在Namespace里的用户和组ID可以与它在host里的ID不一样。

​											**User Namespace最有用的地方在于，host的普通用户进程在容器里可以是0号用户，**									**也就是root用户。**

##### 		3.2	**Cgroup（控制组）**: 

​				主要做**资源控制**。**用于限制和隔离一组进程对系统资源的使用**。其原理是将一组进程放在一个控制组		里，通过给这个控制组分配指定的可用资源，达到控制这一组进程可用资源的目的。

​				对实际资源的分配和管理是由各个Cgroup子系统完成的：

###### 				3.2.1	cpuset子系统

​						 	cpuset可以为一组进程分配指定的CPU和内存节点。

###### 				3.2.2	cpu子系统

​							cpu子系统用于限制进程的CPU占用率。

###### 				3.2.3	cpuacct子系统

​							cpuacct子系统用来统计各个Cgroup的CPU使用情况。

###### 				3.2.4	memory子系统

​							memoryzixt用来显示Cgroup所能使用的内存上限。

###### 				3.2.5	blkio子系统

​							blkiozxt用来限制Cgroup的block I/O带宽。

###### 				3.2.6	devices子系统

​							devices子系统用来控制Cgroup的进程对哪些设备有访问权限。



#### 4 、[Docker镜像](https://docs.docker.com/engine/reference/commandline/image/)

​			镜像：一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

##### 			4.1	列出本机的镜像

```shell
docker images # 列出本地主机上的镜像
	OPTIONS说明：  -a：列出本地所有的镜像
				  -q：只显示镜像ID
				  --digests：显示镜像的摘要信息
				  --no-trunc：显示完整的镜像信息
				  --filter"dangling=true"：显示所有“悬挂”镜像，即没有对应名称和tag的镜像
```

##### 			4.2	创建一个镜像

```shell
docker pull  # 从镜像仓库下载镜像

docker load  # 一般只导入有docker save导出的镜像。导入后的镜像跟原镜像完全一样，包括镜像ID和分层等内容

docker import # 用于导入包含根文件系统的归档，并将之编程Docker镜像，常用来制作Docker基础镜像。
docker export # 把一个镜像导出为根文件系统的归档。

docker build  # 通过Dockerfile文件生成镜像
```

##### 		4.3	搜索镜像

```shell
# 网站 https://hub.docker.com
docker search 
```

##### 		4.4	删除镜像

```shell
docker rmi -f 镜像ID
docker rmi -f $(docker images -qa) # 删除全部镜像
```

##### 		4.5	Docker 镜像生命周期

![1605363214749](C:\Users\15761\AppData\Roaming\Typora\typora-user-images\1605363214749.png)





#### 5、Docker 仓库

​		仓库（repository）用来集中存储Docker镜像，支持镜像分发和更新。

​		5.1	Docker官方的公共仓库 https://hub.docker.com/ ，类似Github托管代码。

​		5.2	[Docker Registry](https://docs.docker.com/engine/reference/commandline/registry/)是构建仓库的核心，用于实现开源Docker镜像的分发.

​				  Docker Registry源码：https://github.com/docker/distribution



#### 6、[Docker	网路](https://docs.docker.com/network/)

​		[docker network 命令](https://docs.docker.com/engine/reference/commandline/network/)

​		Libnetwork项目提供了**容器网路模型（Container Network Model ，简称CNM）**,定义了标准的API用于为容器配置网络，其底层可以适配各种网络驱动。CNM有三个重要概念：

​		1、**沙盒**。沙盒是一个隔离的网络运行环境，保存了容器网络栈的配置，包括了对网络接口、路由表和DNS配置的管理。在Linux平台上，沙盒是用Linux Network Namespace实现的，在其他平台上可能是不同的概念，如FreeBSD Jail。一个沙盒可以包括来自多个网络的多个Endpoint（端点）。

​		2、**Endpoint**。Endpoint将沙盒加入一个网络，Endpoint的实现可以是一对veth pair或者OVS内部端口，当前的Libnetwork使用的是veth pair。一个Endpoint只能隶属于一个沙盒及一个网络。通过给沙盒增加多个Endpoint可以将一个沙盒加入多个网络。
​		3、**网络**。网络包括一组能互相通信的Endpoint。网络的实现可以是Linux bridge、vlan等。

![1605535283110](C:\Users\15761\AppData\Roaming\Typora\typora-user-images\1605535283110.png)



Libnetwork实现了五种驱动（driver）：

| 驱动    |                             特征                             |
| ------- | :----------------------------------------------------------: |
| bridge  |                   Docker默认的容器网路驱动                   |
| host    | 容器与主机共享同一Network Namespace，共享同一套网络协议栈、路由、iptables等 |
| null    |       容器内网络配置为空，需要手动配置网络接口和路由等       |
| remote  |                    Docker网络插件的实现。                    |
| overlay |              Docker原生的跨主机多子网网络方案。              |



#### 7、[数据卷](https://docs.docker.com/storage/volumes/)

​     	Docker容器里产生的数据，如果不通过docker commit生成新的镜像， 使数据作为镜像的一部分保存下来，就会在容器删除后丢失。为了能够**持久化保存和共享容器的数据**，Docker提出了卷（volume）的概念。

```shell
docker run -it -v /宿主机目录:/容器内目录 镜像名
docker run -it -v /宿主机目录:/容器内目录:ro 镜像名 # 以只读的方式挂载一个数据卷
docker inspect 镜像名  # 查看数卷是否挂载成功
```

##### 		7.1	**创建数据卷容器： --volume-from 容器之之间共享**

```shell
# 要创建多个Postgres数据库，并且希望这些数据库之间共享数据，可以先创建一个数据卷容器
docker create -v /dbdata --name dbdata training/postgres /bin/true

# 启动Postgres数据库服务，使用--volumes-from参数将上面生成的数据卷挂载进来，之后启动多个容器，各个容器# 之间就可以通过dbdata数据卷共享数据了：
docker run -d --volumes-from dbdata --name db1 training/postgres
docker run -d --volumes-from dbdata --name db2 training/postgre

#	可以使用--volume-from db1或--volume-from db2的方式挂载dbdata数据卷
docker run -d --name db3 --volumes-from db1 training/postgres
```

​		**数据卷的生命周期一直持续到没有容器使用它为止**

​		使用数据卷容器存储的数据不会轻易丢失，

​		即便删除db1、db2容器甚至是初始化该数据卷的dbdata容器，该数据卷也不会被删除。

​		只有在删除最后一个使用该数据卷的容器时显式地指定docker rm–v$CONTAINER才会删除该数据卷。

##### 	7.2	Docker卷管理的问题

​				1）**只支持本地数据卷。**Docker没有办法把远程服务器的数据卷挂载到本机，

​					  对此 Docker引入了卷插件机制，允许使用者以**第三方插件**的形式，提供对分布式存储的支持。

​				2）**缺乏对数据卷生命周期的有效管理**。Docker没有办法对数据卷进行系统管理。

​					 例如 1、无法使用Docker命令查看当前系统的所有数据卷。

​							  2、只有使用docker rm删除最后一个使用数据卷的容器时显式的加上-v参数，才能删除数据卷，否则数据卷永不会被删除，并且Docker将再也无法管理该数据卷。久而久之这些悬挂数据卷浪费大量的空间。

##### 	7.3	卷插件

​				**工作原理**：社区定义了一套**标准的卷插件REST API**，Docker自身实现了这套API的客户端，它会按照步骤发现、激活插件。当Docker需要创建、挂载、卸载、删除数据卷时，它会向插件发送对应的REST API，由插件来真正完成创建数据卷等工作，这就是卷插件的基本原理。

![1605796277197](C:\Users\15761\AppData\Roaming\Typora\typora-user-images\1605796277197.png)



​		**已有的卷插件**:

​				**·Convoy**：一种基于本地存储的单机版插件，本地支持的存储驱动包括Device Mapper、VFS、EBS等（可将NFS挂载到VFS目录下，实现跨主机存储和共享）。Convoy具有存储备份功能，可以为卷名称设置还原点，并基于还原点恢复数据。由于Convoy是个单机版插件，因此对卷的迁移和共享支持得不是很好。

​				**·Flocker**：另外一个功能很强大的卷插件，支持多种后台存储驱动，包括OpenStack Cinder、AWS EBS、EMC ScaleIO、ZFS等。虽然Flocker支持卷的迁移，但不支持卷共享。







































































































