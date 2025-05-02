# 📦 Encapsulation in Object-Oriented Programming (OOP)

## 🔍 What is Encapsulation?
Encapsulation is one of the four fundamental OOP principles. It is the process of **binding the data (fields)** and **the code (methods)** that manipulates that data into a single unit, called a **class**, and **restricting access** to some of the object's components.

It is often used to **hide the internal representation** of an object from the outside.

---

## ✅ Advantages of Encapsulation
- 🔐 **Data Hiding**: Prevents direct access to internal data; sensitive fields can be made private.
- 🧪 **Validation Logic**: Allows input validation before modifying fields using setters.
- 🔄 **Improved Maintainability**: Internal implementation can be changed without affecting external code.
- 🧩 **Modular Design**: Objects can be developed, tested, and understood independently.
- ✅ **Readability and Usability**: Developers know exactly how to interact with the object.

---

## 🚀 Use Case: User Authentication in Java

### Java Example:
```java
public class User {
    private String username;
    private String password;

    // Constructor
    public User(String username, String password) {
        this.username = username;
        setPassword(password); // Validation
    }

    public String getUsername() {
        return username;
    }

    // No getter for password: it is sensitive data

    public void setPassword(String password) {
        if (password.length() >= 8) {
            this.password = password;
        } else {
            throw new IllegalArgumentException("Password must be at least 8 characters long.");
        }
    }

    public boolean checkPassword(String inputPassword) {
        return this.password.equals(inputPassword);
    }
}
```

### Usage Example:
```java
User user = new User("alice", "securePass123");
System.out.println(user.getUsername());  // prints: alice
System.out.println(user.checkPassword("wrongpass"));  // false
```

---

## 📝 Summary

| Feature            | Description                                    |
|--------------------|------------------------------------------------|
| 🔐 Data Hiding      | Fields are made `private` to hide data.        |
| 📜 Getters/Setters  | Control how fields are accessed or modified.   |
| ⚙ Controlled Access | Add checks and validation in setters/getters. |
| 🧩 Modularity       | Each class controls its own data securely.     |
| 🔄 Flexibility      | Internal changes don't break external code.    |

---

## 🎨 Quick Revision (Color-coded)

- 🔵 **Encapsulation** = Data + Methods + Restricted Access
- 🔴 **Private** variables + 🟢 **Public** getters/setters = control
- 🟡 Sensitive data = No getter OR masked getter
- 🟣 Validation = In setters to ensure data integrity



# 🎭 Abstraction in Object-Oriented Programming (OOP)

## 🔍 What is Abstraction?
Abstraction is one of the core principles of OOP. It involves **hiding the complex implementation details** and showing only the **essential features** of an object or system to the user.

In Java, abstraction is achieved using:
- **Abstract classes**
- **Interfaces**

---

## ✅ Advantages of Abstraction
- 🚫 **Hides Complexity**: Focuses on *what* an object does rather than *how* it does it.
- 🔄 **Improves Modularity**: Encourages separation of responsibilities.
- 🛡️ **Enhances Security**: Users can’t access unnecessary internal details.
- 🧪 **Easier Testing**: Mock interfaces or abstract types for testing.
- 🔧 **Flexible Design**: Implementation can change without impacting users.

---

## 🚀 Use Case: Payment System Abstraction

### Abstract Class Example:
```java
public abstract class Payment {
    public abstract void pay(double amount);

    public void receipt() {
        System.out.println("Payment successful.");
    }
}
```

### Subclass Implementation:
```java
public class CreditCardPayment extends Payment {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}
```

### Interface Example:
```java
public interface PaymentMethod {
    void pay(double amount);
}
```

### Implementation:
```java
public class UpiPayment implements PaymentMethod {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI.");
    }
}
```

### Usage Example:
```java
PaymentMethod payment = new UpiPayment();
payment.pay(500.0);  // Output: Paid 500.0 using UPI.
```

---

## 📝 Summary

| Feature               | Description                                        |
|-----------------------|----------------------------------------------------|
| 🎭 Abstraction         | Focus on essential behavior, hide implementation  |
| 🧩 Abstract Class      | Can have abstract + concrete methods              |
| 🔌 Interface           | All methods are abstract (Java 7 and below)       |
| 🔄 Flexibility         | Easy to swap implementations                      |
| 🚫 Internal Complexity | Users don't see internal details                  |

---

## 🎨 Quick Revision (Color-coded)

- 🔵 **Abstraction** = Essential Features + Hide Details
- 🔴 **Abstract Classes** = Partial implementation + abstraction
- 🟢 **Interfaces** = 100% abstraction (until Java 8 default methods)
- 🟡 **Use** when multiple implementations are possible
- 🟣 **Focus** on *what* an object should do, not *how*

# 🔌 When to Use Interface vs Abstract Class in Java

## 🧩 Use an Interface When:

| Scenario | Explanation |
|----------|-------------|
| ✅ You want to define a **contract** or capability | Example: `Runnable`, `Comparable` — classes can implement multiple interfaces like "can run" or "can compare". |
| ✅ **Multiple inheritance** is needed | Java allows a class to implement multiple interfaces but extend only one class. |
| ✅ You only need **method declarations** | Interfaces are good when no base implementation is required. |
| ✅ You want to ensure **loose coupling** | Promotes more flexible and decoupled design (e.g., services implementing interfaces). |
| ✅ You want to use **Java 8+ features** | Interfaces can now have `default` and `static` methods, making them more powerful. |

📌 **Example**: `List`, `Map`, `Set` are all interfaces in Java’s Collections Framework.

---

## 🧩 Use an Abstract Class When:

| Scenario | Explanation |
|----------|-------------|
| ✅ You want to provide **common behavior or code reuse** | Abstract classes can have both implemented (concrete) and unimplemented (abstract) methods. |
| ✅ You need to define **common state/fields** | Abstract classes can have instance variables, constructors, and state. |
| ✅ You want to enforce a **template pattern** | Use abstract methods to enforce steps that subclasses must override. |
| ✅ You want to ensure **controlled inheritance** | Since you can extend only one class, this is good for strong type hierarchies. |

📌 **Example**: `HttpServlet` in Java EE is an abstract class — provides default behavior, and developers override only needed parts.

---

## 🆚 Interface vs Abstract Class Cheat Sheet

| Feature                  | Interface                        | Abstract Class                    |
|--------------------------|----------------------------------|-----------------------------------|
| Inheritance              | Multiple (via interfaces)        | Single (via extends)              |
| Methods                  | Abstract + default (Java 8+)     | Abstract + concrete               |
| Fields                   | `public static final` only       | Instance variables allowed        |
| Constructors             | ❌ Not allowed                   | ✅ Allowed                         |
| Access Modifiers         | Public methods only (until Java 9) | Any (private/protected/public) |
| Use case                | Capability-based, contract        | Base class, partial implementation|

---

## 🧠 Quick Rule of Thumb

> 🔸 Use **interface** when behavior is **common across unrelated classes**.  
> 🔹 Use **abstract class** when behavior is **shared within a family of related classes**.




# 🧬 Inheritance in Object-Oriented Programming (OOP)

## 🔍 What is Inheritance?
Inheritance is another core principle of OOP. It allows a class (**child/subclass**) to **inherit fields and methods** from another class (**parent/superclass**), promoting **code reusability and hierarchy**.

It supports the concept of "**is-a**" relationship.

---

## ✅ Advantages of Inheritance
- 🔁 **Code Reusability**: Reuse existing logic from parent classes without rewriting.
- 🧩 **Modular Design**: Helps organize classes in a hierarchical manner.
- ⬆️ **Extensibility**: New functionality can be added to existing code.
- 🔄 **Maintainability**: Common logic centralized in the base class reduces duplication.
- 🧠 **Polymorphism Support**: Enables dynamic method dispatch at runtime.

---

## 🚀 Use Case: Employee Hierarchy in Java

### Java Example:
```java
// Parent Class
public class Employee {
    protected String name;
    protected int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public void displayInfo() {
        System.out.println("Name: " + name + ", ID: " + id);
    }
}

// Child Class
public class Manager extends Employee {
    private int teamSize;

    public Manager(String name, int id, int teamSize) {
        super(name, id);
        this.teamSize = teamSize;
    }

    public void displayManagerInfo() {
        displayInfo(); // Inherited method
        System.out.println("Team Size: " + teamSize);
    }
}
```

### Usage Example:
```java
Manager mgr = new Manager("Bob", 101, 5);
mgr.displayManagerInfo();
```

---

## 📝 Summary

| Feature             | Description                                      |
|---------------------|--------------------------------------------------|
| 🧬 "is-a" Relation   | Subclass is a specialized form of superclass     |
| 📦 Code Reuse       | Inherits fields and methods                      |
| 🔄 Override Methods | Subclass can override superclass methods         |
| ⬆️ Extendable Design | Add extra features without changing parent class |
| 🔁 Super Keyword     | Call superclass constructor or methods           |

---

## 🎨 Quick Revision (Color-coded)

- 🔵 **Inheritance** = Reuse + Extend existing classes
- 🔴 **Super** = Access parent class constructor/methods
- 🟢 **Override** = Customize inherited behavior
- 🟡 **Protected** = Visible in subclass, hidden from outside
- 🟣 **"is-a"** = Subclass relationship with superclass
