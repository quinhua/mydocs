# 变量
```css
$color-4481FE: #4481FE;

div{
   background:$color-4481FE;
}

```
# 父选择器
```css
$color-4481FE: #4481FE;

div{
   &:hover{
     background:$color-4481FE;
   }
}

```
# 属性嵌套
```css
div {
  font: {
    size: 10px;
    family:Source Han Sans CN;
    weight:500;
  }
}

```
# 基础选择器 @extend
```css
.box{
  color:red
}

div{
  @extend .box;
  height: 90px;
}


```
# 混入
```css
// 无参
@mixin flex {
    display: flex;
    justify-content: center;
    align-items: center;
}

div{
   @include flex;
   width:120px;
}

// 有参
@mixin flex($direction:row) {
    display: flex;
    flex-direction: $direction;
    justify-content: center;
    align-items: center;
}

div{
   @include flex(column);
   width:120px;
}
```
# 函数
```css
@function total($n){
    @return 20 * $n;
}

div{
 width: total(40)px;
}
```