我们都知道less无法在浏览器中直接使用，浏览器不能直接对其识别

那我就是想在浏览器中使用less呢？应该怎么做？

## 解决

> 通过less解析插件less.js（JavaScript插件）把less文件解析成css代码

1.去   [下载地址](https://github.com/less/less.js)  下载 `dist`包下 `js` 文件并引入自己的项目中

2.less 样式文件一定要在 Less.js之前引入，这样才能保证 .less 文件被正确编译。

### html

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <link rel="stylesheet/less" type="text/css" href="../css/less.css" />
        //引入less.js文件
        <script src="../js/less.js"></script>
    </head>
    <body>
        hello,less
    </body>
</html>
```

### less

```less
@charset "utf-8";
@baseColor: pink;
@fontSize: 30px;

* {
    margin: 0;
    padding: 0;
}

body,
html {
    width: 100%;
    height: 100%;
    background-color: @baseColor;
    display: grid;
    justify-content: center;
    align-content: center;
    font-size: @fontSize;
}
```
