---
title: 安装docker
authot: lincc
date: 2024-07-15 周一
time: 09:35
tags: 
reference:
  - https://docs.docker.com/engine/install/centos/
---
[docker官方文档](https://docs.docker.com/engine/install/centos/)
## 移除旧版本docker
```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装 yum-utils 工具类，用来配置docker的下载地址源
```bash
sudo yum install -y yum-utils
```
```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

## 安装docker引擎
包含
- docker-ce (引擎)
- docker-ce-cli (引擎的命令行程序)
- containerd.io (运行时容器环境)
- docker-buildx-plugin (构建镜像的工具)
- docker-compose-plugin (做批量的工具)
```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
## 启动docker
```bash
sudo systemctl start docker
```
## 开机自启动
```bash
systemctl enable docker
```


