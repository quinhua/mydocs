## 1.设置`PowerShell默`认启动Windows Terminal

```bash
1.从`Microsoft Store`下载并打开`Windows Terminal`
2.点击上方的下拉三角，点击设置，在启动中默认终端应用程序设置为`Windows`终端
```

## 2.开启毛玻璃主题

[官方教程](https://ohmyposh.dev/docs/installation/windows)

- 1.从`github`下载[Cascadia Code PL](https://github.com/microsoft/cascadia-code/releases)字体，如果`github`访问不了可以从[这里](https://qianhuiya.lanzouy.com/iGKxt0r5urxi)下载
- 2.将字体包解压，并将`otf/static/`下所有的文件复制到`C:\Windows\Fonts`下，会自动安装字体
- 3.打开`Windows PowerShell`，点击 设置 => 打开`JSON文件`，修改`profiles` => `list` 下`name`为`Windows PowerShell`的配置,
  主要修改或添加`useAcrylic，acrylicOpacity，colorScheme，cursorColor，face`这几项

```json
{
        "commandline": "%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
        "hidden": false,
        "name": "Windows PowerShell",
		"font": 
     	{
        	"face": "Cascadia Code PL"
     	},
		"useAcrylic" : true,
     	"acrylicOpacity": 0.7,
     	"colorScheme" : "Frost",
     	"cursorColor" : "#000000"
},
```

- 4.在`schemes`下添加`Frost`主题

  ```json
  {
        "name" : "Frost",
        "background" : "#FFFFFF",
        "black" : "#3C5712",
        "blue" : "#17b2ff",
        "brightBlack" : "#749B36",
        "brightBlue" : "#27B2F6",
        "brightCyan" : "#13A8C0",
        "brightGreen" : "#89AF50",
        "brightPurple" : "#F2A20A",
        "brightRed" : "#F49B36",
        "brightWhite" : "#741274",
        "brightYellow" : "#991070",
        "cyan" : "#3C96A6",
        "foreground" : "#000000",
        "green" : "#6AAE08",
        "purple" : "#991070",
        "red" : "#8D0C0C",
        "white" : "#6E386E",
        "yellow" : "#991070"
  }
  ```

  ## 3.安装oh-my-posh

  [官方教程](https://ohmyposh.dev/docs/installation/windows)
  在`Windows Terminal`中输入以下命

  ### 1.安装

  ```bash
  # 安装oh-my-posh
  # 如果该目录下没有winget.exe需要先到微软应用商店下载安装一下
  # 如果执行此命令下载失败，可以多试几次
  winget install JanDeDobbeleer.OhMyPosh -s winget
  
  # 打开配置文件
  notepad $PROFILE
  
  # 如果执行上面的命令时提示文件不存在，先执行下面的命令新建一个，然后再执行上面的命令打开配置文件
  New-Item -Path $PROFILE -Type File -Force
  
  # 将下面的命令复制到配置文件中保存并关闭
  oh-my-posh init pwsh | Invoke-Expression
  ```
  ### 2.更新
  
  ```bash
  # 更新oh-my-posh
  winget upgrade JanDeDobbeleer.OhMyPosh -s winget
  ```
  
  ### 3.卸载
  
  ```bash
  # 移除缓存文件
  Remove-Item $env:POSH_PATH -Force -Recurse
  # 卸载oh-my-posh
  Uninstall-Module oh-my-posh -AllVersions
  ```
  
  ### 4.异常情况
  
  ```bash
  # 1.提示无法加载文件 C:\Users\87897\Documents\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本，执行下面的命令更换脚本执行策略,然后输入Y
  set-ExecutionPolicy RemoteSigned
  
  # 2.提示无法将“oh-my-posh”项识别为 cmdlet、函数、脚本文件或可运行程序的名称
  重启电脑让环境变量生效即可解决
  ```
  
  ### 5.更换主题
  
  ```bash
  1. 可以使用`Get-PoshThemes`命令查看可用主题，官方教程中也展示了所有可用的主题及其展示效果
  2.通过`notepad $profile`命令打开配置文件，并使用下面的命令覆盖，用户名替换为自己的用户目录，这里我使用的是`M365Princess`主题，也可替换成其他主题，只需要在环境变量 C:\Users\用户名\AppData\Local\Programs\oh-my-posh\themes\ 下找到合适的主题文件后，将`M365Princess.omp.json`替换掉即可
  ```
  
  > oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH/M365Princess.omp.json" | Invoke-Expression


​    