>gitter地址:    [gitter](https://gitter.im/)
sidecar地址:   [sidecar](https://sidecar.gitter.im/)


>Gitter 是 GitHub 存储库的开发人员和用户的即时通讯聊天室系统。 Gitter 作为软件即服务提供商，提供包括免费选项和所有基本功能，以及创建单个私人聊天室的能力，和个人和组织的付费订阅选项，允许他们创建任意数量的私人聊天室。
该服务可以为 GitHub 上的各个 Git 存储库创建个人聊天室。聊天室隐私遵循关联 GitHub 存储库的隐私设置：因此，私有的 GitHub 存储库的聊天室对于访问存储库的人员也是私有的。
它还可以将连接到聊天室的地址信息放置在 git 存储库的 README 文件中，以引起项目所有用户和开发人员的注意。用户也可以通过 GitHub 登录 Gitter 访问他们访问的存储库的私人聊天室。（注意： GitHub 密码是不与 Gitter 共享）
Gitter 类似于 IRC 和 Slack。但与 IRC 不同的是，它像Slack一样，会将所有聊天记录存档至云端。

# 1.访问 Gitter 官网，并注册用户
访问gitter官网 [gitter](https://gitter.im/)  进行登录
![jhiu8765redfv](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jhiu8765redfv.6v8rnyrruto0.webp)
注册用户，这里支持 GitLab，GitHub, Twitter 三种方式来授权登录，本人选的是 GitHub。
![tgf5edvoi876rtdc4e](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tgf5edvoi876rtdc4e.4tu3cmwgby00.webp)
# 2.创建自己的聊天室
点击左侧加号先创建社区，再创建房间
![jmiujytre45r](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jmiujytre45r.7esor7dr1000.webp)

### 创建社区

![hnuj87654erfgt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hnuj87654erfgt.3czv7m42m3w0.webp)

### 创建房间

![sde4r5t6yytgrf](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/sde4r5t6yytgrf.67jrw66gl9w0.webp)

点击create完成房间的创建。

# 2.配置房间
![dwdececfwf34f](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/dwdececfwf34f.75a16o30o240.webp)

点击more

![xxxgfvfrexcfrwdc](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/xxxgfvfrexcfrwdc.8z2rhluye68.webp)

点击发送拉去请求后，前往仓库同意一下，回到房间已经添加了一个成员。
![vfrgtecvfrgtervgtr544](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/vfrgtecvfrgtervgtr544.x85nl9fgwls.webp)

# 3.借助 Sidecar 集成 gitter 到个人网站
>Sidecar 能够帮助你快速便捷的集成 gitter, 仅仅需要添加几行 javascript 代码即可，开箱即用，你还可以通过一些配置来自定义它。

进入Sidecar 官方网站 [sidecar](https://sidecar.gitter.im)

![hji9876rcfvgthy](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hji9876rcfvgthy.201gjv9o46kg.webp)

>将之前标注的配置房间中画圈的复制过来进行粘贴，类型为   用户名/房间名称

然后复制 javascript 代码，集成到个人网站中：
```javascript
<script>
    ((window.gitter = {}).chat = {}).options = {
      room: 'quinhua/webchat'
    };
  </script>
  <script src="https://sidecar.gitter.im/dist/sidecar.v1.js" async defer></script>
```

![gtrfer567ygtfred](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gtrfer567ygtfred.6dc4hjzedsk0.webp)

然后回到房间内可以看到刚才发送的话在房间内也显示了。