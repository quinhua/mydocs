# 安装
### npm
```npm
npm install art-template --save
```
### 在浏览器中实时编译
可下载到本地导入,也可直接链接到项目
```html
https://unpkg.com/art-template@4.13.2/lib/template-web.js
```

# 实例
```html
<!DOCTYPE HTML>
<html>
<head>
  <meta charset="UTF-8">
  <title>template基本使用</title>
  <script src="https://unpkg.com/art-template@4.13.2/lib/template-web.js"></script>
</head>
<body>
  <div id="content"></div>
  <script id="test" type="text/html">
  {{if isAdmin}}
  <h1>{{title}}</h1>
  <ul>
      {{each list value i}}
          <li>索引 {{i + 1}} ：{{value}}</li>
      {{/each}}
  </ul>
  {{/if}}
  {{$data}}
  </script>
  <script>
    var data = {
      title: '基本例子',
      isAdmin: true,
      list: ['文艺', '博客', '摄影', '电影', '民谣', '旅行', '吉他']
    };
    var html = template('test', data);
    document.getElementById('content').innerHTML = html;
  </script>
</body>
</html>
```
![bnjhgfdsew45tf](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/bnjhgfdsew45tf.webp)

# 输出
```javascript
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
```

# 条件
```javascript
{{if value}} ... {{/if}}
{{if v1}} ... {{else if v2}} ... {{/if}}
```

# 循环
```javascript
{{each target}}
    {{$index}} {{$value}}
{{/each}}
```