### SpringCloud  使用

#### 流程：

##### 1.引入依赖  配置文件

```xml
<dependency>
	<groupId>com.alibaba.cloud</groupId>
	<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

```properties
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
  application:
  	name:项目工程名
```

@EnableDiscoveryClient  启动调用

注：

最新版本负载均衡使用loadbalancer 不再使用ribbon  需要导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-loadbalancer</artifactId>
    <version>3.0.2</version>
</dependency>
```

##### 2.使用openFegin

微服务之前调用使用 openFegin

引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

扫描路径

```
@EnableFeignClients(basePackages = "com.czx.shopmall.member.fegin")
```

使用:调用方

```java
@FeignClient("配置文件中的 工程名")
public interface couponFefinInterface {
    @RequestMapping("调用uri")
    public R test();
}
```

#### 3.配置中心

添加依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>

 <!--使用了bootstrap.yml文件之后需要引入bootstrap的依赖-->

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
    <version>3.0.2</version>
</dependency>
```

增加文件 bootstrap.yml  在 resources目录下 配置如下 

```properties
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
  application:
  	name:项目工程名
```

在nacos中配置中心添加配置

获取配置  @RefreshScope

其他配置

```yml
spring:
  cloud:
    nacos:
      config:
        namespace: mall-coupon   
        group: dev
        #  扩展配置 使用线上的配置文件 把配置文件进行拆分 配置data-id时 需配置文件后缀.
        extension-configs:
          - data-id:
            group: dev
            refresh: true 

```

#### 4.网关---参考文档

```yaml
https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/
```

#### 5.跨域问题解决

在网关中增加Filter处理类 添加请求头

```java
@Configuration
public class CorsConfig  {
    @Bean
    public CorsWebFilter corsWebFilter(){
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedMethod("*");
        config.addAllowedHeader("*");
        //高版本使用  允许所有域名访问
        config.addAllowedOriginPattern("*");
        //低版本使用  允许所有域名访问
        config.addAllowedOrigin("*");
        //同意携带cookie跨域
        config.setAllowCredentials(true);
        UrlBasedCorsConfigurationSource  source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**",config);
        return new CorsWebFilter(source);

    }
}
```

# OSS存储

阿里云OSS存储 文档网址：https://help.aliyun.com/document_detail/32006.html

直接使用依赖包不用原生的：

```java
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-oss</artifactId>
    <version>2.1.1.RELEASE</version>
</dependency>
```

OSS浏览器直传文件403：阿里云-->基础设置-->来源-->*



### 数据校验

```java
public  String methodName(@Valid validRequestEntity,BindingResult result){
    //BindingResult 需要紧跟@Valid检验参数
    result.hasErrors()
}
```

### 统一异常处理

```java
@RestControllerAdvice(basePack="包路径")
public class AllExceptionAdvice{
    @ExcetionHandler(value=Exception.class)
    public R exceptionMethod(Exception e ) {
        
    }
}
```



