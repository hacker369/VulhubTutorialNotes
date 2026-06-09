# Jupyter Notebook 未授权访问漏洞

## 漏洞影响

```plain
Jupyter Notebook
```

## 网络测绘

```plain
app="Jupyter-Notebook" && body="Terminal"
```

## 环境搭建

Vulhub运行测试环境：

```plain
docker-compose up -d
```

运行后，访问http://your-ip:8888将看到Jupyter Notebook的Web管理界面，并没有要求填写密码。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780888240619-2d46b815-cd27-4913-8b3d-cb56b1fd54e2.png)

## 漏洞复现

选择 new -> terminal 即可创建一个控制台：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780888240575-7d5cabdb-0e86-41e6-8cae-067c7b3353da.png)

直接执行任意命令：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780888240744-9fd0dbde-d6d3-483f-bb58-84fb205e7a3a.png)

执行命令并反弹shell：

监听21端口，成功接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780888240668-e93ea218-3863-4639-a233-a33bc156152e.png)