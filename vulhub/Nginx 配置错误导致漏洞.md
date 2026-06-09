# Nginx 配置错误导致漏洞

## 运行测试环境

```plain
docker compose up -d
```

运行成功后，Nginx将会监听8080/8081/8082三个端口，分别对应三种漏洞。

## Mistake 1. CRLF注入漏洞

Nginx会将$uri进行解码，导致传入%0d%0a即可引入换行符，造成CRLF注入漏洞。

错误的配置文件示例（原本的目的是为了让http的请求跳转到https上）：

```plain
location / {
    return 302 https://$host$uri;
}
```

Payload: http://your-ip:8080/%0d%0aSet-Cookie:%20a=1，可注入Set-Cookie头。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893653977-11ae6d90-0c9e-45a7-b0d4-0d37f486befc.png)

利用《[Bottle HTTP 头注入漏洞探究](https://www.leavesongs.com/PENETRATION/bottle-crlf-cve-2016-9964.html#tdsub)》中的技巧，即可构造一个XSS漏洞：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893653968-d8dee8e9-ccd1-435e-a730-88fce0564de0.png)

## Mistake 2. 目录穿越漏洞

Nginx在配置别名（Alias）的时候，如果忘记加/，将造成一个目录穿越漏洞。

错误的配置文件示例（原本的目的是为了让用户访问到/home/目录下的文件）：

```plain
location /files {
    alias /home/;
}
```

Payload: http://your-ip:8081/files../，成功穿越到根目录：

## ![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893653983-a2db98b0-49c0-4c5d-8e44-24cd3b656aa5.png)

## Mistake 3. add_header被覆盖

Nginx配置文件子块（server、location、if）中的add_header，将会覆盖父块中的add_header添加的HTTP头，造成一些安全隐患。

如下列代码，整站（父块中）添加了CSP头：

```plain
add_header Content-Security-Policy "default-src 'self'";
add_header X-Frame-Options DENY;

location = /test1 {
    rewrite ^(.*)$ /xss.html break;
}

location = /test2 {
    add_header X-Content-Type-Options nosniff;
    rewrite ^(.*)$ /xss.html break;
}
```

但/test2的location中又添加了X-Content-Type-Options头，导致父块中的add_header全部失效：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893654234-bbf7f6f2-0fdb-4202-b67f-60968ee20c61.png)

XSS可被触发：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893654409-5eaa9ca4-1e56-4cf9-9cc6-5a0be76818f7.png)