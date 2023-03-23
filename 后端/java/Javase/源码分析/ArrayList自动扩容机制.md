```java
        //默认的构造函数,初始化一个ArrayList实例,默认为空数组,默认容量为10;       
        ArrayList<Test> a = new ArrayList<>();
		
		private static final int DEFAULT_CAPACITY = 10;//默认对象数组的容量大小=10;
        private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};//空Object数组;
        transient Object[] elementData;//ArrayList底层Object数组,用于存放实际元素;
		private int size;//ArrayList中实际存放的元素个数,默认值(初始值)=0;
		
        //默认的构造函数,默认为空数组,默认容量为10; 
		public ArrayList() {
            //elementData初始化为空Object数组
        	this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    	}

		public boolean add(E e) {
            //确保当前ArrayList维护的数组拥有内部容量，size是添加前数组内元素的数量
            ensureCapacityInternal(size + 1); 
            //将元素存储在数组elementData的尾部 
            elementData[size++] = e;
            return true;
    	}
		
        //计算得到数组中的最小容量
		private void ensureCapacityInternal(int minCapacity) {
            //判断是否调用空参构造进行初始化
            if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
                //最小容量取默认容量10与minCapacity之间的最大值)
                minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
            }
        	ensureExplicitCapacity(minCapacity);
   		 }

		private void ensureExplicitCapacity(int minCapacity) {
            modCount++;
            //当插入数据之后的数组容量比elementData声明时的大小要大时==》ArrayList的存储能力不足,需要进行扩容
            if (minCapacity - elementData.length > 0)
                grow(minCapacity);
    	}

		//
		private void grow(int minCapacity) {
            //获取当前elementData数组的长度
            int oldCapacity = elementData.length;
            //设置新的存储容量为原来的1.5倍
            int newCapacity = oldCapacity + (oldCapacity >> 1);
            //与最小需要空间比较
            //判断扩容后的新数组容量是否比minCapacity小,满足就使用minCapacity,不满足就使用新容量newCapacity
            if (newCapacity - minCapacity < 0)
                newCapacity = minCapacity;
            //与最大数组长度比较
            //若新容量大于默认的最d,则重新计算新数组容量
            if (newCapacity - MAX_ARRAY_SIZE > 0)
                newCapacity = hugeCapacity(minCapacity);
            //调用Arrays.copyOf()方法将elementData数组指向新的内存空间newCapacity的连续空间
            //并将elementData的数据复制到新的内存空间
            elementData = Arrays.copyOf(elementData, newCapacity);
    	}
		
		//minCapacity大于MAX_ARRAY_SIZE时返回Integer.MAX_VALUE
		//否则返回Integer.MAX_VALUE-8作为新数组长度
        private static int hugeCapacity(int minCapacity) {
            if (minCapacity < 0)
                //OOM错误 : 内存溢出错误
                //暴露错误对象让JVM识别,程序崩溃!
           		 throw new OutOfMemoryError();
            return (minCapacity > MAX_ARRAY_SIZE) ?
                Integer.MAX_VALUE :
                MAX_ARRAY_SIZE;
        }
```

