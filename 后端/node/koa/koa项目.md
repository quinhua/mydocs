# 一.koa起步

## 1.项目初始化

执行 `npm init -y` ,生成 `package.json`

```npm 
npm init -y
```

## 2.安装koa

执行命令

```npm
npm install koa
```

![image](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/image.5vbn2g5gptc0.webp)

##  3.编写基本app

创建 `src/main.js`

```js
//1.导入koa包
const Koa = new require("Koa");

//2。实例化app对象
const app = new Koa();

//3.编写中间件
app.use((ctx, next) => {
    ctx.body = "hello,koa!"
})

//4.启动服务器
app.listen(3000,()=>{
    console.log("server is running on http://localhost:3000");
})
```

## 4.测试

在终端使用 `node src/main.js`

# 二.项目的基本优化

## 1.自动重启服务

安装 `nodemon`

```npm
npm install nodemon -D
```

编写 `package.json ` 修改启动文件

```js
"scripts": {
    "dev":"nodemon ./src/main.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

执行`npm run dev`启动项目

## 2.读取配置文件

安装 `dotenv` ,作用：读取根目录中的 `.env` 文件,将配置写入 `process.env` 中

```npm
npm install dotenv
```

创建 `根目录/.env` 文件

```js
//.env
PORT = 9999

DB_HOST = localhost
DB_PORT = 3306
DB_DATABASE = quinhua
DB_USERNAME = root
DB_PASSWORD = 1234
```

创建 `src/config/config.default.js` 导出变量

```js
const dotenv = require("dotenv");
dotenv.config();

//console.log(process.env.PORT);

module.exports = process.env;
```

执行命令 `node src/config/config.default.js` 可看到控制台输出 `9999`

改写 `src/main.js` 

```js
const Koa = new require("Koa");
const { PORT } = require("./config/config.default");

const app = new Koa();

app.use((ctx, next) => {
    ctx.body = "hello,koa!"
})

app.listen(PORT, () => {
    console.log(`server is running on http://localhost:${PORT}`);
})
```

## 3. 路径别名

安装 `module-alias`,为项目中路径设置别名

```js
npm install module-alias
```

配置 `package.json`,与 `scripts` 同级

```js
  "_moduleAliases": {
    //别名:文件夹
    "@": "src",
    "@config":"src/config"
  }
```

项目中使用

```js
require('module-alias/register')//放置于主文件中所有代码前
const app = require("@/app");//使用别名y
```

并将项目中所有引入的路径全部替换

# 三.路由

路由：根据不同的URL,调用对应的处理函数

## 1.安装 koa-router

执行 `npm install koa-router`

## 2.编写路由

创建 `src/router/user.route.js`文件并写入

```js
const Router = require("koa-router");

const router = new Router({ prefix: "/users" });

//GET /users/
router.get("/", (ctx, next) => {
    ctx.body = "hello,koa!";
});

module.exports = router
```

改写 `src/main.js`

```js
const Koa = new require("Koa");
const { PORT } = require("@config/config.default");

const app = new Koa();

const userRouter = require("@router/user.route");

app.use(userRouter.routes());

app.listen(PORT, () => {
    console.log(`server is running on http://localhost:${PORT}`);
});
```

# 四. 目录结构优化

## 1.将 `http` 服务和 `app` 业务拆分

创建 `src/app/index.js`写入

```js
const Koa = new require("Koa");
const app = new Koa();

const userRouter = require("@router/user.route");
app.use(userRouter.routes());

module.exports = app
```

 改写 `src/main.js`写入

```js
require('module-alias/register')
const app = require("@/app");

const { PORT } = require("@config/config.default");
app.listen(PORT, () => {
    console.log(`server is running on http://localhost:${PORT}`);
});
```

## 2. 拆分路由和控制器

路由：解析URL,分发给控制器对应的方法

控制器：处理不同的业务

创建 `src/controller/user.controller.js` 编写

```js
class UserController {
    async register(ctx, next) {
        ctx.body = "用户注册成功"
    }
}

module.exports = new UserController()
```

改写 `router/user.route.js`

```js
const Router = require("koa-router");
const router = new Router({ prefix: "/users" });

const {
    register
} = require("@controller/user.controller")

router.get("/register", register);

module.exports = router
```

# 五. 解析body

## 1.安装 `koa-body`

```npm
npm install koa-body
```

## 2.注册中间件

[注意]将 `koa-body` 注册在所有路由之前

改写 `src/app/index.js`

```js
const Koa = require("Koa")
const { koaBody } = require("koa-body")

const userRouter = require("@router/user.route")
const app = new Koa()

app.use(koaBody())

app.use(userRouter.routes())

module.exports = app
```

## 3.解析请求数据

在 `src/controller/user.controller.js` 使用

```js
class UserController {
    async register(ctx, next) {
        console.log(ctx.request.body)
        ctx.body = ctx.request.body
    }
}

module.exports = new UserController()
```

## 4.拆分数据层

service层用来做数据库处理

创建 `src/service/user.service.js` 写入

```js
class UserService{
    async createUser(username,password){
        return "写入数据库成功"
    }
}

module.exports=new UserService()
```

在 `src/controller/user.controller.js` 使用

```js
const { createUser } = require("@service/user.service")

class UserController {
    async register(ctx, next) {
        const { username, password } = ctx.request.body
        const res = await createUser(username, password)
        ctx.body = res
    }
}

module.exports = new UserController()
```

# 六.数据库操作

sequelize ORM 数据库工具

ORM： 对象关系映射

- 数据表映射(对应)一个类
- 数据表中的数据行(记录)对应一个对象
- 数据表字段对应对象的属性
- 数据表的操作对应对象的方法

## 1.安装

安装 `sequlize`以及 `mysql2` 

```npm
npm install sequelize mysql2
```

## 2.连接数据库

创建 `src/db/seq.js` 写入

```js
const { Sequelize, DataTypes } = require('sequelize');
const {
    DB_HOST,
    DB_PORT,
    DB_DATABASE,
    DB_USERNAME,
    DB_PASSWORD
} = require("../config/config.default")
//这里使用相对路径,使用别名访问不了变量

//设置timezone: '+08:00'变为北京时间
const seq = new Sequelize(DB_DATABASE, DB_USERNAME, DB_PASSWORD, {
    host: DB_HOST,
    dialect: "mysql",
    timezone: '+08:00'
});

//测试是否连接成功
// seq.authenticate().then((res) => {
//     console.log("数据库连接成功")
// }).catch((err) => { console.log(err) })

module.exports = seq
```

测试数据库是否连接成功

```npm
node src/db/seq.js
```

## 3.创建数据模型

创建 `src/model/user.model.js` 写入

```js
//数据模型
const { DataTypes } = require('sequelize')
const seq = require("../db/seq");

//创建名为User的模型,sequelize会自动将User变为Users
//sequelize中id、createdAt(创建-时间戳)、updatedAt(更新-时间戳) 会被自动创建
//allowNull:false设置字段不为空
//unique:true设置字段唯一
//defaultValue设置默认值
//comment注释

const User = seq.define('User', {
    uuid: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4
    },
    username: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true,
        comment: "用户名,唯一"
    },
    password: {
        type: DataTypes.CHAR(64),
        allowNull: false,
        comment: "密码"
    },
    is_admin: {
        type: DataTypes.BOOLEAN,
        allowNull: false,
        defaultValue: 0,
        comment: "是否为管理员,默认值不是管理员,1是0不是"
    }
});

// User.sync() - 如果表不存在,则创建该表(如果已经存在,则不执行任何操作)
// User.sync({ force: true }) - 将创建表,如果表已经存在,则将其首先删除
// User.sync({ alter: true }) - 这将检查数据库中表的当前状态(它具有哪些列,它们的数据类型等),
//然后在表中进行必要的更改以使其与模型匹配.

//User.sync({ force: true })//强制同步数据库(创建数据表),执行当前语句创库之后进行注释

module.exports = User
```

先将 `//User.sync({ force: true })` 取消注释

 执行 `node src/model/user.model.js` 进行创建数据库,可看到数据库输出创库建表语句

[注意] 执行创建数据库后需要将 `User.sync({ force: true })` 进行注释掉,否则项目运行一次就创建一次

执行完毕后查看数据库 `quinhua.users`数据库创建成功

## 4. 注册接口 

使用 `User` 数据模型注入注册业务中

改写 `src/service/user.service.js` 

```js
const User = require("@model/user.model.js")

class UserService {
    async createUser(username, password) {
        const res = await User.create({ username, password })
        return res.dataValues
    }
}

module.exports = new UserService()
```

控制层返回数据

改写  `src/controller/user.controller.js`

```js
const { createUser } = require("@service/user.service")

class UserController {
    async register(ctx, next) {
        const { username, password } = ctx.request.body
        const res = await createUser(username, password)
        ctx.body = {
            code:0,
            message:"用户注册成功",
            result:{
                id:res.id,
                uuid:res.uuid,
                username:res.username
            }
        }
    }

    async login(ctx, next) {
        ctx.body = "用户登录成功"
    }
}

module.exports = new UserController()
```

## 5. 查找业务

改写 `src/service/user.service.js` 

```js
const User = require("@model/user.model.js")

class UserService {
    //注册用户
    async createUser(username, password) {
        const res = await User.create({ username, password })
        return res.dataValues
    }

    async getUserInfo({ id, uuid, username, password, is_admin }) {
        //短路算法
        const whereObj = {}
        id && Object.assign(whereObj, { id })
        uuid && Object.assign(whereObj, { uuid })
        username && Object.assign(whereObj, { username })
        password && Object.assign(whereObj, { password })
        is_admin && Object.assign(whereObj, { is_admin })

        const res = User.findOne({
            attributes: ["id", "uuid", "username", "password", "is_admin"],
            where: whereObj
        })
        return res ? res.dataValues : null
    }
}

module.exports = new UserService()
```

改写  `src/controller/user.controller.js`

```js
const { createUser, getUserInfo } = require("@service/user.service")

class UserController {
    async register(ctx, next) {
        const { username, password } = ctx.request.body
        if (!username || !password) {
            console.error("用户名或密码为空", ctx.request.body)
            ctx.status = 400
            ctx.body = {
                code: 10001,
                message: "用户名或密码为空",
                result: ""
            }
            return
        }
        if (getUserInfo({ username })) {
            ctx.status = 409
            ctx.body = {
                code: 10002,
                message: "用户已经存在",
                result: ""
            }
            return
        }

        const res = await createUser(username, password)
        ctx.body = {
            code: 0,
            message: "用户注册成功",
            result: {
                id: res.id,
                uuid: res.uuid,
                username: res.username
            }
        }
    }

    async login(ctx, next) {
        ctx.body = "用户登录成功"
    }
}

module.exports = new UserController()
```

# 七.错误处理中间件

## 1.统一错误处理

- 在出错的地方使用`ctx.app.emit`提交错误
- 在 app 中通过`app.on`监听

编写统一的错误定义文件

创建 `src/constant/err.type.js`

```js
module.exports = {
    userErrorNameOrPwdNull: {
        code: 10001,
        message: "用户名或密码为空",
        result: ""
    },
    userErrorAlreadHad: {
        code: 10002,
        message: "用户已经存在",
        result: ""
    },
    userErrorRegisterErr: {
        code: 10003,
        message: "用户注册错误",
        result: ""
    },
}
```

## 2.统一错误码

创建 `src/app/errHandler.js`

```js
module.exports = (err, ctx) => {
    let status = 500
    switch (err.code) {
        case 10001:
            status = 400
            break
        case 1002:
            status = 409
            break
        default:
            status = 500
    }
    ctx.status = status
    ctx.body = err
}
```

改写 `src/app/index.js`

```js
const Koa = require("Koa")
const { koaBody } = require("koa-body")

const userRouter = require("@router/user.route")
const app = new Koa()

app.use(koaBody())

app.use(userRouter.routes())

//统一的错误处理
const errHandler=require("./errHandler")
app.on("error",errHandler)

module.exports = app
```



## 3.错误处理函数

创建 `src/middleware/user.middleware.js`

```js
const { getUserInfo } = require("../service/user.service")
const {
    userErrorNameOrPwdNull,
    userErrorAlreadHad,
    userErrorRegisterErr
} = require("../constant/err.type")

const userValidatorNameOrPwdNull = async (ctx, next) => {
    const { username, password } = ctx.request.body
    if (!username || !password) {
        console.error('用户名或密码为空', ctx.request.body)
        ctx.app.emit("error", userErrorNameOrPwdNull, ctx)
        return
    }
    await next()
}

const userValidatorAlreadHad = async (ctx, next) => {
    const { username } = ctx.request.body
    try {
        const res = await getUserInfo({ username })
        if (await getUserInfo({ username })) {
            console.error('用户名已经存在', { username })
            ctx.app.emit("error", userErrorAlreadHad, ctx)
            return
        }
    } catch (err) {
        console.error("获取用户信息错误", err)
        ctx.app.emit('error', userErrorRegisterErr, ctx)
        return
    }
    await next()
}

module.exports = {
    userValidatorNameOrPwdNull,
    userValidatorAlreadHad
}
```

## 4.错误处理的使用

`src/router/user.route.js`

```js
const Router = require("koa-router")
const router = new Router({ prefix: "/users" })
const {
    userValidatorNameOrPwdNull, userValidatorAlreadHad
} = require("@middleware/user.middleware.js")
const {
    register, login
} = require("@controller/user.controller")

router.post("/register", userValidatorNameOrPwdNull, userValidatorAlreadHad, register)
router.post("/login", login)

module.exports = router
```

# 八.加密

数据加密：在将密码保存到数据库之前, 要对密码进行加密处理

加盐加密

```js
npm install bcryptjs
```

编写 `utils/util/bcrypt.js`

```js
const bcrypt = require('bcryptjs')

const hashSync = (data) => {
    const salt=bcrypt.genSaltSync(10)
    const result=bcrypt.hashSync(data, salt);
    return result
};

const compare = (data, hashdata) => {
    const result = bcrypt.compareSync(data, hashdata)
    return result
}

module.exports = {
    hashSync,
    compare
}
```



在 `src/middleware/user.middleware.js` 使用

```js
const { getUserInfo } = require("../service/user.service")
const { hashSync } = require("../utils/util/bcrypt")
const {
    userErrNameOrPwdNull,
    userErrAlreadHad,
    userErrRegisterErr
} = require("../constant/err.type")

//...
const userVerifyHashSync = async (ctx, next) => {
    const { password } = ctx.request.body
    ctx.request.body.password = hashSync(password)
    await next()
}
module.exports = {
    userVerifyNameOrPwdNull,
    userVerifyAlreadHad,
    userVerifyHashSync
}
```

# 九.登录验证

改写 `src/constant/err.type.js`

```js
module.exports = {
    userErrNameOrPwdNull: {
        code: 10001,
        message: "用户名或密码为空",
        result: ""
    },
    userErrAlreadHad: {
        code: 10002,
        message: "用户已经存在",
        result: ""
    },
    userErrRegisterErr: {
        code: 10003,
        message: "用户注册错误",
        result: ""
    },
    userErrNoHad:{
        code: 10004,
        message: "不存在当前用户",
        result: ""
    },
    userErrLoginErr:{
        code: 10005,
        message: "用户登录失败",
        result: ""
    },
    userErrPwdErr:{
        code: 10005,
        message: "用户名或密码错误",
        result: ""
    }
}
```

`src/middleware/user.middleware.js` 验证用户

```js
const { getUserInfo } = require("@service/user.service")
const { hashSync,compareSync } = require("@utils/util/bcrypt")
const {
    userErrNameOrPwdNull,
    userErrAlreadHad,
    userErrRegisterErr,
    userErrNoHad,
    userErrLoginErr,
    userErrPwdErr
} = require("../constant/err.type")

//...
const userVerifyHashSync = async (ctx, next) => {
    const { password } = ctx.request.body
    ctx.request.body.password = hashSync(password)
    await next()
}

const userVerifyLogin = async (ctx, next) => {
    const { username, password } = ctx.request.body
    try {
        const res = await getUserInfo({ username })
        if (!res) {
            console.error('用户名不存在', { username })
            ctx.app.emit('error', userErrNoHad, ctx)
            return
        }

        if (!compareSync(password, res.password)) {
            ctx.app.emit('error', userErrPwdErr, ctx)
            return
        }
    } catch (err) {
        console.error(err)
        return ctx.app.emit('error', userErrLoginErr, ctx)
    }

    await next()
}

module.exports = {
    userVerifyNameOrPwdNull,
    userVerifyAlreadHad,
    userVerifyHashSync,
    userVerifyLogin
}
```

# 十.用户认证

登录成功后, 给用户颁发一个令牌 token, 用户在以后的每一次请求中携带这个令牌.

jwt: jsonwebtoken

- header: 头部
- payload: 载荷
- signature: 签名

```npm
npm install jsonwebtoken
```

创建 `src/utils/util/jwt.js`

```js
const jwt = require('jsonwebtoken');
const config = require('../config');

//颁发token
const tokenSign = (data) => {//config.token.jwtExpiresTime
    const result = jwt.sign(data, config.token.jwtSecret, { expiresIn: config.token.jwtExpiresTime })
    return result
}

//验证token
const tokenVerify = (data) => {
    const result = jwt.verify(data, config.token.jwtSecret)
    console.log(result)
    return result
}

module.exports = {
    tokenSign,
    tokenVerify
}
```

## 1.登录颁发token

`src/controller/user.controller.js` 使用

```js
//登录接口   
async login(ctx, next) {
        const { username, password } = ctx.request.body
        try {
            // 从返回结果对象中剔除password属性
            const { password, ...res } = await getUserInfo({ username })

            ctx.body = {
                code: 0,
                message: '用户登录成功',
                result: {
                    token: tokenCarry(res),
                },
            }
        } catch (err) {
            console.error('用户登录失败', err)
        }
    }
```

## 2.验证token

改写 `src/constant/err.type.js`

```js
    tokenExpiredError:{
        code: 10101,
        message: "token已过期",
        result: ""
    },
    invalidToken:{
        code: 10102,
        message: "无效的token",
        result: ""
    }
```



创建 `src/middleware/auth.middleware.js`

```js
	const { tokenVerify } = require("@utils/util/jwt")
const {
    tokenExpiredError,
    invalidToken
} = require("@constant/err.type")

const userAuthToken = async (ctx, next) => {
    const { authorization } = ctx.request.header
    const token = authorization.replace('Bearer ', '')
    try {
        ctx.state.user = tokenVerify(token)
    } catch (err) {
        switch (err.name) {
            case 'TokenExpiredError':
                console.error('token已过期', err)
                return ctx.app.emit('error', tokenExpiredError, ctx)
            case 'JsonWebTokenError':
                console.error('无效的token', err)
                return ctx.app.emit('error', invalidToken, ctx)
        }
    }
    await next()
}

module.exports = {
    userAuthToken
}
```







