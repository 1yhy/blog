---
title: 'CentOS 7安装Docker'
description: '在CentOS 7上安装Docker'
date: 2024-10-10
---

# Centos7安装Docker
---
1. 如果之前有装过Docker,建议先卸载旧版本相关依赖

```bash
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine
```

2. 安装

```bash
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# Step 3: 更新并安装 Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce docker-ce-cli containerd.io

# Step 4: 开启Docker服务并设置它在系统启动时自动启动
sudo systemctl start docker
sudo systemctl enable docker

# Step 5: 验证 Docker 是否安装成功并查看其版本
docker --version
```

3. 安装完成后，最新版自带了`docker compose`命令，可以无需安装`docker-compose`
4. 开启阿里云镜像加速（非必须）

- 进入[阿里云控制台首页](https://aliyun.com/)
- 进入管理控制台 -> 登录账号 -> 左上角控制台 -> 产品与服务 -> 容器 -> 容器镜像服务 -> 点击侧边栏镜像工具 -> 镜像加速器 -> 复制链接

![image-20241010235437889](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410102354031.png)

![image-20241010235331386](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410102353167.png)

![image-20241010235543013](https://yhyblog-2023-2-8.oss-cn-hangzhou.aliyuncs.com/md/2024/202410102355323.png)

- 编辑 daemon 配置文件`vim /etc/docker/daemon.json`替换加速器地址保存

```bash
{
  "registry-mirrors": ["https://**********.mirror.aliyuncs.com"]
}
```

- 重启 docker

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```
