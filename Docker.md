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























































