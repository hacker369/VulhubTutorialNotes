# kkFileView ZipSlip 远程命令执行漏洞

## 漏洞环境

执行如下命令启动一个kkFileView 3.4.0服务器：

```plain
docker compose up -d
```

服务启动后，访问http://your-ip:8012即可查看到首页。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133166-8d2fd6da-5c79-4263-ac02-c39348349f66.png)

## 漏洞复现

首先，修改并执行[poc.py](https://github.com/vulhub/vulhub/blob/master/kkfileview/4.3-zipslip-rce/poc.py#tdsub)，生成POC文件：

```plain
python poc.py
```

然后，test.zip将被写入到当前目录下。

上传test.zip和[sample.odt](https://github.com/vulhub/vulhub/blob/master/kkfileview/4.3-zipslip-rce/sample.odt#tdsub)两个文件到kkFileView服务中：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133188-e24f689f-2271-4828-8d6f-fbc596912cd1.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133065-f70b6c84-d4cb-45c5-b60a-32e0279d774b.png)

然后，点击test.zip的“预览”按钮，可以看到zip压缩包中的文件列表：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133045-26347707-a1f5-42f5-bd3e-43541cd23d22.png)

最后，点击sample.odt的“预览”按钮，触发代码执行漏洞。

可见，touch /tmp/success已经成功被执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133336-7a6dac79-ab14-4ee0-bf5a-177096c64020.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780889133804-b4183bbc-ecbf-4dce-b122-637522431ea4.png)