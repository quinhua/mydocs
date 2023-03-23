# 安装jsx

```npm
npm install @vitejs/plugin-vue-jsx
```

# 配置 `vite.config.js`

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

export default defineConfig({
  plugins: [
    vue(),
    vueJsx()
  ]
})
```

# 配置`tsconfig.ts`

```ts
	"jsx": "preserve",
    "jsxFactory": "h",
    "jsxFragmentFactory": "Fragment",
```

# 修改 `main.js`

```js
import { createApp } from 'vue'
//import App from './App.jsx'
# 或 tsx
import App from './App.tsx'
createApp(App).mount('#app')
```

# 修改 `App.vue` 

将`.vue`改为 `.jsx`或`.tsx`

```js
import { defineComponent } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    return () => (
      <>
        <div>123</div>
      </>
    );
  },
});
# 或
import { defineComponent } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    const render=()=>{
      return (
        <div>123</div>
      )
    }
    return render;
  },
});
#################################
//app.vue
<template>
  <renderDom></renderDom>
</template>

<script setup lang="ts">
import { ref } from "vue"
import renderDom from "./components/demo"
</script>
//app.tsx
import { defineComponent } from "vue";
import renderDom from "./components/demo"

export default defineComponent({
  name: "App",
  component:{renderDom},
  setup() {
    return () => (
      <>
        <renderDom></renderDom>
      </>
    );
  },
});
//demo.tsx
const renderDom=()=>{
    return (
            <div>hello world</div>
    )
}
export default renderDom
```

# 注意

## 1.ref

`jsx/tsx`中用 `ref` 获取的返回值需要加上`.value`才能获取到

## 2.class

属性名class需要写成className

```ts
//./reset.css
.cred{
    color:red;
}

import { defineComponent } from "vue";
import style from  "./reset.css"
export default defineComponent({
  name: "App",
  setup() {
    return () => (
      <>
        <div className={style.cred}>123</div>
      </>
    )
  },
});
```

内联样式写法：`style={{属性:属性值}}`

```ts
import { defineComponent } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    return () => (
      <>
        <div style={{ color: "red" }}>123</div>
      </>
    )
  },
});

```

## 3.`v-if` 

`jsx/tsx`中无法使用`v-if` 使用`{布尔值 && 内容}`或三元运算

```ts
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    const flag1:boolean=true
    const flag2=ref<boolean>(false)
    return () => (
      <>
        {flag1 && < div>这个显示</ div> }
        {flag2.value && < div>这个不显示</ div> }
      </>
    )
  },
});
```

## 4.`v-for`  

`jsx/tsx`中无法使用`v-for` ,使用`.map` 

```ts
import { defineComponent, reactive } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    const list: Array<number> = reactive([1, 2, 3]);
    return () => (
      <>
        {
            list.map(item => (
         		 <li>值为：{item}</li>
        	))
  		}
        {
          	list.map(e=>{
                  return <div>{e}</div>
            })
        }
      </>
    )
  },
});
```

## 5.`v-on` 

`jsx/tsx`中无法使用`v-on` ,使用`onClick={函数名}`

```ts
import { defineComponent,ref,reactive } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    const cl = () => {
      console.log("我是按钮")
    };
    return () => (
      <>
        <button onClick={cl}>按钮</button>
      </>
    )
  },
});
```

## 6.v-bind

`jsx/tsx`在vue中不支持`v-bind`，但可以随便起名字

```ts
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    const arr:number[] =[1,2,3,4,5]
    const v:string="return"
    const fun=(res:string)=>{
      console.log("我是"+res)
    }
    return () => (
      <>
        <div onClick={fun.bind(this,v)}>{v}</div>
      </>
    )
  },
});
```

## 7.v-model

```ts
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "App",
  setup() {
    let val = ref("请输入...")
    return () => (
      <>
        <input type="text" v-model={val.value}  placeholder="123" />
      </>
    )
  },
});
```

## 8.v-slot

`jsx/tsx`中无法使用`v-slot`，使用`v-slots`

```ts
//demo.tsx
import { defineComponent, ref } from "vue";
export default defineComponent({
    setup(props, { slots }) {
        return () => (
            <>
                <h1>{slots.default ? slots.default() : "我是插槽"}</h1>
                <h2>{slots.bar?.()}</h2>
            </>
        );
    },
});
```

第一种

```ts
//app.tsx
 import { defineComponent } from "vue";
 import Demo from "./components/demo";
 export default defineComponent({
   components: { Demo },
   setup() {
     const slots = {
       bar: () => <span>这个会渲染到h2中</span>,
     };
     return () => (
       <>
         <Demo v-slots={slots}>
           <div>这个会渲染到H1中</div>
         </Demo>
       </>
     );
   },
 });
```

第二种

```ts
import { defineComponent } from "vue";
import Demo from "./components/demo";
export default defineComponent({
  name: "App",
  components: { Demo },
  setup() {
    const slots = {
      default: () => <div>这个会渲染到H1中</div>,
      bar: () => <span>这个会渲染到h2中</span>,
    };
    return () => (
      <>
        <Demo v-slots={slots} />
      </>
    );
  },
});
```

## 9.props

```ts
//demo.tsx
import { defineComponent, ref } from "vue";
export default defineComponent({
    props: ["propsData","sex"],
    setup(props) {
        return () => (
            <>
                <div>{props.propsData.age}</div>
                <div>{props.propsData.name}</div>
                <div>{props.sex}</div>
            </>
        );
    },
});
//app.tsx
import { defineComponent } from "vue";
import Demo from "./components/demo";

type UserInfo = {
  age: number,
  name: string
}

export default defineComponent({
  name: "App",
  components: { Demo },
  setup() {
    const userInfo: UserInfo = {
      age: 23,
      name: "张三"
    }
    return () => (
      <>
        <Demo propsData={userInfo} sex="男"/>
      </>
    );
  },
});
```

```ts
//demo.tsx
type Props = {
    title:string
}
const renderDom = (props:Props) => {
    return (
        <>
            <div>{props.title}</div>
            <button onClick={clickTap.bind(this,props.title)}>点击</button>
        </>
    )
}
const clickTap=(r:string)=>{
    console.log(r)
}
export default renderDom
//app.tsx
import { defineComponent,ref } from "vue";
import renderDom from "./components/demo"

export default defineComponent({
  name: "App",
  component:{renderDom},
  setup() {
    const v=ref<string>("123")
    return () => (
      <>
        <renderDom title={v.value}></renderDom>
      </>
    );
  },
});
```

