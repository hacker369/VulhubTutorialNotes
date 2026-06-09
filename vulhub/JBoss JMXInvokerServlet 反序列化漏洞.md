# JBoss JMXInvokerServlet 反序列化漏洞

## 漏洞环境

启动漏洞环境

```plain
docker compose up -d
```

首次执行时会有1~3分钟时间初始化，初始化完成后访问http://your-ip:8080/即可看到JBoss默认页面。

## 漏洞复现

JBoss在处理/invoker/JMXInvokerServlet请求的时候读取了对象，所以我们直接将[ysoserial](https://github.com/frohoff/ysoserial#tdsub)生成好的POC附在POST Body中发送即可。整个过程可参考[jboss/CVE-2017-12149](https://github.com/vulhub/vulhub/tree/master/jboss/CVE-2017-12149#tdsub)，我就不再赘述。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780886569608-4ba21a7c-8fb7-4ef5-b78d-d6a2ad0cacc1.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780886569713-50bece5f-f450-4eb8-a75c-a28bf13e4ed2.png)

网上已经有很多EXP了，比如[DeserializeExploit.jar](https://cdn.vulhub.org/deserialization/DeserializeExploit.jar#tdsub)，直接用该工具执行命令、上传文件即可：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780886569839-52feda7c-d9b1-4e35-9153-bf0a492b46a5.png)