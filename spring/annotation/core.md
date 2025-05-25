Spring Core Annotations - In-Depth Guide

This guide covers the foundational annotations used in every Spring Boot application. These annotations are essential for configuration, component scanning, and auto-configuration.


---

1. @SpringBootApplication

Purpose:

Marks the main class of a Spring Boot application. It is a meta-annotation that combines:

@Configuration – Declares class as a configuration source

@EnableAutoConfiguration – Enables Spring Boot’s auto-configuration

@ComponentScan – Scans for components in the current package and sub-packages


Example:

@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}

Real-World Use Case:

This is used as the entry point in every Spring Boot application. Spring Boot handles classpath scanning and bean creation automatically with this annotation.

Internal Working:

On startup, SpringBootApplication triggers component scanning.

Loads META-INF/spring.factories to determine auto-configurations to apply.


Interview Insights:

Q: What does @SpringBootApplication do internally? A: It's a composition of three annotations. It triggers scanning, enables autoconfig, and marks the class for configuration.



---

2. @Component, @Service, @Repository, @Controller

Purpose:

All of these are specializations of @Component. They mark classes as Spring-managed beans with different semantics:

Annotation	Typical Use

@Component	General-purpose Spring Bean
@Service	Business logic or service layer
@Repository	Data access layer (with JPA)
@Controller	MVC Controller


Example:

@Service
public class OrderService {
    public void processOrder() {}
}

Real-World Use Case:

@Service for business logic like payment processing.

@Repository for interacting with the database.


Internal Working:

All are detected by @ComponentScan

Spring uses proxy classes to manage lifecycle and injection.

@Repository adds exception translation in Spring Data (via PersistenceExceptionTranslationPostProcessor).


Interview Insights:

Q: What's the difference between @Component and @Service? A: @Service adds semantic clarity and may include additional processing like AOP advice.



---

3. @Configuration

Purpose:

Defines a class as a source of bean definitions (Java-based configuration).

Example:

@Configuration
public class AppConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

Real-World Use Case:

Used to define beans not managed by Spring by default (like third-party library objects).

Internal Working:

Beans defined in @Configuration classes are singleton and proxied by CGLIB to ensure method calls return the same instance.


Interview Insights:

Q: Why use @Configuration over XML config? A: Type safety, readability, and IDE support.



---

4. @Bean

Purpose:

Declares a bean returned by a method in a @Configuration class.

Example:

@Bean
public ObjectMapper objectMapper() {
    return new ObjectMapper();
}

Real-World Use Case:

For third-party beans or beans requiring custom construction logic.

Internal Working:

Spring calls the factory method and manages the bean lifecycle.



---

5. @ComponentScan

Purpose:

Tells Spring where to search for annotated components.

Example:

@SpringBootApplication(scanBasePackages = "com.example")

Real-World Use Case:

Used when project structure spans multiple base packages.

Interview Insights:

Q: When do you need to explicitly use @ComponentScan? A: When your beans are not in the same package or subpackage of the main class.



---

6. @Import

Purpose:

Manually imports additional @Configuration classes.

Example:

@Configuration
@Import({SecurityConfig.class, DataSourceConfig.class})
public class MainConfig {}

Real-World Use Case:

Modular projects with different configuration classes.


---

7. @Lazy

Purpose:

Delays the initialization of a bean until it's actually needed.

Example:

@Bean
@Lazy
public ExpensiveService service() {
    return new ExpensiveService();
}

Use Case:

Speeds up application startup by loading heavy beans only when required.


---

Let me know when you're ready and I’ll continue with the next file: web-layer-annotations.md.

