+++
title = 'CentOS7搭建Uptime Kuma，实时监控你的个人网站状态'
description = '自建一款监控个人网站的工具'
date = 2024-10-11T00:00:32+08:00
tags = [
    "Uptime Kuma",
    "运维",
    "监控"
]
+++

# 服务器部署一款开源的监控工具：[Uptime Kuma](https://github.com/louislam/uptime-kuma)
---

> **前置条件：在服务器安装完成`Docker`**


1. 创建安装目录

```bash
sudo -i

mkdir -p /root/data/docker_data/uptime_kuma
```

2. 编辑`docker-compose.yml`文件

```bash
cd /root/data/docker_data/uptime_kuma

vim docker-compose.yml

# 输入安装脚本,以下版本为1.23.X版本，如需最新版本，需自行修改
version: '3.3'

services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    volumes:
      - ./uptime-kuma-data:/app/data
    ports:
      - 3001:3001  # <Host Port>:<Container Port>
    restart: always
```

3. 运行安装命令

```bash
docker compose up -d

# 打开防火墙3001d
```

4. 输入http://服务器地址:3001 访问

![image-20241011005920166](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110059112.png)

![image-20241011005823115](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110058830.png)
