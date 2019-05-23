# Spring Boot 配置文件

#### Spring Boot 自定义配置文件

```text
 自定义配置文件 xxx.properties 放到指定位置：src/main/resource 

 加载自定义配置文件，使用注解： @PropertySource("classpath:xxx.properties")
 
 使用配置项，使用注解： @Value("${key}")
 
 
```