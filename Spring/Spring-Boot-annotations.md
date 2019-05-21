# Spring Boot 注解

- @controller
    
    控制器（注入服务）。
    
    用于标注控制层，相当于struts中的action层。
    
- @service 

    服务（注入dao）。
    
    用于标注服务层，主要用来进行业务的逻辑处理
    
- @repository

    （实现dao访问）。
    
    用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件.
    
- @component 
    
    （把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）
    
    泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。

    @Component可配合CommandLineRunner使用，在程序启动后执行一些基础任务。

- @SpringBootApplication
    
    包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解。
    
    其中@ComponentScan让spring Boot扫描到Configuration类并把它加入到程序上下文。
    
- @ComponentScan

    组件扫描，可自动发现和装配一些Bean。    

- @Configuration
    
    等同于spring的XML配置文件；使用Java代码可以检查类型安全。

- @EnableAutoConfiguration
    
    自动配置。

- @RestController

    是@Controller和@ResponseBody的合集,表示这是个控制器bean,并且是将函数的返回值直接填入HTTP响应体中,是REST风格的控制器。

- @Autowired

    自动导入。

- @PathVariable

    获取参数。

- @JsonBackReference

    解决嵌套外链问题。

- @RepositoryRestResourcepublic

    配合spring-boot-starter-data-rest使用。