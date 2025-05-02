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
