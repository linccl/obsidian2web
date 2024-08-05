---
title: 安装 picList
authot: lincc
date: 2024-07-10 周三
time: 16:27
tags: 
reference:
  - https://cloud.tencent.com/developer/article/2375818
---
## 配置
- [官方文档](https://piclist.cn/)
- 创建一个文件夹，并在文件夹中创建 docker-compose.yml 文件
```yaml
services:

  piclist:
    image: 'kuingsmile/piclist:latest'
    container_name: piclist
    restart: always
    networks:
      blog_net:
        ipv4_address: 172.19.0.5
    volumes:
      - '$PWD/data/piclist:/root/.piclist'
    # 需要设置 piclist_key 环境变量
    command: node /usr/local/bin/picgo-server -k ${PICLIST_KEY}

networks:
  blog_net:
    driver: bridge
    ipam:
     config:
       - subnet: 172.19.0.0/16
```

- [[centos配置环境变量|添加环境变量]]并启动 piclist 容器, 此环境变量用于 client(obsidian) 和 piclist server 之间的鉴权
```bash
echo "export piclist_key='123456'" >> ${HOME}/.bash_profile # 将 123456 设置为自定义的密码 
source ${HOME}/.bash_profile 
docker compose up -d
```
- 修改 `data/piclist/config.json` 的配置, 以[[部署兰空图床|兰空图床]]为例添加图床配置, 内容自行修改, [官方有配置文件的详细文档](https://piclist.cn/configure.html#%E5%85%B0%E7%A9%BA%E5%9B%BE%E5%BA%8A),
```json
{
  "picBed": {
    "uploader": "lskyplist",
    "current": "lskyplist",
    "lskyplist": {
       "_configName": "lskyplist", // 图床配置名
          "host": "http://自己的ip或者域名", // 服务器地址
          "version": "V2", // 服务端版本
          "token": "Bearer 1|q8tsdfYssdfUz3ur5bjl7sdsf3ce50KsdfsdfnUF1", // 服务端token
          "strategyId": "", // 存储策略 ID
          "albumId": "1", // 相册 ID
          "permission": "private" // 权限
    }
  },
  "picgoPlugins": {
      
  }
}
```
- 最后重启一下 piclist
```bash
docker restart piclist
```
#### 说明

#### uploader 可以在 pc 版 piclist 的配置文件中找到

PicList的配置文件在不同系统里是不一样的。

- Windows: `%APPDATA%\piclist\data.json`
- Linux: `$XDG_CONFIG_HOME/piclist/data.json` or `~/.config/piclist/data.json`
- macOS: `~/Library/Application\ Support/piclist/data.json`

比如，在windows里你可以在：

`C:\Users\你的用户名\AppData\Roaming\piclist\data.json`找到它。

在linux里你可以在：

`~/.config/piclist/data.json`里找到它。

macOS同理。

提示

PicList-core的配置文件名为`config.json`。

##### host/服务器地址

服务器的地址，例如 `https://www.lsky.pro`。

##### version/服务端版本

服务端版本，目前支持V1和V2。注意只有V2版本支持云端删除。

##### token

注意填写时token需要以`Bearer`开头，例如 `Bearer 123456789`。

V1版本的token可以在个人设置页面查看。

使用 `cURL` 命令获取V2版本token:

``` bash
curl --location --request POST 'https://{host}/api/v1/tokens' \
--form 'email="{email}"' \
--form 'password="{password}"'
```

将`{host}`、`{email}`、`{password}`替换为自己的信息，然后执行命令，即可获取到token。

##### strategyId/存储策略ID

如果是 V1 或 V2 使用默认存储策略的用户，请留空。V2 版本的用户可以在个人设置页面查看。

##### albumId/相册ID
相册ID用于指定图片上传到哪个相册，如果不填写，则上传到默认相册。

##### Permission/权限

设置图片的权限，可选参数，默认为 `private`。


## nginx 配置
[[宝塔配置反向代理#配置过程]]
#### 加上需要的 location
```
    location /piclist/ {
        proxy_pass http://172.19.0.5:36677/;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Real-Port $remote_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header HTTP_X_FORWARDED_FOR $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header Accept-Encoding "br";
    }
```

^c9a217
