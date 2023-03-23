```xml
//1.进入虚拟机打开终端
//2.输入命令 nmtui 进入网络管理器
//3.上下键选择到 `编辑连接` 回车
//4.上下键选择到 以太网`ens33` 并选择编辑选项
//5.查看VMware,编辑->虚拟网络编辑器->点击Vmnet8->NAT设置->查看 `网关IP(G)` ,例如 `192.168.199.2`
//6.回到终端,上下键选择 `IPv4配置` 设置为 `手动` ,并将其后的 `显示` 设置为 `隐藏` ,出现地址、网关、DNS服务器等配置
//7.设置地址为步骤5的网关IP的前三位并自定义最后一位再加上 子网掩码`/24`,例如 `192.168.199.110/24`
//8.设置网关为步骤5得网关IP,例如 `192.168.199.2`
//9.设置DNS服务器为 `114.114.114.114`,固定值
//10.设置完IPv4的配置后，上下键选择当前页的最后一项 `确定`,之后页面选择 `返回`,之后再选择 `确定` 就回到了终端页面
//11.输入命令 `systemctl restart network` 重置网络
//12.继续输入命令 `ifconfig` , 终端出现 `ens33` 下 有 inet 192.168.199.110 [inet 第7步骤设置的地址],设置成功
//13.验证是否成功,终端处输入 `ping www.baidu.com` 或者输入ping 宿主机 `ping 宿主机ip[打开cmd,键入ipconfig ,找到以太网适配器 以太网的IPv4 地址]`,或者在宿主机 `ping 192.168.199.2`出现以下即为配置成功:

正在 Ping 192.168.199.110 具有 32 字节的数据:
来自 192.168.199.110 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.199.110 的回复: 字节=32 时间=1ms TTL=64
来自 192.168.199.110 的回复: 字节=32 时间=1ms TTL=64
来自 192.168.199.110 的回复: 字节=32 时间=1ms TTL=64

192.168.199.110 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 0ms，最长 = 1ms，平均 = 0ms
```

```xml
//etc/sysconfig/network-scripts

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=8c5d80f3-774e-44c4-a489-45e32e10269b
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.199.110
PREFIX=24
GATEWAY=192.168.199.2
DNS1=114.114.114.114
```

## 报错

> Job for network,service failed because the control process exited with error cod e See "systemctl status network,service" and "journalctl - xe" for details.

```linux
systemctl stop NetworkManager
systemctl restart network
```

