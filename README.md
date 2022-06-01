如何编译一个自己的docker-ce发行版
1. 准备一个海外机器4core + 16G + 50G硬盘
2. 系统要求：Ubuntu 18.04 + Go 1.13.15
3. 安装docker-ce 最新版 （20.10.16）
```
cd /data
wget https://golang.google.cn/dl/go1.17.9.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.9.linux-amd64.tar.gz 
export PATH=$PATH:/usr/local/go/bin
go version

apt-get update
apt-get install     ca-certificates     curl     gnupg     lsb-release
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
5. 检出打包项目
```
git clone https://github.com/docker/docker-ce-packaging packaging
cd packaging && git checkout -b 20.10 origin/20.10
export VERSION=20.10.16
export DOCKER_CLI_REPO=https://github.com/shezhua/cli.git
export DOCKER_ENGINE_REPO=https://github.com/shezhua/moby.git
export PLATFORM="Docker Engine - Bomayu SE"
export DOCKER_CLI_REF=v20.10.16.bomayu
export DOCKER_ENGINE_REF=v20.10.16.bomayu
```
6. 打包centos
```
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
