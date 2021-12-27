springweb默认使用Gson序列化

背景：一直默认使用jackson的，最近有个场景用了动态类（数据库里存的Java代码，业务读取后编译成class），才发现使用的jackson不能把这个动态类解析成json，换成gson后解决了，所以就全局更改了。
又因为springfox swagger默认使用的jackson，所以改成了Gson后还需要修改springfox定义json序列化。
看来还是jackson支持更好，轻易不要换它，以免引入不必要的问题麻烦。
代码如下：
```java
    /**
    * 使用Springweb定义的GsonHttpMessageConverter
    */
    @Bean
    public GsonHttpMessageConverter customGsonHttpMessageConverter(Gson gson) {
        return new GsonHttpMessageConverter(gson);
    }

    /**
     * Springfox 默认使用 Jackson 做序列化。若使用 Gson，则解析的 Json 格式会有误，需要修改Json.class序列化。
     */
    @Bean
    public Gson gson() {
        return new GsonBuilder()
                .registerTypeAdapter(Json.class, new SpringfoxJsonToGsonAdapter())
                .create();
    }
    public class SpringfoxJsonToGsonAdapter implements JsonSerializer<Json> {
        @Override
        public JsonElement serialize(Json json, Type type, JsonSerializationContext jsonSerializationContext) {
            return JsonParser.parseString(json.value());
        }
    }
```