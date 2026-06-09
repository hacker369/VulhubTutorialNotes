# VulhubTutorialNotes
This is a tutorial note on how I cleared the Vulhub target range, including pictures and text.
这是关于本人通关Vulhub靶场的一些教程笔记心得，包含图文。
在打靶场的时候也遇到了各种奇奇怪怪的问题,当然也是我上网搜索资料后解决了.遂在这里记录一下.

Vulhub靶场总体来说是个不错的靶场,很适合网安萌新学到一定程度来练习,通过使用docker去一键部署靶场环境,省去了繁琐的步骤,真的的做到了一键开箱即用.模拟真实的实战环境.当然靶场也在不断的更新案例.希望你们在靶场里面玩的开心,水平能上升到一定的程度.

对 dev 分支下面的 README.md 进行文本内容添加修改。

# Vulhub镜像清理

在学习的过程中，会不断的拉取镜像，随着拉取的镜像越来越多，有可能会出现空间使用不足，这个时候就可以把一些不用的镜像清理掉。

## 1. 停止并删除 Docker 容器和网络

首先，确保所有与 Vulhub 相关的 Docker 容器都已经停止并删除。你可以使用以下命令：

```
# 首先进入你的vulhub目录下
cd vulhub

# 停止并删除所有容器
docker-compose down -v

# 删除所有未使用的镜像、容器、网络和数据卷
docker system prune -a -f
docker volume prune -f 
```

## 2. 删除 Vulhub 文件

删除 Vulhub 的文件和目录。假设你将 Vulhub 克隆到 /path/to/vulhub，你可以使用以下命令删除它：

```
rm -rf /path/to/vulhub
```

## 3. 卸载 Docker 和 Docker Compose

如果你不再需要 Docker 和 Docker Compose，可以使用以下命令卸载它们：

卸载 Docker

```
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
```

卸载 Docker Compose

Docker Compose 通常是通过下载的二进制文件来安装的，可以使用以下命令删除：

```
sudo rm /usr/local/bin/docker-compose
```

## 4. 清理残留数据

删除 Docker 的残留数据和配置文件：

```
sudo rm -rf /var/lib/docker
sudo rm -rf /etc/docker
sudo rm -rf /etc/systemd/system/docker.service.d
sudo rm -rf /var/run/docker.sock
```

重启系统以确保所有更改生效：

```
sudo reboot
```
