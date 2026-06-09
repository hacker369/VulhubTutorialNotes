# XDebug 远程调试漏洞（代码执行）

## 测试环境

编译及启动测试环境

```plain
docker compose build
docker compose up -d
```

启动完成后，访问http://your-ip:8080/即可发现主页是一个简单的phpinfo，在其中可以找到xdebug的配置，可见开启了远程调试。

## 漏洞利用

因为需要使用dbgp协议与目标服务器通信，所以无法用http协议复现漏洞。

我编写了一个[漏洞复现脚本](https://github.com/vulhub/vulhub/blob/master/php/xdebug-rce/exp.py#tdsub)，指定目标web地址、待执行的php代码即可：

```plain
# 要求用python3并安装requests库
python3 exp.py -t http://127.0.0.1:8080/index.php -c 'shell_exec('id');'
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896974935-0fab944b-04ee-4114-ae9e-d846fd132f48.png)

**重要说明：因为该通信是一个反向连接的过程，exp.py启动后其实是会监听本地的9000端口（可通过-l参数指定）并等待XDebug前来连接，所以执行该脚本的服务器必须有外网IP（或者与目标服务器处于同一内网）。**