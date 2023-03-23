# 使用 qrcodejs生成二维码
qrcode.js 是一个用于生成二维码的 JavaScript 库。主要是通过获取 DOM 的标签,再通过 HTML5 Canvas 绘制而成,不依赖任何库。
### 安装
npm
> npm install --save qrcode


cdn
>https://cdn.bootcdn.net/ajax/libs/qrcodejs/1.0.0/qrcode.js

[bootcdn](https://www.bootcdn.cn/)搜qrcodejs引用可用版本。

# 先来看一个例子

```html
<!DOCTYPE>
<html>
<head>
    <meta charset="UTF-8">
    <title>林扬生</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/qrcodejs/1.0.0/qrcode.js"></script>
</head>
<body>
    <div id="qrcode"></div>
</body>
<script type="text/javascript">
    new QRCode("qrcode", {
        text: "https://www.baidu.com/",
        width: 128,
        height: 128,
        colorDark : "#000000",
        colorLight : "#ffffff",
        correctLevel : QRCode.CorrectLevel.H
    });
</script>
</html>
```
![fvgt54r567tre](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fvgt54r567tre.1rs738r4yeow.webp)

# 使用方法

1.引入qrcode.js
```javascript
<script src="https://cdn.bootcdn.net/ajax/libs/qrcodejs/1.0.0/qrcode.js"></script>
```
2.DOM结构
```html
<div id="qrcode"></div>
```
2.初始化生成的二维码,并设置二维码的宽高，颜色，背景等属性
```javascript
    new QRCode("qrcode", {
        text: "",
        width: 128,
        height: 128,
        colorDark : "#000000",
        colorLight : "#ffffff",
        correctLevel : QRCode.CorrectLevel.H
    });
```
# 参数说明
```javascript
new QRCode(element, option)
```
|  名称   |  说明  |
|  :--: | :--:  | :--:|
| element  | 显示二维码的元素或该元素的 ID |
| option  | 参数配置 |

# option 参数说明

|  左对齐   |  居中  |  右对齐 | 
|  :-- | :--:  | --:|
| width  | 256| 二维码宽度|
| height  | 256| 二维码高度|
| typeNumber  | 4| |
| colorDark  | #000000| 前景色|
| colorLight  | #ffffff| 背景色|
| correctLevel  | QRCode.CorrectLevel.L| 容错级别：L、M、Q、H
