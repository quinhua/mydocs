[heX官网首页](http://hex.youdao.com/)
# 1. 下载 heX 二进制包

[二进制包](http://hex.youdao.com/zh-cn/downloads/index.html)
首先选择一个合适的二进制包，对于 web 前端开发者而言，heX web 开发者发行包肯定是最好的选择。
将二进制包解压到本地后，`hex/Release/hexclient.exe` 是主程序文件，双击即可运行 heX，只不过此时打开的是 heX 的默认欢迎页面chrome://version，里面描述了一些基本信息。

# 2. 编写 web 前端代码
在`hex/Release`主程序文件所在的目录下创建一个用于写 Hello World 程序的测试目录test，同时在其中新建 HTML、CSS、JavaScript 等文件。如：
![1663503515769](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/1663503515769.73j0kp6qvfk0.webp)
`test/html/index.html`
```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>林扬生</title>
  <link rel="stylesheet" href="../css/index.css">
</head>
<body>
<img id="img" src="data:image/webp;base64,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" alt="">
  <script>
    require('./test/js/index');
  </script>
</body>
</html>
```

`test/css/index.css`
```css
body {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
#img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  -webkit-user-drag: none;
  cursor: pointer;
}
```
`test/js/index.js`
```js
window.setTimeout(function () {
    document.getElementById("img").style.borderRadius="10%";
  }, 1000);
```

# 3. 修改 manifest.json
想要个性化定制 heX，则需要从 manifest.json 开始。
这里我们需要修改 first_page 参数，将其修改为$(AppDir)test/html/index.html，即将应用程序执行入口改为上面编写的 web 页面。
```json
{
    "first_page": "$(AppDir)test/html/index.html",       // 首页，可以是 URL 或者一个本地文件路径,$(AppDir) 代表当前程序路径（包含“/”）
    "application_title": "qianhuiya",      // 程序默认标题，alert 等窗口使用
    "application_shortname": "qianhuiya",        // 应用程序别名
    "use_grit_package": true,               // 使用打包资源
    "icon_path": "",                        // 程序默认图标路径
    "use_node": true,                       // 是否开启 Node.js
    "version": "1.0",                       // 程序版本信息
    "locale": "zh-CN",                      // 浏览器区域设置
    "multiple_process": false,              // 是否为多进程模式
    "launch_node_in_all_pages": false,      // 在打开的所有页面中使用 Node.JS
    "load_node_manually": false,            // 是否手动加载 Node.js
    "disable_async_node_apis": false,       // 是否禁用 Node.JS 异步 API
    "remote_debugging_port": 65432,         // 远程调试端口
    "disable_debug_log": true,              // 是否禁止生成 Chromium 调试信息
    "quit_after_main_window_closed": false, // 是否在主窗口关闭后退出
    "cache_path": "data",                   // 缓存路径
    "npapi_plugin_directory": "",           // NPAPI 插件路径
    "disable_ime_composition": false,       // 禁用 IME composition
    "extensions": [                         // heX 扩展名称列表
      "hex_dialog",
      "hex_sleep",
      "hex_shortcut"
    ],
    "extension_path": "",                   // heX 扩展的路径
    "single_instance": true,                // 是否为单一实例模式
    "window_class_name": "qianhuiya",     // 主窗口类名
    "form": {
        "style": "captionless",             // 窗口类型：标准、无标题、桌面 Widget
        "plain": false,                     // 是否为扁平窗口
        "system_buttons": true,             // 是否显示默认的系统控制按钮
        "transmission_color": "none",       // 穿透颜色
        "transparent_browser": true,        // 是否为透明浏览器
        "fixed": false,                     // 窗体是否可以调整大小
        "disable_form_apis": false,         // 是否禁用所有窗口相关 API
        "opacity": "none",                  // 窗口透明度
        "hook_system_command": false,       // 是否拦截窗口的系统命令
        "launch_state": "normal",           // 启动初始状态
        "launch_width": 800,                // 启动初始宽度
        "launch_height": 600,               // 启动初始高度
        "launch_x": "screen_centered",      // 启动初始 X 轴位置
        "launch_y": "screen_centered",      //  启动初始 Y 轴位置
        "min_width": 0,                     // 最小宽度
        "min_height": 0,                    // 最小高度
        "max_width": 0,                     // 最大宽度
        "max_height": 0,                    // 最大高度
        "border_width": 5                   // 模拟边框区域宽度
    },
    "browser": {
        "no_proxy_server": false,
        "winhttp-proxy-resolver": false,
        "disable_gpu": true,
        "disable_3d_apis": false,
        "disable_databases": false,
        "disable_experimental_webgl": false,
        "disable_file_system": false,
        "disable_geolocation": false,
        "disable_gpu_process_prelaunch": true,
        "disable_java": false,
        "disable_javascript": false,
        "disable_javascript_i18n_api": false,
        "disable_local_storage": false,
        "disable_logging": false,
        "disable_plugins": false,
        "disable_renderer_accessibility": false,
        "disable_session_storage": false,
        "disable_speech_input": false,
        "disable_web_sockets": false,
        "in_process_gpu": false,
        "in_process_plugins": false,
        "enable_media_stream": true,
        "web_security_disabled": true,
        "file_access_from_file_urls_allowed": true,
        "universal_access_from_file_urls_allowed": true
    }
}
```
# 4. 运行程序
双击 `hex/Release/hexclient.exe`，窗口显现出现图片。