---
title: springboot和redis实现session共享
date: 2018-07-05 18:31:40
categories:
  - springMVC
tags:
  - springMVC
  - spring
  - 后端
---
1. 添加@EnableRedisHttpSession来开启spring session支持
```
@Configuration  
@EnableRedisHttpSession  
public class RedisSessionConfig {  
}  
```

2. @EnableRedisHttpSession这个注解是由spring-session-data-redis提供的，所以在pom.xml文件中添加
```
<dependency>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-redis</artifactId>  
</dependency>  
<dependency>  
        <groupId>org.springframework.session</groupId>  
        <artifactId>spring-session-data-redis</artifactId>  
</dependency>
```

3. application.yml 添加redis配置
```
spring:
  redis.host: xxxxxx
  redis.port: xxxx
  redis.password: xxxx
```


4. idea启动项目两个实例
4.1 选择“Edit Configurations”，取消“Single instance only”选项
4.2 复制springboot 运行配置
4.3 在VM options项中填写-Dserver.port=8082，设置这个实例的运行端口为8082
