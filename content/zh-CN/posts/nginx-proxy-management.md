+++
title = '部署Nginx Proxy Manager，一款开源的Nginx可视化应用'
description = '服务器部署Nginx Proxy Manager，可视化你的Nginx操作，轻松配置和反向代理Https'
date = 2024-10-11T00:00:32+08:00
tags = [
    "Nginx Proxy Manager",
    "运维",
    "Nginx"
]
+++

# 自建[Nginx Proxy Manager](https://nginxproxymanager.com/guide/#quick-setup)
---

> **前置条件：在服务器安装完成`Docker`**

## 部署NPM

1.  创建安装NPM的目录，例如：

```bash
sudo -i
mkdir -p /root/data/docker_data/npm
```

2. 编辑`docker-compose.yml`文件

```bash
cd /root/data/docker_data/npm

vim docker-compose.yml

# 输入安装脚本
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'  # 保持默认即可，不建议修改左侧的80
      - '81:81'  # 冒号左边可以改成自己服务器未被占用的端口
      - '443:443' # 保持默认即可，不建议修改左侧的443
    volumes:
      - ./data:/data # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 data 文件夹中
      - ./letsencrypt:/etc/letsencrypt  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 letsencrypt 文件夹中


# 记得打开服务器防火墙端口号 81，查看端口是否被占用（以 81 为例），输入：
lsof -i:81  #查看 81 端口是否被占用，如果被占用，重新自定义一个端口
#如果啥也没出现，表示端口未被占用
```

3. 安装NPM

```bash
cd /root/data/docker_data/npm
docker compose up -d
```

4. 输入 [http://ip:81](http://ip:81/) 访问

```bash
默认登陆名和密码：

Email: admin@example.com
Password: changeme
```

## 更新 Nginx Proxy Manager

```bash
cd /root/data/docker_data/npm

docker compose down

cp -r /root/data/docker_data/npm /root/data/docker_data/npm.archive #备份一份数据

docker compose pull

docker compose up -d

docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```

## 卸载 Nginx Proxy Manager

```bash
cd /root/data/docker_data/npm

docker compose down

rm -rf /root/data/docker_data/npm
```

## Nginx Proxy Manager添加反向代理

![image-20241011001526417](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110015772.png)

![image-20241011001620277](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110016029.png)

![image-20241011001840586](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110018054.png)



## Nginx Proxy Manager配置Https

1. **第一种：使用免费的证书服务**

![image-20241011002052102](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110020881.png)

2. **第二种：上传自己的证书，上传成功后到`Hosts`配置，即可看到自己上传的证书**

![image-20241011002319913](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410110023003.png)


> **如果需要配置https，点击SSL，选择证书和勾选需要的配置即可，不需要修改Proxy Hots里的http为https**
