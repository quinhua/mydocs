# 1.安装json-server
[json-server](https://github.com/typicode/json-server)

```npm
npm install -g json-server  全局安装
npm install json-server     局部安装
```
# 2.创建数据
```js
//创建db.json存放json数据
{
    "users": [
        {
            "id": 1,
            "username": "张三",
            "password": "zs123"
        },
        {
            "id": 2,
            "username": "李四",
            "password": "ls123"
        },
        {
            "id": 3,
            "username": "王五",
            "password": "ww123"
        }
    ]
}
```
局部安装json-server需使用npm init -y 命令生成package.json
```js
  "scripts": {
    "serve": "json-server db.json"
    //"serve": "json-server --watch db.json"   //监听
  },
```

# 3.启动json服务器
```npm
npm run serve                 //局部
json-server db.json           //全局
json-server --watch db.json   //全局-监听
json-server --watch db.json --port 3004//全局-监听-端口
```
# 路由  
```npm
GET    /users
GET    /users/1
POST   /users
PUT    /users/1
DELETE /users/1
```
# 使用例子
```html
<body h-screen m-0 p-0 flex justify-center items-center>
  <button border-hidden p-4 b-rd-2 bg-blue c-white>获取全部用户</button>
  <button border-hidden p-4 b-rd-2 bg-purple c-white>获取id=3的用户</button>
  <button border-hidden p-4 b-rd-2 bg-pink c-white>添加赵六</button>
  <button border-hidden p-4 b-rd-2 bg-orange c-white>修改id=4</button>
  <button border-hidden p-4 b-rd-2 bg-red c-white>删除id=4</button>
  <script src="https://cdn.jsdelivr.net/npm/@unocss/runtime/attributify.global.js"></script>
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  <script>
    const btn = document.getElementsByTagName("button");
    btn[0].onclick = () => {
      axios.get("http://localhost:3000/users").then((res) => {
        if (res.status == 200) {
          console.log(res.data);
        }
      }).catch((err) => { console.log(err) });
    }
    btn[1].onclick = () => {
      axios.get("http://localhost:3000/users/3").then((res) => {
        if (res.status == 200) {
          console.log(res.data);
        }
      }).catch((err) => { console.log(err) });
    }
    btn[2].onclick = () => {
      axios.post("http://localhost:3000/users", {
        "username": "赵六",
        "password": "zl123"
      }).then((res) => {
        if (res.status == 200) {
          console.log("添加成功");
        }
      }).catch((err) => { console.log(err) });
    }
    btn[3].onclick = () => {
      axios.put("http://localhost:3000/users/4", {
        "username": "赵六啊",
        "password": "zl666"
      }).then((res) => {
        if (res.status == 200) {
          console.log("修改成功");
        }
      }).catch((err) => { console.log(err) });
    }
    btn[4].onclick = () => {
      axios.delete("http://localhost:3000/users/4").then((res) => {
        if (res.status == 200) {
          console.log("删除成功");
        }
      }).catch((err) => { console.log(err) });
    }
  </script>
</body>
```
