# 24.外部化配置

##配置随机值

```properties
my.secret = $ {random.value}
 my.number = $ {random.int}
 my.bignumber = $ {random.long}
 my.uuid = $ {random.uuid}
 my.number.less.than.ten = $ {random.int（10）}
 my.number.in.range = $ {random.int [1024,65536]}
```

该`random.int*`语法是`OPEN value (,max) CLOSE`其中的`OPEN,CLOSE`任何字符和`value,max`是整数。如果`max`提供，则`value`是最小值，`max`是最大（独占）。



更换其他名字的配置文件

```shell
$ java -jar myproject.jar --spring.config.name = myproject
```

```shell
$ java -jar myproject.jar --spring.config.location = classpath：/default.properties,classpath：/override.properties
```

## 占位符

```properties
app.name = MyApp
 app.description = $ {app.name}是一个Spring Boot应用程序
```

## YAML

[YAML](http://yaml.org/)是JSON的超集，因此是用于指定分层配置数据的非常方便的格式

### 加载YAML

Spring框架提供了两个方便的类，可以用来加载YAML文档。在`YamlPropertiesFactoryBean`将加载YAML作为`Properties`和 `YamlMapFactoryBean`将加载YAML作为`Map`。

例子:

```yaml
environments:
    dev:
        url: http://dev.bar.com
        name: Developer Setup
    prod:
        url: http://foo.bar.com
        name: My Cool App
```

YAML列表被表示为具有`[index]`解引用的属性键，例如

```yaml
my:
   servers:
       - dev.bar.com
       - foo.bar.com
```

```properties
my.servers[0]=dev.bar.com
my.servers[1]=foo.bar.com
```

使用这些属性时需要使用注解`@ConfigurationProperties`转化为`java.util.list`(或`set`) 

```java
@ConfigurationProperties(prefix="my")
public class Config {

    private List<String> servers = new ArrayList<String>();

    public List<String> getServers() {
        return this.servers;
    }
}
```

> 使用`@Value` 获取YAML配置的属性

数据的宽松绑定

> 标准的驼峰命名
> person-first-name
> person_fist_name
> PERSON_FIRST_NAME

