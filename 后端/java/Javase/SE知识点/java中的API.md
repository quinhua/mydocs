## API的使用

```java
API : Application Programming Interface [应用程序编程接口] -> 帮助文档,词典 [对JDK的翻译文档]
```

![ft6543ggfdhy6](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ft6543ggfdhy6.7ddvovpopu80.webp)
![kol9876trgfew3](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/kol9876trgfew3.66j59ldmi580.webp)
![rew34r5tgfrefvgb](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rew34r5tgfrefvgb.3h5q9570cui0.webp)
![fvdr45t6yhgbtfrew3e](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fvdr45t6yhgbtfrew3e.16nd5wdwyjsw.webp)

![dre45tgyhgbfrewgbhg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/dre45tgyhgbfrewgbhg.18v7x2yaz4xs.webp)

## Scanner键盘录入

```java
已知推理未知 :
    键盘录入整数 : int nextInt() -> 已知
    键盘录入字符串 : String nextString() -> 猜

真实情况 :
    String next() : 能录入字符串 但是遇到了空格不再继续录入了;
    String nextLine()  : 能录入所有的字符串 --> 目标方法 [一次录入一行内容]

总结 : Scanner 对象能录入的是基本数据类型和字符串        
```

```java
//实例:计算句子中英文大小写及数字出现的次数
public static void main(String[] args) {  
    Scanner sc = new Scanner(System.in);  
    System.out.println("请输入一句话:");  
    String line = sc.nextLine();  
    int smallcount = 0;  
    int bigcount = 0;  
    int numbercount = 0;  
    int othercount = 0;  
    for (int i = 0; i < line.length(); ++i) {  
        char ch = line.charAt(i);  
        if (ch >= 'a' && ch <= 'z') {  
            smallcount++;  
        } else if (ch >= 'A' && ch <= 'Z') {  
            bigcount++;  
        } else if (ch >= '0' && ch <= '9') {  
            numbercount++;  
        } else {  
            othercount++;  
        }  
    }  
    System.out.println("smallcount = " + smallcount);  
    System.out.println("bigcount = " + bigcount);  
    System.out.println("numbercount = " + numbercount);  
    System.out.println("othercount = " + othercount);  
}
```

## Random类中seed参数

```java
目标 : API中方法的形参名隐藏着很多信息 ;
    index : 索引
    count/length : 个数
    arr : 数组
    ...

Random中带种子参数的构造方法 :
    Random(long seed) : seed -> 种子 
        功能 : 如果使用带种子的构造方法创建random对象,只要种子不变那么每次获取随机数的数值都不变
```

```java
//示例1:输出指定范围内的随机数
public static void main(String[] args) {  
    int max=89;  
    int min=40;  
    System.out.println(rRandom(min,max));  
    System.out.println(mathRandom(min,max));  
}  
public static int rRandom(int min,int max){  
    Random r=new Random();  
    return r.nextInt(max-min)+min;  
}  
public static int mathRandom(int min,int max){  
    return (int)Math.round(Math.random()*(max-min)+min);  
}
```

## String 类的概述[重点]

![45ygherggt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/45ygherggt.grp68m0qflc.webp)

```java
String 类 : 字符串 类  [引用数据类型]

String 类对象的特点 :
    1. 只要是双引号引起来的内容都是 String 类的对象
    2. String 类是Java中唯一一个 不需要new就可以创建的对象类型; ->  String str = "HelloWorld";
    3. String 类是Java中唯一一个 可以做运算的引用数据类型; [运算只能是 加法运算(拼接)]
    4. String 类是Java中唯一一个 既是引用数据类型又是常量类型的数据类型
    5. String 类型的本质是 字符数组 ; "Hello" -> char[] chs = {'H','e','l','l','o'};
    6. String 类型的本本质是 字节数组 ; "abc" - > byte[] bys = {97,98,99};
    7. String 类型的对象一旦被定义 不能改变(长度和内容都不能变) [如果改变了一定是生成了新的字符串对象]
    8. String 类型的对象使用可以被共享 -> 不能变带来的好处
    9. String 类型的对象做 拼接 非常消耗内存资源 [如果是字符串常量拼接因为有常量优化机制所以不耗内存,只有字符串变量做拼接消耗内存资源] -> 不能变带来的坏处
        "Hello" + "World" + "Java" -> 内存中有 : "Hello" , "World" , "Java" ,"HelloWorldJava" -> 不消耗内存
        String str = "Hello";
        str + "World" + "Java" - > "Hello" , "World" , "Java" ,"HelloWorldJava" ,"HelloWorld" -> 消耗内存
```

### String 类的构造方法

```java
String 类对象的创建方式 :
    1. 直接赋值 : String str = "abc"; -> 最推荐的方式

    //废物构造
    2. String() : 创建一个内容为空的字符串对象 -> ""
    3. String(String original) : 根据传入的字符串对象内容来创建一个字符串对象 
        String str = new String("Hello"); -> "Hello"

    //具备转换功能的构造方法 :
    //char[] --> String
    String(char[] value) : 把传入的字符数组中所有的字符内容组成一个字符串对象  
    String(char[] value, int offset, int count) : 把传入的字符数组的一部分内容转换成字符串对象
        offset : 偏移量 [起始索引]
        count : 几个 [转几个]

   //byte[] --> String
   String(byte[] bytes) : 把传入的字节数组中所有的字节内容组成一个字符串对象 
   String(byte[] bytes, int offset, int length) 把传入的字节数组中一部分的字节内容组成一个字符串对象  //offset : 偏移量 [起始索引] length : 几个 [转几个]
```

### String类的比较功能

```java
    == : 比较是否相等
        基本数据类型 : 数值是否相等
        引用数据类型[对象] : 一定是地址值是否相等

 boolean equals(Object anObject)  : 判断调用方法的字符串对象和传入的对象内容是否相同 -> 校验用户名和密码
 boolean equalsIgnoreCase(String anotherString) : 判断调用方法的字符串对象和传入的字符串对象内容是否相同 -> 忽略大小写   -> 校验验证码
```

```java
//示例
public static void main(String[] args) {  
    String s1="helloworld";  
    String s2="helloworld";  
    String s3="HelloWorld";  
    String s4="hello";  
    String s5="world";  
    System.out.println(s1==s2);//true  
    System.out.println(s2==s3);//false  
    System.out.println(s4+s5==s1);//false  
    System.out.println("qianhui"==s1);//false  
    System.out.println(s1.equals(s2));//true  
}
```

### equals方法的源码分析

```java
equals:用于检测一个对象是否等于另外一个对象
源码 :
    1. 大局观 : 学会分层
    2. 夯实的语法基础
    3. 连蒙带猜

equals方法的源码 : 
    public boolean equals(Object anObject) {
        //第一阶段
        //判断(调用方法的对象 和 传入方法的对象 的地址值是否相同) -> 目的 : 提高代码的效率
        //如果2个对象的地址值相同,说明是同一个对象,那么没有必要比较内容! 直接返回true!
        if (this == anObject) { // 自己比自己
            return true;
        }

        //前提 : 传入的对象和调用方法的对象不是同一个对象

        //第二阶段
        //if(传入的数据是否属于字符串类型) -> false : 说明传入的数据不是字符串类型
        //作用 : 1. 提高代码效率 2. 提高代码的安全性 [为向下转型做准备]
        if (anObject instanceof String) {
            //能进来说明 实参一定是String类型
            String anotherString = (String)anObject;//向下转型 -> 实参 : anotherString
            //char[] value : 是字符串对象的底层字符数组
            //调用方法的字符串对象的底层数组的长度
            int n = /*this.*/value.length;//获取字符串对象的字符个数
            //第三阶段 : 
            /*
                调方法的字符串对象的字符个数 == 传入方法的字符串对象的字符个数

                目的: 提高代码的效率
               */
            if (n == anotherString.value.length) {
                //能进此 if 说明 : 2个字符串对象的字符个数是一样的!
                //比较2个字符串对象的每一个索引位置的字符都相同!
                //接下来就是遍历字符串
                char v1[] = /*this.*/value;//获取调用方法的字符串对象底层数组 -> V1
                char v2[] = anotherString.value; //获取传入方法的字符串对象底层数组 -> V2

                //初始化语句
                int i = 0;
                //判断条件语句
                while (n-- != 0) { //--> 等效于 i < n
                    //能进来说明 字符串的字符还没有比较完
                    //循环体语句
                    //反向逻辑比较 : v1数组和v2数组只要有一个i位置的索引元素不相同,equals方法就直接返回false
                    if (v1[i] != v2[i]){
                         return false;
                    }
                    //步进表达式
                    i++;
                }
                //2个字符串对象中每一个索引位置的字符都相同才能从上方的循环能够走下来!
                return true; //equals方法就返回true
            }
        }
        //第四阶段
        //1. 如果传入方法数据的类型不是String类型,那么equals方法的结果直接返回false
        //2. 如果传入方法字符串对象的长度和调用方法字符串对象的长度不相同直接返回false
        return false;//比较内容不相等
    }
```

```java
//示例:用户登录
public static void main(String[] args) {  
    String username = "admin";  
    String password = "123";  
    String code = "Q1h6";  
    Scanner sc = new Scanner(System.in);  
    for (int i = 0; i < 3; i++) {  
        System.out.println("请输入用户名:");  
        String uname = sc.nextLine();  
        System.out.println("请输入密码:");  
        String upwd = sc.nextLine();  
        System.out.println("请输入验证码 [Q1h6]:");  
        String ucode = sc.nextLine();  

        if (username == null || password == null || code == null) {  
            System.out.println("用户名或密码或验证码为空!");  
        } else {  
            if (username.equals(uname) && password.equals(upwd) && code.equalsIgnoreCase(ucode)) {  
                System.out.println("用户-" + username + "-登录成功!");  
                break;  
            } else {  
                if (i == 2) {  
                    System.out.println("3次机会用完，用户账号被锁定!请联系管理员!");  
                    break;  
                }  
                System.out.println("用户还有" + (2 - i) + "次机会尝试机会!");  
            }  
        }  
    }  
}
```

### String类的获取功能

```java
//字符串对象的遍历
*char charAt(int index) : 获取字符串对象指定位置上的字符
*int length() : 获取字符串对象的长度 [字符个数]

//获取传入的字符/字符串第一次出现在调用方法字符串对象的索引,如果没有出现返回 -1
 int indexOf(int ch) 
 int indexOf(String str) 
 //fromIndex 从哪个索引开始算第一次出现
 int indexOf(int ch, int fromIndex) 
 int indexOf(String str, int fromIndex)   

//获取传入的字符/字符串最后一次出现在调用方法字符串对象的索引,如果没有出现返回 -1  
 int lastIndexOf(int ch) 
 int lastIndexOf(String str) 
 int lastIndexOf(int ch, int fromIndex) 
 int lastIndexOf(String str, int fromIndex)    
```

```java
//示例
public static void main(String[] args) {  
    String str="HelloWorld!";  
    //判断字符第一出现的位置下标  
    System.out.println(str.indexOf("e"));  
    System.out.println(str.indexOf(101));  
    System.out.println(str.indexOf("l"));  
    System.out.println(str.indexOf("y"));  
    System.out.println("----------");  
    //判断字符串第一次出现的位置下标  
    System.out.println(str.indexOf("el"));  
    System.out.println(str.indexOf("le"));  
    System.out.println("----------");  
    //判断字符串从指定下标开始出现的位置下标  
    System.out.println(str.indexOf("l",0));  
    System.out.println("----------");  
    //判断字符串最后一次出现的位置下标  
    System.out.println(str.lastIndexOf("l"));
}
```

### String类的判断功能

```java
判断功能 : 判断方法的功能返回值都是boolean

boolean contains(CharSequence s) : 判断传入的字符串是否包含于调用方法的字符串内部

//判断开头和结尾的方法
boolean startsWith(String prefix) : 判断调用方法的字符串对象是否以prefix字符串开头 -> 判断姓氏
boolean endsWith(String suffix) : 判断调用方法的字符串对象是否以suffix字符串结尾 -> 判断文件名后缀
```

```java
//示例
public static void main(String[] args) {  
    String str="HelloWorld!";   
    //判断字符串中是否包含指定字符串  
    System.out.println(str.contains("oWo"));  
    System.out.println(str.contains("ooo"));  
    System.out.println("----------");  
    //判断字符串是否是以指定字符串开头  
    System.out.println(str.startsWith("Hello"));  
    System.out.println(str.startsWith("hello"));  
    System.out.println("----------");  
    //判断字符串是否是以指定字符串结尾  
    System.out.println(str.endsWith("World!"));  
    System.out.println(str.endsWith("ddd"));  
    System.out.println("----------");   
}
```

### String类的转换功能

```java
    //char[] --> String
    String(char[] value) : 把传入的字符数组中所有的字符内容组成一个字符串对象  
    String(char[] value, int offset, int count) : 把传入的字符数组的一部分内容转换成字符串对象
        offset : 偏移量 [起始索引]
        count : 几个 [转几个]

    //String --> char[]
     char[] toCharArray() : 把字符串对象转换成字符数组        

   //byte[] --> String
   String(byte[] bytes) : 把传入的字节数组中所有的字节内容组成一个字符串对象 
   String(byte[] bytes, int offset, int length) 把传入的字节数组中一部分的字节内容组成一个字符串对象  //offset : 偏移量 [起始索引] length : 几个 [转几个]

   //String -> byte[]
    byte[] getBytes() : 把字符串对象转换成字节数组 [按照平台默认编码格式]
    byte[] getBytes(String charsetName)  : 把字符串对象转换成字节数组 [按照指定的编码格式] 

    UTF-8 : 一个汉字占用3个字节
    GBK : 一个汉字占用2个字节
      -> 汉字字节都是以负数开头的      

----------------------------------------------------------------
 String toUpperCase() : 把字符串对象中所有英文小写字符转换成英文大写字符
 String toLowerCase() : 把字符串对象中所有英文大写字符转换成英文小写字符      a`
```

```java
//示例
public static void main(String[] args) {  
    //把字符串数组转换为字节数组  
    char[] chs=str.toCharArray();  
    System.out.println(Arrays.toString(chs));  
    System.out.println("----------");  
    //把字符串数组转换为字符数组  
    byte[] bys=str.getBytes();  
    System.out.println(Arrays.toString(bys));  
    System.out.println("----------");  
    //英文字符转换为小写  
    System.out.println(str.toLowerCase());  
    //英文字符转换为大写  
    System.out.println(str.toUpperCase());  
}
```

### String类的替换功能

```java
String replace(CharSequence target, CharSequence replacement) : 把调用方法的字符串中target字符串替换成replacement,生成新的字符串对象
    target : 目标
    replacement : 替换物
```

```java
//示例
public static void main(String[] args) {  
    //敏感字屏蔽  
    Scanner sc=new Scanner(System.in);  
    System.out.println("录入:");  
    String line=sc.nextLine();  
    //TMD- >* * *  
    String replace=line.replace("TMD","***");  
    System.out.println("replace替换:"+replace);  
}
```

### String类的截取功能

```java
 String substring(int beginIndex) : 截取调用方法字符串对象内容 -> [beginIndex,结尾]
 String substring(int beginIndex, int endIndex) : 截取调用方法字符串对象内容 ->[beginIndex,endIndex)
```

```java
//示例
public static void main(String[] args) {  
    String str="HelloWorld";  
    //i下标- >结尾  
    System.out.println(str.substring(1));  
    //i下标- >j下标 [i,j)  左闭右开
    System.out.println(str.substring(2,4));  
    System.out.println(str.substring(5,str.length()));  
}
```

### String类的切割功能

```java
String[] split(String regex)  : 按照传入的regex字符串内容进行切割,得到切割后的字符串数组
    //按什么切,就传入什么!! 如果不小心触碰了正则规则 就在要切的内容前面加 \\

String regex : [regex : 正则表达式] -> 不要花时间研究[要用baidu]

    正则表达式 : 独立的语法规则 [校验,判断] -> 语法突出简化[不简单]

常用的正则规则 :
    x+ : x出现一次或者多次都按一次算
    . : 通配符 // \\. 
    \ : 转义符 -> \t : tab ,  \r\n : 转行
```

```java
//示例
public static void main(String[] args) {  
    String str1="HelloWorld";  
    String str2="1991-11-22";  
    System.out.println(Arrays.toString(str1.split("o")));  
    System.out.println(Arrays.toString(str2.split("-")));  
}
```

### String类的匹配功能

```java
boolean matches(String regex) : 判断调用方法的字符串对象内容是否符合传入的正则规则

需要查找常用正则规则请百度!![面向百度编程]
```

```java
//示例:强密码校验
public static void main(String[] args) {  
    String pwd="15236117757ffF";  
    boolean flag=pwd.matches("^(?=.*\\d)(?=.*[a-z])(?=.*[A-Z])[a-zA-Z0-9]{8,16}$");  
    System.out.println(flag);  
}
```

### String类的其他功能

```java
String concat(String str) : 连接 [废物方法 : 可以用加法取代]
String toString() : 把调用方法的字符串对象转换成一个字符串对象 [源代码 : return this;]

static String valueOf(任意类型的数据) : 把传入的任意对象转换成字符串对象 [底层逻辑 : 转换的逻辑是依仗toString()方法的逻辑]
     1. 基本数据类型的实参 : valueOf的逻辑 -> 基本数据类型值 + ""
    2. 引用数据类型的实参 : valueOf的逻辑 -> 引用数据类型数据.toString()的结果
        String.valueOf(学生对象) --> 学生对象.toString() [调用的是学生类的toString()]

String trim() : 去除字符串对象两端的空格      //拿到用户给的数据时,会把字符串数据trim一下
```

## StringBuilder/StringBuffer

![y45gyte5rgyt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/y45gyte5rgyt.5h44bwvvwy00.webp)

```java
StringBuilder/StringBuffer : 关于字符串对象操作的类 [StringBuilder/StringBuffer 和 String 没有类结构上的关系,千万不要直接赋值 : StringBuilder sb = "Hello"; (错误)]  
​  
StringBuilder/StringBuffer : 便捷操作字符串的"工具类" -> 字符串缓冲区类["可变字符串"]  
​  
StringBuilder 和 StringBuffer 区别 :  
    //相同点 : StringBuilder/StringBuffer具备相同的API  
    StringBuilder : 效率高,但是线程不安全  
    StringBuffer : 效率低,但是线程安全  

教学以StringBuilder为例 :  

构造方法 :  
    StringBuilder() : 创建一个内容为空的字符串缓冲区对象,缓冲区内的长度为0 容量是 16  
    //String --> StringBuilder  
    StringBuilder(String str) : 创建一个内容为str的字符串缓冲区对象,缓冲区内的长度为str.length(), 容量是 16 + str.length()  
          
成员方法 :  
    StringBuilder append(任意类型的数据) : 往字符串缓冲区对象内部添加任意内容,返回自己;   
    StringBuilder reverse() : 把字符串缓冲区中的字符 一键翻转并返回自己  
    StringBuilder insert(int offset,任意类型的数据)  :往字符串缓冲区的指定索引位置插入任意类型的数据并返回自己  
    //StringBuilder --> String  
    String toString() : 把字符串缓冲区对象转换成字符串对象 
```

```java
int capacity()  : 获取字符串缓冲区内的容量  
char charAt(int index) : 获取字符串缓冲区指定索引位置的字符  
StringBuilder delete(int start, int end) : 删除sb对象start索引到end索引中的字符,并返回自己  
-> 区间 [start,end) --> 猜的  
StringBuilder deleteCharAt(int index)  : 删除sb对象中指定索引位置上的字符,并返回自己  
void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)  : 把sb对象中从srcBegin到srcEnd索引位置的字符复制到dst数组中(dstBegin 目标数组从哪个索引开始存储复制来的字符)  
```

```java
  -> 区间 [srcBegin,srcEnd)  
    int indexOf(String str) : 获取字符串缓冲区中str第一次出现的索引位置,不存在返回-1  
    int indexOf(String str, int fromIndex) : 从fromIndex索引开始算str第一次出现     
    int lastIndexOf(String str)    
    int lastIndexOf(String str, int fromIndex)    
    int length()  : 长度 [字符个数]         
    StringBuilder replace(int start, int end, String str)  :替换字符串缓冲区类中start到end的字符内容,用str替换; 并返回自己      
    //截取字符串缓冲区中的字符内容,返回的是String  
    String substring(int start)   
    String substring(int start, int end)  
```

```java
//示例1:
public static void main(String[] args) {  
    //创建sb对象  
    StringBuilder sb1 = new StringBuilder();  
    System.out.println("sb1 = " + sb1);// 字符串缓冲区中的内容 : ""    System.out.println("sb1.length() = " + sb1.length());//长度 = 0    System.out.println("sb1.capacity() = " + sb1.capacity());//容量 = 16    StringBuilder sb2 = new StringBuilder("Hello");  
    System.out.println("sb2 = " + sb2);// 字符串缓冲区中的内容 : "Hello"    System.out.println("sb2.length() = " + sb2.length());//长度 = 5    System.out.println("sb2.capacity() = " + sb2.capacity());//容量 = 16 + 5    sb2.append("World");  
    System.out.println("sb2 = " + sb2);  
    System.out.println("sb2.capacity() = " + sb2.capacity());//容量 = 16 + 5}
//示例2:
public static void main(String[] args) {  
    //创建sb对象  
    StringBuilder sb = new StringBuilder();  
    System.out.println("sb = " + sb);//""  
    //链式编程 : 对象调用完方法后还是一个对象,那么就可以继续调用此对象的功能  
    sb.append(100).append("Hello").append(13.14).append(false);  
    System.out.println("sb = " + sb);//100Hello13.14false  
    //翻转  
    sb.reverse();  
    System.out.println("sb = " + sb);  
}
```

## Object

```java
Object : Java所有类[包括数组]直接或者间接的父类 [根类,超类,基类]   
​  
构造方法 :  
    Object() : 为什么构造方法中默认赠送加载父类的方法是 super()  
          
成员方法 :  
    int hashCode()  : 获取对象的哈希值 [根据对象在内存中的地址值进行计算(哈希算法)得到的哈希值]  
        //现阶段我们就直接理解 : 如果没有重写Object类中的hashCode方法,就认为hashCode方法获取对象的十进制地址值  
        哈希值就是对象的地址值 : 错!  

hashCode方法可以重写,重写后的hashCode方法可以修改方法的逻辑  
重写 hashCode方法也不是获取对象的地址值   
    Class<?> getClass()  : 获取对象的字节码对象 [Class : 字节码的类型(类的类)]        
        //一个类的字节码对象只会有一个,我们可以通过不同方式获取到的字节码对象进行地址值比较来校验对象的类型  
        对象.getClass() -> 此对象所属类的字节码对象  
        类名.class -> 获取此类的字节码对象  
            (对象.getClass() == 类名.class) -等效于-> (对象名 instanceof 类名)
```

### Object类中的toString方法

```java
String toString() : 把对象转换成String类型  
​  
Object 类中toString方法的源码 :   
    public String toString() {  
        return getClass().getName() + "@" + Integer.toHexString(hashCode());  
    }  
    /*this.*/getClass().getName() : 调用方法的对象获取自己的字节码对象的名称 [包名.类名]  
    @ : 分隔符  
    Integer.toHexString(this.hashCode()) :  
        1. Integer.toHexString(获取调用方法的对象的十进制地址值)  

           2. Integer.toHexString(this.hashCode()) :获取对象的16进制地址值 
    return 对象所属类的包名.类名 @ 对象的16进制地址值;      
​  
总结 :  
1. 直接输出对象就等效于输出对象.toString()的结果  
2. 如果输出对象没有看到对象的包名+地址值 说明此对象的类重写了Object类中的toString  
3. 我们一般会在事物描述类中主动的重写对象的toString方法[重写的逻辑是 : 返回对象的属性值而不是地址值]
```

### Object类中的equals方法

```java
Object类中的equals方法源码  
    public boolean equals(Object obj) {  
        return (this == obj);  
    }  
​  
Object 源码的equals方法的逻辑是比较2个对象的地址值是否相同;  
​  
但是我希望2个对象进行equals比较时,比较的是对象的属性值是否都相同,所以我需要重写Object类中的equals方法!
```

### Objects

```java
Objects 工具类 : 诞生的版本 -> JDK7版本   
    -> 关于对象操作的工具类 [类中最喜欢做非空校验!]  

静态方法 :  
    static boolean isNull(Object obj)  : 判断obj是否为null [是null : true ,不是null : false]  
    static boolean nonNull(Object obj)  : 判断obj是否不为null [不是null : true ,是null : false]  

    static boolean equals(Object a, Object b)  : 对a进行非空校验再调用a的equals方法  
        源码 : return (a == b) || (a != null && a.equals(b));  
    static int hashCode(Object o)  : 对o进行非空校验再调用o的hashCode方法  
        源码 : return o != null ? o.hashCode() : 0;  
​  
    static String toString(Object o)  : 对o进行非空校验,再调用o的toString方法  
        源码: return String.valueOf(o); -> 把任意对象转换成String类型  
            String类中valueOf方法的源码 :  (obj == null) ? "null" : obj.toString();  
    static String toString(Object o, String nullDefault)        
        源码: (o == null) ? nullDefault : obj.toString();  
      return (o != null) ? o.toString() : nullDefault;
```

## 事物描述类的标准编写步骤[重点]

```java
1. 私有所有属性  
2. 自动生成无参构造方法  
3. 自动生成全参构造方法  
4. 自动生成getter and setter方法  
5. 自动重写equals 和 hashCode 方法  
6. 自动重写toString()方法
```

## Arrays

```java
Arrays : 关于数组操作的工具类   
​  
静态方法 :  
    //已知  
    漂亮打印数组 : static String toString(任意类型的数组);  
    利用"快速排序"逻辑对数组元素进行排序 :  static void sort(基本数据类型数组) //默认升序  
          
    //现在学的 : 晚自习要自己检验!  
    static int binarySearch(int[] a, int key)  : 查找key元素在a数组中的索引位置      
        //原理 : 二分查找法 [前提 : 序列必须有序]  
​  
    static int[] copyOf(int[] original, int newLength)  : 数组复制  
        //把原数组中所有的元素复制到一个新的长度为newLength的新数组中,并把新数组的地址值返回  
        int[] original : 原数组  
        int newLength : 新数组的长度  
        返回值类型 : int[] -> 最后得到的新数组
        static int[] copyOfRange(int[] original, int from, int to)  : 数组复制 [带范围的]  
        把原数组中from索引到to索引的元素复制到新数组中,并把新数组的地址值进行返回 [from,to)  
                                                    
        static boolean equals(int[] a, int[] a2)  : 比较2个数组的内容是否相同                                            static void fill(int[] a, int val)  : 把val元素填充到a数组中[填满]
```

## System

```java
System : 关于系统操作的工具类  
​  
static PrintStream out  -> System.out : 标准的系统输出流对象 [打印数据到控制台]  
static InputStream in  -> System.in : 标准的系统输入流对象 [录入数据到程序中]  
static PrintStream err -> System.err : 标准的系统错误/异常输出流对象[打印数据到控制台]  
    //1. 红色字体 2. 开启了新的线程去打印数据[交替打印]  

静态成员方法 :  
    static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) : 数组的复制  
        Object src : 原数组  
        int srcPos : 原数组的复制起始索引  
        Object dest : 目标数组  
        int destPos : 目标数组的起始接收索引  
        int length : 复制几个元素  
          
    static void exit(int status) : jvm退出      
        int status : 状态码 [非0即为异常退出]  
        /*  
            continue : 结束本轮次的循环  
            break : 结束整个循环/switch语句  
            return : 结束方法并把结果返回给调用者  
            System.exit(状态码) : JVM退出  
        */  
          
     static void gc() : 垃圾回收机制[垃圾回收器]     
       
     static long currentTimeMillis() : 获取当前时间毫秒值   
        当前时间毫秒值 = 当前时间 - 时间原点 -> 转换成毫秒单位  
          
     代码世界中的时间原点 :   
        0 毫秒的时间 : 1970年01月01日 00:00:00 [零时区]  
        0 毫秒的时间 : 1970年01月01日 08:00:00 [+8时区] 东八区
```

## 时间体系

![ghyngjt5453ertg5345](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ghyngjt5453ertg5345.492gey3h6rc0.webp)

```java
代码中的时间的特殊情况 :  
    1. 代码世界中的时间原点 :   
        0 毫秒的时间 : 1970年01月01日 00:00:00 [零时区]  
        0 毫秒的时间 : 1970年01月01日 08:00:00 [+8时区] 东八  
    2. 时间单位的换算   
            1 秒 = 1000 毫秒  
            1 毫秒 = 1000 微秒  
            1 微秒 = 1000 纳秒  
    3. 时间的单词   
    4. 时间的文化差异 :   
        a . 国外没有农历[阴历]  
        //文化差异问题在JDK8版本解决了  
        b . 每个星期的第一天 : 中国 -> 周一  国外[基督教] -> 周日  
        c . 月份的索引 从 0 开始  
    5. 判断闰年 : 每四年一闰,每100年不闰,每400年又闰一次  
    6. 时间的类型 :  
        long : 毫秒值 -> 最好计算的时间单位  
        Date : 特定的时间瞬间 -> 中间键  
        String : 字符串时间 -> 最容易表示的时间单位  
            "1998-08-08 00:00:00"  
        Calendar : 日历类型时间 -> 单独设置和获取某个日历字段信息    
```

> JDK7版本的时间操作

### Date

```java
Date : 类 Date 表示特定的瞬间，精确到毫秒。  [时间戳]  

构造方法 :  
    Date() : 创建Date对象,让Date对象指向创建Date对象的瞬间;  
    Date(long date) : 创建Date对象,先让时间归零,再加上传入的时间毫秒值定位的瞬间  
        long date : 毫秒值  

成员方法 :  
    long getTime() : 获取Date对象指向的时间瞬间距离时间原点过去了多少毫秒  
    void setTime(long time) : 修改Date对象指定的时间瞬间 [先让时间归零,再加上传入的时间毫秒值定位的瞬间]  

时间类型之间的转换 :        
    long --> Date  
        1. Date(long date)  
        2. void setTime(long time)  
    Date --> long  
        long getTime()
```

### SimpleDateFormat

```java
String 类型的时间 和 Date 类型时间的 转换 :  
    String 类型的时间 -> Date 类型时间 : 解析字符串时间  
        Date parse(String time)  
    Date 类型时间 -> String 类型的时间 : 格式化Date时间  
        String format(Date date)  

SimpleDateFormat : 关于时间格式化和解析的类      
    构造方法 :  
        //String pattern : 指定的解析和格式化时间的时间格式代码  
        SimpleDateFormat(String pattern)   
            例如时间格式代码 : yyyy年MM月dd日 HH时mm分ss秒  

格式化Date时间 :   
    1. Date 类中自带的一些格式化时间格式的方法 :  
        String toLocaleString()  : 以本地时区的习惯去格式化Date时间对象  
        String toGMTString() :以格林威治时间习惯去格式化Date时间对象 [时间转换为临时区时间]   

2.  SimpleDateFormat 类中格式化Date时间的方法 :  
        String format(Date date) : 把传入的Date时间格式化成字符串时间 [时间格式化格式 : 由创建SimpleDateFormat对象时指定]  
     解析String时间 :    
     SimpleDateFormat 类中解析String时间的方法 :  
        Date parse(String time)  
            注意事项 : 解析时间时 时间格式代码一定要和字符串时间的格式一致!  
            注意事项 : 调用parse方法时有编译时异常需要抛出 [alt + 回车 选择第一个!]    
```

```java
//示例:
public static void main(String[] args) throws ParseException {  
    long now = System.currentTimeMillis();  
    SimpleDateFormat format = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒 a E");  
    String formatdate = format.format(now);  
    System.out.println("formatted = " + formatdate);  

    //解析时间    
String time="2022年12月07日 09时49分13秒";  
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");  
    Date parseTime = simpleDateFormat.parse(time);  
    System.out.println("parseTime = " + parseTime);  
}
```

![7675re564tgv345te](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/7675re564tgv345te.2sltg0k0jrk0.webp)

### Calendar

```java
 时间文化差异都体现在日历类中 !  

如何创建日历类的对象 :  
    //多态   
      Calendar rightNow = Calendar.getInstance();  
日历类对象的功能 :  
    //获取日历上的时间信息  
    int get(int field)   
        返回值 int : 拿到的时间信息  
        形参 int field : 需要获取的是哪个时间信息  
        时间信息 : Calendar.时间信息的单词  
    //设置日历上的时候信息  
     void set(int field, int value)   
         形参 int field : 需要设置的是哪个时间信息      
         形参 int value : 需要设置的值是多少  

     void set(int year, int month, int date, int hourOfDay, int minute, int second)      
     日历类型时间和Date时间之间的转换               
        Calendar --> Date   
            Date getTime()  
        Date --> Calendar  
            void setTime(Date date)   

    日历类型时间和long时间之间的转换     
        Calendar --> long   
            long getTimeInMillis()    
        long --> Calendar  
            void setTimeInMillis(long millis) 
```

```java
//示例1:出生天数
public static void main(String[] args) throws ParseException {  
    Scanner scanner = new Scanner(System.in);  
    System.out.println("请输入您的出生年月日:[示例:1990-10-03]");  
    String TimeOfBirth=scanner.nextLine();  
    System.out.println(getTimToLive(TimeOfBirth));  
}  
public static String getTimToLive(String TimeOfBirth) throws ParseException {  
    long nowTime = System.currentTimeMillis();//当前时间戳  
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");//格式化时间格式  
    Date parseTimeOfBirth = simpleDateFormat.parse(TimeOfBirth + " 00:00:00");//格式化出生时间  
    long timeStamp = parseTimeOfBirth.getTime();//将Date时间转换为long类型  
    long daytimeStamp = 1000 * 60 * 60 * 24;//一天的毫秒数  
    long TimToLive = (nowTime - timeStamp) / daytimeStamp;//计算天数  
    return "您已经在这个美丽的世界生活了 " + TimToLive + " 天了!";  
}
//示例2：出生天数
public static void main(String[] args) {  
    Scanner scanner = new Scanner(System.in);  
    System.out.println("请输入您的出生年份:");  
    int year = scanner.nextInt();  
    System.out.println("请输入您的出生月份:");  
    int month = scanner.nextInt() - 1;  
    System.out.println("请输入您的出生日期:");  
    int day = scanner.nextInt();  
    System.out.println("getTimToLive(year,month,day) = " + getTimToLive(year, month, day));  
}  
public static String getTimToLive(int year,int month,int day){  
    Calendar calendar = Calendar.getInstance();  
    calendar.set(year, month, day, 0, 0, 0);  
    long nowTime = System.currentTimeMillis();  
    long timeStamp = calendar.getTimeInMillis();  
    long daytimeStamp = 1000 * 60 * 60 * 24;  
    long TimToLive = (nowTime - timeStamp) / daytimeStamp;  
    return "您已经在这个美丽的世界生活了 " + TimToLive + " 天了!";  
}
//示例3:计算两个时间之间的间隔
public static void main(String[] args) throws ParseException {  
    String lastTime = "1999-11-12 00:00:00";  
    String nextTime = "2022-12-06 00:00:00";  
    //默认r="day",day/hour/minute/second  
    System.out.println(getTimeInterval(lastTime, nextTime, "day"));  
}  
public static long getTimeInterval(String lastTime, String nextTime, String r) throws ParseException {  
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");  
    long lastStampTime = simpleDateFormat.parse(lastTime).getTime();  
    long nextStampTime = simpleDateFormat.parse(nextTime).getTime();  
    long dayTimeStamp = 1000 * 60 * 60 * 24;  
    long hourTimeStamp = 1000 * 60 * 60;  
    long minuteTimeStamp = 1000 * 60;  
    long secondTimeStamp = 1000;  
    long result = dayTimeStamp;  
    switch (r){  
        case "day":  
            result=dayTimeStamp;  
            break;  
        case "hour":  
            result=hourTimeStamp;  
            break;  
        case "minute":  
            result=minuteTimeStamp;  
            break;  
        case "second":  
            result=secondTimeStamp;  
            break;  
        default:  
            System.out.println("请正确输入:day、hour、minute、second");  
    }  
    return (nextStampTime - lastStampTime) /result ;  
}
```

> JDK8版本新的时间API [解决了文化差异的问题] 

### LocalDateTime/LocalDate/LocalTime

```java
LocalDateTime/LocalDate/LocalTime : 本地年月日时分秒/本地年月日/本地时分秒

1. LocalDateTime是一个不可变的日期时间对象[改变了时间值记得接收结果]
2. 时间表示为纳秒精度
3. 线程安全的

创建对象的方法 :
    static LocalDateTime now() : 创建 LocalDateTime 对象,并让时间指向现在 
    static LocalDateTime now(ZoneId zone)  

    static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second) : 创建 LocalDateTime 对象,并让时间指向指定时间  
    static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second)  
```

```java
//示例:
public static void main(String[] args) {  
    //创建对象  
    LocalDateTime now = LocalDateTime.now();  
    System.out.println("now = " + now);  
    LocalDateTime time1 = LocalDateTime.of(2008, 8, 8, 20, 8, 8);  
    System.out.println("time1 = " + time1);  
    //枚举类名.枚举项  
    LocalDateTime time2 = LocalDateTime.of(2011, Month.NOVEMBER, 11, 11, 11, 11);  
    System.out.println("time2 = " + time2);  
}
```

#### LocalDateTime的until方法[计算时间间隔的方法]

```java
long until(Temporal endExclusive, TemporalUnit unit) 

读方法 :起始时间.直到(结束时间,时间单位)    

Temporal :  LocalDateTime/LocalDate/LocalTime的父接口   
TemporalUnit : 时间单位类[ChronoUnit]的父接口 
```

#### DateTimeFormatter

```java
LocalDateTime 的格式化和解析操作 :
    格式化 : LocalDateTime --> String 
    解析 : String --> LocalDateTime

老版本中日期格式化和解析的类 : SimpleDateFormat
    1. 创建对象时提供解析和格式化日期的 日期格式代码
    2. 提供解析和格式化的方法 :
         格式化的方法 : String format(Date date)
         解析的方法 : Date parse(String time)    

新版本中日期格式化和解析的类 :  DateTimeFormatter  
    1. 创建对象时提供解析和格式化日期的 日期格式代码 [和老版本的格式是一致]

创建DateTimeFormatter对象的方法 :
    static DateTimeFormatter ofPattern(String pattern)  
        String pattern 日期格式代码 

新版本中 解析 和 格式化 时间的方法 在 LocalDateTime 类中 
    格式化 : 
        String format(DateTimeFormatter formatter) 
    解析 :    
        static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter) 
              CharSequence text : 需要解析的字符串时间
```

```java
//示例:
public static void main(String[] args){  
    //日期格式化  
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH时mm分ss秒");  
    LocalDateTime now = LocalDateTime.now();  
    System.out.println("now = " + now);  
    String format = formatter.format(now);  
    System.out.println("format = " + format);  

    //解析时间  
    String time="2022年12月07日 09时49分13秒";  
    LocalDateTime parse = LocalDateTime.parse(time, formatter);  
    System.out.println("parse = " + parse);  
}
```

#### LocalDateTime的get系列方法(获取值)

```java
int getYear()  :获取年信息
int getMonthValue() :获取月份信息(1-12)
int getDayOfMonth(): 获取天信息      
int getHour() :获取小时信息    
int getMinute():获取分钟信息
int getSecond(): 获取秒钟信息    
-------------------------------------------    
DayOfWeek getDayOfWeek():获取星期信息  
int getDayOfYear():获取年中天信息  
Month getMonth(): 获取月份信息(枚举对象返回)   
int getNano():获取纳秒信息     
```

#### LocalDateTime的plus系列方法(加)

```java
plus系列方法是对日期字段进行做加法的操作!
LocalDateTime plusYears(long years) 
LocalDateTime plusMonths(long months)   
LocalDateTime plusDays(long days)  
LocalDateTime plusHours(long hours)  
LocalDateTime plusMinutes(long minutes)  
LocalDateTime plusSeconds(long seconds) 
-------------------------------------------------    
LocalDateTime plusNanos(long nanos)  
LocalDateTime plusWeeks(long weeks)  

注意:
    1. 记得接收, 因为时间修改是生成新的LocalDateTime对象,老的LocalDateTime对象不会改变
    2. 如果你传入的是正数就是做加法,如果你传入的是负数就是做减法    
```

#### LocalDateTime的minus系列方法(减)

```java
minus系列方法是对日期字段进行做减法的操作!
LocalDateTime minusYears(long years) 
LocalDateTime minusMonths(long months)   
LocalDateTime minusDays(long days)  
LocalDateTime minusHours(long hours)  
LocalDateTime minusMinutes(long minutes)  
LocalDateTime minusSeconds(long seconds) 
-------------------------------------------------    
LocalDateTime minusNanos(long nanos)  
LocalDateTime minusWeeks(long weeks) 

注意:
    1. 记得接收, 因为时间修改是生成新的LocalDateTime对象,老的LocalDateTime对象不会改变
    2. 如果你传入的是正数就是做减法,如果你传入的是负数就是做加法       
```

#### LocalDateTime的with系列方法(设置值)

```java
with系列方法是对日期字段进行设置值的操作!
LocalDateTime withYears(long years) 
LocalDateTime withMonths(long months)   
LocalDateTime withDays(long days)  
LocalDateTime withHours(long hours)  
LocalDateTime withMinutes(long minutes)  
LocalDateTime withSeconds(long seconds) 
-------------------------------------------------    
LocalDateTime withNanos(long nanos)  
LocalDateTime withWeeks(long weeks) 

注意:
    1. 记得接收, 因为时间修改是生成新的LocalDateTime对象,老的LocalDateTime对象不会改变
```

```java
示例:
public static void main(String[] args) {  
    //创建LocalDateTime  
    LocalDateTime now = LocalDateTime.now();  
    //获取时间 -> get系列  
    System.out.println("now.getMonthValue() = " + now.getMonthValue());//12  
    System.out.println("now.getMonth() = " + now.getMonth());//DECEMBER  
    //加时间 -> plus / 减时间 -> minus    LocalDateTime newTime1 = now.plusDays(25);  
    System.out.println("now = " + now);  
    System.out.println("newTime1 = " + newTime1);  
    LocalDateTime newTime2 = now.plusDays(-10);  
    System.out.println("newTime2 = " + newTime2);  
    //设置时间 with    LocalDateTime newTime3 = now.withYear(1991);  
    System.out.println("newTime3 = " + newTime3);  
}
```

#### Period

```java
Period : 年月日的时间间隔类

创建Period对象 : 
    static Period between(LocalDate startDateInclusive, LocalDate endDateExclusive) 

long toTotalMonths()          
```

```java
//示例:
public static void main(String[] args) {  
    //创建年月日时间间隔对象  
    Period between = Period.between(LocalDate.of(1991, 1, 1),  
            LocalDate.now());  
    //P 31Y11M6D  
    System.out.println("between = " + between);  
    //int getDays()  
    System.out.println("between.getDays() = " + between.getDays());  
    //long get(TemporalUnit unit)  
    System.out.println("between.get(ChronoUnit.DAYS) = " + between.get(ChronoUnit.DAYS));  
    System.out.println("between.toTotalMonths() = " + between.toTotalMonths());  
}
```

#### Duration

```java
Duration : 时分秒的时间间隔对象

创建Duration : 
    static Duration between(Temporal startInclusive, Temporal endExclusive)  
        //Temporal :  LocalDateTime/LocalDate/LocalTime的父接口 
        注意 : 创建Duration对象时既可以传入LocalDateTime也可以传入LocalTime [LocalDate : 传入不了的,不接受年月日]

long toDays()  : 快速计算这个时间间隔对象间隔的总天数
```

```java
//示例:
public static void main(String[] args) {  
    //创建年月日时间间隔对象  
    Duration between = Duration.between(LocalDateTime.of(1991, 1, 1,0,0,0),  
            LocalDateTime.now());  
    //PT 279922H43M54.443S  
    System.out.println("between = " + between);  
    //快速获取活了多少天呢 ?    System.out.println("between.toDays() = " + between.toDays());  
}
```

#### BigDecimal

```java
BigDecimal : 关于小数的精准运算的类 
    //不可变的、任意精度的有符号十进制数
构造方法 :
    BigDecimal(double val) //不要用!
     BigDecimal(String val) //强烈推荐!

四则运算方法 :
    加 : BigDecimal add(BigDecimal augend)  
    减 : BigDecimal subtract(BigDecimal subtrahend)  
    乘 : BigDecimal multiply(BigDecimal multiplicand)  
    除 : BigDecimal divide(BigDecimal divisor) 
    模 : BigDecimal remainder(BigDecimal divisor)  

//带舍入模式的除法 :
    BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode)  
        BigDecimal divisor : 除数
        int scale : 保留小数点后几位
        RoundingMode roundingMode : 舍入模式
            RoundingMode.UP : 向上取整
            RoundingMode.DOWN : 向下取整
            RoundingMode.HALF_UP : 四舍五入
```

```java
//示例:
public static void main(String[] args) {  
    BigDecimal bd1 = new BigDecimal("0.1");  
    BigDecimal bd2 = new BigDecimal("0.2");  
    BigDecimal result = bd1.add(bd2);  
    System.out.println("result = " + result);  
}
```

![hyty765rhby4r5](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hyty765rhby4r5.1wvtys515gdc.webp)

## 集合体系[重点]

```java
集合 : 长度可变的"数组" [集合的底层是 : 数组 (集合 = 数组 + 数据结构)]
    数据结构 : 数据在容器中的存储逻辑

集合的特点 :
    1. 长度可变 -> 好处
    2. 只能存对象[不能存基本数据类型数据] -> 弊端(被解决了)

集合的分类 :
    1. 单列集合[长度可变的一维数组]
    2. 双列集合[两根长度可变的一维数组]
```

### 单列集合体系结构

![ge4rstgset345sz](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ge4rstgset345sz.7djhqm6evk00.webp)
![gerdtfyuj45tftgyhbwserf](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gerdtfyuj45tftgyhbwserf.244kcdkwxpcw.webp)

```java
学习容器的方法 :
     1. 如何创建容器
     2. 容器增删改查四类功能
     3. 如何遍历容器
```

### Collection< E >集合

![ghrtghrgberttyg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ghrtghrgberttyg.706huy1xcwk0.webp)

```java
Collection<E>集合 : 单列集合的根节点 [所有的单列集合都是其子接口或者实现类]

创建Collection<E>集合 : 多态的方式创建 [未来使用时不要这样创建,要创建就创建 具体的实现类对象]
    Collection<E> 集合名 = new 具体的实现类<>();
    格式解释 :
        <E> : 泛型
            泛型的作用 : 约束集合中元素的数据类型 
            泛型的使用 : 在创建集合对象时,拿具体的类名去替换E字母 <String>,<Student>..
            注意事项 : 不可以使用基本数据类型名称去替换 E -> <int> 不可以  
            注意事项 : 在等号右边的E可以省略,但是尖括号不要省略
    举例 :         
        Collection<String> col = new ArrayList<>();
        Collection<Student> col = new HashSet<>();

增删改查四类功能 : 
    增 : add
         boolean add(E e) : 往集合中添加元素,并返回添加是否成功
             //等号右边具体new的是ArrayList : 依次往集合中添加元素,永远返回true!
             //等号右边具体new的是HashSet : 往集合中添加元素,并返回添加是否成功!
    删 : remove
        //删一个元素,根据元素的值进行删除
         boolean remove(Object o) : 根据传入的元素值,删除集合中的元素,返回删除是否成功
        //清空
        void clear() : 清空集合中所有的元素
    改 : set  //Collection集合没有修改方法,因为没有索引
    查 : get  //Collection集合没有查询方法,因为没有索引
        int size() : 获取集合的长度,元素的个数 [数组: length属性 , 字符串 : length()方法]

        boolean contains(Object o)  : 查询传入的元素o是否包含于集合中
        boolean isEmpty() : 查询集合是否为空集合 [没有元素的集合叫空集合] 
        //空 和 null的区别 -> 空 : 内容为空  null -> 对象无地址

遍历功能 
    1. 转数组
         Object[] toArray() 
    2. 迭代器 : 专门用来遍历的机器
        Iterator<E> iterator()  
    3. 增强for : 特殊的for循环结构
        单列集合名.for //只要能用迭代器对象遍历,就可以使用增强for [增强for的底层就是迭代器]
```

```java
public static void main(String[] args) {  
    Collection<String> collection = new ArrayList<>();  
    System.out.println("collection1 = " + collection);  

    //添加:add  
    collection.add("张三");  
    collection.add("李四");  
    System.out.println("collection = " + collection);  

    //删除:remove  
    collection.remove("李四");  
    System.out.println("collection = " + collection);  

    //获取集合长度  获取集合中元素的个数:size  
    int collectionSize=collection.size();  
    System.out.println("collection.size() = " + collectionSize);  

    //清空:clear  
    //collection.clear();    //System.out.println("collection = " + collection);  
    //查询元素是否存在于集合中:contains  
    boolean collectionIsHaveEl1=collection.contains("张三");  
    boolean collectionIsHaveEl2=collection.contains("李四");  
    System.out.println("collectionIsHaveEl1 = " + collectionIsHaveEl1);  
    System.out.println("collectionIsHaveEl2 = " + collectionIsHaveEl2);  

    //查询集合是否为空:isEmpty  
    boolean collectionIdIsNull=collection.isEmpty();  
    System.out.println("collectionIdIsNull = " + collectionIdIsNull);  
}
```

### Iterator< E >迭代器

```java
Iterator<E> 迭代器 -> 接口
    抽象方法 :
        boolean hasNext() : "看一眼" -> 判断是否还有下一个元素
        E next() : "舀一勺" -> 获取下一个元素

使用步骤 :
    1, 先获取迭代器对象
    2. 循环判断是否有下一个元素
    3. 如果有 获取下一个元素
```

#### 增强for

```java
增强for : [超级for,foreach]

格式 :
    for(集合的元素类型 变量名: 集合名/数组名){
        //操作变量名就是在操作集合中的元素
    }
//不要使用增强for循环遍历集合时,对集合进行修改操作 ! --> 增强for只用来快速遍历集合!
增强for作用在集合上,增强for的底层原理是 : 迭代器 

增强for作用在数组上,增强for的底层原理是 : 普通fori
```

#### 迭代器的底层原理

```java
迭代器的底层原理 :
    1. 创建迭代器对象,会在内存中生成一个原集合的镜像 [存在于迭代器对象内部]
    2. 创建迭代器对象,迭代器对象会停留在集合对象的第一个可停留位置
    3. 迭代器对象遍历集合时,会时刻对比原集合和镜像内容是否一致 [next()方法在对比]
    4. next() : a. 对比镜像和原集合内容 b.获取指针右边元素 c. 移动指针到下一个可停留位置
    5. hasNext() : 判断指针的右边是否有下一个元素


ConcurrentModificationException : 并发修改异常 [迭代器专属异常]
    产生的原因 : 在迭代器对象遍历集合时,原集合内容和迭代器对象内部镜像不一致就会立刻报并发修改异常
            //如果在使用迭代器对象遍历集合时,使用集合对象对原集合内容做 修改,就会出现此问题
    解决方案 : 
        不要在使用迭代器对象遍历集合时,使用集合对象修改集合内容 [会导致镜像和集合内容不一致!]
```

![tg3waeftgtfr3w4](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tg3waeftgtfr3w4.39lyvcbabmk0.webp)

```java
public static void main(String[] args) {  
        Collection<String> collection = new ArrayList<>();  
        collection.add("张三");  
        collection.add("李四");  
        collection.add("王五");  
        collection.add("赵六");  
        System.out.println("集合元素为: "+ collection);  
        //！！！！！！！！！！错误的向下转型  
//        Object[] collectionArray = collection.toArray();  
//        //collection.toArray()本来就-是-Object[]，所以不能向下转型  
//        String[] s = (String[]) collectionArray;  
//  
//        Animation d = new Dog();  
//        // Dog本来就-属于-Animation类型，所以可以向下转型  
//        Dog dog = (Dog) d;  

        //对集合进行遍历  
        //1  
        System.out.println("使用 toArray() 遍历:");  
        Object[] collectionArray = collection.toArray();  
        for (int i = 0; i < collectionArray.length; ++i) {  
            String objEl = (String) collectionArray[i];  
            System.out.print(objEl+" ");  
        }  
        System.out.println();  

        //2  
        //迭代器:iterator  ,只能用于集合，数组不能用  
        //查看下一个元素是否存在:hasNext  
        //获取迭代器对象  
        System.out.println("使用迭代器 iterator 遍历:");  
        Iterator<String> iterator = collection.iterator();  
        //while循环  
        while (iterator.hasNext()){  
            String el = iterator.next();  
            System.out.print(el+" ");  
        }  
        System.out.println();  

        //3  
        //增强for  
        System.out.println("使用 增强for 遍历:");  
        for (String s : collection) {  
            System.out.print(s+" ");  
        }  
        System.out.println();  
        System.out.println("使用 增强for 遍历数组:");  
        int[] arr={1,2,3,4,5};  
        for (int e : arr) {  
            System.out.print(e+" ");  
        }  
        System.out.println();  
    }
```

## 包装类[重要]

```java
集合中是不可以存储基本数据类型元素 !! [泛型也不可以用基本数据类型名称去替换]
    ArrayList<int> list = new ArrayList<>(); --> 代码编译报错!

如果想往集合中存储基本数据类型的值,必须使用基本数据类型对应的引用数据类型[包装类型]来代替此基本数据类型值  

包装类型 -> 把基本数据类型包装[变成了]成引用数据类型
    基本数据类型        包装类型
    byte            Byte
    short            Short
    int                Integer
    long            Long
    float            Float
    double            Double
    char            Character 
    boolean            Boolean
    //如果要往集合中存储基本数据类型时,集合的泛型就写基本数据类型对应的包装类型即可    

//重点    
1. 包装类型有 自动装箱 和 自动拆箱 的特性 : [不需要创建包装类型对象]
     自动装箱 : 基本数据类型 --> 引用数据类型
         Integer num = 100; //底层隐藏代码 : Integer num = Integer.valueOf(100);
     自动拆箱 : 引用数据类型 --> 基本数据类型
         int i = num; //底层隐藏代码 : int i = num.intValue();

2. 包装类型中有 把字符串类型的基本数据类型值 转换成 真正基本数据类型值的方法 : 
    "100" , "true" , "3.14" -> 字符串类型的基本数据类型值

    "100" + "200" --> "100200"

    通用方法的格式 : static 基本数据类型 parseXxx(字符串类型的基本值)  //除了Character没有
        Integer : static int parseInt("字符串类型的整数值")
        Double :  static double parseDouble("字符串类型的小数值")   
        Boolean : static boolean parseBoolean("字符串类型的布尔值")  
        ...等等    
```

```java
//示例:
public static void main(String[] args) {   
    //ArrayList<int> num = new ArrayList<>();不能用  
    ArrayList<Integer> list = new ArrayList<>();  
    list.add(100);  
    list.add(200);  
    list.add(300);  
    for (Integer integer : list) {  
        System.out.print(integer + " ");  
    }  
}
```

## List< E >接口

![nj5ryhu5ryhe56yh](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/nj5ryhu5ryhe56yh.5oh6oa5pweg0.webp)

```java
List<E>接口的集合具备的特点 :
    1. 长度可变
    2. 只能存对象
    3. 元素有索引
    4. 元素可以重复
    5. 元素存取有序

创建对象
    //多态的形式创建对象
    List<E> 集合名 = new 实现类对象<>();
增删改查 //List集合相较于Collection集合 新增索引操作的方法
    增 :
        boolean add(E e) : 依次添加元素永远返回true
        void add(int index, E element)  : 在指定索引位置插入元素element,后方元素依次后移一位
    删 :
        boolean remove(Object obj) : 按照元素值删除集合中的元素
        E remove(int index) : 删除指定索引位置的元素,把被删除的元素返回
        void clear() : 清空集合中所有的元素
    改 : 
        E set(int index, E element) : 把指定索引位置的元素值修改成 element ,并把被修改的元素返回
    查 : 
        boolean contains(Object obj) : 查询传入的元素是否包含于集合中
        boolean isEmpty() : 查询集合是否为空集合

        int size() : 查询集合中的元素个数/集合的长度
        E get(int index) : 查询指定索引位置的元素值

        int indexOf(Object o)  : 查询o元素第一次出现在集合的索引位置,如果没有出现返回 -1 索引
        int lastIndexOf(Object o)  : 查询o元素最后一次出现在集合的索引位置,如果没有出现返回 -1 索引
遍历 :
    1. 转数组 : Object[] toArray()
    2. 迭代器 : Iterator<E> iterator()
    3. 增强for : 集合名.for
    4. 普通fori : 集合名.fori
    5. 列表迭代器[List的专属迭代器] : ListIterator<E> listIterator()  
```

### ListIterator < E >

```java
ListIterator<E> : 列表迭代器 -> List集合的专属迭代器
    ListIterator<E> 是 Iterator<E> 的子接口 !!

ListIterator<E> 可以解决并发修改异常 !!    
    //并发修改异常: 在使用迭代器对象遍历集合时,创建了迭代器对象后只要集合内容和迭代器中镜像内容不一致就会导致并发修改异常 [不一致 : 对集合内容进行修改]

ListIterator<E>迭代器可以主动修改集合中的元素内容 [先修改镜像内容 并立刻同步回原集合]    
```

```java
//示例
public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"张三","李四","王五");
        for (int i = 0; i < list.size(); ++i) {
            String s = list.get(i);
            if(s.startsWith("张")){
                list.remove(s);
                i--;
            }
        }
        System.out.println("list = " + list);

        for (int i = list.size()-1; i >=0; --i) {
            String s = list.get(i);
            if(s.startsWith("张")){
                list.remove(s);
            }
        }
        System.out.println("list = " + list);
    }
```

![j56trjn567jn](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/j56trjn567jn.7fstnxvckjo0.webp)

### 数据结构

```java
数据结构 : 本质就是数据存储在容器内的一套逻辑 [数据结构属于算法学科中的一类]

Java 攻城狮对数据结构的掌握要求 : 先理解原理,后续[很后续]再研究如何用代码实现[不适合初学者]

集合 = 数组 + 数据结构; [集合的命名 = 数据结构 + 集合类型]
```

#### 数据结构之栈和队列

![423trf342rft243](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/423trf342rft243.1qynojitz5a8.webp)

#### 数据结构之数组结构

![g453tg34e5tg34](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/g453tg34e5tg34.6w55vjo519g0.webp)

### ArrayList< E >集合

![hbr5345rt3](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hbr5345rt3.455cur2zkbc0.webp)

```java
ArrayList<E>集合 : 是元素可重复单列集合中 使用最频繁的集合 !

创建对象
    ArrayList<E> 集合名 = new ArrayList<>();

增删改查 //List集合相较于Collection集合 新增索引操作的方法
    增 :
        boolean add(E e) : 依次添加元素永远返回true
        void add(int index, E element)  : 在指定索引位置插入元素element,后方元素依次后移一位
    删 :
        //int num = 3;
        boolean remove(Object obj) : 按照元素值删除集合中的元素
        E remove(int index) : 删除指定索引位置的元素,把被删除的元素返回
        void clear() : 清空集合中所有的元素
    改 : 
        E set(int index, E element) : 把指定索引位置的元素值修改成 element ,并把被修改的元素返回
    查 : 
        boolean contains(Object obj) : 查询传入的元素是否包含于集合中
        boolean isEmpty() : 查询集合是否为空集合

        int size() : 查询集合中的元素个数/集合的长度
        E get(int index) : 查询指定索引位置的元素值

        int indexOf(Object o)  : 查询o元素第一次出现在集合的索引位置,如果没有出现返回 -1 索引
        int lastIndexOf(Object o)  : 查询o元素最后一次出现在集合的索引位置,如果没有出现返回 -1 索引
遍历 :
    1. 转数组 : Object[] toArray()
    2. 迭代器 : Iterator<E> iterator()
    3. 增强for : 集合名.for -> 仅仅只是遍历集合那么推荐增强for
    4. 普通fori : 集合名.fori --> 最推荐 [有复杂操作]
    5. 列表迭代器[List的专属迭代器] : ListIterator<E> listIterator()      
```

```java
///示例
public static void main(String[] args) {
        ArrayList<String> list=new ArrayList();
        //添加
        list.add("张三");
        list.add("李四");
        list.add("王五");
        list.add("赵六");
        //插入
        list.add(1,"武七");
        //删除
        list.remove("张三");//根据元素
        list.remove(3);//根据索引
        //修改
        list.set(0,"李四111");
        //获取
        System.out.println("list.get(2) = " + list.get(2));

        //包含
        System.out.println("list.contains(\"赵六\") = " + list.contains("王五"));

        //返回第一次出现的位置
        System.out.println("list.indexOf(\"王五\") = " + list.indexOf("王五"));


        //1
        Object[] objects = list.toArray();
        for (Object o : objects) {
            System.out.print(o+" ");
        }
        System.out.println();
        for (int i = 0; i < objects.length; ++i) {
            String o=(String) objects[i];
            System.out.print(o+" ");
        }
        System.out.println();

        //遍历
        //2
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()){
            Object o = iterator.next();
            System.out.print(o+" ");
        }
        System.out.println();

        //3
        for (Object o : list) {
            System.out.print(o+" ");
        }
        System.out.println();

        //4
        for (int i = 0; i < list.size(); i++) {
            Object o = list.get(i);
            System.out.print(o+" ");
        }
        System.out.println();

        //5
        ListIterator listIterator = list.listIterator();
        while (listIterator.hasNext()){
            Object o = listIterator.next();
            System.out.print(o+" ");
        }
    }
```

#### ArrayList集合扩容机制

```java
ArrayList集合扩容机制 :

理论版 : [重点]
    1. 创建ArrayList集合,JVM会在内存中创建一个长度为 0 的Object类型的数组 -> Object[]
        //集合在设计之初 就是被设计为一个 什么引用类型都能存储的长度可变的容器 ; [JDK1.2版本]
        //泛型概念 是从JDK1.5版本才出现的; 是为了更方便的去使用集合! 如果不写泛型,泛型默认是Object
        //泛型是一个停留在编译阶段的类型约束; [泛型是带不到运行阶段] -> 当集合对象在堆内存中真正创建后泛型是没有的;
    2. 当往ArrayList集合添加第一个元素时,底层源码会创建一个长度为10的新数组 -> 第一次扩容
        相当于创建无参ArrayList的时候,JVM创建了一个空数组,并且这个数组的容量为1
    3. 当往ArrayList集合添加第十一个元素时,底层源码会创建一个长度为15的新数组 -> 第二次扩容
    ....
    4. 扩容的公式 : 新数组的长度 = 老数组长度 + (老数组长度 >> 1);  [新长度 = 1.5倍的老长度]

往当往ArrayList集合添加第1024个元素时,底层源码中的数组长度为多少 ?  1234

源码版 : [理解]

拔高环节 :
    //判断 (新容量 - 最大数组长度 > 0)
    if (newCapacity - MAX_ARRAY_SIZE > 0)  //MAX_ARRAY_SIZE : int的最大值 - 8 
            //新容量被赋值为 hugeCapacity 方法的结果
            newCapacity = hugeCapacity(minCapacity);

    private static int hugeCapacity(int minCapacity) {
        //minCapacity = size + 1; --> size : 集合的元素个数[长度]
        //if (minCapacity < 0) 成立的条件是 : 集合中的元素的个数已经达到了int的最大值个
        if (minCapacity < 0) {
            //OOM错误 : 内存溢出错误
            throw new OutOfMemoryError();//暴露错误对象让JVM识别,程序崩溃!
        }

        //(集合元素个数 + 1 > 最大数组长度) ? int的最大值 : 最大数组长度返回[int的最大值-8];
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }

hugeCapacity 为最后几次扩容工作 :
    1. 如果元素个数达到了int的最大值个,程序奔溃 -> OutOfMemoryError 内存溢出错误
    2. 如果元素个数没有达到[int的最大值-8](前提 : 扩容的长度超过了[int的最大值-8]),那么新数组的长度可能不是老数组长度的1.5倍 , 而是数组的最大长度[int的最大值-8]
    3. 如果元素个数达到了最大数组长度[int的最大值-8],那么新数组的长度 就是 int的最大值;
```

![43rf34rf34](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/43rf34rf34.44utkjvpikw0.webp)

#### hugeCapacity方法的逻辑

![gse5343w243fws](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gse5343w243fws.29b93o1dls00.webp)

### 数据结构之链表结构

##### 数据结构之单向链表

![jhh5y4ehh534](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jhh5y4ehh534.7smgfwwfzg.webp)

##### 数据结构之双向链表

![geshgbe546g4w](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/geshgbe546g4w.1c284t1a92tc.webp)

### LinkedList < E >集合

![ft34wtrf34r](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ft34wtrf34r.1ikyphtw3wn4.webp)

```java
LinkedList<E>集合 : 底层数据结构为 双向链表的 单列集合实现;

证明是双向链表 :
    private static class Node<E> {
        //元素
        E item;
        //下一个
        Node<E> next;
        //上一个
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }

创建对象
    LinkedList<E> 集合名 = new LinkedList<>();

增删改查 , 遍历 : 具备List接口中所有的方法 :
    增 :
        boolean add(E e) : 依次添加元素永远返回true
        void add(int index, E element)  : 在指定索引位置插入元素element,后方元素依次后移一位
    删 :
        boolean remove(Object obj) : 按照元素值删除集合中的元素
        E remove(int index) : 删除指定索引位置的元素,把被删除的元素返回
        void clear() : 清空集合中所有的元素
    改 : 
        E set(int index, E element) : 把指定索引位置的元素值修改成 element ,并把被修改的元素返回
    查 : 
        boolean contains(Object obj) : 查询传入的元素是否包含于集合中
        boolean isEmpty() : 查询集合是否为空集合

        int size() : 查询集合中的元素个数/集合的长度
        E get(int index) : 查询指定索引位置的元素值

        int indexOf(Object o)  : 查询o元素第一次出现在集合的索引位置,如果没有出现返回 -1 索引
        int lastIndexOf(Object o)  : 查询o元素最后一次出现在集合的索引位置,如果没有出现返回 -1 索引
遍历 :
    1. 转数组 : Object[] toArray()
    2. 迭代器 : Iterator<E> iterator()
    3. 增强for : 集合名.for
    4. 普通fori : 集合名.fori
    5. 列表迭代器[List的专属迭代器] : ListIterator<E> listIterator()  

LinkedList<E>的特殊方法 : 关于头尾操作的方法        
    void addFirst(E e)  
    void addLast(E e) 

    E getFirst()  
    E getLast()  

    E removeFirst() 
    E removeLast()  
```

```java
//示例  
public static void main(String[] args) {
        HashSet<String> hashSet = new HashSet<>();
        hashSet.add("美国");
        hashSet.add("中国");
        hashSet.add("小日本");
        System.out.println("hashSet = " + hashSet);

        for (String s : hashSet) {
            System.out.print(s + " ");
        }
        System.out.println();

        Iterator<String> iterator = hashSet.iterator();
        while (iterator.hasNext()) {
            String next = iterator.next();
            System.out.print(next + " ");
        }
        System.out.println();
    }
```

### Vector< E >集合

```java
Vector<E>集合 : 最早的单列集合 ["可变"数组]

从JDK2版本并入到了集合体系被ArrayList取代;

Vector<E>内部是线程安全的,在多线程场景下使用单列集合 推荐使用 Vector<E>集合 ! [线程安全伴随着效率低,所以在单线程场景下 使用 ArrayList集合即可]
```

### Set< E >接口

![456g4hr54tergr35g](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/456g4hr54tergr35g.1wp8gygtmav4.webp)

```java
Set<E>接口 : 元素不可重复,元素无索引的单列集合的根节点
    特点:
        1. 长度可变
        2. 只能存对象
        3. 元素不能重复
        4. 元素无索引
        5. 元素存取无序

创建对象的方法 :
    //多态
    Set<E> 集合名 = new 实现类对象<>(); //实现类 : HashSet, TreeSet

增删改查四类功能 :
    1. 增 :
        boolean add(E e) : 添加元素,返回元素是否添加成功! [去重]
    2. 删 :
        boolean remove(Object o) : 按照元素值删除元素,返回删除是否成功
        void clear(): 清空
    3. 改 : 因为没有索引 所以没有set方法
    4. 查 : 因为没有索引 所以没有get方法
            boolean contains(Object o) : 是否包含
            boolean isEmpty(): 是否为空
            int size() : 集合长度/元素个数

遍历 :
    1. 转数组
    2. 迭代器
    3. 增强for
```

#### HashSet

>  散列表的大小默认为16，加载因子为0.75.

![fsadf34gf3w4](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fsadf34gf3w4.3fouqxesgbg0.webp)

```java
//示例 
public static void main(String[] args) {
        HashSet<String> hashSet = new HashSet<>();
        hashSet.add("张三");
        hashSet.add("李四");
        hashSet.add("王五");
        hashSet.remove("王五");
        System.out.println("hashSet.contains(\"李四\") = " + hashSet.contains("李四"));
        Iterator<String> iterator = hashSet.iterator();
        while (iterator.hasNext()) {
            String next = iterator.next();
            System.out.println(next + " ");
        }
    }
```

#### 数据结构之哈希表结构

![fserfr43t384sf](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fserfr43t384sf.3jdadi73xag0.webp)

#### HashSet< E >

```java
HashSet<E> : 元素唯一,元素存取无序,元素无索引的单列集合 [使用最多的Set集合]

HashSet<E> : 底层数据结构是Hash表

创建对象
    HashSet<E> 集合名 = new HashSet<>();
增删改查
    增 : boolean add(E e)
    删 :
        boolean remove(Object obj)
        void clear()
    改 :
    查 : 
        boolean contains(Object obj)
        boolean isEmpty()
        int size()    
遍历
    1. 转数组
    2. 迭代器
    3. 增强for
```

#### HashSet< E >去重的原理

```java
结论 :
        1. 如果类中没有重写hashCode和equals方法,那么哈希表结构中去重是按照对象的地址值去重
        2. 如果想按照对象的属性值进行去重,那么在对象所在的类中需要重写hashCode方法和equals方法(自动生成) [按照属性值去重]

核心代码 : e.hash == hash && ((k = e.key) == key || key.equals(k))      

加分项 :    
为什么要在核心逻辑中加上 : (k = e.key) == key    
    1. 在添加同一个对象时 --> set.add(stu1); set.add(stu1); 
    2. 集合中添加是 包装类类型的对象 set.add(100); set.add(100);
```

#### LinkedHashSet< E >

```java
LinkedHashSet<E> : 底层数据结构是链表 + 哈希表结构的set集合实现;
    特点 :  存取有序,元素唯一,元素无索引

创建对象,增删改查,遍历 功能和 HashSet一致 [LinkedHashSet<E> 是 HashSet<E>的子类]    
```

#### 数据结构之树和二叉树结构

![ghbn465thfc345](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ghbn465thfc345.5kpspfgnre80.webp)

#### 数据结构之二叉查找树

![h57h5698uighy9056](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/h57h5698uighy9056.l871hc6hjlo.webp)

#### 数据结构之平衡二叉树

![fewf34fef343fth56](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fewf34fef343fth56.8jbm3lv0a1k.webp)

#### 数据结构之红黑树结构

![fg4uyhejtrf83u4irt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fg4uyhejtrf83u4irt.48c8zfhsofe.webp)

#### 红黑树结构添加元素的逻辑

![gh5b4v6hjgyh5u64](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gh5b4v6hjgyh5u64.792p8jwr4a80.webp)

#### TreeSet< E >

```java
TreeSet<E> : 底层数据结构是红黑树结构的 Set 集合实现 ;
    红黑树结构可以对数据进行 排序 !!
        但是 需要程序员提供排序规则 !!
    排序的逻辑 : 新老元素做减法!!
        1 . 0 --> 剔除
        2 . > 0 放在老元素的右边
        3 . < 0 放在老元素的左边

创建对象 :
    TreeSet<E> 集合名 = new TreeSet<>();

增删改查 和 遍历 方式 Set 接口一致!!

JDK中常用类默认的排序规则: 默认升序
    Integer :  自然升序
    String : ASCII编码表的升序 / 越靠前字符的优先级别越高 / 中文字符串不讨论排序问题[不知道中文的码值]

自定义类的排序规则 : 需要由程序员通过比较器进行排序规则的提供
    Comparable<E> -> 绑定比较器
    Comparator<E> -> 独立比较器
```

#### Comparable< E > -> 绑定比较器

```java
Comparable<E> -> 绑定比较器 -> 接口
    为什么叫绑定比较器 : 原因是因为它和需要排序对象所在的类绑定在一起[需要排序的类实现接口]

接口中的抽象方法 :
    int compareTo(E o) //重写compareTo就是在编写此类对象的排序规则
        //找到关键因子,对关键因子进行下方逻辑操作
        this - o : 升序逻辑
        o - this : 降序逻辑

使用步骤 :
    //哪个类实现此接口,Comparable<E>接口的泛型就是哪个类!
    1. 找到需要排序的类,让此类实现Comparable<E>,重写接口中的 compareTo 方法
    2. 在方法内编写排序逻辑即可
```

#### Comparator< E > -> 独立比较器

```java
Comparator<E> -> 独立比较器 : 独立存在,优先级比绑定比较器高! -> 接口
    独立比较器 还实现了 排序的解耦问题; -> 因为独立比较器是跟着容器走的,跟着排序方法走的

Comparator<E> -> 独立比较器接口中的抽象方法 :
    //o1 : 新元素 o2 : 老元素
    int compare(E o1,E o2);
    升序 : o1 - o2 , 降序 : o2 - o1;
//重写compare方法的方法体就是在编写容器内元素的排序规则!

使用方法 :
    TreeSet<E>集合的构造中 : 
        TreeSet(Comparator<E> comparator)    
```

#### 数组的排序和List集合的排序

```java
数组的排序 : Arrays[数组操作的工具类]中 sort()
    1. static void sort(基本数据类型元素的数组) : 按照快速排序的逻辑对数组内容进行排序[与比较器无关]
    2. static void sort(引用数据类型元素的数组) :
        a. 要么引用数据类型元素提供默认的排序规则 [绑定比较器]
        b. 要么sort方法提供排序规则 [独立比较器]    
             static void sort(引用数据类型元素的数组,独立比较器对象)
            //static void sort(T[] a, Comparator<T> c) {

//之前学习的是Set集合的排序 : TreeSet            
List集合的排序 :  Collections[单列集合操作的工具类]中 sort()
    //注意 : 因为List集合中允许有重复元素,那么当排序时出现相减等于0的情况,会把新元素放在和老元素相同的位置而不是剔除元素 [剔除元素 : TreeSet集合]
    1. static void sort(List<T> list) //使用绑定比较器的默认排序规则
    2. static void sort(List<T> list, Comparator<T> c) //独立比较器的单独排序规则
```

#### 斗地主案例[无规则排序算法]

## 双列集合概述

```java
双列集合 : 由2根单列集合组成的集合 [映射集合,夫妻对集合] -> 元素成对出现
    双列集合左边的集合 : 键集 key  --> Set
    双列集合右边的集合 : 值集 value --> Collection[元素可以重复的]

双列集合的特点 :
    1. 长度可变
    2. 只能存对象
    3. 键集元素唯一  
    4. 值集元素可以重复  [一妻多夫]  
    5. 双列集合是存取无序 [键集是无序的,值跟着键走(夫唱妇随)]    
    6. 双列集合没有索引    
```

![vgegy5h4t6u78j](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/vgegy5h4t6u78j.356cwlbh90e0.webp)

#### 双列集合体系结构

![kmujiyhhase45](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/kmujiyhhase45.1rcn12n3aha8.webp)

#### Map<K,V>

```java
Map<K,V> 接口 是双列集合的根节点 ;

创建对象
   Map<K,V> 集合名 = new 具体的双列集合对象<>();
    格式解释 :
        <K,V> : 泛型 [K : 键集元素的数据类型,V : 值集元素的数据类型]
            //学号 - 学生对象 , 姓名 - 年龄 , 属性名 - 属性值
增删改查
   增/改 :  V put(K key, V value)  -> 往集合添加一组映射关系
               //添加成功返回null , 如果添加失败[如果添加了重复的键]返回此键对应的值   
   删 :
     V remove(Object key) : 根据传入的键删除集合中的一对元素,并返回被删除的值
     boolean remove(Object key,Object value) : 根据键值对删除集合中的一对元素,返回删除是否成功            void clear() : 清空集合中所有的元素
   查 :
     V get(Object key) : 根据键获取此键对应的值 [根据丈夫找媳妇]
     int size() : 获取集合的长度和元素个数 [有多少对元素]

     Set<K> keySet()  : 获取双列集合所有的键 [召集所有的丈夫]  -> 双列集合的键是一个Set集合
     Collection<V> values() : 获取双列集合的所有值 [召集所有的妻子] -> 双列集合的值是一个Collection集合 

     boolean containsKey(Object key)  : 查询集合中是否有传入的键
     boolean containsValue(Object value)  : 查询集合中是否有传入的值
     boolean isEmpty() : 判断集合是否为空集合   

遍历 :
    1. 召集所有的丈夫[Set集合] -> 遍历Set集合拿到所有的丈夫 -> 根据丈夫寻找妻子
           Set<K> keySet() -> 遍历这个set集合 -> V get(Object key)

    2. 召集所有的结婚证[Set集合(存储着结婚证对象)] -> 遍历Set集合获取到每一本结婚证 -> 根据结婚证对象获取结婚证对象内封装的丈夫和妻子 //Entry : 结婚证类型
        Set<Map.Entry<K,V>> entrySet()  --> 遍历这个Set集合[得到每一个结婚证对象] -> 根据entry对象获取entry对象上封装的键和值[Entry : K getKey(), V getValue()] 
```

#### HashMap<K,V>

```java
HashMap<K,V> : 最常用的双列集合 

HashMap<K,V> 的对象创建,增删改查,遍历功能和Map接口中的方法一致!

HashMap<K,V>的去重 : 对键集元素进行去重,而键集元素是一个Set集合; HashMap集合的键集就是HashSet,所以去重原理 根据 hashCode方法 和 equals方法的编写逻辑进行去重操作!    
```

#### LinkedHashMap<K,V>

```java
LinkedHashMap<K,V> : 底层数据结构是 链表 + 哈希表
    键集 : LinkedHashSet<K> -> 元素变成存取有序的集合

LinkedHashMap<K,V> 是 HashMap<K,V> 的子类!

HashMap<K,V> 是一个存取无序的双列集合
LinkedHashMap<K,V> 是一个存取有序的双列集合        
```

#### TreeMap<K,V>

```java
TreeMap<K,V> 底层数据结构是一个红黑树结构的双列集合 [红黑树能实现排序功能,一定记得提供排序规则]

    TreeMap<K,V>的键集就是一个TreeSet<K>,要想对TreeMap<K,V>集合元素进行排序只需要对键集提供排序规则即可 !
    1. 绑定比较器 : 泛型类自己实现
    2. 独立比较器 : TreeMap(Comparator<K> comparator) 


TreeMap<K,V> 的对象创建,增删改查,遍历功能和Map接口中的方法一致!    
```

#### Hashtable<K,V>

```java
Hashtable<K,V> : 最早的双列集合 [和Vector很像];
    在JDK2版本Hashtable<K,V>被HashMap<K,V>取代;

Hashtable<K,V>是一个线程安全的双列集合[效率低下],所以在多线程场景下不推荐使用Hashtable<K,V>,推荐使用ConcurrentHashMap<K,V>[线程安全,但是效率比Hashtable<K,V>要高]
```

#### 双列集合的案例

```java
    public static void main(String[] args) {
        //学习集合<班级名称,班级学生集合>
        //班级学生集合<学生对象,年龄>
        HashMap<String, HashMap<Student, String>> classes = new HashMap<>();
        HashMap<Student, String> c1 = new HashMap<>();
        c1.put(new Student("李四",20),"男");
        HashMap<Student, String> c2 = new HashMap<>();
        c2.put(new Student("王五",21),"女");
        c2.put(new Student("赵六",22),"女");
        HashMap<Student, String> c3 = new HashMap<>();
        c3.put(new Student("张三",23),"男");
        c3.put(new Student("吴七",24),"男");
        c3.put(new Student("孙八",25),"男");
        classes.put("高一",c1);
        classes.put("高二",c2);
        classes.put("高三",c3);
        //1.keySet()+keySet()
//        Set<String> classNames = classes.keySet();
//        for (String className : classNames) {
//            HashMap<Student, String>  classValue= classes.get(className);
//            Set<Student> students = classValue.keySet();
//            for (Student student : students) {
//                String name = student.getName();
//                int age = student.getAge();
//                String sex = classValue.get(student);
//                System.out.println("班级为="+className+",学生名字="+name+",学生年龄="+age+",学生性别="+sex);
//            }
//        }

        //2.entrySet()+entrySet()
//        Set<Map.Entry<String, HashMap<Student, String>>> classKeys = classes.entrySet();
//        for (Map.Entry<String, HashMap<Student, String>> classKey : classKeys) {
//            String className = classKey.getKey();
//            HashMap<Student, String> classValue = classKey.getValue();
//            Set<Map.Entry<Student, String>> students = classValue.entrySet();
//            for (Map.Entry<Student, String> student : students) {
//                Student sKey = student.getKey();
//                String name = sKey.getName();
//                int age = sKey.getAge();
//                String sex = student.getValue();
//                System.out.println("班级为="+className+",学生名字="+name+",学生年龄="+age+",学生性别="+sex);
//            }
//        }

        //3.keySet+entrySet()
//        Set<String> classNames = classes.keySet();
//        for (String className : classNames) {
//            HashMap<Student, String> classKeys = classes.get(className);
//            Set<Map.Entry<Student, String>> students = classKeys.entrySet();
//            for (Map.Entry<Student, String> student : students) {
//                Student key = student.getKey();
//                int age = key.getAge();
//                String name = key.getName();
//                String sex = student.getValue();
//                System.out.println("班级为="+className+",学生名字="+name+",学生年龄="+age+",学生性别="+sex);
//            }
//        }

        //4.keySet+entrySet()
        Set<Map.Entry<String, HashMap<Student, String>>>  classKeys = classes.entrySet();
        for (Map.Entry<String, HashMap<Student, String>> classKey : classKeys) {
            String className = classKey.getKey();
            HashMap<Student, String> value = classKey.getValue();
            Set<Student> students = value.keySet();
            for (Student student : students) {
                String name = student.getName();
                int age = student.getAge();
                String sex = value.get(student);
                System.out.println("班级为="+className+",学生名字="+name+",学生年龄="+age+",学生性别="+sex);
            }
        }
    }
```

## 泛型

```java
泛型 : <E>,<K,V>,<T> -> 一种说不清楚的引用数据类型
    泛 : 说不清楚
    型 : 引用数据类型
    解释 : 泛型在定义时并不清楚是什么引用数据类型,只有在具体使用泛型时拿具体的引用数据类型去替换泛型

使用场景 : 在定义集合时使用泛型,泛型的作用是用来约束集合中元素的数据类型; //约束数据类型

泛型的格式 :
    <字母>

注意事项:
    1. 字母可以是大写也可以是小写 //<E>,<e> --> 推荐使用大写字母
    2. 单个泛型可以是多个字母也可以是单个字母 // <OK> -> 推荐使用单个大写字母
    3. 一个尖括号内可以有多个泛型类型的定义 //<K,V> , <A,B,C>     
```

### 泛型类

```java
泛型类 : 在定义类时,在类的声明上定义泛型,此类就是泛型类; [ArrayList<E> 集合都是泛型类]

泛型类的定义格式 :
    public class 类名<泛型>{
        //代表一种不明确的引用数据类型可以在本类中使用了
    }

泛型类的使用 :
    在使用此类时,拿具体的引用数据类型类名去替换泛型字母,替换后泛型就指定了数据类型!//如果不理会泛型,泛型默认是Object类型
```

### 泛型方法

```java
泛型方法 : 在定义方法是,在方法的声明上定义泛型,此方法就是泛型方法;

泛型方法的定义格式 :
    权限修饰符 状态修饰符 <泛型> 返回值类型 方法名(形式参数列表){
        //代表一种不明确的引用数据类型可以在本方法内使用了 [作用域比泛型类的泛型小]
        方法体;
        //return 值;
    }

泛型方法的使用 : 泛型方法中的泛型一定会在形参上使用.那么在调用此方法时,传入什么类型的实参泛型就会变成什么类型
```

### 泛型接口[泛型的延用问题]

```java
泛型接口 : 在定义接口时,在接口的声明上定义泛型,此接口就是泛型接口; [List<E> 接口就是泛型接口]

泛型接口的定义格式 :
    public interface 接口名<泛型>{
        //代表一种不明确的引用数据类型可以在本接口中使用了
    }

泛型接口的使用 :
    在使用此接口类时,拿具体的引用数据类型类名去替换泛型字母,替换后泛型就指定了数据类型!//如果不理会泛型,泛型默认是Object类型

泛型的延用问题 :
    在父类型/父接口中定义了泛型类型,那么子类可以延用父类/父接口的泛型类型;
        延用了 : 实现类/子类就可以继续使用此泛型 [绝大部分集合体系都是延用了父类/父接口的泛型]
        没有延用 : 实现类/子类就没有泛型概念了[不可以再使用父类/父接口的泛型]  --> Properties [属性集(Hashtable的子类)] 
```

### 泛型的通配符和上限下限问题[重点]

```java
需求 : 创建一个方法,方法的形参可以接受任意泛型类型的ArrayList集合

泛型的通配符 : ?

    泛型的上限 : 泛型类型最大是什么 -> <? extends 类型>
    泛型的下限 : 泛型类型最小是什么 -> <? super 类型>

    不管上限和下限都是包含 extends / super 后方的类型

    //为什么是下限呢 ?
    因为元素的类型是 T 类型, 要把T类型的元素装进容器内,那么容器的泛型必须>=T类型
    //此方法的功能是批量添加元素 : 把第二个参数的elements 批量添加到第一个集合中!
    static <T> boolean addAll(Collection<? super T> c, T... elements)

    看上限下限集合泛型的小技巧 : 看不见
    static <T> boolean addAll(Collection<T> c, T... elements)    
```

## 异常体系

```java
异常体系的根节点 : Throwable
    错误 : Error  // 严重的代码问题,必须修改代码才能解决问题
    异常 : Exception // 可以被代码手段[抛出去/包起来]解决的问题

常见的运行时异常 :
    1.NullPointerException - 空指针引用异常
    2.ClassCastException - 类型强制转换异常。
    3.IllegalArgumentException - 传递非法参数异常。
    4.ArithmeticException - 算术运算异常
    5.ArrayStoreException - 向数组中存放与声明类型不兼容对象异常
    6.IndexOutOfBoundsException - 下标越界异常
    7.NegativeArraySizeException - 创建一个大小为负数的数组错误异常
    8.NumberFormatException - 数字格式异常
    9.SecurityException - 安全异常
    10.UnsupportedOperationException - 不支持的操作异常 
```

![sgerg3wtgw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/sgerg3wtgw.2yzc5f1psxc0.webp)

### JVM如何处理异常

```java
JVM处理异常的方式 :
        1. 立刻中断程序 [崩了]
        2. 打印异常的信息
            a. 异常的类型 : ArithmeticException [数学异常]
            b. 异常的错误信息 : / by zero
            c. 异常产生的行号 : at com.atguigu.e_jvm_control.Demo.main(Demo.java:7)
            d. 异常产生的线程 : Exception in thread "main"
            e. 以红色字体打印异常的信息
            f. 开启新的线程打印异常错误信息
```

### 自己何如处理程序异常

> 总结 : 
> 
> 1. 现阶段 : 
>    1. 运行时期异常不用管,程序报错解决问题
>    2. 编译时期异常抛出去,程序编译不报错可以执行;如果程序真的有问题,再去解决问题
> 2. 后阶段 :
>    1. 运行时期和编译时期都推荐使用包起来方式[try..catch方式]处理 -> 程序员自己去解决问题

#### 抛出去

```java
抛出去 : 把异常抛给调用者处理,谁调用谁处理 [如果谁都不处理,给JVM处理] -> 现阶段最常用的方式

抛出去的关键字 : throws

使用格式 :
    权限修饰符 状态修饰符 返回值类型 方法名(形式参数列表) throws 异常类型{

    }
//运行时期异常不用抛
运行时期异常抛出去处理 : 没有意义 [运行时期异常抛出去处理没有意义,抛不抛都一样]
//编译时期异常抛出能让程序正常执行,但是该报错还是报错
编译时期异常抛出去处理 : 有意义,程序可以正常执行了 [如果代码没有异常,不报错;如果程序有异常,控制台报错]

抛出去的注意事项 :
    1. 多态性 : 抛出父类型异常,子类型异常一并抛出了 [最大抛 Exception]
    2. 一个方法上可以抛出多个异常类型,中间使用逗号分隔     
```

#### 包起来

```java
包起来 : 把异常通过 try .. catch 进行包裹; --> 程序员自己去处理

try .. catch 的格式 :
    try{
       可能产生异常的代码 
    }catch(需要捕获异常类型 对象名){
       处理异常的代码
    }

//包起来的方式是未来最常用的处理异常的方式 [现阶段推荐throws方式]
运行时期异常包起来处理 : 有意义,程序可以继续向后执行
编译时期异常包起来处理 : 有意义,1. 程序不再编译报错 , 2. 程序可以继续向后执行
```

![4e45g4whyuw4](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/4e45g4whyuw4.2ylgchzmxz80.webp)

### try .. catch的执行流程

```java
try .. catch : 关于异常的一种处理方式 

try .. catch 的格式 :
    try{
       可能产生异常的代码 
    }catch(需要捕获异常类型 对象名){
       处理异常的代码
    }   

try .. catch的执行流程 :
        1. try中有异常,catch捕获到 :
            a . 代码执行到异常位置会立刻终止try中代码执行
            b . 进入catch中执行catch中处理异常的代码
            c . 执行try .. catch 结构后方的代码

        2. try中有异常,catch没有捕获到 :
            a . 代码执行到异常位置会立刻终止try中代码执行
            b . 没有捕获到异常,不会进入到catch中执行
            c . JVM介入处理这个没有捕获到的异常对象 [按照JVM处理异常的方式处理]

        3. try中无异常 [catch肯定捕获不到] :
            a. 正常执行try代码块中的代码,不会进入到catch中执行
            b. 当try中代码执行完毕,执行 try..catch 格式后方代码

        4. try中有异常,catch捕获到了,但是catch代码有异常 :
            a. 代码执行到异常位置会立刻终止try中代码执行
            b. 进入catch中执行catch中处理异常的代码
            c. 遇到catch中的异常代码[因为没有处理],JVM介入处理异常[按照JVM处理异常的方式处理]
            d. 不会执行 try..catch 格式后方代码
```

```java
//示例:   
    public static void main(String[] args) {
        System.out.println("开始");
        try {
            System.out.println(7 / 0);
        } catch (ArithmeticException e) {
            System.out.println("数字异常");
        }
        System.out.println("结束");
    }
```

### try .. catch的注意事项

```java
    1. 一个catch只能捕获一种异常类型
        //(ArithmeticException e,NullPointerException e1) 错误写法
    2. 一个try..catch格式中可以有多个catch
        try{
            //可能产生问题的代码
        }catch(异常类型A e){//捕获数学异常
            //自己处理异常A的方式
        }catch (异常类型B e){
            //自己处理异常B的方式
        }
    3. 捕获父类型异常,子类型异常也一并捕获了 [多态性]
    4. 不可以先捕获父类型异常再捕获子类型异常
    5. 可以先捕获子类型异常再捕获父类型异常 [可以写捕获父类型异常兜底]
    6. 不可以把同一类型的异常捕获2次
```

### 异常对象的功能

```java
方法来自于 Throwable[异常或者错误的根节点] : 
    String getMessage() : 打印异常的错误信息
    String toString() : 打印异常的类型 + 异常的错误信息
    ** void printStackTrace() :  打印异常的类型 + 异常的错误信息 + 异常出现的代码位置 + 红色字体 + 开启新线程打印
```

### finally 关键字

```java
final : 修饰符[adj] -> 最终的
    类 : 类没有子类
    方法 : 方法不能被重写
    成员变量 : 成员变量变自定义常量 [public static final 数据类型 常量名 = 初始化值;] 
    局部变量 : 局部变量变局部的常量 [不能变量]  -> 实时final修饰

finally 代码块 : 一般try..catch使用 [try..catch..finally / try..finally]
    格式 : 
        try{
           可能产生异常的代码 
        }catch(需要捕获异常类型 对象名){
           处理异常的代码
        }finally{
           无论如何都会执行的代码 // 关闭资源
        } 

注意 : finally 只有在前方使用了 System.exit(0);[JVM退出] 的方法,那么 finally不执行的;
```

### 如何自定义异常并使用

```java
如何自定义异常 : [领导干的]
    1. 定义类 [类名 : XxxxException] -> Xxx: 表示异常的名称
    2. 思考此异常类型是编译时期异常还是运行时期异常 :
        a. 编译时期异常 : extends Exception
        b. 运行时期异常 : extends RuntimeException
    3. 在异常类中自动生成构造方法 :
        第一个 : 无参构造
        第二个 : 带有一个String形参的带参构造   

自定义异常如何使用 :
    1. 定义方法
    2. 编写异常产生的条件 [在某种条件下产生异常]
    3. 在条件内,创建异常对象并暴露!
        throw new 异常类型("异常的提示信息"); //throw作用是把异常对象暴露给JVM
```

```java
//示例:
public class Test {
    public static void main(String[] args) throws MyException {
        setName("张三刚刚给法国");
        setAge(210);
    }
    public static void setName(String name) throws MyException {
        if(name.length()>5){
            throw new MyException("姓名不合法");
        }
        System.out.println("姓名为："+name);
    }
    public static void setAge(int age){
        if(age<0||age>=120){
            throw new MyRuntimeException("年龄不合法");
        }
        System.out.println("年龄为："+age);
    }
}
class MyException extends Exception{
    public MyException() {
        super();
    }

    public MyException(String message) {
        super(message);
    }
}
class MyRuntimeException extends RuntimeException{
    public MyRuntimeException() {
        super();
    }

    public MyRuntimeException(String message) {
        super(message);
    }
}
```

### throws 和 throw

```java
throws : 抛出异常 
    作用的位置 : 在方法的形参后方[ throws 异常类型 ]
    抛出异常 : 编译时期异常不处理[抛/包]就不可以运行程序,运行时异常可以不抛!  

throw : 暴露异常
    作用的位置 : 在方法内!
    暴露异常 : 在方法内的某个条件下,把创建的异常对象暴露出来,被JVM识别!
        暴露的是运行时期异常 : 可以在暴露时不处理
        暴露的是编译时期异常 : 需要在暴露时处理,处理方案推荐 : 抛出去的方式处理[不要选包起来]    
```

## 递归算法

```java
算法 : 逻辑[写代码的方式]
递归算法 : 在方法内部调用方法自己[本身]
    //递 : 递进[不断的调用方法自己] 归 : 回归[方法执行完毕弹栈的过程]

生活中的递归 :
    //主角 : 老和尚 和 小和尚
    从前有座山,山里有座庙,庙里有个老和尚,老和尚给小和尚讲故事;
        //主角 : 故事里的老和尚 和 故事里的小和尚
        从前有座山,山里有座庙,庙里有个老和尚,老和尚给小和尚讲故事;
            从前有座山,山里有座庙,庙里有个老和尚,老和尚给小和尚讲故事;
                从前有座山,山里有座庙,庙里有个老和尚,老和尚给小和尚讲故事;
                    .....
     //主角 : 我                    
     学编程 -> 赚钱 -> 娶媳妇 -> 生娃娃;
        //主角 : 我的娃娃
        学编程 -> 赚钱 -> 娶媳妇 -> 生娃娃;
            //主角 : 我娃娃的娃娃
            学编程 -> 赚钱 -> 娶媳妇 -> 生娃娃;
                学编程 -> 赚钱 -> 娶媳妇 -> 生娃娃;
                    ...

递归的要素 :
    1. 递归一定要定义方法,方法一定要有参数
    2. 要在方法内部调用方法本身[再次调用方法时,要有实际参数的变化(变化方向 : 往出口的方向改变[往不满足递归条件的方向前进])]
    3. 递归一定要有出口 //递归如果没有出口,会一直执行下去[永远不会停-> 栈内存溢出错误(StackOverflowError)]   

递归的编写技巧 :
    //找到递归逻辑的出口 ,  找到递归逻辑的规律
    1. 定义方法,定义方法的形参
    2. 先编写出口 [判断]    
    3. 再编写递归逻辑    

递归注意事项 : 不要太多次调用方法, 因为会出现栈内存溢出的错误!        
```

```java
//示例    
    public static void main(String[] args) {
        System.out.println(Sum1(5));
        System.out.println(Sum2(5));
        System.out.println(Jiec(5));
    }
    public static int Sum1(int n){
        int sum=0;
        for (int i = 1; i <= n; i++) {
            sum+=i;
        }
        return sum;
    }
    public static int Sum2(int n){
        if(n==1){
            return 1;
        }
        return n+Sum2(n-1);
    }
    public static int Jiec(int n){
        if(n==1){
            return 1;
        }
        return n*Jiec(n-1);
    }
```

#### 递归程序执行内存图

![tyhuj67u4r5tn](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tyhuj67u4r5tn.75e62dgr1ng0.webp)

### 快速排序逻辑

```java
快速排序逻辑 : 快排为什么快? [Arrays.sort(基本数据类型数组) 底层逻辑就是快速排序!]
```

![qd2ws3e78iuu](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/qd2ws3e78iuu.651duwfzwqs0.webp)

![huifru8935t83](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/huifru8935t83.1toq7gmiv1k0.webp)

## Debug断点调试[IDEA的使用]

```java
bug[甲虫] : 程序中的错误/问题;

Debug断点调试 : 让程序慢下来,一句话一句话的慢慢执行 [程序员点一下执行一下]

Debug断点调试的使用步骤 :
    1. 打断点 [怎么打断点 : 对着行号点一下[取消就再点一下]/哪里打断点 : 哪里不会点哪里]
    2. 右键以Debug模式执行程序
    3. 点击下一步按钮,一步一步的执行程序

断点调试注意事项 :
    1. 一个方法内点一个断点
    2. 如果涉及到方法的调用,最好在调用的方法内打一个断点[需要查看方法内部的执行流程]  
```

![jkwe4rf67u8i123efg5](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jkwe4rf67u8i123efg5.1t6lzcb2h15s.webp)
