# **Spring Validator**

## Important Validation Annotation
| jakarta.validation.constraints.* | org.hibernate.validator.constraints.*                                                                    |
|:---------------------------------|:---------------------------------------------------------------------------------------------------------|
| @Digits                          | This is not the default behaviour and need to configure explicitly using @Lazy                           |
| @Email                           | The SingleTon bean will be created when the app is trying to refer the bean for the first time.          |
| @Max                             | Application will throw an exception runtime, if bean creating is failed, due to any dependent exceptions |
| @Min                             | The performance will be impacted if we try to use Lazy for all beans inside an application.              | 
| @NotBlank                        | Lazy can be followed for the beans that are used in a very remote scenario inside an application.        |
|@NotEmpty                         |
|@NotNull                          |
|@Pattern                          |
|@Size                             |



- **Jakarta Validation** is a standard and should be preferred for portability.
- **Hibernate Validator** extends Jakarta Validation and is useful for extra constraints.
- **Spring Boot uses Hibernate Validator by default**, so you get both standard and additional constraints.