# 1.不能连接

### 问题:

```sql
ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost:3306' (10061)
```

![rgbedtgghrftbydgtg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rgbedtgghrftbydgtg.1aqo1k46dxk0.webp)

### 解决办法：

```sql
mysqld --remove mysql，然后手动把data文件夹和my.ini文件删除了
mysqld --install （安装mysql）
mysqld --initialize --user=root --console （初始化mysql,生成出事密码）
net start mysql （启动mysql）
mysql -u root -p （进入mysql,输入初始密码）
set password=‘password’; （设置密码）
```

