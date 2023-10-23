# Spring Security

## 前言

### RBAC

`RBAC`是基于角色的访问控制(Role-Based Access Controll)的缩写，在`RBAC`中，权限与角色相关联，用户通过成为适当角色的成员而得到这些角色的权限。这就极大地简化了权限的管理。

### 权限框架

- `Shiro`
- `Spring Security`

两者的区别：

1、Spring Security 基于Spring 开发，项目若使用 Spring 作为基础，配合 Spring Security 做权限更加方便，而 Shiro 需要和 Spring 进行整合开发；

2、Spring Security 功能比 Shiro 更加丰富些，例如安全维护方面；

3、Spring Security 社区资源相对比 Shiro 更加丰富；

4、Shiro 的配置和使用比较简单，Spring Security 上手复杂些；

5、Shiro 依赖性低，不需要任何框架和容器，可以独立运行.Spring Security 依赖Spring容器；

6、shiro 不仅仅可以使用在web中，它可以工作在任何应用环境中。在集群会话时Shiro最重要的一个好处或许就是它的会话是独立于容器的。

## 简介

Spring Security是一个能够为基于Spring的企业应用系统提供声明式的安全访问控制解决方案的安全框架。它提供了一组可以在Spring应用上下文中配置的Bean，充分利用了Spring IoC，DI（控制反转Inversion of Control ,DI:Dependency Injection 依赖注入）和AOP（面向切面编程）功能，为应用系统提供声明式的安全访问控制功能，减少了为企业系统安全控制编写大量重复代码的工作。

## 基本原理

SpringSecurity 本质是一个过滤器链。

SpringSecurity 采用的是责任链的设计模式，它有一条很长的过滤器链。现在对这条过滤器链的各个进行说明:

1） WebAsyncManagerIntegrationFilter：将 Security 上下文与 Spring Web 中用于处理异步请求映射的 WebAsyncManager 进行集成。

2） SecurityContextPersistenceFilter：在每次请求处理之前将该请求相关的安全上下文信息加载到 SecurityContextHolder 中，然后在该次请求处理完成之后，将 SecurityContextHolder 中关于这次请求的信息存储到一个“仓储”中，然后将 SecurityContextHolder 中的信息清除，例如在Session中维护一个用户的安全信息就是这个过滤器处理的。

3） HeaderWriterFilter：用于将头信息加入响应中。

4） CsrfFilter：用于处理跨站请求伪造处理。

5） LogoutFilter：用于处理退出登录。

6） UsernamePasswordAuthenticationFilter：用于处理基于表单的登录请求，从表单中获取用户名和密码。默认情况下处理来自 /login 的请求。从表单中获取用户名和密码时，默认使用的表单 name 值为 username 和 password，这两个值可以通过设置这个过滤器的usernameParameter 和 passwordParameter 两个参数的值进行修改。

7） DefaultLoginPageGeneratingFilter：如果没有配置登录页面，那系统初始化时就会配置这个过滤器，并且用于在需要进行登录时生成一个登录表单页面。

8） BasicAuthenticationFilter：检测和处理 http basic 认证。

9） RequestCacheAwareFilter：用来处理请求的缓存。

10） SecurityContextHolderAwareRequestFilter：主要是包装请求对象request。

11） AnonymousAuthenticationFilter：检测 SecurityContextHolder 中是否存在 Authentication 对象，如果不存在为其提供一个匿名 Authentication。

12） SessionManagementFilter：管理 session 的过滤器

13） ExceptionTranslationFilter：处理 AccessDeniedException 和 AuthenticationException 异常。

14） FilterSecurityInterceptor：可以看做过滤器链的出口。

15） RememberMeAuthenticationFilter：当用户没有登录而直接访问资源时, 从 cookie 里找出用户的信息, 如果 Spring Security 能够识别出用户提供的remember me cookie, 用户将不必填写用户名和密码, 而是直接登录进入系统，该过滤器默认不开启。

## 运行流程

![](pic\1lc.png)



## 使用

创建springboot项目

添加依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

直接运行项目，在浏览器中访问，会跳转到登录页面。

![](pic\2login.png)

此页面是Spring Security提供的内置登录页面。
默认用户名：user
密码是随机生成的，在项目启动时，可以在控制台日志中看到。

输入账号和密码后，即可登录成功。

### 配置类

配置类用于配置Spring Security的各项功能。

继承自WebSecurityConfigurerAdapter类，重写configure方法进行配置。

1） openldLogin() 用于基于Openld的脸证
2） **headers()** 将安全标头添加到响应
3） cors() 配置跨域资源共享( CORS )
4） sessionManagement() 允许配置会话管理
5） portMapper() 允许配置一个PortMapper(HttpSecurity#(getSharedObject(class)))，其他提供SecurityConfigurer的对象使用 PortMapper 从 HTTP 重定向到 HTTPS 或者从 HTTPS 重定向到 HTTP。默认情况下，Spring Security使用一个PortMapperImpl映射 HTTP 端口8080到 HTTPS 端口8443，HTTP 端口80到 HTTPS 端口443
6） jee() 配置基于容器的预认证。 在这种情况下，认证由Servlet容器管理
7） x509() 配置基于x509的认证
8） **rememberMe()** 允许配置“记住我”的验证
9） **authorizeRequests()** 允许基于使用HttpServletRequest限制访问
10） requestCache() 允许配置请求缓存
11） **exceptionHandling()** 允许配置错误处理
12） securityContext() 在HttpServletRequests之间的SecurityContextHolder上设置SecurityContext的管理。 当使用WebSecurityConfigurerAdapter时，这将自动应用
13） servletApi() 将HttpServletRequest方法与在其上找到的值集成到SecurityContext中。 当使用WebSecurityConfigurerAdapter时，这将自动应用
14） **csrf()** 添加 CSRF 支持，使用WebSecurityConfigurerAdapter时，默认启用
15） **logout()** 添加退出登录支持。当使用WebSecurityConfigurerAdapter时，这将自动应用。默认情况是，访问URL”/ logout”，使HTTP Session无效来清除用户，清除已配置的任何#rememberMe()身份验证，清除SecurityContextHolder，然后重定向到”/login?success”
16） anonymous() 允许配置匿名用户的表示方法。 当与WebSecurityConfigurerAdapter结合使用时，这将自动应用。 默认情况下，匿名用户将使用org.springframework.security.authentication.AnonymousAuthenticationToken表示，并包含角色 “ROLE_ANONYMOUS”。
17） **formLogin()** 指定支持基于表单的身份验证。如果未指定。FormLoginConfigurer#loginPage(String)，则将生成默认登录页面。
18） oauth2Login() 根据外部OAuth 2.0或OpenID Connect 1.0提供程序配置身份验证。
19） requiresChannel() 配置通道安全。为了使该配置有用，必须提供至少一个到所需信道的映射。
20） httpBasic() 配置 Http Basic 验证。
21） addFilterBefore() 在指定的Filter类的位置前添加过滤器。

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    // 创建 BCryptPasswordEncoder 注入容器  指定密码使用哪种加密算法
    @Bean
    public BCryptPasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Autowired
    private TokenFilter tokenFilter;

    @Autowired
    private AuthenticationEntryPointImpl authenticationEntryPoint;

    @Autowired
    private AccessDeniedHandlerImpl accessDeniedHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                // 设置不会创建一个session对象  Spring Security will never create an HttpSession and it will never use it to obtain the SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/login/v1/login").permitAll()
                .antMatchers("/captcha/get").permitAll()
                .antMatchers("/ding/dingLogin").permitAll()//钉钉
                .antMatchers("/login/v1/loginNcToDing").permitAll()//健康驿站
                .antMatchers("/captcha/check").permitAll()
                //app
                .antMatchers("/app/query").permitAll()
                .antMatchers("/getAppFile").permitAll()
                //swagger
                .antMatchers("/v2/api-docs").permitAll()
                .antMatchers("/favicon.ico").permitAll()
                .antMatchers("/swagger-ui.html").permitAll()
                .antMatchers("/swagger-resources/**").permitAll()
                .antMatchers("/webjars/**").permitAll()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();

        // 添加过滤器
        // 把token校验过滤器添加到过滤器链中  把token过滤器添加到 验证用户名和密码的前面
        http.addFilterBefore(tokenFilter, UsernamePasswordAuthenticationFilter.class);

        //  配置异常处理器
        http.exceptionHandling().authenticationEntryPoint(authenticationEntryPoint)
                .accessDeniedHandler(accessDeniedHandler);

        // 允许跨域
        http.cors();
    }

    @Bean  // 必须要加@Bean 注解才能从容器中获取到
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```

### 用户配置

#### 使用配置文件

在项目的配置文件中添加账号和密码的配置信息。

```yml
spring:
  security:
    user:
      name: tom # 账号
      password: 123 # 密码
```

#### 使用配置类

在配置类中添加如下代码。

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //
        auth.inMemoryAuthentication()
                // 设置用户名
                .withUser("jerry")
                // 设置密码（密文密码）
                .password(new BCryptPasswordEncoder().encode("123"))
                // 设置角色，不设置启动不了
                .roles("");
    }

    @Bean
    public PasswordEncoder passwordEncoder(){
        // 定义加密方式
        return new BCryptPasswordEncoder();
    }
}
```

此方法会覆盖掉在配置文件中配置的账号信息。

#### 使用数据库

1） 用户实体类实现UserDetails接口

```java
public class User implements UserDetails {

    private String username;    // 用户名
    private String password;    // 密码

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        List<SimpleGrantedAuthority> authorities = new ArrayList<>();
        // 配置用户的角色及权限
        authorities.add(new SimpleGrantedAuthority("角色名"));
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        // 是未过期的用户
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        // 是未锁定的用户
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        // 是未过期的凭证
        return true;
    }

    @Override
    public boolean isEnabled() {
        // 是启用的用户
        return true;
    }
}
```

2） 实现UserDetailsService接口

```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) throws AuthenticationException{
        // 根据用户名，获取用户信息，查询数据库
        User user = userMapper.selectByUsername(username);
        if (user == null) {
            throw new BadCredentialsException("用户名不存在！！！");
        }
        return user;
    }
}
```

### 路径配置

authorizeRequests()方法返回URL配置对象，在此对象上可以对URL的权限进行配置。

配置URL：

1） anyRequest() 任何请求。

2） antMatchers() 指定路径。

配置认证方式：

1） authenticated() 保护URL，需要用户登录。

2） permitAll() 指定URL无需保护，一般应用与静态资源文件。

3） hasRole(String role) 限制单个角色访问，角色将被增加 “ROLE_” .所以”ADMIN” 将和 “ROLE_ADMIN”进行比较。

4） hasAuthority(String authority) 限制单个权限访问。

5） hasAnyRole(String… roles)允许多个角色访问。

6） hasAnyAuthority(String… authorities) 允许多个权限访问。

7） access(String attribute) 该方法使用 SpEL表达式, 所以可以创建复杂的限制。

8） hasIpAddress(String ipaddressExpression) 限制IP地址或子网。

例如：

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest()                      // 所有请求
                .authenticated()                   // 需要登录

                .antMatchers("/admin")             // /admin路径
                .hasRole("admin")                  // 需要ROLE_admin角色

                .antMatchers("/user/**")           // /user/路径下的所有资源
                .hasAnyRole("admin", "user")       // 需要ROLE_admin或ROLE_user角色

                .antMatchers("/test", "/api/test") // /test路径和/api/test路径
                .permitAll();                      // 无需登录
    }
}
```

### 登录退出

#### 自定义登录退出

```java
http
    .formLogin()                                      //表单登陆 
    .loginPage("/user/login.html").permitAll()        // 设置登录页面
    .loginProcessingUrl("/user/login").permitAll()    // 设置登录提交路径
    .successHandler(successHandler)                   // 成功时的跳转
    .failureHandler(failureHandler)                   // 失败时的跳转
    .and()
    .logout()                                         // 退出操作
    .logoutRequestMatcher(new AntPathRequestMatcher("/user/logout"))  // 退出路径
    .logoutSuccessUrl("/user/login.html");            // 退出成功跳转
```

#### 记住登录

```
http.
    .rememberMe()
    .userDetailsService(userDetailsService);
```

开启自动登录时，会添加一个`remember-me`的参数。

```html
<input type="checkbox" name="remember-me"> Remember me on this computer.
```

#### 关闭跨域防御

```java
// 关闭 跨域防御
http.csrf().disable();
```

## 注解

### @EnableGlobalMethodSecurity

用于开启Spring方法级注解。

需要在配置类前添加此注解。

该注解提供了prePostEnabled 、securedEnabled 和 jsr250Enabled 三种不同的机制来实现注解式开发。

#### prePostEnabled

prePostEnabled = true 会解锁@PreAuthorize 和 @PostAuthorize 两个注解。

@PreAuthorize 注解会在方法执行前进行验证。
@PostAuthorize 注解会在方法执行后进行验证。

```java
public interface UserService {
    List<User> findAllUsers();

    @PostAuthorize ("returnObject.type == authentication.name")
    User findById(int id);
    
    //  @PreAuthorize("hasRole('ADMIN')") 必须拥有 ROLE_ADMIN 角色。
    @PreAuthorize("hasRole('ROLE_ADMIN')")
    void updateUser(User user);
    
    @PreAuthorize("hasRole('ADMIN') AND hasRole('DBA')")
    void deleteUser(int id);
    
    // @PreAuthorize("principal.username.startsWith('Felordcn')") 用户名开头为 Felordcn 的用户才能访问。
    // @PreAuthorize("#id.equals(principal.username)") 入参 id 必须同当前的用户名相同。
    // @PreAuthorize("#id < 10") 限制只能查询 id 小于 10 的用户

}
```

内置表达式

![](pic\3nz.png)

**@PreFilter和@PostFilter注解**

对集合类型的参数执行过滤，移除结果为false的元素。
基于方法入参相关的表达式，对入参进行过滤。分页慎用！该过程发生在接口接收参数之前。
入参必须为 java.util.Collection 且支持 remove(Object) 的参数。
如果有多个集合需要通过 filterTarget=<参数名> 来指定过滤的集合。
内置保留名称 filterObject 作为集合元素的操作名来进行评估过滤。

```java
// 指定过滤的参数，过滤偶数
@PreFilter(filterTarget="ids", value="filterObject%2==0")
public void delete(List ids, List username)
```

#### Secured

@Secured注解是用来定义业务方法的安全配置。在需要安全[角色/权限等]的方法上指定 @Secured，并且只有那些角色/权限的用户才可以调用该方法。
@Secured缺点（限制）就是不支持Spring EL表达式。不够灵活。并且指定的角色必须以ROLE_开头，不可省略。该注解功能要简单的多，默认情况下只能基于角色（默认需要带前缀 ROLE_）集合来进行访问控制决策。该注解的机制是只要其声明的角色集合（value）中包含当前用户持有的任一角色就可以访问。也就是 用户的角色集合和 @Secured 注解的角色集合要存在非空的交集。 不支持使用 SpEL 表达式进行决策。

```java
@Secured({"ROLE_user"})
void updateUser(User user);

@Secured({"ROLE_admin", "ROLE_user1"})
void updateUser();
```

#### jsr250E

启用 JSR-250 安全控制注解，这属于 JavaEE 的安全规范（现为 jakarta 项目）。一共有五个安全注解。如果你在 @EnableGlobalMethodSecurity 设置 jsr250Enabled 为 true ，就开启了 JavaEE 安全注解中的以下三个：

1.@DenyAll： 拒绝所有访问

2.@RolesAllowed({“USER”, “ADMIN”})： 该方法只要具有"USER", "ADMIN"任意一种权限就可以访问。这里可以省略前缀ROLE_，实际的权限可能是ROLE_ADMIN

3.@PermitAll： 允许所有访问