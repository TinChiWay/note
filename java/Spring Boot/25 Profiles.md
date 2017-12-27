# Profiles

Spring Profiles提供了一种分离部分应用程序配置的方法，并使其仅在某些环境中可用。任何`@Component`或`@Configuration`可以标记`@Profile`以限制何时加载：

```java
@Configuration 
@Profile（“production”）
 public  class ProductionConfiguration {
    // ...
}
```

# Logging(日志)

Spring Boot为[Java Util Logging](http://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html)，[Log4J2](https://logging.apache.org/log4j/2.x/)和[Logback](http://logback.qos.ch/)提供了默认配置 。默认情况下，如果使用“Starter”，Logback将用于日志记录。

> Java有很多可用的日志框架。如果上面的列表看起来很混乱，请不要担心。一般来说，你不需要改变你的日志依赖性，Spring Boot的默认设置就可以正常工作。

> Logback没有一个`FATAL`级别（它被映射到`ERROR`）

默认日志配置会在写入消息时将消息回送到控制台。默认情况下`ERROR`，`WARN`和`INFO`级别的消息被记录。你也可以通过启动一个`--debug`标志来启动你的应用程序来启用“调试”模式。
```shell
$ java -jar myapp.jar --debug
```

> 你也可以`debug=true`在你的指定`application.properties`。

通过使用`--trace` (或在配置里设置`trace=true`)对核心记录器（嵌入式容器，Hibernate模式生成和整个Spring产品组合）选择的跟踪记录。

## 写入日志文件

默认情况下，Spring Boot将只能登录到控制台，不会写入日志文件。如果除了输出控制台之外还想写日志文件，你需要设置一个`logging.file`或者一个 `logging.path`属性（比如你的`application.properties`）。

> 日志记录属性独立于实际的日志记录基础结构。因此，特定的配置键（如`logback.configurationFile`Logback）不受Spring Boot的管理。

