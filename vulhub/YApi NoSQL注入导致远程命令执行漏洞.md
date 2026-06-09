# YApi NoSQL注入导致远程命令执行漏洞

## 漏洞环境

执行如下命令启动一个YApi v1.10.2服务：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:3000即可看到YApi首页。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968158393-219d27bb-3b38-46c7-9dce-7dc794f3a717.png)

## 漏洞复现

本漏洞的利用需要YApi应用中至少存在一个项目与相关数据，否则无法利用。Vulhub环境中的YApi是一个即开即用、包含测试数据的服务器，所以可以直接进行漏洞复现。

使用[这个POC](https://github.com/vulhub/vulhub/blob/master/yapi/mongodb-inj/poc.py#tdsub)来复现漏洞：

```plain
python poc.py --debug one4all -u http://127.0.0.1:3000/
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968158471-ebc48eca-bcc0-4228-be2c-93916cd52195.png)