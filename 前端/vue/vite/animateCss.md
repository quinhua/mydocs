# 安装

```npm
npm install animate.css
```

# 使用

```html
    <router-view v-slot="{ Component }">
      <transition name="animate_slide">
        <component :is="Component" />
      </transition>
    </router-view>
```

```css
.animate_slide-enter-active {
  animation: slideInLeft 0.7s;
}
.animate_slide-leave-active {
  animation: slideOutLeft 0.3s;
}
```

