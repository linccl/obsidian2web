---
title: 搭建glance信息聚合网站
authot: lincc
date: 2024-07-11 周四
time: 17:49
tags:
---
## 参考资料
[参考博客](https://blog.laoda.de/archives/docker-compose-install-glance)
[GitHub地址](https://github.com/glanceapp/glance)
## 搭建过程
```bash
mkdir glance

cd glance

mkdir assets

vim glance.yml
```


下面是glance.yml文件的demo
```yaml
pages:
  - name: Home
    columns:
      - size: small
        widgets:
          - type: calendar
          - type: clock
            hour-format: 24h
            timezones:
              - timezone: Europe/Paris
                label: Paris
              - timezone: America/New_York
                label: New York
              - timezone: Asia/Tokyo
                label: Tokyo

          - type: monitor
            cache: 1m
            title: Services
            sites:
              - title: Jellyfin
                url: https://jellyfin.yourdomain.com
                icon: /assets/jellyfin-logo.png
              - title: Gitea
                url: https://gitea.yourdomain.com
                icon: /assets/gitea-logo.png
              - title: Immich
                url: https://immich.yourdomain.com
                icon: /assets/immich-logo.png
              - title: AdGuard Home
                url: https://adguard.yourdomain.com
                icon: /assets/adguard-logo.png
              - title: Vaultwarden
                url: https://vault.yourdomain.com
                icon: /assets/vaultwarden-logo.png

          - type: bookmarks
            groups:
              - links:
                  - title: Gmail
                    url: https://mail.google.com/mail/u/0/
                  - title: Amazon
                    url: https://www.amazon.com/
                  - title: Github
                    url: https://github.com/
                  - title: Wikipedia
                    url: https://en.wikipedia.org/
              - title: Entertainment
                color: 10 70 50
                links:
                  - title: Netflix
                    url: https://www.netflix.com/
                  - title: Disney+
                    url: https://www.disneyplus.com/
                  - title: YouTube
                    url: https://www.youtube.com/
                  - title: Prime Video
                    url: https://www.primevideo.com/
              - title: Social
                color: 200 50 50
                links:
                  - title: Reddit
                    url: https://www.reddit.com/
                  - title: Twitter
                    url: https://twitter.com/
                  - title: Instagram
                    url: https://www.instagram.com/


      - size: full
        widgets:
          - type: rss
            title: News
            style: horizontal-cards
            limit: 10
            collapse-after: 10
            cache: 3h
            feeds:
              - url: https://blog.laoda.de/rss.xml
                title: 我不是咕咕鸽
          - type: videos
            channels:
              - UCJeNmdZBL8QahqCbzxj4l3Q

      - size: small
        widgets:
          - type: weather
            location: Shanghai, China

          - type: markets
            markets:
              - symbol: SPY
                name: S&P 500
              - symbol: TSLA
                name: TSLA
              - symbol: BTC-USD
                name: Bitcoin
              - symbol: NVDA
                name: NVIDIA
              - symbol: AAPL
                name: Apple
              - symbol: MSFT
                name: Microsoft
              - symbol: GOOGL
                name: Google
              - symbol: AMD
                name: AMD
```
新建 docker-compose.yml 文件
```bash
vim docker-compose.yml
```

下面是docker-compose.yml文件内容
```yaml
services:
  glance:
    image: glanceapp/glance
    container_name: glance
    restart: unless-stopped
    ports:
      - 8080:8080        # 左边的8080可以自由修改成服务器上没有被占用的端口，右边的8080不要动。
    volumes:
      - ./glance.yml:/app/glance.yml
      - ./assets:/app/assets
      - /etc/TZ:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
```


lsof -i:8080  #查看 8080 端口是否被占用，如果被占用，重新自定义一个端口

我的被占用了，改成了8090端口

启动 glance
docker compose up -d   # 注意，老版本用户用 docker-compose up -d


## nginx 配置
[[宝塔配置反向代理#配置过程]]
#### 加上需要的 location
```nginx
    location / {
      proxy_pass http://127.0.0.1:8090/;       # 注意改成你实际使用的端口
      rewrite ^/(.*)$ /$1 break;
      proxy_redirect off;
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Upgrade-Insecure-Requests 1;
      proxy_set_header X-Forwarded-Proto https;
    }
```

^afe786
