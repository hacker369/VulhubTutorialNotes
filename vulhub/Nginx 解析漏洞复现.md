# Nginx 解析漏洞复现

版本信息：

lNginx 1.x 最新版

lPHP 7.x最新版

由此可知，该漏洞与Nginx、php版本无关，属于用户配置不当造成的解析漏洞。

直接执行docker compose up -d启动容器，无需编译。

访问http://your-ip/uploadfiles/nginx.png和http://your-ip/uploadfiles/nginx.png/.php即可查看效果。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803117792-646e229f-60c2-44a2-9204-627631e0c717.png)

增加/.php后缀，被解析成PHP文件：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803126649-735c5da4-d01f-4eb4-82fd-9ca6592e414b.png)

访问http://your-ip/index.php可以测试上传功能，上传代码不存在漏洞，但利用解析漏洞即可getshell：

上传一个图片马，里面包含了一个一句话木马

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803126500-50023fcc-d24b-4805-a754-fc81d6b5a402.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136012-e51d233b-8a47-4da7-8f2e-0a8b452e9064.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136273-5f5b41df-b5d9-47ca-9819-e0272606dc65.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136168-99dc9d47-feb4-4619-ae59-24d4a87c246b.png)

访问该图片成功上传

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164296-d3669b29-bf84-48f0-93bf-fd360887a2fc.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164227-03deca31-12af-4f29-9c19-cfd209b216f7.png)

然后复制url链接后面加上/.php使用蚁剑成功连接

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164094-99389e2b-0c4d-40fc-a9f2-07e2f039cac6.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164244-8be166b2-626b-432b-b0a4-d5ecd7acc566.png)