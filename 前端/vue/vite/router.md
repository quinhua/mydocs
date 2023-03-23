# 安装

```npm
npm install vue-router@4
```
```js
import { createRouter, createWebHistory, createWebHashHistory, createMemoryHistory, RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
    {
        path: '/',
        redirect: "/a"
    },
    {
        path: '/a',
        name:"a",
        component: () => import('../components/demo1.vue')
    }, {
        path: '/b',
        name:"b",
        component: () => import('../components/demo2.vue')
    }]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router;
```

# 使用

原来的vue2路由是通过this.$route和this.$router来控制的。现在vue3有所变化，useRoute相当于以前的this.$route，而useRouter相当于this.$router

## 通过useRouter来手动控制路由变化

```js
<script setup lang="ts">
	import { useRouter } from "vue-router"
	const route=useRouter()
	route.push("/b")
</script>	
```

## 通过useRouter传参的三种方式

### 隐式传参`params`

版本大于`4.1.4`，官方已删除隐式传参！！！

`params`用作动态传参！！！

### 显式传参`query`

```js
<script setup>
	import { useRouter } from "vue-router"
	const route=useRouter();
	route.push({
			path: '/b',
			query: {
				name: '张三',
				age: 23
			}
		})
</script>	
```

path与query是一对，name和params是一对，不要混用。

通过useRoute来接收query参数

```js
<script setup>
	import { useRoute } from "vue-router"
	const route=useRoute();
	console.log(route.query)
</script>	
```

- 显式query会很明显的跟在新的url上，而隐式params不会
- 隐式params在刷新后可能会消失，我们可以在它存在的时候存如缓存中，如localstorage
- 隐式params比显式query相对而言更安全，不会将参数直接暴露给用户
- 显示query在浏览器的url上，如果你直接通过字符串的方式去取，可能会涉及转码等问题，当然useRoute将这些都处理好了，所以还是推荐通过useRoute.query去取显式路由的参数