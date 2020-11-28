[官方安装文档](https://docs.docker.com/docker-for-windows/install/)

##### 下载

![1606554029507](images/1606554029507.png)



[下载Docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows/?tab=resources)

![1606553896579](images/1606553896579.png)



##### 安装

![1606554411251](images/1606554411251.png)

确认Windows虚拟化已启用

![1606554428328](images/1606554428328.png)



![1606554718449](images/1606554718449.png)



##### 启动

![1606554846374](images/1606554846374.png)

error1：

![1606556717638](images/1606556717638.png)



```shell
docker run -d -p 80:80 docker/getting-started
```

![1606556348040](images/1606556348040.png)

error2：

```shell
docker:error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.36/containers/json: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
```

![1606556387168](images/1606556387168.png)

修改设置

![1606557051669](images/1606557051669.png)

重启

![1606556463821](images/1606556463821.png)



![1606556473348](images/1606556473348.png)



![1606556482814](images/1606556482814.png)



![1606556492100](images/1606556492100.png)



![1606556499629](images/1606556499629.png)

##### 启动成功

![1606556510645](images/1606556510645.png)

查看docker version

![1606556521715](images/1606556521715.png)





















































