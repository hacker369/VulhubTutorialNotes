# Apache HTTPD 多后缀解析漏洞

## 漏洞环境

运行如下命令启动一个稳定版Apache，并附带PHP 7.3环境：

```plain
docker compose up -d
```

## 漏洞复现

环境运行后，访问http://your-ip/uploadfiles/apache.php.jpeg即可发现，phpinfo被执行了，该文件被解析为php脚本。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879077728-f669dc2c-d59d-4e70-aa1b-b954974bbae8.png)

http://your-ip/index.php中是一个白名单检查文件后缀的上传组件，上传完成后并未重命名。我们可以通过上传文件名为xxx.php.jpg或xxx.php.jpeg的文件，利用Apache解析漏洞进行getshell。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879077592-363f90e8-54d4-4e6d-bbbc-adc37a5c182c.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879077617-5d95d3eb-2470-4a1e-812d-b5222c4585a0.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879077772-cff57ca0-4926-4998-82dd-cc8b41824f0f.png)