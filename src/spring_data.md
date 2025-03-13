# Spring Data

Properties for database connection

```java
spring.datasource.url=jdbc:mysql://192.168.14.24:3306/schema_name
spring.datasource.username=app
spring.datasource.password=Cr@ar346

# Enable SQL logging
spring.jpa.show-sql=true

# Format SQL for readability
spring.jpa.properties.hibernate.format_sql=true

# Enable SQL query logging
logging.level.org.hibernate.SQL=DEBUG

# Log parameter values bound to SQL queries
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

## **Auditing Support By Spring Data**

### **What is Auditing?**

Auditing is the process of tracking and logging changes made to data over time. It involves recording **who** made the change, **what** was changed, **when** the change was made, and sometimes **why** the change was made. Auditing is crucial for maintaining data integrity, ensuring compliance with regulations, and debugging issues.

Example of an Auditing Class →

```java
@MappedSuperclass
@Data
@EntityListeners(AuditingEntityListener.class)
public class AuditableBase {
    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @CreatedBy
    @Column(updatable = false)
    private Long createdBy;

    @LastModifiedDate
    @Column(insertable = false)
    private LocalDateTime updatedAt;

    @LastModifiedBy
    @Column(insertable = false)
    private Long updatedBy;
}

//This configuration class is needed cause it gives us the loggedIn user id
@Configuration
@EnableJpaAuditing(auditorAwareRef = "auditorProvider")
public class AuditConfig {

    @Bean
    public AuditorAware<Long> auditorProvider() {
        return () -> Util.getLoggedInUserId();
    }
}
```

`@MappedSuperClass` : A `@MappedSuperclass` is **not an entity itself.** It is used to define common fields (e.g., auditing fields like `createdAt`, `createdBy`, etc.) that can be reused across multiple entities.

**What is `@MappedSuperclass`?**

- `@MappedSuperclass` is a JPA annotation used to define a **base class** that is **not an entity itself** but provides common fields or mappings that can be inherited by entity classes.
- The fields and mappings defined in the `@MappedSuperclass` are treated as if they were defined directly in the subclass entities.
- A `@MappedSuperclass` cannot be queried or persisted directly. It is only used for inheritance.

`@EntityListeners(AuditingEntityListener.class)` :

- The `@EntityListeners` annotation is used to specify **callback listener classes** for an entity or mapped superclass.
- The `AuditingEntityListener` class is provided by Spring Data JPA and is responsible for **automatically populating auditing fields** (e.g., `createdAt`, `createdBy`, `updatedAt`, `updatedBy`) when the entity is persisted or updated.