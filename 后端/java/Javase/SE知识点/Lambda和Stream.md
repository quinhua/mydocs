## 面向函数式编程思想

```java
面向过程编程思想 : 凡事必躬亲 [所有编程思想的鼻祖]
面向对象编程思想 : 自己的事情别人做 [懒人思维]

面向函数式编程思想 : 当某个场景只能是一类对象完成且此类对象的功能有且仅有一个,那么可以省略此类对象的创建和此类对象功能方法的声明[对面向对象编程思想的简化]

面向函数式编程的使用前提 : 被使用的必须是 函数式接口 才行!
```

## 函数式接口

```java
函数式接口 : 有且仅有一个抽象方法的接口是函数式接口 [自定义常量,默认方法,静态方法,私有方法的个数没有要求]

回忆已经学过的函数式接口 :
	Runnable 接口 : void run()
    Callable<V> 接口 : V call()    
    Comparable<T> 接口 : 绑定比较器 
    	int compareTo(T o)
    Comparator<T> 接口 : 独立比较器
    	int compare(T o1,T o2)
    FileFilter 接口 : 文件过滤器
    	boolean accept(File fileName)
        
    @FunctionalInterface : 约束函数式接口的格式   
    示例:
        @FunctionalInterface
        public interface Demo {
            abstract void show();
            public static final String NAME="张三";
        }
```

```java
//示例
    public static void main(String[] args) {
        //匿名内部类
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("子线程-"+Thread.currentThread().getName()+"启动了");
            }
        }).start();
        Runnable runnable1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("子线程-"+Thread.currentThread().getName()+"启动了");
            }
        };
        new Thread(runnable1).start();
        Runnable runnable2 = ()->{System.out.println("子线程-"+Thread.currentThread().getName()+"启动了");};
        new Thread(runnable2).start();
        Runnable runnable3 = ()->System.out.println("子线程-"+Thread.currentThread().getName()+"启动了");
        new Thread(runnable3).start();
        //Lambda表达式
        new Thread(()->{
                System.out.println("子线程-"+Thread.currentThread().getName()+"启动了");
            }
        ).start();
        new Thread(()->System.out.println("子线程-"+Thread.currentThread().getName()+"启动了")).start();
    }
```

## lambda

### Lambda表达式[关于匿名内部类的简化操作]

```java
Lambda表达式的前提条件 :
	被使用的必须是函数式接口!! 
        
Lambda表达式的格式 :
    (重写方法的形参列表) -> {
		//编写重写方法的方法体;
    }

Lambda表达式的简化格式 :
        1. 如果重写的抽象方法有且仅有一句话,
        那么可以省略重写方法的大括号,语句结尾的分号和return [要省必须一起省]
        2. Lambda表达式重写方法的形参类型可以省略
        3. 如果抽象方法的形参列表有且仅有一个形参,可以省略包裹形参的小括号
```

```java
//无参无返回值
    @FunctionalInterface
    public interface InterA {
        public abstract void show();
    }
    public class DemoA {
        public static void main(String[] args) {
            //匿名内部类
            InterA(new InterA() {
                @Override
                public void show() {
                    System.out.println("show()方法执行");
                }
            });
            InterA interA1 = new InterA() {
                @Override
                public void show() {
                    System.out.println("show()方法执行");
                }
            };
            interA1.show();
            InterA interA2 = ()->{System.out.println("show()方法执行");};
            interA2.show();
            InterA interA3 = ()->System.out.println("show()方法执行");
            interA3.show();
            //Lambda表达式
            InterA(()->{
                System.out.println("show()方法执行");
            });
            //方法体只有一句话,省略花括号、分号、return
            InterA(()->System.out.println("show()方法执行"));
        }
        public static void InterA(InterA intera){
            intera.show();
        }
    }
//无参有返回值
    @FunctionalInterface
    public interface InterB {
        public abstract int get();
    }
	public class DemoB {
        public static void main(String[] args) {
            //匿名内部类
            InterB(new InterB() {
                @Override
                public int get() {
                    return 666;
                }
            });
            InterB interB1 = new InterB(){
                @Override
                public int get() {
                    return 666;
                }
            };
            System.out.println("interB1.get() = " + interB1.get());
            InterB interB2 = ()->{return 666;};
            System.out.println("interB2.get() = " + interB2.get());
            InterB interB3 = ()->666;
            System.out.println("interB3.get() = " + interB3.get());
            //Lambda表达式
            InterB(()->{return 666;});
            InterB(()->666);
        }
        public static void InterB(InterB interb){
            System.out.println("interb.get() = " + interb.get());
        }
	}
//有参无返回值
    @FunctionalInterface
    public interface InterC {
        public abstract void operation(int a,int b);
    }
    public class DemoC {
        public static void main(String[] args) {
            int num1 = 10;
            int num2 = 20;
            //匿名内部类
            Add(new InterC() {
                @Override
                public void operation(int a, int b) {
                    System.out.println(a+"+"+b+"="+(a+b));
                }
            }, num1, num2);
            InterC interC1 = new InterC() {
                @Override
                public void operation(int a, int b) {
                    System.out.println(a+"+"+b+"="+(a+b));
                }
            };
            Add(interC1, num1, num2);
            InterC interC2 = (int a, int b) -> {
                System.out.println(a+"+"+b+"="+(a+b));
            };
            Add(interC2, num1, num2);
            InterC interC3 = (int a, int b) -> System.out.println(a+"+"+b+"="+(a+b));
            Add(interC3, num1, num2);
            //Lambda表达式
            Add((a,b)->{
                System.out.println(a+"+"+b+"="+(a+b));
            },10,20);
            Add((a,b)-> System.out.println(a+"+"+b+"="+(a+b)),num1,num2);
            Subtraction((a,b)->{
                System.out.println(a+"-"+b+"="+(a-b));
            },num1,num2);
            Subtraction((a,b)->System.out.println(a+"-"+b+"="+(a-b)),num1,num2);
        }

        public static void Add(InterC interc, int a, int b) {
            interc.operation(a,b);
        }

        public static void Subtraction(InterC interc, int a, int b) {
            interc.operation(a, b);
        }
    }
//有参有返回值
    @FunctionalInterface
    public interface InterD {
        public abstract int add(int a,int b);
    }	
    public class DemoD {
        public static void main(String[] args) {
            int num1=10;
            int num2=20;
            //匿名内部类
            InterD(new InterD() {
                @Override
                public int add(int a, int b) {
                    return a+b;
                }
            },num1,num2);
            InterD interD1 = new InterD() {
                @Override
                public int add(int a, int b) {
                    return a+b;
                }
            };
            InterD(interD1,num1,num2);
            InterD interD2 = (int a,int b)->{return a+b;};
            InterD(interD2,num1,num2);
            InterD interD3 = (int a,int b)->a+b;
            InterD(interD3,num1,num2);
            //Lambda表达式
            InterD((a,b)->{
                return a+b;
            },num1,num2);
            InterD((a,b)-> a+b,num1,num2);
        }
        public static void InterD(InterD interd,int a,int b){
            int result = interd.add(a, b);
            System.out.println(a+"+"+b+"=" + result);
        }
    }
```



### 方法引用[关于Lambda表达式的简化操作] 

```java
最基础的使用方式 : 创建子类,实现类 继承或者实现父类/父接口
匿名内部类 : 匿名内部类是对常规使用 子类,实现类 的简化书写 [适用于任何继承或者实现的场景]
Lambda表达式 : Lambda表达式是对匿名内部类的简写 [前提条件 : 被使用的必须是函数式接口] -> 比较苛刻
方法引用 : 方法引用是对Lambda表达式的简写 [了解内容 : 因为条件太苛刻了]
	1. 必须能用Lambda
	2. 要求重写的方法方法体有且仅有一句代码 -> 极为严苛
	3. 必须要求重写的方法体的内容是 : -> 变态条件
		a. 对象调方法 -> 对象名::方法名
		b. 类名调用静态方法 -> 类名::静态方法名
		c. 创建对象 -> 类名::new
		d. 创建数组 -> 数组类型::new (int[]::new)
```

```java
//示例
    @FunctionalInterface
        interface InterAA {
            public abstract void print(String str);
        }
        @FunctionalInterface
        interface InterBB{
            public abstract int change(String str);
        }
        @FunctionalInterface
        interface InterCC{
            public abstract Date get();
        }
        @FunctionalInterface
        interface InterDD {
            public abstract int[] get(int length);
     }
    public class DemoMethod {
        public static void main(String[] args) {
            //1.方法引用 [对象调用方法] -> 对象名::方法名
            methodAA((str)->{
                System.out.println(str);
            },"hello");
            methodAA((str)->System.out.println(str),"hello");
            methodAA(System.out::println,"hello");

            System.out.println("---------------");
            //2.方法引用 [类名调用静态方法] -> 类名::静态方法名
            InterBB interBB2 = (str)->{return Integer.parseInt(str);};
            methodBB(interBB2,"100");
            InterBB interBB3 = (str)->Integer.parseInt(str);
            methodBB(interBB3,"100");
            InterBB interBB4 = Integer::parseInt;
            methodBB(interBB4,"100");

            System.out.println("---------------");
            //3.方法引用 [创建对象] -> 类名::new
            methodCC(()->{return new Date(0);});
            methodCC(Date::new);

            System.out.println("---------------");
            //4.方法引用 [创建数组对象] -> 数组类型::new
            methodDD(new InterDD() {
                @Override
                public int[] get(int length) {
                    return new int[length];
                }
            },3);
            methodDD((length)->{
                return new int[length];
            },4);
            methodDD(int[]::new,5);
        }
        public static void methodAA(InterAA interAA,String str){
            interAA.print(str);
        }
        public static void methodBB(InterBB interBB,String str){
            int num = interBB.change(str);
            System.out.println("num = " + num);
        }
        public static void methodCC(InterCC interCC){
            Date date = interCC.get();
            System.out.println("date = " + date);
        }
        public static void methodDD(InterDD interDD,int length){
            int[] list = interDD.get(length);
            Arrays.toString(list);
            int len = list.length;
            System.out.println("len = " + len);
        }
    }
```



### Stream流[关于集合的简化操作]

```java
Stream流[关于集合的简化操作] -> Stream流中大量使用到 Lambda表达式 和 方法引用;

Stream流 : 流-> 流水线[工厂]

案例需求

按照下面的要求完成集合的创建和遍历

- 创建一个集合，存储多个字符串元素
- 把集合中所有以"张"开头的元素存储到一个新的集合
- 把"张"开头的集合中的长度为3的元素存储到一个新的集合
- 遍历上一步得到的集合

		//创建一个集合，存储多个字符串元素
        ArrayList<String> list = new ArrayList<String>();

        list.add("林青霞");
        list.add("张曼玉");
        list.add("王祖贤");
        list.add("柳岩");
        list.add("张敏");
        list.add("张无忌");
```

# Stream流

### Stream流的工作流程

![sefhyt34ewrftgbhe56dsr](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/sefhyt34ewrftgbhe56dsr.1pirmfmelt28.webp)

### 进流方法[进工厂的方法]

```java
1. 单列集合 : 
	Collection<E> 接口中的默认方法 : default Stream<E> stream()  //单列集合名.stream()
        
2. 数组 :
	Arrays 数组操作工具类中 : static <T> Stream<T> stream(T[] array)  //Arrays.stream(任意类型的数组)
        
3. 一组同类型的数据 :
	Stream<T> 接口中的静态方法 : static <T> Stream<T> of(T... values) //Stream.of(任意个同类型的数据)
        
4. 双列集合 //不能直接进流 --> 先把双列集合变成单列集合再进流
     a. 先获取双列集合的键集[Set集合],再Set集合进流
     		//双列集合.keySet().stream()
     b. 先获取双列集合的值集[Collection集合],再Collection集合进流
     		//双列集合.values().stream()
     c. 先获取键值对对象集合[Set集合],再Set集合进流	
     		//双列集合.entrySet().stream()
```

```java
//示例
    public static void main(String[] args) {
        //1.单列集合
        ArrayList<String> list1 = new ArrayList<>();
        list1.add("张三");
        list1.add("李四");
        list1.stream().forEach(s->System.out.print(s+" "));
        System.out.println("\n"+"----------");
        HashSet<String> list2 = new HashSet<>();
        list2.add("王五");
        list2.add("赵六");
        list2.stream().forEach(s->System.out.print(s+" "));

        System.out.println("\n"+"----------");
        //2.数组
        int[] arr=new int[3];
        arr[0]=1;
        arr[1]=2;
        arr[2]=3;
        Arrays.stream(arr).forEach(s->System.out.print(s+" "));

        System.out.println("\n"+"----------");
        //3.一组同类型数据
        Stream.of("宋七", "孙八").forEach(s->System.out.print(s+" "));

        System.out.println("\n"+"----------");
        //4.双列集合-推荐使用entrySet().stream()
        HashMap<String, Integer> map = new HashMap<>();
        map.put("中国", 1);
        map.put("美国", 2);
        map.put("苏联", 3);
        map.keySet().stream().forEach((s)->System.out.print(s+" "));
        System.out.println("\n"+"----------");
        map.values().stream().forEach((s)->System.out.print(s+" "));;
        System.out.println("\n"+"----------");
        map.entrySet().stream().forEach((s)->System.out.print(s+" "));;
    }
```

### 中间方法[车间方法]

#### 过滤车间

```java
Stream<T> filter(Predicate<? super T> predicate)
    
Predicate[函数式接口] : 判断性接口
	抽象方法 : boolean test(T t)  
        //重写test方法就是在编写过滤条件
```

### 跳过车间和截取车间

```java
跳过车间 : Stream<T> skip(long n)  -> long n : 跳过几个元素,保留剩下的元素

截取车间 : Stream<T> limit(long maxSize)  -> long maxSize : 保留几个元素 [从第一个元素开始保留]
```

#### 去重车间

```java
Stream<T> distinct() : 把流中重复的元素进行去重,得到一个新的没有重复元素的流
	//去重逻辑 : 看equals方法的逻辑
```

### 排序车间

```java
Stream<T> sorted()  : 按照T元素默认的排序规则对流中元素进行排序

Stream<T> sorted(Comparator<? super T> comparator ): 按照独立比较器提供的功能对流对象元素进行排序
```

### 合并车间

```java
static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b) : 把a流和b流的元素合并到新的流对象中 
```

### 转换车间

```java
<R> Stream<R> map(Function<T,R> mapper)  
    
Function : 函数式接口 [转换型接口]
	有且仅有一个抽象方法 : R apply(T t)
        //重写apply的逻辑就是如何把T类型的元素转换成R类型的逻辑
```

### 终结方法[出工厂的方法]

> 特点 : 方法的返回值类型不再是 Stream类型

### 遍历出厂

```java
void forEach(Consumer<? super T> action)  
    Consumer : 函数式接口 -> 消费性接口
    	有且仅有一个抽象方法 : void accept(T t)  
            //重写accept的方法体就是在操作流中的数据 [外层的foreach方法你看做循环,重写的方法体看做循环体语句]
```

### 统计出厂

```java
long count()  : 统计流中元素的个数
```

### 收集出厂

```java
1. 收集成数组出厂 : Object[] toArray()  

//收集成集合出厂的方法 :
    <R,A> R collect(Collector<T,A,R> collector)  
    	//Collector 收集器接口 [不是一个函数式接口]
    	//如何获取收集器的实现类对象 : 借助工具类 Collectors
    		static <T> Collector<T,?,List<T>> toList()  -> Collectors.toList()
    		static <T> Collector<T,?,Set<T>> toSet()  -> Collectors.toSet()
    		static <T,K,V> Collector<T,?,Map<K,V>> toMap(
    Function<T,K> keyMapper, Function<T,V> valueMapper)  
    
2. 收集成List集合出厂 : [元素可重复]
		List<T> list = 流对象.collect(Collectors.toList());
3. 收集成Set集合出厂 : [元素不可以重复]
		Set<T> set = 流对象.collect(Collectors.toSet());
4. 收集成Map集合出厂 :
		//如何把流中数据转成键和值元素 : Function : 函数式接口 [转换型接口]
			有且仅有一个抽象方法 : R apply(T t)
        //重写apply的逻辑就是如何把T类型的元素转换成R类型的逻辑
		流对象.collect(Collectors.toMap(如何把流中数据转成键元素,如何把流中数据转换成值元素));
```

```java
//示例1
     public static void main(String[] args) {
            //skip
            Stream.of(1, 2, 3, 4, 5).skip(2).forEach(el -> System.out.print(el + " "));
            System.out.println("\n"+"-------------");
            //limit
            Stream.of(1, 2, 3, 4, 5).limit(3).forEach(el -> System.out.print(el + " "));
            System.out.println("\n"+"-------------");
            //distinct
            Stream.of(1, 1, 2, 2, 3, 3, 4, 4, 5, 5).distinct().forEach(el->System.out.print(el+" "));
            System.out.println("\n"+"-------------");
            //sorted-默认排序(升序)
            Stream.of(2, 1,9,3,5,2,7,12,0,4).sorted().forEach(el->System.out.print(el+" "));
            System.out.println("\n"+"-------------");
            //独立比较器的排序规则
            //升序:o1-o2、降序:o2-o1
            Stream.of(2, 1,9,3,5,2,7,12,0,4).sorted((o1,o2)->o1-o2).forEach(el->System.out.print(el+" "));
            System.out.println("\n"+"++++++++++++++++++");
            Stream.of(2, 1,9,3,5,2,7,12,0,4).sorted((o1,o2)->o2-o1).forEach(el->System.out.print(el+" "));
            System.out.println("\n"+"-------------");
            //concat合并
            Stream.concat(Stream.of(1,2,3),Stream.of(4,5)).forEach(el->System.out.print(el+" "));
            System.out.println("\n"+"-------------");
            //map转换
    //        Stream.of("1","2","3","4","5").map(new Function<String, Integer>() {
    //            @Override
    //            public Integer apply(String s) {
    //                return Integer.parseInt(s);
    //            }
    //        });
            Stream.of("1","2","3","4","5").map(Integer::parseInt).forEach(el->System.out.print(el+" "));;
     }
//示例2
	public static void main(String[] args) {
        //count
        System.out.println(Stream.of(1, 2, 3, 4, 5).count());
        //collect-1.收集成数组出厂-Collectors.toList()-元素可重复
        System.out.println(Stream.of(1, 2, 3, 4, 5).collect(Collectors.toList()));
        //collect-2.收集成List出厂-Collectors.toList()-元素可重复
        System.out.println(Stream.of(1, 2, 3, 4, 5).collect(Collectors.toList()));
        //collect-3.收集成Set出厂-Collectors.toSet()-元素不重复
        System.out.println(Stream.of(1, 2, 3, 4, 5, 1).collect(Collectors.toSet()));
        System.out.println(Stream.of(1, 2, 3, 4, 5).collect(Collectors.toSet()));
        //collect-4.收集成Map出厂
        System.out.println(Stream.of("1", "2", "3", "4", "5")
                .collect(Collectors.toMap(new Function<String, String>() {
                    @Override
                    public String apply(String s) {
                        return s;
                    }
                }, new Function<String, Integer>() {
                    @Override
                    public Integer apply(String s) {
                        return Integer.parseInt(s);
                    }
                })));
        System.out.println(Stream.of("1", "2", "3", "4", "5")
                .collect(Collectors.toMap(x->x, Integer::parseInt)));
    }
```



### 综合案例

```java
案例需求 :
    /*
        - 现在有两个ArrayList集合，分别存储6名男演员名称和6名女演员名称，要求完成如下的操作:
            - 1.男演员只要名字为3个字的前三人
            - 2.女演员只要姓林的，并且不要第一个
            - 3.把过滤后的男演员姓名和女演员姓名合并到一起
            - 4.把上一步操作后的元素作为构造方法的参数创建演员对象 -> String 转换 Actor
            - 5.把转换后的流收集成List集合并遍历List集合
    */
    演员类Actor :
    public class Actor implements Serializable {
        private static final long serialVersionUID = -6779071120303143237L;
        private String name;

        public Actor() {
        }

        public Actor(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Actor)) return false;
            Actor actor = (Actor) o;
            return Objects.equals(getName(), actor.getName());
        }

        @Override
        public int hashCode() {
            return Objects.hash(getName());
        }

        @Override
        public String toString() {
            return "Actor{" +
                    "name='" + name + '\'' +
                    '}';
        }
	}
    public class ActorDemo {
        public static void main(String[] args) {
            //1
            ArrayList<String> manList = new ArrayList<>();
            manList.add("周润发");
            manList.add("成龙");
            manList.add("刘德华");
            manList.add("吴京");
            manList.add("周星驰");
            manList.add("李连杰");
            ArrayList<String> womanList = new ArrayList<String>();
            womanList.add("林心如");
            womanList.add("张曼玉");
            womanList.add("林青霞");
            womanList.add("柳岩");
            womanList.add("林志玲");
            womanList.add("王祖贤");
            Stream.concat(
                    manList.stream().filter(el->el.length()==3).limit(3),
                    womanList.stream().filter(el->el.startsWith("林")).skip(1)
                    )
                    .map(Actor::new)
                    .collect(Collectors.toList())
                    .forEach(el-> System.out.println(el+" "));
            
       		//2
    //        Stream.concat(
    //                Stream.of("周润发","成龙","刘德华","吴京","周星驰","李连杰").filter(el->el.length()==3).limit(3),
    //                Stream.of("林心如","张曼玉","林青霞","柳岩","林志玲","王祖贤").filter(el->el.startsWith("林")).skip(1)
    //        )
    //                .map(Actor::new)
    //                .collect(Collectors.toList())
    //                .forEach(el-> System.out.print(el+" "));
        }
    }
```
