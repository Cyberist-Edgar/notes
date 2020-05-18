### 官方相关教程

[网址]( https://spring.io/guides )

### [Spring Initializr]( https://start.spring.io/)



使用Spring Initializr可以初始化一个项目骨架，并且是最快的方式，它会帮我们进行大量的配置





### 路由控制

注解

- `@Getmapping`
- `@Postmapping`
- `@RequestMapping(method=GET)`

能够保证HTTP 的 GET请求能够映射到对应的处理函数中



// 能够使得返回的数据是Json格式二点

`@RestController`  <=> `@Control`和 `@ResponseBody`



#### 获取请求信息

- 直接将参数写到形式参数中，适合于 `GET`，但是不适合`POST`

```java
public String hello(String username){
    return "Hello " + username;
}
```



- 通过`HttpServletRequest`接受请求参数，POST和GET均可

  ```java
  public String hello(HttpServletRequest request){
      String username = request.getParameter("username");
      return "Hello " + username;
  }
  ```

  

- 



`@RequestParam` 可以获取GET和POST 的请求信息，但是名称必须对应

```java
 @RequestMapping(value = "/api/getUser", method = RequestMethod.POST)
    public List<User> getUser(@RequestParam(value = "username", required = false) String username){
        System.out.println(username);
        System.out.println(userDao.findAllByUsername(username));
        return userDao.findAllByUsername(username);
    }
```

`@ PathVariable `获取路径中的参数

```java
@RequestMapping(value="/hello/{username}/")
public String hello(@PathVariable String username){
    return "hello + " + username;
}
```



- `RequestBody` 用户接受请求体信息，用于 POST

  - 需要先定义一个类，其中包含这些属性

  ```java
  @RequestMapping(value="/hello")
  public String hello(@RequestBody UserDTO userDTO){
      System.out.println(userDTO.getUsername())
      return o;
  }
  ```

  

<hr/>
<hr/>
### `Bean`

很多组件都是 Bean，比如

```
@Component
@Service
@Repository
@Controller
@Configuration
```

 Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。 

一个`方法`可以被`@Bean`注解，同时类应该被`@Configuration`进行注解，但是返回的结果必须是一个对象，然后在之后的调用之中，可以直接使用该对象, 

```java
@Data
public class ConfigDemoBean {
    private String type = "ConfigDemoBean";

    public String getName(String name) {
        return name + " _" + type;
    }
}

@Configuration
public class BeanLoadConfig {
    @Bean
    public ConfigDemoBean configDemoBean() {
        return new ConfigDemoBean();
    }
}

```



一个`类`可以被`@Component`注解，在之后使用该方法的实例之后可以直接使用对应的方法

```java 
@Component
public class AnoDemoBean {
    private String type = "AnoDemoBean";

    public String getName(String name) {
        return name + " _" + type;
    }
}
```



`@Bean`参数

````
value -- bean别名和name是相互依赖关联的，value,name如果都使用的话值必须要一致
name -- bean名称，如果不写会默认为注解的方法名称
autowire -- 自定装配默认是不开启的，建议尽量不要开启，因为自动装配不能装配基本数据类型、字符串、数组等，这是自动装配设计的局限性，以及自动装配不如显示依赖注入精确
initMethod -- bean的初始化之前的执行方法，该参数一般不怎么用，因为可以完全可以在代码中实现
destroyMethod -- bean销毁执行的方法
````

`@Component`参数

```
value -- 指定名称
```

> 参数有哪些，都可以到对应的class文件中找到
>
> cltr + 左键




#### 使用方式： `依赖注入`

`@Autowired` 

 将注解`@Autowired`或者`@Resource`添加到成员变量上，即表示这个成员变量会由Spring容器注入对应的Bean对象 

```java
@Autowired
private ConfigDemoBean configDemoBean;
```



也可以加到`Setter`方法上

```java
private FacDemoBean facDemoBean;

@Autowired
private void setFacDemoBean(FacDemoBean facDemoBean) {
    this.facDemoBean = facDemoBean;
}
```

也可以放到类的构造方法上

```java
public class DemoController {
    private AnoDemoBean anoDemoBean;
    public DemoController(AnoDemoBean anoDemoBean) {
        this.anoDemoBean = anoDemoBean;
    }
}
```

@Autowired采取的策略为`按照类型注入`。

 

```
public class UserService {
    @Autowired
    private UserDao userDao; 
}
```

如上代码所示，这样装配回去spring容器中找到类型为UserDao的类，然后将其注入进来。这样会产生一个问题，当一个类型有多个bean值的时候，会造成无法选择具体注入哪一个的情况，这个时候我们需要配合着@Qualifier使用。

@Qualifier告诉spring具体去装配哪个对象。

```java
public class UserService {
    @Autowired
    @Qualifier(name="userDao1")    
    private UserDao userDao; 
}
```

这个时候我们就可以通过类型和名称定位到我们想注入的对象。

`@Resource`

@Resource注解由J2EE提供，需要导入包javax.annotation.Resource。

@Resource`默认按照ByName自动注入`。



```java
public class UserService {
    @Resource  
    private UserDao userDao; 
    @Resource(name="studentDao")  
    private StudentDao studentDao; 
    @Resource(type="TeacherDao")  
    private TeacherDao teacherDao; 
    @Resource(name="manDao",type="ManDao")  
    private ManDao manDao; 
}    
```



①如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常。

②如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常。

③如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会抛出异常。

④如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。



<hr/>
<hr/>






### 读取application.properties的数据

首先需要创建一个对象，并且注册为Spring组件和配置属性

```java
@ConfigurationProperties(prefix="test")
@PropertySource("classpath:application.properties")  // 这个指定哪一个properties文件,其他类型不行
@Component
@Data  // lombok中的，可以为类生成setter和getter，toString等方法
public class TestConfig {
    private String name;
    private String school;
}
```

然后在对应的文件中

```properties
test.name=Edgar
test.school=SJTU
```

在使用的时候自动注入即可

```java
@Autowired
private TestConfig testConfig;
```

这样这个对象可以使用test下的数据了

```java
 @Test
    void TestConfigTest(){
        System.out.println(testConfig.getName());
        System.out.println(testConfig.getSchool());
        System.out.println(name);
    }
```



当然也可以直接通过 `@Value`注解将数据直接注入到变量中

```java
@Value("${test.name}")
private String name;  // 也可以这样直接获取属性中的值，但是不太推荐

@Value("hello")  // 或者直接一个文本
private String name;

@Value("#{10*3}")  // 或者表达式
private Interger num;
```



|              | @ConfigurationProperties           | @Value       |
| ------------ | ---------------------------------- | ------------ |
|              | 批量指定                           | 一个一个指定 |
| 松散绑定     | 支持                               | 不支持       |
| SpEL(表达式) | 不支持                             | 支持         |
| 数据校验     | 支持(@Validated，然后@Email之类的) | 不支持       |
| 复杂类型封装 | 支持                               | 不支持       |



如果只是获取配置文件中的某项值，建议使用`@Value`

如果专门写了一个Bean和配置文件进行映射，使用`@ConfigurationProperties`





这个时候，如果在properties设置中文打印出来之后显示乱码，可以在file encoding 中设置为



### 拦截器









### 使用JPA

`pom.xml`

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```



```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/spring?useSSL=false&serverTimezone=CTT
spring.datasource.username=root
spring.datasource.password=Edgar
# 是否打印出 sql 语句
spring.jpa.show-sql=true

spring.jpa.open-in-view=false
# 每次运行的时候数据库执行的方法，create 就意味着每次运行的时候就重新创建
spring.jpa.hibernate.ddl-auto=update
```





定义表

```java
@Entity  // 实体类，为数据库
@Data
@NoArgsConstructor
@AllArgsConstructor
@Table(name = "User")  // 指定数据库的表格
public class User {
    /**
     * 主键
     */
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    @Column(name = "username", columnDefinition = "varchar(30) not null",unique = true)
    private String username;

    @Column(name = "age", columnDefinition = "smallint null")
    private Integer age;

    @Column(name = "password", columnDefinition = "varchar(30) not null")
    private String password;

    @Column(name = "profile", columnDefinition = "varchar(100) null")
    private String profile;

}

```







<hr/>
<hr/>
### 集成swagger-ui

`pom.xml`

```properties
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```



配置文件

```java
package com.edgar.blog.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .pathMapping("/")
                .select()
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
                        .title("Swagger")
                        .description("Swagger")
                        .version("1.0")
                        .contact(new Contact("Edgar","https://blog.csdn.net/weixin_44676081","201648748@qq.com"))
                        .license("The Apache License")
                        .licenseUrl("https://blog.csdn.net/weixin_44676081")
                        .build());
    }
}


```



在需要显示的数据库表前面可以使用

`@ApiModel`



在对应的controller类使用`@Api`, 其对应的方法使用, `@ApiOperation`





### 处理跨越请求

使用`Cors`

- 在`Controller`上使用`@CrossOrigin`

  ```java
  //@CrossOrigin //所有域名均可访问该类下所有接口
  @CrossOrigin("https://blog.csdn.net") // 只有指定域名可以访问该类下所有接口
  public class CorsTest2Controller {
  
      @GetMapping("/sayHello")
      public String sayHello() {
          return "hello";
      }
  }
  ```

- 全局配置

  ```java
  /**
   * 跨域配置
   */
  @Configuration
  public class CorsConfig implements WebMvcConfigurer {
  
      @Bean
      public WebMvcConfigurer corsConfigurer()
      {
          return new WebMvcConfigurer() {
              @Override
              public void addCorsMappings(CorsRegistry registry) {
                  registry.addMapping("/**").
                          allowedOrigins("https://www.dustyblog.cn"). //允许跨域的域名，可以用*表示允许任何域名使用
                          allowedMethods("*"). //允许任何方法（post、get等）
                          allowedHeaders("*"). //允许任何请求头
                          allowCredentials(true). //带上cookie信息
                          exposedHeaders(HttpHeaders.SET_COOKIE).maxAge(3600L); //maxAge(3600)表明在3600秒内，不需要再发送预检验请求，可以缓存该结果
              }
          };
      }
  }
  ```

  

### 