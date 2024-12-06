
# SOLID Principles in Employee Management System

## 1. Single Responsibility Principle (SRP)

**Scenario**: A class should have only one reason to change. Let's separate the employee data management and file operations.

### Bad Example:
```java
class Employee {
    private String name;
    private int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public void saveToFile() {
        // Code to save employee data to file
    }
}
```

Here, the `Employee` class is doing more than one thing: managing employee data and handling file operations.

### Good Example:
```java
class Employee {
    private String name;
    private int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    // Getters and other business logic methods
}

class EmployeeFileOperations {
    public void saveToFile(Employee employee) {
        // Code to save employee data to a file
    }
}
```

Now, the `Employee` class is focused only on employee-related data, while the `EmployeeFileOperations` class handles file saving.

---

## 2. Open/Closed Principle (OCP)

**Scenario**: A class should be open for extension but closed for modification. You should be able to add new functionality without changing the existing code.

### Bad Example:
```java
class EmployeeReport {
    public void generateReport(Employee employee, String reportType) {
        if (reportType.equals("TEXT")) {
            // Generate text report
        } else if (reportType.equals("PDF")) {
            // Generate PDF report
        }
    }
}
```

Here, you have to modify the `EmployeeReport` class every time you want to support a new report type.

### Good Example:
```java
interface ReportGenerator {
    void generateReport(Employee employee);
}

class TextReportGenerator implements ReportGenerator {
    public void generateReport(Employee employee) {
        // Generate text report
    }
}

class PDFReportGenerator implements ReportGenerator {
    public void generateReport(Employee employee) {
        // Generate PDF report
    }
}

class EmployeeReport {
    public void generateReport(Employee employee, ReportGenerator generator) {
        generator.generateReport(employee);
    }
}
```

Now, `EmployeeReport` is open for extension (you can add new report generators), but closed for modification.

---

## 3. Liskov Substitution Principle (LSP)

**Scenario**: Subtypes should be substitutable for their base types without altering the correctness of the program.

### Bad Example:
```java
class Employee {
    public double calculateBonus() {
        return 1000;
    }
}

class ContractEmployee extends Employee {
    @Override
    public double calculateBonus() {
        throw new UnsupportedOperationException("Contract employees don't get bonuses");
    }
}
```

This violates LSP because `ContractEmployee` cannot be used in place of `Employee`.

### Good Example:
```java
abstract class Employee {
    public abstract double calculateBonus();
}

class FullTimeEmployee extends Employee {
    public double calculateBonus() {
        return 1000;
    }
}

class ContractEmployee extends Employee {
    public double calculateBonus() {
        return 0; // Contract employees get no bonus.
    }
}
```

Now, `FullTimeEmployee` and `ContractEmployee` can both be used as `Employee` without causing issues.

---

## 4. Interface Segregation Principle (ISP)

**Scenario**: No client should be forced to depend on methods it does not use.

### Bad Example:
```java
interface Employee {
    void writeCode();
    void manageTeam();
}

class Developer implements Employee {
    public void writeCode() {
        // Developer writes code
    }

    public void manageTeam() {
        throw new UnsupportedOperationException("Developers don't manage teams");
    }
}

class Manager implements Employee {
    public void writeCode() {
        throw new UnsupportedOperationException("Managers don't write code");
    }

    public void manageTeam() {
        // Manager manages the team
    }
}
```

Here, both `Developer` and `Manager` are forced to implement methods they don’t use.

### Good Example:
```java
interface Coder {
    void writeCode();
}

interface TeamLead {
    void manageTeam();
}

class Developer implements Coder {
    public void writeCode() {
        // Developer writes code
    }
}

class Manager implements TeamLead {
    public void manageTeam() {
        // Manager manages the team
    }
}
```

Now, both the `Developer` and `Manager` implement only the relevant interfaces.

---

## 5. Dependency Inversion Principle (DIP)

**Scenario**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

### Bad Example:
```java
class EmployeeDatabase {
    public Employee getEmployee(int id) {
        // Code to fetch employee from the database
        return new Employee();
    }
}

class EmployeeService {
    private EmployeeDatabase database;

    public EmployeeService() {
        this.database = new EmployeeDatabase();
    }

    public Employee getEmployeeDetails(int id) {
        return database.getEmployee(id);
    }
}
```

Here, `EmployeeService` depends on the concrete `EmployeeDatabase`, violating DIP.

### Good Example:
```java
interface EmployeeRepository {
    Employee getEmployee(int id);
}

class EmployeeDatabase implements EmployeeRepository {
    public Employee getEmployee(int id) {
        // Code to fetch employee from the database
        return new Employee();
    }
}

class EmployeeService {
    private EmployeeRepository repository;

    public EmployeeService(EmployeeRepository repository) {
        this.repository = repository;
    }

    public Employee getEmployeeDetails(int id) {
        return repository.getEmployee(id);
    }
}
```

Now, `EmployeeService` depends on the abstraction `EmployeeRepository` rather than the concrete `EmployeeDatabase`.

---

## Conclusion:
- **SRP**: Separate responsibilities into different classes (e.g., `Employee` and `EmployeeFileOperations`).
- **OCP**: Extend the system (e.g., add new report generators) without modifying existing code.
- **LSP**: Ensure subclasses (e.g., `FullTimeEmployee`, `ContractEmployee`) can be used as their base class (`Employee`) without errors.
- **ISP**: Break down interfaces so that classes implement only the methods they need (e.g., `Coder`, `TeamLead`).
- **DIP**: Depend on abstractions rather than concrete classes (e.g., `EmployeeRepository`).
