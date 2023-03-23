---
dg-publish: true
dg-home: true
---

```

# 1.安装
[vueuse](https://vueuse.org/)

```npm
npm i @vueuse/core
# 或者使用 cdn
<script src="https://unpkg.com/@vueuse/core"></script>
```
# 2.使用
## 2.1切换暗黑主题
```js
//首先设置全局样式
html.dark{
  background: #333;
}
html.dark button {
  background: pink;
  border: none;
}
//组件中使用
<template>
   <button icon-btn @click="toggleDark()">
      切换
    </button>
    <button>看我</button>
</template>

<script setup lang="ts">
import { defineComponent, ref } from "vue";
import { useDark, useToggle } from '@vueuse/core'

const isDark = useDark({
  selector: "html",
  valueDark: "dark",
  valueLight: "light"
});
const toggleDark: any = useToggle(isDark)
</script>
```

## 2.2存储

```ts
//store.tsx
import { createGlobalState, useStorage } from '@vueuse/core'
const useGlobalState = createGlobalState(
    () => useStorage('quinhua-token', {
        token:"Bear fjksdlfjef"
    }
    ))
export {
    useGlobalState
}
```

```ts
//app.tsx
import { defineComponent, ref } from "vue";
import { useGlobalState } from "./store"

export default defineComponent({
  name: "App",
  setup() {
    const state1 = useGlobalState()
    const state2 = () => {
      console.log(JSON.parse(JSON.stringify(state1.value)))
    }
    const state3 = () => {
      localStorage.setItem("quinhua-token", JSON.stringify({ name: "李四", age: 24 }))
    }
    return () => (
      <>
        <button onClick={state2}>打印</button>
        <button onClick={state3}>修改</button>
      </>
    );
  },
});
```

##  2.3取消ref的value

```ts
import { defineComponent } from 'vue'
import { ref} from "vue"
import { get } from '@vueuse/core'
export default defineComponent({
  name: 'App',
  setup() {
    const a = ref<string>("张三")
    const b = get(a)
    return () => (
      <>
        <div>{b}</div>
      </>
    )
  }
})
```

