# 1.安装

```npm
npm install ws
```

# 2.使用

创建 `main.js`

```js
// 导入ws模块
const WebSocket=require('ws');
let wss=new WebSocket.Server({
    port:9999
});
// 用户连接时触发
wss.on('connection',function(ws,request){
    // 接收数据时触发
    ws.on('message',function(message){
        // 默认接收的message是一个字符串 需用用JSON.parse()转成对象
        let info=JSON.parse(message);
        // 如果是登录请求 为客户端对象ws添加一个user属性info中的user属性
        if(info.type==='login'){
            ws['user']=info.user;
        }else if(info.type==='message'){
        // 如果是信息请求 则遍历wss.clients这个客户端set对象 
        // 注意 这个对象是set类型 所以需要使用forEach进行遍历
            wss.clients.forEach(element => {
                // 如果遍历到的客户端的user和info中的to相同 则发送信息给该客户端
                if(element['user']===info.to){
                    element.send(info.message)
                }
            });
        }
    })
})
```

创建两个 `html` 页面,只需要每个页面的 `user`  不同即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ws</title>
</head>
<body>
    <label>需要发送的信息:<input type="text" id="message"></label>
    <label>接收者:<input type="text" id="receiver"></label>
    <button id="sendMessage">发送信息</button>
    <button id="login">登录</button><br/>
    <span id="span"></span>
    <script>
        // 连接服务器
        let ws=new WebSocket('ws://127.0.0.1:9999');
        let sendMessage=document.getElementById('sendMessage');
        let login=document.getElementById('login');
        let message=document.getElementById('message');
        let receiver=document.getElementById('receiver');
        // 当服务器打开之后 默认直接进行登录
        ws.onopen=function(){
            ws.send(JSON.stringify({
            type:"login",  // type：login表示登录 用于后端逻辑判断
            // user:只要确保每一个html页面中的不一样就可以了
            user:"A"       // user：用户名 用于区分用户并且作为别的用户发送信息时候的接收者
        }))
        }
        // 接收信息 当服务器返回信息时打印信息内容
        ws.onmessage=function(message){
            document.getElementById("span").innerHTML=message.data
        }
        // 发生信息 按钮点击时 发送信息 
        sendMessage.onclick=function(){
            ws.send(JSON.stringify({
                type:"message",        // type:message 表示发送信息 后端逻辑判断用
                to:receiver.value,     // 需要发送给谁
                message:message.value, // 需要发送的内容
            }))
        }
    </script>
</body>
</html>
```

