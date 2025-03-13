# **Aspect Oriented Programming**

Aspect is simply a piece of code that Spring Framework executes when a specific method is called which has business logic inside them.

## **Parts of AOP:**

Aspect: this means what code or logic we want the spring to execute, if a specific method is called.

Advice: When do we want the spring to execute the given aspect. Like before or after the method call.

Pointcut: This determines which methods in the application are gonna trigger the Advice.

## **Weaving Inside AOP**

**Weaving** is the process of **injecting** (or applying) aspects (like logging, security, transactions) into the target object **at specific points** (join points) during the application lifecycle.

It integrates **advice** into the **target methods** based on the **pointcut** definition.

Different types of weaving: Compile time weaving, load time weaving and runtime weaving (Spring AOP uses this)

## Types of Advises:

1. @Before
2. @AfterReturning
3. @AfterThrowing
4. @After
5. @Around

Simple implementation of AOP in spring →

```java
//Service class
@Service
class UserService{
	public void userServiceMethod(){
		System.out.Println("Hello World");
	}
}

// AOP
@Aspect
@Component
public class LoggingAspect{
	//PointCut: Matches any service in userService class
	// ther are lots of other exptessions than execution
	@Before("execution(* com.example.service.UserService.*(..))")
	public void loggingInfo(){
		System.out.Println("Logging before method execution");
	}
}
```

## **AOP step by step process**

1. When the application starts, Spring **registers** all beans annotated with `@Aspect`.
2. If a bean’s method matches an AOP **Pointcut**, Spring **wraps that bean with a proxy**.
3. The proxy replaces the original bean in the **Spring context**.
4. When a method is called, it **first** goes to the **proxy**.
5. The proxy **checks** if any AOP advice (e.g., `@Before`, `@After`, `@Around`) should be applied.
6. If applicable, the **aspect code runs** before or after the actual method.
