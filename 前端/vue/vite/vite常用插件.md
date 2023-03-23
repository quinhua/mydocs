# 1.iconify图标

## 1.安装

[unocss](https://uno.antfu.me/)
[iconify](https://icon-sets.iconify.design/)

```npm
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
//z
<template>
    <div i-akar-icons:full-screen></div>
    <div i-emojione:dog></div>
    <div i-akar-icons:music></div>
    <div i-emojione:first-quarter-moon-face></div>
    <div i-emojione:full-moon-face></div>
</template>
```

# 2.@指向src

## 1.配置

找到文件vite.config.js,写入以下配置即可

```js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import { resolve } from "path";
const pathResolve = (dir) => resolve(__dirname, dir);

export default defineConfig({
  plugins: [
    vue()
  ],
  resolve: {
    alias: {
      "@": pathResolve("./src") // 新增
    }
  },
})
```

## 2.使用

```js
import "@/assets/styl/index.scss";
```

# 3.env环境变量

## 1.创建

在根目录下创建两个文件,分别为`.env.development`开发模式和` .env.production`生产模式。

```js
//.env.development
//VITE_开头
VITE_TITLE=第一个网页
```

```js
//.env.production
//VITE_开头
VITE_TITLE=第一个网页
```

## 2.配置

```js
//package.json

  "scripts": {
    "dev": "vite --mode development",
    "build": "vite build --mode production"
  },
```

## 3.使用

```
console.log(import.meta.env.VITE_TITLE);
```

## 4.运行

```npm
npm run dev
# or
npm run build
```

# 4.mitt事件总线

vue3中取消了事件总线，所以用`mitt`插件事件总线传递数据

## 1.安装

```npm
npm install mitt
```

## 2.使用

`A.vue`、`B.vue`互为兄弟组件，`app.vue`父组件

```ts
//src/utils/bus   main.jsdao'ru
import mitt from "mitt";
const bus = {};
const emitter = mitt();
bus.on = emitter.on;
bus.off = emitter.off;
bus.emit = emitter.emit;
 
export default bus
```

```ts
//src/views/A.vue
<template>
  <h1>A</h1>
  <button @click="emit">emit</button>
</template>

<script setup lang="ts">
import { reactive, getCurrentInstance } from "vue"
import bus from '../utils/bus'
const instance = getCurrentInstance()
type M = {
  name: string
  age: number
}
const val: M = reactive({
  name: "张三",
  age: 23
})
const emit = () => {
  const { name, age } = val
  bus.emit("on-emit1", { name, age })
  bus.emit("on-emit2", { name, age })
  bus.emit("on-emit3", { name, age })
  bus.emit("on-emit4", { name, age })
}
</script>
```

```ts
//src/views/B.vue
<template>
  <h1>B</h1>
</template>

<script setup lang="ts">
import { getCurrentInstance } from "vue"
import bus from '../utils/bus'
const instance = getCurrentInstance()
//触发事件
bus.on("on-emit", (res) => {
  console.log(res)
})
//*表示监听所有的事件触发
bus.on("*", (res) => {
  console.log(res)
})
</script>
```

```ts
//src/app.vue
<template>
  <div>
    <A></A>
    <B></B>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue"

import bus from './utils/bus'
import  A from "./components/A.vue"
import  B from "./components/B.vue"
</script>
```

# 5.自动导入

## 1.安装

```npm
npm install -D unplugin-auto-import
```

## 2.配置vite.config.js

```js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import AutoImport from 'unplugin-auto-import/vite'

export default defineConfig({
  plugins: [
    vue(),
    AutoImport({
      imports:["vue","vue-router"],//自动导入的依赖名字
      dts:"src/global.d.ts"//生成的文件夹/名字
    })
  ]
})
```

# 6.svg自动导入

## 1.安装
```npm 
npm i vite-plugin-svg-icons -D
```
## 2.配置文件
```js
//vite.config.js
import { defineConfig } from 'vite'
import { resolve } from "path";
const pathResolve = (dir) => resolve(__dirname, dir);
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'

export default defineConfig({
  plugins: [
	    vue(),
	    createSvgIconsPlugin({
	        iconDirs: [resolve(process.cwd(), 'src/assets/svg')],
	        symbolId: 'icon-[dir]-[name]',
	     })
	  ]
})
```
## 3.封装组件
```html
<template>
  <div :style="{ width: `${props.width}`, height: `${props.height}` }">
    <svg
      aria-hidden="true"
      :class="props.name"
      :size="props.size"
      :style="{ width: `${props.width}`, height: `${props.height}` }"
    >
      <use :xlink:href="symbolId" rel="external nofollow" :fill="props.color" />
    </svg>
  </div>
</template>
<script setup>
import { computed } from "vue";
const props = defineProps({
  prefix: {
    type: String,
    default: "icon",
  },
  name: {
    type: String,
    required: true
  },
  color: {
    type: String,
    default: "#333",
  },
  size: {
    type: String,
    default: "1em",
  },
  width: {
    type: String,
    default: "1em",
  },
  height: {
    type: String,
    default: "1em",
  },
});
const symbolId = computed(() => `#${props.prefix}-${props.name}`);
</script>
```
## 4.main.js
```js
import { createApp } from 'vue'
import App from './App.vue';
import 'virtual:svg-icons-register';//注册组件
import svgIcon from "@/components/icon.vue";//组件
function setup() {
    const app = createApp(App)
    app
        .component('icon', svgIcon)
        .mount('#app')
}
setup()
```
## 5.使用
```js
<Icon name="home" width="400px" height="400px"  flex center></Icon>
```