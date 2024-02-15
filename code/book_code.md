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
