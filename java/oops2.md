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
