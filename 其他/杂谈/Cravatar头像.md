### Cravatar头像

> [Cravatar](https://cravatar.cn/) 是 Gravatar 在中国的完美替代方案,从此你可以自由的上传和分享头像。

# 基本概念

>Cravatar 头像服务可以像普通的图片 URL 一样请求，具体格式是：
>https://cravatar.cn/avatar/HASH

>其中 HASH 部分是你的电子邮箱的哈希值，此电子邮箱必须在 Cravatar.cn 上注册并绑定头像，否则会尝试返回 Gravatar 头像和 QQ 头像，如果都不存在，则返回默认头像。

### 电子邮箱的哈希方法

1.去除首位两边的空格。
2.所有字母转小写。
3.计算 MD5 值。[在线md5加密解密](https://www.cmd5.com/)
> https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg
> 
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg">

### 指定图片格式
当前支持四种图片返回格式，分别是：jpg、jpeg、png、gif、webp(需浏览器支持)。
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpeg
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.png
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.gif
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.webp

<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpeg">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.png">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.gif">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.webp">

### 调整头像大小
默认情况下 80×80 尺寸的头像，但是可以通过 s 或 size 参数来指定要获取的头像大小。

>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=120
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=200
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?size=120
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?size=400

<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=120">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=200">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?size=120">
<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?size=200">

### 默认头像
如果你的邮箱哈希无法匹配到任何头像，则返回的默认头像：
>https://cravatar.cn/avatar/666.jpg

<img src="https://cravatar.cn/avatar/666.jpg">

内置的默认头像，只需要传入 d=默认头像ID 即可调用：
- 404：如果没有与电子邮件哈希关联的图像，则不加载任何图像，而是返回 HTTP 404（未找到文件）响应
- mp：一个简单的卡通风格的人物轮廓
- identicon：一个几何图案（随机生成）
- monsterid：具有不同颜色、面孔的“怪物”（随机生成）
- wavatar：具有不同特征和背景的人脸（随机生成）
- retro：8位色的像素人脸（随机生成）
- robohash：具有不同颜色、面部的机器人（随机生成）
- blank：透明的 PNG 图像（为方便演示，已为其添加了一个边框）

>https://cravatar.cn/avatar/666.jpg?d=404
https://cravatar.cn/avatar/666.jpg?&d=mp
https://cravatar.cn/avatar/666.jpg?&d=identicon
https://cravatar.cn/avatar/666.jpg?&d=monsterid
https://cravatar.cn/avatar/666.jpg?&d=wavatar
https://cravatar.cn/avatar/666.jpg?&d=retro
https://cravatar.cn/avatar/666.jpg?&d=robohash
https://cravatar.cn/avatar/666.jpg?&d=blank


<img src="https://cravatar.cn/avatar/666.jpg?&d=404">
<img src="https://cravatar.cn/avatar/666.jpg?&d=mp">
<img src="https://cravatar.cn/avatar/666.jpg?&d=identicon">
<img src="https://cravatar.cn/avatar/666.jpg?&d=monsterid">
<img src="https://cravatar.cn/avatar/666.jpg?&d=wavatar">
<img src="https://cravatar.cn/avatar/666.jpg?&d=retro">
<img src="https://cravatar.cn/avatar/666.jpg?&d=robohash">
<img src="https://cravatar.cn/avatar/666.jpg?&d=blank">

### 强制加载默认头像
如果由于某种原因想强制始终返回默认头像，可以使用 f 或 forcedefault 参数并将其值设置为 y。
>https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=200&d=retro&f=y

<img src="https://cravatar.cn/avatar/2c50240f4bb504a3ab2009f67ef8d2ad.jpg?s=100&d=monsterid&f=y">