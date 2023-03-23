## HashSet特征

```java
1.实现了Collection接口的子类：Set接口。
2.HashSet的储存是无序的，即遍历的顺序和我们添加的顺序无关。
3.HashSet底层的数据结构是哈希表。根据哈希表得出的哈希值代表该对象的储存位置
4.HashSet不能添加重复的元素，是基于HashMap实现的
5.按照地址值去重不用重写equals()和hashCode(),按照du
```

## HashSet去重原理

```java
HashSet中的元素不重复是利用了HashMap中的key不能重复的特性
HashMap是通过数组+链表+红黑树的方式实现,如下:
```

![hn56ejhn5ehes](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hn56ejhn5ehes.3i8fbmgr2dm0.webp)

```java
    HashSet<Test> h = new HashSet<>();
	
	private transient HashMap<E,Object> map;
	private static final Object PRESENT = new Object();

	public boolean add(E e) {
        //add()方法实际上是在调用HashMap的put()方法
        //HashSet的底层实现逻辑是基于HashMap实现的
        return map.put(e, PRESENT)==null;
    }
	
	//put()向一个HashMap中插入键值对
	public V put(K key, V value) {
        //putVal()封装了HashMap的去重的底层实现,也是HashSet去重特点的实现。
        return putVal(hash(key), key, value, false, true);
    }

	final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict) {
       	//...
        //h
       if (e.hash == hash &&((k = e.key) == key || (key != null && key.equals(k))))
		//...
        return null;
    }
```

