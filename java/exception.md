
# Exception Handling in Java

## 1. Exception Handling Overview
Exception handling in Java is a mechanism to handle runtime errors, ensuring the program's normal flow. It uses a combination of `try`, `catch`, `finally`, `throw`, and `throws` constructs.

### Key Points:
- **Checked Exceptions**: Exceptions checked at compile-time. Must be handled or declared in the method signature (e.g., `IOException`).
- **Unchecked Exceptions**: Exceptions checked at runtime. Extends `RuntimeException` (e.g., `NullPointerException`).

---

## 2. Creating a Custom Checked Exception

### Steps:
1. Extend the `Exception` class.
2. Add constructors for custom messages or additional fields.
3. Throw the exception using the `throw` keyword.
4. Handle it using a `try-catch` block or declare it in the `throws` clause.

### Example:
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class CustomCheckedExceptionDemo {
    public static void validateAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be at least 18.");
        }
        System.out.println("Age is valid: " + age);
    }

    public static void main(String[] args) {
        try {
            validateAge(16);
        } catch (InvalidAgeException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
```

---

## 3. Creating a Custom Unchecked Exception

### Steps:
1. Extend the `RuntimeException` class.
2. Add constructors for custom messages or additional fields.
3. Use the exception without declaring it in the `throws` clause.

### Example:
```java
class InsufficientBalanceException extends RuntimeException {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

public class CustomUncheckedExceptionDemo {
    private double balance;

    public CustomUncheckedExceptionDemo(double initialBalance) {
        this.balance = initialBalance;
    }

    public void withdraw(double amount) {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient balance.");
        }
        balance -= amount;
        System.out.println("Withdrawal successful. Remaining balance: " + balance);
    }

    public static void main(String[] args) {
        CustomUncheckedExceptionDemo account = new CustomUncheckedExceptionDemo(500);
        try {
            account.withdraw(600);
        } catch (InsufficientBalanceException e) {
            System.out.println("Exception Caught: " + e.getMessage());
        }
    }
}
```

---

## 4. Converting Exceptions

### Converting a Checked Exception to an Unchecked Exception:
1. Catch the checked exception.
2. Wrap it in a `RuntimeException` and re-throw it.

#### Example:
```java
import java.io.*;

public class CheckedToUnchecked {
    public static void readFile(String fileName) {
        try {
            FileReader file = new FileReader(fileName);
        } catch (FileNotFoundException e) {
            throw new RuntimeException("File not found: " + fileName, e);
        }
    }

    public static void main(String[] args) {
        readFile("non_existent_file.txt");
    }
}
```

### Converting an Unchecked Exception to a Checked Exception:
1. Catch the unchecked exception.
2. Wrap it in a custom checked exception and re-throw it.

#### Example:
```java
class CustomCheckedException extends Exception {
    public CustomCheckedException(String message, Throwable cause) {
        super(message, cause);
    }
}

public class UncheckedToChecked {
    public static void process(int value) throws CustomCheckedException {
        try {
            int result = 10 / value;
        } catch (ArithmeticException e) {
            throw new CustomCheckedException("Division by zero occurred", e);
        }
    }

    public static void main(String[] args) {
        try {
            process(0);
        } catch (CustomCheckedException e) {
            System.out.println("Caught checked exception: " + e.getMessage());
        }
    }
}
```

---

## 5. Summary

### Key Points to Remember:
1. **Checked Exceptions**:
   - Extend `Exception`.
   - Force handling or declaration using `throws`.
   - Example: `IOException`, `SQLException`.

2. **Unchecked Exceptions**:
   - Extend `RuntimeException`.
   - No compile-time checks required.
   - Example: `ArithmeticException`, `NullPointerException`.

3. **Custom Exceptions**:
   - Use meaningful names to represent domain-specific errors.
   - Checked: Extend `Exception`.
   - Unchecked: Extend `RuntimeException`.

4. **Conversion**:
   - Checked to Unchecked: Wrap in `RuntimeException`.
   - Unchecked to Checked: Wrap in a custom `Exception`.

By understanding these concepts, you’ll be well-prepared to handle questions on exception handling in Java during interviews.
