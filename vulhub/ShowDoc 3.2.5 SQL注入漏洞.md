# ShowDoc 3.2.5 SQL注入漏洞

## 漏洞环境

执行如下命令启动一个ShowDoc 2.8.2服务器：

```plain
docker compose up -d
```

服务启动后，访问http://your-ip:8080即可查看到ShowDoc的主页。初始化成功后，使用帐号showdoc和密码123456登录用户界面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907821410-9abd0bc2-7c69-4106-9857-3883a54118c7.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907821431-1961d435-ad19-4789-9e77-8afda0499b91.png)

## 漏洞复现

一旦一个用户登录进ShowDoc，其用户token将会被保存在SQLite数据库中。相比于获取hash后的用户密码，用户token是一个更好地选择。

在利用该漏洞前，需要安装验证码识别库，因为该漏洞需要每次请求前传入验证验：

```plain
pip install onnxruntime ddddocr requests
```

然后，执行[这个POC](https://github.com/vulhub/vulhub/blob/master/showdoc/3.2.5-sqli/poc.py#tdsub)来获取token：

```plain
python3 poc.py -u http://localhost:8080
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907821433-cf46c39b-dc72-46b2-ab7e-ae58494c146e.png)

测试一下这个token是否是合法的：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780907821340-a101a272-7028-4826-9b45-2e380b03a81a.png)