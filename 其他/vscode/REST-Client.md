# 在VsCode中发送http请求

## 1.安装插件

在VsCode中搜索插件`REST Client`,点击安装。

## 2.使用

创建 `api.http` 文件并编写如下

```js
get https://example.com/comments/1
```

之后上面会出现`Send Request`按钮,点击将会发送`get`请求,请求结果将会出现在右侧新窗口。