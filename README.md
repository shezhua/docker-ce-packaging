# 如何编译一个自己的docker-ce发行版
1. 准备一个海外机器4core + 16G + 50G硬盘
2. 系统要求：Ubuntu 18.04 + Go 1.13.15 （不是必须的）
3. 安装docker-ce 最新版

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

sed -i 's/Docker Engine - Community/Docker Engine - Bomayu SE/g' common.mk
export VERSION=20.10.24
export DOCKER_CLI_REPO=https://github.com/shezhua/cli.git
export DOCKER_ENGINE_REPO=https://github.com/shezhua/moby.git
export DOCKER_CLI_REF=v20.10.24.bomayu
export DOCKER_ENGINE_REF=v20.10.24.bomayu
```
6. 打包centos
```
make centos-7
make centos-8
```
7. 后续创建私有repos
```
yum install -y createrepo
mkdir -p /data/yum.repos/centos/7/x86_64,/data/yum.repos/centos/8/x86_64
# cp rpm file to centos/7/x86_64

# 首次创建
createrepo -dpo /data/yum.repos/centos/7/x86_64 /data/yum.repos/centos/7/x86_64
createrepo -dpo /data/yum.repos/centos/8/x86_64 /data/yum.repos/centos/8/x86_64

# 后续更新
createrepo --update /data/yum.repos/centos/7/x86_64
createrepo --update /data/yum.repos/centos/8/x86_64

# 同步到文件仓库
```

8. 客户机安装
```
yum install -y yum-utils
yum-config-manager --add-repo=https://repo.bomayu.com/centos/docker-bomayu-se.repo
yum install -y docker-ce docker-ce-cli docker-ce-rootless-extras docker-compose-plugin docker-scan-plugin containerd.io
systemctl start docker
systemctl enable docker
docker ps
docker version
```
