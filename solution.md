# Solution

This solution will deploy [REA Systems Engineer practical task](https://github.com/rea-cruitment/simple-sinatra-app) to a Centos 7 host, a physical or a virtual machine, with [Docker](https://www.docker.com/).

## Prepare a linux host (physical or virtual) with docker up and running

```
[fli@192-168-1-10 ~]$ sudo systemctl is-active docker
[sudo] password for fli: 
active
[fli@192-168-1-10 ~]$ docker info
Client:
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 14
 Server Version: 19.03.5
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Native Overlay Diff: true
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
 containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
 runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-1062.9.1.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 7.586GiB
 Name: 192-168-1-10.tpgi.com.au
 ID: 45NJ:KYEP:OZMC:QMQD:PKAM:7VVO:NG6J:NUQU:O7R2:JWBE:4TKV:HHBZ
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Username: fen9li
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

[fli@192-168-1-10 ~]$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[fli@192-168-1-10 ~]$ 
```

## Run below command to deploy and done

```
docker run -d --rm --name sinatra -p 80:9292 fen9li/sinatra-test:v1.0.0
```

## Q & A