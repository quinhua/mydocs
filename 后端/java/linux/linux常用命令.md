## 1.Linux系统目录

```xml
├── bin -> usr/bin # 用于存放二进制命令
├── boot # 内核及引导系统程序所在的目录
├── dev # 所有设备文件的目录（如磁盘、光驱等）
├── etc # 配置文件默认路径、服务启动命令存放目录
├── home # 用户家目录，root用户为/root
├── lib -> usr/lib # 32位库文件存放目录
├── lib64 -> usr/lib64 # 64位库文件存放目录
├── media # 媒体文件存放目录
├── mnt # 临时挂载设备目录
├── opt # 自定义软件安装存放目录
├── proc # 进程及内核信息存放目录
├── root # Root用户家目录
├── run # 系统运行时产生临时文件，存放目录
├── sbin -> usr/sbin # 系统管理命令存放目录
├── srv # 服务启动之后需要访问的数据目录
├── sys # 系统使用目录
├── tmp # 临时文件目录
├── usr # 系统命令和帮助文件目录
└── var # 存放内容易变的文件的目录
```

## 2.用户操作操作

### 1.添加用户

```xml
//该用户家目录在 /home/用户名
useradd 用户名

//-d给新创建的用户名指定家目录
useradd -d 指定目录 用户名

useradd -g 指定组名 用户名
```

### 2.添加/修改密码

```xml
//给用户指定密码
passwd 用户名
```

### 3.切换用户

```xml
su 用户名
//用户权限高的切换到权限低的不需要输入密码，反之则需要
```

### 4.删除用户

```xml
//删除用户，但是保留家目录
userdel 用户名

//删除用户及家目录
userdel -rf 用户名
```

### 5.查看用户

```xml
//查看所有创建的用户 [0-1000:系统用户,0为root,1000-65535:可创建]
cat /etc/passwd

//查看当前是哪个用户
whoami

//查看用户是否存在
id 用户名
```

### 6.新增组

```xml
groupadd 组名
```

### 7.修改用户的组

```xml
usermod -g 组名 用户名

//创建用户时直接指定组
useradd -g 组名 用户名
```

### 8.将用户从组中删除

```xml
gpasswd -d 用户名 组名
```

### 9.删除组

```xml
groupdel 组名

//强制删除组
groupdel -f 组名
```

## 3.目录操作

### 1.基础操作

```xml
pwd				查看当前工作目录
clear 			清除屏幕
cd ~ & cd /root/			当前用户目录
cd /			根目录
cd -			上一次访问的目录
cd ..			上一级目录
ll				查看当前目录下内容（LL的小写）
```

### 2.创建目录

```xml
mkdir aaa		在当前目录下创建aaa目录，相对路径；
mkdir ./bbb		在当前目录下创建bbb目录，相对路径；
mkdir /ccc		在根目录下创建ccc目录，绝对路径；
```

### 3.递归创建目录

```xml
mkdir -p img/dog 
```

### 4.移动目录

```xml
mv	/aaa /bbb		    将根目录下的aaa目录，移动到bbb目录下(假如没有bbb目录，则重命名为bbb)；
mv	bbbb usr/bbb		将当前目录下的bbbb目录，移动到usr目录下，并且修改名称为bbb；
mv	bbb usr/aaa			将当前目录下的bbbb目录，移动到usr目录下，并且修改名称为aaa；
```

### 5.复制目录

```xml
cp -r /aaa /bbb			将/目录下的aaa目录复制到/bbb目录下，在/bbb目录下的名称为aaa
cp -r /aaa /bbb/aaa		将/目录下的aa目录复制到/bbb目录下，且修改名为aaa;
cp -r test/img/1.png test/a 将test目录下img目录下的1.png复制到test目录下a目录下
```

### 6.重命名目录

```xml
mv 原目录名称 新目录名称   mv img img2 
```

### 7.删除目录

```xml
rm -r /bbb			普通删除。会询问你是否删除每一个文件
rmdir img		目录的删除
```

### 8.强制删除

```xml
rm -rf img			强制删除img目录及子目录
```

### 9.搜索目录

```xml
find / -name 'b'		查询根目录下（包括子目录），名以b的目录和文件；
find / -name 'b*'		查询根目录下（包括子目录），名以b开头的目录和文件； 
find -name a.png 		查询当前目录下，名为a.png的目录
```

## 4.文件操作

### 1.创建文件

```xml
touch 1.txt 创建名为1.txt的文件
touch hello.java 创建名为hello.java的文件
```

### 2.删除文件

```xml
rm -r hello .java		删除当前目录下的hello.java文件（每次回询问是否删除y：同意）
rm -f a.txt b.txt 		同时删除多个文件
```

### 3.强制删除文件

```xml
rm -rf hello.java		强制删除当前目录下的hello.java文件
rm -rf ./a*			强制删除当前目录下以a开头的所有文件；
rm -rf ./*			强制删除当前目录下所有文件（慎用）；
```

### 4.搜索文件

```xml
find . -name "*" -size 145800c -print  打印当前文件夹下指定大小的文件
```

### 5.查看文件类型

```xml
file a.txt    查看a.txt的文件类型
```

## 5.文件内容操作

### 1.修改文件内容

```xml
vim hello.java   	进入一般模式
i(按键)   		进入插入模式(编辑模式)
ESC(按键)  		退出
:wq! 			强制保存退出（shift+：调起输入框）
:wq 			保存退出（shift+：调起输入框）
:q！			不保存退出（shift+：调起输入框）（内容有更改）(强制退出，不保留更改内容)
:q				不保存退出（shift+：调起输入框）（没有内容更改）
set nu         显示行号 [需要先esc退出]
```

### 2.文件内容的查看

```xml
cat hello.java		查看a.java文件的最后一页内容；
more hello.java		从第一页开始查看a.java文件内容，按回车键一行一行进行查看，
                    按空格键一页一页进行查看，q退出；
less hello.java		从第一页开始查看a.java文件内容，按回车键一行一行的看，
                    按空格键一页一页的看，支持使用PageDown和PageUp翻页，q退出；

tail -f hello.java			查看a.java文件的后10行内容；
head hello.java				查看a.java文件的前10行内容；
tail -f hello.java			查看a.java文件的后10行内容；
head -n 7 hello.java		查看a.java文件的前7行内容；
tail -n 7 hello.java		查看a.java文件的后7行内容；
```

> **总结下more 和 less的区别:**
>
> 1.less可以按键盘上下方向键显示上下内容,more不能通过上下方向键控制显示
>
> 2.less不必读整个文件，加载速度会比more更快
>
> 3.less退出后shell不会留下刚显示的内容,而more退出后会在shell上留下刚显示的内容.
>
> 4.more不能后退.

### 3.搜索文件内容

```xml
grep under hello.java		在hello.java文件中搜索under字符串，大小写敏感，显示行；
grep -n under hello.java		在hello.java文件中搜索under字符串，大小写敏感，显示行及行号；
grep -v under hello.java		在hello.java文件中搜索under字符串，大小写敏感，显示没搜索到的行；
grep -i under hello.java		在hello.java文件中搜索under字符串，大小写敏感，显示行；
grep -ni under hello.java		在hello.java文件中搜索under字符串，大小写敏感，显示行及行号；
```

### 4.覆盖和追加内容

```xml
//一个 > 表示覆盖
//两个 >> 表示追加

echo 'Hello World' > test/hello.java  将Hello World字符串覆盖到test目录下hello.java文件的内容

echo 'Hello World' >> test/hello.java  将Hello World字符串追加到test目录下hello.java文件的内容中
```

## 6.压缩和解压缩

### 1.tar

#### 1.压缩

```xml
//.tar
tar -cvf ab.tar a.java b.java			//将当前目录下a.java、b.java一起打包为ab.tar
tar -cvf all.tar ./*					//将当前目录下的所欲文件一起打包压缩成all.tar文件


//.tar.gz
tar -zcvf ab.tar.gz a.java b.java		//将当前目录下a.java、b.java一起打包为ab.tar.gz
tar -zcvf start.tar.gz ./*				//将当前目录下的所欲文件一起打包压缩成all.tar.gz文件
```

#### 2.解压

```xml
//.tar
tar -xvf ab.tar						//ab.tar压缩包，到当前文件夹下；
tar -xvf all.tar -C usr/local 		//（C为大写，中间无空格）
										//ab.tar压缩包，到/usr/local目录下；
//.tar.gz
tar -zxvf ab.tar.gz					//解压start.tar.gz压缩包，到当前文件夹下；
tar -zxvf ab.tar.gz -C usr/local 	//（C为大写，中间无空格）
									//解压ab.tar.gz压缩包，到/usr/local目录下；

//.tar.xz
tar xf node-v12.18.1-linux-x64.tar.xz  //解压xz压缩包
```

### 2.zip

#### 1.压缩

```
//文件压缩
zip a.zip a.txt							//将单个文件压缩
zip a.zip a.txt	b.txt					//将多个单个文件一起压缩

//目录压缩
zip -r imgs.zip imgs/					//将目录进行压缩
zip -r imgslist.zip img1/ img2/			//将多个目录一起压缩为imgslist	
```

#### 2.解压

```xml
unzip imgsli.zip  								//解压一个zip格式压缩包

unzip -d img imglist.zip			//将`english.zip`包，解压到指定目录下`img`

unzip -d /usr/app/com.lydms.english.zip			//将`english.zip`包，解压到指定目录下`/usr/app/`
```

## 7.curl

### 1.get请求

```xml
curl https://jdg3h6.lafyun.com:443/bingone
```

### 2.post请求

```xml
curl -d'qq=2867462354' -X POST https://jdg3h6.lafyun.comqq43/
```

## 8.date

```xml
date 指定格式显示时间： date +%Y:%m:%d 、date +%Y-%m-%d

%H : 小时(00..23)
%M : 分钟(00..59)
%S : 秒(00..61)
%X : 相当于 %H:%M:%S
%d : 日 (01..31)
%m : 月份 (01..12)
%Y : 完整年份 (0000..9999)
%F : 相当于 %Y-%m-%d

date +%Y-%m-%d-%X-%S-%M-%H
date +%Y-%m-%d
date +%X:%S:%M:%H
```

## 9.进程

### 1.查看进程状态信息

```xml
ps aux

ps -ef
```

### 2.杀死进程

```xml
kill -s 9 进程号PID
```

## 10.rpm

### 1.安装软件包

```xml
rpm -ivh xxx.rpm
```

### 2.卸载软件包

```xml
rpm -evh 软件名称

//简写
rpm -e 软件名称
```

### 3.显示所有软件包

```xml
rpm -qa

//结果分页
rpm -qa | more
rpm -qa | less

//过滤结果
rpm -qa | grep 指定安装包名称
```

### 4.查询软件包的安装路径

```xml
rpm -ql 软件包名称
```

### 5.查询指定软件安装详细信息

```xml
rpm -qi 软件包名称
```

### 6.升级指定软件包

```xml
rpm -Uvh 软件包
```

### 7.查询软件包是否已经安装

```xml
rpm -q 软件包名称
```

### 8.查询软件包的依赖关系

```xml
rpm -qR 软件包名称
```

## 11.yum

```xml
yum的仓库配置文件位于/etc/yum.reops.d目录下

yum的主配置文件 /etc/yum.conf文件
```

### 1.安装软件

```xml
yum -y install package
```

### 2.卸载软件

```xml
yum -y remove package
```

### 3.清空缓存

```xml
yum clean packages 														# 清除缓存目录下的软件包，清空的是(/var/cache/yum)下的缓存
yum clean headers 														# 清除缓存目录下的 headers
yum clean oldheaders 													# 清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) 	# 清除缓存目录下的软件包及旧的headers
yum 安装一个软件的时候会把软件包下载到本地指定的目录中，所以为了节省磁盘空间，可以用上述命令清空缓存
```

### 4.显示信息

```xml
yum list          														# yum list显示所有已经安装和可以安装的程序包   
yum list <package_name> 												# 显示安装包信息rpm，显示installed ，这里是包名，版本和仓库名
yum list repolist all													#查询所有的yum仓库
yum info <package_name>                           						#显示安装包rpm的详细信息
yum groupinfo <group_name>             									#显示程序组group信息
```

### 5.搜索与查看

```xml
  yum search 软件名称 													#根据关键字string查找安装包
  yum deplist <package_name>											# 仅仅 查看程序rpm依赖情况
  yum provides */命令													# 查看命令是由哪个包提供的（这个命令很有帮助）
```

### 6.升级yum

```xml
yum check-update 														#检查可更新的软件有哪些
yum update 																#更新升级所有软件包
yum update <package_name> 												#更新指定程序包package，   
yum upgrade <package_name> 												#升级指定程序包package
```

## 12防火墙

```ssh
# 查看防火墙状态
systemctl status firewalld

# 临时关闭防火墙
systemctl stop firewalld

# 永久关闭防火墙
systemctl disable firewalld

# 启动防火墙
systemctl start firewalld

# 重启防火墙
systemctl restart firewalld

# 设置防火墙自启
systemctl enable firewalld

# 防火墙临时放行端口
firewall-cmd --add-port=8080/tcp

# 防火墙永久放行端口
firewall-cmd --permanent --add-port=8080/tcp

# 重载防火墙规则
firewall-cmd --reload

# 防火墙移除放行端口
firewall-cmd --remove-port=8080/tcp

# 查看防火墙放行规则
firewall-cmd --list-all
```

