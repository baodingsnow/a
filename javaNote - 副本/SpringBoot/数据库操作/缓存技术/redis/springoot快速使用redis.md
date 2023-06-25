

# 准备工作

yml中配置日志输出级别以观察SQL的执行情况

```yml
logging:
  level:
    com:
      springboot:
        mapper: debug
```

其中`com.spring.mapper`为MyBatis的Mapper接口路径。

然后编写如下测试方法：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
public class ApplicationTest {

    @Autowired
    private StudentService studentService;
    
    @Test
    public void test() throws Exception {
        Student student1 = this.studentService.queryStudentBySno("001");
        System.out.println("学号" + student1.getSno() + "的学生姓名为：" + student1.getName());
        
        Student student2 = this.studentService.queryStudentBySno("001");
        System.out.println("学号" + student2.getSno() + "的学生姓名为：" + student2.getName());
    }
}
```

第二个查询虽然和第一个查询完全一样，但其还是对数据库进行了查询。接下来引入缓存来改善这个结果。

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

```java
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

我们在StudentService接口中加入了`@CacheConfig`注解，queryStudentBySno方法使用了注解`@Cacheable(key="#p0")`，即将id作为redis中的key值。当我们更新数据的时候，应该使用`@CachePut(key="#p0.sno")`进行缓存数据的更新，否则将查询到脏数据，因为该注解保存的是方法的返回值，所以这里应该返回Student。

```java
@Repository("studentService")
public class StudentServiceImpl implements StudentService{
    @Autowired
    private StudentMapper studentMapper;
    
    @Override
    public Student update(Student student) {
        this.studentMapper.update(student);
        return this.studentMapper.queryStudentBySno(student.getSno());
    }
    
    @Override
    public void deleteStudentBySno(String sno) {
        this.studentMapper.deleteStudentBySno(sno);
    }
    
    @Override
    public Student queryStudentBySno(String sno) {
        return this.studentMapper.queryStudentBySno(sno);
    }
}
```

# redis下载及配置

Redis的下载地址为https://github.com/MicrosoftArchive/redis/releases，Redis 支持 32 位和 64 位。这个需要根据你系统平台的实际情况选择，这里我们下载 Redis-x64-xxx.zip压缩包到C盘。打开一个CMD窗口，输入如下命令：

```cmd
C:\Users\Administrator>cd c:\Redis-x64-3.2.100

c:\Redis-x64-3.2.100>redis-server.exe redis.windows.conf
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 3.2.100 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 6404
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

[6404] 25 Dec 09:47:58.890 # Server started, Redis version 3.2.100
[6404] 25 Dec 09:47:58.898 * DB loaded from disk: 0.007 seconds
[6404] 25 Dec 09:47:58.898 * The server is now ready to accept connections on port 6379
```

然后打开另外一个CMD终端，输入：

```cmd
C:\Users\Administrator>cd c:\Redis-x64-3.2.100

c:\Redis-x64-3.2.100>redis-cli.exe -p 6379
127.0.0.1:6379>
```

准备工作做完后，接下来开始在Spring Boot项目里引入Redis：

```
<!-- spring-boot redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

在application.yml中配置Redis：

```
spring:
  redis:
    # Redis数据库索引（默认为0）
    database: 0
    # Redis服务器地址
    host: localhost
    # Redis服务器连接端口
    port: 6379
    pool:
      # 连接池最大连接数（使用负值表示没有限制）
      max-active: 8
      # 连接池最大阻塞等待时间（使用负值表示没有限制）
      max-wait: -1
      # 连接池中的最大空闲连接
      max-idle: 8
      # 连接池中的最小空闲连接
      min-idle: 0
    # 连接超时时间（毫秒）
    timeout: 0
```

接着创建一个Redis配置类：

```java
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    // 自定义缓存key生成策略
    @Bean
    public KeyGenerator keyGenerator() {
        return new KeyGenerator() {
            @Override
            public Object generate(Object target, java.lang.reflect.Method method, Object... params) {
                StringBuffer sb = new StringBuffer();
                sb.append(target.getClass().getName());
                sb.append(method.getName());
                for (Object obj : params) {
                    sb.append(obj.toString());
                }
                return sb.toString();
            }
        };
    }

    // 缓存管理器
    @Bean
    public CacheManager cacheManager(@SuppressWarnings("rawtypes") RedisTemplate redisTemplate) {
        RedisCacheManager cacheManager = new RedisCacheManager(redisTemplate);
        // 设置缓存过期时间（秒）
        cacheManager.setDefaultExpiration(3600);
        return cacheManager;
    }

    @Bean
    public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory factory) {
        StringRedisTemplate template = new StringRedisTemplate(factory);
        setSerializer(template);// 设置序列化工具
        template.afterPropertiesSet();
        return template;
    }

    private void setSerializer(StringRedisTemplate template) {
        @SuppressWarnings({ "rawtypes", "unchecked" })
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setValueSerializer(jackson2JsonRedisSerializer);
    }
}
```

redis desktop manager    redis可视化工具