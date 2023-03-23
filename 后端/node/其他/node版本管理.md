# 1.nvm
nvm 主要是用来管理 nodejs 和 npm 版本的工具，可以用来切换不同版本的 nodejs。
[github地址](https://github.com/coreybutler/nvm-windows/)
[nvm下载地址](https://github.com/coreybutler/nvm-windows/releases)，下载nvm-setup.zip

首先把nvm-setup.zip解压到比如f:/nvm/nvm 中(其它盘也可以)；然后以管理员的身份运行nvm-setup . 选择nvm安装目录为f:\nvm\nvm，node安装目录为 f:\nvm\nodejs,修改settings.txt的内容为：
> root: f:\nvm\nvm
path: f:\nvm\nodejs
arch: 64
proxy: none
node_mirror: http://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/

然后修改nvm环境变量:
>NVM_HOME:E:\dev\nvm
NVM_SYMLINK:E:\dev\nodejs
PATH:%NVM_HOME%;%NVM_SYMLINK%（path已存【%NVM_HOME%;%NVM_SYMLINK%】在添加到最后）
# 常见使用命令
```nvm list```：查看当前本机使用 nvm 已安装的nodejs的版本列表

```nvm arch```：查看当前本机是 32 bit 还是 64 bit

```nvm install node@版本号```：安装指定版本的 nodejs

```nvm install latest```：安装最新版本的 nodejs

```nvm install 14.15.1```：安装 14.15.1 版本的 nodejs

```nvm uninstall node@版本号```：卸载指定版本的 nodejs

```nvm uninstall 14.15.1```：卸载 14.15.1 版本的 nodejs

```nvm use node@版本号```：使用指定版本的 nodejs(该版本是已经安装过后的)

```nvm use 14.15.1```：使用已安装的 14.15.1 版本的 nodejs

```nvm root```：查看本机安装的 nvm 的安装目录地址

# 2.gnvm

GNVM 是一个简单的 Windows 下 Node.js 多版本管理器，类似的 nvm nvmw nodist 。
# 安装gnvm
>[github地址](https://github.com/Kenshin/gnvm)
>[gnvm安装地址](https://github.com/Kenshin/gnvm-bin/blob/master/64-bit/gnvm.exe?raw=true)

下载完以后会有一个gnvm.exe 文件,将其移动到node.js文件中去。
![jhuytrfgbytrdfvgt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jhuytrfgbytrdfvgt.3q0r3c3o7fc0.webp)

# 验证gnvm是否可用
> 使用命令: gnvm version

![jkiu9876trfdvr5t](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jkiu9876trfdvr5t.2h3oeznd7200.webp)

出现版本号即为安装成功

# gnvm相关指令
### 1.安装多个 node 版本
```npm
　　gnvm install latest     // 安装最新版本的 node 
　　gnvm install 10.0.0     // 安装指定版本，也可以指定安装32位或64位，eg: gnvm install 10.0.0-x64
　　gnvm update latest     // 更新本地 latest 的 node 版本
```

### 2.卸载任意版本的 node
```npm
　　gnvm uninstall latest    // 卸载最新版本的 node 
　　gnvm uninstall 10.0.0   // 卸载指定版本
```

### 3.查看本地所有安装的 node 版本
```npm
　　gnvm ls
```
### 4.切换任意版本的 node

```npm
　　gnvm use 10.0.0
```

### 5.安装 npm

```npm
　　gnvm npm latest
```

### 6.安装淘宝镜像

```npm
　　gnvm config registry TAOBAO
```