
# Solution Build Guide

This `Solution Build Guide` steps how to solve [REA Systems Engineer practical task](https://github.com/rea-cruitment/simple-sinatra-app) and deploys it to a local linux host. 

## Before start: ensure Docker service is up and running in your local linux host
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

## Step 1: clone `https://github.com/rea-cruitment/simple-sinatra-app.git`
```
[fli@192-168-1-10 ~]$ git clone https://github.com/rea-cruitment/simple-sinatra-app.git
Cloning into 'simple-sinatra-app'...
remote: Enumerating objects: 37, done.
remote: Total 37 (delta 0), reused 0 (delta 0), pack-reused 37
Unpacking objects: 100% (37/37), done.
[fli@192-168-1-10 ~]$

[fli@192-168-1-10 ~]$ cd simple-sinatra-app/
[fli@192-168-1-10 simple-sinatra-app]$ 

[fli@192-168-1-10 simple-sinatra-app]$ ll -a
total 24
drwxrwxr-x.  3 fli fli  106 Feb 10 15:26 .
drwx-----x. 55 fli fli 4096 Feb 10 15:26 ..
-rw-rw-r--.  1 fli fli   50 Feb 10 15:26 config.ru
-rw-rw-r--.  1 fli fli   43 Feb 10 15:26 Gemfile
drwxrwxr-x.  8 fli fli  163 Feb 10 15:26 .git
-rw-rw-r--.  1 fli fli   18 Feb 10 15:26 .gitignore
-rw-rw-r--.  1 fli fli   50 Feb 10 15:26 helloworld.rb
-rw-rw-r--.  1 fli fli 2058 Feb 10 15:26 README.md
[fli@192-168-1-10 simple-sinatra-app]$ 
```

## Step 2: prepare your working repository

* Create new repository `simple-sinatra-app` in your GitHub account 

Here is the guide on how to [Create a repo](https://help.github.com/en/github/getting-started-with-github/create-a-repo) on GitHub.

* Change remote url to your new repository

```
[fli@192-168-1-10 simple-sinatra-app]$ git remote set-url origin git@github.com:fen9li/simple-sinatra-app.git
[fli@192-168-1-10 simple-sinatra-app]$ git remote -v
origin  git@github.com:fen9li/simple-sinatra-app.git (fetch)
origin  git@github.com:fen9li/simple-sinatra-app.git (push)
[fli@192-168-1-10 simple-sinatra-app]$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
[fli@192-168-1-10 simple-sinatra-app]$
```

* Create, change to new working branch `develop` and push to GitHub repo

```
[fli@192-168-1-10 simple-sinatra-app]$ git checkout -b develop
Switched to a new branch 'develop'
[fli@192-168-1-10 simple-sinatra-app]$ git push --set-upstream origin develop
The authenticity of host 'github.com (52.64.108.95)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,52.64.108.95' (RSA) to the list of known hosts.
Enumerating objects: 37, done.
Counting objects: 100% (37/37), done.
Delta compression using up to 8 threads
Compressing objects: 100% (33/33), done.
Writing objects: 100% (37/37), 5.08 KiB | 578.00 KiB/s, done.
Total 37 (delta 16), reused 0 (delta 0)
remote: Resolving deltas: 100% (16/16), done.
To github.com:fen9li/simple-sinatra-app.git
 * [new branch]      develop -> develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
[fli@192-168-1-10 simple-sinatra-app]$ 
```

## Step 3: create `Dockerfile` as below locally
> Note: I use [Visual Studio Code](https://code.visualstudio.com/). You can use `vim` if you dont have any other editor.    

```
[fli@192-168-1-10 simple-sinatra-app]$ vim Dockerfile
[fli@192-168-1-10 simple-sinatra-app]$ cat Dockerfile
FROM ruby:2.7.0-slim

RUN apt-get update -qq && apt-get install -y build-essential

ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/
RUN bundle config set without 'development test'
RUN bundle install

ADD . $APP_HOME

EXPOSE 9292

CMD ["bundle", "exec", "rackup", "--host", "0.0.0.0", "-p", "9292"]
[fli@192-168-1-10 simple-sinatra-app]$ 
```

## Step 4: Build docker image `sinatra-test:v1.0.0`

```
docker build --no-cache -t sinatra-test:v1.0.0 .
```

## Step 5: Deploy to local linux host and test it
* Deploy to local linux host
```
[fli@192-168-1-10 ~]$ docker run -d --rm --name sinatra -p 80:9292 sinatra-test:v1.0.0
86368cbc3fe8cca9d36ce86924097281e79cf0ecde434458dc32cb9747188ce2
[fli@192-168-1-10 ~]$ 
```

* Check the container is up and running on port `80`
```
[fli@192-168-1-10 ~]$ docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                  NAMES
86368cbc3fe8        sinatra-test:v1.0.0   "bundle exec rackup …"   13 seconds ago      Up 10 seconds       0.0.0.0:80->9292/tcp   sinatra
[fli@192-168-1-10 ~]$ 
[fli@192-168-1-10 ~]$ docker port 863
9292/tcp -> 0.0.0.0:80
[fli@192-168-1-10 ~]$ 
```

* `curl localhost` now and see `Hello World!`
```
[fli@192-168-1-10 ~]$ curl localhost
Hello World![fli@192-168-1-10 ~]$ 
```

* Double check docker logs
```
[fli@192-168-1-10 ~]$ docker logs 863
[2020-02-13 23:28:25] INFO  WEBrick 1.6.0
[2020-02-13 23:28:25] INFO  ruby 2.7.0 (2019-12-25) [x86_64-linux]
[2020-02-13 23:28:25] INFO  WEBrick::HTTPServer#start: pid=1 port=9292
172.17.0.1 - - [13/Feb/2020:23:29:05 +0000] "GET / HTTP/1.1" 200 12 0.0787
[fli@192-168-1-10 ~]$   
```

* It also works well in browser. 

![run-simple-sinatra-app-by-docker-container](screenshots/run-simple-sinatra-app-by-docker-container.png)

## Step 6: dont forget git housekeeping
> Note: you can merge it to `master` branch if you want.    

```
git add .
git commit -am 'publish solution'
git push
```

## (optional) Step 7: tag and push docker image to [DockerHub](https://hub.docker.com/)

* Tag docker image
```
[fli@192-168-1-10 ~]$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
sinatra-test         v1.0.0              03243cafe1e3        3 days ago          390MB
ruby                 2.7.0-slim          b913dc62d63c        11 days ago         149MB
[fli@192-168-1-10 ~]$ docker tag 03243cafe1e3 fen9li/sinatra-test:v1.0.0
[fli@192-168-1-10 ~]$

[fli@192-168-1-10 ~]$ docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
fen9li/sinatra-test   v1.0.0              03243cafe1e3        3 days ago          390MB
sinatra-test          v1.0.0              03243cafe1e3        3 days ago          390MB
ruby                  2.7.0-slim          b913dc62d63c        11 days ago         149MB
jenkins/jenkins       lts                 d9abe6b78d13        2 weeks ago         570MB
[fli@192-168-1-10 ~]$ 
```

* Push to DockerHub
```
[fli@192-168-1-10 ~]$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: fen9li
Password: 
WARNING! Your password will be stored unencrypted in /home/fli/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[fli@192-168-1-10 ~]$ 

[fli@192-168-1-10 ~]$ docker push fen9li/sinatra-test:v1.0.0
The push refers to repository [docker.io/fen9li/sinatra-test]
605fb97e726f: Pushed 
b95527fc9403: Pushed 
1d19c9ac67c5: Pushed 
a80e93cfbf55: Pushed 
07fa219dbca3: Pushed 
93eb6e3867f4: Pushed 
a5b96a33a938: Mounted from library/ruby 
95ab215e27f8: Mounted from library/ruby 
29d6d7a2eb3a: Mounted from library/ruby 
6bab58ebc554: Mounted from library/ruby 
488dfecc21b1: Mounted from library/ruby 
v1.0.0: digest: sha256:64a1d2267cb726ec0b84beecb4c9198d7b7e16696cfea2b7b285a818c5749aef size: 2621
[fli@192-168-1-10 ~]$ 
```

## Q & A
