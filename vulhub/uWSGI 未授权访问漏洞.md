# uWSGI 未授权访问漏洞

## 漏洞环境

执行如下命令启动nginx+uwsgi环境：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可查看一个Web应用，其uwsgi暴露在8000端口。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964767559-e71eb923-5c7c-4f69-8a8d-97bdc79e3843.png)

## 漏洞复现

使用[poc.py](https://github.com/vulhub/vulhub/blob/master/uwsgi/unacc/poc.py#tdsub)，执行命令

```plain
python poc.py -u your-ip:8000 -c "touch /tmp/success"：
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964767442-06ca95a0-1669-4324-8216-1572bf2b2096.png)

执行docker compose exec web bash进入容器，可见/tmp/success已经成功执行：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964767460-f0b90511-5aee-46ed-b6b7-58f0f9cf3c60.png)