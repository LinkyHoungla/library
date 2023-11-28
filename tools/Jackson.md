# Spring MVC

SpringMVC的HTTP序列化和反序列化核心是`HttpMessageConverter`，在SSM项目中，我们需要在xml配置文件中注入`MappingJackson2HttpMessageConverter`。告诉SpringMVC我们需要JSON格式的转换。

```jsx
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
  <property name="messageConverters">
   <list >
      <ref bean="mappingJacksonHttpMessageConverter" />
   </list>
  </property>
 </bean>
 <bean id="mappingJacksonHttpMessageConverter"
  class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
  <property name="supportedMediaTypes">
   <list>
    <value>text/html;charset=UTF-8</value>
   </list>
  </property>
 </bean>
```

我们使用的`@RequestBody`和`@ResponseBody`注解，他们的作用就是将报文反序列化/序列化POJO对象。在请求时`SpringMVC`会在请求头中寻找`contentType`参数，然后去匹配能够处理这种类型的消息转换器。而在返回数据时，`SpringMVC`根据请求头的`Accept`属性，再将对象转换成响应报文。

针对于JSON这个结构，Spring默认使用Jackson来进行序列化和反序列化。在SpringBoot中会将`MappingJackson2HttpMessageConverter`自动装载到IOC容器中。

```
源码：org.springframework.http.converter.json.MappingJackson2HttpMessageConverter
```



```java
@Configuration
@ConditionalOnClass(ObjectMapper.class)
@ConditionalOnBean(ObjectMapper.class)
@ConditionalOnProperty(name = HttpMessageConvertersAutoConfiguration.PREFERRED_MAPPER_PROPERTY,
            havingValue = "jackson", matchIfMissing = true)
protected static class MappingJackson2HttpMessageConverterConfiguration {

  @Bean
  @ConditionalOnMissingBean(value = MappingJackson2HttpMessageConverter.class,
                            ignoredType = { 
"org.springframework.hateoas.mvc.TypeConstrainedMappingJackson2HttpMessageConverter",
"org.springframework.data.rest.webmvc.alps.AlpsJsonHttpMessageConverter" })
  public MappingJackson2HttpMessageConverter mappingJackson2HttpMessageConverter(ObjectMapper objectMapper) {
    return new MappingJackson2HttpMessageConverter(objectMapper);
  }
}
```

## 自定义

`MappingJackson2HttpMessageConverter`借助的是Jackson来完成序列化，那么若是可以修改`Jackson`的配置，便可自定义输出响应内容。

对于Date或者LocalDateTime类型，我们希望按照`yyyy-MM-dd HH:mm:ss`格式输出。

借着我们对`ObjectMapper`进行功能加强(设置时间类序列化格式)。注意：该段代码并未覆盖SpringBoot自动装配的`ObjectMapper`对象，而是加强其配置。详情请参考——[SpringBoot2.x下的ObjectMapper配置原理](https://www.jianshu.com/p/68fce8b23341)



```kotlin
   @Bean
    public Jackson2ObjectMapperBuilderCustomizer customJackson() {
        return jacksonObjectMapperBuilder -> {
            //若POJO对象的属性值为null，序列化时不进行显示
            jacksonObjectMapperBuilder.serializationInclusion(JsonInclude.Include.NON_NULL);
            
            //针对于Date类型，文本格式化
            jacksonObjectMapperBuilder.simpleDateFormat("yyyy-MM-dd HH:mm:ss");

            //针对于JDK新时间类。序列化时带有T的问题，自定义格式化字符串
            JavaTimeModule javaTimeModule = new JavaTimeModule();
            javaTimeModule.addSerializer(LocalDateTime.class,new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
            javaTimeModule.addDeserializer(LocalDateTime.class,new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
            jacksonObjectMapperBuilder.modules(javaTimeModule);

        };
    }
```

再次请求，可以看到，得到的结果如下图所示。我们已经改变@ResponseBody对返回对象序列化的格式输出。

![img](https:////upload-images.jianshu.io/upload_images/16013479-a6f1561689e0a72a.png?imageMogr2/auto-orient/strip|imageView2/2/w/399/format/webp)

SpringBoot2.x会自动装载`MappingJackson2HttpMessageConverter`进行消息转换。而`MappingJackson2HttpMessageConverter`会获取Spring容器中的`ObjectMapper`配置，来进行Jackson的序列化和反序列化。

> 注意：在SpringBoot2.x环境下，不要将自定义的ObjectMapper对象放入Spring容器！这样会将原有的ObjectMapper配置覆盖。

`ObjectMapper`是JSON操作的核心，`Jackson`的JSON操作都是在`ObjectMapper`中实现的。

### springboot

SpringBoot2.x默认装载了ObjectMapper到Spring容器，在yml配置文件中使用`spring.jackson`为前缀的配置可以修改，也可以在代码中实现`Jackson2ObjectMapperBuilderCustomizer`接口去修改ObjectMapper配置。



```java
@Configuration
//注解通俗的说就是Spring工程中引用了Jackson的包 才会构建这个bean
@ConditionalOnClass(ObjectMapper.class)
public class JacksonAutoConfiguration {
    //默认的feature配置
    private static final Map<?, Boolean> FEATURE_DEFAULTS;
    //SpringBoot2.x配置WRITE_DATES_AS_TIMESTAMPS=false，即Date日期不会转换为时间戳类型
    static {
        Map<Object, Boolean> featureDefaults = new HashMap<>();
        featureDefaults.put(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        FEATURE_DEFAULTS = Collections.unmodifiableMap(featureDefaults);
    }

    @Bean
    public JsonComponentModule jsonComponentModule() {
        return new JsonComponentModule();
    }

    @Configuration
    @ConditionalOnClass(Jackson2ObjectMapperBuilder.class)
    static class JacksonObjectMapperConfiguration {
        @Bean
        @Primary
        //若自定义ObjectMapper放入Spring容器中，该配置不会生效！
        @ConditionalOnMissingBean
        //获取Spring容器中的Jackson2ObjectMapperBuilder 。
        public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder) {
            return builder.createXmlMapper(false).build();
        }
    }

    @Configuration
    @ConditionalOnClass(Jackson2ObjectMapperBuilder.class)
    static class JacksonObjectMapperBuilderConfiguration {

        private final ApplicationContext applicationContext;

        JacksonObjectMapperBuilderConfiguration(ApplicationContext applicationContext) {
            this.applicationContext = applicationContext;
        }

        @Bean
        //若自定义配置Jackson2ObjectMapperBuilder 对象，那么该配置不会生效
        @ConditionalOnMissingBean
        //List<Jackson2ObjectMapperBuilderCustomizer>会读取Spring容器中所有
        //Jackson2ObjectMapperBuilderCustomizer子类Bean，并按照order优先级进行排序。
        public Jackson2ObjectMapperBuilder jacksonObjectMapperBuilder(
                List<Jackson2ObjectMapperBuilderCustomizer> customizers) {
            Jackson2ObjectMapperBuilder builder = new Jackson2ObjectMapperBuilder();
            builder.applicationContext(this.applicationContext);
            //按照order读取所有自定义的Jackson2ObjectMapperBuilderCustomizer 对象。
            customize(builder, customizers);
            return builder;
        }
        //调用子类实现customize方法定制Jackson2ObjectMapperBuilder 对象
        private void customize(Jackson2ObjectMapperBuilder builder,
                List<Jackson2ObjectMapperBuilderCustomizer> customizers) {
            for (Jackson2ObjectMapperBuilderCustomizer customizer : customizers) {
                customizer.customize(builder);
            }
        }

    }
    //SpringBoot2.x提供的修改ObjectMapper的配置类，可以在配置文件中修改配置。
    @Configuration
    @ConditionalOnClass(Jackson2ObjectMapperBuilder.class)
    //配置以`spring.jackson`作为前缀。
    @EnableConfigurationProperties(JacksonProperties.class)
    static class Jackson2ObjectMapperBuilderCustomizerConfiguration {

        @Bean
        public StandardJackson2ObjectMapperBuilderCustomizer standardJacksonObjectMapperBuilderCustomizer(
                ApplicationContext applicationContext,
                JacksonProperties jacksonProperties) {
            return new StandardJackson2ObjectMapperBuilderCustomizer(applicationContext,
                    jacksonProperties);
        }

        static final class StandardJackson2ObjectMapperBuilderCustomizer
                implements Jackson2ObjectMapperBuilderCustomizer, Ordered {

            private final ApplicationContext applicationContext;
            //配置文件中的属性
            private final JacksonProperties jacksonProperties;

            StandardJackson2ObjectMapperBuilderCustomizer(
                    ApplicationContext applicationContext,
                    JacksonProperties jacksonProperties) {
                this.applicationContext = applicationContext;
                this.jacksonProperties = jacksonProperties;
            }
            //优先级为0，若我们想自定义实现StandardJackson2ObjectMapperBuilderCustomizer类，那么order优先级要大于0
            @Override
            public int getOrder() {
                return 0;
            }
            //根据配置去定制builder对象
            @Override
            public void customize(Jackson2ObjectMapperBuilder builder) {

                if (this.jacksonProperties.getDefaultPropertyInclusion() != null) {
                    builder.serializationInclusion(
                            this.jacksonProperties.getDefaultPropertyInclusion());
                }
                if (this.jacksonProperties.getTimeZone() != null) {
                    builder.timeZone(this.jacksonProperties.getTimeZone());
                }
               //设置序列化和反序列化的特征
                configureFeatures(builder, FEATURE_DEFAULTS);
                configureVisibility(builder, this.jacksonProperties.getVisibility());
                configureFeatures(builder, this.jacksonProperties.getDeserialization());
                configureFeatures(builder, this.jacksonProperties.getSerialization());
                configureFeatures(builder, this.jacksonProperties.getMapper());
                configureFeatures(builder, this.jacksonProperties.getParser());
                configureFeatures(builder, this.jacksonProperties.getGenerator());
                configureDateFormat(builder);
                //设置序列化的命名策略
                configurePropertyNamingStrategy(builder);
                configureModules(builder);
                configureLocale(builder);
            }

            private void configureFeatures(Jackson2ObjectMapperBuilder builder,
                    Map<?, Boolean> features) {
                features.forEach((feature, value) -> {
                    if (value != null) {
                        if (value) {
                            builder.featuresToEnable(feature);
                        }
                        else {
                            builder.featuresToDisable(feature);
                        }
                    }
                });
            }

            private void configureVisibility(Jackson2ObjectMapperBuilder builder,
                    Map<PropertyAccessor, JsonAutoDetect.Visibility> visibilities) {
                visibilities.forEach(builder::visibility);
            }
           //配置Date格式化类
            private void configureDateFormat(Jackson2ObjectMapperBuilder builder) {
                // 支持全限定名的DateFormat或者时间格式串例如`yyyy-MM-dd HH:mm:ss`
                String dateFormat = this.jacksonProperties.getDateFormat();
                if (dateFormat != null) {
                    try {
                        Class<?> dateFormatClass = ClassUtils.forName(dateFormat, null);
                        builder.dateFormat(
                                (DateFormat) BeanUtils.instantiateClass(dateFormatClass));
                    }
                    catch (ClassNotFoundException ex) {
                       //若不是上送DateFormat的全限定名，那么使用SimpleDateFormat 进行格式化
                        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(
                                dateFormat);
                        // 在Jackson 2.6.3 我们需要设置一个 TimeZone时区，若未设置，则使用默认的时区（UTC）
                        TimeZone timeZone = this.jacksonProperties.getTimeZone();
                        if (timeZone == null) {
                            timeZone = new ObjectMapper().getSerializationConfig()
                                    .getTimeZone();
                        }
                        simpleDateFormat.setTimeZone(timeZone);
                        builder.dateFormat(simpleDateFormat);
                    }
                }
            }

            private void configurePropertyNamingStrategy(
                    Jackson2ObjectMapperBuilder builder) {
                // We support a fully qualified class name extending Jackson's
                // PropertyNamingStrategy or a string value corresponding to the constant
                // names in PropertyNamingStrategy which hold default provided
                // implementations
                String strategy = this.jacksonProperties.getPropertyNamingStrategy();
                if (strategy != null) {
                    try {
                        configurePropertyNamingStrategyClass(builder,
                                ClassUtils.forName(strategy, null));
                    }
                    catch (ClassNotFoundException ex) {
                        configurePropertyNamingStrategyField(builder, strategy);
                    }
                }
            }

            private void configurePropertyNamingStrategyClass(
                    Jackson2ObjectMapperBuilder builder,
                    Class<?> propertyNamingStrategyClass) {
                builder.propertyNamingStrategy((PropertyNamingStrategy) BeanUtils
                        .instantiateClass(propertyNamingStrategyClass));
            }

            private void configurePropertyNamingStrategyField(
                    Jackson2ObjectMapperBuilder builder, String fieldName) {
                // Find the field (this way we automatically support new constants
                // that may be added by Jackson in the future)
                Field field = ReflectionUtils.findField(PropertyNamingStrategy.class,
                        fieldName, PropertyNamingStrategy.class);
                Assert.notNull(field, () -> "Constant named '" + fieldName
                        + "' not found on " + PropertyNamingStrategy.class.getName());
                try {
                    builder.propertyNamingStrategy(
                            (PropertyNamingStrategy) field.get(null));
                }
                catch (Exception ex) {
                    throw new IllegalStateException(ex);
                }
            }

            private void configureModules(Jackson2ObjectMapperBuilder builder) {
                Collection<Module> moduleBeans = getBeans(this.applicationContext,
                        Module.class);
                builder.modulesToInstall(moduleBeans.toArray(new Module[0]));
            }

            private void configureLocale(Jackson2ObjectMapperBuilder builder) {
                Locale locale = this.jacksonProperties.getLocale();
                if (locale != null) {
                    builder.locale(locale);
                }
            }

            private static <T> Collection<T> getBeans(ListableBeanFactory beanFactory,
                    Class<T> type) {
                return BeanFactoryUtils.beansOfTypeIncludingAncestors(beanFactory, type)
                        .values();
            }
        }
    }
}
```

**配置文件修改**

```csharp
spring:
  jackson:
      serialization:
        write-dates-as-timestamps: true   # date日期转换为时间戳
    date-format: yyyy-MM-dd HH:mm:ss   #指定日期格式，比如yyyy-MM-dd HH:mm:ss，或者具体的格式化类的全限定名
```

**java修改**

```java
@Component
public class MyJackson2ObjectMapperBuilderCustomizer implements Jackson2ObjectMapperBuilderCustomizer, Ordered {
    @Override
    public void customize(Jackson2ObjectMapperBuilder jacksonObjectMapperBuilder) {
       //禁止Date类型转化为时间戳。
       jacksonObjectMapperBuilder.featuresToDisable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
    }

    @Override
    public int getOrder() {
        return 1;
    }
}
```

# 序列化

上文讲述了如何在SpringBoot2.x环境下去修改容器中的ObjectMapper。那么ObjectMapper提供了什么样的配置供开发人员操作？

![img](https:////upload-images.jianshu.io/upload_images/16013479-417b6662d5bc5e92.png?imageMogr2/auto-orient/strip|imageView2/2/w/848/format/webp)

## 1. ObjectMapper的参数配置

### 1.1 针对于ObjectMapper直接配置

`源码：com.fasterxml.jackson.databind.SerializationFeature`该类是一个枚举类型，只有一个`boolean`类型的参数。即开启/禁用该设置。一般我们使用`ObjectMapper objectMapper = new ObjectMapper();`创建出来的`ObjectMapper`对象，实际上包含了`SerializationFeature`类的默认配置。我们若想修改配置，可以使用下面的方法，如代码1所示：

代码1：修改ObjectMapper中的SerializationFeature参数。

```java
//SerializationFeature代表配置；state代表状态
public ObjectMapper configure(SerializationFeature f, boolean state)
//启用SerializationFeature配置
public ObjectMapper enable(SerializationFeature f)
//禁用配置
public ObjectMapper disable(SerializationFeature f)
```

#### 1.2 SpringBoot2.x使用Builder进行配置

我们按照SpringBoot2.x的思路，需要自定义`Jackson2ObjectMapperBuilderCustomizer`子类并加入到IOC容器中。

并且在SpringBoot2.x中，引入了下面的依赖。使用builder创建`ObjectMapper`时注册`objectMapper.registerModule(new JavaTimeModule())`。**解决了JDK1.8的新时间类LocalDateTime的问题。**

```xml
<dependency>
  <groupId>com.fasterxml.jackson.datatype</groupId>
  <artifactId>jackson-datatype-jsr310</artifactId>
</dependency>
```

**核心代码：可以调整ObjectMapper序列化和反序列化特性。**

```kotlin
@Component
public class MyJackson2ObjectMapperBuilderCustomizer implements Jackson2ObjectMapperBuilderCustomizer, Ordered {
    @Override
    public void customize(Jackson2ObjectMapperBuilder jacksonObjectMapperBuilder) {
        //若POJO对象的属性值为null，序列化时不进行显示
        jacksonObjectMapperBuilder.serializationInclusion(JsonInclude.Include.NON_NULL);
        //若POJO对象的属性值为""，序列化时不进行显示
        jacksonObjectMapperBuilder.serializationInclusion(JsonInclude.Include.NON_EMPTY);
        //DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES相当于配置，JSON串含有未知字段时，反序列化依旧可以成功
        jacksonObjectMapperBuilder.failOnUnknownProperties(false);
        //序列化时的命名策略——驼峰命名法
        jacksonObjectMapperBuilder.propertyNamingStrategy(PropertyNamingStrategy.SNAKE_CASE);
        //针对于Date类型，文本格式化
        jacksonObjectMapperBuilder.simpleDateFormat("yyyy-MM-dd HH:mm:ss");

        //针对于JDK新时间类。序列化时带有T的问题，自定义格式化字符串
        JavaTimeModule javaTimeModule = new JavaTimeModule();
        javaTimeModule.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        javaTimeModule.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        jacksonObjectMapperBuilder.modules(javaTimeModule);

//            jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        //默认关闭，将char[]数组序列化为String类型。若开启后序列化为JSON数组。
        jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_CHAR_ARRAYS_AS_JSON_ARRAYS);

        //默认开启，若map的value为null，则不对map条目进行序列化。(已废弃)。
        // 推荐使用：jacksonObjectMapperBuilder.serializationInclusion(JsonInclude.Include.NON_NULL);
        jacksonObjectMapperBuilder.featuresToDisable(SerializationFeature.WRITE_NULL_MAP_VALUES);

        //默认开启，将Date类型序列化为数字时间戳(毫秒表示)。关闭后，序列化为文本表现形式(2019-10-23T01:58:58.308+0000)
        //若设置时间格式化。那么均输出格式化的时间类型。
        jacksonObjectMapperBuilder.featuresToDisable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        //默认关闭，在类上使用@JsonRootName(value="rootNode")注解时是否可以包裹Root元素。
        // (https://blog.csdn.net/blueheart20/article/details/52212221)
//            jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRAP_ROOT_VALUE);
        //默认开启：如果一个类没有public的方法或属性时，会导致序列化失败。关闭后，会得到一个空JSON串。
        jacksonObjectMapperBuilder.featuresToDisable(SerializationFeature.FAIL_ON_EMPTY_BEANS);

        //默认关闭，即以文本(ISO-8601)作为Key，开启后，以时间戳作为Key
        jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS);

        //默认禁用，禁用情况下，需考虑WRITE_ENUMS_USING_TO_STRING配置。启用后，ENUM序列化为数字
        jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_ENUMS_USING_INDEX);

        //仅当WRITE_ENUMS_USING_INDEX为禁用时(默认禁用)，该配置生效
        //默认关闭，枚举类型序列化方式，默认情况下使用Enum.name()。开启后，使用Enum.toString()。注：需重写Enum的toString方法;
        jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_ENUMS_USING_TO_STRING);

        //默认开启，空Collection集合类型输出空JSON串。关闭后取消显示。(已过时)
        // 推荐使用serializationInclusion(JsonInclude.Include.NON_EMPTY);
        jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_EMPTY_JSON_ARRAYS);

        //默认关闭，当集合Collection或数组一个元素时返回："list":["a"]。开启后，"list":"a"
        //需要注意，和DeserializationFeature.ACCEPT_SINGLE_VALUE_AS_ARRAY 配套使用，要么都开启，要么都关闭。
//            jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_SINGLE_ELEM_ARRAYS_UNWRAPPED);

        //默认关闭。打开后BigDecimal序列化为文本。(已弃用)，推荐使用JsonGenerator.Feature.WRITE_BIGDECIMAL_AS_PLAIN配置
//            jacksonObjectMapperBuilder.featuresToEnable(SerializationFeature.WRITE_BIGDECIMAL_AS_PLAIN);
        //默认关闭，即使用BigDecimal.toString()序列化。开启后，使用BigDecimal.toPlainString序列化，不输出科学计数法的值。
        jacksonObjectMapperBuilder.featuresToEnable(JsonGenerator.Feature.WRITE_BIGDECIMAL_AS_PLAIN);

        /**
         * JsonGenerator.Feature的相关参数（JSON生成器）
         */

        //默认关闭，即序列化Number类型及子类为{"amount1":1.1}。开启后，序列化为String类型，即{"amount1":"1.1"}
        jacksonObjectMapperBuilder.featuresToEnable(JsonGenerator.Feature.WRITE_NUMBERS_AS_STRINGS);

        /******
         *  反序列化
         */
        //默认关闭，当JSON字段为""(EMPTY_STRING)时，解析为普通的POJO对象抛出异常。开启后，该POJO的属性值为null。
        jacksonObjectMapperBuilder.featuresToEnable(DeserializationFeature.ACCEPT_EMPTY_STRING_AS_NULL_OBJECT);
        //默认关闭
//            jacksonObjectMapperBuilder.featuresToEnable(DeserializationFeature.ACCEPT_EMPTY_STRING_AS_NULL_OBJECT);
        //默认关闭，若POJO中不含有JSON中的属性，则抛出异常。开启后，不解析该字段，而不会抛出异常。
        jacksonObjectMapperBuilder.featuresToEnable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
    }

    @Override
    public int getOrder() {
        return 1;
    }
}
```

