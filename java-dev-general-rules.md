# Global Rules for a Windsurf Java Developer AI Agent

---

# Your role description
You are an expert in Java programming, Spring Boot, Spring Framework, Gradle, Maven, JUnit, and related Java technologies.

Code Style and Structure
- Write clean, efficient, and well-documented Java code with accurate Spring Boot examples.
- Use Spring Boot best practices and conventions throughout your code.
- Implement RESTful API design patterns when creating web services.
- Use descriptive method and variable names following camelCase convention.
- Structure Spring Boot applications: controllers, services, repositories, models, configurations.

Spring Boot Specifics
- Use Spring Boot starters for quick project setup and dependency management.
- Implement proper use of annotations (e.g., @SpringBootApplication, @RestController, @Service).
- Utilize Spring Boot's auto-configuration features effectively.
- Implement proper exception handling using @ControllerAdvice and @ExceptionHandler.

Naming Conventions
- Use PascalCase for class names (e.g., UserController, OrderService).
- Use camelCase for method and variable names (e.g., findUserById, isOrderValid).
- Use ALL_CAPS for constants (e.g., MAX_RETRY_ATTEMPTS, DEFAULT_PAGE_SIZE).

Java and Spring Boot Usage
- Use Java 17 or later features when applicable (e.g., records, sealed classes, pattern matching).
- Leverage Spring Boot 3.x features and best practices.
- Use Spring Data JPA for database operations when applicable.
- Implement proper validation using Bean Validation (e.g., @Valid, custom validators).

Configuration and Properties
- Use application.properties or application.yml for configuration.
- Implement environment-specific configurations using Spring Profiles.
- Use @ConfigurationProperties for type-safe configuration properties.

Dependency Injection and IoC
- Use constructor injection over field injection for better testability.
- Leverage Spring's IoC container for managing bean lifecycles.

Testing
- Write unit tests using JUnit 5 and Spring Boot Test.
- Use MockMvc for testing web layers.
- Implement integration tests using @SpringBootTest.
- Use @DataJpaTest for repository layer tests.

Performance and Scalability
- Implement caching strategies using Spring Cache abstraction.
- Use async processing with @Async for non-blocking operations.
- Implement proper database indexing and query optimization.

Security
- Implement Spring Security for authentication and authorization.
- Use proper password encoding (e.g., BCrypt).
- Implement CORS configuration when necessary.

Logging and Monitoring
- Use SLF4J with Logback for logging.
- Implement proper log levels (ERROR, WARN, INFO, DEBUG).
- Use Spring Boot Actuator for application monitoring and metrics.

API Documentation
- Use Springdoc OpenAPI (formerly Swagger) for API documentation.

Data Access and ORM
- Use Spring Data JPA for database operations.
- Implement proper entity relationships and cascading.
- Use database migrations with tools like Flyway or Liquibase.

Build and Deployment
- Use Maven for dependency management and build processes.
- Implement proper profiles for different environments (dev, test, prod).
- Use Docker for containerization if applicable.

Follow best practices for:
- RESTful API design (proper use of HTTP methods, status codes, etc.).
- Microservices architecture (if applicable).
- Asynchronous processing using Spring's @Async or reactive programming with Spring WebFlux.

Adhere to SOLID principles and maintain high cohesion and low coupling in your Spring Boot application design.

---

# MercadoLibre Internal Libraries Priority

## Rule: Always prioritize MercadoLibre's internal libraries over external/Spring Framework equivalents

When working with REST clients, JSON processing, routing, and other common functionalities, **ALWAYS** search for and use MercadoLibre's internal libraries first before considering Spring Framework or other external libraries.

## Key Internal Libraries to Prioritize:

### REST Client Operations
- **Use**: `com.mercadolibre.restclient.MeliRestClient`
- **Instead of**: Spring's `RestTemplate`, `WebClient`, or other HTTP clients
- **Related classes**:
    - `com.mercadolibre.restclient.Response`
    - `com.mercadolibre.restclient.exception.RestException`
    - `com.mercadolibre.restclient.http.Headers`
    - `com.mercadolibre.restclient.http.HttpStatus`

### JSON Processing
- **Use**: `com.mercadolibre.json.JsonUtils`
- **Instead of**: Jackson's `ObjectMapper`, Spring's JSON utilities

### Routing and Context
- **Use**: `com.mercadolibre.routing.RoutingHelper`
- **Instead of**: Custom routing implementations

### Configuration and Pools
- **Use**: Internal pool configurations (e.g., `RestPoolConfig.getAmlCasesPoolKey()`)
- **Instead of**: Generic connection pool configurations

## Implementation Priority:
1. **First**: Search for `com.mercadolibre.*` packages and internal libraries
2. **Second**: Check if MercadoLibre has specific configurations or utilities for the task
3. **Last Resort**: Use Spring Framework or external libraries only if no internal equivalent exists

## Example Usage Pattern:
java
// ✅ CORRECT - Using MercadoLibre internal libraries
@Component
public class MyServiceImpl {
private final MeliRestClient restClient;

    public void makeRequest() {
        Headers headers = new Headers();
        Response response = restClient
            .withPool(RestPoolConfig.getMyPoolKey())
            .get(uri, headers, RoutingHelper.getOrCreateMeliContext());
        
        MyDTO result = JsonUtils.INSTANCE.toObject(response.getString(), MyDTO.class);
    }
}

// ❌ AVOID - Using external libraries when internal ones exist
@Component
public class MyServiceImpl {
private final RestTemplate restTemplate; // Don't use this if MeliRestClient is available
}

## When suggesting solutions:
- Always mention and prioritize internal MercadoLibre libraries
- Provide examples using the internal APIs
- Only suggest Spring/external alternatives if explicitly asked or if no internal library exists
- Consider MercadoLibre's conventions and patterns for configuration, error handling, and logging 

---
## Style Guide for Java Code in Mercado Libre

Our style guide is mainly inspired by [Google's](https://google.github.io/styleguide/javaguide.html), we also took some elements from [Oracle's](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf) ruleset. We use the following conventions:

### Line length

To read full lines on our 13" screens, lines should be at **max 140 characters long**.

**Don't 🚫**

```java
public void foo(String firstParameter, String secondParameter, String thirdParameter) {
  String result = methodWithReallyLongSignatureNameWhichAlsoTakesALotOfParameters(firstParameter, secondParameter, thirdParameter, firstField, secondExtraField)
  System.out.println(result);
}
```

**Do ✅**

```java
public void foo(String firstParameter, String secondParameter, String thirdParameter) {
  String result = methodWithReallyLongSignatureNameWhichAlsoTakesALotOfParameters(
    firstParameter, secondParameter, thirdParameter, firstField, secondExtraField
  )
  System.out.println(result);
}
```

### Indentation

We use 2 whitespaces for an indent to make the best use of the horizontal space on our screens.

**Don't 🚫**

```java
if (foo) {
    bar();
}
```

**Do ✅**

```java
if (foo) {
  bar();
}
```

### Whitespaces

We don't like trailing whitespaces because they're useless, make the code less compact, and may lead to inconsistencies.

**Don't 🚫**

```java
public void foo(String      bar ) {
  System.out.println(bar);
}
```

**Do ✅**

```java
public void foo(String bar) {
  System.out.println(bar);
}
```

### Single Statements

Although Java allows multiple statements on a single line, it's confusing to read and not useful.

**Don't 🚫**

```java
public void foo(String bar) {
  String barToUpperCase = bar.toUpperCaser(); System.out.println(bar);
}
```

**Do ✅**

```java
public void foo(String bar) {
  String barToUpperCase = bar.toUpperCaser();
  System.out.println(bar);
}
```

### Import Statements

Imports are part of the code that needs to be read too! To make them easier to read we prefer the following rules:

* One import per line
* No wildcard imports
* Imports separated into 2 groups static and non-static (separated by a single blank line)
* No blank lines between same group imports
* Imports in the same group alphabetically sorted

**Don't 🚫**

```java
import static com.mercadolibre.baz // static import not separated
import com.mercadolibre.* // wildcard

import com.mercadolibre.foo // blank line between same group imports
import com.mercadolibre.bar // not sorted
```

**Do ✅**

```java
import static com.mercadolibre.baz // static imports in the different group separated by a blank line

import com.mercadolibre.bar
import com.mercadolibre.foo // alphabetically sorted
```

### Braces

For convention popularity reasons, we've decided to go with Google's [K&R style](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style).

**Don't 🚫**

```java
public void foo()
{
  if (bar) baz(); // optional braces not used
}
```

**Do ✅**

```java
public void foo() {
  if (bar) { baz(); } // optional braces used
}
```

### Line Wrapping

Wrapping lines may help improve readability in certain statements. To get the best out of it, we use the following rules:

* No line-wrapping in import statements
* Mandatory line-wrapping in chained method calls
* Optional line wrapping in other cases

**Don't 🚫**

```java
import com.mercadolibre
  .bar // line-wrapping in import statement

public void foo() {
  Baz baz = new Foo().getBar().getBaz(); // chained method calls not wrapped
}
```

**Do ✅**

```java
import com.mercadolibre.bar // single line for import statement

public void foo() {
  Baz baz = new Foo()
              .getBar()
              .getBaz(); // wrapped chained method calls

  printObject("baz", Baz); // OK but not mandatory
  printObject(
    "baz",
    Baz
  ) // also OK but not mandatory
}
```

### Final Variables

Java allows the use of the `final` keyword when variables' values are not modified to guarantee immutability.

* Class's fields: required when the field is never modified
* Methods' parameters: accepted but not required
* Methods' local variables: accepted but not required

**Don't 🚫**

```java
public class Foo {
  private Logger logger = LoggerFactory.getLogger(this.class); // Assumes logger is never modified so should be final
}
```

**Do ✅**

```java
public class Foo {
  private final Logger logger = LoggerFactory.getLogger(this.class); // use of final keyword
  private int count = 0; // it's ok because the counter is modified

  public void bar(final int baz) { // accepted but not required
    count++;
    final int barPlusCount = count + baz; // accepted but not required
    System.out.println(barPlusCount);
  }

}
```

#### Naming convention

| Type            | Case                       |
| ---             | ---                        |
| Packages        | lowercase (no underscores) |
| Classes         | UpperCamelCase             |
| Enum name       | UpperCamelCase             |
| Enum value      | CAPS_WITH_UNDER            |
| Exceptions      | UpperCamelCase             |
| Methods         | lowerCamelCase             |
| Constants       | CAPS_WITH_UNDER            |
| Fields          | lowerCamelCase             |
| Parameters      | lowerCamelCase             |
| Local variables | lowerCamelCase             |

#### Documentation

Javadoc comments should be present in every public method following the [Javadoc format](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html).

**Don't 🚫**

```java

public int successfullyAttacked(int incomingDamage) { // undocumented public method
    health -= incomingDamage;
    return health;
}

// attack monster
public int attack(int outgoingDamage, Monster monster) { // poorly documented method
  monster.successfullyAttacked(damage);
}

```

**Do ✅**

```java
/**
 * <p>This is a simple description of the method. . .
 * <a href="http://www.supermanisthegreatest.com">Superman!</a>
 * </p>
 * @param incomingDamage the amount of incoming damage
 * @return the amount of health hero has after attack
 * @see <a href="http://www.link_to_jira/HERO-402">HERO-402</a>
 * @since 1.0
 */
public int successfullyAttacked(int incomingDamage) {
    health -= incomingDamage;
    return health;
}
```

## [Full Java Standard Ruleset](/languages/java_rules.md)~~
