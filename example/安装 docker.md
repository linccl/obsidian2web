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


以下是针对 `Ubuntu` 的安装步骤：

### 1. 移除旧版本 Docker
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### 2. 更新 APT 包索引并安装依赖包
```bash
sudo apt-get update
```
```bash
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```

### 3. 添加 Docker 的官方 GPG 密钥
```bash
sudo mkdir -p /etc/apt/keyrings
```
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### 4. 设置稳定版 Docker 软件源
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. 安装 Docker 引擎及相关组件
```bash
sudo apt-get update
```
```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 6. 启动 Docker 服务
```bash
sudo systemctl start docker
```

### 7. 设置 Docker 开机自启动
```bash
sudo systemctl enable docker
```

