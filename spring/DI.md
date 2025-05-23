
# Dependency Injection in Spring Boot

## 1. Constructor Injection — Recommended (Most Preferred)

### ✅ Use Case: Mandatory dependency (cannot work without it)

**Example:** A service that processes payments, which must have a `PaymentGateway`.

```java
@Service
public class PaymentService {
    private final PaymentGateway paymentGateway;

    @Autowired
    public PaymentService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public void processPayment(Order order) {
        paymentGateway.charge(order);
    }
}
```

### ✔ Advantages:
- Ensures dependencies are not null.
- Promotes immutability.
- Good for unit testing (use constructor args in test).
- Catches circular dependencies early (compile-time).

### ❌ Disadvantages:
- More boilerplate when many dependencies (solved with Lombok `@RequiredArgsConstructor`).

---

## 2. Setter Injection — Optional Dependency or Reconfigurable Later

### ✅ Use Case: Notification service is optional, only needed if enabled in config.

```java
@Service
public class ReportService {
    private NotificationService notificationService;

    @Autowired
    public void setNotificationService(NotificationService notificationService) {
        this.notificationService = notificationService;
    }

    public void generate() {
        if (notificationService != null) {
            notificationService.sendReportGeneratedNotification();
        }
    }
}
```

### ✔ Advantages:
- Allows optional dependencies.
- Can re-inject or reconfigure at runtime.

### ❌ Disadvantages:
- Risk of NullPointerException if method not called.
- More verbose, less readable than constructor injection.

---

## 3. Field Injection — Not Recommended

### ✅ Use Case: Might see it in older code or quick prototypes.

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
```

### ❌ Disadvantages:
- Hidden dependencies (not visible in constructor).
- Harder to test (requires reflection or Spring context).
- Violates Dependency Inversion Principle (DIP).
- Increases risk of circular dependency issues.

---

## Real-World Circular Dependency Example

```java
@Service
public class OrderService {
    private final InventoryService inventoryService;

    @Autowired
    public OrderService(InventoryService inventoryService) {
        this.inventoryService = inventoryService;
    }
}

@Service
public class InventoryService {
    private final OrderService orderService;

    @Autowired
    public InventoryService(OrderService orderService) {
        this.orderService = orderService;
    }
}
```

### ❗ Problem:
- Constructor injection will fail with `BeanCurrentlyInCreationException`.

### ✅ Solution:
Use setter injection in one of the services or add `@Lazy`.

```java
@Autowired
public InventoryService(@Lazy OrderService orderService) {
    this.orderService = orderService;
}
```

---

## Quick Summary: When to Use Which?

| Injection Type     | When to Use                                            | Testability | Circular Dependency Safe | Best Practice |
|--------------------|--------------------------------------------------------|-------------|---------------------------|----------------|
| Constructor        | Mandatory dependencies, immutability, clean code       | ✅ Easy     | ❌ Fails fast              | ✅ Yes         |
| Setter             | Optional/replacable dependencies                        | ✅ OK       | ✅ Safer                   | ⚠ Sometimes   |
| Field              | Legacy code, quick prototypes                          | ❌ Hard     | ❌ Hidden, risky           | ❌ Avoid       |

---

## Interview-Oriented Case Study: E-commerce System

### Components and DI strategy:

- `OrderController` - constructor injection (recommended)
- `OrderService` - constructor injection (recommended)
- `NotificationService` - setter injection (optional)
- `PaymentGateway` - constructor injection (required)

```java
@RestController
public class OrderController {

    private final OrderService orderService;

    @Autowired
    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @PostMapping("/order")
    public ResponseEntity<?> placeOrder(@RequestBody Order order) {
        orderService.placeOrder(order);
        return ResponseEntity.ok("Order placed");
    }
}
```

---

## Sample Interview Questions Based on This:

### 1. Why is constructor injection preferred over field injection?
**Answer:** Constructor injection makes dependencies explicit, promotes immutability, is easier to unit test, and fails fast on circular dependencies.

### 2. Give a use case where setter injection is appropriate.
**Answer:** When a dependency is optional or configurable at runtime, such as a notification service that is conditionally used.

### 3. What problems does field injection create in a large codebase?
**Answer:** It hides dependencies, breaks the Dependency Inversion Principle, complicates testing, and increases the likelihood of circular dependencies.

### 4. What’s the impact of DI on unit testing?
**Answer:** DI (especially constructor-based) allows for easier mocking of dependencies, making unit testing more straightforward.

### 5. How can you solve circular dependency errors in Spring?
**Answer:** Use `@Lazy` annotation or switch to setter injection for one of the beans involved.

### 6. Explain how Spring resolves multiple beans of the same type.
**Answer:** You can use `@Qualifier` or `@Primary` to specify which bean to inject when multiple candidates exist.

### 7. In which scenario would you use `@Lazy` with DI?
**Answer:** To break a circular dependency or to delay initialization of a heavy bean until it’s actually needed.

---
