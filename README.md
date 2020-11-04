Docker官网 https://www.docker.com/

Docker源码 ：https://github.com/moby/moby

[深入浅出作者NigelPoulton的Github](https://github.com/nigelpoulton)

#### Docker能干什么

​	Docker的主要目标是**“Build,Ship and Run Any App,Anywhere”**.

​	就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，是用户的APP及其运行环境能够做到“一次封装，到处运行”。



![1604494124563](C:\Users\15761\AppData\Roaming\Typora\typora-user-images\1604494124563.png)

#### Docker的基本组成

​		image：镜像

​		container：容器

​		repository：仓库



#### 关于容器技术

​		容器技术，又称为容器虚拟化，是一种操作系统虚拟化，是一种轻量级的虚拟化技术 。

​		**容器=cgroup+namespace+rootfs+容器引擎（用户态工具）**

​					其中各项的功能分别为：		

​					·Cgroup：资源控制。
​					·Namespace：访问隔离。
​					·rootfs：文件系统隔离。
​					·容器引擎：生命周期控制



​		容器的核心技术主要包括**Namespace**和**Cgroup**这2个内核特性。

​		Namespace（命名空间），主要做**访问隔离**。其原理是针对一类资源进行抽象，并将其封装在一起提供给一个容器使用，对于这类资源，因为每个容器都有自己的抽象，而它们彼此之间是不可见的，所以就可以做到访问隔离。

​		Cgroup（控制组），主要做**资源控制**。**用于限制和隔离一组进程对系统资源的使用**。其原理是将一组进程放在一个控制组里，通过给这个控制组分配指定的可用资源，达到控制这一组进程可用资源的目的。



























