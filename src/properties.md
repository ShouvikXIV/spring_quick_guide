# **Properties**

let’s say I have a value in properties file and I want to retrieve it in the java file. here’s how to do it.

```java
//Activing a property
spring.profiles.active=live

// in properties file
test.string = "your-name"

// in the java file
@Value("${test.string}")
private String testString;
```

Reading properties.

For this first we need to have Environment bean. and from that bean we can use the .getProperty() method.

```java
....
@Autowired
private Environment environment;
private void log(){
log.info(environment.getProperty("test.string"));
}
```