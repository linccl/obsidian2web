## 判断系统架构
查看内核版本: `uname -r`

要判断系统的架构是ARM还是AMD（x86_64），可以通过几个方法来确定。以下是一些常用的方法：

### 方法一：通过内核名称
从内核名称 `4.18.0-348.7.1.el8_5.x86_64` 中可以看出，它包含 `x86_64`，这表明系统是基于x86_64架构的。x86_64是AMD和Intel的64位架构。

### 方法二：使用`uname`命令
可以使用`uname`命令来检查系统的架构类型。运行以下命令：

```bash
uname -m
```

这个命令的输出将显示系统的硬件架构类型：
- 如果输出是 `x86_64`，则系统是基于AMD/Intel的x86_64架构。
- 如果输出是 `aarch64` 或其他与ARM相关的字符串，则系统是基于ARM架构。

### 方法三：查看CPU信息
可以查看 `/proc/cpuinfo` 文件来获取详细的CPU信息。运行以下命令：

```bash
cat /proc/cpuinfo
```

在输出中查找以下信息：
- 对于x86_64架构，通常会看到 `vendor_id` 为 `GenuineIntel` 或 `AuthenticAMD`。
- 对于ARM架构，会看到 `CPU architecture` 为 `8` 或其他与ARM相关的描述。

### 方法四：使用`lscpu`命令
可以使用 `lscpu` 命令来获取CPU架构信息。运行以下命令：

```bash
lscpu
```

在输出中查找 `Architecture` 字段：
- 如果显示为 `x86_64`，则系统是基于x86_64架构。
- 如果显示为 `aarch64` 或其他与ARM相关的字符串，则系统是基于ARM架构。

### 示例
以下是一些命令的示例输出：

#### `uname -m`
```bash
$ uname -m
x86_64
```

#### `lscpu`
```bash
$ lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              4
...
```

#### `/proc/cpuinfo`
```bash
$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 158
model name      : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
...
```

通过上述方法，可以轻松确定系统是基于ARM架构还是AMD/Intel的x86_64架构。

## 查看端口是否放开
在 CentOS 系统上查看某个端口是否放开，可以通过以下几种方法实现：

### 方法一：使用 `firewall-cmd` 命令

CentOS 7 和 CentOS 8 使用 `firewalld` 作为默认的防火墙管理工具。你可以使用 `firewall-cmd` 命令来查看特定端口的状态。

1. 检查防火墙状态：
   ```bash
   sudo firewall-cmd --state
   ```

2. 查看所有已打开的端口：
   ```bash
   sudo firewall-cmd --list-ports
   ```

3. 查看特定端口是否放开：
   ```bash
   sudo firewall-cmd --query-port=<port>/<protocol>
   ```
   例如，检查 TCP 端口 80 是否放开：
   ```bash
   sudo firewall-cmd --query-port=80/tcp
   ```

### 方法二：使用 `iptables` 命令

在较早的 CentOS 版本中，或者如果你手动配置了 `iptables`，你可以使用以下命令查看规则：

1. 列出所有 `iptables` 规则：
   ```bash
   sudo iptables -L -n
   ```

2. 查找特定端口的规则：
   ```bash
   sudo iptables -L -n | grep <port>
   ```
   例如，检查 TCP 端口 80 的规则：
   ```bash
   sudo iptables -L -n | grep 80
   ```

### 方法三：使用 `ss` 或 `netstat` 命令查看端口状态

你也可以查看端口当前的监听状态：

1. 使用 `ss` 命令（CentOS 7 及以后推荐）：
   ```bash
   ss -tuln | grep <port>
   ```
   例如，检查端口 80 是否被监听：
   ```bash
   ss -tuln | grep 80
   ```

2. 使用 `netstat` 命令（CentOS 6 及更早版本）：
   ```bash
   netstat -tuln | grep <port>
   ```
   例如，检查端口 80 是否被监听：
   ```bash
   netstat -tuln | grep 80
   ```

### 示例

假设我们要检查端口 8080 是否放开并被监听，具体步骤如下：

1. 使用 `firewall-cmd` 查看端口是否放开：
   ```bash
   sudo firewall-cmd --query-port=8080/tcp
   ```
   返回 `yes` 表示端口放开，`no` 表示没有放开。

2. 使用 `ss` 命令查看端口是否被监听：
   ```bash
   ss -tuln | grep 8080
   ```
   返回结果如 `LISTEN` 表示端口被监听，否则端口未被监听。

通过这些方法，你可以确定特定端口在 CentOS 系统上是否被防火墙放行并且被监听。
## 放行端口
要放开指定端口（例如 8081）以允许 TCP 连接，你需要使用 `firewall-cmd` 命令来修改防火墙配置。以下是步骤：

### 步骤 1：打开端口

1. 立即打开端口（这不会持久化到防火墙规则，只在当前会话中有效）：
   ```bash
   sudo firewall-cmd --add-port=8081/tcp
   ```

2. 持久化端口规则（这会确保端口在重启后仍然开放）：
   ```bash
   sudo firewall-cmd --add-port=8081/tcp --permanent
   ```

### 步骤 2：重新加载防火墙配置

为了使持久化的规则生效，你需要重新加载防火墙：
   ```bash
   sudo firewall-cmd --reload
   ```

### 步骤 3：验证端口是否开放

最后，验证端口是否成功开放：
   ```bash
   sudo firewall-cmd --query-port=8081/tcp
   ```

如果操作成功，应该返回 `yes`。完整操作如下：

```bash
sudo firewall-cmd --add-port=8081/tcp        # 临时开放端口
sudo firewall-cmd --add-port=8081/tcp --permanent  # 持久化端口开放
sudo firewall-cmd --reload                    # 重新加载防火墙规则
sudo firewall-cmd --query-port=8081/tcp      # 验证端口是否开放
```

### 示例

假设你执行了上述命令，预期的输出如下：

```bash
[root@ser845529143888 ~]# sudo firewall-cmd --add-port=8081/tcp
success
[root@ser845529143888 ~]# sudo firewall-cmd --add-port=8081/tcp --permanent
success
[root@ser845529143888 ~]# sudo firewall-cmd --reload
success
[root@ser845529143888 ~]# sudo firewall-cmd --query-port=8081/tcp
yes
```

这样，端口 8081 就被成功开放，并且会在系统重启后依然开放。