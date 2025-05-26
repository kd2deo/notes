
# Spring Boot Core Annotations — Deep Dive with Business Use Cases

This document provides an in-depth look into **Spring Boot core annotations**, real-world **business logic use cases**, **interview insights**, and best practices. It covers both foundational and advanced annotations.

---

## 🔹 `@SpringBootApplication`

### ✅ What It Does:
Marks the main class of a Spring Boot application. Combines:
- `@Configuration`
- `@EnableAutoConfiguration`
- `@ComponentScan`

### 🧾 Syntax:
```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

### 🎯 Business Use Case:
Acts as the entry point for starting the application, enabling component scanning and auto-configuration.

### 🧠 Interview Insight:
**Q:** What does `@SpringBootApplication` do internally?
**A:** It’s a meta-annotation that combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.

---

## 🔹 `@Component`, `@Service`, `@Repository`, `@Controller`

### ✅ What They Do:
Declare Spring-managed components:
- `@Component`: Generic bean
- `@Service`: Business logic
- `@Repository`: Data access layer
- `@Controller`: MVC Controller (HTML)
- `@RestController`: RESTful web services

### 🎯 Business Use Case:
In a banking app:
- `@Service`: Handles fund transfer logic
- `@Repository`: Performs DB operations
- `@RestController`: Exposes REST API for transactions

### 🧠 Interview Insight:
**Q:** What's the difference between `@Component` and `@Service`?
**A:** Functionally the same, but `@Service` adds semantic clarity and may be used in AOP.

---

## 🔹 `@Autowired`

### ✅ What It Does:
Performs dependency injection.

### 🧾 Syntax:
```java
@Service
public class OrderService {
    private final PaymentService paymentService;

    @Autowired
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### 🎯 Business Use Case:
Inject `PaymentService` into `OrderService` to handle online payment processing.

### 🧠 Interview Insight:
Constructor injection is preferred over field injection for testability and immutability.

---

## 🔹 `@Value`

### ✅ What It Does:
Injects values from property files.

### 🧾 Syntax:
```java
@Value("${app.name}")
private String appName;
```

### 🎯 Business Use Case:
Inject version or title from application properties to be shown in a UI header.

### 🧠 Interview Insight:
Use `@Value` for simple values, but prefer `@ConfigurationProperties` for structured config.

---

## 🔹 `@ConfigurationProperties`

### ✅ What It Does:
Binds external configuration to POJO fields.

### 🧾 Syntax:
```yaml
app:
  name: FinanceService
  maxUsers: 100
```

```java
@ConfigurationProperties(prefix = "app")
@Component
public class AppProperties {
    private String name;
    private int maxUsers;
    // getters/setters
}
```

### 🎯 Business Use Case:
In a multi-tenant SaaS app, configurations differ for customer tiers (e.g., user limits, features).

### 🧠 Interview Insight:
More scalable and type-safe than `@Value`.

---

## 🔹 `@Profile`

### ✅ What It Does:
Activates a bean only if the specified profile is active.

### 🧾 Syntax:
```java
@Profile("dev")
@Component
public class DevEmailService implements EmailService {
    public void sendEmail(String to) {
        System.out.println("Sending DEV email to: " + to);
    }
}
```

```java
@Profile("prod")
@Component
public class SmtpEmailService implements EmailService {
    public void sendEmail(String to) {
        // Real SMTP
    }
}
```

### 🎯 Business Use Case:
Use mock email sender in development, real SMTP sender in production.

### 🧠 Interview Insight:
Enables **environment-specific logic** without changing code.

---

## 🔹 `@ConditionalOnProperty`

### ✅ What It Does:
Controls bean registration based on property values.

### 🧾 Syntax:
```yaml
feature.sms.enabled: true
```
```java
@Bean
@ConditionalOnProperty(name = "feature.sms.enabled", havingValue = "true")
public SmsService smsService() {
    return new SmsService();
}
```

### 🎯 Business Use Case:
Enable/disable features like SMS alerts per region or user type.

### 🧠 Interview Insight:
Used for **feature toggling** and **A/B testing**.

---

## 🔹 `@RestController`, `@GetMapping`, `@PostMapping`

### ✅ What They Do:
Expose HTTP endpoints.

### 🧾 Syntax:
```java
@RestController
@RequestMapping("/api/products")
public class ProductController {

    @GetMapping
    public List<Product> getAll() {
        return productService.findAll();
    }

    @PostMapping
    public Product create(@RequestBody Product product) {
        return productService.save(product);
    }
}
```

### 🎯 Business Use Case:
Expose product listing and creation APIs for e-commerce frontend/mobile apps.

### 🧠 Interview Insight:
`@RestController` = `@Controller` + `@ResponseBody`.

---

## 🔹 `@EnableScheduling` & `@Scheduled`

### ✅ What They Do:
Run background tasks at fixed intervals or cron schedules.

### 🧾 Syntax:
```java
@EnableScheduling
@Configuration
public class ScheduleConfig {}

@Component
public class ReportJob {

    @Scheduled(cron = "0 0 8 * * MON-FRI")
    public void generateReport() {
        // Generate daily report
    }
}
```

### 🎯 Business Use Case:
Send daily performance reports or clean old data periodically.

### 🧠 Interview Insight:
Support `fixedRate`, `fixedDelay`, and cron expressions.

---

## 🔹 `@SpringBootTest`

### ✅ What It Does:
Bootstraps entire Spring context for integration testing.

### 🧾 Syntax:
```java
@SpringBootTest
public class ProductServiceTest {

    @Autowired
    private ProductService productService;

    @Test
    void shouldReturnAllProducts() {
        List<Product> products = productService.getAll();
        assertFalse(products.isEmpty());
    }
}
```

### 🎯 Business Use Case:
Verify that the full Spring context loads and beans are wired correctly.

### 🧠 Interview Insight:
**Q:** When to use `@SpringBootTest` vs unit test?
**A:** Use for end-to-end or integration testing when multiple beans or DB are involved.

---

## ✅ Summary Table

| Annotation               | Type           | Business Scenario                                    | Common Pairing                             |
|--------------------------|----------------|-----------------------------------------------------|---------------------------------------------|
| `@SpringBootApplication`| Bootstrapping  | App entry point, configuration                      | `SpringApplication.run()`                   |
| `@Component/@Service`   | Bean Mgmt      | Generic or service layer components                 | DI annotations like `@Autowired`            |
| `@Value`                | Config         | Inject simple values                                | `application.properties`                    |
| `@ConfigurationProperties` | Config     | Structured config binding                           | `@Component`, `@Enable...`                  |
| `@Profile`              | Env-specific   | Dev vs Prod logic                                   | `@Service`, `@Repository`                   |
| `@ConditionalOnProperty` | Feature Toggle| Enable/disable features                             | Spring config properties                    |
| `@RestController`       | API            | Expose REST endpoints                               | `@RequestMapping`, `@PostMapping`           |
| `@Scheduled`            | Scheduling     | Background jobs                                     | `@EnableScheduling`                         |
| `@SpringBootTest`       | Testing        | Integration testing                                 | JUnit, Mockito                              |

---

