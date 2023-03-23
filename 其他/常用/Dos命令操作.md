# 打开命令提示符
> 菜单栏或win+r输入cmd回车

### 更换盘符(盘符:)
> e:

### 查看当前目录下的文件及文件夹(dir)
> dir
> /p 显示信息满一屏时，暂停显示，按任意键后显示下一屏
/o 排序显示。o后面可以接不同意义的字母
/w 只显示文件名目录名，每行五个文件名。即宽行显示
/s 将目录及子目录的全部目录文件都显示
/a 显示隐藏文件

### 进入文件夹(cd 文件夹的名字)
> cd imgs

### 返回到根目录(cd\)
### cd\

### 返回上一级目录(cd..)
> cd..

### 清空屏幕(cls)
> cls

### 创建文件夹(md 文件名程)
> md imgs

### 创建文件(type null>文件名、copy nul>文件名、cd>文件名、echo 内容>文件名)
> type nul>1.txt
> copy nul>2.txt
> cd>3.txt
> echo 123>4.txt

### 编辑txt文件(echo 内容>文件名、echo 内容>>文件名)
> echo 123>1.txt(添加内容)
> echo 123>>1.txt(追加内容)

### 查看文件内容(type 文件名)
> type 1.txt

### 删除文件夹(rd 文件夹名称)
> rd imgs
> rd /s imgs(删除文件夹及子目录)

### 删除文件(del 文件名)
> del tiger.jpg

### 重命名文件(ren 原文件名 新文件名)
> ren 1.jpg 2.jpg

### 退出(exit)
> exit