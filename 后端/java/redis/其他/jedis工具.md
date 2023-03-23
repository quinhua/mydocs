> `jedis`可以通过java程序连接到redis

## 1.pom.xml安装依赖

```xml
	<jedis.version>4.2.3</jedis.version>

	<dependency>
          <groupId>redis.clients</groupId>
          <artifactId>jedis</artifactId>
          <version>${jedis.version}</version>
    </dependency>
```

## 2.测试

```xml
public class JedisTest {
    private static final String HOST ="192.168.199.110";
    private static final int PORT=6379;
    public static void main(String[] args) {
        //set();
        get();
    }
    public static void set(){
        Jedis jedis = new Jedis(HOST, PORT);
        String country = jedis.set("country","中国");
        jedis.close();
    }
    public static void get(){
        Jedis jedis = new Jedis(HOST, PORT);
        String country = jedis.get("country");
        System.out.println("country = " + country);
        jedis.close();
    }
}
```

## 3.编写工具类

```xml
//utils/JedisUtils.java

public class JedisUtils {
        private static JedisPool jedisPool;

        static {
            InputStream inputStream = JedisUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
            Properties properties = new Properties();
            try {
                properties.load(inputStream);
                int maxTotal = Integer.valueOf(properties.getProperty("jedis.maxTotal"));
                int maxIdle = Integer.valueOf(properties.getProperty("jedis.maxIdle"));
                long maxWaitMillis = Integer.valueOf(properties.getProperty("jedis.maxWaitMillis"));
                String host = properties.getProperty("jedis.host");
                int port = Integer.valueOf(properties.getProperty("jedis.port"));

                //创建Jedis链接池对象
                JedisPoolConfig poolConfig = new JedisPoolConfig();
                poolConfig.setMaxTotal(maxTotal);
                poolConfig.setMaxIdle(maxIdle);
                poolConfig.setMaxWaitMillis(maxWaitMillis);
                jedisPool = new JedisPool(
                        poolConfig,
                        host,
                        port
                );
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

        public static Jedis getJedis() {
            return jedisPool.getResource();
        }

        public static void close(Jedis jedis) {
            if ( null != jedis) {
                jedis.close();
                jedis = null;
            }
        }
}
```

## 4.配置jedis.properties

```xml
# redis主机地址
jedis.host=192.168.199.110

# redis端口 
jedis.port=6379

# 最大连接数 
jedis.maxTotal=1000

# 控制一个pool最多有多少个状态为空闲的jedis实例 
jedis.maxIdle=100

# 最大的等待毫秒数，如果超过等待时间，则直接抛JedisConnectionException
jedis.MaxWaitMillis=10000
```

## 5.工具类的使用

```xml
public class JedisTest {
    public static void main(String[] args) {
        //set();
        get();
    }
    public static void set(){
        Jedis jedis = JedisUtils.getJedis();
        String country = jedis.set("country","中国");
        JedisUtils.close(jedis);
    }
    public static void get(){
        Jedis jedis = JedisUtils.getJedis();
        String country = jedis.get("country");
        System.out.println("country = " + country);
        JedisUtils.close(jedis);
    }
}
```

