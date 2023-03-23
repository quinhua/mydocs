```java
//所有的IO流都是从以下四个抽象基类，超级父类中分出来的：
（1）字节输入流：InputStream
（2）字节输出流：OutputStream
（3）字符输入流：Reader
（4）字符输出流：Writer

//可以延伸出很多IO流
//和文件相关
（1）文件字节输入流：FileInputStream
（2）文件字节输出流：FileOutputStream
（3）文件字符输入流：FileReader
（4）文件字符输出流：FileWriter

//缓冲流
（1）字节输入缓冲流：BufferedInputStream
（2）字节输出缓冲流：BufferedOutputStream
（3）字符输入缓冲流：BufferedReader
（4）字符输出缓冲流：BufferedWriter

//转换流
（1）InputStreamReader：把字节输入流转为字符输入流，解码
（2）OutputStreamWriter：把字符输出流转为字节输出流，编码

//数据流
（1）字节输入数据流：DataInputStream
（2）字节输出数据流：DataOutputStream

//对象流
（1）对象输入流：ObjectInputStream，用于对象的序列化
（2）对象输出流：ObjectOutputStream，用于对象的反序列化

//打印流
（1）字节打印流：PrintStream
（2）字符打印流：PrintWriter
```

## File

#### File类

![byh456yq345567u](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/byh456yq345567u.2zucqd5dg5q0.webp)

```java
File类 : 表示计算机中所有的文件和文件夹; [计算机硬盘上除了文件就是文件夹]

如何创建File对象 :
    File(String pathname) : 传入文件路径[String],创建File对象并指向这个路径的文件/文件夹
    File(String parent, String child) :传入文件路径[String],创建File对象并指向这个路径的文件/文件夹 [文件路径拆分成了父路径和子路径]
    File(File parent, String child) :  以子父路径的形式创建一个File对象但是File对象的父路径是File类型  

    创建File对象做了哪些事情 :
        1. 创建File对象
        2. 在堆内存中开辟了内存空间
        3. 文件不存在 : 不会创建文件
        4. 文件存在 : 不会覆盖源文件
        5. 创建File对象只会让一个File类型的对象指向路径结尾的文件/文件夹 [其他的事情什么都没有发生] 
```

![rgfvr5tg4e2365y](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rgfvr5tg4e2365y.6qsdcg4meow0.webp)

#### File类的静态常量

```java
static String pathSeparator -> ;
          与系统有关的路径分隔符，为了方便，它被表示为一个字符串。 
static char pathSeparatorChar -> ;
          与系统有关的路径分隔符。 
static String separator -> \
          与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。 
static char separatorChar -> \
          与系统有关的默认名称分隔符。 
```

#### File类的创建功能

```java
//创建文件
* boolean createNewFile() : 创建File对象指向的文件 //只会创建文件
    文件存在,会创建失败;[不会覆盖源文件]
//创建文件夹
boolean mkdir() : 创建File对象指向的单级文件夹 //只会创建文件夹 
* boolean mkdirs() : 创建File对象指向的单级文件夹或者多级文件夹 //只会创建文件夹  

//返回值 : 是否创建成功     
```

#### 相对路径和绝对路径[重点]

```java
相对路径和绝对路径 : 对文件/文件夹的路径的一种表达形式!

绝对路径 : 文件/文件夹的完整路径 [1. 绝对能找到文件所在的位置 2. 以盘符开头]    

相对路径 : 相对于参照物的路径 [1. 比绝对路径表达要简单 2. 不以盘符开头 3. 不能表示计算机中所有的文件/文件夹] 
    参照物 : 当前项目的根目录[IO体系]


寄快递 :
    绝对路径 -> 地球:亚洲\\中国\\湖北\\武汉\\江夏区\\茅店山中路\\东湖网谷\\6栋\\4楼\\尚硅谷\\403\\谈斌
    相对路径 //参照物 : 你已经在东湖网谷了[举例]
        -> 6栋\\4楼\\尚硅谷\\403\\谈斌

注意 : 如果你的模块结构比较混乱,那么也有可能用不了相对路径!        
```

#### File类的删除功能[了解]

```java
boolean delete() : 删除File对象指向的文件/空文件夹 [不走回收站]
```

#### File类的判断功能

```java
//重要的
 boolean exists() : 判断文件是否存在
 boolean isDirectory() : 判断file对象指向的是否是一个文件夹[是文件夹就返回true] 
 boolean isFile() : 判断file对象指向的是否是一个文件[是文件就返回true]   

//理解     
 boolean isAbsolute() : 判断创建File对象时是否用的是绝对路径
 boolean isHidden() : 判断创建File对象指向的文件/文件夹是否是隐藏文件/文件夹    
```

#### File类的获取功能

```java
//重要
String getName() : 获取File对象指向的文件/文件夹的名称 [文件的名称 : 文件名.后缀名]

//理解    
//获取File对象指向的文件/文件夹的绝对路径
File getAbsoluteFile()  -> 返回的是 File对象
String getAbsolutePath() -> 返回的是 String对象 

//获取File对象指向的文件/文件夹的父路径  [不管你如何分隔子父路径,这里的父路径都是路径结尾的文件/文件夹的前置所有路径]  
String getParent() -> 返回的是 String对象   
File getParentFile() -> 返回的是 File对象  

String getPath() : 获取创建File对象时传入的路径 

long length() : 获取File对象指向的文件的大小 [大小 : 所占用的字节数] 
    //获取不了文件夹的大小 [文件夹无大小]
```

```java
//示例1
    public static void main(String[] args) throws IOException {
        //绝对路径
        File file1 = new File("E:\\img\\av\\fdse456ygfr.6a7kw12qdww0.jpg");
        //判断是目录还是文件
        boolean isdirectory = file1.isDirectory();
        boolean isfile = file1.isFile();
        System.out.println("directory = " + isdirectory);
        System.out.println("isfile = " + isfile);
        //文件字节大小
        long fileSize = file1.length();
        System.out.println("fileSize = " + fileSize);
        //获取文件父级目录aA
        String file1Parent = file1.getParent();
        File parentFile1 = file1.getParentFile();
        System.out.println("file1Parent = " + file1Parent);
        System.out.println("parentFile1 = " + parentFile1);
        //判断路径是否存在
        File file2 = new File("E:\\imgs\\1.png");
        System.out.println("file2.exists() = " + file2.exists());
        //相对路径
        File file3 = new File("data.txt");
        System.out.println("file3.length() = " + file3.length());
        //获取文件定义时使用的路径
        System.out.println("file3.getPath() = " + file3.getPath());
        //获取文件的绝对路径
        System.out.println("file3.getAbsoluteFile() = " + file3.getAbsoluteFile());
        //获取文件名称(带后缀)
        System.out.println("file1.getName() = " + file1.getName());
        //获取文件最后修改的时间
        System.out.println("file1.lastModified() = " + new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(file1.lastModified()));
    }
//示例2
        public static void main(String[] args) throws IOException {
        //创建文件
        File f1 = new File("data1.txt");
        boolean exists1=f1.exists();
        System.out.println("f1.exists() = " + exists1);
        boolean newFile1=f1.createNewFile();
        System.out.println("f1.createNewFile() = " + newFile1);

        //创建一级文件夹
        File f2 = new File("E:\\aaa");
        System.out.println("f2.exists() = " + f2.exists());
        boolean mkdir1=f2.mkdir();
        System.out.println("mkdir1 = " + mkdir1);

        //创建多级文件夹
        File f3 = new File("E:\\bbb\\ccc");
        boolean mkdir2=f3.mkdirs();
        System.out.println("mkdir2 = " + mkdir2);

        //删除文件夹
        boolean d1 = f2.delete();
        boolean d2 = f3.delete();
        System.out.println("d1 = " + d1);
        System.out.println("d2 = " + d2);

        //多级文件夹下的文件创建
        File f4 = new File("E:\\ddd\\eee");
        File f5 = new File(f4, "bbb.txt");
        f4.mkdirs();
        f5.createNewFile();

        File f6 = new File("test");
        File f7 = new File(f6,"test.txt");
        f6.mkdirs();
        f7.createNewFile();

        File f8 = new File("day_17\\test");
        File f9 = new File(f8,"test.txt");
        f8.mkdirs();
        f9.createNewFile();
    }
```

#### File类的遍历功能

```java
File类的遍历功能 :
    File[] listFiles() : 获取File对象指向的文件夹下的文件/文件夹 
        注意 :
            1. 只能获取当根目录下的文件和文件夹 [子文件夹中的文件和文件夹获取不到的]
            2. 如果File对象指向的是一个文件,返回 null [会出现空指针异常]
            3. 如果File对象指向的是一个空文件夹,返回一个长度为0的File数组  

File类的带过滤条件的遍历功能 :            
    File[] listFiles(FileFilter filter)  
        FileFilter : 文件过滤器接口 
            抽象方法 : boolean accept(File file)
                //重写accept方法就是在编写文件过滤的条件[为true就通过,为false就不通过]
```

```java
//示例1
    public static void main(String[] args) {
        String path="E:\\img";
        File file = new File(path);
        getFileList(file);
    }
    public static void getFileList(File root){
        if(root.isFile()){
            System.out.println("  文件有:"+root.getName());
            return;
        }
        System.out.println("文件夹有:"+root.getName());
        File[] files = root.listFiles();
        for (File file : files) {
            getFileList(file);
        }
    }
//示例2
    public static void main(String[] args) {
        String path="E:\\aaa";
        File file = new File(path);
        File[] files = file.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {                
                return pathname.getName().endsWith(".txt")|| pathname.isDirectory();
            }
        });
        for (File file1 : files) {
            System.out.println("file1 = " + file1);
        }
    }
```

## IO流的概述[重点]

![by45yujherset6hrftghrsawt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/by45yujherset6hrftghrsawt.2jpt5n3j3860.webp)

![hufuirf8r35n34534dfg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hufuirf8r35n34534dfg.32ah8qdyg340.webp)

```java
IO流 : IO流是操作计算机硬盘上文件内容的技术手段 [实现了硬盘数据和内存[代码]数据进行交互的技术手段]
    //集合 : 临时存储的容器

I : input -> 输入 [读]
O : output -> 输出 [xi]
流 : 水流 [IO流技术让代码和硬盘中的文件建立连接(管道),数据以字节/字符的形式在管道内传输] 
相对路径起点 : 项目根目录

何时使用输入流,何时使用输出流 : [参照物 : 当前Java代码]
    读取硬盘文件内的数据 : 输入流
    往硬盘文件中写数据 : 输出流  

IO流的分类 : 
    1. 按照流向来分 :
        输入流 -> Input / Read
        输出流 -> Output / Write

    2. 按照操作的文件类型分 : 
            /*
            硬盘中文件类型分为两大类 :
                字节文件 -> 计算机硬盘里所有的文件都是字节文件 [计算机中一切皆字节]
                字符文件 -> 文件内全是字符的文件是字符文件 [.txt,.java,.html,.js...]  
                    //office全家桶 : 都不是字符文件而是字节文件! [.word,.ppt...]
                    //鉴别字符文件的办法 : 使用windows自带的记事本工具能打开并阅读的文件是字符文件
            */
        字节流 -> 可以操作所有文件的流 [操作字符文件不方便] -> 流的名称以Stream结尾
        字符流 -> 只能操作计算机中字符文件[不可以操作其他的字节文件] -> 流的名称以Reader/Writer结尾

认识流 :
    OutputStream : 字节输出流 
    FileReader : 字符输入流 
    InputStreamReader : 字符输入流 [转换流: 可以把字节输入流转换成字符输入流]   
    PrintStream : 字节输出流 [Print : 打印(输出)]    
```

#### 字节流

```java
OutputStream / InputStream -> 根节点
*FileOutputStream / FileInputStream -> 常用的字节流 [文件字节流]    
BufferedOutputStream / BufferedInputStream -> 高效字节流 [缓冲字节流]  
```

#### OutputStream / InputStream

```java
OutputStream / InputStream : 字节输出流 / 字节输入流  --> 字节流的根节点 [抽象类]
```

#### FileOutputStream / FileInputStream

```java
FileOutputStream / FileInputStream : 文件字节输出流 / 文件字节输入流         
```

##### FileOutputStream 文件字节输出流

```java
FileOutputStream 文件字节输出流 
    构造方法 : 
        //形参的含义 : 传入目标文件的路径 [目标文件 : 要写入数据的文件]
        * FileOutputStream(String name)
        FileOutputStream(File file)   

    创建FileOutputStream对象做了哪些事情 :
        1. 创建了FileOutputStream对象
        2. 在堆内存中开辟了内存空间
        3. 创建流对象并指向路径结尾的文件 [不能指向一个文件夹]
        4. 如果文件不存在 : 会帮忙创建新文件 [文件的前置路径要正确]
        5. 如果文件存在 : 会帮忙创建新文件去覆盖老文件 [文件的前置路径要正确] 

    写数据的功能 : write
       * void write(int b) : 写单个字节
        void write(byte[] b) : 写多个字节 [写整个字节数组]
       ** void write(byte[] b, int off, int len) : 写多个字节 [写字节数组的一部分]
            int off : 其实索引 [从哪个索引开始写]
            int len : 写几个字节    

    注意事项 :
        1. 如何换行 ["\r\n"换行符]
        2. 如何写字符串 [String -> byte : byte[] getBytes() ]
        3. 写汉字字符串的一部分要注意一个汉字占用几个字节[查看平台的编码格式] :
            GBK : 一个汉字占用2个字节
            UTF-8 : 一个汉字占用3个字节
        4. 如何实现追加写的功能
            //第一个参数 : 目标文件的地址
            //第二个参数 : 追加写开关 [默认是关闭的]
            FileOutputStream(String name, boolean append)
            FileOutputStream(File file, boolean append)
```

##### FileInputStream 文件字节输入流

```java
FileInputStream 文件字节输入流 [没有追加读]
    构造方法 :
        //形参的含义 : 传入源文件的路径 [源文件 : 需要读数据的文件]
        * FileInputStream(String name)
        FileInputStream(File file)     

     FileInputStream创建对象做了哪些事情 :
        1. 创建了FileInputStream对象
        2. 在堆内存中开辟了内存空间
        3. 创建流对象并指向路径结尾的文件 [不能指向一个文件夹]
        4. 源文件不存在 : 报错 [源文件必须存在]   

     读数据的功能 : read    
         *int read() : 一次读一个字节 [把读到的字节返回给调用者]
         //byte[] b : 读数据的容器
         **int read(byte[] b) : 一次读一个字节数组 [批量读] -> 把读到的字节个数返回给调用者,读到的字节数据存到字节数组中
         int read(byte[] b, int off, int len) : 一次读一个字节数组的一部分
             int off : 其实索引 [从哪个索引开始读]
            int len : 读几个     

      注意事项 :
        1. ASCII码表中的字符都是占用1个字节数
        2. 输出时不要加换行,因为字节输入流可以读到文件中的换行符
        3. 字节输入流一次读一个字节读文件,读到文件末尾返回 -1
        4. 字节流操作字符文件不方便,会出现乱码问题    
```

![faefhr6egvsgbgs](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/faefhr6egvsgbgs.2s1522baclu0.webp)

#### BufferedOutputStream / BufferedInputStream

```java
BufferedOutputStream / BufferedInputStream : 高效字节输出流 / 高效字节输入流 
    带Buffered的流都是高效的流,带有自动刷新缓冲区[小推车]功能的流 ;

创建流对象的方法 : //包装流 : 流本身不具备读写能力,真正读写的流对象还是基本流
    BufferedOutputStream(OutputStream out) 
    BufferedInputStream(InputStream in) 

读写方法 :  //相较于基本流没有新增新的读写功能
    写的方法 : write -> BufferedOutputStream
        * void write(int b) : 写单个字节
        void write(byte[] b) : 写多个字节 [写整个字节数组]
       ** void write(byte[] b, int off, int len) : 写多个字节 [写字节数组的一部分]

    读的方法 : read -> BufferedInputStream   
         *int read() : 一次读一个字节 [把读到的字节返回给调用者]
         //byte[] b : 读数据的容器
         **int read(byte[] b) : 一次读一个字节数组 [批量读] -> 把读到的字节个数返回给调用者,读到的字节数据存到字节数组中
         int read(byte[] b, int off, int len) : 一次读一个字节数组的一部分

高效流什么时候使用 : 当源文件足够大的时候使用高效流的效率更高;

高效字节输出流的追加写功能如何实现 :
    BufferedOutputStream bos = new BufferedOutputStream(
                new FileOutputStream("目标文件地址",true));
```

#### 字符流

```java
Writer / Reader -> 根节点
FileWriter / FileReader -> 文件字符流
*BufferedWriter / BufferedReader -> 最常用的字符流 [高效字符流/字符缓冲流]  
```

#### Writer / Reader

```java
Writer / Reader : 字符流的根节点 [抽象类]
    1. 字符流只能操作字符文件
    2. 字符流的本质其实也是字节流 [只是对字节流进行高度封装] -> 字符流 = 字节流 + 编码格式;
    3. 字符流都是缓冲流[带缓冲区的流,但是不带Buffered就不带自动刷新的功能,所以不算高效流]
```

#### FileWriter / FileReader

```java
FileWriter / FileReader : 文件字符输出流 / 文件字符输入流 [普通字符流]
```

##### FileWriter 文件字符输出流

```java
FileWriter 文件字符输出流的构造方法 :
    //直接传入目标文件的地址
    * FileWriter(String fileName) 
    FileWriter(File file) 
    //带有追加写功能的字符输出流对象
    //[追加写功能 : FileOutputStream(文件字节输出流) / FileWriter(文件字符输出流)]
    FileWriter(String fileName, boolean append)
    FileWriter(File file, boolean append)     

    FileWriter 文件字符输出流写数据的方法 :  write
        * 1. 一次写一个字符 -> void write(int c) 
        2. 一次写一个字符数组 -> void write(char[] cbuf)  
        **3. 一次写一个字符数组的一部分 -> void write(char[] cbuf, int off, int len)  
        4. 一次写一个字符串 -> void write(String str) 
        5. 一次写一个字符串的一部分 -> void write(String str, int off, int len) 
```

##### FileReader 文件字符输入流

```java
FileReader 文件字符输入流的构造方法 :
    //直接传入源文件的地址
    * FileReader(String fileName) 
    FileReader(File file) 

    FileReader 文件字符输入流读数据的方法 :  read
        * 1. 一次读一个字符 ->  int read()  
        ** 2. 一次读一个字符数组 -> int read(char[] cbuf) 
        3. 一次读一个字符数组的一部分 -> int read(char[] cbuf, int off, int len)  
        //文件字符输入流没有读字符串的功能!![读字符串功能的类 : BufferedReader]
        //4. 一次读一个字符串
        //5. 一次读一个字符串的一部分
```

#### BufferedWriter / BufferedReader

```java
BufferedWriter / BufferedReader : 高效字符输出/输入流 
    1. Buffered : 高效 -> 带自动刷新的缓冲区 
    2. 回忆BufferedOutputStream / BufferedInputStream的使用技巧[创建对象上] 
        //高效流本身不具备读写能力,需要在创建高效流对象时,传入基本流对象

BufferedWriter / BufferedReader 的构造方法 :
    BufferedWriter(Writer out) 
    BufferedReader(Reader in)  

BufferedWriter / BufferedReader 的常规读写方法 :
    BufferedWriter的写数据方法 :
        * 1. 一次写一个字符 -> void write(int c) 
        2. 一次写一个字符数组 -> void write(char[] cbuf)  
        **3. 一次写一个字符数组的一部分 -> void write(char[] cbuf, int off, int len)  
        ***4. 一次写一个字符串 -> void write(String str) 
        5. 一次写一个字符串的一部分 -> void write(String str, int off, int len) 

   BufferedReader的读数据方法 :
        * 1. 一次读一个字符 ->  int read()  
        ** 2. 一次读一个字符数组 -> int read(char[] cbuf) 
        3. 一次读一个字符数组的一部分 -> int read(char[] cbuf, int off, int len)  
```

#### BufferedWriter / BufferedReader 的特殊功能[重点]

```java
BufferedReader 有特殊的读数据的方法 :
    String readLine() : 读一行数据 [读字符串] //弊端 : 不读每行结尾的换行符 

BufferedWriter 有特殊写换行符的方法 :
    void newLine() : 根据不同系统写换行符 
```

```java
//示例
public static void main(String[] args) throws IOException {
        copyStreamCh("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyStreamByteArray("E:\\io\\test\\1.webp", "E:\\io\\test\\1copy.webp");
        copyBufferedCh("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyBufferedByteArray("E:\\io\\test\\1.webp", "E:\\io\\test\\1copy.webp");
        copyString("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyStringArray("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyBufferedString("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyBufferedStringArray("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
        copyBufferedStringLine("E:\\io\\test.txt", "E:\\io\\testcopy.txt");
    }

    //文件字节流一次一个字节复制文件
    public static void copyStreamCh(String oldFile, String newFile) throws IOException {
        FileInputStream fis = new FileInputStream(oldFile);
        FileOutputStream fos = new FileOutputStream(newFile);
        int readData;
        while ((readData = fis.read()) != -1) {
            fos.write(readData);
        }
        fis.close();
        fos.close();
    }

    //文件字节流一次一个字节数组复制文件
    public static void copyStreamByteArray(String oldFile, String newFile) throws IOException {
        FileInputStream fis = new FileInputStream(oldFile);
        FileOutputStream fos = new FileOutputStream(newFile);
        byte[] bytes = new byte[1024];
        int len;
        while ((len = fis.read(bytes)) != -1) {
            fos.write(bytes, 0, len);
        }
        fis.close();
        fos.close();
    }

    //高效文件字节流一次一个字节复制文件
    public static void copyBufferedCh(String oldFile, String newFile) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(oldFile));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(newFile));
        int readData;
        while ((readData = bis.read()) != -1) {
            bos.write(readData);
        }
        bis.close();
        bos.close();
    }

    //高效文件字节流一次一个字节数组复制文件
    public static void copyBufferedByteArray(String oldFile, String newFile) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(oldFile));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(newFile));
        byte[] bytes = new byte[1024];
        int len;
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, len);
        }
        bis.close();
        bos.close();
    }

    //文件字符流一次一个字符复制文件
    public static void copyString(String oldFile, String newFile) throws IOException {
        FileReader fr = new FileReader(oldFile);
        FileWriter fw = new FileWriter(newFile);
        int readData;
        while ((readData = fr.read()) != -1) {
            fw.write(readData);
            fw.flush();
        }
        fr.close();
        fw.close();
    }

    //文件字符流一次一个字符数组复制文件
    public static void copyStringArray(String oldFile, String newFile) throws IOException {
        FileReader fr = new FileReader(oldFile);
        FileWriter fw = new FileWriter(newFile);
        char[] chars = new char[1024];
        int len;
        while ((len = fr.read(chars)) != -1) {
            fw.write(chars, 0, len);
            fw.flush();
        }
        fr.close();
        fw.close();
    }

    //高效文件字符流一次一个字符复制文件
    public static void copyBufferedString(String oldFile, String newFile) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(oldFile));
        BufferedWriter bw = new BufferedWriter(new FileWriter(newFile));
        int readData;
        while ((readData = br.read()) != -1) {
            bw.write(readData);
        }
        br.close();
        bw.close();
    }

    //高效文件字符流一次一个字符数组复制文件
    public static void copyBufferedStringArray(String oldFile, String newFile) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(oldFile));
        BufferedWriter bw = new BufferedWriter(new FileWriter(newFile));
        char[] chars = new char[1024];
        int len;
        while ((len = br.read(chars)) != -1) {
            bw.write(chars, 0, len);
        }
        br.close();
        bw.close();
    }

    //高效文件字符流一次一行复制字符文件
    public static void copyBufferedStringLine(String oldFile, String newFile) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(oldFile));
        BufferedWriter bw = new BufferedWriter(new FileWriter(newFile));
        String line;
        while ((line = br.readLine()) != null) {
            bw.write(line);
            bw.newLine();
        }
        br.close();
        bw.close();
    }
```

#### "杂"流 [掌握]

```java
InputStreamReader / OutputStreamWriter -> 转换流 
System.in / System.out -> 系统标准输入/输出流
ObjectInputStream / ObjectOutputStream -> 反序列化流/序列化流 [对象操作流]
PrintStream / PrintWriter -> 打印字节流 / 打印字符流 [都是输出流! 打印流只有输出没有输入]    
```

#### InputStreamReader / OutputStreamWriter

```java
InputStreamReader / OutputStreamWriter : 字符流 [转换流]
    目标1 : 掌握InputStreamReader / OutputStreamWriter 把字节流转换成字符流的操作
        //字符流 = 字节流 + 编码格式
    目标2 : 掌握设置文件读和写的编码格式设置操作 [字符文件乱码问题]   

InputStreamReader / OutputStreamWriter 首先是字符流,字符流的读写方式它们都具备
    InputStreamReader :
        一次读一个字符 , 一次读一个字符数组 , 一次读一个字符数组的一部分
    OutputStreamWriter :
        一次写一个字符 , 一次写一个字符数组 , 一次写一个字符数组的一部分 , 一次写一个字符串 , 一次写一个字符串的一部分

// 目标1 : 掌握InputStreamReader / OutputStreamWriter 把字节流转换成字符流的操作
通过转换流的构造方法实现 :         
   InputStreamReader(InputStream in) : 把传入的字节输入流转换成字符输出流;
   OutputStreamWriter(OutputStream out) : 把传入的字节输出流转换成字符输出流; 

//目标2 : 掌握设置文件读和写的编码格式设置操作 [字符文件乱码问题]  // 按照文件的编码格式对文件进行读和写即可 
    //此构造验证 : 字符流 = 字节流 + 编码格式
    InputStreamReader(InputStream in, String charsetName) : 设置读的编码格式;
    OutputStreamWriter(OutputStream out, String charsetName) : 设置写的编码格式;
```

#### 编码和解码[重要]

```java
码 : 二进制码 -> 0101010111000011 -> 一组 bit -> 一组 byte [字节数组]
编 : 编写 [编写二进制码] //把程序员看得懂的转换成看不懂的[计算机看得懂]
    (char[] --> String) --> byte[]
解 : 解析 [解析二进制码] //把程序员看不懂的转换成看的懂的[程序员看得懂]   
    byte[] --> (String --> char[])


要想文件不乱码 : //根本 : 编码和解码的方式一致!
    1. 保证文件(文件存储在硬盘上[编码])和平台(平台要打开此文件[解码])的编码格式一致即可    [推荐]
        //要修改一定修改文件的编码格式和平台一致! 不要去修改平台的编码格式!
    2. 按照文件的编码格式对文件进行读和写即可  [理解] 

解决文件乱码问题最最推荐的做法 : 修改文件的编码格式和平台的编码格式一致! [不要动平台的编码格式和用代码去读和写] 
```

![rg34ryjwed4t5u4r](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rg34ryjwed4t5u4r.8d7ubul42i8.webp)

#### 编码表

```java
编码表 : 是编码和解码操作中最重要的环节 [密码本]

最基础的编码表 : ASCII [美国标准信息交换码表] //所有的编码表都是以ASCII编码表开头   
    -> ASCII 码表中有的内容在任何编码表下都不会乱码

中国的字符集 : GB系列//字符集 : 编码表的统称 
    GB2312 编码表 -> 最早的
    GB18030 编码表
    GBK 编码表 -> 现在最常用的中文编码表

拉丁字符集 : ISO系列
    ISO-8859-1 : 最常用的拉丁编码

//国际字符统一组织推出了 UniCode字符集 : 要求所有在互联网上的内容都必须使用此字符集中的编码表 [UTF-8]   
   UTF-8 [IDEA默认编码格式]
   UTF-16
   UTF-32     

Windows系统中的ANSI : ANSI 并不是一种编码表 -> ANSI : 本地字符集   
    Windows系统的默认字符集 : Windows系统在不同时区,不同系统语言环境下默认字符集不一样的! 
        在中国 : ANSI -> GBK
        在欧洲 : ANSI -> ISO-8859-1
        在美国 : ANSI -> ASCII
        ....    
```

#### System.in / System.out

```java
System.in  : 系统标准输入流 [功能 : 只能做键盘录入(源文件的位置 : 键盘)]
System.out : 系统标准输出流 [功能 : 只能做控制台打印(目标文件的位置 : 控制台)]   

//字节流    
System.in 的数据类型 : InputStream
System.out 的数据类型 : PrintStream    
```

#### ObjectInputStream / ObjectOutputStream

```java
ObjectInputStream / ObjectOutputStream : 对象操作流 [把永久存储的对象读取代码中/把对象做永久存储]
    //序列化 : 把数据永久存储[存储在硬盘上]
    ObjectInputStream : 反序列化流
    ObjectOutputStream : 序列化流

构造方法 : //包装流
    ObjectInputStream(InputStream in) 
    ObjectOutputStream(OutputStream out)

ObjectInputStream / ObjectOutputStream 字节流 : 具备字节流的常规读写功能
    读 : 一次读一个字节,一次读一个字节数组,一次读一个字节数组的一部分
    写 : 一次写一个字节,一次写一个字节数组,一次写一个字节数组的一部分   

ObjectInputStream / ObjectOutputStream 读写对象的方法 
   ObjectInputStream : 读对象的方法
       Object readObject()  
   ObjectOutputStream : 写对象的方法   
       void writeObject(Object obj) 

注意事项 :
    1. 自定义描述类的对象要被序列化操作,必须让此对象的类实现序列化接口 //Serializable
    2. ClassNotFoundException : 如果对象所属的类没了,在读取此对象时因为对象找不到所属类报错
    3. EOFException : End of File Exception [文件末尾异常]   
    4. InvalidClassException : 非法类异常 
         存在对象文件中的对象 和 模块中的类 成员变量/成员方法 不匹配 导致的;
```

#### 自定义描述类的标准编写步骤[重点]

```java
自定义描述类的标准编写步骤[重点] : JavaBean / domain
1. 私有所有属性
2. 自动生成无参构造
3. 自动生成全参构造
4. 自动生成getter and setter方法
5. 自动重写equals 和 hashCode方法
6. 自动重写toString方法
7. 实现 Serializable 接口
8. 自动生成序列化ID : serialVersionUID 

//行为 : 如果有需求在编写行为[方法]    
```

##### 设置序列化ID操作

![rfdevg67u236745wsedf](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rfdevg67u236745wsedf.2dsqf0ffw340.webp)

#### Properties 属性集[重点]

```java
Properties 属性集 : 双列集合 [Hashtable的子类集合]
    注意事项 : 
        1. Properties是没有泛型的,它的默认泛型是 <Object,Object> [没有延用父类的泛型]
        2. Properties 属性集和IO配合时要求键和值的数据类型必须都是String才能够实现; [主动的添加键值的时候传入字符串类型的键和值]   

对象创建 : 没有泛型
    Properties 集合名 = new Properties(); //不能加尖括号

增删改查和遍历 : 和Map接口的操作是一样的

方法形参为String类型的一些增删改查方法 : 
    替换put方法 :  Object setProperty(String key, String value) //调用 Hashtable 的方法 put。
    替换get方法 :  String getProperty(String key)  
    替换keySet方法 :  Set<String> stringPropertyNames() //返回此属性列表中的键集  

//和IO配合的方法 : 一键存储
    存数据: //String comments : 注释
        void store(OutputStream out, String comments)  
        void store(Writer writer, String comments)     
    取数据:
        void load(InputStream inStream) 
        void load(Reader reader)      
```

## 综合案例

```java
//StudentInfo.txt
    s1,张三,23,上海
    s2,李四,21,天津
    s3,王五,22,北京
    s4,赵六,20,武汉
//Student
    public class Student implements Serializable {
        private static final long serialVersionUID = -3384127067038005900L;
        private String sid;
        private String name;
        private int age;
        private String address;
        public Student() {
        }
        public Student(String sid, String name, int age, String address) {
            this.sid = sid;
            this.name = name;
            this.age = age;
            this.address = address;
        }
        public String getSid() {
            return sid;
        }

        public void setSid(String sid) {
            this.sid = sid;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public int getAge() {
            return age;
        }
        public void setAge(int age) {
            this.age = age;
        }
        public String getAddress() {
            return address;
        }
        public void setAddress(String address) {
            this.address = address;
        }
        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Student)) return false;
            Student student = (Student) o;
            return getAge() == student.getAge() && Objects.equals(getSid(), student.getSid()) && Objects.equals(getName(), student.getName()) && Objects.equals(getAddress(), student.getAddress());
        }
        @Override
        public int hashCode() {
            return Objects.hash(getSid(), getName(), getAge(), getAddress());
        }
        @Override
        public String toString() {
            return "Student{" +
                    "sid='" + sid + '\'' +
                    ", name='" + name + '\'' +
                    ", age=" + age +
                    ", address='" + address + '\'' +
                    '}';
        }
    }
//Student
    public class StudentManger {
        public static void main(String[] args) throws IOException {
            System.out.println("----------欢迎进入学生管理系统----------");
            System.out.println("1.查询学生信息");
            System.out.println("2.添加学生信息");
            System.out.println("3.删除学生信息");
            System.out.println("4.修改学生信息");
            System.out.println("5.退出学生信息管理系统");
            System.out.println("----------请选择功能:(1-5)----------");
            Scanner sc = new Scanner(System.in);
            while (true){
                System.out.print("选择:\t");
                String choise = sc.nextLine();
                switch (choise){
                    case "1":
                        findStudentInfo();
                        break;
                    case "2":
                        addStudentInfo();
                        break;
                    case "3":
                        deleteStudentInfo();
                        break;
                    case "4":
                        updateStudentInfo();
                        break;
                    case "5":
                        System.out.println("----------退出成功----------");
                        System.exit(0);
                    default:
                        System.out.println("您输入的选择有误,重新输入!");
                        break;
                }
            }
        }
        public static final String FILEPATH="src/com/qh/ios/StudentInfo.txt";
        //读取文件
        private static ArrayList<Student> readFile(String filePath) throws IOException {
            BufferedReader br = new BufferedReader(new FileReader(filePath));
            ArrayList<Student> students = new ArrayList<>();
            String line;
            while ((line=br.readLine())!=null){
                String[] datas = line.split(",");
                Student student = new Student(datas[0], datas[1], Integer.parseInt(datas[2]), datas[3]);
                students.add(student);
            }
            br.close();
            return students;
        }
        //写入文件
        private static void writeFile(ArrayList<Student> list) throws IOException {
            BufferedWriter bw = new BufferedWriter(new FileWriter(FILEPATH));
            for (Student student : list) {
                StringBuilder sb = new StringBuilder();
                sb.append(student.getSid()).append(",")
                        .append(student.getName()).append(",")
                        .append(student.getAge()).append(",")
                        .append(student.getAddress());
                bw.write(sb.toString());
                bw.newLine();
            }
            bw.close();
        }
        //查询学生信息
        private static void findStudentInfo() throws IOException {
            ArrayList<Student> students = readFile(FILEPATH);
            if(students.isEmpty()){
                System.out.println("系统内目前还没有学生信息,请添加后重试!");
                return;
            }
            System.out.println("学生信息如下 :");
            System.out.println("-----------------------------------------------");
            System.out.println("|\t学号\t|\t姓名\t\t|\t年龄\t\t|\t籍贯\t\t|\t");
            for (Student student : students) {
                String sid = student.getSid();
                String name = student.getName();
                int age = student.getAge();
                String address = student.getAddress();
                System.out.println("|\t"+sid + "\t|\t" + name + "\t\t|\t" + age + "\t\t|\t" + address+"\t\t|");
            }
            System.out.println("-----------------------------------------------");
        }
        //判断是否存在sid
        private static boolean doesItExistSID(String sid) throws IOException {
            ArrayList<Student> students = readFile(FILEPATH);
            Iterator<Student> iterator = students.iterator();
            while (iterator.hasNext()){
                Student next = iterator.next();
                if(next.getSid().equals(sid)){
                    return true;
                }
            }
            return false;
        }
        //根据sid添加学生信息
        private static void addStudentInfo() throws IOException {
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要添加的学号:");
            String addAid = sc.nextLine();
            if(doesItExistSID(addAid)){
                System.out.println("您录入的学号 " + addAid + " 已经存在 , 请确认后再来 !");
                return;
            }
            System.out.println("请输入要添加的姓名:");
            String addName = sc.nextLine();
            System.out.println("请输入要添加的年龄:");
            String addAge = sc.nextLine();
            System.out.println("请输入要添加的籍贯:");
            String naddAddress = sc.nextLine();
            Student student = new Student(addAid, addName, Integer.parseInt(addAge), naddAddress);

            ArrayList<Student> students = readFile(FILEPATH);
            students.add(student);
            Collections.sort(students, new Comparator<Student>() {
                @Override
                public int compare(Student o1, Student o2) {
                    return o1.getSid().compareTo(o2.getSid());
                }
            });
            writeFile(students);
            System.out.println("学生信息添加成功!");
        }

        //根据sid删除学生信息
        private static void deleteStudentInfo() throws IOException {
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要删除的学号:");
            String deleteSid = sc.nextLine();
            if(!doesItExistSID(deleteSid)){
                System.out.println("您要删除的学生学号 " + deleteSid + " 不存在 ! 请确认后再来!");
                return;
            }
            ArrayList<Student> students = readFile(FILEPATH);
            for (int i = 0; i < students.size(); i++) {
                String sid = students.get(i).getSid();
                if(sid.equals(deleteSid)){
                    students.remove(i);
                    break;
                }
            }
            writeFile(students);
            System.out.println("学生信息删除成功!");
        }
        //根据sid修改学生信息
        private static void updateStudentInfo() throws IOException {
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要修改的学号:");
            String updateSid = sc.nextLine();
            if(!doesItExistSID(updateSid)){
                System.out.println("您要修改的学生学号 " + updateSid + " 不存在 ! 请确认后再来!");
                return;
            }
            System.out.println("请输入学号为 " + updateSid + " 学生的新姓名 : ");
            String updateName = sc.nextLine();
            System.out.println("请输入学号为 " + updateSid + " 学生的新年龄 : ");
            String updateAge = sc.nextLine();
            System.out.println("请输入学号为 " + updateSid + " 学生的新籍贯 : ");
            String updateAddress = sc.nextLine();

            ArrayList<Student> students = readFile(FILEPATH);
            for (int i = 0; i < students.size(); i++) {
                Student student = students.get(i);
                if (student.getSid().equals(updateSid)){
                    student.setName(updateName);
                    student.setAge(Integer.parseInt(updateAge));
                    student.setAddress(updateAddress);
                    break;
                }
            }
            writeFile(students);
            System.out.println("学生信息修改成功!");
        }

    }
```
