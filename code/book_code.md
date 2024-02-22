# Info
This is code and output from examples in the book.

# Get where I need to be locally:
`cd GitHub`

# Chapter 1: Before you begin
## Section 1.3: Creating your lab environment

```bash
λ docker version
Client:
 Cloud integration: v1.0.35+desktop.10
 Version:           25.0.2
 API version:       1.44
 Go version:        go1.21.6
 Git commit:        29cf629
 Built:             Thu Feb  1 00:18:45 2024
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.27.1 (136059)
 Engine:
  Version:          25.0.2
  API version:      1.44 (minimum version 1.24)
  Go version:       go1.21.6
  Git commit:       fce6e0c
  Built:            Thu Feb  1 00:23:21 2024
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.6.28
  GitCommit:        ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

λ docker-compose version
Docker Compose version v2.24.3-desktop.1
```

Ran `docker container rm -f $(docker container ls -aq)` after reviewing using the inner command + looking at Docker Desktop. Output:
```
0abff2d96d5b
87ddbfc97279
15ae1467eab8
74b602815d30
142849ea21dd
7bc71307599e
f7df8cdbabdc
b9d81252bcb5
80a34ab51304
8df4b6fde6e7
e9f0244c2690
ee45b9251b22
a99bae8ab43a
4b294480cb4a
bcd1ae0ac831
c19de38e21ed
5219650380e7
3e57f1ae10f4
fb8fe1a98f91
f507bca644f6
b7c2cb298daf
7b314a636e60
c0f40a430f72
b313048204d1
9f7ddd4edfb4
a96a7a47d3b3
127dd3936288
041942d8aaef
```
Also just reset the Kube cluster & ran [the cleanup from that book's repo](https://github.com/paulkaefer/kiamol_code/blob/main/code/cleanup.md).

# Chapter 2: Understanding Docker and running Hello World
## Section 2.1: Running Hello World in a container

```
λ docker container run diamol/ch02-hello-diamol
Unable to find image 'diamol/ch02-hello-diamol:latest' locally
latest: Pulling from diamol/ch02-hello-diamol
941f399634ec: Pull complete 
93931504196e: Pull complete 
d7b1f3678981: Pull complete 
Digest: sha256:c4f45e04025d10d14d7a96df2242753b925e5c175c3bea9112f93bf9c55d4474
Status: Downloaded newer image for diamol/ch02-hello-diamol:latest
---------------------
Hello from Chapter 2!
---------------------
My name is:
e66928f48980
---------------------
Im running on:
Linux 6.6.12-linuxkit aarch64
---------------------
My address is:
inet addr:<redacted> Bcast:<redacted> Mask:255.255.0.0
---------------------
```

Run again:
```
---------------------
Hello from Chapter 2!
---------------------
My name is:
be0202fbc034
---------------------
Im running on:
Linux 6.6.12-linuxkit aarch64
---------------------
My address is:
inet addr:<redacted> Bcast:<redacted> Mask:255.255.0.0
---------------------
```

The IP addresses stayed the same.

## Section 2.2: So what is a container?

## Section 2.3: Connecting to a container like a remote computer

```bash
docker container run --interactive --tty diamol/base
```
Interactive output:
```
Unable to find image 'diamol/base:latest' locally
latest: Pulling from diamol/base
941f399634ec: Already exists 
716aca3e500c: Pull complete 
Digest: sha256:787fe221a14f46b55e224ea0436aca77d345c3ded400aaf6cd40125e247f35c7
Status: Downloaded newer image for diamol/base:latest
/ # hostname
bb4ca7368a7d
/ # date
Wed Feb 21 17:19:40 UTC 2024
/ # 
...
/ # ping paulkaefer.com
PING paulkaefer.com (45.55.201.68): 56 data bytes
64 bytes from 45.55.201.68: seq=0 ttl=63 time=69.511 ms
64 bytes from 45.55.201.68: seq=1 ttl=63 time=66.832 ms
64 bytes from 45.55.201.68: seq=2 ttl=63 time=65.552 ms
64 bytes from 45.55.201.68: seq=3 ttl=63 time=65.872 ms
64 bytes from 45.55.201.68: seq=4 ttl=63 time=64.640 ms
64 bytes from 45.55.201.68: seq=5 ttl=63 time=64.975 ms
64 bytes from 45.55.201.68: seq=6 ttl=63 time=73.681 ms
^C
--- paulkaefer.com ping statistics ---
7 packets transmitted, 7 packets received, 0% packet loss
round-trip min/avg/max = 64.640/67.294/73.681 ms
```

```bash
λ docker container ls
CONTAINER ID   IMAGE         COMMAND     CREATED              STATUS              PORTS     NAMES
bb4ca7368a7d   diamol/base   "/bin/sh"   About a minute ago   Up About a minute             zealous_curie

λ docker container top bb
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                15377               15359               0                   17:19               ?                   00:00:00            /bin/sh
```

I ran `docker container logs`, which had interesting output. It did show the command history above + the below (including text to the input; buffer overflow?). My bash prompt might have issues.
```
^[[24;5R^[[24;5R^[[24;5R^[[24;5R^[[24;5R^[[24;5R^[[24;5R^[[24;5R^[[24;5R
Wed Feb 21 11:21:58
~
paulkaefer ~ λ 4;5R4;5R4;5R4;5R4;5R4;5R4;5R4;5R4;5R
```

```bash
λ docker container inspect bb
[
    {
        "Id": "bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f",
        "Created": "2024-02-21T17:19:20.775801008Z",
        "Path": "/bin/sh",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 15377,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-02-21T17:19:21.011966675Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:e65ed3e91e579a862befe0d866ffe1e3377d64245c9e9fd82fffaf9e3c961f49",
        "ResolvConfPath": "/var/lib/docker/containers/bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f/hostname",
        "HostsPath": "/var/lib/docker/containers/bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f/hosts",
        "LogPath": "/var/lib/docker/containers/bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f/bb4ca7368a7d4f2d4bbf54e60f8ec62f9440d51263e76cb7c6127840fe0cbe3f-json.log",
        "Name": "/zealous_curie",
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
            "ConsoleSize": [
                24,
                80
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
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
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
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
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
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
                "LowerDir": "/var/lib/docker/overlay2/6f24187df61842b971b710781646b3207f6b5f8ee91403bbd2cadb2e1b89ce1a-init/diff:/var/lib/docker/overlay2/e277e88cfdcdaebdd02dd62c6895317878989035b8bb4b4473a698cd1523a343/diff:/var/lib/docker/overlay2/b98b86a3f22ac5b8eea66b9839e5b6657501a5dbb4f478c2fec1e8885f4a4cc1/diff",
                "MergedDir": "/var/lib/docker/overlay2/6f24187df61842b971b710781646b3207f6b5f8ee91403bbd2cadb2e1b89ce1a/merged",
                "UpperDir": "/var/lib/docker/overlay2/6f24187df61842b971b710781646b3207f6b5f8ee91403bbd2cadb2e1b89ce1a/diff",
                "WorkDir": "/var/lib/docker/overlay2/6f24187df61842b971b710781646b3207f6b5f8ee91403bbd2cadb2e1b89ce1a/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "bb4ca7368a7d",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh"
            ],
            "Image": "diamol/base",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "739ec72efe5a05549f505f48e8f902ffdb99192ad34ae4ac07dc8f68d824ee7c",
            "SandboxKey": "/var/run/docker/netns/739ec72efe5a",
            "Ports": {},
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "4bbdb627b07eda724fa76e971f4a0a4cfd67891bbfeed29dcdb2ea1db48b955c",
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
                    "MacAddress": "02:42:ac:11:00:02",
                    "NetworkID": "f7c09f3a29a7af95bf3d3ff3d3ec658ea02fd992e495a85f674ab4ca6d5618c7",
                    "EndpointID": "4bbdb627b07eda724fa76e971f4a0a4cfd67891bbfeed29dcdb2ea1db48b955c",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DriverOpts": null,
                    "DNSNames": null
                }
            }
        }
    }
]
```

## Section 2.4: Hosting a website in a container
```bash
λ docker container ls --all
CONTAINER ID   IMAGE                      COMMAND                 CREATED          STATUS                      PORTS     NAMES
bb4ca7368a7d   diamol/base                "/bin/sh"               5 minutes ago    Exited (0) 19 seconds ago             zealous_curie
6f2c1e8ac1f8   diamol/ch02-hello-diamol   "/bin/sh -c ./cmd.sh"   11 minutes ago   Exited (0) 11 minutes ago             charming_meninsky
aa93dcb21074   diamol/ch02-hello-diamol   "/bin/sh -c ./cmd.sh"   11 minutes ago   Exited (0) 11 minutes ago             quirky_newton
be0202fbc034   diamol/ch02-hello-diamol   "/bin/sh -c ./cmd.sh"   12 minutes ago   Exited (0) 12 minutes ago             thirsty_albattani
e66928f48980   diamol/ch02-hello-diamol   "/bin/sh -c ./cmd.sh"   13 minutes ago   Exited (0) 13 minutes ago             romantic_hawking
```

```bash
λ docker container run --detach --publish 8088:80 diamol/ch02-hello-diamol-web
Unable to find image 'diamol/ch02-hello-diamol-web:latest' locally
latest: Pulling from diamol/ch02-hello-diamol-web
dce8679b510e: Pull complete 
c111552b26df: Pull complete 
ef4263ddbf41: Pull complete 
cac4170335ae: Pull complete 
0a26a833371b: Pull complete 
994988584430: Pull complete 
Digest: sha256:fe5a4c954fe2df5cadeea304ab632533f8ece9e31bd219ea22fdbf8b597571eb
Status: Downloaded newer image for diamol/ch02-hello-diamol-web:latest
d5e4da70f31d8490504781867f2c3db1bbf8070c6f59d01ec89ae13ecf4b6751
```

```bash
λ docker container ls
CONTAINER ID   IMAGE                          COMMAND              CREATED          STATUS          PORTS                  NAMES
d5e4da70f31d   diamol/ch02-hello-diamol-web   "httpd-foreground"   20 seconds ago   Up 19 seconds   0.0.0.0:8088->80/tcp   romantic_wescoff
```
It works! At `http://localhost:8088`, I see:

> Hello from Chapter 2!
> This is [Learn Docker in a Month of Lunches](https://www.manning.com/books/learn-docker-in-a-month-of-lunches).

Running `docker container stats`:
```
CONTAINER ID   NAME               CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O    PIDS
d5e4da70f31d   romantic_wescoff   0.00%     4.668MiB / 8.726GiB   0.05%     3.85kB / 1.97kB   0B / 4.1kB   82
```

Cleanup:
```bash
Wed Feb 21 11:30:37
~/GitHub/diamol/ch02
paulkaefer ~/GitHub/diamol/ch02 λ docker container rm d5e4da70f31d
Error response from daemon: cannot remove container "/romantic_wescoff": container is running: stop the container before removing or force remove

Wed Feb 21 11:30:40
~/GitHub/diamol/ch02
paulkaefer ~/GitHub/diamol/ch02 λ docker container rm --force d5e4da70f31d
d5e4da70f31d

λ docker container rm --force $(docker container ls --all --quiet)
bb4ca7368a7d
6f2c1e8ac1f8
aa93dcb21074
be0202fbc034
e66928f48980
```

## Section 2.5: Understanding how Docker runs containers

## Chapter 2 Lab
```
docker container run --detach --publish 8088:80 diamol/ch02-hello-diamol-web
```

My work:
```bash
λ docker container run --detach --publish 8088:80 diamol/ch02-hello-diamol-web
b02396891985eed71002f3cc504aeba05b3380d84ccf8b5215e1f47e32146de9
```
Ah, this didn't work because I changed the file locally, but this pulls from docker hub.

Following the README:
```bash
λ docker container exec b02  ls /usr/local/apache2/htdocs
index.html
λ docker container cp index.html b02:/usr/local/apache2/htdocs/index.html
Successfully copied 2.05kB to b02:/usr/local/apache2/htdocs/index.html
```

# Chapter 3: Building your own Docker images

## Section 3.1: Using a container image from Docker Hub
[Image](https://hub.docker.com/r/diamol/ch03-web-ping) that pings the author's blog:
```bash
docker image pull diamol/ch03-web-ping
```
Output:
```
Using default tag: latest
latest: Pulling from diamol/ch03-web-ping
0362ad1dd800: Pull complete 
b09a182c47e8: Pull complete 
39d61d2ed871: Pull complete 
b4e2115e274a: Pull complete 
f5cca017994f: Pull complete 
f504555623f6: Pull complete 
Digest: sha256:2f2dce710a7f287afc2d7bbd0d68d024bab5ee37a1f658cef46c64b1a69affd2
Status: Downloaded newer image for diamol/ch03-web-ping:latest
docker.io/diamol/ch03-web-ping:latest
```

```bash
docker container run -d --name web-ping diamol/ch03-web-ping
```
Output:
```
e81a0297a124fa7a8fd01c199b54d99b5a7738a98b59fe4219959170913cf11c
```

Running `docker container logs web-ping` shows that it is running the pings.

```bash
λ docker rm -f web-ping
web-ping
λ docker container run --env TARGET=google.com diamol/ch03-web-ping
** web-ping ** Pinging: google.com; method: HEAD; 3000ms intervals
Making request number: 1; at 1708613920449
...
```

## Section 3.2: Writing your first Dockerfile
Explained files in `~/GitHub/diamol/ch03/exercises/web-ping` (except for `Jenkinsfile`).

## Section 3.3: Building your own container image

```bash
docker image build --tag web-ping .
```
Output:
```
[+] Building 1.9s (9/9) FINISHED                           docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 190B                                       0.0s
 => [internal] load metadata for docker.io/diamol/node:latest              1.7s
 => [auth] diamol/node:pull token for registry-1.docker.io                 0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [1/3] FROM docker.io/diamol/node:latest@sha256:dfee522acebdfdd9964aa9  0.0s
 => => resolve docker.io/diamol/node:latest@sha256:dfee522acebdfdd9964aa9  0.0s
 => => sha256:dfee522acebdfdd9964aa9c88ebebd03a20b6dd5739 1.41kB / 1.41kB  0.0s
 => => sha256:6467efe6481aace0c317f144079c1a321b91375a828 1.16kB / 1.16kB  0.0s
 => => sha256:8e0eeb0a11b3a91cc1d91b5ef637edd153a64a3792e 5.66kB / 5.66kB  0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 881B                                          0.0s
 => [2/3] WORKDIR /web-ping                                                0.0s
 => [3/3] COPY app.js .                                                    0.0s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:fa972ea9b32ed16dcb9966ae48a05b8aa2e8ea62a0a65  0.0s
 => => naming to docker.io/library/web-ping                                0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/zg3g2t6nw6vjdazfm5oi29hwl
```
No “successfully built” and “successfully tagged” messages (mentioned in the text), but (1) it says `FINISHED` and (2) no errors.

```bash
λ docker image ls 'w*'
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
web-ping     latest    fa972ea9b32e   About a minute ago   75.5MB
```

Running it...
```bash
docker container run -e TARGET=docker.com -e INTERVAL=5000 web-ping
# works; the below will ping my website:
docker container run -e INTERVAL=5000 web-ping
```

## Section 3.4: Understanding Docker images and image layers
```bash
λ docker image history web-ping
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
fa972ea9b32e   4 minutes ago   CMD ["node" "/web-ping/app.js"]                 0B        buildkit.dockerfile.v0
<missing>      4 minutes ago   COPY app.js . # buildkit                        846B      buildkit.dockerfile.v0
<missing>      4 minutes ago   WORKDIR /web-ping                               0B        buildkit.dockerfile.v0
<missing>      4 minutes ago   ENV INTERVAL=3000                               0B        buildkit.dockerfile.v0
<missing>      4 minutes ago   ENV METHOD=HEAD                                 0B        buildkit.dockerfile.v0
<missing>      4 minutes ago   ENV TARGET=paulkaefer.com                       0B        buildkit.dockerfile.v0
<missing>      4 years ago     /bin/sh -c #(nop)  CMD ["node"]                 0B        
<missing>      4 years ago     /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      4 years ago     /bin/sh -c #(nop) COPY file:238737301d473041…   116B      
<missing>      4 years ago     /bin/sh -c apk add --no-cache --virtual .bui…   5.11MB    
<missing>      4 years ago     /bin/sh -c #(nop)  ENV YARN_VERSION=1.16.0      0B        
<missing>      4 years ago     /bin/sh -c addgroup -g 1000 node     && addu…   65.1MB    
<missing>      4 years ago     /bin/sh -c #(nop)  ENV NODE_VERSION=10.16.0     0B        
<missing>      4 years ago     /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      4 years ago     /bin/sh -c #(nop) ADD file:66f49017dd7ba2956…   5.29MB
```
The `238737301d473041…` is mentioned @ `ch03-web-ping` on Docker Hub.









