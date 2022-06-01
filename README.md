如何编译一个自己的docker-ce发行版
1. 准备一个海外机器4core + 16G + 50G硬盘
2. 系统要求：Ubuntu 18.04 + Go 1.13.15
3. 安装docker-ce 最新版 （20.10.2）
4. git clone https://github.com/docker/docker-ce-packaging packaging
```
cd packaging && git checkout -b 20.10 origin/20.10
export VERSION=20.10.16
export DOCKER_CLI_REPO=https://github.com/shezhua/cli.git
export DOCKER_ENGINE_REPO=https://github.com/shezhua/moby.git
export PLATFORM="Docker Engine - Bomayu SE"
export DOCKER_CLI_REF=v20.10.16.bomayu
export DOCKER_ENGINE_REF=v20.10.16.bomayu

make centos-7
make centos-8
```

# Docker CE Packaging

This repo contains the open source scripts for packaging
[Docker CE products](https://store.docker.com/search?offering=community&q=&type=edition).

This repository is solely maintained by Docker, Inc.

The scripts will build for this list of packages types:

* DEB packages for Ubuntu 20.04 Focal
* DEB packages for Ubuntu 18.04 Bionic
* DEB packages for Ubuntu 16.04 Xenial
* DEB packages for Debian 10 Buster
* RPM packages for Fedora 33
* RPM packages for Fedora 32
* RPM packages for Fedora 31
* RPM packages for CentOS 8
* RPM packages for CentOS 7
* RPM packages for Red Hat Enterprise Linux 7
* TGZ and ZIP files with static binaries
