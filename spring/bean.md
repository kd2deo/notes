# Spring Bean Life Cycle – Complete Guide with Business Use Cases

_Last updated: 2025-05-23_

---

## 🌱 1. Bean Instantiation

### What happens:
Spring creates an instance of the bean using a constructor or factory method.

### ✅ When to use:
- When you need **default values** or **object creation logic** early on.

### 🔄 Real-World Example:
```java
@Component
public class EmailConfig {
    public EmailConfig() {
        System.out.println("Default EmailConfig created");
    }
}
```

---

## 🧩 2. Dependency Injection

### What happens:
Spring injects values or dependencies via `@Autowired`, `@Value`, or constructor injection.

### ✅ When to use:
- Wire services, configurations, or resources required by your bean.

### 🔄 Real-World Example:
```java
@Component
public class EmailService {
    @Autowired
    private EmailConfig config;
}
```

---

## 🧠 3. Aware Interfaces

### What happens:
Bean gets access to Spring container-specific information like `ApplicationContext`.

### ✅ When to use:
- When you need access to context, environment, or bean name.

### 🔄 Real-World Example:
```java
@Component
public class AppMetadata implements ApplicationContextAware {
    public void setApplicationContext(ApplicationContext ctx) {
        String appName = ctx.getEnvironment().getProperty("spring.application.name");
        System.out.println("App name: " + appName);
    }
}
```

---

## ⚙️ 4. BeanPostProcessor

### What happens:
Intercepts beans before and after initialization.

### ✅ When to use:
- Apply logging, validation, profiling, or wrap with proxy logic.

### 🔄 Real-World Example:
```java
@Component
public class AuditLoggerProcessor implements BeanPostProcessor {
    public Object postProcessBeforeInitialization(Object bean, String name) {
        if (bean instanceof Auditable) {
            System.out.println("Auditable bean: " + name);
        }
        return bean;
    }
}
```

---

## 🧼 5. Initialization (`@PostConstruct`, `InitializingBean`, init-method)

### What happens:
Custom logic executed after dependencies are set.

### ✅ When to use:
- Run startup logic like data load, validation, or setup tasks.

### 🔄 Real-World Example:
```java
@Component
public class StartupCacheLoader {
    @Autowired
    private CacheService cacheService;

    @PostConstruct
    public void loadCache() {
        cacheService.loadInitialData();
    }
}
```

---

## ⏹️ 6. Destruction (`@PreDestroy`, `DisposableBean`, destroy-method)

### What happens:
Custom cleanup before bean is destroyed.

### ✅ When to use:
- Close connections, stop threads, flush data.

### 🔄 Real-World Example:
```java
@Component
public class MessageQueueListener {
    @PreDestroy
    public void stop() {
        System.out.println("Stopping queue listener");
    }
}
```

---

## 🏗️ Bean Scope Use Cases

| Scope        | Use Case                      | Lifecycle Support       |
|--------------|-------------------------------|--------------------------|
| Singleton    | Services, Config Beans         | Full lifecycle support   |
| Prototype    | PDFGenerator, FileExporter     | No destroy callback      |
| Request      | UserSessionBean                | Per HTTP request         |
| Session      | Cart, LoginInfo                | Per user session         |
| Application  | App-wide metrics/logs          | Per application context  |
| Websocket    | Chat session handling          | Per websocket session    |

---

## 🧪 Interview Questions & Answers

### Q1: What are the stages of Spring Bean life cycle?
**A:** Instantiation → Dependency Injection → Awareness → PostProcess Before Init → Initialization → PostProcess After Init → Destruction

### Q2: Difference between `@PostConstruct` and `InitializingBean`?
**A:** `@PostConstruct` is annotation-based and more readable. `InitializingBean` is interface-based.

### Q3: What is the use of `BeanPostProcessor`?
**A:** It allows interception of bean creation logic. Useful for AOP, logging, validation, etc.

### Q4: When is `@PreDestroy` not called?
**A:** For prototype-scoped beans, Spring does not manage destruction.

### Q5: How does Spring manage resources?
**A:** Through initialization (`@PostConstruct`) and destruction (`@PreDestroy`) callbacks.

### Q6: Can you access `ApplicationContext` from a bean?
**A:** Yes, by implementing `ApplicationContextAware`.

### Q7: Why use `@Lazy` in Spring?
**A:** To delay bean initialization until it is actually needed.

---

**Prepared by: ChatGPT**  
**For: Spring Boot Interview Preparation**
