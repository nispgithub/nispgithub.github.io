## Jwt简介和结构

### Jwt简介

> 官网地址: `https://jwt.io/introduction/`
>
> `Jwt(json web token)`是一个开放标准(rfc7519)，它定义了一种紧凑的、自包含的方式，用于在各方之间以`JSON`对象安全地传输信息。此信息可以验证和信任，因为它是数字签名的。`Jwt`可以使用密钥(使用HMAC算法)或使用RSA或ECDSA的公钥/私钥对进行签名。
>
> 通俗解释：`Jwt`简称`JSON Web Token`,也就是通过`JSON`形式作为`Web`应用中的令牌，用于在各方之间安全地将信息作为`JSON`对象传输。在数据传输过程中还可以完成数据加密、签名等相关处理。

### Jwt能做什么

> **认证**
>
> 这是使用`Jwt`的最常见方案。一旦用户登录，每个后续请求将包括`Jwt`，从而允许用户访问该令牌允许的路由，服务和资源。单点登录是当今广泛使用`Jwt`的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。
>
> **信息交换**
>
> 在各方之间安全地传输信息的好方法，因为可以对`Jwt`进行签名（例如，使用公钥/私钥对），所以您可以确保发件人是他们所有人，由于签名是使用标头和有效负载计算的，因此您还可以验证内容是否遭到篡改。

### Jwt结构

> ```
> header.payload.singnature
> ```
>
> - 标头`Header`
> - 有效载荷`Payload`
> - 签名`Signature`

#### Header

> 标头通常由两部分组成：令牌的类型（即`JWT`）和所使用的签名算法，例如`HMAC`、 `SHA256`或`RSA`。它会使用`Base64`编码组成`Jwt`结构的第一部分。
>
> `Base64`是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。
>
> ```json
> {
>  "alg": "HS256",
>  "typ": "JWT"
> }
> ```

#### Payload

> 令牌的第二部分是有效负载，其中包含声明。声明是有关实体（通常是用户）和其他数据的声明。同样的，它会使用`Base64`编码组成`Jwt`结构的第二部分。
>
> ```json
> {
>   "sub": "1234567890",
>   "name": "John Doe",
>   "admin": true
> }
> ```

#### Signature

##### 是什么

> 前面两部分都是使用`Base64`进行编码的，即前端可以解开知道里面的信息。`Signature`需要使用编码后的`header`和`payload`以及我们提供的一个密钥，然后使用`header`中指定的签名算法（例如HS256）进行签名。签名的作用是保证`Jwt`没有被篡改过。

##### 签名目的

> 最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的`Jwt`的话，那么服务器端会判断出新的头部和负载形成的签名和`Jwt`附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

##### 信息安全问题

> 在这里大家一定会问一个问题：`Base64`是一种编码，是可逆的，那么我的信息不就被暴露了吗？
>
> 是的。所以，在`Jwt`中，**不应该在负载里面加入任何敏感的数据**。在上面的例子中，我们传输的是用户的名字。这个值实际上不是什么敏感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在`Jwt`中了。如果将用户的密码放在了`Jwt`中，那么怀有恶意的第三方通过`Base64`解码就能很快地知道你的密码了。因此`Jwt`适合用于向`Web`应用传递一些非敏感信息。`JWT`还经常用于设计用户认证和授权系 统，甚至实现`Web`应用的单点登录。

### Jwt优势

> - 简洁(Compact)：可以通过`URL`，`POST`参数或者在`HTTP header`发送，因为数据量小，传输速度也很快；
> - 自包含(Self-contained)：负载中包含了所有用户所需要的信息，避免了多次查询数据库；
> - 因为`Token`是以`JSON`加密的形式保存在客户端的，所以`Jwt`是跨语言的，原则上任何`Web`形式都支持。
> - 不需要在服务端保存会话信息，特别适用于分布式微服务。

## 前后端分离认证

![](pic\3jwt.png)

1. 前端通过`Web`表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个`HTTP POST`请求。建议的方式是通过`SSL`加密的传输（`https`协议），从而避免敏感信息被嗅探；
2. 后端核对用户名和密码成功后，根据特定规则生成`Jwt`；
3. 后端将`Jwt`字符串作为登录成功的返回结果返回给前端。前端可以将返回的结果保存在`localStorage`或`sessionStorage`上，退出登录时前端删除保存的`Jwt`即可；
4. 后续前端每次请求时将`Jwt`放入`HTTP Header`中；
5. 后端检查是否存在，如存在验证`Jwt`的有效性；
6. 验证通过后后端使用`Jwt`中包含的用户信息进行其他逻辑操作，返回相应结果。

## 使用

添加依赖

```xml
 <!-- Token生成与解析-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

添加工具类

```java
package com.situ.rbac.util;

import com.situ.rbac.entity.User;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.ExpiredJwtException;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;

public class JwtUtil {
    /**
     * 令牌前缀
     */
    public static final String TOKEN_PREFIX = "Bearer";
    /**
     * 令牌自定义标识
     */
    private static final String TOKEN_HEADER = "Authorization-Token";

    /**
     * 令牌秘钥
     */
    private static final String TOKEN_SECRET = "secret";

    /**
     * 令牌有效期（默认30分钟）
     */
    private static final long EXPIRATION = 1800L;
    protected static final long MILLIS_SECOND = 1000;


    /**
     * 从数据声明生成令牌
     *
     * @param claims 数据声明
     * @return 令牌
     */
    private static String createToken(Map<String, Object> claims) {
        return TOKEN_PREFIX +  Jwts.builder()
                .setClaims(claims)
                .signWith(SignatureAlgorithm.HS512, TOKEN_SECRET)
                .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION * MILLIS_SECOND))
                .compact();
    }

    /**
     * 从令牌中获取数据声明
     *
     * @param token 令牌
     * @return 数据声明
     */
    public static Claims parseToken(String token) {
        return Jwts.parser()
                .setSigningKey(TOKEN_SECRET)
                .parseClaimsJws(token.substring(TOKEN_PREFIX.length()))
                .getBody();
    }

    /**
     * 创建令牌
     *
     * @param User 用户信息
     * @return 令牌
     */
    public static String createToken(User User) {
        String token = User.getUsername();
        Map<String,Object> claims = new HashMap<>();
        claims.put("login", token);
        return createToken(claims);
    }

    /**
     * 验证令牌是否过期
     *
     * @param token 令牌
     * @return
     */
    public static boolean verifyToken(String token) {
        try {
            return parseToken(token).getExpiration().before(new Date());
        }catch (ExpiredJwtException e){
            return true;
        }
    }

}


```

登录接口

返回token

```java
package com.situ.rbac.controller;

import com.situ.rbac.common.ResponseBean;
import com.situ.rbac.entity.User;
import com.situ.rbac.service.IUserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RestController;

/**
 * <p>
 * 用户表 前端控制器
 * </p>
 *
 * @author admin
 * @since 2023-10-23
 */
@RestController
@RequestMapping("/rbac/user")
public class UserController {
    @Autowired
    private IUserService userService;

    @PostMapping("/login")
    public ResponseBean login(User user){
        try {
            //service业务处理
            return ResponseBean.ok(userService.login(user));
        }catch (Exception e){
            System.out.println("UserController--login:" + e.getMessage());
            return ResponseBean.failed(e.getMessage());
        }
    }

}

```

```java
package com.situ.rbac.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.situ.rbac.entity.*;
import com.situ.rbac.mapper.*;
import com.situ.rbac.service.IUserService;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.situ.rbac.util.JwtUtil;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;

/**
 * <p>
 * 用户表 服务实现类
 * </p>
 *
 * @author admin
 * @since 2023-10-23
 */
@Service
@RequiredArgsConstructor//构造器注入
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements IUserService {

    private final AuthenticationManager authenticationManager;

    @Override
    public String login(User user) {

        // 用户验证
        Authentication authentication = null;
        try {
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(
                    user.getUsername(), user.getPassword());
            // 该方法会去调用UserDetailsServiceImpl.loadUserByUsername
            authentication = authenticationManager.authenticate(authenticationToken);
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage());
        }
        User loginUser = (User) authentication.getPrincipal();
        // 生成token
        return JwtUtil.createToken(loginUser);
    }
}

```

SecurityConfig

注入AuthenticationManager

放开登录接口

```java
package com.situ.rbac.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig {

//    @Bean
//    public WebSecurityCustomizer webSecurityCustomizer() {
//             return (web) -> web.ignoring().antMatchers("/images/**", "/js/**","/css/**",  "/webjars/**");
//    }
    @Bean
    public AuthenticationManager authManager(HttpSecurity http, PasswordEncoder bCryptPasswordEncoder, UserDetailsService userDetailsService)
            throws Exception {
        return http.getSharedObject(AuthenticationManagerBuilder.class)
                .userDetailsService(userDetailsService)
                .passwordEncoder(bCryptPasswordEncoder)
                .and()
                .build();
    }
    // 创建 BCryptPasswordEncoder 注入容器  指定密码使用哪种加密算法
    @Bean
    public BCryptPasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Bean
     public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
//        http.formLogin()//指定支持基于表单的身份验证
//            .and()
        http.authorizeRequests()//允许基于使用HttpServletRequest限制访问
//            .antMatchers("/admin")
//            .hasRole("ADMIN")//限制单个角色访问，角色将被增加 “ROLE_” .所以”ADMIN” 将和 “ROLE_ADMIN”进行比较
//            .antMatchers("/user")
//            .hasAnyRole("ADMIN","USER")//多个
            //swagger
            .antMatchers("/v2/api-docs").permitAll()
            .antMatchers("/favicon.ico").permitAll()
            .antMatchers("/swagger-ui.html").permitAll()
            .antMatchers("/swagger-resources/**").permitAll()
            .antMatchers("/webjars/**").permitAll()
            .antMatchers("/test/**").permitAll()
            .antMatchers("/rbac/user/login").anonymous()//登录接口
            .anyRequest()     // 所有请求
            .authenticated();  // 需要登录

        // 允许跨域
        http.cors().configurationSource(corsConfigurationSource()).and().csrf().disable();
        return http.build();
    }

    public CorsConfigurationSource corsConfigurationSource() {
        //跨域配置
        CorsConfiguration configuration = new CorsConfiguration();
        //允许访问的客户端域名
        configuration.addAllowedOrigin("*");
        //允许访问的客户端请求头
        configuration.addAllowedHeader("*");
        //允许访问的客户端请求方式，get post
        configuration.addAllowedMethod("*");
        //暴露那些头部信息
        configuration.addExposedHeader("*");
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();

        source.registerCorsConfiguration("/**", configuration);

        return source;

    }

}

```

添加登录过滤器

```java
package com.situ.rbac.config.filter;

import com.situ.rbac.entity.User;
import com.situ.rbac.service.IUserLoginService;
import com.situ.rbac.service.impl.UserLoginServiceImpl;
import com.situ.rbac.util.JwtUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.annotation.Resource;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Date;
import java.util.Map;

@Component
public class TokenFilter extends OncePerRequestFilter {
    
    public static final String AUTHORIZATION_TOKEN = "Authorization-Token";
    @Autowired
    private UserLoginServiceImpl userLoginService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        // 获取token
        String token = request.getHeader(AUTHORIZATION_TOKEN);
        if (!StringUtils.hasText(token) || "null".equals(token)){
         // 放行  后面还有别的过滤器进行验证 所以这里如果请求头里没有 直接放行就行了
         filterChain.doFilter(request,response);
         return;
        }
        //解析token
        String name;
        try {
             name = JwtUtil.parseToken(token).get("login").toString();
        }catch (Exception e){
            throw new RuntimeException("无效的token");
        }
        //判断是否超时
        if(JwtUtil.verifyToken(token)){
            throw new RuntimeException("登录超时");
        }
        //将用户信息放到Security上下文中
        User loginUser = (User)userLoginService.loadUserByUsername(name);
        // 存入SecurityContextHolder
        UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken =
                new UsernamePasswordAuthenticationToken(loginUser,null,loginUser.getAuthorities());
        SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);

        // 放行
        filterChain.doFilter(request,response);
    }

    /**
     * 获取token
     * @param request request
     * @return 返回token
     */
    private String getToken(HttpServletRequest request) {
        String token = null;
        Map<String, String[]> queryParams =  request.getParameterMap();
        String[] stringList = queryParams.get(AUTHORIZATION_TOKEN);
        if (stringList != null && stringList.length > 0) {
            token = stringList[0];
        }
        String tokenHeader = request.getHeader(AUTHORIZATION_TOKEN);
        if (tokenHeader != null) {
            token = tokenHeader;
        }
        return token;
    }

}

```

SecurityConfig

添加登录过滤器

// 添加过滤器
 // 把token校验过滤器添加到过滤器链中  把token过滤器添加到 验证用户名和密码的前面
 http.addFilterBefore(tokenFilter, UsernamePasswordAuthenticationFilter.class);

```java
package com.situ.rbac.config;

import com.situ.rbac.config.filter.TokenFilter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig {

    @Autowired
    private TokenFilter tokenFilter;

//    @Bean
//    public WebSecurityCustomizer webSecurityCustomizer() {
//             return (web) -> web.ignoring().antMatchers("/images/**", "/js/**","/css/**",  "/webjars/**");
//    }
    @Bean
    public AuthenticationManager authManager(HttpSecurity http, PasswordEncoder bCryptPasswordEncoder, UserDetailsService userDetailsService)
            throws Exception {
        return http.getSharedObject(AuthenticationManagerBuilder.class)
                .userDetailsService(userDetailsService)
                .passwordEncoder(bCryptPasswordEncoder)
                .and()
                .build();
    }
    // 创建 BCryptPasswordEncoder 注入容器  指定密码使用哪种加密算法
    @Bean
    public BCryptPasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Bean
     public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
//        http.formLogin()//指定支持基于表单的身份验证
//            .and()
        //不通过Session获取SecurityContext
        // 设置不会创建一个session对象  Spring Security will never create an HttpSession and it will never use it to obtain the SecurityContext
        http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeRequests()//允许基于使用HttpServletRequest限制访问
//            .antMatchers("/admin")
//            .hasRole("ADMIN")//限制单个角色访问，角色将被增加 “ROLE_” .所以”ADMIN” 将和 “ROLE_ADMIN”进行比较
//            .antMatchers("/user")
//            .hasAnyRole("ADMIN","USER")//多个
            //swagger
            .antMatchers("/v2/api-docs").permitAll()
            .antMatchers("/favicon.ico").permitAll()
            .antMatchers("/swagger-ui.html").permitAll()
            .antMatchers("/swagger-resources/**").permitAll()
            .antMatchers("/webjars/**").permitAll()
            .antMatchers("/test/**").permitAll()
            .antMatchers("/rbac/user/login").anonymous()//登录接口
            .anyRequest()     // 所有请求
            .authenticated();  // 需要登录
        // 添加过滤器
        // 把token校验过滤器添加到过滤器链中  把token过滤器添加到 验证用户名和密码的前面
        http.addFilterBefore(tokenFilter, UsernamePasswordAuthenticationFilter.class);

        // 允许跨域
        http.cors().configurationSource(corsConfigurationSource()).and().csrf().disable();

        return http.build();
    }

    public CorsConfigurationSource corsConfigurationSource() {
        //跨域配置
        CorsConfiguration configuration = new CorsConfiguration();
        //允许访问的客户端域名
        configuration.addAllowedOrigin("*");
        //允许访问的客户端请求头
        configuration.addAllowedHeader("*");
        //允许访问的客户端请求方式，get post
        configuration.addAllowedMethod("*");
        //暴露那些头部信息
        configuration.addExposedHeader("*");
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();

        source.registerCorsConfiguration("/**", configuration);

        return source;

    }

}

```

