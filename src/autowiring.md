# **Autowiring**

`@Autowired` annotation is used to do the wiring between different beans. Because of this annotation we don’t need to create an object of a class explicitly. It makes the dependency injection possible.

**If there are multiple beans of same type, how does @Autowired annotation tries solves this problem before throwing an exception**

1. so, what it does first is, looks for bean with same name as the constructor parameter.
2. Now, if it the bean name and parameter name are not the same, then it looks for the bean marked as primary.
3. Finally, if it cannot find any bean marked as primary, then it will look for if any specification is made using `@Qualifier` annotation.

```java
@Autowired
public Constructor(@Qualifier("nameOfTheBean") BeanType name){...}
```

and if none of the above work, the `NoUniqueBeanDefinitionException` will be thrown.

## **Bean Scopes and Lifecycle**

### **Singleton Bean Scope**

Default scope of bean is Singleton.

Singleton means we will always get the same instance for a single bean. If we have multiple beans with same type, the singleton scope will make sure, we’ll have only one instance of each bean. for example: if we have multiple beans for type vehicle (vehicle1, vehicle2, vehicle3), the singleton scope will make sure we have only one instance of vehicle1, vehicle2 and vehicle3.

Singleton is used the most. For single threaded application, singleton beans are more suitable.

**Race Condition** occurs when two threads access a shared variable at the same time. Mutable singleton beans result to race condition.

### **Eager and Lazy instantiation**

Eager instantiation means all the beans will be created at the starting of the application.

Lazy instantiation means the bean will only be created only when the application is trying to refer to it for the first time.

By default spring follows Eager instantiation. To instantiate bean as Lazy, use `@Lazy`

Possibility to have runtime error in Lazy instantiation.

Difference between Lazy and Eager Instantiation →

| Eager                                                                                                | Lazy                                                                                                     |
|:-----------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------|
| This is the default behaviour inside Spring Framework.                                               | This is not the default behaviour and need to configure explicitly using @Lazy                           |
| The Singleton bean will be created during the startup of the application.                            | The SingleTon bean will be created when the app is trying to refer the bean for the first time.          |
| The server will not start if bean is not able to create due to any dependent exceptions.             | Application will throw an exception runtime, if bean creating is failed, due to any dependent exceptions |
| Spring context will occupy lot of memory if we try to use eager for all beans inside an application. | The performance will be impacted if we try to use Lazy for all beans inside an application.              | 
| Eager can be followed for all the beans which are required very commonly inside an application.      | Lazy can be followed for the beans that are used in a very remote scenario inside an application.        |

### **Prototype Bean Scope**

`@Scope(BeanDefinition.SCOPE_PROTOTYPE)`

In prototype bean scope, every time we request a reference of a bean, a new instance of that object will be created by spring. (Rarely used)

Prototype scope doesn’t create race condition.

Difference between Singleton and Prototype scope →

| Singleton                                                                                 | Prototype                                                                        |
|:------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------|
| This is the default scope inside Spring Framework.                                        | Need to explicitly configure using @Scope(BeanDefinition_SCOPE_PROTOTYPE)        |
| The same object instance will be returned every time we refer a bean inside the code.     | New object instance will be returned every time we refer a bean inside the code. |
| We can configure to create the beans during the start up or when the first time referred. | Spring always create new object when we try to refer the bean.                   |
| Immutable objects can be ideal for singleton scope.                                       | Mutable objects can be ideal for prototype scope.                                |
| Most commonly used.                                                                       | Very rarely used.                                                                |

### **Request Bean Scope (Less common)**:

- Used in web applications where a new bean instance is created for each HTTP request.
- Useful for holding request-specific data, such as form submissions or user input.
- Not as common as singleton or prototype but still used in web applications.

### **Session Bean Scope (Less common)**:

- Used in web applications where a bean instance is created and tied to an HTTP session.
- Useful for storing user-specific data, such as login information or shopping cart contents.
- Less common than singleton or prototype but still used in web applications with session management.

### **Application Bean Scope (Rare)**:

- Rarely used.
- Similar to the singleton scope but scoped to a ServletContext.
- Typically used for shared resources in a web application context.