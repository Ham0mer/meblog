---
title: qBittorrent
published: 2025-09-06
description: '记录PT中用到的工具'
image: ''
tags: ['PT','工具']
category: '工具'
draft: false 
lang: 'zh_CN'
---

:::tip
个人觉得4.3.9版本是最好用的！
:::
注意`config`和`downloads`要在本地创建！
```sh frame="none"
docker run -d \
  --name=qbittorrent \
  --network=host \
  --restart=unless-stopped \
  -v /root/qbt/config/config:/config \
  -v /root/qbt/dwn/downloads:/downloads \
  -e TZ=Asia/Shanghai \
  -e WEBUI_PORT=9090 \
  linuxserver/qbittorrent:14.3.9
```