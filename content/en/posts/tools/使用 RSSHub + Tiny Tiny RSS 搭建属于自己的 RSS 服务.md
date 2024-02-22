---
title: 使用 RSSHub + Tiny Tiny RSS 搭建属于自己的 RSS 服务
date: 2022-09-27
tags: 
  - rss
categories:
  - tools
---

<!--more-->

![](https://cdn.hcplantern.cn/img/202205151528372.png)

前言：

最近了解到 RSS 这种古老的信息聚合方式。虽然现在互联网环境早已被移动 APP 分割成孤岛，RSS 也已然没落，不过秉持着传承互联网分享精神的初衷，本人决定尝试一下使用 RSS 来获取内容与信息。为了能够得到最好的体验，我决定自建 RSSHub 并使用 Tiny Tiny RSS 来在服务器上管理信息来源，以便做到多设备同步 RSS 源。（再次感叹，有个服务器真的能做很多事情）

## 部署 RSSHub & Awesome TTRSS

本部分使用 Docker-Compose 来部署 RSSHub 与 TTRSS 服务。

参考 ：

1. [部署 | RSSHub](https://docs.rsshub.app/install/)
2. [🐋 Awesome TTRSS | 🐋 Awesome TTRSS (henry.wang)](https://ttrss.henry.wang/zh/)

这里我将两部分的 `docker-compose.yml` 文件合并。

配置文件细节：

1. **RSSHub 部分**无需更改，后续增加 cookie 时可以参考 **RSSHub 文档**在 environment 中添加参数。
2. **TTRSS 部分**，注意：一定要更改两处 **password**。一开始本人没有更改而导致 TTRSS 怎么都进不去……此外还需要更改 `SELF_URL_PATH` 为访问的域名，后面我使用了 **Nginx** 进行反代并设置 https 。如果不使用域名，也可以仅使用 ip： `http://{your_server_ip}:181` 。

```yaml
version: '3'

services:
  # rsshub part
  rsshub:
    image: diygod/rsshub
    restart: always
    ports:
      - '1200:1200'
    environment:
      NODE_ENV: production
      CACHE_TYPE: redis
      REDIS_URL: 'redis://redis:6379/'
      PUPPETEER_WS_ENDPOINT: 'ws://browserless:3000'
      BILIBILI_COOKIE_{your_bilibili_id}: {your_cookie} # refer to https://docs.rsshub.app/install/
    depends_on:
      - redis
      - browserless

  browserless:
    image: browserless/chrome
    restart: always
    ulimits:
      core:
        hard: 0
        soft: 0

  redis:
    image: redis:alpine
    restart: always
    volumes:
      - redis-data:/data

  # ttrss part
  service.rss:
    image: wangqiru/ttrss:latest
    container_name: ttrss
    ports:
      - 181:80
    environment:
      - SELF_URL_PATH=https://ttrss.hcplantern.cn/ # please change to your own domain
      - DB_PASS={your_passwd} # use the same password defined in `database.postgres`
      - PUID=1000
      - PGID=1000
      - ALLOW_PORTS=1200
    volumes:
      - feed-icons:/var/www/feed-icons/
    networks:
      - public_access
      - service_only
      - database_only
    stdin_open: true
    tty: true
    restart: always

  service.mercury:
    # set Mercury Parser API endpoint to `service.mercury:3000` on TTRSS plugin setting page
    image: wangqiru/mercury-parser-api:latest
    container_name: mercury
    networks:
      - public_access
      - service_only
    restart: always
	# 这将使 Watchtower 跳过对 mercury-parser-api 的更新检测
    labels:
      - com.centurylinklabs.watchtower.enable=false

  service.opencc:
    # set OpenCC API endpoint to `service.opencc:3000` on TTRSS plugin setting page
    image: wangqiru/opencc-api-server:latest
    container_name: opencc
    environment:
      - NODE_ENV=production
    networks:
      - service_only
    restart: always

  database.postgres:
    image: postgres:13-alpine
    container_name: postgres
    environment:
      - POSTGRES_PASSWORD={your_passwd} # feel free to change the password
    volumes:
      - ~/postgres/data/:/var/lib/postgresql/data # persist postgres data to ~/postgres/data/ on the host
    networks:
      - database_only
    restart: always

  utility. watchtower:
    container_name: watchtower
    image: containrrr/watchtower:latest
    volumes:
      - /var/run/docker. sock:/var/run/docker. sock
    environment:
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_POLL_INTERVAL=86400
    restart: always

volumes:
  redis-data:
  feed-icons:


networks:
  public_access: # Provide the access for ttrss UI
  service_only:
    # Provide the communication network between services only
    internal: true
  database_only:
    # Provide the communication between ttrss and database only
    internal: true
```

写好配置文件后，运行 `sudo docker-compose up -d` 启动 docker 服务。

关闭 docker 服务使用命令 `sudo docker-compose down`

## Nginx 反向代理

参考：

1. [Nginx配置反向代理，一篇搞定！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/451825018)
2. [用Docker部署个人RSS服务（TTRSS+RSSHub） - Elnath&#39;s Spaceship (elnathmojo.github.io)](https://elnathmojo.github.io/2019/06/03/%E7%94%A8Docker%E9%83%A8%E7%BD%B2%E4%B8%AA%E4%BA%BARSS%E6%9C%8D%E5%8A%A1-TTRSS+RSSHub.html)

首先，我为两个服务添加了两个二级域名并通过 A 记录解析到服务器 ip，并为这两个二级域名分别申请免费 Https 证书（阿里云），将证书上传到服务器。

之后填写 Nginx 配置：

1. 将两个 Http 请求转成 Https 请求。
2. 设置监听两个 Https 请求时，只需填写域名、SSL 证书路径以及代理的 ip 地址即可完成。

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name ttrss.hcplantern.cn rsshub.hcplantern.cn;
    # 将请求转成https
    rewrite ^ https://$host$request_uri? permanent;
}

server {
    # 监听443端口
    listen 443 ssl;
    # 你的域名
    server_name ttrss.hcplantern.cn; 
    # ssl证书的pem文件路径
    ssl_certificate  /home/Plantern/rss/ttrss/7689569_ttrss.hcplantern.cn.pem;
    # ssl证书的key文件路径
    ssl_certificate_key /home/Plantern/rss/ttrss/7689569_ttrss.hcplantern.cn.key;
    location / {
    # 代理的ip地址
     proxy_pass  http://121.40.140.239:181;
     proxy_set_header Host $http_host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    # 监听443端口
    listen 443 ssl;
    # 你的域名
    server_name rsshub.hcplantern.cn; 
    # ssl证书的pem文件路径
    ssl_certificate  /home/Plantern/rss/rsshub/7689578_rsshub.hcplantern.cn.pem;
    # ssl证书的key文件路径
    ssl_certificate_key /home/Plantern/rss/rsshub/7689578_rsshub.hcplantern.cn.key;
    location / {
    # 代理的ip地址
     proxy_pass  http://121.40.140.239:1200;
     proxy_set_header Host $http_host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 设置 TTRSS

上述设置完毕之后，可以通过 ` https://ttrss.hcplantern.cn` 访问 TTRSS 管理界面，初始账号：`admin` ；初始密码：`password`。一定要第一时间修改。

在设置界面，可以选择内置主题以及插件。由于使用 Docker-Compose 安装，已经包括了许多常用插件如 `fever`、`mercury_fulltext`、 `remove_iframe_sandbox` 等，启用这几个插件。

同时，在通用里启用「允许外部客户端通过 API 来访问该账户」：

<img src="https://cdn.hcplantern.cn/img/202204281327644.png" style="zoom: 50%;" />

在 **Fever Emulation** 设置部分，首先设置 **Fever** 访问密码，之后便可以在 **RSS 客户端**使用 ` https://ttrss.hcplantern.cn/plugins.local/fever/` 来访问 **Fever 服务**（ps. 亲测需要去掉 .local，如图）

<img src="https://cdn.hcplantern.cn/img/202204281330161.png" style="zoom:50%;" />

这样，通过 TTRSS 管理你的订阅源，再在客户端中使用 fever 访问你的 TTRSS，Perfect !

具体如何添加订阅源，参考 [介绍 | RSSHub](https://docs.rsshub.app/)

## Windows 客户端：「RSS 追踪」

在 **Microsoft Store** 中可以搜索这款软件，可以暂且选择试用并下载。

安装完毕之后，建议使用 **Fiddler** 或其他应用关闭沙盒环境，参考：[如何为 Windows 10 UWP 应用设置代理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/29989157)

打开，选择 **Fever**，填写域名、用户名和密码：

<img src="https://cdn.hcplantern.cn/img/202204281348137.png" style="zoom: 67%;" />

之后便可以正常使用了。

<img src="https://cdn.hcplantern.cn/img/202204281350115.png" style="zoom: 67%;" />

## Android 客户端：「FeedMe」、「知微」

2022-5-5 更新

<img src="https://cdn.hcplantern.cn/img/202205052144848.png" style="zoom: 33%;" />

FeedMe 不错，界面相比于「知微」更加美观。

---

目前暂时使用这个应用，同样可添加 Fever 服务。界面如下：

<img src="https://cdn.hcplantern.cn/img/202204281354724.png" style="zoom: 33%;" />

## IOS 客户端：「Reeder」

需要使用非国区账号下载，支持 Fever。

<img src="https://cdn.hcplantern.cn/img/202205052147460.png" style="zoom: 33%;" />

## 参考资料：

1. [介绍 | RSSHub](https://docs.rsshub.app/)
2. [部署 | RSSHub](https://docs.rsshub.app/install/)
3. [🐋 Awesome TTRSS | 🐋 Awesome TTRSS (henry.wang)](https://ttrss.henry.wang/zh/)
4. [Nginx配置反向代理，一篇搞定！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/451825018)
5. [用Docker部署个人RSS服务（TTRSS+RSSHub） - Elnath&#39;s Spaceship (elnathmojo.github.io)](https://elnathmojo.github.io/2019/06/03/%E7%94%A8Docker%E9%83%A8%E7%BD%B2%E4%B8%AA%E4%BA%BARSS%E6%9C%8D%E5%8A%A1-TTRSS+RSSHub.html)
6. [如何为 Windows 10 UWP 应用设置代理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/29989157)
