# 安装

```npm
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

#  创建配置文件

接下来，生成 `tailwind.config.js` 和 `postcss.config.js` 文件：

```npm
npx tailwindcss init -p
```

会看到在项目根目录创建了一个 `tailwind.config.cjs` 文件：

```js
// tailwind.config.cjs
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

一个 `postcss.config.cjs` 文件：

```js
// postcss.config.cjs
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

# 配置`tailwind.config.cjs`文件

配置 Tailwind 来移除生产环境下没有使用到的样式声明

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

# 创建`index.css`文件

```css
/* ./src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

# 引入到`main.js`

```js
import { createApp } from 'vue'
import App from './App'
import "./index.css"

async function setupApp() {
    const app = createApp(App)
    app.mount('#app')
}
setupApp()
```

# 插件`Tailwind CSS IntelliSense`

`vscode`中搜索`Tailwind CSS IntelliSense` 进行安装，编写`Tailwind CSS`会有提示