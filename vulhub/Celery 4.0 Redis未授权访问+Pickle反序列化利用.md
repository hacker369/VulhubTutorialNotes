# Celery <4.0 Redis未授权访问+Pickle反序列化利用

## 漏洞环境

执行如下命令启动Celery 3.1.23 + Redis：

```plain
docker compose up -d
```

## 漏洞复现

漏洞利用脚本exploit.py仅支持在python3下使用

```plain
pip install redis 
python exploit.py [主机IP]
```

查看结果：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213485-d46430e0-67ab-410b-9c04-c19c3d5649ab.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213663-96980268-196b-4e31-b7e4-754523c07c56.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213567-a1e5b8a4-28f7-461c-9cfa-6f4dcbe99912.png)

可以看到如下任务消息报错：

```plain
docker compose exec celery ls -l /tmp
```

可以看到成功创建了文件celery_success

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213453-b814b507-9148-4f04-a17c-591f4c69068d.png)

# 