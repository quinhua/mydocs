## 1.jackson下载

```java
//下载jar包
-jackson-annotations
-jackson-core
-jackson-databind
```

## 2.jackson使用

```java
		//jackson工具
        ObjectMapper om = new ObjectMapper();
        //对象转json
        String jacksonToJson = om.writeValueAsString(users);
        System.out.println("jacksonToJson = " + jacksonToJson);
        //json转对象
        Users jacksonFromJson = om.readValue(jacksonToJson, Users.class);
        System.out.println("jacksonFromJson = " + jacksonFromJson);
```

## 3.封装

```java
//utils/JsonUtil.java
public class JsonUtil {
    /**
     * @param request
     * @param tClass
     * @return java.lang.Object
     * @Descript 获取客户端传递的json类型的请求参数，并且转成JavaBean对象
     **/
    public static Object parseJsonToBean(HttpServletRequest request, Class<? extends Object> tClass) {
        //请求体的数据就在BufferReader里面
        BufferedReader bufferedReader = null;
        try {
            //1. 获取请求参数:如果是普通类型的请求参数"name=value&name=value"那么就使用request.getXXX()
            //如果是json请求体的参数，则需要进行json解析才能获取
            bufferedReader = request.getReader();
            StringBuilder stringBuilder = new StringBuilder();
            String body = "";
            while ((body = bufferedReader.readLine()) != null) {
                stringBuilder.append(body);
            }
            //目标:将json里面的数据封装到JavaBean里面:这就叫json解析
            ObjectMapper objectMapper = new ObjectMapper();
            return objectMapper.readValue(stringBuilder.toString(), tClass);
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        } finally {
            try {
                assert bufferedReader != null;
                bufferedReader.close();
            } catch (IOException e) {
                e.printStackTrace();
                throw new RuntimeException(e.getMessage());
            }
        }
    }

    /**
     * @param response
     * @param object
     * @Descript 将对象转成json字符串并且响应到客户端
     **/
    public static void writeResult(HttpServletResponse response, Object object) {
        try {
            //添加一行代码
            response.setContentType("text/html;charset=utf-8");
            ObjectMapper objectMapper = new ObjectMapper();
            String jsonStr = objectMapper.writeValueAsString(object);
            response.getWriter().write(jsonStr);
        } catch (IOException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }
}
```

