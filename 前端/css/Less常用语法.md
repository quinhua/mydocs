# 安装
>npm install less -g

>vscode安装扩展 Easy LESS 进行实时编译.less文件,生成到同目录文件夹中。

# 变量
```css
@theme-color:red;

.box{
   background: @theme-color;
}
```
# 父级选择
```css
.box{
   width:200px;
   &:hover{
      color:@theme-color;
   }
}
```
# 嵌套
```css
body {
    margin:0;
    padding: 0;
    .div {
        width: 200px;
        height: 200px;
        background: #000;
    }
}
```
# 计算
```css
    .div {
        width: 200px+20*2px;
        height: 200px;
        background: #000;
    }
```
# 选择器和属性名
```css
@mydiv:div;
@mywidth:width;

.@{mydiv}{
    @{mywidth}:200px;
    height:200px;
    background: #000;
}

```
# 普通混合
```css
.bar{
    background: #fff;
}

.div{
    width: @width;
    height: @height;
    background: @bgcolor;
    &:hover{
        .bar
    }
}
```
# 带参数的混合
```css
.width(@width:200px){
    width:@width;
}
.mini(@radius:5px;@padding:10px){
    border-radius: @radius;
    padding: @padding;
}
.div{
    .width();//空为默认值200px
    //.width(300px);
    height:200px;
    background: #000;
    .mini();//空为默认值5px、10px
    //.mini(50%,0);
}

```
# 命名空间
```css
#main(){
    .reset{
        margin:0;
        padding: 0;
    }
    .bgcolor{
        background: #000;
    }
}

body {
    #main>.reset;//也可写为#main.reset();
    .div {
        width: 200px;
        height: 200px;
        #main>.bgcolor;
    }
}

```
# 映射
>自定义键值对，混合和规则集作为一组值的映射使用，与数组对象类似。
```css
   #main(){
      bgcolor:#000;
   }
   .div {
        width: 200px;
        height: 200px;
        background: #main[bgcolor];
   }
```