# sleep使用场景
>很多语言里都有 sleep()，delay() 等方法，
它可以让我们的程序等待 N 秒后再进行后续的操作。
js 中有 setTimeout() 方法来实现设定一段时间后执行某个任务，但写法很丑陋，需要提供回调函数：
```javascript
setTimeout(function(){
    //...
}, 3000);
```
### sleep实现
```javascript
(async function () {
    function sleep(time) {//核心代码
        return new Promise((resolve) => setTimeout(resolve, time));
    }
    console.log(' --------- begin, ' + new Date());
    await sleep(1000 * 3);
    console.log(' --------- over, ' + new Date());
})();
```