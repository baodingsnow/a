==在程序中可以使用缓存的技术来节省对数据库的开销==。Spring Boot对缓存提供了很好的支持，我们几乎不用做过多的配置即可使用各种缓存实现。这里主要介绍Redis缓存实现。

> 两次查询结果一样，但程序仍会执行程序
>
> 设置缓存后  第二次查询不执行，但会获得结果，从缓冲中获取

# 使用缓存

要开启Spring Boot的缓存功能，需要在pom中引入`spring-boot-starter-cache`：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```



接着在Spring Boot入口类中加入`@EnableCaching`注解开启缓存功能：

```java
@SpringBootApplication
@EnableCaching
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```

在StudentService接口中加入缓存注解：

```Java
@CacheConfig(cacheNames = "student")
@Repository
public interface StudentService {
    @CachePut(key = "#p0.sno")
    Student update(Student student);
    
    @CacheEvict(key = "#p0", allEntries = true)
    void deleteStudentBySno(String sno);
    
    @Cacheable(key = "#p0")
    Student queryStudentBySno(String sno);
}
```

```
在Spring Boot中可使用的缓存注解有：

缓存注解
@CacheConfig：主要用于配置该类中会用到的一些共用的缓存配置。在这里@CacheConfig(cacheNames = "student")：配置了该数据访问对象中返回的内容将存储于名为student的缓存对象中，我们也可以不使用该注解，直接通过@Cacheable自己配置缓存集的名字来定义；

@Cacheable：配置了queryStudentBySno函数的返回值将被加入缓存。同时在查询时，会先从缓存中获取，若不存在才再发起对数据库的访问。该注解主要有下面几个参数：

value、cacheNames：两个等同的参数（cacheNames为Spring 4新增，作为value的别名），用于指定缓存存储的集合名。由于Spring 4中新增了@CacheConfig，因此在Spring 3中原本必须有的value属性，也成为非必需项了；

key：缓存对象存储在Map集合中的key值，非必需，缺省按照函数的所有参数组合作为key值，若自己配置需使用SpEL表达式，比如：@Cacheable(key = "#p0")：使用函数第一个参数作为缓存的key值，更多关于SpEL表达式的详细内容可参考https://docs.spring.io/spring/docs/current/spring-framework-reference/integration.html#cache；

condition：缓存对象的条件，非必需，也需使用SpEL表达式，只有满足表达式条件的内容才会被缓存，比如：@Cacheable(key = "#p0", condition = "#p0.length() < 3")，表示只有当第一个参数的长度小于3的时候才会被缓存；

unless：另外一个缓存条件参数，非必需，需使用SpEL表达式。它不同于condition参数的地方在于它的判断时机，该条件是在函数被调用之后才做判断的，所以它可以通过对result进行判断；

keyGenerator：用于指定key生成器，非必需。若需要指定一个自定义的key生成器，我们需要去实现org.springframework.cache.interceptor.KeyGenerator接口，并使用该参数来指定；

cacheManager：用于指定使用哪个缓存管理器，非必需。只有当有多个时才需要使用；

cacheResolver：用于指定使用那个缓存解析器，非必需。需通过org.springframework.cache.interceptor.CacheResolver接口来实现自己的缓存解析器，并用该参数指定；

@CachePut：配置于函数上，能够根据参数定义条件来进行缓存，其缓存的是方法的返回值，它与@Cacheable不同的是，它每次都会真实调用函数，所以主要用于数据新增和修改操作上。它的参数与@Cacheable类似，具体功能可参考上面对@Cacheable参数的解析；

@CacheEvict：配置于函数上，通常用在删除方法上，用来从缓存中移除相应数据。除了同@Cacheable一样的参数之外，它还有下面两个参数：

allEntries：非必需，默认为false。当为true时，会移除所有数据；

beforeInvocation：非必需，默认为false，会在调用方法之后移除数据。当为true时，会在调用方法之前移除数据。
```

# 缓存实现

Redis的下载地址为https://github.com/MicrosoftArchive/redis/releases，Redis 支持 32 位和 64 位。这个需要根据你系统平台的实际情况选择，这里我们下载 Redis-x64-xxx.zip压缩包到C盘。打开一个CMD窗口，输入如下命令：