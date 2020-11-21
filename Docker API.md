

```shell
[root@localhost ~]# vim /etc/sysconfig/docker
[root@localhost ~]# cat  /etc/sysconfig/docker
#启动Docker守护进程时，需要添加-H参数并指定开启的访问端口
OPTIONS='-H=tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock'
[root@localhost ~]# systemctl stop docker
[root@localhost ~]# systemctl start docker
[root@localhost ~]# docker images
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
ubuntu                               latest              9140108b62dc        8 weeks ago         72.9MB
curldocker                           latest              f1959a19ad13        2 months ago        215MB
centos                               centos7             7e6257c9f8d8        3 months ago        203MB
centos                               latest              0d120b6ccaa8        3 months ago        215MB
tomcat                               latest              2ae23eb477aa        3 months ago        647MB
alpine                               latest              a24bb4013296        5 months ago        5.57MB
nigelpoulton/pluralsight-docker-ci   latest              dd7a37fe7c1e        10 months ago       604MB
openjdk                              8-jdk-alpine        a3562aa0b991        18 months ago       105MB
ansible/centos7-ansible              latest              688353a31fde        3 years ago         447M
[root@localhost ~]# docker -H 127.0.0.1:2375 info
Client:
 Debug Mode: false

Server:
 Containers: 41
  Running: 0
  Paused: 0
  Stopped: 41
 Images: 10
 Server Version: 19.03.12
 Storage Driver: devicemapper
  Pool Name: docker-8:3-50796903-pool
  Pool Blocksize: 65.54kB
  Base Device Size: 10.74GB
  Backing Filesystem: xfs
  Udev Sync Supported: true
  Data file: /dev/loop0
  Metadata file: /dev/loop1
  Data loop file: /var/lib/docker/devicemapper/devicemapper/data
  Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
  Data Space Used: 3.102GB
  Data Space Total: 107.4GB
  Data Space Available: 2.195GB
  Metadata Space Used: 6.431MB
  Metadata Space Total: 2.147GB
  Metadata Space Available: 2.141GB
  Thin Pool Minimum Free Space: 10.74GB
  Deferred Removal Enabled: true
  Deferred Deletion Enabled: true
  Deferred Deleted Device Count: 0
  Library Version: 1.02.164-RHEL7 (2019-08-27)
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-327.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 1.939GiB
 Name: localhost.localdomain
 ID: 3SVS:BMKI:E4UA:RAYU:BPEV:HV42:IUMU:ZNTU:N777:EFNO:NRUX:WA7P
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://544zxoak.mirror.aliyuncs.com/
 Live Restore Enabled: false

WARNING: API is accessible on http://0.0.0.0:2375 without encryption.
         Access to the remote API is equivalent to root access on the host. Refer
         to the 'Docker daemon attack surface' section in the documentation for
         more information: https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
WARNING: the devicemapper storage-driver is deprecated, and will be removed in a future release.
WARNING: devicemapper: usage of loopback devices is strongly discouraged for production use.
         Use `--storage-opt dm.thinpooldev` to specify a custom block storage device.


#  调用/images/json接口可以获取镜像列表
#	python -mjson.tool格式化json
[root@localhost ~]# curl http://localhost:2375/images/json | python -mjson.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  4155    0  4155    0     0   564k      0 --:--:-- --:--:-- --:--:--  579k
[
    {
        "Containers": -1,
        "Created": 1601073270,
        "Id": "sha256:9140108b62dc87d9b278bb0d4fd6a3e44c2959646eb966b86531306faa81b09b",
        "Labels": null,
        "ParentId": "",
        "RepoDigests": [
            "ubuntu@sha256:bc2f7250f69267c9c6b66d7b6a81a54d3878bb85f1ebb5f951c896d13e6ba537"
        ],
        "RepoTags": [
            "ubuntu:latest"
        ],
        "SharedSize": -1,
        "Size": 72872854,
        "VirtualSize": 72872854
    },
    {
        "Containers": -1,
        "Created": 1599355955,
        "Id": "sha256:f1959a19ad135ca9fda80b99331e343cf2166d32a7db22782c5ec77c97ca4c34",
        "Labels": {
            "org.label-schema.build-date": "20200809",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS"
        },
        "ParentId": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "RepoDigests": null,
        "RepoTags": [
            "curldocker:latest"
        ],
        "SharedSize": -1,
        "Size": 215055481,
        "VirtualSize": 215055481
    },
    {
        "Containers": -1,
        "Created": 1597083609,
        "Id": "sha256:7e6257c9f8d8d4cdff5e155f196d67150b871bbe8c02761026f803a704acb3e9",
        "Labels": {
            "org.label-schema.build-date": "20200809",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS",
            "org.opencontainers.image.created": "2020-08-09 00:00:00+01:00",
            "org.opencontainers.image.licenses": "GPL-2.0-only",
            "org.opencontainers.image.title": "CentOS Base Image",
            "org.opencontainers.image.vendor": "CentOS"
        },
        "ParentId": "",
        "RepoDigests": [
            "centos@sha256:19a79828ca2e505eaee0ff38c2f3fd9901f4826737295157cc5212b7a372cd2b"
        ],
        "RepoTags": [
            "centos:centos7"
        ],
        "SharedSize": -1,
        "Size": 203325730,
        "VirtualSize": 203325730
    },
    {
        "Containers": -1,
        "Created": 1597083589,
        "Id": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "Labels": {
            "org.label-schema.build-date": "20200809",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS"
        },
        "ParentId": "",
        "RepoDigests": [
            "centos@sha256:76d24f3ba3317fa945743bb3746fbaf3a0b752f10b10376960de01da70685fbd"
        ],
        "RepoTags": [
            "centos:latest"
        ],
        "SharedSize": -1,
        "Size": 215055481,
        "VirtualSize": 215055481
    },
    {
        "Containers": -1,
        "Created": 1596655386,
        "Id": "sha256:2ae23eb477aa82782438e429f22e551c1a093e2aebb804fb3d1463dd510c16cb",
        "Labels": null,
        "ParentId": "",
        "RepoDigests": [
            "tomcat@sha256:9de2415ccf10fe8e5906e4b72eda21649a7a1d0b88e9111f8409062599f3728e"
        ],
        "RepoTags": [
            "tomcat:latest"
        ],
        "SharedSize": -1,
        "Size": 647423822,
        "VirtualSize": 647423822
    },
    {
        "Containers": -1,
        "Created": 1590787186,
        "Id": "sha256:a24bb4013296f61e89ba57005a7b3e52274d8edd3ae2077d04395f806b63d83e",
        "Labels": null,
        "ParentId": "",
        "RepoDigests": [
            "alpine@sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321"
        ],
        "RepoTags": [
            "alpine:latest"
        ],
        "SharedSize": -1,
        "Size": 5570176,
        "VirtualSize": 5570176
    },
    {
        "Containers": -1,
        "Created": 1579361364,
        "Id": "sha256:dd7a37fe7c1e6f3b9bcd1c51cad0a54fde3f393ac458af3b009b2032978f599d",
        "Labels": {
            "MAINTAINER": "nigelpoulton@hotmail.com",
            "org.label-schema.build-date": "20190927",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS"
        },
        "ParentId": "",
        "RepoDigests": [
            "nigelpoulton/pluralsight-docker-ci@sha256:61bc64850a5f2bfbc65967cc33feaae8a77c8b49379c55aaf05bb02dcee41451"
        ],
        "RepoTags": [
            "nigelpoulton/pluralsight-docker-ci:latest"
        ],
        "SharedSize": -1,
        "Size": 604123236,
        "VirtualSize": 604123236
    },
    {
        "Containers": -1,
        "Created": 1557538337,
        "Id": "sha256:a3562aa0b991a80cfe8172847c8be6dbf6e46340b759c2b782f8b8be45342717",
        "Labels": null,
        "ParentId": "",
        "RepoDigests": [
            "openjdk@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3"
        ],
        "RepoTags": [
            "openjdk:8-jdk-alpine"
        ],
        "SharedSize": -1,
        "Size": 104796897,
        "VirtualSize": 104796897
    },
    {
        "Containers": -1,
        "Created": 1482135553,
        "Id": "sha256:688353a31fdee02a966d1f83e9210f77b5a63baaaacbedb81ca35f6231cfeb6c",
        "Labels": {
            "build-date": "20161214",
            "license": "GPLv2",
            "name": "CentOS Base Image",
            "vendor": "CentOS"
        },
        "ParentId": "",
        "RepoDigests": [
            "ansible/centos7-ansible@sha256:39eff7d56b96530d014083cd343f7314c23acbd1ecf37eb75a71a2f6584d0b02"
        ],
        "RepoTags": [
            "ansible/centos7-ansible:latest"
        ],
        "SharedSize": -1,
        "Size": 447160864,
        "VirtualSize": 447160864
    }
]
[root@localhost ~]# docker run -it centos /bin/bash
[root@c4787c1799e5 /]# [root@localhost ~]# 
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
c4787c1799e5        centos              "/bin/bash"         About a minute ago   Up About a minute                       keen_bose


# /containers/json接口可以获取正在运行中的容器列表
[root@localhost ~]# curl http://localhost:2375/containers/json | python -mjson.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   982  100   982    0     0   134k      0 --:--:-- --:--:-- --:--:--  159k
[
    {
        "Command": "/bin/bash",
        "Created": 1605924653,
        "HostConfig": {
            "NetworkMode": "default"
        },
        "Id": "c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b",
        "Image": "centos",
        "ImageID": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "Labels": {
            "org.label-schema.build-date": "20200809",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS"
        },
        "Mounts": [],
        "Names": [
            "/keen_bose"
        ],
        "NetworkSettings": {
            "Networks": {
                "bridge": {
                    "Aliases": null,
                    "DriverOpts": null,
                    "EndpointID": "08cd104bb749460f82649c17ce4ab2264e773f67c1b433e48d5070f27653a7c6",
                    "Gateway": "172.17.0.1",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "IPAMConfig": null,
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "Links": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "NetworkID": "0bd6a3015a3c04e711db25a7268c57249b00b2f6e8bd0157a6048b63a958a45c"
                }
            }
        },
        "Ports": [],
        "State": "running",
        "Status": "Up 2 minutes"
    }
]

 # 监控容器。使用容器id获取该容器底层信息
[root@localhost ~]# curl http://localhost:2375/containers/c4787c1799e5/json | python -mjson.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  4323    0  4323    0     0   689k      0 --:--:-- --:--:-- --:--:--  844k
{
    "AppArmorProfile": "",
    "Args": [],
    "Config": {
        "AttachStderr": true,
        "AttachStdin": true,
        "AttachStdout": true,
        "Cmd": [
            "/bin/bash"
        ],
        "Domainname": "",
        "Entrypoint": null,
        "Env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        ],
        "Hostname": "c4787c1799e5",
        "Image": "centos",
        "Labels": {
            "org.label-schema.build-date": "20200809",
            "org.label-schema.license": "GPLv2",
            "org.label-schema.name": "CentOS Base Image",
            "org.label-schema.schema-version": "1.0",
            "org.label-schema.vendor": "CentOS"
        },
        "OnBuild": null,
        "OpenStdin": true,
        "StdinOnce": true,
        "Tty": true,
        "User": "",
        "Volumes": null,
        "WorkingDir": ""
    },
    "Created": "2020-11-21T02:10:53.708594013Z",
    "Driver": "devicemapper",
    "ExecIDs": null,
    "GraphDriver": {
        "Data": {
            "DeviceId": "144",
            "DeviceName": "docker-8:3-50796903-44e041ad2ddde3ca042ee0dbadb0b84efa04f92bba8c60830c53050bcd2bed4f",
            "DeviceSize": "10737418240"
        },
        "Name": "devicemapper"
    },
    "HostConfig": {
        "AutoRemove": false,
        "Binds": null,
        "BlkioDeviceReadBps": null,
        "BlkioDeviceReadIOps": null,
        "BlkioDeviceWriteBps": null,
        "BlkioDeviceWriteIOps": null,
        "BlkioWeight": 0,
        "BlkioWeightDevice": [],
        "CapAdd": null,
        "CapDrop": null,
        "Capabilities": null,
        "Cgroup": "",
        "CgroupParent": "",
        "ConsoleSize": [
            0,
            0
        ],
        "ContainerIDFile": "",
        "CpuCount": 0,
        "CpuPercent": 0,
        "CpuPeriod": 0,
        "CpuQuota": 0,
        "CpuRealtimePeriod": 0,
        "CpuRealtimeRuntime": 0,
        "CpuShares": 0,
        "CpusetCpus": "",
        "CpusetMems": "",
        "DeviceCgroupRules": null,
        "DeviceRequests": null,
        "Devices": [],
        "Dns": [],
        "DnsOptions": [],
        "DnsSearch": [],
        "ExtraHosts": null,
        "GroupAdd": null,
        "IOMaximumBandwidth": 0,
        "IOMaximumIOps": 0,
        "IpcMode": "private",
        "Isolation": "",
        "KernelMemory": 0,
        "KernelMemoryTCP": 0,
        "Links": null,
        "LogConfig": {
            "Config": {},
            "Type": "json-file"
        },
        "MaskedPaths": [
            "/proc/asound",
            "/proc/acpi",
            "/proc/kcore",
            "/proc/keys",
            "/proc/latency_stats",
            "/proc/timer_list",
            "/proc/timer_stats",
            "/proc/sched_debug",
            "/proc/scsi",
            "/sys/firmware"
        ],
        "Memory": 0,
        "MemoryReservation": 0,
        "MemorySwap": 0,
        "MemorySwappiness": null,
        "NanoCpus": 0,
        "NetworkMode": "default",
        "OomKillDisable": false,
        "OomScoreAdj": 0,
        "PidMode": "",
        "PidsLimit": null,
        "PortBindings": {},
        "Privileged": false,
        "PublishAllPorts": false,
        "ReadonlyPaths": [
            "/proc/bus",
            "/proc/fs",
            "/proc/irq",
            "/proc/sys",
            "/proc/sysrq-trigger"
        ],
        "ReadonlyRootfs": false,
        "RestartPolicy": {
            "MaximumRetryCount": 0,
            "Name": "no"
        },
        "Runtime": "runc",
        "SecurityOpt": null,
        "ShmSize": 67108864,
        "UTSMode": "",
        "Ulimits": null,
        "UsernsMode": "",
        "VolumeDriver": "",
        "VolumesFrom": null
    },
    "HostnamePath": "/var/lib/docker/containers/c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b/hostname",
    "HostsPath": "/var/lib/docker/containers/c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b/hosts",
    "Id": "c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b",
    "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
    "LogPath": "/var/lib/docker/containers/c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b/c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b-json.log",
    "MountLabel": "",
    "Mounts": [],
    "Name": "/keen_bose",
    "NetworkSettings": {
        "Bridge": "",
        "EndpointID": "08cd104bb749460f82649c17ce4ab2264e773f67c1b433e48d5070f27653a7c6",
        "Gateway": "172.17.0.1",
        "GlobalIPv6Address": "",
        "GlobalIPv6PrefixLen": 0,
        "HairpinMode": false,
        "IPAddress": "172.17.0.2",
        "IPPrefixLen": 16,
        "IPv6Gateway": "",
        "LinkLocalIPv6Address": "",
        "LinkLocalIPv6PrefixLen": 0,
        "MacAddress": "02:42:ac:11:00:02",
        "Networks": {
            "bridge": {
                "Aliases": null,
                "DriverOpts": null,
                "EndpointID": "08cd104bb749460f82649c17ce4ab2264e773f67c1b433e48d5070f27653a7c6",
                "Gateway": "172.17.0.1",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "IPAMConfig": null,
                "IPAddress": "172.17.0.2",
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "Links": null,
                "MacAddress": "02:42:ac:11:00:02",
                "NetworkID": "0bd6a3015a3c04e711db25a7268c57249b00b2f6e8bd0157a6048b63a958a45c"
            }
        },
        "Ports": {},
        "SandboxID": "d7a03ee69f7fd5b26ccb25c9248dbfdad7ac9163f798a5ac270da8d0f8e081a5",
        "SandboxKey": "/var/run/docker/netns/d7a03ee69f7f",
        "SecondaryIPAddresses": null,
        "SecondaryIPv6Addresses": null
    },
    "Path": "/bin/bash",
    "Platform": "linux",
    "ProcessLabel": "",
    "ResolvConfPath": "/var/lib/docker/containers/c4787c1799e56537df90c5fcd02a5f40ac13a005162547016d8f68190a50b51b/resolv.conf",
    "RestartCount": 0,
    "State": {
        "Dead": false,
        "Error": "",
        "ExitCode": 0,
        "FinishedAt": "0001-01-01T00:00:00Z",
        "OOMKilled": false,
        "Paused": false,
        "Pid": 5573,
        "Restarting": false,
        "Running": true,
        "StartedAt": "2020-11-21T02:10:54.31313752Z",
        "Status": "running"
    }
}
[root@localhost ~]# 


```

