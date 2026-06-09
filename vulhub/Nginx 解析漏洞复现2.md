# Nginx 解析漏洞复现

Nginx解析漏洞复现。

版本信息：

lNginx 1.x 最新版

lPHP 7.x最新版

由此可知，该漏洞与Nginx、php版本无关，属于用户配置不当造成的解析漏洞。

直接执行docker compose up -d启动容器，无需编译。

访问http://your-ip/uploadfiles/nginx.png和http://your-ip/uploadfiles/nginx.png/.php即可查看效果。

正常显示：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893905606-3e5baf0c-670d-42d0-ab22-d160fef050d1.png)

增加/.php后缀，被解析成PHP文件：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893905894-5469b9e9-a887-474b-87ad-4ce7493d47fb.png)

访问http://your-ip/index.php可以测试上传功能，上传代码不存在漏洞，但利用解析漏洞即可getshell：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893905811-8eb5f38b-0374-4734-b8d9-95b1c4ac2992.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893905935-0aa6da85-1c8f-4b10-81f6-56842630dfb7.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893905897-7a56fd3f-124a-4068-bcda-f920a5bc36c5.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893906765-ca79aefc-c191-4968-858a-e1e407fbf15f.png)

反弹shell

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893907119-325ea7f6-41bc-44ce-96c6-4f50ee38d7a1.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893907344-be04233d-dd25-43a5-9550-70ef6400a290.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780893908066-af0065b3-3c87-4fd2-bbd0-874535da8a18.png)