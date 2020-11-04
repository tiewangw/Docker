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

​		一般来说，容器技术主要包括**Namespace**和**Cgroup**这2个内核特性。

​		Namespace（命名空间），主要做访问隔离，

​		Cgroup（控制组），主要做资源控制。