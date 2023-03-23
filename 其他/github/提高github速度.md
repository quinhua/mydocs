<!--
 * @Author: quinhua 2867463524@qq.com
 * @LastEditTime: 2022-09-06 10:11:09
 * @Description: 
 *   Blog:  http://qianhuine.top:8090/
 *   Github:  https://github.com/quinhua
 *   不忘初心，方得始终。
 * Copyright (c) 2022 by quinhua 2867463524@qq.com, All Rights Reserved. 
-->
# GitHub加速下载项目的方法

国内在github上克隆项目总是异常的慢，据我多次克隆观察，下载速度最快就20k/s左右，特别是在克隆比较大的项目时简直慢得无法忍受！下面介绍一种加载克隆项目的方法。
## 利用码云来转接做下载加速

1. 首先你得有一个 [码云](https://gitee.com/) 的账号

2. 登录码云之后在页面右上角的加号选择`从GitHub/GitLab导入项目`

3. 选择`从URL导入`，粘贴从GitHub复制来的仓库地址，然后导入，这个导入过程一般是很快的。
4. 从码云克隆刚导入的这个项目，克隆速度会快很多，网速好的能达到几兆每秒（具体速度就看你的网速了，吐槽一下我家网速，总在关键时刻显示"视频加载中"....）

5. 另外要注意的一点，克隆下来的项目关联的是码云的仓库，如果你需要关联github仓库需要更改远程仓库。

   ```bash
   git remote -v # 查看关联的远程仓库
   git remote rm <仓库名> # 删除远程仓库
   git remote add <仓库名> <远程仓库地址> # 关联远程仓库，仓库名一般使用origin
   ```
这个方法适合用于克隆比较大的项目，如果克隆小项目，20k/s的速度好像还能将就~~
## 利用工具进行加速(fastgithub)
github地址：[fastgithub](https://github.com/dotnetcore/FastGithub)
[点此下载安装](https://objects.githubusercontent.com/github-production-release-asset-2e65be/375939072/637edf98-6d5a-4df4-922b-359f9f5f72f1?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20220906%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220906T021010Z&X-Amz-Expires=300&X-Amz-Signature=4176ad72bdacaf24c8e73765190d2d06c42419ef0300f72607245c151a7dd3a4&X-Amz-SignedHeaders=host&actor_id=49524392&key_id=0&repo_id=375939072&response-content-disposition=attachment%3B%20filename%3Dfastgithub_win-x64.zip&response-content-type=application%2Foctet-stream)


