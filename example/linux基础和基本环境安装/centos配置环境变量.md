---
title: centos配置系统环境变量
authot: lincc
date: 2024-07-10 周三
time: 17:49
tags:
---
在 CentOS 中设置环境变量，可以通过修改 `/etc/profile` 文件来实现，这会对所有用户生效。如果只需要对当前用户生效，可以修改用户的 `.bashrc` 文件。下面是详细的步骤：

### 1. 设置系统范围的环境变量

编辑 `/etc/profile` 文件以设置系统范围的环境变量。

1. 打开 `/etc/profile` 文件：
    
```bash
sudo vim /etc/profile
```
    
2. 在文件的末尾添加如下行：
    
```bash
export PICLIST_KEY=your_value
```
    
3. 保存并关闭文件。
    
4. 使更改生效：
    
```bash
source /etc/profile
```

### 2. 设置用户范围的环境变量

编辑用户的 `.bashrc` 文件以设置当前用户的环境变量。

1. 打开用户的 `.bashrc` 文件：
    
```bash
vim ~/.bashrc
```
    
2. 在文件的末尾添加如下行：
    
```bash
export YOUR_KEY=your_value
```
    
3. 保存并关闭文件。
    
4. 使更改生效：
    
```bash
source ~/.bashrc
```
    

### 命令行写法
```bash
echo "export your_key='123456'" >> ${HOME}/.bash_profile # 将 123456 设置为环境变量的值
source ${HOME}/.bash_profile
```

### 验证环境变量

要验证环境变量是否设置成功，可以运行以下命令：

```bash
echo $YOUR_KEY
```

如果输出为 `your_value`，则说明环境变量设置成功。

### 注意事项

- 确保你用的是正确的值替换 `your_value`。
- 修改 `/etc/profile` 需要超级用户权限，所以需要使用 `sudo` 命令。