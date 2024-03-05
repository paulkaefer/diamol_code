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

Trying on Windows:
```CMD
C:\Users\paulk\GitHub>docker version
Client:
 Cloud integration: v1.0.35+desktop.11
 Version:           25.0.3
 API version:       1.44
 Go version:        go1.21.6
 Git commit:        4debf41
 Built:             Tue Feb  6 21:13:02 2024
 OS/Arch:           windows/amd64
 Context:           default

Server: Docker Desktop 4.28.0 (139021)
 Engine:
  Version:          25.0.3
  API version:      1.44 (minimum version 1.24)
  Go version:       go1.21.6
  Git commit:       f417435
  Built:            Tue Feb  6 21:14:25 2024
  OS/Arch:          linux/amd64
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

...and on Windows:
```
C:\Users\paulk\GitHub\diamol>docker container run diamol/ch02-hello-diamol
Unable to find image 'diamol/ch02-hello-diamol:latest' locally
latest: Pulling from diamol/ch02-hello-diamol
31603596830f: Pull complete
93931504196e: Pull complete
d7b1f3678981: Pull complete
Digest: sha256:c4f45e04025d10d14d7a96df2242753b925e5c175c3bea9112f93bf9c55d4474
Status: Downloaded newer image for diamol/ch02-hello-diamol:latest
---------------------
Hello from Chapter 2!
---------------------
My name is:
94340ec4197a
---------------------
Im running on:
Linux 5.15.133.1-microsoft-standard-WSL2 x86_64
---------------------
My address is:
inet addr:172.17.0.2 Bcast:172.17.255.255 Mask:255.255.0.0

...

C:\Users\paulk\GitHub\diamol>docker container run diamol/ch02-hello-diamol
---------------------
Hello from Chapter 2!
---------------------
My name is:
967b94072646
---------------------
Im running on:
Linux 5.15.133.1-microsoft-standard-WSL2 x86_64
---------------------
My address is:
inet addr:172.17.0.2 Bcast:172.17.255.255 Mask:255.255.0.0
---------------------
```

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
Similar on Windows, thanks to WSL2.

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
Worked on PowerShell, but not cmd.exe.

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

```bash
λ docker image ls
REPOSITORY                                                       TAG                                                                          IMAGE ID       CREATED         SIZE
web-ping                                                         latest                                                                       fa972ea9b32e   2 hours ago     75.5MB
kiamol/ch21-kubeless-cli                                         latest                                                                       df1324156b4f   10 days ago     996MB
kiamol/ch20-todo-save-handler                                    latest                                                                       ce87fd0b26d4   10 days ago     216MB
kiamol/ch20-todo-list                                            latest                                                                       009df9cedb42   10 days ago     234MB
kiamol/ch21-kubeless-cli                                         <none>                                                                       c4054264eaed   2 weeks ago     996MB
kiamol/ch20-todo-save-handler                                    <none>                                                                       a343f3a9079e   2 weeks ago     216MB
kiamol/ch20-todo-list                                            <none>                                                                       dc7144c8aa94   2 weeks ago     234MB
kiamol/ch20-user-controller                                      latest                                                                       13d234589d5f   2 weeks ago     95.9MB
meta<redacted>                                                   latest                                                                       8c43........   2 weeks ago     934MB
docker/desktop-kubernetes                                        kubernetes-v1.29.1-cni-v1.4.0-critools-v1.29.0-cri-dockerd-v0.3.8-1-debian   e953cb83dd7e   4 weeks ago     422MB
registry.k8s.io/kube-apiserver                                   v1.29.1                                                                      f3b81ff188c6   5 weeks ago     123MB
registry.k8s.io/kube-proxy                                       v1.29.1                                                                      b125ba1d1878   5 weeks ago     85.4MB
registry.k8s.io/kube-controller-manager                          v1.29.1                                                                      8715bb0e3bc2   5 weeks ago     118MB
registry.k8s.io/kube-scheduler                                   v1.29.1                                                                      140ecfd0789f   5 weeks ago     58MB
ts_metadata_exports                                              latest                                                                       80b220c973a2   3 months ago    276MB
registry.k8s.io/etcd                                             3.5.10-0                                                                     79f8d13ae8b8   3 months ago    136MB
hubproxy.docker.internal:5555/docker/desktop-kubernetes          kubernetes-v1.28.2-cni-v1.3.0-critools-v1.28.0-cri-dockerd-v0.3.4-1-debian   19d261e62622   4 months ago    411MB
registry.k8s.io/kube-apiserver                                   v1.28.2                                                                      30bb499447fe   5 months ago    120MB
registry.k8s.io/kube-controller-manager                          v1.28.2                                                                      89d57b83c178   5 months ago    116MB
registry.k8s.io/kube-scheduler                                   v1.28.2                                                                      64fc40cee371   5 months ago    57.8MB
registry.k8s.io/kube-proxy                                       v1.28.2                                                                      7da62c127fc0   5 months ago    68.3MB
registry.k8s.io/coredns/coredns                                  v1.11.1                                                                      2437cf762177   6 months ago    57.4MB
hubproxy.docker.internal:5555/docker/desktop-kubernetes          kubernetes-v1.27.2-cni-v1.2.0-critools-v1.27.0-cri-dockerd-v0.3.2-1-debian   8ba658ef36dd   8 months ago    398MB
registry.k8s.io/kube-apiserver                                   v1.27.2                                                                      72c9df6be7f1   9 months ago    115MB
registry.k8s.io/kube-scheduler                                   v1.27.2                                                                      305d7ed1dae2   9 months ago    56.2MB
registry.k8s.io/kube-controller-manager                          v1.27.2                                                                      2ee705380c3c   9 months ago    107MB
registry.k8s.io/kube-proxy                                       v1.27.2                                                                      29921a084542   9 months ago    66.5MB
registry.k8s.io/etcd                                             3.5.9-0                                                                      9cdd6470f48c   9 months ago    181MB
docker/desktop-vpnkit-controller                                 dc331cb22850be0cdd97c84a9cfecaf44a1afb6e                                     3750dfec169f   9 months ago    35MB
hubproxy.docker.internal:5555/docker/desktop-kubernetes          kubernetes-v1.25.9-cni-v1.1.1-critools-v1.25.0-cri-dockerd-v0.2.6-1-debian   72db983f29ff   10 months ago   385MB
registry.k8s.io/kube-apiserver                                   v1.25.9                                                                      b781b2ba4d81   10 months ago   123MB
registry.k8s.io/kube-controller-manager                          v1.25.9                                                                      97b0bebd519d   10 months ago   113MB
registry.k8s.io/kube-proxy                                       v1.25.9                                                                      7d06ab3388e9   10 months ago   58.1MB
registry.k8s.io/kube-scheduler                                   v1.25.9                                                                      591d2f8e126e   10 months ago   49.4MB
registry.k8s.io/coredns/coredns                                  v1.10.1                                                                      97e04611ad43   12 months ago   51.4MB
registry.k8s.io/etcd                                             3.5.7-0                                                                      24bc64e91103   13 months ago   181MB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.25.4-cni-v1.1.1-critools-v1.25.0-cri-dockerd-v0.2.6-1-debian   54fd0a8aabc0   14 months ago   383MB
hubproxy.docker.internal:5555/docker/desktop-kubernetes          kubernetes-v1.25.4-cni-v1.1.1-critools-v1.25.0-cri-dockerd-v0.2.6-1-debian   54fd0a8aabc0   14 months ago   383MB
registry.k8s.io/etcd                                             3.5.6-0                                                                      ef2458028240   15 months ago   181MB
registry.k8s.io/kube-apiserver                                   v1.25.4                                                                      8e49cdf98f4d   15 months ago   123MB
registry.k8s.io/kube-scheduler                                   v1.25.4                                                                      0d46ff411d67   15 months ago   49.3MB
registry.k8s.io/kube-proxy                                       v1.25.4                                                                      754f6cdb0cbb   15 months ago   58MB
registry.k8s.io/kube-controller-manager                          v1.25.4                                                                      829662131775   15 months ago   113MB
registry.k8s.io/pause                                            3.9                                                                          829e9de338bd   16 months ago   514kB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.25.2-cni-v1.1.1-critools-v1.24.2-cri-dockerd-v0.2.5-1-debian   181339469f74   17 months ago   349MB
k8s.gcr.io/kube-apiserver                                        v1.25.2                                                                      0b1fb9b45fa3   17 months ago   123MB
k8s.gcr.io/kube-proxy                                            v1.25.2                                                                      68348500321c   17 months ago   58MB
k8s.gcr.io/kube-controller-manager                               v1.25.2                                                                      c6c296d44024   17 months ago   113MB
k8s.gcr.io/kube-scheduler                                        v1.25.2                                                                      873dc124ec69   17 months ago   49.3MB
kubernetesui/dashboard                                           v2.7.0                                                                       20b332c9a70d   17 months ago   244MB
registry.k8s.io/etcd                                             3.5.5-0                                                                      b9a1dfeddea9   17 months ago   179MB
kiamol/ch11-gogs                                                 latest                                                                       1785843863ee   17 months ago   103MB
kiamol/ch04-todo-list                                            latest                                                                       130d01894885   17 months ago   137MB
kiamol/ch03-numbers-web                                          latest                                                                       02ceb290312a   17 months ago   115MB
kiamol/ch03-numbers-api                                          latest                                                                       e1e7fbeb1eba   17 months ago   111MB
kiamol/ch02-whoami                                               latest                                                                       73906425ea05   18 months ago   111MB
kiamol/ch17-user-cert-generator                                  latest                                                                       3a29180d53fb   18 months ago   54.8MB
kiamol/ch17-todo-list-configurator                               latest                                                                       35649eefd455   18 months ago   53MB
kiamol/ch17-rbac-tools                                           latest                                                                       0778797b0091   18 months ago   98.6MB
kiamol/ch17-kube-explorer                                        latest                                                                       2b2e74ebe041   18 months ago   141MB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.25.0-cni-v1.1.1-critools-v1.24.2-cri-dockerd-v0.2.5-1-debian   bf50bf323fa3   18 months ago   349MB
k8s.gcr.io/kube-apiserver                                        v1.25.0                                                                      4a10a2c929d5   18 months ago   123MB
k8s.gcr.io/kube-controller-manager                               v1.25.0                                                                      35cb04d93081   18 months ago   113MB
k8s.gcr.io/kube-proxy                                            v1.25.0                                                                      f5e24e0ab5f8   18 months ago   58MB
k8s.gcr.io/kube-scheduler                                        v1.25.0                                                                      b933d73979ff   18 months ago   49.3MB
kiamol/ch03-numbers-web                                          v2                                                                           c294735f79f1   19 months ago   115MB
kiamol/ch15-cert-generator                                       latest                                                                       0a14ce81da98   20 months ago   53.6MB
kiamol/ch09-vweb                                                 v1                                                                           ec739aa9e4e2   20 months ago   22MB
kiamol/ch09-vweb                                                 v2                                                                           0d24f8a89765   20 months ago   22MB
kiamol/ch07-timecheck                                            latest                                                                       4bdd7b80676f   20 months ago   88.6MB
kiamol/ch07-simple-proxy                                         latest                                                                       76e0b841f2c0   20 months ago   92.9MB
kiamol/ch05-pi                                                   latest                                                                       0392193fe9c4   20 months ago   116MB
kiamol/ch02-hello-kiamol                                         latest                                                                       f648c1ff0b2b   20 months ago   22MB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.24.2-cni-v1.1.1-critools-v1.24.2-cri-dockerd-v0.2.1-1-debian   476af46fb131   20 months ago   353MB
k8s.gcr.io/pause                                                 3.8                                                                          4e42fb3c9d90   20 months ago   514kB
registry.k8s.io/pause                                            3.8                                                                          4e42fb3c9d90   20 months ago   514kB
k8s.gcr.io/kube-apiserver                                        v1.24.2                                                                      21a90cbceae7   20 months ago   126MB
k8s.gcr.io/kube-controller-manager                               v1.24.2                                                                      cbadad6da5e7   20 months ago   116MB
k8s.gcr.io/kube-proxy                                            v1.24.2                                                                      4f5059b7cbd8   20 months ago   106MB
k8s.gcr.io/kube-scheduler                                        v1.24.2                                                                      25034e073eab   20 months ago   50MB
k8s.gcr.io/etcd                                                  3.5.4-0                                                                      8e041a3b0ba8   21 months ago   179MB
kubernetesui/metrics-scraper                                     v1.0.8                                                                       a422e0e98235   21 months ago   42.3MB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.24.1-cni-v0.8.5-critools-v1.24.2-cri-dockerd-v0.2.1-1-debian   f0446f591051   21 months ago   350MB
k8s.gcr.io/coredns                                               v1.9.3                                                                       b19406328e70   21 months ago   47.7MB
registry.k8s.io/coredns/coredns                                  v1.9.3                                                                       b19406328e70   21 months ago   47.7MB
k8s.gcr.io/kube-apiserver                                        v1.24.1                                                                      7c5896a75862   21 months ago   126MB
k8s.gcr.io/kube-proxy                                            v1.24.1                                                                      fcbd620bbac0   21 months ago   106MB
k8s.gcr.io/kube-controller-manager                               v1.24.1                                                                      f61bbe9259d7   21 months ago   116MB
k8s.gcr.io/kube-scheduler                                        v1.24.1                                                                      000c19baf6bb   21 months ago   50MB
hubproxy.docker.internal:5000/docker/desktop-kubernetes          kubernetes-v1.24.0-cni-v0.8.5-critools-v1.17.0-cri-dockerd-v0.2.0-1-debian   44b0e7b8434b   21 months ago   337MB
k8s.gcr.io/kube-apiserver                                        v1.24.0                                                                      b62a103951f4   22 months ago   126MB
k8s.gcr.io/kube-controller-manager                               v1.24.0                                                                      59fad34d4fe0   22 months ago   116MB
k8s.gcr.io/kube-proxy                                            v1.24.0                                                                      66e1443684b0   22 months ago   106MB
k8s.gcr.io/kube-scheduler                                        v1.24.0                                                                      b81513b3bfb4   22 months ago   50MB
k8s.gcr.io/etcd                                                  3.5.3-0                                                                      a9a710bb96df   22 months ago   178MB
kiamol/ch14-image-gallery                                        latest                                                                       cedb4dededc9   22 months ago   17.5MB
kiamol/ch09-vweb                                                 v3                                                                           a69cef12396a   22 months ago   5.32MB
k8s.gcr.io/pause                                                 3.7                                                                          e5a475a03805   23 months ago   514kB
gcr.io/k8s-minikube/kicbase                                      v0.0.29                                                                      c3fce30d7a06   2 years ago     1.04GB
kiamol/ch14-image-of-the-day                                     latest                                                                       cc6eb70b3efb   2 years ago     236MB
kiamol/ch14-access-log                                           latest                                                                       b58587fa8448   2 years ago     128MB
kiamol/ch13-kibana                                               latest                                                                       d6414d8b6548   2 years ago     361MB
kiamol/ch13-elasticsearch                                        latest                                                                       68115ced5cc1   2 years ago     337MB
kiamol/ch10-web-ping                                             latest                                                                       ffae50e822a6   2 years ago     111MB
k8s.gcr.io/kube-apiserver                                        v1.22.5                                                                      66ba38aa55f7   2 years ago     119MB
k8s.gcr.io/kube-proxy                                            v1.22.5                                                                      d665e23ad35c   2 years ago     97.4MB
k8s.gcr.io/kube-controller-manager                               v1.22.5                                                                      fc9438491212   2 years ago     113MB
k8s.gcr.io/kube-scheduler                                        v1.22.5                                                                      ecf6859f4d39   2 years ago     49.3MB
fluent/fluent-bit                                                1.8.11                                                                       f7255a4c349b   2 years ago     166MB
k8s.gcr.io/coredns/coredns                                       v1.8.6                                                                       edaa71f2aee8   2 years ago     46.8MB
k8s.gcr.io/etcd                                                  3.5.0-0                                                                      2252d5eb703b   2 years ago     364MB
k8s.gcr.io/coredns/coredns                                       v1.8.4                                                                       6d3ffc2696ac   2 years ago     44.4MB
docker/desktop-vpnkit-controller                                 v2.0                                                                         2edf9c994f19   2 years ago     19.2MB
docker/desktop-storage-provisioner                               v2.0                                                                         c027a58fa0bb   2 years ago     39.8MB
nginx                                                            1.18-alpine                                                                  f4ab10c6f38e   2 years ago     20.5MB
k8s.gcr.io/pause                                                 3.5                                                                          f7ff3c404263   2 years ago     484kB
diamol/ch02-hello-diamol-web                                     latest                                                                       40ba417b2294   2 years ago     54.6MB
diamol/base                                                      latest                                                                       e65ed3e91e57   2 years ago     6.89MB
adminer                                                          4.7-standalone                                                               784bd3608a38   3 years ago     79.8MB
diamol/ch03-web-ping                                             latest                                                                       bfce5d697312   3 years ago     75.5MB
diamol/ch02-hello-diamol                                         latest                                                                       c65623b61864   3 years ago     5.3MB
kubeless/function-controller                                     v1.0.7                                                                       94e7c07e8bd3   3 years ago     85.3MB
postgres                                                         11.8-alpine                                                                  c1280c34ec99   3 years ago     154MB
quay.io/kubernetes-ingress-controller/nginx-ingress-controller   0.33.0                                                                       ab6277d9d07e   3 years ago     322MB
connecteverything/nats-operator                                  0.7.2                                                                        cbd9274b401d   3 years ago     44.3MB
nginx                                                            1.17-alpine                                                                  377eb1b97ee5   3 years ago     18.9MB
moby/buildkit                                                    v0.7.1                                                                       619d0bf5fa9f   3 years ago     78.6MB
chartmuseum/chartmuseum                                          v0.12.0                                                                      723ca60d48e9   3 years ago     63.5MB
postgres                                                         11.6-alpine                                                                  94d455984111   4 years ago     150MB
kubeless/http-trigger-controller                                 v1.0.1                                                                       d6f09f3299d9   4 years ago     83.4MB
kubeless/cronjob-trigger-controller                              v1.0.1                                                                       aec1dd30bb57   4 years ago     77.1MB
```

Cool that my `web-ping`/`diamol/ch03-web-ping` are using 75.5MB, roughly what the author's screenshot shows.

```bash
λ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          128       14        16.59GB   14.96GB (90%)
Containers      41        20        585.7kB   213B (0%)
Local Volumes   2         2         11.75MB   0B (0%)
Build Cache     52        0         59.74MB   59.74MB
```

Nice!

## Section 3.5: Optimizing Dockerfiles to use the image layer cache

```bash
cd ../web-ping-optimized
docker image build -t web-ping:v3 .
```
One observation is that I (still) don't see the seven steps as mentioned. Might be the newest Docker working differently than the version the author used.


## Chatper 3 Lab
1. pull `diamol/ch03-lab` from Docker Hub; actually just run the command in step 2, as it should pull as well.
```bash
docker container run diamol/ch03-lab
```
2. ssh into it to look at `/diamol/ch03.txt`
```bash
docker container run --interactive --tty diamol/ch03-lab
```
Output:
```
Unable to find image 'diamol/ch03-lab:latest' locally
latest: Pulling from diamol/ch03-lab
941f399634ec: Already exists 
716aca3e500c: Already exists 
d347025eebd3: Pull complete 
c55b6ee61343: Pull complete 
Digest: sha256:161fc42d4a6ea122e2534e884a7ef7c737433e4aa098ba0523816e63f39e05c2
Status: Downloaded newer image for diamol/ch03-lab:latest
/diamol #
```
3. edit a version of the file locally
```
/diamol # cat ch03.txt 
Lab solution, by: 
```
4. copy it in? or is there a way to just create an image...
```bash
docker container ls --all
docker container cp ch03.txt d9:/diamol/ch03.txt
```
Output:
```
Successfully copied 2.05kB to d9:/diamol/ch03.txt
```
`d9e51b3ae422` = `adoring_lalande`

```bash
docker exec -it adoring_lalande /bin/bash

docker exec adoring_lalande cat /diamol/ch03.txt
```

Both returned:
```
Error response from daemon: container d9e51b3ae4224ae21207ba842743beadf56d7d6ef30692539163e5e9fcc54b7c is not running
```

Followed the README:
```bash
λ docker container run -it --name ch03lab diamol/ch03-lab
/diamol # echo "Paul Kaefer" >> ch03.txt 
/diamol # cat ch03.txt 
Lab solution, by: Paul Kaefer
/diamol # exit
λ docker container commit ch03lab ch03-lab-soln
sha256:287288b5add451692ff4359c5bbbfe48d8058ee8ecf89c49899752d3defd831b

Fri Feb 23 11:32:17
~/GitHub/diamol/ch03/lab
paulkaefer ~/GitHub/diamol/ch03/lab λ docker container run ch03-lab-soln cat ch03.txt
Lab solution, by: Paul Kaefer
```

Then can interactively run using `docker container run -it ch03-lab-soln`.


# Chapter 4: Packaging applications from source code into Docker Images
Ran some cleanup first:
```bash
Tue Feb 27 10:14:46
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker container ls --all
CONTAINER ID   IMAGE                          COMMAND                  CREATED      STATUS                      PORTS                  NAMES
95c5bad54f01   ch03-lab-soln                  "/bin/sh"                3 days ago   Exited (0) 3 days ago                              eager_brown
7a71db381f47   ch03-lab-soln                  "cat ch03.txt"           3 days ago   Exited (0) 3 days ago                              pensive_jennings
dac17359b750   ch03-lab-soln                  "/bin/sh"                3 days ago   Exited (0) 3 days ago                              pedantic_curie
87dec210787e   ch03-lab-soln                  "cmd /s /c type ch03…"   3 days ago   Created                                            jolly_dubinsky
9dc4830ee653   ch03-lab-soln                  "cat ch03.txt"           3 days ago   Exited (0) 3 days ago                              goofy_roentgen
07e4d427fd38   diamol/ch03-lab                "/bin/sh"                3 days ago   Exited (0) 3 days ago                              ch03lab
d9e51b3ae422   diamol/ch03-lab                "/bin/sh"                4 days ago   Exited (0) 4 days ago                              adoring_lalande
8fdb767b8adb   diamol/ch03-lab                "/bin/sh"                4 days ago   Exited (0) 4 days ago                              nostalgic_ride
5f9b66b52776   diamol/ch03-lab                "/bin/sh"                4 days ago   Exited (0) 4 days ago                              recursing_davinci
ee36bca0aca7   web-ping                       "docker-entrypoint.s…"   5 days ago   Exited (0) 5 days ago                              competent_hoover
6ea71af0669e   web-ping                       "docker-entrypoint.s…"   5 days ago   Exited (0) 5 days ago                              gallant_lewin
f95c888fefce   diamol/ch03-web-ping           "docker-entrypoint.s…"   5 days ago   Exited (0) 5 days ago                              unruffled_chandrasekhar
8d0cc2462e1a   diamol/base                    "/bin/sh"                5 days ago   Exited (0) 5 days ago                              xenodochial_chandrasekhar
b02396891985   diamol/ch02-hello-diamol-web   "httpd-foreground"       5 days ago   Exited (255) 21 hours ago   0.0.0.0:8088->80/tcp   laughing_wu

Tue Feb 27 10:14:49
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker container rm -f 95c5bad54f01 7a71db381f47 dac17359b750  87dec210787e 9dc4830ee653 07e4d427fd38 d9e51b3ae422 8fdb767b8adb 5f9b66b52776 ee36bca0aca7 6ea71af0669e f95c888fefce b02396891985
95c5bad54f01
7a71db381f47
dac17359b750
87dec210787e
9dc4830ee653
07e4d427fd38
d9e51b3ae422
8fdb767b8adb
5f9b66b52776
ee36bca0aca7
6ea71af0669e
f95c888fefce
b02396891985

Tue Feb 27 10:15:32
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker container ls --all
CONTAINER ID   IMAGE         COMMAND     CREATED      STATUS                  PORTS     NAMES
8d0cc2462e1a   diamol/base   "/bin/sh"   5 days ago   Exited (0) 5 days ago             xenodochial_chandrasekhar

Tue Feb 27 10:16:20
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker image ls -f reference='diamol/*'
REPOSITORY                     TAG       IMAGE ID       CREATED       SIZE
diamol/ch02-hello-diamol-web   latest    40ba417b2294   2 years ago   54.6MB
diamol/ch03-lab                latest    8a8853903859   2 years ago   6.89MB
diamol/base                    latest    e65ed3e91e57   2 years ago   6.89MB
diamol/ch03-web-ping           latest    bfce5d697312   3 years ago   75.5MB
diamol/ch02-hello-diamol       latest    c65623b61864   3 years ago   5.3MB

Tue Feb 27 10:16:23
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker image rm -f  40ba417b2294 8a8853903859 bfce5d697312 c65623b61864
Untagged: diamol/ch02-hello-diamol-web:latest
Untagged: diamol/ch02-hello-diamol-web@sha256:fe5a4c954fe2df5cadeea304ab632533f8ece9e31bd219ea22fdbf8b597571eb
Deleted: sha256:40ba417b2294a9756f77ac6a63bcdef1edaa0afb8849f7d760b41ceb8b929e73
Deleted: sha256:313d76222fce936fe658c078838b39a9d761cf3c43d023dd1e1627f1f953bdc5
Deleted: sha256:da38f4a8ac9d5262ad1678a3965e5bf510e60bb4180bb870d4ebc0414a8f7918
Deleted: sha256:ca09e7e93505470ddbc92bb1bd39e543c680818e1e19834c25c07bc3b0907b64
Deleted: sha256:84ef683740a51e1cf432cd9962fb90ff6e23d54077efe1574557d17516997585
Deleted: sha256:208fd3acdfad4e4e79ca97f80725f757d239dcf3e6ac1a4f356e68bbea196c2c
Deleted: sha256:02a88cfff88f8556682dab1a86e2722e894583ee4bfc3096c1310dbbcfb1e959
Untagged: diamol/ch03-web-ping:latest
Untagged: diamol/ch03-web-ping@sha256:2f2dce710a7f287afc2d7bbd0d68d024bab5ee37a1f658cef46c64b1a69affd2
Deleted: sha256:bfce5d697312823efb7296a50273b5cdcc75fea7c2be44cd0461f9b8a28ccf8c
Deleted: sha256:12d9ba838c6db6a0536092ce4328360988caa791fd7a7ca9f165c5f5fbdf0c43
Deleted: sha256:8298d7b08b55b96f07033064ded4c246abd6112b8ec983bc2e50045958c18184
Untagged: diamol/ch02-hello-diamol:latest
Untagged: diamol/ch02-hello-diamol@sha256:c4f45e04025d10d14d7a96df2242753b925e5c175c3bea9112f93bf9c55d4474
Deleted: sha256:c65623b61864596c264340e2123e205139ebafd52694afd24fed35c226ad1daf
Deleted: sha256:5467c0f933c7741ed2d345c50462b3699253bccd19b9bb0ba10514a1efabed72
Deleted: sha256:19fe9b91aa30a4b437a57ce4a7a52b4cd98ee6f283e0db3b0b204b7f2fd10779
Error response from daemon: conflict: unable to delete 8a8853903859 (cannot be forced) - image has dependent child images

Tue Feb 27 10:16:39
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker image ls -f reference='diamol/*'
REPOSITORY        TAG       IMAGE ID       CREATED       SIZE
diamol/ch03-lab   latest    8a8853903859   2 years ago   6.89MB
diamol/base       latest    e65ed3e91e57   2 years ago   6.89MB

Tue Feb 27 10:16:44
~/GitHub/diamol/ch04
paulkaefer ~/GitHub/diamol/ch04 λ docker image rm -f  8a8853903859
Error response from daemon: conflict: unable to delete 8a8853903859 (cannot be forced) - image has dependent child images
```
Interesting! I would have thought the lab was depended on the base, not the other way around.
We dug into this; the lab <ins>solution</ins> is what is dependent on `diamol/ch03-lab`.

## Section 4.1: Who needs a build server when you have a Dockerfile?

```bash
λ pwd
/Users/paulkaefer/GitHub/diamol/ch04/exercises/multi-stage
λ docker image build -t multi-stage .
[+] Building 0.5s (9/9) FINISHED                                                                                              docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                          0.0s
 => => transferring dockerfile: 302B                                                                                                          0.0s
 => [internal] load metadata for docker.io/diamol/base:latest                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                             0.0s
 => => transferring context: 2B                                                                                                               0.0s
 => [build-stage 1/2] FROM docker.io/diamol/base:latest                                                                                       0.0s
 => [build-stage 2/2] RUN echo 'Building...' > /build.txt                                                                                     0.3s
 => [test-stage 2/3] COPY --from=build-stage /build.txt /build.txt                                                                            0.0s
 => [test-stage 3/3] RUN echo 'Testing...' >> /build.txt                                                                                      0.1s
 => [stage-2 2/2] COPY --from=test-stage /build.txt /build.txt                                                                                0.0s
 => exporting to image                                                                                                                        0.0s
 => => exporting layers                                                                                                                       0.0s
 => => writing image sha256:41746b5df1a680bfa4ed7e01cb1d38ad5713bc2f8e3ba97fe94981f8aa38ee9b                                                  0.0s
 => => naming to docker.io/library/multi-stage                                                                                                0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/0vg25noz7tjc4cr9ogtybr076
```
Neat; opens in Docker Desktop:
![](./attachments/ch04/Screenshot_2024-02-27_1043AM.png)

## Section 4.2: App walkthrough: Java source code

```bash
cd ch04/exercises/image-of-the-day
docker image build -t image-of-the-day .
```
There are errors:
```
1.047 [ERROR] [ERROR] Some problems were encountered while processing the POMs:
1.047 [FATAL] Non-resolvable parent POM for com.sixeyed.diamol:iotd-service:0.1.0: Could not transfer artifact org.springframework.boot:spring-boot-starter-parent:pom:2.1.3.RELEASE from/to central (https://repo.maven.apache.org/maven2): Transfer failed for https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.1.3.RELEASE/spring-boot-starter-parent-2.1.3.RELEASE.pom and 'parent.relativePath' points at wrong local POM @ line 10, column 13
```
Not on my personal computer, though!
```
[+] Building 105.1s (17/17) FINISHED                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.1s
...
 => => naming to docker.io/library/image-of-the-day                        0.0s
```

```bash
λ docker network create nat
0ea2cdff209e987d4a106a49c36e1a024ab5b758abf21e746f66bf3bbc270ac1
```

```bash
docker container run --name iotd -d -p 800:80 --network nat image-of-the-day
```

## Section 4.3: App walkthrough: Node.js source code
```bash
cd ch04/exercises/access-log
docker image build -t access-log .
```
The output includes:
```
1.232 npm ERR! code SELF_SIGNED_CERT_IN_CHAIN
```

```bash
λ docker container run --name accesslog -d -p 801:80 --network nat access-log
```
Output:
```
b296e92a9fa1abe9042d31ca67f458e71d52248b4de2f7ef05e980935ed2bdcd
```
And `http://localhost:801/stats` shows:
```
{"logs":0}
```

Ooh, `http://localhost:800/image` also shows:
```
{"url":"https://apod.nasa.gov/apod/image/2402/Simeis147_Vetter_960.jpg","caption":"Supernova Remnant Simeis 147","copyright":"\nStéphane Vetter\n(Nuits sacrées)\n"}
```


## Section 4.4: App walkthrough: Go source code

```bash
λ cd ch04/exercises/image-gallery
λ docker image build -t image-gallery .
λ docker image ls -f reference=diamol/golang -f reference=image-gallery
# work:
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
image-gallery   latest    2dc75a11cdfa   13 seconds ago   26.2MB

# personal:
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
image-gallery   latest    bf5865de7158   6 seconds ago   27.1MB
```
Hmm, no sign of `diamol/golang` on either. Ran `docker pull diamol/golang` on both devices. Personal computer now shows:
```
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
image-gallery   latest    bf5865de7158   2 minutes ago   27.1MB
diamol/golang   latest    119cb20c3f56   3 years ago     803MB
```
Interesting! Just 708MB on my work computer.

```bash
λ docker container ls
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                 NAMES
f61c113302d1   image-of-the-day   "java -jar /app/iotd…"   18 minutes ago   Up 17 minutes   0.0.0.0:800->80/tcp   iotd
b296e92a9fa1   access-log         "docker-entrypoint.s…"   20 minutes ago   Up 20 minutes   0.0.0.0:801->80/tcp   accesslog
λ docker container run -d -p 802:80 --network nat image-gallery
3cec79c380b193e23b1ba82f9975e3b5467d4b8c573dce3e7858b0d737c9bbbb
```
It works, is beautiful (screenshot below), and I now see `{"logs":2}` at `http://localhost:801/stats`.
![](./attachments/ch04/Section_4.4_Go_app_running.png)

On Windows:
```PowerShell
PS> docker image build -t image-gallery .
[+] Building 63.5s (15/15) FINISHED
...
PS> docker network create nat
d9334b040d2c100cc2a120aec343efaa1d6d7658c9c61873e33a7cdea564b1fe
PS> cd ../access-log/
PS> docker image build -t access-log .
[+] Building 13.5s (13/13) FINISHED
...
PS> docker container run --name accesslog -d -p 801:80 --network nat access-log
b6f17414b515b7513f8ab279b6b96e3efdcdae3661ef17690eab48489de408f1
PS> docker container run -d -p 802:80 --network nat image-gallery
b09803357b82d3a01e968d50a2d08d4998f8358ff2d27bdac5e3239a0da9cbc2
```
Hmm, timed out. `http://localhost:801/` shows `{"code":"ResourceNotFound","message":"/ does not exist"}`.
Will continue in Chapter 5 anyway...

## Section 4.5: Understanding multi-stage Dockerfiles

## Lab
Pressed for time, so I just compared the Dockerfiles; they make sense. The one question is why the `EXPOSE`/`CMD`/`ENV` command order is swapped in the author's solution.
```bash
λ docker container run
 -d -p 804:80 ch04-lab
8f5e1d051c08db11c9e3d4a59176675a7884a3e7d27f5273c2f6d93474ce07c8

λ docker image ls
REPOSITORY                                                TAG                                                                          IMAGE ID       CREATED              SIZE
ch04-lab                                                  latest                                                                       b38da5696cc6   About a minute ago   832MB
...

λ docker image build -t ch04-lab:optimized -f Dockerfile.optimized .

λ docker container run
 -d -p 805:80 ch04-lab:optimized
b19244747d034ce093f22e1de07f7b8f75122b8891bf763e5ea4a5ed27ec65b6

λ docker image ls
REPOSITORY                                                TAG                                                                          IMAGE ID       CREATED          SIZE
ch04-lab                                                  optimized                                                                    5d215edf99c2   40 seconds ago   17MB
ch04-lab                                                  latest                                                                       b38da5696cc6   4 minutes ago    832MB
...
```
Note: may have also created one with hash `7502731ea2223f1da7107e5ae764a8525acc7d101a1a851d0ac66a9bf65d39b9` on my Mac.


## Section 5.2: Pushing your own images to Docker Hub
```
export dockerId="paulcarrot"
docker login --username $dockerId
```
It worked. It also said:
> For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/

On Windows:
```PowerShell
PS C:\Users\paulk>  $dockerId="paulcarrot"
PS C:\Users\paulk> docker login --username $dockerId
Password:

Login Succeeded
```

```bash
λ docker image tag image-gallery $dockerId/image-gallery:v1
λ docker image ls --filter refe
rence=image-gallery --filter reference='*/image-gallery'
REPOSITORY                 TAG       IMAGE ID       CREATED        SIZE
image-gallery              latest    bf5865de7158   45 hours ago   27.1MB
paulcarrot/image-gallery   v1        bf5865de7158   45 hours ago   27.1MB
λ docker image push $dockerId/image-gallery:v1
The push refers to repository [docker.io/paulcarrot/image-gallery]
179a37ef0366: Pushed 
1fde46b75bd0: Pushed 
8b51bcbf75f0: Pushed 
04b6a91e2203: Pushed 
f87269b94a71: Mounted from diamol/base 
89ae5c4ee501: Mounted from diamol/base 
v1: digest: sha256:5df838c969e42468350ca084d673a61889bb69fd895f59bfc3ed48291c7b5a52 size: 1573
```

Windows:
```PowerShell
PS> docker image tag image-gallery $dockerId/image-gallery:v2
PS> docker image ls --filter reference=image-gallery --filter reference='*/image-gallery'
REPOSITORY                 TAG       IMAGE ID       CREATED         SIZE
image-gallery              latest    f65c2609fbd9   7 minutes ago   27.1MB
paulcarrot/image-gallery   v2        f65c2609fbd9   7 minutes ago   27.1MB
PS> docker image push $dockerId/image-gallery:v2
The push refers to repository [docker.io/paulcarrot/image-gallery]
74088eb05a12: Pushed
f37472f6baef: Pushed
83ecb1a17f5a: Pushed
00717ff0a783: Pushed
f87269b94a71: Layer already exists
89ae5c4ee501: Layer already exists
v2: digest: sha256:73020a683ec1f724c133f36862650a44532b090976639aff3dea928b94474115 size: 1573
```
I do see `v2` at https://hub.docker.com/repository/docker/paulcarrot/image-gallery/general.

On Mac, running `echo "https://hub.docker.com/r/$dockerId/image-gallery/tags"` outputs `https://hub.docker.com/r/paulcarrot/image-gallery/tags`.

## Section 5.3: Running and using your own Docker registry

```bash
# run the registry with a restart flag so the container gets
# restarted whenever you restart Docker:
docker container run -d -p 5001:5000 --restart always diamol/registry
```
Output:
```
# with 5000:5000 from the book:
Unable to find image 'diamol/registry:latest' locally
latest: Pulling from diamol/registry
31603596830f: Already exists 
792f5419a843: Already exists 
3fec9ac2e0fe: Pull complete 
aea0fab1b866: Pull complete 
b72408f4bc4f: Pull complete 
6a2aeb3b52c0: Pull complete 
Digest: sha256:49c5a928c870d496013d98d4357c93f35b1896a0385ef275664bc43e69b14950
Status: Downloaded newer image for diamol/registry:latest
20f8f49b7ee1e0a2b6d8b3915caf6382aa330a185ee721e387334757cce2f736
docker: Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:5000 -> 0.0.0.0:0: listen tcp 0.0.0.0:5000: bind: address already in use.

# tried again with 5001:5000:
Unable to find image 'diamol/registry:latest' locally
latest: Pulling from diamol/registry
31603596830f: Already exists 
792f5419a843: Already exists 
3fec9ac2e0fe: Pull complete 
aea0fab1b866: Pull complete 
b72408f4bc4f: Pull complete 
6a2aeb3b52c0: Pull complete 
Digest: sha256:49c5a928c870d496013d98d4357c93f35b1896a0385ef275664bc43e69b14950
Status: Downloaded newer image for diamol/registry:latest
10040b0d7b1e7001999eb73fd86d6b4abd8e685a06a90fb1653d684fdfd961c0
```

Similar on Windows:
```PowerShell
PS> docker container run -d -p 5000:5000 --restart always diamol/registry
Unable to find image 'diamol/registry:latest' locally
latest: Pulling from diamol/registry
31603596830f: Already exists
792f5419a843: Already exists
3fec9ac2e0fe: Pull complete
aea0fab1b866: Pull complete
b72408f4bc4f: Pull complete
6a2aeb3b52c0: Pull complete
Digest: sha256:49c5a928c870d496013d98d4357c93f35b1896a0385ef275664bc43e69b14950
Status: Downloaded newer image for diamol/registry:latest
5be9ae0782288e85136f5ea85e18358c6ff788479e26e9d9d9885cd37e136764
```

```bash
λ echo $'\n127.0.0.1 registry.local' | sudo tee -a /etc/hosts

127.0.0.1 registry.local

λ ping registry.local
PING registry.local (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.050 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.086 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.100 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.080 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.096 ms
^C
--- registry.local ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.050/0.082/0.100/0.018 ms
```

On Windows, I ran `Add-Content -Value "127.0.0.1 registry.local" -Path /windows/system32/drivers/etc/hosts` in an Admin PS window.
```PowerShell
PS> ping registry.local

Pinging registry.local [127.0.0.1] with 32 bytes of data:
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128

Ping statistics for 127.0.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
PS> docker image tag image-gallery registry.local:5000/gallery/ui:v1
```
Might have messed up the insecure registry piece. Resetting Docker Desktop to factory defaults...

Attempting to use the registry on Mac:
```bash
docker image tag image-gallery registry.local:5001/gallery/ui:v1
```

Followed the steps to update my Docker client settings.
```bash
λ docker image push registry.local:5001/gallery/ui:v1
The push refers to repository [registry.local:5001/gallery/ui]
An image does not exist locally with the tag: registry.local:5001/gallery/ui
```

On Windows:
```PowerShell
PS> docker image tag image-gallery registry.local:5000/gallery/ui:v1
PS> docker image push registry.local:5000/gallery/ui:v1
The push refers to repository [registry.local:5000/gallery/ui]
a26d56b97c91: Pushed
73bad7eebeba: Pushed
223d79037852: Pushed
571617631403: Pushed
f87269b94a71: Pushed
89ae5c4ee501: Pushed
v1: digest: sha256:a995c6888bb77475502e69c439165cc38b2cdad3a268a8275aa290ae284e8dcb size: 1573
```

## Section 5.4: Using image tags effectively
These ran with no output:
```bash
docker image tag image-gallery registry.local:5001/gallery/ui:latest
docker image tag image-gallery registry.local:5001/gallery/ui:2
docker image tag image-gallery registry.local:5001/gallery/ui:2.1
docker image tag image-gallery registry.local:5001/gallery/ui:2.1.106
```

On Windows:
```PowerShell
docker image tag image-gallery registry.local:5000/gallery/ui:latest
docker image tag image-gallery registry.local:5000/gallery/ui:2
docker image tag image-gallery registry.local:5000/gallery/ui:2.1
docker image tag image-gallery registry.local:5000/gallery/ui:2.1.106
```

Testing:
```bash
λ docker pull registry.local:5001/gallery/ui:2.1.106
Error response from daemon: manifest for registry.local:5001/gallery/ui:2.1.106 not found: manifest unknown: manifest unknown
```

On Windows:
Docker config:
```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "insecure-registries": ["registry.local:5000"],
  "experimental": false
}
```
Results:
```PowerShell
PS> docker push registry.local:5000/gallery/ui:2.1.106
The push refers to repository [registry.local:5000/gallery/ui]
An image does not exist locally with the tag: registry.local:5000/gallery/ui
PS> docker pull registry.local:5000/gallery/ui:2.1.106
Error response from daemon: manifest for registry.local:5000/gallery/ui:2.1.106 not found: manifest unknown: manifest unknown
```
After re-doing some of the steps above (I may have used `:5001` the first time), I see:
```PowerShell
PS> docker pull registry.local:5000/gallery/ui
Using default tag: latest
Error response from daemon: manifest for registry.local:5000/gallery/ui:latest not found: manifest unknown: manifest unknown
PS> docker images
REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
image-gallery                    latest    ea959d038ba6   17 minutes ago   27.1MB
registry.local:5000/gallery/ui   2         ea959d038ba6   17 minutes ago   27.1MB
registry.local:5000/gallery/ui   2.1       ea959d038ba6   17 minutes ago   27.1MB
registry.local:5000/gallery/ui   2.1.106   ea959d038ba6   17 minutes ago   27.1MB
registry.local:5000/gallery/ui   latest    ea959d038ba6   17 minutes ago   27.1MB
registry.local:5000/gallery/ui   v1        ea959d038ba6   17 minutes ago   27.1MB
registry.local:5001/gallery/ui   2         ea959d038ba6   17 minutes ago   27.1MB
registry.local:5001/gallery/ui   2.1       ea959d038ba6   17 minutes ago   27.1MB
registry.local:5001/gallery/ui   2.1.106   ea959d038ba6   17 minutes ago   27.1MB
registry.local:5001/gallery/ui   latest    ea959d038ba6   17 minutes ago   27.1MB
registry.local:5001/gallery/ui   v1        ea959d038ba6   17 minutes ago   27.1MB
access-log                       latest    52abfeb47ac4   18 minutes ago   92.2MB
diamol/registry                  latest    1b466d248ca0   3 years ago      27.4MB
```

## Section 5.5: Turning official images into golden images

```bash
cd ch05/exercises/dotnet-sdk
docker image build -t golden/dotnetcore-sdk:3.0 .

cd ../aspnet-runtime
docker image build -t golden/aspnet-core:3.0 .
```
Output:
```
[+] Building 78.1s (8/8) FINISHED                          docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.1s
 => => transferring dockerfile: 249B                                       0.0s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for mcr.microsoft.com/dotnet/core/sdk:3.0.10  1.3s
 => [1/3] FROM mcr.microsoft.com/dotnet/core/sdk:3.0.100@sha256:ee3376d5  75.1s
 => => resolve mcr.microsoft.com/dotnet/core/sdk:3.0.100@sha256:ee3376d5f  0.0s
 => => sha256:b7a128769df1909f91b589d0a4a2e1c1671aebc047a 7.81MB / 7.81MB  2.3s
 => => sha256:1128949d0793d2435bb1f0640a777f32feee88b71 10.00MB / 10.00MB  2.2s
 => => sha256:c7b7d16361e00faca0e9393f3f43923f25ceb121 50.38MB / 50.38MB  13.0s
 => => sha256:65c0187e27b42bc45ea5ef98226c7ef37d1b706e100 2.01kB / 2.01kB  0.0s
 => => sha256:170a7f2ec51a5cd1f6e68a3142ebb78f92949543f4d 6.54kB / 6.54kB  0.0s
 => => sha256:ee3376d5f94027cb649dba88e3caefda8363623b575 2.88kB / 2.88kB  0.0s
 => => sha256:667692510b7038b74e221f92eb33610e4968b669 51.77MB / 51.77MB  22.6s
 => => sha256:efefb401e9dccd3e703e80b80fb9bbd3847f66c6d 13.70MB / 13.70MB  4.2s
 => => sha256:9deb21478e67b8ff1256de3e2639cefcab485f 116.09MB / 116.09MB  68.0s
 => => sha256:460e7bb4980298c8d51ffc08a56e3287127742d453 6.31kB / 6.31kB  13.3s
 => => extracting sha256:c7b7d16361e00faca0e9393f3f43923f25ceb1210face878  5.3s
 => => sha256:475935cb0b6d0d3f088b6637b4685a6c18050de4 12.40MB / 12.40MB  20.0s
 => => extracting sha256:b7a128769df1909f91b589d0a4a2e1c1671aebc047a9f46b  0.7s
 => => extracting sha256:1128949d0793d2435bb1f0640a777f32feee88b71d4fe234  0.7s
 => => extracting sha256:667692510b7038b74e221f92eb33610e4968b669c8a71837  5.4s
 => => extracting sha256:efefb401e9dccd3e703e80b80fb9bbd3847f66c6d7c64d51  0.3s
 => => extracting sha256:9deb21478e67b8ff1256de3e2639cefcab485f4fd5e25355  6.1s
 => => extracting sha256:460e7bb4980298c8d51ffc08a56e3287127742d4536b3516  0.0s
 => => extracting sha256:475935cb0b6d0d3f088b6637b4685a6c18050de45a68c3e9  0.7s
 => [internal] load build context                                          0.0s
 => => transferring context: 107B                                          0.0s
 => [2/3] WORKDIR src                                                      1.4s
 => [3/3] COPY global.json .                                               0.0s
 => exporting to image                                                     0.1s
 => => exporting layers                                                    0.1s
 => => writing image sha256:788f703eea82f1de38ed3f41b5f964fa148d8979aedf8  0.0s
 => => naming to docker.io/golden/dotnetcore-sdk:3.0                       0.0s

[+] Building 18.8s (5/5) FINISHED                          docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.1s
 => => transferring dockerfile: 230B                                       0.0s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for mcr.microsoft.com/dotnet/core/aspnet:3.0  0.6s
 => [1/1] FROM mcr.microsoft.com/dotnet/core/aspnet:3.0@sha256:559b6a8ef  17.9s
 => => resolve mcr.microsoft.com/dotnet/core/aspnet:3.0@sha256:559b6a8efa  0.0s
 => => sha256:579be85d9bf6def816023c880770083e9c15120a0fc 4.77kB / 4.77kB  0.0s
 => => sha256:68ced04f60ab5c7a5f1d0b0b4e7572c5a4c8cce44 27.09MB / 27.09MB  9.9s
 => => sha256:e936bd534ffb9280632b9c0c87e0f5ab3031e53da 17.06MB / 17.06MB  7.6s
 => => sha256:caf64655bcbb2fc4b0077421d3d4d74abbcbeae0b9e 1.02MB / 1.02MB  0.2s
 => => sha256:559b6a8efaff85116a01db6047cd91aaf1b5d6d425c 2.53kB / 2.53kB  0.0s
 => => sha256:99fbfe67ce9c6c413efbc346c97f2881122be29216f 1.38kB / 1.38kB  0.0s
 => => sha256:d1927dbcbcab79b6a3a9073a07a412fcbd17609d 31.21MB / 31.21MB  16.0s
 => => sha256:64166705448110ce6a01140b01312c5bafacd0e4fe 8.09MB / 8.09MB  11.4s
 => => extracting sha256:68ced04f60ab5c7a5f1d0b0b4e7572c5a4c8cce44866513d  5.4s
 => => extracting sha256:e936bd534ffb9280632b9c0c87e0f5ab3031e53da489a242  0.8s
 => => extracting sha256:caf64655bcbb2fc4b0077421d3d4d74abbcbeae0b9e1e860  0.0s
 => => extracting sha256:d1927dbcbcab79b6a3a9073a07a412fcbd17609d998f2cbc  1.2s
 => => extracting sha256:64166705448110ce6a01140b01312c5bafacd0e4fe686195  0.2s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:1313317e956fec2e77668b4715fcc4a8a07d92141bc93  0.0s
 => => naming to docker.io/golden/aspnet-core:3.0                          0.0s
```

### Chapter 5 Lab
`http://registry.local:5000/v2` does show `{}` after I click to contine, despite my browser warning me about the insecure connection
Same with `http://registry.local:5001/v2/`.

```bash
λ curl http://registry.local:5001/v2/gallery/ui/tags/list
{"errors":[{"code":"NAME_UNKNOWN","message":"repository name not known to registry","detail":{"name":"gallery/ui"}}]}
```

Windows:
```PowerShell
PS> curl http://registry.local:5000/v2/gallery/ui/tags/list


StatusCode        : 200
StatusDescription : OK
Content           : {"name":"gallery/ui","tags":["v1","latest"]}

RawContent        : HTTP/1.1 200 OK
                    Docker-Distribution-Api-Version: registry/2.0
                    X-Content-Type-Options: nosniff
                    Content-Length: 45
                    Content-Type: application/json; charset=utf-8
                    Date: Tue, 05 Mar 2024 18:38:40 GMT...
Forms             : {}
Headers           : {[Docker-Distribution-Api-Version, registry/2.0], [X-Content-Type-Options, nosniff],
                    [Content-Length, 45], [Content-Type, application/json; charset=utf-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 45
PS> curl --head http://registry.local:5000/v2/gallery/ui/manifests/latest -H 'Accept: application/vnd.docker.distribution.manifest.v2+json'
HTTP/1.1 200 OK
Content-Length: 5176
Content-Type: application/vnd.docker.distribution.manifest.v1+prettyjws
Docker-Content-Digest: sha256:f5a0c941440e051e716343e2d9e6569470fbd454d0eb1f71ac03a1f9b1b595bd
Docker-Distribution-Api-Version: registry/2.0
Etag: "sha256:f5a0c941440e051e716343e2d9e6569470fbd454d0eb1f71ac03a1f9b1b595bd"
X-Content-Type-Options: nosniff
Date: Tue, 05 Mar 2024 18:40:22 GMT

curl: (6) Could not resolve host: application

PS> curl -X DELETE http://registry.local:5000/v2/gallery/ui/manifests/sha256:f5a0c941440e051e716343e2d9e6569470fbd454d0eb1f71ac03a1f9b1b595bd
{"errors":[{"code":"MANIFEST_UNKNOWN","message":"manifest unknown"}]}

PS> curl http://registry.local:5000/v2/gallery/ui/tags/list
{"name":"gallery/ui","tags":["v1","latest"]}

(cmd)> curl -X DELETE http://registry.local:5000/v2/gallery/ui/manifests/sha256:f5a0c941440e051e716343e2d9e6569470fbd454d0eb1f71ac03a1f9b1b595bd
{"errors":[{"code":"MANIFEST_UNKNOWN","message":"manifest unknown"}]}
```


