# 27 开发web应用

您可以使用嵌入式Tomcat，Jetty或Undertow轻松创建自包含的HTTP服务器。大多数Web应用程序将使用`spring-boot-starter-web`模块快速启动并运行。

## Spring Web MVC framework

Spring MVC允许您创建特殊`@Controller` 或`@RestController`bean来处理传入的HTTP请求。您的控制器中的方法使用`@RequestMapping`注释映射到HTTP 。

这是一个典型的例子`@RestController`来提供JSON数据：

```java
@RestController 
@RequestMapping（value =“/ users”）
 public  class MyRestController {
    @RequestMapping（value =“/ {user}”，method = RequestMethod.GET）
     public User getUser（ @PathVariable Long user）{
         // ...
    }

    @RequestMapping（value =“/ {user} / customers”，method = RequestMethod.GET） 
    List <Customer> getUserCustomers（ @PathVariable Long user）{
         // ...
    }

    @RequestMapping（value =“/ {user}”，method = RequestMethod.DELETE）
     public User deleteUser（ @PathVariable Long user）{
         // ...
    }
}	
```

### Spring MVC自动配置

自动配置在Spring的默认设置之上添加以下功能：

- 包括`ContentNegotiatingViewResolver`和`BeanNameViewResolver` bean类。
- 支持静态资源，包括对WebJars的支持（见下文）。
- 自动登记`Converter`，`GenericConverter`，`Formatter`bean类。
- 支持`HttpMessageConverters`（见下文）。
- 自动注册`MessageCodesResolver`（见下文）。
- 静态`index.html`支持。
- 自定义`Favicon`支持（见下文）。
- 自动使用一个`ConfigurableWebBindingInitializer`bean（见下文）。

### HttpMessageConverters

Spring MVC使用该`HttpMessageConverter`接口来转换HTTP请求和响应。

如果你需要添加或定制转换器，你可以使用Spring Boot的`HttpMessageConverters`类：

```java
import org.springframework.boot.autoconfigure.web.HttpMessageConverters;
import org.springframework.context.annotation.*;
import org.springframework.http.converter.*;

@Configuration
public class MyConfiguration {

    @Bean
    public HttpMessageConverters customConverters() {
        HttpMessageConverter<?> additional = ...
        HttpMessageConverter<?> another = ...
        return new HttpMessageConverters(additional, another);
    }

}
```

### 静态内容

默认情况下，Spring Boot将从类路径或者根目录下的`/static`（ `/public`或者`/resources`或者`/META-INF/resources`）目录中提供静态内容`ServletContext`。它使用Spring MVC的`ResourceHttpRequestHandler`，所以你可以通过添加你自己`WebMvcConfigurerAdapter`的 `addResourceHandlers`方法来修改这个行为。