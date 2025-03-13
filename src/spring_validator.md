# **Spring Validator**

## Important Validation Annotation
| jakarta.validation.constraints.* | org.hibernate.validator.constraints.* |
|:---------------------------------|:--------------------------------------|
| @Digits                          | @CreditCardNumber                     |
| @Email                           | @Length                               |
| @Max                             | @Currency                             |
| @Min                             | @Range                                | 
| @NotBlank                        | @URL                                  |
|@NotEmpty                         | @UniqueElement                        |
|@NotNull                          | @EAN                                  |
|@Pattern                          | @ISBN                                 |
|@Size                             |                                       |



- **Jakarta Validation** is a standard and should be preferred for portability.
- **Hibernate Validator** extends Jakarta Validation and is useful for extra constraints.
- **Spring Boot uses Hibernate Validator by default**, so you get both standard and additional constraints.