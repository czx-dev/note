### spring-boot-starter是什么：

#### 		简化配置

### 自定义spring-boot-starter

##### 1.删除启动类，test

##### 2.引入依赖

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

##### 3.@ConfigurationProperties(prefix = '')创建配置类 读取参数

##### 4.生成配置类

```java
@Configuration
@EnableConfigurationProperties(TianProperties.class)# 启动注解
@ConditionalOnClass(TianService.class)#这个类被实例时才实例被修饰类
@ConditionalOnProperty(prefix = "spring.tian", value = "enabled", matchIfMissing = true,havingValue ="")
#配置文件 注解前缀prefix 属性value matchIfMissing 为true 只要有值即可  havingValue 为特定值才生效
public class TianServiceAutoConfiguration {

    @Autowired
    private TianProperties properties;

    @Bean
    @ConditionalOnMissingBean(TianService.class)#这个类不存在时,调用例被修饰类
    public TianService tianService() {
        return new TianService(properties);
    }
}
```

##### 5.resource 创建META-INF  创建spring.factories文件 

```java
#spring.factories文件 
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.demo.a.TianServiceAutoConfiguration
```

##### 6.生成普通jar 包

```java
# 默认生成可执行jar包 需要有启动类
mvn clean install package -D spring-boot.repackage.skip=true 
```

##### 7.其他项目引入

###### 	1.值与自定义starter pom文件中相同

```java
        <dependency>
            <groupId>com.demo</groupId>
            <artifactId>my-spring-boot-starter</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```



