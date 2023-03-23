# 闭包长什么样子

```javascript
    function bb(a) {
        return function (b) {
            return a + b;
        }
    }
    var a=bb(10);
    var b=a(20);
    console.log(b);//30
    // 或
    var c=bb(10)(20)
    console.log(c);//30
```

> 上面的例子就是一个闭包。 我的理解是，闭包有这么几个元素:

1.最外层是一个有名字的函数，通常都需要传入参数或者在这一层定义一些变量。
2.这个有名字的函数返回一个匿名函数，通常都需要传入参数或者定义一些变量。
3.这个匿名函数返回的值通常跟上面两点中的入参或者变量有关。

>闭包就是能够读取其他函数内部变量的函数，在本质上是函数内部和函数外部链接的桥梁.

来看几个例子:

##  例一

```javascript
    var age = 24;
    function foo() {
        console.log(age);       //1
        var name = "林扬生";
        return function () {
            console.log(name);  //3
        }
    }
    console.log(name);          //2
    var bar = foo();
    bar();
```
函数内部肯定是可以访问到全局变量的值的，所以在foo()函数中去打印age肯定是可以打印出来的，就像代码中的标记1，但是呢在标记2处打印的函数内部的变量name是打印不出来的。

但是当我们用到闭包的时候，在foo函数内部返回一个函数，在返回的函数中去用到局部变量name，最后呢在外部调用，这时候的bar仍然在函数外部，但是完全可以拿到局部变量的name值。这就是闭包的定义吧，同样呢也是闭包的第一个特点。

## 例二

```javascript
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>

    var lis = document.getElementsByTagName("li");
    for(var i=0;i<lis.length;i++){
        (function(i){
            lis[i].onclick = function(){
                console.log(i);
            };
        })(i);
    }
```

这个是在事件处理中闭包的应用，也就是说我们按照传统的思想去写的话，相信我不管点击第几个都会给你打印5的，但是闭包处理过的就可以实现我们想要的功能。
> 但是在这我要说句题外话：在实际开发中我是不会这样使用的,太复杂了,我会首选使用let。