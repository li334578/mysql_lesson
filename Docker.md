# Docker概述

隔离：打包装箱，每个箱子是相互隔离的。

# Docker历史

容器化

虚拟机

虚拟化技术

# Docker的基本组成

镜像(image)：模板，通过镜像可以创建多个容器。

容器(container)：利用容器技术，独立运行一个或多个组应用，通过镜像来创建。启动、停止、删除。简易的linux系统

仓库(repository)：存放镜像。

## Docker原理

Docker是一个CS结构的系统，守护进程。通过Socket从客户端访问。

DockerServer 接收到Docker-client的指令，就会执行这个指令。

Docker为什么比VM更快

1.Docker有着比虚拟机更小的抽象层

2.Docker利用的是宿主机的内核

# Docker的常用命令

## 帮助命令

```shell
docker version # 显示docker的版本信息
docker info # 显示docker的系统信息，包括镜像和容器的数量。
docker -- help # 帮助命令
```

## 镜像命令

```shell
docker images # 查看
[root@localhost docker]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        11 months ago       13.3kB
# 解释
REPOSITORY 镜像的仓库源
TAG 镜像的标签
IMAGE ID 镜像的ID
CREATED 镜像的创建时间
SIZE 镜像的大小

# 可选项
Options:
# 列出所有镜像
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
# 过滤
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
# 只显示镜像的ID
  -q, --quiet           Only show numeric IDs

```

---

```shell
docker search # 搜索镜像
[root@localhost docker]# docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10202               [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3753                [OK]                
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   746                                     [OK]
--------------------------------
Usage:	docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
--filter=STARS=3000 #搜索星数大于3000的
[root@localhost docker]# docker search mysql --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   10202               [OK]                
mariadb             MariaDB is a community-developed fork of MyS…   3753                [OK] 
```

-------

```shell
docker pull # 下载镜像
[root@localhost docker]# docker pull --help

Usage:	docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output 
-------------------------------------------------
docker pull mysql [:tag]
如果不写tag默认拉取最新的latest
[root@localhost ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
852e50cd189d: Pull complete 
29969ddb0ffb: Pull complete 
a43f41a44c48: Pull complete 
5cdd802543a3: Pull complete 
b79b040de953: Pull complete 
938c64119969: Pull complete 
7689ec51a0d9: Pull complete 
a880ba7c411f: Pull complete 
984f656ec6ca: Pull complete 
9f497bce458a: Pull complete 
b9940f97694b: Pull complete 
2f069358dc96: Pull complete 
Digest: sha256:4bb2e81a40e9d0d59bd8e3dc2ba5e1f2197696f6de39a91e90798dd27299b093
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址
----------------------------------
docker pull mysql # 等价
docker pull docker.io/library/mysql:latest
----------------------------------
# 指定版本下载
docker pull mysql:5.7
```

---

```shell
docker rmi # 删除镜像 i->id
[root@localhost ~]# docker rmi --help

Usage:	docker rmi [OPTIONS] IMAGE [IMAGE...]

Remove one or more images

Options:
  -f, --force      Force removal of the image
      --no-prune   Do not delete untagged parents
# 删除指定的镜像
docker rmi -f 镜像id
# 删除多个镜像
docker rmi -f 镜像id 镜像id 镜像id
# 递归删除所有镜像
docker rmi -f $(docker images -aq)
```



## 容器命令

说明：有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习

```shell
docker pull centos
```

新建容器并启动

```shell
docker run # 容器启动命令
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                  Add a custom host-to-IP mapping (host:ip)
  -a, --attach list                    Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16            Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device list       Block IO weight (relative device weight) (default [])
      --cap-add list                   Add Linux capabilities
      --cap-drop list                  Drop Linux capabilities
      --cgroup-parent string           Optional parent cgroup for the container
      --cidfile string                 Write the container ID to the file
      --cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int              Limit CPU real-time period in microseconds
      --cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                 CPU shares (relative weight)
      --cpus decimal                   Number of CPUs
      --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                         Run container in background and print container ID
      --detach-keys string             Override the key sequence for detaching a container
      --device list                    Add a host device to the container
      --device-cgroup-rule list        Add a rule to the cgroup allowed devices list
      --device-read-bps list           Limit read rate (bytes per second) from a device (default [])
      --device-read-iops list          Limit read rate (IO per second) from a device (default [])
      --device-write-bps list          Limit write rate (bytes per second) to a device (default [])
      --device-write-iops list         Limit write rate (IO per second) to a device (default [])
      --disable-content-trust          Skip image verification (default true)
      --dns list                       Set custom DNS servers
      --dns-option list                Set DNS options
      --dns-search list                Set custom DNS search domains
      --domainname string              Container NIS domain name
      --entrypoint string              Overwrite the default ENTRYPOINT of the image
  -e, --env list                       Set environment variables
      --env-file list                  Read in a file of environment variables
      --expose list                    Expose a port or a range of ports
      --gpus gpu-request               GPU devices to add to the container ('all' to pass all GPUs)
      --group-add list                 Add additional groups to join
      --health-cmd string              Command to run to check health
      --health-interval duration       Time between running the check (ms|s|m|h) (default 0s)
      --health-retries int             Consecutive failures needed to report unhealthy
      --health-start-period duration   Start period for the container to initialize before starting health-retries countdown (ms|s|m|h) (default 0s)
      --health-timeout duration        Maximum time to allow one check to run (ms|s|m|h) (default 0s)
      --help                           Print usage
  -h, --hostname string                Container host name
      --init                           Run an init inside the container that forwards signals and reaps processes
  -i, --interactive                    Keep STDIN open even if not attached
      --ip string                      IPv4 address (e.g., 172.30.100.104)
      --ip6 string                     IPv6 address (e.g., 2001:db8::33)
      --ipc string                     IPC mode to use
      --isolation string               Container isolation technology
      --kernel-memory bytes            Kernel memory limit
  -l, --label list                     Set meta data on a container
      --label-file list                Read in a line delimited file of labels
      --link list                      Add link to another container
      --link-local-ip list             Container IPv4/IPv6 link-local addresses
      --log-driver string              Logging driver for the container
      --log-opt list                   Log driver options
      --mac-address string             Container MAC address (e.g., 92:d0:c6:0a:29:33)
  -m, --memory bytes                   Memory limit
      --memory-reservation bytes       Memory soft limit
      --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
      --mount mount                    Attach a filesystem mount to the container
      --name string                    Assign a name to the container
      --network network                Connect a container to a network
      --network-alias list             Add network-scoped alias for the container
      --no-healthcheck                 Disable any container-specified HEALTHCHECK
      --oom-kill-disable               Disable OOM Killer
      --oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)
      --pid string                     PID namespace to use
      --pids-limit int                 Tune container pids limit (set -1 for unlimited)
      --platform string                Set platform if server is multi-platform capable
      --privileged                     Give extended privileges to this container
  -p, --publish list                   Publish a container's port(s) to the host
  -P, --publish-all                    Publish all exposed ports to random ports
      --read-only                      Mount the container's root filesystem as read only
      --restart string                 Restart policy to apply when a container exits (default "no")
      --rm                             Automatically remove the container when it exits
      --runtime string                 Runtime to use for this container
      --security-opt list              Security Options
      --shm-size bytes                 Size of /dev/shm
      --sig-proxy                      Proxy received signals to the process (default true)
      --stop-signal string             Signal to stop a container (default "SIGTERM")
      --stop-timeout int               Timeout (in seconds) to stop a container
      --storage-opt list               Storage driver options for the container
      --sysctl map                     Sysctl options (default map[])
      --tmpfs list                     Mount a tmpfs directory
  -t, --tty                            Allocate a pseudo-TTY
      --ulimit ulimit                  Ulimit options (default [])
  -u, --user string                    Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                  User namespace to use
      --uts string                     UTS namespace to use
  -v, --volume list                    Bind mount a volume
      --volume-driver string           Optional volume driver for the container
      --volumes-from list              Mount volumes from the specified container(s)
  -w, --workdir string                 Working directory inside the container
--------------------------------------------------
# 参数说明
--name="Name" # 容器名字 用来区分容器
-d # 后台方式运行
-it # 交互方式运行，进入容器查看内容
-p # 指定容器的端口 8080:8080
	-p ip:主机端口:容器端口
	-p 主机端口:容器端口
	-p 容器端口
	容器端口
-P # 随机指定端口
# 启动并进入容器
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker run -it centos /bin/bash
[root@fcaf546fd2a8 /]# 
[root@fcaf546fd2a8 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
bash: clear: command not found
[root@fcaf546fd2a8 /]# exit
exit
```

----

列出所有正在运行的容器

```shell
docker ps
[root@iZ8vbdsaostzztcixyun1sZ /]# docker ps --help

Usage:	docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
--------------------------------------------
   # 列出当前正在运行的容器
-a # 列出当前正在运行的容器+带出历史运行的容器
-n=? # 显示最近创建的容器
-q # 只显示容器的编号
[root@iZ8vbdsaostzztcixyun1sZ /]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@iZ8vbdsaostzztcixyun1sZ /]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                        PORTS               NAMES
fcaf546fd2a8        centos              "/bin/bash"         2 minutes ago       Exited (127) 50 seconds ago                       brave_meitner
ffaef70739b5        hello-world         "/hello"            6 hours ago         Exited (0) 6 hours ago                            festive_kepler

```

---

```shell
docker exit # 退出当前容器并停止
[root@iZ8vbdsaostzztcixyun1sZ /]# docker exit --help

Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit
Ctrl + P + Q # 容器不停止退出
```

---

```shell
# 移除容器
docker rm
[root@iZ8vbdsaostzztcixyun1sZ /]#  docker rm --help

Usage:	docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers

Options:
  -f, --force     Force the removal of a running container (uses SIGKILL)
  -l, --link      Remove the specified link
  -v, --volumes   Remove anonymous volumes associated with the container
# 移除指定容器，不能移除正在运行的容器，如果要强制移除使用-f
docker rm 容器id
# 移除所有容器
docker rm -f $(docker ps -aq)
docker ps -a -q | xargs docker rm # 也可以删除所有的容器
```

---

```shell
# 启动和停止容器
docker start 容器id # 启动容器
docker restart 容器id # 重启容器
docker stop 容器id # 停止正在运行的容器
docker kill 容器id # 强制停止正在运行的容器
```

## 常用的其他命令

```shell
# 后台启动容器
docker run -d centos
# docker ps 发现容器停止了 docker 容器使用后台运行，就必须要有一个前台进程。docker发现没有应用了，就会自动停止
```

---

```shell
# 查看日志命令
docker logs
[root@iZ8vbdsaostzztcixyun1sZ /]# docker logs --help

Usage:	docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
      --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
docker logs -tf --tail 100 容器id
docker logs -tf 容器id
# 写shell脚本
docker run -d centos /bin/bash -c "while true;do echo helloBash;sleep 1;done"
```

---

```shell
# 查看容器中的进程信息
docker top 命令
[root@iZ8vbdsaostzztcixyun1sZ /]# docker top e5df58841fd6
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                3404                3387                0                   21:09               ?                   00:00:00            /bin/bash -c while true;do echo helloBash;sleep 1;done
root                3620                3404                0                   21:09               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
docker top 容器id
```

---

```shell
# 查看镜像元数据
docker inspect --help
[root@iZ8vbdsaostzztcixyun1sZ /]# docker inspet --help

Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit
---------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ /]# docker inspect e5df58841fd6
[
    {
        "Id": "e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f",
        "Created": "2020-12-02T13:09:15.292048801Z",
        "Path": "/bin/bash",
        "Args": [
            "-c",
            "while true;do echo helloBash;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3404,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-12-02T13:09:15.632034339Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f/hostname",
        "HostsPath": "/var/lib/docker/containers/e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f/hosts",
        "LogPath": "/var/lib/docker/containers/e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f/e5df58841fd6b45786f115ec7c0fac5ef2926ff931d5d50888bde678341b064f-json.log",
        "Name": "/reverent_edison",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
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
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/aec8853250b5a739247e63fce8ee6a3f3699a74922eeed391568e067fd60dcc8-init/diff:/var/lib/docker/overlay2/dd33e1b837d9a9a1fa79158f90d5b6e11bf4cf7ca91602652af7ab6764fc8d48/diff",
                "MergedDir": "/var/lib/docker/overlay2/aec8853250b5a739247e63fce8ee6a3f3699a74922eeed391568e067fd60dcc8/merged",
                "UpperDir": "/var/lib/docker/overlay2/aec8853250b5a739247e63fce8ee6a3f3699a74922eeed391568e067fd60dcc8/diff",
                "WorkDir": "/var/lib/docker/overlay2/aec8853250b5a739247e63fce8ee6a3f3699a74922eeed391568e067fd60dcc8/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "e5df58841fd6",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash",
                "-c",
                "while true;do echo helloBash;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "9bfdfb661ee1c09ee16b19e7655d76550b81d9b16ca8abe65278dcdb400ffdfb",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/9bfdfb661ee1",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "9a511f926fdbd94a8960980459d425c113d346f40e7546abfaca855183fa0e1a",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "bd7748cf0e3c3c0dc713d3c59e939739685cbd473fcee6977bc1191fdf6ddd81",
                    "EndpointID": "9a511f926fdbd94a8960980459d425c113d346f40e7546abfaca855183fa0e1a",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
-------------------------------------------------------
docker inspect 容器id
```

---

```shell
# 进入当前正在运行的容器
# 通常容器都是使用后台方式运行的，需要进入容器修改一些配置
docker exec -it 容器id /bin/bash
------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ /]# docker exec -it e5df58841fd6 /bin/bash
[root@e5df58841fd6 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@e5df58841fd6 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 13:09 ?        00:00:00 /bin/bash -c while true;do echo helloBash;sleep 1;done
root       440     0  0 13:16 pts/0    00:00:00 /bin/bash
root       469     1  0 13:16 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root       470   440  0 13:16 pts/0    00:00:00 ps -ef
----------------------------------------------------------
docker attach 容器id
# 正在执行的代码
docker exec # 进入容器后开启一个新的终端
docker attach # 进入容器正在执行的终端
```

----

```shell
# 从容器内拷贝文件到主机上
docker cp 容器id:容器内的路径 目的主机路径
[root@iZ8vbdsaostzztcixyun1sZ /]# docker cp --help
-----------------------------------------------------------
Usage:	docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
	docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Copy files/folders between a container and the local filesystem

Use '-' as the source to read a tar archive from stdin
and extract it to a directory destination in a container.
Use '-' as the destination to stream a tar archive of a
container source to stdout.

Options:
  -a, --archive       Archive mode (copy all uid/gid information)
  -L, --follow-link   Always follow symbol link in SRC_PATH
-----------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ /]# docker cp f95ce0163c41:/java.txt /
-----------------------------------------------------------
# 拷贝是一个手动的过程，未来使用 -v卷的技术，可以实现自动同步
```

# 小结

![](http://cdn.liwenbo.cool/dockerBash.png)

```shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

# 练习
## docker 安装nginx

```shell
# 1.搜索镜像 docker search
# 2.拉取镜像 docker pull
# 3.运行测试
docker run -d --name nginx01 -p 3344:80 nginx
-d 后台运行
--name 给容器命名
-p 宿主机端口
--------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ /]# docker run -d --name nginx01 -p 3344:80 nginx
e16dac9f17bc5082c6768a64273a89685360c2a907e0122de2d141895a400c28
[root@iZ8vbdsaostzztcixyun1sZ /]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
e16dac9f17bc        nginx               "/docker-entrypoint.…"   25 seconds ago      Up 24 seconds       0.0.0.0:3344->80/tcp   nginx01
[root@iZ8vbdsaostzztcixyun1sZ /]# netstat -anp | grep 3344
tcp6       0      0 :::3344                 :::*                    LISTEN      32498/docker-proxy  
[root@iZ8vbdsaostzztcixyun1sZ /]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
-------------------------------------------------------
# 进入容器
[root@iZ8vbdsaostzztcixyun1sZ /]# docker exec -it nginx01 /bin/bash
root@e16dac9f17bc:/# ls
bin  boot  dev	docker-entrypoint.d  docker-entrypoint.sh  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@e16dac9f17bc:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@e16dac9f17bc:/# cd /etc/nginx/
root@e16dac9f17bc:/etc/nginx# ls
conf.d	fastcgi_params	koi-utf  koi-win  mime.types  modules  nginx.conf  scgi_params	uwsgi_params  win-utf
root@e16dac9f17bc:/etc/nginx# 
# 每次修改nginx配置文件都需要进入容器内部，十分麻烦。-v数据卷
```

## docker 安装tomcat

```shell
# 官方的使用
docker run -it -rm tomcat:9.0
# 之前的启动都是后台，停止了容器之后，容器还可以查到
# docker run -it -rm 一般用来测试，用完即删
# 先下载再启动
docker pull tomcat # 下载最新版的
docker run -d -p 3355:8080 --name tomcat01 tomcat # 启动最新版的
# 指定版本号的
docker pull tomcar:9.0
docker run -d -p 3356:8080 --name tomcat01 tomcat:9.0
------------------------------------------------------
docker ps
[root@iZ8vbdsaostzztcixyun1sZ /]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS                    NAMES
14d99b632fe5        tomcat:9.0          "catalina.sh run"   4 seconds ago        Up 2 seconds        0.0.0.0:3356->8080/tcp   tomcat02
83efc34b12f8        tomcat              "catalina.sh run"   About a minute ago   Up About a minute   0.0.0.0:3355->8080/tcp   tomcat01
------------------------------------------------------
# 进入容器
[root@iZ8vbdsaostzztcixyun1sZ /]# docker exec -it tomcat01 /bin/bash
root@83efc34b12f8:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
# linux命令不全、没有webapps。默认是最小的镜像，所有不必要的都被剔除了。仅保证最小可用的环境。
cp -r ./webapps.dist/* ./webapps
```

# docker镜像


## commit镜像

```shell
docker commit 提交容器成为一个新的版本
# 命令和git原理类似
docker commit -m="提交的信息描述" -a="作者" 容器id 目标镜像名:[TAG]
------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker commit --help

Usage:	docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)
------------------------------------------------------------
docker commit -a="lwb" -m"add webapps application" 14d99b632fe5 tomcat02:1.0
------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
tomcat02            1.0                 cd7437aab119        6 seconds ago       654MB
------------------------------------------------------------
# 以后可以直接使用修改过的镜像来操作

```

学习方式说明：理解概念，但是一定要实践，最后实践和理论相结合一次搞定这个知识。

```shell
# 如果你想保存当前容器的状态，就可以通过commit来提交，获得一个镜像
# 类似于虚拟机的快照
```

# 容器数据卷

## 什么是容器数据卷

docker就是将应用和环境打包成一个镜像

如果数据都在容器中，容器删除后数据就会丢失。

**数据持久化** MySQL的数据可以存储在本地

docker容器产生的数据，同步到本地。这就是卷技术

将我们的容器目录挂载到Linux上面！

总结：容器的持久化和同步操作！容器间也是可以数据共享的。

## 使用数据卷

> 方式一：直接使用命令挂载 -v

```shell
docker run -it -v 主机目录：容器目录 
docker run -it -v /home/test:/home centos /bin/bash
----------------------------------------------------------------
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/test",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
----------------------------------------------------------------
[root@90fda77c5cb6 home]# touch 1.txt
[root@90fda77c5cb6 home]# ls
1.txt
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ test]# ls
1.txt
[root@iZ8vbdsaostzztcixyun1sZ test]# 
----------------------------------------------------------------
# 停止容器，在宿主机上修改文件，依然可以同步到容器中
```

- 好处：以后修改直接在宿主机上修改即可，容器会自动同步

## MySQL实战

思考：MySQL的数据持久化问题

```shell
# 启动mysql需要配置密码的
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=654321 --name mysql01 mysql:5.7
# -d 后台运行
# -p 端口映射
# -v 数据卷挂载
# -e 环境配置
## --name 容器名字
```

## 具名挂载和匿名挂载

```shell
# 匿名挂载
-v 容器挂载
docker run -d -P --name nginx01 -v /etc/nginx nginx
[root@iZ8vbdsaostzztcixyun1sZ data]# docker volume --help
----------------------------------------------------------------
Usage:	docker volume COMMAND

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

Run 'docker volume COMMAND --help' for more information on a command.
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ data]# docker volume ls
DRIVER              VOLUME NAME
local               b61fcc815032209cf2a3f1127a86a2655c156a7031eb3d56b87e3263931a8689
----------------------------------------------------------------
docker volume ls # 查看所有卷的情况
# 匿名挂载，使用-v的时候只指定了容器内的路径没有指定宿主机的路径
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ data]# docker run -d -P --name nignx03 -v juming-nginx:/etc/nginx nginx
7b16cc1da8ea02106cb827d4d6ecceb7114b3922c7df4fb868c330011bd18c67
[root@iZ8vbdsaostzztcixyun1sZ data]# docker volume ls
DRIVER              VOLUME NAME
local               b61fcc815032209cf2a3f1127a86a2655c156a7031eb3d56b87e3263931a8689
local               juming-nginx
----------------------------------------------------------------
# 具名挂载 通过-v 卷名:容器内路径
# 查看一下 docker volume inspect
[root@iZ8vbdsaostzztcixyun1sZ data]# docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2020-12-03T21:02:03+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
----------------------------------------------------------------
# 所有的docker容器内的卷，没有指定目录的情况下都是在 /var/lib/docker/volumes/xxxxx/_data
# 我们可以通过具名挂载很方便的找到一个卷，大多数情况下都是使用具名挂载
```

## 如何确定是具名挂载还是匿名挂载

```shell
-v 容器内的路径 # 匿名挂载
-v 卷名:容器内的路径 # 具名挂载
-v /宿主机路径:容器内的路径 # 指定路径挂载
```

## 拓展

```shell
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
# 通过-v 容器内路径:ro/rw 改变读写权限
# ro read only 只读
# rw read write 可读可写
# 一旦设定了这个容器的权限 容器对挂载出来的内容就有限定了
# ro 说明这个路径只能通过宿主机来改变，容器内部是无法操作的
```

## 初识DockerFile

DockerFile 就是用来构建docker镜像的文件，就是一段命令脚本。

通过这个脚本可以生成镜像，镜像是一层一层的。

> 方式二：

```shell
FROM centos

VOLUME ["volueme01","volume02"]

CMD echo "-------end volume success-------------"
CMD /bin/bash
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ docker-test-volume]# docker build --help

Usage:	docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is 'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN instructions during build (default "default")
      --no-cache                Do not use cache when building the image
  -o, --output stringArray      Output destination (format: type=local,dest=path)
      --platform string         Set platform if server is multi-platform capable
      --progress string         Set type of progress output (auto, plain, tty). Use plain to show container output (default "auto")
      --pull                    Always attempt to pull a newer version of the image
  -q, --quiet                   Suppress the build output and print image ID on success
      --rm                      Remove intermediate containers after a successful build (default true)
      --secret stringArray      Secret file to expose to the build (only if BuildKit enabled): id=mysecret,src=/local/secret
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
      --squash                  Squash newly built layers into a single new layer
      --ssh stringArray         SSH agent socket or keys to expose to the build (only if BuildKit enabled) (format: default|<id>[=<socket>|<key>[,<key>]])
      --stream                  Stream attaches to server to negotiate build context
  -t, --tag list                Name and optionally a tag in the 'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ docker-test-volume]# docker build -f ./dockerfile1 -t lwb/centos .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 0d120b6ccaa8
Step 2/4 : VOLUME ["volueme01","volume02"]
 ---> Running in 3824e5d2898b
Removing intermediate container 3824e5d2898b
 ---> 0478cb2167c9
Step 3/4 : CMD echo "-------end volume success-------------"
 ---> Running in 4d7c2eb09560
Removing intermediate container 4d7c2eb09560
 ---> 76a850ab78ed
Step 4/4 : CMD /bin/bash
 ---> Running in 043e49f85508
Removing intermediate container 043e49f85508
 ---> 19e9adbd4644
Successfully built 19e9adbd4644
Successfully tagged lwb/centos:latest
----------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ docker-test-volume]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
lwb/centos          latest              19e9adbd4644        38 seconds ago      215MB
----------------------------------------------------------------
# 启动自己构建的镜像 volueme01  volume02是生成镜像的时候自动挂载的,数据卷目录
# 这个卷一定是和外部有一个同步的目录的。匿名挂载
[root@iZ8vbdsaostzztcixyun1sZ docker-test-volume]# docker run -it 19e9adbd4644 /bin/bash
[root@4a1b092875dc /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volueme01  volume02
----------------------------------------------------------------
"Mounts": [
            {
                "Type": "volume",
                "Name": "cd7587e11891fc8f7981e2ede2a5c57db0036189f7e92ee6fad1b382fcf1128f",
                "Source": "/var/lib/docker/volumes/cd7587e11891fc8f7981e2ede2a5c57db0036189f7e92ee6fad1b382fcf1128f/_data",
                "Destination": "volueme01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "79d18e8c5eb5aeafa8e5ff99bad33fb98dbe59a327ae39392feb2a24c9c5070f",
                "Source": "/var/lib/docker/volumes/79d18e8c5eb5aeafa8e5ff99bad33fb98dbe59a327ae39392feb2a24c9c5070f/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
----------------------------------------------------------------
# 查看卷挂载内容
[root@iZ8vbdsaostzztcixyun1sZ _data]# pwd
/var/lib/docker/volumes/cd7587e11891fc8f7981e2ede2a5c57db0036189f7e92ee6fad1b382fcf1128f/_data
[root@iZ8vbdsaostzztcixyun1sZ _data]# ls
inner.txt

```

- 1.创建一个DockerFile文件，名字随意 建议使用DockerFile
- 2.文件中的内容指令(大写) 参数
- 3.每个命令就是docker的一层

这种方式是使用最多的，通常情况下我们会构建自己的镜像。

假设构建镜像时，没有挂载卷，需要手动挂载镜像 -v 卷名:容器内的路径

## 数据卷容器

多个MySQL同步数据

```shell
# 启动三个容器，通过我们刚才自己写的镜像启动
docker run -it --name cnet01 19e9adbd4644
docker run -it --name cent02 --volumes-from cent01 19e9adbd4644
docker run -it --name cent03 --volumes-from cent01 19e9adbd4644
# 三个容器之间数据共享
# cent02 cent03挂载到cent01上cent01是父容器
# 删除cent01，cent02，cent03的文件依然可以访问
# 备份机制
```

多个mysql数据共享

```shell
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=654321 --name mysql01 mysql:5.7
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=654321 --name mysql02 --volumes-from mysql01 mysql:5.7
```

- 结论：容器之间配置信息传递，数据卷的生命周期一直持续到容器用为止。
- 一旦持久化到本地，这个时候本地的数据是不会删除的。

# DockerFile

## DockerFile介绍

DockerFile是用来构建docker镜像的文件，命令参数文件。

构建步骤：

1.编写一个DockerFile文件

2.docker build 构建成一个镜像

3.docker run 运行镜像

4.docker push 发布镜像（发布到dockerhub、阿里云镜像仓库）

## DockerFile的构建过程

基础知识：

1.每个保留关键字（指令）必须是大写字母

2.执行必须是从上到下顺序执行

3.# 表示注释

4.每个指令都会创建提交一个新的镜像层

docker是面向开发的，我们以后要发布项目，做镜像，就需要编写 DockerFile，这个文件十分简单。

DockerFile：构建文件，定义了一切步骤

DockerImages：通过DockerFile构建生成的镜像，最终要发布和运行的产品。

Docker容器：容器就是镜像运行起来提供服务的

## DockerFile的指令

```shell
FROM # 基础镜像
MAINTAINER # 镜像是谁写的 姓名+邮箱
RUN # docker镜像需要运行的命令
ADD # 步骤：tomcat镜像，这个tomcat压缩包，添加内容
WORKDIR # 镜像的工作目录
VOLUME # 需要挂载到哪个位置
EXPOSE # 暴露端口配置 类似-p
CMD # 指定容器启动的时候要执行的命令，只有最后一个会生效，可被替代
ENTRYPOINT # 指定容器启动的时候要执行的命令，可以追加命令
ONBUILD # 当构建一个被继承的 DockerFile 这个时候就会运行ONBUILD的指令 触发指令
COPY # 类似ADD,将我们的文件拷贝到我们的镜像当中。
ENV # 构建的时候设置环境变量
```

## 实战测试 centos

DockerHub中绝大多数镜像都是从`FROM scratch`开始的

然后配置需要的软件来配置进行的构建。

> 创建一个centos

```dockerfile
# 编写DockerFile文件
FROM centos
MAINTAINER lwb<liwenbo4922@163.com>

ENV MYPATH /user/lcoal
WORKDIR $MYPATH

RUN yum -y install vim

RUN yum -y install net-tools

EXPOSE 8888

CMD echo $MYPATH

CMD echo "end---------"

CMD /bin/bash 
# 通过文件来构建自己的镜像
# 命令 docker build -f myDockerFileCentos -t mycentos:0.1 .
# Successfully built e5636cae8a5f
# Successfully tagged mycentos:0.1
# 测试运行
```

----------------

```shell
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run -it mycentos:0.1
[root@0466fabb141e lcoal]# ls
[root@0466fabb141e lcoal]# pwd
/user/lcoal
[root@0466fabb141e lcoal]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.4  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:04  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

可以列出本地的镜像变更历史

```shell
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker history e5636cae8a5f
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
e5636cae8a5f        6 minutes ago       /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B                  
e414338b9da1        6 minutes ago       /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B                  
6bdf6ad59f99        6 minutes ago       /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B                  
ca0508f38aa6        6 minutes ago       /bin/sh -c #(nop)  EXPOSE 8888                  0B                  
d6821df1ef36        6 minutes ago       /bin/sh -c yum -y install net-tools             22.8MB              
5c98d0b4730a        6 minutes ago       /bin/sh -c yum -y install vim                   57.3MB              
51ac3540f015        6 minutes ago       /bin/sh -c #(nop) WORKDIR /user/lcoal           0B                  
5695319584b8        6 minutes ago       /bin/sh -c #(nop)  ENV MYPATH=/user/lcoal       0B                  
5e95aa7c9baa        6 minutes ago       /bin/sh -c #(nop)  MAINTAINER lwb<liwenbo492…   0B                  
0d120b6ccaa8        3 months ago        /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B                  
<missing>           3 months ago        /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B                  
<missing>           3 months ago        /bin/sh -c #(nop) ADD file:538afc0c5c964ce0d…   215MB 
```

```shell
# 测试cmd命令
# 编写DockerFile
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# cat cmdTest 
FROM centos
CMD ["ls","-a"]
# 构建镜像
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker build -f cmdTest -t cmd-test .
Sending build context to Docker daemon  3.072kB
Step 1/2 : FROM centos
 ---> 0d120b6ccaa8
Step 2/2 : CMD ["ls","-a"]
 ---> Running in 71ee996183c8
Removing intermediate container 71ee996183c8
 ---> 7a540c569aba
Successfully built 7a540c569aba
Successfully tagged cmd-test:latest
# 执行镜像
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run 7a540c569aba
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
# 追加命令 -l ls-l
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run cmd-test -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\": executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 
# cmd的情况下 -l 替换了CMD["ls","-a"] 报ERROR
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run cmd-test ls -al
total 56
drwxr-xr-x   1 root root 4096 Dec  4 13:31 .
drwxr-xr-x   1 root root 4096 Dec  4 13:31 ..
-rwxr-xr-x   1 root root    0 Dec  4 13:31 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 Dec  4 13:31 dev
drwxr-xr-x   1 root root 4096 Dec  4 13:31 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64
drwx------   2 root root 4096 Aug  9 21:40 lost+found
drwxr-xr-x   2 root root 4096 May 11  2019 media
drwxr-xr-x   2 root root 4096 May 11  2019 mnt
drwxr-xr-x   2 root root 4096 May 11  2019 opt
dr-xr-xr-x 121 root root    0 Dec  4 13:31 proc
dr-xr-x---   2 root root 4096 Aug  9 21:40 root
drwxr-xr-x  11 root root 4096 Aug  9 21:40 run
lrwxrwxrwx   1 root root    8 May 11  2019 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 May 11  2019 srv
dr-xr-xr-x  13 root root    0 Dec  4 13:31 sys
drwxrwxrwt   7 root root 4096 Aug  9 21:40 tmp
drwxr-xr-x  12 root root 4096 Aug  9 21:40 usr
drwxr-xr-x  20 root root 4096 Aug  9 21:40 var
# 可以正常执行
```

```shell
# 测试ENTRYPOINT
# 编写
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# cat entrypointTest 
FROM centos
ENTRYPOINT ["ls","-a"]
# 构建
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker build -f entrypointTest -t en-test .
Sending build context to Docker daemon  4.096kB
Step 1/2 : FROM centos
 ---> 0d120b6ccaa8
Step 2/2 : ENTRYPOINT ["ls","-a"]
 ---> Running in c6475b65559d
Removing intermediate container c6475b65559d
 ---> 664c651c3a35
Successfully built 664c651c3a35
Successfully tagged en-test:latest
# 运行
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run en-test
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
# 拼接参数
[root@iZ8vbdsaostzztcixyun1sZ dockerfile]# docker run en-test -l
total 56
drwxr-xr-x   1 root root 4096 Dec  4 13:36 .
drwxr-xr-x   1 root root 4096 Dec  4 13:36 ..
-rwxr-xr-x   1 root root    0 Dec  4 13:36 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 Dec  4 13:36 dev
drwxr-xr-x   1 root root 4096 Dec  4 13:36 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64
drwx------   2 root root 4096 Aug  9 21:40 lost+found
drwxr-xr-x   2 root root 4096 May 11  2019 media
drwxr-xr-x   2 root root 4096 May 11  2019 mnt
drwxr-xr-x   2 root root 4096 May 11  2019 opt
dr-xr-xr-x 120 root root    0 Dec  4 13:36 proc
dr-xr-x---   2 root root 4096 Aug  9 21:40 root
drwxr-xr-x  11 root root 4096 Aug  9 21:40 run
lrwxrwxrwx   1 root root    8 May 11  2019 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 May 11  2019 srv
dr-xr-xr-x  13 root root    0 Dec  4 13:31 sys
drwxrwxrwt   7 root root 4096 Aug  9 21:40 tmp
drwxr-xr-x  12 root root 4096 Aug  9 21:40 usr
drwxr-xr-x  20 root root 4096 Aug  9 21:40 var

```

## 实战测试 tomcat

1.准备镜像文件 tomcat压缩包，JDK压缩包。

```shell
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# ll
total 200672
-rw-r--r-- 1 root root  11437266 Nov 12 23:53 apache-tomcat-9.0.40.tar.gz
-rw-r--r-- 1 root root 194042837 Dec 20  2018 jdk-8u202-linux-x64.tar.gz
```



2.编写DockerFile文件,官方命名`DockerFile`，build的时候会自动寻找这个文件，不需要手动指定了

```dockerfile
# [root@iZ8vbdsaostzztcixyun1sZ tomcat]# cat Dockerfile
FROM centos
MAINTAINER lwb<liwenbo4922@163.com>
COPY readme.txt /usr/local/readme.txt
ADD jdk-8u202-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.40.tar.gz /usr/local/
RUN yum -y install vim
ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_202
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.40
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.40
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080
CMD /usr/local/apache-tomcat-9.0.40/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.40/bin/logs/catalina.out 
```

3.构建镜像

`docker build -t diy-tomcat .`

4.启动镜像

`docker run -d -p 3356:8080 --name lwbtomcat -v /home/tomcat/test:/usr/local/apache-tomcat-9.0.40/webapps/test -v /home/tomcat/tomcalogs/:/usr/local/apache-tomcat-9.0.40/logs diy-tomcat`

备注这里一定要暴露和映射8080端口，否则需要手动修改tomcat的端口文件

创建文件夹WEB-INF

```shell
[root@iZ8vbdsaostzztcixyun1sZ test]# ls
index.html  WEB-INF
-------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ WEB-INF]# ls
web.xml
```



访问 `http://39.99.162.169:3356/test/`



## 发布自己的镜像

> Docker Hub

```shell
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker login --help

Usage:	docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
--------------------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker login -u 18737843824
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
--------------------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker tag --help

Usage:	docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
--------------------------------------------------------------------------
# docker tag
root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker tag 容器ID liwenbodocker/mytomcat
--------------------------------------------------------------------------
# docker push
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker push --help

Usage:	docker push [OPTIONS] NAME[:TAG]

Push an image or a repository to a registry

Options:
      --disable-content-trust   Skip image signing (default true)
--------------------------------------------------------------------------
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker push liwenbodocker/mytomcat:latest
The push refers to repository [docker.io/liwenbodocker/mytomcat]
3be17d77a93a: Mounted from library/tomcat 
a84354b89db3: Mounted from library/tomcat 
438ec47051c4: Mounted from library/tomcat 
94982bbe98d5: Mounted from library/tomcat 
39341dafb261: Mounted from library/tomcat 
4b9227ba273c: Mounted from library/tomcat 
712264374d24: Mounted from library/tomcat 
475b4eb79695: Mounted from library/tomcat 
f3be340a54b9: Mounted from library/tomcat 
114ca5b7280f: Mounted from library/tomcat 
latest: digest: sha256:4527a552568f7d706173d8065278cd1abaa7edce186a149a5a2de251e12e6c3c size: 2421

```

## 总结

![](http://cdn.liwenbo.cool/docker_total.png)

# Docker网络（铺垫）

## 理解docker0

清空所有环境

```shell
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker exec -it a47e273e6de2 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
88: eth0@if89: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
# linux 能不能ping同容器内部
```

> 原理

每启动一个docker容器，docker都会给docker容器分配一个ip。我们只要安装了docker，就会有一个网卡，docker0。桥接模式，使用的技术是evth-pair技术！

容器带来网卡都是成对出现的

evth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连。

正因为有这个特性，evth-pair充当一个桥梁，连接各种虚拟网络设备的

openstack, Docker容器之间的连接，ovs的连接，都是使用evth-pair技术

结论：tomcat01和tomcat02是共用的一个路由器，docker0

所有的容器不指定网络的情况下，都是 docker0路由的， docker-会给我们的容器分配一个默认的可用IP

> 小结

Docker使用的是 Linux的桥接，宿主机中是一个 Dokcer容器的网桥 docker0

Docker中的所有的网络接口都是虚拟的。虚拟的转发效率高！（内网传递文件！）
只要容器删除，对应网桥一对就没了！

## --link

>思考一个场景，我们编写了一个微服务， database url=ip:，项目不重启，数据库jp换掉了，我们希望可以处理这个问题，可以名字来进行访问容器？

```shell
docker exec -it tomcat02 ping tomcat01
ping:tomcato1:Name or service not known
docker run -d ---name tomcat03 --link tomcat02 tomcat
# 3 ping 2 ok
# 2 ping 3 fail
--link就是我们在 hosts配置中增加了一个 tomcat02的地址映射
```

我们现在玩 Docker已经不建议使用--link了！
自定义网络！不适用 docker0！
dockert0问题：他不支持容器名连接访问！

## 自定义网络如何操作

容器互联：

```shell
# 查看所有的docker网络
[root@iZ8vbdsaostzztcixyun1sZ tomcat]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
bd7748cf0e3c        bridge              bridge              local
8ebb64a04969        host                host                local
df1077e6f0e7        none                null                local
# 网络模式
# bridge 桥接
# none 不配置网络 默认
# host 和宿主机共享网络
# container 容器网络连通（用得少，局限性大)
docker run -d -P --name tomcato1 --net bridge tomcat
# docker0特点：默认，域名不能访问，--link可以打通连接
docker network create --driver bridge --subnet 192 168.0.0/16--gateway 192.168.0.1 mynet
# 启动两个容器连接到自定义网络
docker run -d-P--name tomcat-net-01 --net mynet tomcat
docker run -d-P--name tomcat-net-02 --net mynet tomcat
```

我们自定义的网络dcke都已经帮我们维护好了对应的关系，推荐我们平时这样使用网络！

## 网络连通

```shell
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker network --help

Usage:	docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
[root@iZ8vbdsaostzztcixyun1sZ ~]# docker network connect --help

Usage:	docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container
# 测试打通 tomcat01- mynet连通之后就是将 tomcat01放到了 mynet网络下
#一个容器两个ip地址！
#阿里云服务：公网ip 私网ip
```



```dockerfile
FROM java:8
COPY *.jar/app.jar
CMD ["--server.port=8080"]
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```