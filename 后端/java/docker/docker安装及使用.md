## 1.安装步骤

[Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)

### 1. 确定CenOS7及以上版本

```dockerfile
cat /etc/redhat-release
```

### 2.卸载旧版本

```dockerfile
yum remove docker \
       docker-client \
       docker-client-latest \
       docker-common \
       docker-latest \
       docker-latest-logrotate \
       docker-logrotate \
       docker-engine
```

### 3.yum安装gcc相关

```dockerfile
yum -y install gcc

yum -y install gcc-c++
```

### 4.安装所需软件包

```dockerfile
yum install -y yum-utils
```

### 5.设置stable镜像仓库

```dockerfile
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 6.更新yum软件包索引

```dockerfile
yum makecache fast
```

### 7.安装DOCKER CE

```dockerfile
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 8.查看docker版本

```dockerfile
docker -v
# 或
docker version
```

### 9.启动docker

```dockerfile
# 启动之前关闭防火墙
systemctl stop firewalld
# 禁止开机启动防火墙
systemctl disable firewalld

# 启动docker服务 
systemctl start docker
```

### 10.测试

```dockerfile
docker run hello-world
```

### 11.卸载

```dockerfile
systemctl stop docker

yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

rm -rf /var/lib/docker

rm -rf /var/lib/containerd
```

### 12.配置阿里云镜像加速

前往 [容器镜像服务 (aliyun.com)](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors) 进入容器镜像服务，获取自己的容器镜像服务加速地址

![gva3wer54hu5retw6gshws4u](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gva3wer54hu5retw6gshws4u.1qfggu6uofr4.webp)

```dockerfile
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://你自己的.mirror.aliyuncs.com"]
}
EOF

systemctl daemon-reload

systemctl restart docker
```

> 拉取镜像会报错，出现此问题的原因为未进行`身份识别` ，前往`个人实例` 中找到 `访问凭证`，先设置登录密码再执行命令

![image-20230328113053676](http://rraq343o3.hn-bkt.clouddn.com/markdown/202303281130231.png)

![image-20230328113540321](http://rraq343o3.hn-bkt.clouddn.com/markdown/202303281135632.png)

## 2.docker命令

### 1.帮助启动类命令

#### 1.启动

```dockerfile
systemctl start docker
```

#### 2.停止

```dockerfile
systemctl stop docker
```

#### 3.重启

```dockerfile
systemctl restart docker
```

#### 4.查看状态

```dockerfile
systemctl status docker
```

#### 5.查看版本

```dockerfile
docker -v
# 或
docker version
```

#### 6.开机启动

```dockerfile
systemctl enable docker
```

#### 7.查看概要信息

```dockerfile
docker info
```

#### 8.查看总体帮助文档 

```dockerfile
docker --help
```

#### 9.查看命令帮助文档

```dockerfile
docker 具体命令 --help
```

### 2.镜像命令

#### 1.列出本地主机上的镜像

- REPOSITORY  镜像的数据源

- TAG                  镜像的标签版本号
- IMAGE ID        镜像ID
- CREATED         镜像创建的时间
- SIZE		          镜像大小

```dockerfile
docker images

-a :列出本地所有的镜像（含历史映像层）
docker images -a

-q :只显示镜像ID
docker images -q
```

#### 2.搜索镜像

```dockerfile
docker search 镜像名字

# 只列举出n个镜像，默认25个
docker search 镜像名字 --limit
docker search 镜像名字 --limit n

# 查找关注度大于n的某个镜像
docker search --filter=stars=n 镜像名称     
```

#### 3.拉取镜像

```dockerfile
docker pull 某个镜像名字

# 拉取某个镜像最新版
docker pull 某个镜像名字 latest

# 镜像可能有多个TAG版本，拉取指定TAG版本镜像，没有就是最新版
docker pull 某个镜像名字:TAG
```

#### 4.查看镜像/容器/数据卷所占用的空间

```dockerfile
docker system df 
```

#### 5.删除镜像

```dockerfile
docker rmi 容器Id或容器名称

# 强制删除
docker rmi -f 容器Id或容器名称

# 同时删除多个镜像
docker rmi -f 名字/ID1 名字/ID2 名字/ID3 ...

# 删除全部镜像
docker rmi -f $( docker images -qa )
```

### 3.容器命令

#### 1.新建+启动命令

- `–name=“容器新名字”` 为容器指定一个名称；
- `-d`: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；

- `-i`：以交互模式运行容器，通常与 -t 同时使用；
- `-t`：为容器重新分配一个伪输入终端，通常与 -i 同时使用；启动交互式容器(前台有伪终端，等待交互)；

- `-P`: 随机端口映射，大写P
- `-p`: 指定端口映射，小写p

![xxx](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/xxx.1ecn1y9oqv0g.webp)

```dockerfile
# 以交互模式启动容器，交互式Shell，使用/bin/bash，要退出终端直接输入 exit
docker run -it 容器名称 /bin/bash
# -docker run -it centos /bin/bash 
# -docker run -it ubuntu /bin/bash 

# 别名
docker run -it --name=别名 容器Id或容器名称
```

#### 2.列出当前所有正在运行的容器

```dockerfile
docker ps

# 列出当前所有正在运行的容器+历史上运行过的
docker ps -a

# 显示最近创建的容器

# 显示最近n个创建的容器
docker ps -n 个数

# 静默模式，只显示容器编号
docker ps -q
```

#### 4.退出容器

```dockerfile
# 退出容器停止 
exit

# 退出容器不停止 
ctrl + P + Q
Ctrl + Shift + P + Q
```

#### 5.启动已停止运行的容器

```dockerfile
docker start 容器Id或容器名称
```

#### 6.重启容器

```dockerfile
docker restart 容器Id或容器名称
```

#### 7.停止容器

```dockerfile
docker stop 容器Id或容器名称
```

#### 8.强制停止容器

```dockerfile
docker kill 容器Id或容器名称
```

#### 9.删除已停止的容器

```dockerfile
docker rm 容器Id或容器名称

# 同时删除多个
docker rm -f $( docker ps -a -q )
```

#### 10.以守护方式启动容器[后台]

> docker ps查看一下启动成功与否，有的容器后台运行必须有一个前台进程，比如`ubuntu`，要使用命令`docker run -it ubuntu`,而`redis`则可以直接后台运行
>
> 容器运行的命令如果不是那些一直挂起的命令(比如运行top，tail)，就会自动退出的

```dockerfile
docker run -d 容器Id或容器名称 [并返回容器ID]
```

#### 11.查看容器日志

```dockerfile
docker logs 容器Id

# 查看redis容器日志，参数：-f  跟踪日志输出；-t   显示时间戳；--tail  仅列出最新N条容器日志；
docker logs -f -t --tail=n 容器Id或容器名称

# 查看容器从某时间后的最新n条日志。
docker logs --since="xxxx-xx-xx" --tail=n 容器Id或容器名称
```

#### 12.查看容器进程

```dockerfile
# 所有
docker ps

# 指定
docker top 容器Id
```

#### 13.查看容器详细信息

```dockerfile
docker inspect 容器Id或容器名称
```

#### 14.进入正在运行的容器并以命令行交互

```dockerfile
# exec 是在容器中打开行的容器，可以启动新的进程，用 exit 退出不会导致容器的停止   [推荐使用]
docker exec -it 容器id /bin/bash    [或 bashShell]

# attach 直接进入容器启动命令的终端，不会启动新的进程，用 exit 会导致容器的停止
docker attach 容器id /bin/bash  [或 bashShell]
```

#### 15.从容器内拷贝文件到主机上

```dockerfile
docker cp  容器ID:容器内路径 目的主机路径

# [root@07ea64240119] @ 后为容器ID
```

#### 16.导入与导出

```dockerfile
# 导入
cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号

# 导出
docker export 容器ID > 文件名.tar
```

#### 17.commit编辑操作

> docker commit提交一个新的容器副本使之成为一个新的镜像，类似于java反射

```dockerfile
# 获取容器ID,运行 docker ps 查看容器ID
docker ps
docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]

# ubuntu安装vim
apt-get update
apt-get -y install vim
```