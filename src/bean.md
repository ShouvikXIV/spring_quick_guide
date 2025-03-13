# @Bean
Whenever a method is marked as bean, the returned object of that method will be considered as a spring bean. We need to have `@Configuration` annotation on the class containing bean. It indicates that the class has `@Bean` definition methods. The name of the Bean will be the name of the method. We can give different name for the bean as well→ `@Bean(value=“customName”)`

`AnnotationConfigApplicationContext()` this method is used to manually create beans and initialize them in the IOC Container. But as we use `@SpringBootApplication` annotation we don’t need to call this method manually. `@SpringBootApplication` annotation does this automatically for us.

`NoUniqueBeanDefinitionException`  when more than one bean has the same type. SpringBoot actually can initialize multiple beans with same type but we need to specify the ways to handle and distinguish between them. So, to do that we can use `@Primary` annotation.

`@Component` annotation converts a class to a bean. Meaning, if a class is mentioned as a component, we won’t need to manually create objects of that class. Spring will create and manage all the objects.

`@Bean` is a method level annotation and `@Component` is a class level annotation.

We can add custom logic after a bean is created or a bean is about to be destroyed, using PostConstruct or PreDestroy annotation.
<aside>
💡
PostConstruct and PreDestroy annotations are Deprecated. Alternatives →

</aside>

```java
#Alternative_1
@Component
public class MyService implements InitializingBean, DisposableBean {

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("Bean initialized after properties are set.");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("Cleanup before bean destruction.");
    }
}

#Alternative_2
@Configuration
public class AppConfig {

    @Bean(initMethod = "init", destroyMethod = "cleanup")
    public MyService myService() {
        return new MyService();
    }
}

class MyService {
    public void init() {
        System.out.println("Custom init method called!");
    }

    public void cleanup() {
        System.out.println("Custom destroy method called!");
    }
}

```
