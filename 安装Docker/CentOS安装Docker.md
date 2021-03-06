Docker官方安装文档    [https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)

[RUNOOB教程](https://www.runoob.com/docker/windows-docker-install.html)



1、卸载旧版本

```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



2、  安装yum-utils包（提供yum-config-manager工具）

```shell
yum install -y yum-utils
```



3、 设置stable镜像仓库

```shell
#阿里云仓库(推荐)
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

#官方仓库(不推荐)
yum-config-manager  --add-repo  https://download.docker.com/linux/centos/docker-ce.repo
```



4、清除yum缓存

```shell
yum clean all	
```



5、更新yum软件包索引

```shell
yum makecache fast
# 报错：Cannot find a valid baseurl for repo: base/7/x86_64
# 解决方法： /root/.bash_profile 中添加代理服务器 export http_proxy=ip:8888
```



6、 安装Docker Engine和containerd

```shell
yum install docker-ce docker-ce-cli containerd.io
```



7、 启动docker

```shell
systemctl start docker
```



8、查看docker状态

```shel
systemctl status docker
```



9、 查看docker版本

```shell
docker version 
```



10、 运行hello-world确认docker是否安装

```shell
docker run hello-world
```



11、查询mysql镜像

```shell
docker search mysql
#报错：Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup       registry-1.docker.io on 59.49.49.49:53: dial udp 59.49.49.49:53: connect: network     	   is unreachable
 
#修改 /etc/resolv.conf 把nameserver 改成8.8.8.8
```



12  、拉取mysql镜像

```shell
docker pull mysql:5.7
```



13 、查看docker镜像

```
docker images
```



14、 删除镜像

```shell
docker rmi -f 容器id
#删除全部镜像
docker rmi -f ${docker images -aq}
```



15 、配置aliyun镜像加速器

```shell
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://544zxoak.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```



16、卸载docker

```shell
sudo yum remove docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
```































