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


# Seedbox Installation Script
## 用法
`bash <(wget -qO- https://raw.githubusercontent.com/jerry048/Dedicated-Seedbox/main/Install.sh) -u <用户名称> -p <密码> -c <缓存大小(单位:MiB)> -q <qBittorrent 版本> -l <libtorrent 版本> -b -v -r -3 -x -o`
#### Options
	1. -u: 用户名称
	2. -p: 密码
	3. -c: qBitorrent 的缓存大小
	4. -q: qBittorrent 版本
	5. -l: libtorrent 版本
	6. -b: 安装autobrr
	7. -v: 安装vertex
	8. -r: 安装 autoremove-torrents
	9. -3: 启动 BBR V3
	10.-x: 启动 BBRx
	11.-o: 自定义端口
#### 范例
`bash <(wget -qO- https://raw.githubusercontent.com/jerry048/Dedicated-Seedbox/main/Install.sh) -u jerry048 -p 1LDw39VOgors -c 3072 -q 4.3.9 -l v1.2.19 -v -x`

##### 解释
	1. 用户名称 是 jerry048
	2. 密码 是 1LDw39VOgors
	3. 缓存大小 是 3GB
	4. 安装 qBittorrent 4.3.9 - libtorrent-v1.2.19
	6. 安装 vertex
	7. 启动 BBRx
## 支持平台
	1. 系统
		1. Debian 10+
		2. Ubuntu 20.04+
	
	2. CPU 架构
		1. x86_64
		2. ARM64

## 功能
###### 1. 盒子环境
	1.qBittorrent
	2.autobrr
	3.vertex
	4.autoremove-torrents
###### 2. 优化
	处理器优化
	网络优化
	内核参数调配
	硬盘优化
	BBR V3 或 BBRx
### 进阶优化备注
- 缓存大小应该设置在机器内存大小的 1/4 左右. 如果你使用的是qBittorrent 4.3.x, 你需要考虑到内存溢出的问题并且设置缓存大小在机器内存大小的 1/8. 

- 异步 I/O 线程数的基础设置是 4， 这设置对HDD比较友好. 如果你使用的是SSD甚至是NVMe的话, 你可以调整此参数到 8 甚至到 16. 
	- 在qBittorrent 4.3.x 的话，你可以在高级选项栏目中更改此项设置. 
	- 在qBittorrent 4.1.x 的话, 你可以在 /home/$username/.config/qBittorrent/qBittorrent.conf 里的 [BitTorrent] 栏目下加入 `Session\AsyncIOThreadsCount=8`
		- 请在修改前关闭qBittorrent
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设置
		- aio_threads=8

- 在一些 I/O 较差的机器，send_buffer_low_watermark, send_buffer_watermark & send_buffer_watermark_factor 这三项设置应该调低
	- 在qBittorrent 4.3.x 的话，你可以在高级选项栏目中更改此项设置. 
	- 在qBittorrent 4.1.x 的话，你可以在 /home/$username/.config/qBittorrent/qBittorrent.conf 里的 [BitTorrent] 栏目下加入`Session\SendBufferWatermark=5120`, `Session\SendBufferLowWatermark=1024`和 `ession\SendBufferWatermarkFactor=150`
		- 请在修改前关闭qBittorrent
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设置
		- send_buffer_low_watermark=1048576
		- send_buffer_watermark=5242880
		- send_buffer_watermark_factor=150

- 在一些 CPU 较差的机器，tick_internal 应该调高来节省CPU指令周期
	- qBittorrent 暂时还没为修改这设置
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设置
		- tick_interval=250

- 在/etc/sysctl.conf 设置的 TCP 缓存大小对于一些低端机器来说可能会太大。 请根据情况更改.
	- 在 /etc/sysctl.conf 文档中也能找到别的优化备注

- 文件系统的话, 本人强烈推荐使用 XFS