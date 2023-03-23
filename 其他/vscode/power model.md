在vscode中实现打字的动画效果

# 安装

扩展中搜索 `Power Model` 进行安装

安装完成之后查看该插件右侧设置中的`扩展设置`选项，打开并粘贴写下面配置

```json
 		//* 基础配置 */
        // 是否开启powermode
        "powermode.enabled": true,
        // 是否抖动
        "powermode.enableShake": false,
        // 设置随字体颜色变化
        "powermode.backgroundMode": "mask",
        // 设置时间间隔
        "powermode.comboTimeout": 0,
        /* 样式*/
        //需要哪个样式就把注释打开即可
        "powermode.presets": "particles", // 粒子
        // "powermode.presets": "flames",//火焰
        // "powermode.presets": "exploding-rift",// 炸裂
        // "powermode.presets": "simple-rift",// 爆炸
        // "powermode.presets": "fireworks",""// 烟花
        // "powermode.presets": "magic",// 魔法
        // "powermode.presets": "clippy",// 回形针
```

