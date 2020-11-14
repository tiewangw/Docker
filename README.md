Docker官网 https://www.docker.com/

Docker源码 ：https://github.com/moby/moby

[深入浅出作者NigelPoulton的Github](https://github.com/nigelpoulton)

#### 1 、Docker能干什么

​	Docker的主要目标是**“Build,Ship and Run Any App,Anywhere”**.

​	就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，是用户的APP及其运行环境能够做到“一次封装，到处运行”。



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











































