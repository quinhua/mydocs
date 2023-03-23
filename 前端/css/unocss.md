[UnoCss文档](https://uno.antfu.me/)

[playground](https://unocss.antfu.me/play/)

## 1.安装

```npm
npm i -D unocss
# 或者使用 cdn
<script src="https://cdn.jsdelivr.net/npm/@unocss/runtime/attributify.global.js"></script>
//安装unocss以及图标库
npm i -D unocss @unocss/preset-icons @iconify/json
```

## 2.配置vite.config.js

```js
import { defineConfig } from 'vite'

import Unocss from 'unocss/vite';
import { presetUno, presetAttributify, presetIcons } from 'unocss';
import UnocssIcons from '@unocss/preset-icons'

export default defineConfig({
  plugins: [
    Unocss({
      //自定义规则
      rules:[
        ["flex",{display:"flex"}],
        ["red",{color:"red"}]
      ],
      //配置图标
      presets: [
        UnocssIcons({
          prefix: 'i-',
          extraProperties: {
            display: 'inline-block'
          }
        }),
        presetUno(),
        presetAttributify(),
        presetIcons(),
      ],
    })
  ]
})
```

## 3.使用

```js
//main.js导入
import 'uno.css'
```

```js
<template>
    <div w-screen h-screen bg-red >
        //jsx/tsx中必须加class
         <div i-akar-icons:full-screen ></div>
    </div>
    <div i-akar-icons:full-screen></div>	
    <div i-emojione:dog></div>
    <div i-akar-icons:music></div>
    <div i-emojione:first-quarter-moon-face></div>
    <div i-emojione:full-moon-face></div>
</template>
```

### 尺寸

> xs、sm、md、lg、xl

### other
> .select-none .absolute .relative .fixed
> .overflow-hidden


### 内外边距
>(可省-) .m-0 .ma .m-1(0.25rem) .m-2(0.2rem)

>.ml .ml-0 .ml-1(0.25rem) .ml-2(0.5rem)
.mr .mr-0 .mr-1(0.25rem) .mr-2(0.5rem)
.mt .ml-0 .mt-1(0.25rem) .mt-2(0.5rem)
.mb .mb-0 .mb-1(0.25rem) .mb-2(0.5rem)

>(可省-) .p-0 .pa .p-1(0.25rem) .p-2(0.5rem)


>.pl .ml-0 .pl-1(0.25rem) .pl-2(0.5rem)
.pr .pr-0 .pr-1(0.25rem) .pr-2(0.5rem)
.pt .pt-0 .pt-1(0.25rem) .pt-2(0.5rem)
.pb .pr-0 .pb-1(0.25rem) .pb-2(0.5rem)

### top、bottom、left、right
> .top-0 .top-1(0.25rem) .top-2(0.5rem) .top-4(1rem)
> .bottom-0 .bottom-1(0.25rem) .bottom-2(0.5rem) .bottom-4(1rem)
> .left-0 .left-1(0.25rem) .left-2(0.5rem) .left-4(1rem)
> .right-0 .right-1(0.25rem) .right-2(0.5rem) .right-4(1rem)

### width(height同)
> .w-0 .w-1(0.25rem) .w-2(0.5rem)

> .w-xs(20rem) .w-sm(24rem) .w-md(28rem) .w-md(32rem) .w-xl(36rem)

> .-w-xs(20rem) .-w-sm(24rem) .-w-md(28rem) .-w-md(32rem) .-w-xl(36rem)

> .w-a .w-screen .w-full

###  background
> .bg-black .bg-blue .bg-red .bg-violet

>.bg-repeat
### opacity
> .op10 .op20 .op100
### border
> .b .b-1

>  .b-none .b-dashed .b-solid .b-dotted .b-double

> .b-rd .rd .b-rd-0 .b-rd-1 .b-rd-2
### z-index
> .z-10 .z-100 .-z-10 .-z-100
### flex
> .flex .flex-col .flex-col-reverse .flex-row .flex-row-reverse .flex-wrap .flex-nowrap

> .flex-1 .flex-auto

> .justify-start .justify-end .justify-center


> items-start .items-end .items-center

### text
> .text-1(0.25rem) .text-4(1rem) .text-8(2rem)
> .text-start .text-center .text-end .text-left .text-right

> .text-black .text-blue .text-red .text-violet

> .text-1 .text-2xl .text-3xl

### font
> .fw-100 .fw-200 .fw-300

### animate
#### animate-bounce
> -in、-out、-alt、-in-left、-in-right、-in-up、-in-down、
-out-left、-out-right、-out-up、-out-down
#### animate-back
>-in-down、-in-up、-in-left、-in-right
#### animate-rotate
> -in、-in-down-left、-in-down-right、-in-up-left、-in-up-right
#### animate-fade-in
> -bottom-left、-bottom-right

### cursor
> -none 、-pointer、-wait、-crosshair 、-text

### hover
> hover=''
