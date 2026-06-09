# Hadoop YARN ResourceManager 未授权访问

## 测试环境

运行测试环境

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8088即可看到Hadoop YARN ResourceManager WebUI页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878613878-ef24aa6a-cdd7-4b75-ba97-893b7b8862fc.png)

## 利用

利用方法和原理中有一些不同。在没有 hadoop client 的情况下，直接通过 REST API ([https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/ResourceManagerRest.html](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/ResourceManagerRest.html#tdsub)) 也可以提交任务执行。

利用过程如下：

1.在本地监听等待反弹 shell 连接

2.调用 New Application API 创建 Application

3.调用 Submit Application API 提交

参考 [exp 脚本](https://github.com/vulhub/vulhub/blob/master/hadoop/unauthorized-yarn/exploit.py#tdsub)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878613968-407808f1-9494-40b5-843d-6e6e4fbf29c1.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878614080-3d66457c-326e-4759-9556-dddc61131dc1.png)