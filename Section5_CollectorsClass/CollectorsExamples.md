
```java

class Employee {
    private String name;
    private int age;
    private int salary;
    private String department;
    private int employeeId;
    private String email;
    private String gender;

    // Default constructor
    public Employee() {
    }

    // Parameterized constructor
    public Employee(String name, int age, int salary, String department, int employeeId, String email, String gender) {
        this.name = name;
        this.age = age;
        this.salary = salary;
        this.department = department;
        this.employeeId = employeeId;
        this.email = email;
        this.gender = gender;
    }

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    public int getEmployeeId() {
        return employeeId;
    }

    public void setEmployeeId(int employeeId) {
        this.employeeId = employeeId;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getGender() {
        return this.gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Employee Details:\n" +
                "  Name       : " + name + "\n" +
                "  Age        : " + age + "\n" +
                "  Salary     : " + salary + "\n" +
                "  Department : " + department + "\n" +
                "  EmployeeId : " + employeeId + "\n" +
                "  Email      : " + email + "\n" +
                "  Gender      : " + gender + "\n";
    }

}


public class StreamAPIDemo1 {

    public static void main(String[] args) {

        List<Employee> employees = getAllEmployees();


    }

    public static List<Employee> getAllEmployees() {
        List<Employee> employees = new ArrayList<>();


        employees.add(new Employee("Alice Johnson", 28, 55000, "HR", 101, "alice.johnson@example.com", "Female"));
        employees.add(new Employee("Bob Smith", 35, 72000, "Finance", 102, "bob.smith@example.com", "Male"));
        employees.add(new Employee("Charlie Brown", 40, 68000, "IT", 103, "charlie.brown@example.com", "Male"));
        employees.add(new Employee("Diana Prince", 30, 80000, "Marketing", 104, "diana.prince@example.com", "Female"));
        employees.add(new Employee("Ethan Hunt", 45, 95000, "Operations", 105, "ethan.hunt@example.com", "Male"));
        employees.add(new Employee("Fiona Davis", 26, 48000, "Sales", 106, "fiona.davis@example.com", "Female"));
        employees.add(new Employee("George Miller", 50, 105000, "Management", 107, "george.miller@example.com", "Male"));
        employees.add(new Employee("Hannah Lee", 32, 67000, "Design", 108, "hannah.lee@example.com", "Female"));
        employees.add(new Employee("Ian Clark", 29, 60000, "Support", 109, "ian.clark@example.com", "Male"));
        employees.add(new Employee("Julia Roberts", 38, 85000, "Legal", 110, "julia.roberts@example.com", "Female"));
        employees.add(new Employee("Kevin Turner", 27, 52000, "HR", 111, "kevin.turner@example.com", "Male"));
        employees.add(new Employee("Laura Scott", 33, 75000, "Finance", 112, "laura.scott@example.com", "Female"));
        employees.add(new Employee("Michael Adams", 41, 90000, "IT", 113, "michael.adams@example.com", "Male"));
        employees.add(new Employee("Nina Patel", 36, 78000, "Marketing", 114, "nina.patel@example.com", "Female"));
        employees.add(new Employee("Oscar White", 44, 97000, "Operations", 115, "oscar.white@example.com", "Male"));


        return employees;

    }

}
```

# Collector Class - Practical Examples

> **Dataset Used:** `List<Employee> employees = getAllEmployees();`

These examples demonstrate the most commonly used methods of the **Collectors** class using the provided `Employee` class.

---

# Example 1: Collect Employees into a List (toList)

### Objective

Collect all employees into a new `List`.

```java
List<Employee> employeeList = employees.stream()
        .collect(Collectors.toList());

employeeList.forEach(System.out::println);
```

### Explanation

- Collects all stream elements into a `List`.
- Order of elements is preserved.

---

# Example 2: Collect Unique Departments (mapping + toSet)

### Objective

Collect all unique department names.

```java
Set<String> departments = employees.stream()
        .map(Employee::getDepartment)
        .collect(Collectors.toSet());

System.out.println(departments);
```

### Output

```
[HR, Finance, IT, Marketing, Operations, Sales, Management, Design, Support, Legal]
```

### Explanation

- `map()` extracts department names.
- `toSet()` removes duplicate values automatically.

---

# Example 3: Collect Employee Names into a LinkedList (toCollection)

### Objective

Store employee names inside a `LinkedList`.

```java
LinkedList<String> employeeNames = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.toCollection(LinkedList::new));

System.out.println(employeeNames);
```

### Explanation

Instead of the default `ArrayList`, a `LinkedList` is created.

---

# Example 4: Convert Employees into a Map (toMap)

### Objective

Use Employee ID as key and Employee object as value.

```java
Map<Integer, Employee> employeeMap = employees.stream()
        .collect(Collectors.toMap(
                Employee::getEmployeeId,
                employee -> employee
        ));

employeeMap.forEach((id, emp) ->
        System.out.println(id + " -> " + emp.getName()));
```

### Explanation

Creates a map like

```
101 -> Alice Johnson
102 -> Bob Smith
103 -> Charlie Brown
```

Employee ID becomes the key.

---

# Example 5: Handle Duplicate Keys in toMap()

### Objective

Resolve duplicate keys using a merge function.

```java
List<Employee> list = new ArrayList<>(employees);

list.add(new Employee(
        "Duplicate Employee",
        30,
        60000,
        "IT",
        101,
        "duplicate@example.com",
        "Male"
));

Map<Integer, Employee> map = list.stream()
        .collect(Collectors.toMap(
                Employee::getEmployeeId,
                employee -> employee,
                (existing, duplicate) -> existing
        ));

System.out.println(map.get(101));
```

### Explanation

Without the merge function, Java throws:

```
IllegalStateException: Duplicate key
```

The merge function decides which value to keep.

---

# Example 6: Join All Employee Names (joining)

### Objective

Create one comma-separated String.

```java
String names = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.joining(", "));

System.out.println(names);
```

### Output

```
Alice Johnson, Bob Smith, Charlie Brown...
```

### Explanation

Useful for generating reports or CSV-like output.

---

# Example 7: Count Employees (counting)

### Objective

Count total employees.

```java
long totalEmployees = employees.stream()
        .collect(Collectors.counting());

System.out.println(totalEmployees);
```

### Output

```
15
```

### Explanation

Returns the total number of elements.

---

# Example 8: Calculate Total Salary (summingInt)

### Objective

Find the total salary of all employees.

```java
int totalSalary = employees.stream()
        .collect(Collectors.summingInt(Employee::getSalary));

System.out.println(totalSalary);
```

### Explanation

Adds every employee's salary and returns the total.

---

# Example 9: Find Average Salary (averagingInt)

### Objective

Calculate the average employee salary.

```java
double averageSalary = employees.stream()
        .collect(Collectors.averagingInt(Employee::getSalary));

System.out.println(averageSalary);
```

### Explanation

Returns the average salary as a `double`.

---

# Example 10: Get Salary Statistics (summarizingInt)

### Objective

Get count, sum, average, minimum, and maximum salary.

```java
IntSummaryStatistics statistics = employees.stream()
        .collect(Collectors.summarizingInt(Employee::getSalary));

System.out.println(statistics);
```

### Sample Output

```
count=15
sum=1127000
min=48000
average=75133.33
max=105000
```

### Explanation

One collector provides multiple statistics in a single object.

---

# Example 11: Group Employees by Department (groupingBy)

### Objective

Group employees based on department.

```java
Map<String, List<Employee>> departmentWiseEmployees =
        employees.stream()
                .collect(Collectors.groupingBy(Employee::getDepartment));

departmentWiseEmployees.forEach((department, employeeList) -> {

    System.out.println("\nDepartment : " + department);

    employeeList.forEach(employee ->
            System.out.println(employee.getName()));
});
```

### Explanation

Employees working in the same department are grouped together.

Example

```
IT
    Charlie Brown
    Michael Adams

Finance
    Bob Smith
    Laura Scott
```

---

# Example 12: Count Employees in Each Department

### Objective

Find how many employees belong to each department.

```java
Map<String, Long> employeeCount =
        employees.stream()
                .collect(Collectors.groupingBy(
                        Employee::getDepartment,
                        Collectors.counting()
                ));

System.out.println(employeeCount);
```

### Sample Output

```
{
HR=2,
Finance=2,
IT=2,
Marketing=2,
Operations=2,
Sales=1,
Management=1,
Design=1,
Support=1,
Legal=1
}
```

### Explanation

Uses two collectors together:

- `groupingBy()`
- `counting()`

---

# Example 13: Partition Employees Based on Salary (partitioningBy)

### Objective

Separate employees earning at least ₹75,000.

```java
Map<Boolean, List<Employee>> result =
        employees.stream()
                .collect(Collectors.partitioningBy(
                        employee -> employee.getSalary() >= 75000
                ));

System.out.println("Salary >= 75000");

result.get(true)
        .forEach(employee ->
                System.out.println(employee.getName()));

System.out.println("\nSalary < 75000");

result.get(false)
        .forEach(employee ->
                System.out.println(employee.getName()));
```

### Explanation

Creates exactly two groups:

- `true`
- `false`

Unlike `groupingBy()`, only two groups are created.

---

# Example 14: Group Employees by Gender

### Objective

Group employees based on gender.

```java
Map<String, List<Employee>> genderWiseEmployees =
        employees.stream()
                .collect(Collectors.groupingBy(Employee::getGender));

genderWiseEmployees.forEach((gender, employeeList) -> {

    System.out.println("\n" + gender);

    employeeList.forEach(employee ->
            System.out.println(employee.getName()));
});
```

### Explanation

Groups employees into:

```
Male

Female
```

A common real-world use case for report generation.

---

# Example 15: Find Highest Salary Employee in Each Department

### Objective

Find the highest-paid employee from every department.

```java
Map<String, Optional<Employee>> highestPaidEmployee =
        employees.stream()
                .collect(Collectors.groupingBy(
                        Employee::getDepartment,
                        Collectors.maxBy(
                                Comparator.comparing(Employee::getSalary)
                        )
                ));

highestPaidEmployee.forEach((department, employee) ->

        System.out.println(
                department + " -> " +
                employee.get().getName() +
                " : " +
                employee.get().getSalary()
        )
);
```

### Sample Output

```
Finance -> Laura Scott : 75000
IT -> Michael Adams : 90000
HR -> Alice Johnson : 55000
```

### Explanation

This combines:

- `groupingBy()`
- `maxBy()`
- `Comparator`

A very common interview and real-world scenario.

---

# Summary Table

| Example | Collector Used | Purpose |
|----------|----------------|---------|
| 1 | `toList()` | Collect into List |
| 2 | `toSet()` | Collect unique departments |
| 3 | `toCollection()` | Collect into LinkedList |
| 4 | `toMap()` | Convert to Map |
| 5 | `toMap()` with merge | Handle duplicate keys |
| 6 | `joining()` | Join names into a String |
| 7 | `counting()` | Count employees |
| 8 | `summingInt()` | Calculate total salary |
| 9 | `averagingInt()` | Find average salary |
| 10 | `summarizingInt()` | Get salary statistics |
| 11 | `groupingBy()` | Group by department |
| 12 | `groupingBy() + counting()` | Count employees in each department |
| 13 | `partitioningBy()` | Split based on salary condition |
| 14 | `groupingBy()` | Group by gender |
| 15 | `groupingBy() + maxBy()` | Highest-paid employee in each department |