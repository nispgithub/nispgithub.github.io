# swagger

## 导入依赖

pom文件添加`Swagger2`依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

## 配置`Swagger2`

编写`Swagger2`配置类

```java
@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean//配置docket以配置Swagger具体参数
    public Docket groupRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(groupApiInfo())
            	.enable(false) //禁用Swagger,配置是否启用Swagger，如果是false，在浏览器将无法访问
                .select()
                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo groupApiInfo() {
        Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
   		 return new ApiInfoBuilder()
                .title( "Swagger学习")
                .description("学习演示如何配置Swagger")
                .termsOfServiceUrl("http://terms.service.url/组织链接")
                .contact(contact)
                .version("Apach 2.0 许可")
                .build();
    }
}
```

## 修改yml文件

spring下添加

```yml
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```

修改配置文件

SecurityConfig放行swagger相关资源

```java
 //swagger
            .antMatchers("/v2/api-docs").permitAll()
            .antMatchers("/favicon.ico").permitAll()
            .antMatchers("/swagger-ui.html").permitAll()
            .antMatchers("/swagger-resources/**").permitAll()
            .antMatchers("/webjars/**").permitAll()
```

重启项目，访问测试`http://localhost:9999/swagger-ui.html`

## 常见注解

> `Swagger`的所有注解定义在`io.swagger.annotations`包下
>
> 下面列一些经常用到的，未列举出来的可以另行查阅说明：

| Swagger注解                                              | 说明                                                     |
| -------------------------------------------------------- | -------------------------------------------------------- |
| `@Api(tags = "xxx模块说明")`                             | 描述`Controller`的作用                                   |
| `@ApiOperation("xxx接口说明")`                           | 描述一个类中的一个方法                                   |
| `@ApiModel("xxxPOJO说明")`                               | 作用在模型类上：如VO、BO                                 |
| `@ApiModelProperty(value = "xxx属性说明",hidden = true)` | 作用在类方法和属性上，`hidden`设置为`true`可以隐藏该属性 |
| `@ApiImplicitParams()和@ApiParam("xxx参数说明")`         | 作用在参数、方法和字段上，类似`@ApiModelProperty`        |

```java
@ApiOperation("用户登录接口")
@ApiImplicitParams({
    @ApiImplicitParam(dataType = "string",name = "username", value = "用户登录账号",required = true),
    @ApiImplicitParam(dataType = "string",name = "password", value = "用户登录密码",required = false,defaultValue = "111111")
})
@RequestMapping(value = "/login",method = RequestMethod.GET)
public String login(@RequestParam("username") String name,
                      @RequestParam(value = "password",defaultValue = "111111") String pwd){
    return "";
}
```

