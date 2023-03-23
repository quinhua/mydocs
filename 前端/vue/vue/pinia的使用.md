# 1.安装安装pinia
[pinia](https://pinia.web3doc.top/)
### 1.1 安装命令
```npm
npm install pinia
# 或者使用 yarn
yarn add pinia
# 或者使用 cnpm
cnpm install pinia
```
### 1.2 代码如下:
```js
// main.js

import { createApp } from "vue";
import App from "./App.vue";
import { createPinia } from "pinia";
const pinia = createPinia();

const app = createApp(App);
app.use(pinia);
app.mount("#app");
```

# 2.创建store
### 2.1 代码如下:
```js
/src/store/index.js

import { defineStore } from 'pinia'

//参数是 store 的唯一 id
export const useStore = defineStore('main', {
  state: () => {
    return {
      count: 0,
      name: "张三"
    }
  }
})
```

### 2.2.使用store
```ts
/src/App.vue

<template>
  <div>
    {{ count }}
    <br />
    <button type="primary" @click="changeNum()"> + </button>
    <br />
    {{ name }}
    <br />
    <n-button type="primary" @click="changeName()"> 改名 </button>
    <br />
    <nbutton type="primary" @click="patchStore()"> 批量修改 </button>
  </div>
</template>

<script setup lang="ts">
import { useStore } from "../stores/index";
const state = useStore();
import { storeToRefs } from "pinia";
// const { count,name }=state;//页面数据无需更改时使用,不是响应式。
const { count, name } = storeToRefs(state); //页面数据需要更改时使用,storeToRefs函数将state中的数据变为响应式。
const changeNum = () => {
  state.count += 1;
};
const changeName = () => {
  state.name = "李四";
};
const patchStore = () => {
  //store的$patch进行批量修改
  state.$patch({
    count: 1,
    name: "王五",
  });
};
</script>
```
# 3.getters属性
类似于Vue中的computed计算属性，作用就是返回一个新的结果。
```ts
/src/store/index.js

import { defineStore } from 'pinia'
export const useStore = defineStore('main', {
  state: () => {
    return {
      count: 0,
      name: "张三"
    }
  },
  getters: {
    //直接在getter方法中调用this，this指向的便是store实例
    getCount: (state) => {
      return state.count + 666;
    },
    getName: (state) => {
      return state.name + "666";
    },

    //调用其他getters,不使用箭头函数
    getNamePlus() {
      return this.name + this.getNum;
    }
  }
})
```

```ts
/src/App.vue

<template>
  <div>
    {{ state.getCount }}
    <br/>
    {{ state.getName }}
    <br/>
    {{ state.getNamePlus }}
  </div>
</template>

<script setup lang="ts">
import { useStore } from "../stores/index";
const state = useStore();
</script>
```
# 4.actions属性
类似于Vue中的methods方法属性,用来放置一些处理业务逻辑的方法,包括同步方法和异步方法。
```ts
/src/store/index.js

import { defineStore } from 'pinia'
export const useStore = defineStore('main', {
  state: () => {
    return {
      count: 0,
      name: "张三"
    }
  },
  actions: {
    //把actions方法当作一个普通的方法即可,this指向的是当前store。
    changeCount(count) {
      this.count = count;
    },
    changeName(name) {
      this.name = name;
    }
  },
})

```

```ts
/src/App.vue

<template>
  <div>
    {{ count }}
    <br />
    <n-button type="primary" @click="changeCount"> 修改count </n-button>
    <br />
    {{ name }}
    <br />
    <n-button type="primary" @click="changeName"> 修改name </n-button>
  </div>
</template>

<script setup lang="ts">
import { useStore } from "../stores/index";
const state = useStore();
import { storeToRefs } from "pinia";
const { count,name } = storeToRefs(state);
const changeCount = () => {
  state.changeCount(666);
};
const changeName = () => {
  state.changeName("李四");
};
</script>
```

# 5.持久化存储
使用pinia-plugin-persistedstate实现持久化存储。
[pinia-plugin-persistedstate](https://prazdevs.github.io/pinia-plugin-persistedstate/)
### 5.1安装
```npm
npm i pinia-plugin-persistedstate
# 或者使用 yarn
yarn add pinia-plugin-persistedstate
# 或者使用 pnpm
pnpm i pinia-plugin-persistedstate
```

### 5.2添加实例
```ts
// main.js

import { createApp } from 'vue'
import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
import App from './App.vue'

function setup() {
    const app = createApp(App)
    const pinia = createPinia()
    pinia.use(piniaPluginPersistedstate)
    app
        .use(pinia)

        .mount('#app')
}
setup()
```

```ts
/src/store/index.js

import { defineStore } from 'pinia'
export const useStore = defineStore('main', {
  state: () => {
    return {
      count: 0,
      name: "张三",
      save:[
        { id: '1', name: '李四' },
        { id: '2', name: '王五' },
        { id: '3', name: '赵六' }
      ]
    }
  },
  actions: {
    changeCount(count) {
      this.count = count;
    },
    changeName(name) {
      this.name = name;
    }
  },
  persist: {
    key:"test",//唯一密钥
    paths: ["count","save.[]"],//部分持久状态、持久化整个状态
    storage: localStorage,//sessionStorage、localStorage
  },
})
```
```js
/src/App.vue

<template>
  <div>
    {{ count }}
    <br />
    <n-button type="primary" @click="changeCount"> 修改count </n-button>
    <br />
    {{ name }}
    <br />
    <n-button type="primary" @click="changeName"> 修改name </n-button>
  </div>
</template>

<script setup lang="ts">
import { useStore } from "../stores/index";
const state = useStore();
import { storeToRefs } from "pinia";
const { count,name } = storeToRefs(state);
const changeCount = () => {
  state.changeCount(666);
};
const changeName = () => {
  state.changeName("李四");
};
</script>
```