# 跨域

## 什么是跨域

**跨域：**指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对Javascript施加的安全限制。

例如：a页面想获取b页面资源，如果a、b页面的`协议、域名、端口、子域名`不同，所进行的访问行动都是跨域的，而浏览器为了安全问题一般都限制了跨域访问，也就是不允许跨域请求资源。

注意：跨域限制访问，其实是**浏览器的限制**。理解这一点很重要。

**同源策略：是指协议(http、https)，域名，端口都要相同，其中有一个不同都会产生跨域。**

![](pic\3cors.png)

> 产生跨域问题。在控制台会输出如下信息：

```text
Access to XMLHttpRequest at 'xxxxxxxxxxxxxxxxxx' from origin 'xxxxxxxxxxx' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested
```

示例：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 引入jquery -->
    <script src="js/jquery.js"></script>
    <Script>
        function test(){
        $.ajax({
            url:"http://localhost:9999/test/",
            dataType:"text",
            success:function(){
                console.log("成功");
            },
            error:function(){
                console.log("失败");
            }
        })
    }
    </Script>
</head>
<body>
    <button onclick="test()">测试</button>
</body>
</html>
```

访问报错：

Access to XMLHttpRequest at 'http://localhost:9999/test/' from origin 'null' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

## 如何处理

方法一：在Controller上添加`@CrossOrigin`。单个处理，不推荐使用

方法二：统一处理，配置文件

 允许跨域，添加corsConfigurationSource()方法

```java
package com.situ.rbac.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.annotation.web.configuration.WebSecurityCustomizer;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
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
    // 创建 BCryptPasswordEncoder 注入容器  指定密码使用哪种加密算法
    @Bean
    public BCryptPasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Bean
     public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.formLogin()//指定支持基于表单的身份验证
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

方法三 nginx服务器配置响应header解决跨域问题

