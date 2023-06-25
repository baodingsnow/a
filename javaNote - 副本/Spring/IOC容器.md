# IOC原理

> 在Spring的IoC容器中，我们把所有组件统称为JavaBean，即配置一个组件就是配置一个Bean。





Spring提供的容器又称为IoC容器

什么是Ioc容器    IoC又称为依赖注入（DI：Dependency Injection），它解决了一个最主要的问题：将组件的创建+配置与组件的使用相分离，并且，由IoC容器负责管理组件的生命周期。

# 定制bean

## 注入List

有些时候，我们会有一系列接口相同，不同实现类的Bean。例如，注册用户时，我们要对email、password和name这3个变量进行验证。为了便于扩展，我们先定义验证接口：

```java
public interface Validator {
    void validate(String email, String password, String name);
}
```

然后创建三个不同的实现类分别验证邮箱密码和名字

```java
@Component
public class EmailValidator implements Validator(){}
@Component
public class passwordValidator implements Validator(){}
@Component
public class nameValidator implements Validator(){}
```

对应的Controller   注入的时候注入存放validator类型的List

```java
@Component
public class Validators {
    @Autowired
    List<Validator> validators;

    public void validate(String email, String password, String name) {
        for (var validator : this.validators) {
            validator.validate(email, password, name);
        }
    }
}
```

