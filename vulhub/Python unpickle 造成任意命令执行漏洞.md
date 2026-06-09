# Python unpickle 造成任意命令执行漏洞

## 测试

编译及运行测试环境：

```plain
docker compose build
docker compose up -d
```

访问http://your-ip:8000，显示Hello {username}!。username是取Cookie变量user，对其进行base64解码+反序列化后还原的对象中的“username”变量，默认为“Guest”，伪代码：pickle_decode(base64_decode(cookie['user']))['username'] or 'Guest'。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780898118725-8a77878c-7aed-427c-ab38-344933770117.png)

调用exp.py，反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780898118796-ef590158-3d72-498f-b565-8e4b7ea02439.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780898119258-e49b2681-d5a3-45bb-8704-6815805fac97.png)