---
title: 重拾 Docker
date: 2018-01-05 15:26:24
tags:
  - docker
  - nodejs
categories:
  - nodejs
---
> Nice to meet U, I'm Docker

Docker刚出来那会儿，还是粗暴地一个virtualbox+系统镜像的分发方式，后来有了katematic，再后来有了docker-tools。

一年不见，docker变化真大，最近有项目上nodejs，正好重新认识一下docker。
<!-- more -->

## 一、安装

当前，docker分商业版和社区版，个人当然是安装社区版：[docker-ce-desktop-mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)
> 不推荐使用Homebrew，安装的只是一个docker-cli 命令行工具

### 镜像加速

Docker再国内访问很慢，有些地区是直接墙掉的，所以一定要用镜像加速 `Preferences - Daemon - Registry mirrors` 下增加DaoCloud的免费镜像地址 `http://ebccc999.m.daocloud.io`，选DaoCloud是google出来的结果，速度也不错（宣称永久免费），就选了它。阿里云的还要进控制台看，太麻烦了。填写完保存重启`Apply & Restart`。

安装完，命令行输入`docker info` 查看详细信息。

## 二、试一试

起一个nginx的容器试试 `docker container run --publish 80:80 nginx` ，由于新安装，本地并没有Nginx镜像，所以会从仓库拉一个过来。

```bash
$ docker container run --publish 80:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
e7bb522d92ff: Pull complete
6edc05228666: Pull complete
cd866a17e81f: Pull complete
Digest: sha256:8374bf37a36a58cbbc9a3b8c6f5b838b750a610986d90f5b2437411b38e5e93e
Status: Downloaded newer image for nginx:latest
```

下载完成后就可以看到 [http://localhost](http://localhost) Nginx 启动成功的页面。

如果是后台运行的话，加上 `--detach` 参数：`docker container run --publish 80:80 --detach nginx`，会返回一个容器ID，比如：_0077235b8a9d*************************************_

### 列出运行中的容器

`docker container ls`

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
0077235b8a9d        nginx               "nginx -g 'daemon ..."   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   inspiring_saha
```

### 停止容器

`docker container stop 0077235b8a9d`

### 列出所有容器，包括已经停止的

`docker container ls -a`

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS                NAMES
0077235b8a9d        nginx               "nginx -g 'daemon ..."   4 minutes ago       Up 4 minutes               0.0.0.0:80->80/tcp   inspiring_saha
c4a05b8e8200        nginx               "nginx -g 'daemon ..."   8 minutes ago       Exited (0) 4 minutes ago                        amazing_noyce
```

每个容器的名字 _NAMES_ 是唯一的，可以指定 `docker container run --publish 80:80 --detach --name nginx4node nginx` ，不指定就随机分配一个。

### 重启已停止的容器

`docker container start 0077235b8a9d`

### 查看容器日志

`docker container logs nginx4node`


### 删除一个容器

`docker container rm 0077235b8a9d`

## 参数自定义

`docker container run --publish 8080:80 --detach nginx:*.* --name nginx4node`

+ `8080:80`，前面是宿主机端口，后面是容器的端口
+ `nginx:*.*`，Nginx指定版本号，如果不指定，就是取最新版`nginx:latest`

## 老朋友 Katematic

有时候GUI可以提高生产力，为何一定要在CLI装B呢？通过 Katematic 可以很直观，也很方便的配置镜像。

![katematic](/public/katematic.png)
