# 一、安装 Pug

[pug中文网](https://pugjs.org/)需要有node环境。

[methodot官网](https://factory.methodot.com/)

    npm i -D pug pug-plain-loader

> pug 一般与 pug-plain-loader 一起安装，pug-plain-loader 用于解析 pug。

# 二、编译

先写一个简单的index.pug文件

    doctype html
    html
      head
        meta(charset='UTF-8')
        meta(http-equiv='X-UA-Compatible' content='IE=edge')
        meta(name='viewport' content='width=device-width, initial-scale=1.0')
        title 林扬生
        link(rel='stylesheet' href='https://unpkg.com/element-plus/dist/index.css')
        script(src='https://unpkg.com/vue@3')
        script(src='https://unpkg.com/element-plus')
        script(src='https://cdn.bootcdn.net/ajax/libs/axios/0.27.2/axios.js')
        style.
          body{
          margin:0;
          padding:0;
          }
      body
        #app
          | {{data.msg}}
        script.
          Object.assign(window, Vue);
          const composition = {
          setup() {
          const data = reactive({
          msg: "林扬生"
          });
          const http=()=>{
          axios.get("https://v1.hitokoto.cn").then((res)=>{
          console.log(res.data)  
          })  
          }
          http()
          return {
          data
          }
          }
          }
          const app = createApp(composition).use(ElementPlus).mount("#app");

然后执行命令将其编译为html文件,两种命令:

> 1.pug index.pug

编译出来的是压缩版的代码,即没有空格、换行的代码显示。

> 2.pug -P -w index.pug

编译出来的代码不是压缩版更具可读性,并且可以实时编译,实时预览。

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>林扬生</title>
        <link rel="stylesheet" href="https://unpkg.com/element-plus/dist/index.css">
        <script src="https://unpkg.com/vue@3"></script>
        <script src="https://unpkg.com/element-plus"></script>
        <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.27.2/axios.js"></script>
        <style>
          body{
          margin:0;
          padding:0;
          }
        </style>
      </head>
      <body>
        <div id="app">{{data.msg}}</div>
        <script>
          Object.assign(window, Vue);
          const composition = {
          setup() {
          const data = reactive({
          msg: "林扬生"
          });
          const http=()=>{
          axios.get("https://v1.hitokoto.cn").then((res)=>{
          console.log(res.data)  
          })  
          }
          http()
          return {
          data
          }
          }
          }
          const app = createApp(composition).use(ElementPlus).mount("#app");
        </script>
      </body>
    </html>