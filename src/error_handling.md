# Error Handling In Spring

In **Spring Boot**, exception handling can be effectively managed using the `@RestControllerAdvice` and `@ExceptionHandle` annotations. These annotations allow us to centralize exception handling logic and provide consistent error responses across our application.

`@RestControllerAdvice` is a combination of `@ControllerAdvice` and `@ResponseBody` annotations.

## **Workflow of Exception Handling**

1. **Exception is Thrown**:
   • A method in a controller (or service, repository, etc.) throws an exception.
2. **Spring Looks for Exception Handlers**:
    - Spring first checks if the controller where the exception was thrown has an `@ExceptionHandler` method that matches the thrown exception.
    - If no matching `@ExceptionHandler` is found in the controller, Spring then looks for `@RestControllerAdvice` classes.
3. **Search in `@RestControllerAdvice` Classes**:
    - Spring scans all beans annotated with `@RestControllerAdvice` (or `@ControllerAdvice`).
    - For each `@RestControllerAdvice` bean, it checks if any method annotated with `@ExceptionHandler` matches the thrown exception.
4. **Matching `@ExceptionHandler` Method**:
    - Spring looks for the most specific `@ExceptionHandler` method that can handle the exception.
    - For example, if a `ResourceNotFoundException` (a subclass of `RuntimeException`) is thrown, Spring will prefer a method annotated with `@ExceptionHandler(ResourceNotFoundException.class)` over a method annotated with `@ExceptionHandler(RuntimeException.class)`.
5. **Invoke the Matching Handler**:
    - Once a matching `@ExceptionHandler` method is found, Spring invokes that method.
    - The method processes the exception and returns a response (e.g., a custom error message, HTTP status code, etc.).
6. **Return the Response**:
    - The response from the `@ExceptionHandler` method is sent back to the client.

### Remember!!

- If an `@ExceptionHandler` method is defined in the **controller** where the exception is thrown, it takes precedence over `@RestControllerAdvice`.
- If no matching handler is found in the controller, Spring falls back to `@RestControllerAdvice`.
- `@RestControllerAdvice` provides **global exception handling** for all controllers (or a subset of controllers if configured with attributes like `basePackages` or `annotations`).
- `@ExceptionHandler` in a controller provides **local exception handling** for that specific controller.
- `@RestControllerAdvice` works through Spring’s AOP (Aspect-Oriented Programming) proxy mechanism. When an exception is  thrown, the proxy intercepts it and delegates it to the appropriate `@ExceptionHandler` method.

## **Example of Error Handling**

```java
@RestController
public class MyController {

    @GetMapping("/resource/{id}")
    public String getResource(@PathVariable int id) {
        if (id == 0) {
            throw new ResourceNotFoundException("Resource not found with id: " + id);
        }
        return "Resource found with id: " + id;
    }
}

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleResourceNotFoundException(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGenericException(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("An error occurred: " + ex.getMessage());
    }
}
```