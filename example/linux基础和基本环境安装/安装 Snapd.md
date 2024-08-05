在 CentOS 7.9 系统上安装和使用 Snap 包的过程稍微有些不同。以下是详细的步骤：

### 1. 安装 EPEL 仓库

首先，需要安装 EPEL（Extra Packages for Enterprise Linux）仓库，因为 Snapd 需要的某些依赖项在这个仓库中。

```bash
sudo yum install epel-release
```
### 2. 安装 Snapd

使用 yum 包管理器来安装 Snapd：

```bash
sudo yum install snapd
```
### 3. 启用并启动 Snapd 服务

启用并启动 Snapd 的 socket：

```bash
sudo systemctl enable --now snapd.socket
```
### 4. 创建符号链接

创建一个符号链接，将 `/var/lib/snapd/snap` 连接到 `/snap`：

```bash
sudo ln -s /var/lib/snapd/snap /snap
```

### 5. 手动安装依赖 Snap 包

```bash
sudo snap install gtk-common-themes
sudo snap install gnome-3-28-1804
```

### 6. 安装 Snap 包

假设 `filename.snap` 文件在你的当前目录下，你可以使用以下命令来安装这个 Snap 包：

```bash
sudo snap install --dangerous ./filename.snap
```

### 7. 启动和使用

安装完成后，你可以查看已安装的 Snap 包：

```bash
snap list
```


- [ ] `todo`: 增量式学习(等要用到的时候再学习并完善剩下的)

要启动应用程序，假设它的命令是 `piclist`，可以使用以下命令：

`piclist`

如果不确定命令名称，你可以尝试使用：

`snap run piclist`