
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
>
> The dataset contains **15 employees** across 10 departments with salaries ranging from ₹48,000 to ₹1,05,000.

These examples demonstrate the most commonly used methods of the **Collectors** class using the provided `Employee` class.

---

### 📋 Quick Reference — Dataset at a Glance

| ID  | Name            | Age | Salary  | Department | Gender |
|-----|-----------------|-----|---------|------------|--------|
| 101 | Alice Johnson   | 28  | 55,000  | HR         | Female |
| 102 | Bob Smith       | 35  | 72,000  | Finance    | Male   |
| 103 | Charlie Brown   | 40  | 68,000  | IT         | Male   |
| 104 | Diana Prince    | 30  | 80,000  | Marketing  | Female |
| 105 | Ethan Hunt      | 45  | 95,000  | Operations | Male   |
| 106 | Fiona Davis     | 26  | 48,000  | Sales      | Female |
| 107 | George Miller   | 50  | 105,000 | Management | Male   |
| 108 | Hannah Lee      | 32  | 67,000  | Design     | Female |
| 109 | Ian Clark       | 29  | 60,000  | Support    | Male   |
| 110 | Julia Roberts   | 38  | 85,000  | Legal      | Female |
| 111 | Kevin Turner    | 27  | 52,000  | HR         | Male   |
| 112 | Laura Scott     | 33  | 75,000  | Finance    | Female |
| 113 | Michael Adams   | 41  | 90,000  | IT         | Male   |
| 114 | Nina Patel      | 36  | 78,000  | Marketing  | Female |
| 115 | Oscar White     | 44  | 97,000  | Operations | Male   |

---

# Example 1: Collect Employees into a List (`toList`)

### Objective

Collect all employees into a new `List`.

### What does `Collectors.toList()` do?

It gathers every element from the stream and places them into a new `ArrayList`, preserving the **insertion order** (the order in which elements appeared in the original source).

### When to use?

Use `toList()` when you want the result as a simple list — this is the **most common** collector you'll use in everyday coding.

```java
List<Employee> employeeList = employees.stream()
        .collect(Collectors.toList());

employeeList.forEach(e -> System.out.println(e.getName()));
```

### Output

```
Alice Johnson
Bob Smith
Charlie Brown
Diana Prince
Ethan Hunt
Fiona Davis
George Miller
Hannah Lee
Ian Clark
Julia Roberts
Kevin Turner
Laura Scott
Michael Adams
Nina Patel
Oscar White
```

### Explanation

- `.stream()` creates a sequential stream from the `employees` list.
- `.collect(Collectors.toList())` triggers the pipeline and collects **all 15 employees** into a brand-new `ArrayList`.
- The original `employees` list is **not modified** — a completely new list is returned.
- The order is preserved: Alice (first added) appears first, Oscar (last added) appears last.

> 💡 **Tip:** From **Java 16+**, you can use `.toList()` directly (without `Collectors`) — but note that it returns an **unmodifiable** list.

---

# Example 2: Collect Unique Departments (`mapping` + `toSet`)

### Objective

Collect all unique department names from the employee list.

### What does `Collectors.toSet()` do?

It collects elements into a `HashSet`, which **automatically removes duplicates**. Since a `Set` does not allow duplicate values, each department name appears only once.

### When to use?

Use `toSet()` when you need **unique values** and **don't care about order**.

```java
Set<String> departments = employees.stream()
        .map(Employee::getDepartment)
        .collect(Collectors.toSet());

System.out.println(departments);
```

### Output

```
[Legal, Sales, HR, Design, Operations, Finance, Support, IT, Marketing, Management]
```

> ⚠️ **Note:** The output order may vary because `HashSet` does not guarantee any particular ordering.

### Explanation

- `.map(Employee::getDepartment)` transforms the stream from `Stream<Employee>` → `Stream<String>` by extracting each employee's department name.
- Departments like **"HR"**, **"Finance"**, **"IT"**, **"Marketing"**, and **"Operations"** each appear twice in our dataset (2 employees per department), but the remaining 5 departments have 1 employee each.
- `.collect(Collectors.toSet())` collects all department names into a `HashSet`, which discards duplicates.
- Result: **10 unique departments** out of 15 employees.

```
How it works step by step:

Stream of departments (with duplicates):
HR → Finance → IT → Marketing → Operations → Sales → Management → Design → Support → Legal → HR → Finance → IT → Marketing → Operations

After toSet() (duplicates removed):
{HR, Finance, IT, Marketing, Operations, Sales, Management, Design, Support, Legal}
```

---

# Example 3: Collect Employee Names into a LinkedList (`toCollection`)

### Objective

Store employee names inside a `LinkedList` instead of the default `ArrayList`.

### What does `Collectors.toCollection()` do?

While `toList()` always returns an `ArrayList` and `toSet()` returns a `HashSet`, `toCollection()` lets you **choose exactly which collection type** you want — `LinkedList`, `TreeSet`, `ArrayDeque`, `CopyOnWriteArrayList`, etc.

### When to use?

Use `toCollection()` when you need a **specific collection implementation** (e.g., `LinkedList` for frequent insertions/deletions, `TreeSet` for sorted unique elements).

```java
LinkedList<String> employeeNames = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.toCollection(LinkedList::new));

System.out.println(employeeNames);
System.out.println("Type: " + employeeNames.getClass().getSimpleName());
```

### Output

```
[Alice Johnson, Bob Smith, Charlie Brown, Diana Prince, Ethan Hunt, Fiona Davis, George Miller, Hannah Lee, Ian Clark, Julia Roberts, Kevin Turner, Laura Scott, Michael Adams, Nina Patel, Oscar White]
Type: LinkedList
```

### Explanation

- `.map(Employee::getName)` extracts only the names from each `Employee` object.
- `Collectors.toCollection(LinkedList::new)` tells the collector: *"Don't create the default ArrayList — create a LinkedList instead."*
- `LinkedList::new` is a **Supplier** — it's a method reference to the `LinkedList` no-arg constructor. Every time the collector needs a new container, it calls `new LinkedList<>()`.
- The order of elements is preserved, just like `toList()`.

> 💡 **Real-world use case:** You might use `TreeSet::new` instead if you want the names to be **sorted and unique**:
> ```java
> TreeSet<String> sorted = employees.stream()
>         .map(Employee::getName)
>         .collect(Collectors.toCollection(TreeSet::new));
> // Output: [Alice Johnson, Bob Smith, Charlie Brown, ...]  ← alphabetically sorted
> ```

---

# Example 4: Convert Employees into a Map (`toMap`)

### Objective

Create a `Map` where Employee ID is the **key** and the Employee object is the **value**.

### What does `Collectors.toMap()` do?

It converts stream elements into key-value pairs, building a `HashMap`. You provide two functions:
1. **Key mapper** — extracts the key from each element.
2. **Value mapper** — extracts the value from each element.

### When to use?

Use `toMap()` when you need **fast lookup by a unique identifier** (like ID, email, or username).

```java
Map<Integer, Employee> employeeMap = employees.stream()
        .collect(Collectors.toMap(
                Employee::getEmployeeId,    // Key: employee ID
                employee -> employee        // Value: the employee object itself
        ));

employeeMap.forEach((id, emp) ->
        System.out.println(id + " -> " + emp.getName() + " (" + emp.getDepartment() + ")"));
```

### Output

```
101 -> Alice Johnson (HR)
102 -> Bob Smith (Finance)
103 -> Charlie Brown (IT)
104 -> Diana Prince (Marketing)
105 -> Ethan Hunt (Operations)
106 -> Fiona Davis (Sales)
107 -> George Miller (Management)
108 -> Hannah Lee (Design)
109 -> Ian Clark (Support)
110 -> Julia Roberts (Legal)
111 -> Kevin Turner (HR)
112 -> Laura Scott (Finance)
113 -> Michael Adams (IT)
114 -> Nina Patel (Marketing)
115 -> Oscar White (Operations)
```

### Explanation

- `Employee::getEmployeeId` — Each employee's ID becomes the **key** in the map (e.g., `101`, `102`, ...).
- `employee -> employee` — The entire `Employee` object becomes the **value**. (You can also write this as `Function.identity()`.)
- This creates a `HashMap<Integer, Employee>` with **15 entries**.
- Now you can quickly look up any employee by ID: `employeeMap.get(105)` → returns Ethan Hunt's `Employee` object.

> ⚠️ **Important:** If two employees share the same ID (duplicate keys), Java will throw:
> ```
> java.lang.IllegalStateException: Duplicate key ...
> ```
> See the next example for how to handle this.

---

# Example 5: Handle Duplicate Keys in `toMap()`

### Objective

Resolve duplicate keys using a **merge function** so the program doesn't crash.

### The Problem

If we add a new employee with the same ID as an existing one (e.g., ID `101`), calling `toMap()` without a merge function throws an `IllegalStateException`.

### The Solution

Provide a **third argument** — a merge function `(existing, duplicate) -> ...` — that tells Java which value to keep when a key collision occurs.

```java
List<Employee> list = new ArrayList<>(employees);

// Adding a duplicate employee with the same ID as Alice Johnson (101)
list.add(new Employee(
        "Duplicate Employee",
        30,
        60000,
        "IT",
        101,                      // ← Same ID as Alice Johnson!
        "duplicate@example.com",
        "Male"
));

Map<Integer, Employee> map = list.stream()
        .collect(Collectors.toMap(
                Employee::getEmployeeId,          // Key mapper
                employee -> employee,             // Value mapper
                (existing, duplicate) -> existing  // Merge function: keep the FIRST one
        ));

Employee result = map.get(101);
System.out.println("ID 101 -> " + result.getName() + " (Salary: " + result.getSalary() + ")");
System.out.println("Total entries in map: " + map.size());
```

### Output

```
ID 101 -> Alice Johnson (Salary: 55000)
Total entries in map: 15
```

### Explanation

- We added a 16th employee named **"Duplicate Employee"** with **ID 101** — the same ID as Alice Johnson.
- Without the merge function, Java would throw:
  ```
  java.lang.IllegalStateException: Duplicate key Employee Details: ...
  ```
- The merge function `(existing, duplicate) -> existing` says: *"When there's a key conflict, keep the existing (first) value and discard the duplicate."*
- So Alice Johnson (the original ID 101) is kept, and "Duplicate Employee" is discarded.
- The map ends up with **15 entries** (not 16), because the duplicate was resolved.

> 💡 **Alternative merge strategies:**
> ```java
> // Keep the NEWER (duplicate) value instead:
> (existing, duplicate) -> duplicate
>
> // Keep the one with the HIGHER salary:
> (existing, duplicate) -> existing.getSalary() >= duplicate.getSalary() ? existing : duplicate
> ```

---

# Example 6: Join All Employee Names (`joining`)

### Objective

Concatenate all employee names into a **single comma-separated String**.

### What does `Collectors.joining()` do?

It joins all `String` elements of a stream into one `String`. You can specify:
- A **delimiter** (separator between elements)
- Optionally, a **prefix** and **suffix** around the entire result

### When to use?

Use `joining()` for generating CSV output, display strings, log messages, or any scenario where you need to merge multiple strings into one.

```java
String names = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.joining(", "));

System.out.println(names);
```

### Output

```
Alice Johnson, Bob Smith, Charlie Brown, Diana Prince, Ethan Hunt, Fiona Davis, George Miller, Hannah Lee, Ian Clark, Julia Roberts, Kevin Turner, Laura Scott, Michael Adams, Nina Patel, Oscar White
```

### With Prefix and Suffix

```java
String namesFormatted = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.joining(" | ", "[ ", " ]"));

System.out.println(namesFormatted);
```

### Output (with prefix/suffix)

```
[ Alice Johnson | Bob Smith | Charlie Brown | Diana Prince | Ethan Hunt | Fiona Davis | George Miller | Hannah Lee | Ian Clark | Julia Roberts | Kevin Turner | Laura Scott | Michael Adams | Nina Patel | Oscar White ]
```

### Explanation

- `.map(Employee::getName)` extracts the name from each employee, converting `Stream<Employee>` → `Stream<String>`.
- `Collectors.joining(", ")` concatenates all 15 names, placing `", "` between each pair.
- In the second version, `joining(" | ", "[ ", " ]")` uses `" | "` as delimiter, `"[ "` as prefix, and `" ]"` as suffix.
- Internally, the collector uses a `StringJoiner` which is highly efficient for string concatenation.

> 💡 **Real-world use case:** Generating a quick email recipient list, building a CSV row, or creating a human-readable summary string.

---

# Example 7: Count Employees (`counting`)

### Objective

Count the total number of employees in the stream.

### What does `Collectors.counting()` do?

It counts the number of elements in the stream and returns the result as a `Long`. While `stream.count()` can do the same thing, `Collectors.counting()` is powerful when used **inside other collectors** like `groupingBy()` (see Example 12).

### When to use?

Use `Collectors.counting()` as a standalone for simple counts, or as a **downstream collector** within `groupingBy()` or `partitioningBy()` for counting within groups.

```java
long totalEmployees = employees.stream()
        .collect(Collectors.counting());

System.out.println("Total employees: " + totalEmployees);
```

### Output

```
Total employees: 15
```

### Explanation

- The stream contains all 15 employees from our dataset.
- `Collectors.counting()` simply counts each element as it flows through, returning `15L` (a `long` value).
- This is equivalent to `employees.stream().count()`, but the `Collectors.counting()` form becomes essential when you need to count elements **within groups** (e.g., "how many employees per department?" — see Example 12).

---

# Example 8: Calculate Total Salary (`summingInt`)

### Objective

Find the **total salary** of all employees combined.

### What does `Collectors.summingInt()` do?

It extracts an `int` value from each element using the provided function and returns the **sum** of all those values. Variants also exist: `summingLong()` and `summingDouble()`.

### When to use?

Use `summingInt()` when you need to compute a total (sum) of a numeric property across all elements — total salary, total marks, total order amount, etc.

```java
int totalSalary = employees.stream()
        .collect(Collectors.summingInt(Employee::getSalary));

System.out.println("Total salary of all employees: ₹" + totalSalary);
```

### Output

```
Total salary of all employees: ₹1127000
```

### Explanation

The collector extracts each employee's salary and sums them up:

```
Alice:55000 + Bob:72000 + Charlie:68000 + Diana:80000 + Ethan:95000
+ Fiona:48000 + George:105000 + Hannah:67000 + Ian:60000 + Julia:85000
+ Kevin:52000 + Laura:75000 + Michael:90000 + Nina:78000 + Oscar:97000
= ₹11,27,000
```

- `Employee::getSalary` is a `ToIntFunction` — it tells the collector which integer field to extract from each `Employee`.
- The sum is returned as an `int`. For very large sums, use `Collectors.summingLong()` to avoid integer overflow.

> 💡 **Alternative approach:** You could also use `mapToInt` + `sum()`:
> ```java
> int total = employees.stream().mapToInt(Employee::getSalary).sum();
> ```
> Both approaches give the same result. The `summingInt()` form is more useful as a **downstream collector** inside `groupingBy()`.

---

# Example 9: Find Average Salary (`averagingInt`)

### Objective

Calculate the **average salary** of all employees.

### What does `Collectors.averagingInt()` do?

It extracts an `int` value from each element and computes the **arithmetic mean (average)** of all values. The result is always returned as a `double` for precision.

### When to use?

Use `averagingInt()` for computing averages — average salary, average marks, average age, average order value, etc.

```java
double averageSalary = employees.stream()
        .collect(Collectors.averagingInt(Employee::getSalary));

System.out.println("Average salary: ₹" + String.format("%.2f", averageSalary));
```

### Output

```
Average salary: ₹75133.33
```

### Explanation

The average is calculated as:

```
Average = Total Salary / Number of Employees
Average = 11,27,000 / 15
Average = 75,133.33
```

- `Employee::getSalary` extracts the salary (`int`) from each employee.
- The collector sums all 15 salaries (₹11,27,000) and divides by the count (15).
- The result `75133.33` is returned as a `double`, giving us decimal precision.
- We used `String.format("%.2f", ...)` to display exactly 2 decimal places.

---

# Example 10: Get Salary Statistics (`summarizingInt`)

### Objective

Get **all statistics at once** — count, sum, minimum, maximum, and average — in a single operation.

### What does `Collectors.summarizingInt()` do?

Instead of calling `counting()`, `summingInt()`, `averagingInt()`, `minBy()`, and `maxBy()` separately, this single collector gives you **all five statistics** packed into one `IntSummaryStatistics` object.

### When to use?

Use `summarizingInt()` when you need **multiple statistics** about the same numeric field. It's very efficient because it traverses the stream **only once**.

```java
IntSummaryStatistics statistics = employees.stream()
        .collect(Collectors.summarizingInt(Employee::getSalary));

System.out.println(statistics);
System.out.println();
System.out.println("Count   : " + statistics.getCount());
System.out.println("Sum     : ₹" + statistics.getSum());
System.out.println("Min     : ₹" + statistics.getMin());
System.out.println("Max     : ₹" + statistics.getMax());
System.out.println("Average : ₹" + String.format("%.2f", statistics.getAverage()));
```

### Output

```
IntSummaryStatistics{count=15, sum=1127000, min=48000, average=75133.333333, max=105000}

Count   : 15
Sum     : ₹1127000
Min     : ₹48000
Max     : ₹105000
Average : ₹75133.33
```

### Explanation

From our 15 employees, the collector computes:

| Statistic | Value | Meaning |
|-----------|-------|---------|
| `count`   | 15 | Total number of employees |
| `sum`     | ₹11,27,000 | Sum of all salaries |
| `min`     | ₹48,000 | Lowest salary (Fiona Davis — Sales) |
| `max`     | ₹1,05,000 | Highest salary (George Miller — Management) |
| `average` | ₹75,133.33 | Average salary across all employees |

- The `IntSummaryStatistics` object provides getter methods for each statistic: `getCount()`, `getSum()`, `getMin()`, `getMax()`, `getAverage()`.
- This is **far more efficient** than running 5 separate stream pipelines, because the stream is traversed only once.

> 💡 **Variants:** `summarizingLong()` and `summarizingDouble()` are also available for `long` and `double` fields respectively.

---

# Example 11: Group Employees by Department (`groupingBy`)

### Objective

Group employees based on their department — creating a `Map` where the key is the department name and the value is a `List` of employees in that department.

### What does `Collectors.groupingBy()` do?

It works like the SQL `GROUP BY` clause. It classifies each element based on a **classifier function** (the field/property you want to group by) and collects elements with the same key into a `List`.

### When to use?

Use `groupingBy()` whenever you need to **categorize data** — group orders by status, students by grade, products by category, etc. This is one of the **most powerful and frequently used** collectors.

```java
Map<String, List<Employee>> departmentWiseEmployees =
        employees.stream()
                .collect(Collectors.groupingBy(Employee::getDepartment));

departmentWiseEmployees.forEach((department, employeeList) -> {

    System.out.println("\nDepartment : " + department + " (" + employeeList.size() + " employees)");

    employeeList.forEach(employee ->
            System.out.println("   - " + employee.getName() + " (₹" + employee.getSalary() + ")"));
});
```

### Output

```
Department : HR (2 employees)
   - Alice Johnson (₹55000)
   - Kevin Turner (₹52000)

Department : Finance (2 employees)
   - Bob Smith (₹72000)
   - Laura Scott (₹75000)

Department : IT (2 employees)
   - Charlie Brown (₹68000)
   - Michael Adams (₹90000)

Department : Marketing (2 employees)
   - Diana Prince (₹80000)
   - Nina Patel (₹78000)

Department : Operations (2 employees)
   - Ethan Hunt (₹95000)
   - Oscar White (₹97000)

Department : Sales (1 employees)
   - Fiona Davis (₹48000)

Department : Management (1 employees)
   - George Miller (₹105000)

Department : Design (1 employees)
   - Hannah Lee (₹67000)

Department : Support (1 employees)
   - Ian Clark (₹60000)

Department : Legal (1 employees)
   - Julia Roberts (₹85000)
```

### Explanation

```
How groupingBy works internally:

Employee Stream:
Alice(HR) → Bob(Finance) → Charlie(IT) → Diana(Marketing) → ... → Oscar(Operations)

Classifier function: Employee::getDepartment

Result Map:
┌──────────────┬────────────────────────────────────┐
│ Key (Dept)   │ Value (List<Employee>)             │
├──────────────┼────────────────────────────────────┤
│ "HR"         │ [Alice Johnson, Kevin Turner]       │
│ "Finance"    │ [Bob Smith, Laura Scott]            │
│ "IT"         │ [Charlie Brown, Michael Adams]      │
│ "Marketing"  │ [Diana Prince, Nina Patel]          │
│ "Operations" │ [Ethan Hunt, Oscar White]           │
│ "Sales"      │ [Fiona Davis]                       │
│ "Management" │ [George Miller]                     │
│ "Design"     │ [Hannah Lee]                        │
│ "Support"    │ [Ian Clark]                         │
│ "Legal"      │ [Julia Roberts]                     │
└──────────────┴────────────────────────────────────┘
```

- `Employee::getDepartment` is the **classifier function** — it determines which "bucket" each employee goes into.
- Employees with the **same department** end up in the same list.
- 5 departments have 2 employees each; 5 departments have 1 employee each — totaling 15 employees across 10 groups.

---

# Example 12: Count Employees in Each Department

### Objective

Find **how many employees** belong to each department — instead of listing them, just count them.

### What makes this different from Example 11?

In Example 11, the value of each group was a `List<Employee>`. Here, we use a **downstream collector** (`Collectors.counting()`) to replace the default list with a **count**.

### When to use?

Use `groupingBy()` + `counting()` when you need aggregated counts per group — number of orders per customer, number of students per class, etc.

```java
Map<String, Long> employeeCount =
        employees.stream()
                .collect(Collectors.groupingBy(
                        Employee::getDepartment,   // Classifier: group by department
                        Collectors.counting()      // Downstream: count instead of list
                ));

System.out.println("Employees per department:");
employeeCount.forEach((dept, count) ->
        System.out.println("   " + dept + " : " + count));
```

### Output

```
Employees per department:
   HR : 2
   Finance : 2
   IT : 2
   Marketing : 2
   Operations : 2
   Sales : 1
   Management : 1
   Design : 1
   Support : 1
   Legal : 1
```

### Explanation

- This is a **two-level collector** — an outer collector and an inner (downstream) collector:
  - **Outer:** `groupingBy(Employee::getDepartment)` — groups employees by department.
  - **Inner (downstream):** `Collectors.counting()` — instead of collecting employees into a `List`, it **counts** them.
- Without the downstream collector: `Map<String, List<Employee>>` (default).
- With `Collectors.counting()` as downstream: `Map<String, Long>`.
- Result tells us: HR has 2 employees, Sales has 1, etc.

> 💡 **Key concept:** The second argument to `groupingBy()` is called a **downstream collector**. It determines what happens to the elements **within each group**. You can use `counting()`, `summingInt()`, `maxBy()`, `mapping()`, `toSet()`, or even another `groupingBy()` for multi-level grouping!

---

# Example 13: Partition Employees Based on Salary (`partitioningBy`)

### Objective

Split employees into exactly **two groups**: those earning **₹75,000 or more** and those earning **less than ₹75,000**.

### What does `Collectors.partitioningBy()` do?

It divides the stream into exactly **two partitions** based on a `Predicate` (true/false condition):
- `true` → elements that satisfy the condition
- `false` → elements that don't

### How is it different from `groupingBy()`?

| `groupingBy()` | `partitioningBy()` |
|---|---|
| Can create **many groups** (based on values) | Always creates exactly **2 groups** (true/false) |
| Key can be any type | Key is always `Boolean` |
| Groups may be missing if no match | Both `true` and `false` keys are **always present** |

```java
Map<Boolean, List<Employee>> result =
        employees.stream()
                .collect(Collectors.partitioningBy(
                        employee -> employee.getSalary() >= 75000
                ));


result.get(true).forEach(employee ->
        System.out.println("   " + employee.getName() + " - ₹" + employee.getSalary()));

result.get(false).forEach(employee ->
        System.out.println("   " + employee.getName() + " - ₹" + employee.getSalary()));
```

### Output

```
   Diana Prince - ₹80000
   Ethan Hunt - ₹95000
   George Miller - ₹105000
   Julia Roberts - ₹85000
   Laura Scott - ₹75000
   Michael Adams - ₹90000
   Nina Patel - ₹78000
   Oscar White - ₹97000

   Alice Johnson - ₹55000
   Bob Smith - ₹72000
   Charlie Brown - ₹68000
   Fiona Davis - ₹48000
   Hannah Lee - ₹67000
   Ian Clark - ₹60000
   Kevin Turner - ₹52000
```

### Explanation

- The predicate `employee -> employee.getSalary() >= 75000` is evaluated for each employee.
- **8 employees** earn ₹75,000 or more → they go into the `true` partition.
- **7 employees** earn less than ₹75,000 → they go into the `false` partition.
- Unlike `groupingBy()`, **both partitions always exist** in the result map — even if one group is empty, the key will be present with an empty list.

> 💡 **Real-world use case:** Partitioning students into pass/fail, partitioning orders into delivered/pending, partitioning products into in-stock/out-of-stock.

---

# Example 14: Group Employees by Gender

### Objective

Group employees based on their gender.

### When to use?

This is another classic `groupingBy()` use case — splitting data by a categorical property. Useful for generating gender-based reports, analytics dashboards, or diversity metrics.

```java
Map<String, List<Employee>> genderWiseEmployees =
        employees.stream()
                .collect(Collectors.groupingBy(Employee::getGender));

genderWiseEmployees.forEach((gender, employeeList) -> {

    System.out.println("\n" + gender + " (" + employeeList.size() + " employees):");

    employeeList.forEach(employee ->
            System.out.println("   - " + employee.getName()
                    + " | " + employee.getDepartment()
                    + " | ₹" + employee.getSalary()));
});
```

### Output

```
Female (7 employees):
   - Alice Johnson | HR | ₹55000
   - Diana Prince | Marketing | ₹80000
   - Fiona Davis | Sales | ₹48000
   - Hannah Lee | Design | ₹67000
   - Julia Roberts | Legal | ₹85000
   - Laura Scott | Finance | ₹75000
   - Nina Patel | Marketing | ₹78000

Male (8 employees):
   - Bob Smith | Finance | ₹72000
   - Charlie Brown | IT | ₹68000
   - Ethan Hunt | Operations | ₹95000
   - George Miller | Management | ₹105000
   - Ian Clark | Support | ₹60000
   - Kevin Turner | HR | ₹52000
   - Michael Adams | IT | ₹90000
   - Oscar White | Operations | ₹97000
```

### Explanation

- `Employee::getGender` is the classifier — it returns either `"Male"` or `"Female"`.
- The collector creates exactly **2 groups** (in this dataset): `"Female"` with 7 employees and `"Male"` with 8 employees.
- This looks similar to `partitioningBy()`, but there's a key difference: `groupingBy()` groups by **any value** (here, String values `"Male"` and `"Female"`), while `partitioningBy()` groups by `true`/`false` only.

> 💡 **When to use `groupingBy` vs `partitioningBy` for two groups?**
> - Use `partitioningBy()` when the condition is a boolean check (e.g., salary ≥ 75000).
> - Use `groupingBy()` when grouping by a property/field (e.g., gender, department, city).

---

# Example 15: Find the Highest Paid Employee in Each Department

### Objective

Find the employee who has the **highest salary** in each department.

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
                " : ₹" +
                employee.get().getSalary()
        )
);
```

### Output

```
HR -> Alice Johnson : ₹55000
Finance -> Laura Scott : ₹75000
IT -> Michael Adams : ₹90000
Marketing -> Diana Prince : ₹80000
Operations -> Oscar White : ₹97000
Sales -> Fiona Davis : ₹48000
Management -> George Miller : ₹105000
Design -> Hannah Lee : ₹67000
Support -> Ian Clark : ₹60000
Legal -> Julia Roberts : ₹85000
```

### Explanation

This example uses two collectors:

- `groupingBy()` → Groups employees by department.
- `maxBy()` → Finds the employee with the highest salary in each department.

For example, in the **IT** department:

```
IT Employees

Charlie Brown   ₹68,000
Michael Adams   ₹90,000

Highest Salary

Michael Adams   ₹90,000
```

The same process is performed for every department.

> **Note:** `maxBy()` returns an `Optional<Employee>`, so we use `employee.get()` to access the employee. In real applications, it's better to check if a value is present before calling `get()`.

---

# Summary Table

| Example | Collector Used | Purpose | Result Type |
|---------|----------------|---------|-------------|
| 1 | `toList()` | Collect all employees into a List | `List<Employee>` |
| 2 | `toSet()` | Collect unique department names | `Set<String>` |
| 3 | `toCollection()` | Collect names into a LinkedList | `LinkedList<String>` |
| 4 | `toMap()` | Create ID → Employee lookup map | `Map<Integer, Employee>` |
| 5 | `toMap()` with merge | Handle duplicate keys gracefully | `Map<Integer, Employee>` |
| 6 | `joining()` | Concatenate all names into one String | `String` |
| 7 | `counting()` | Count total employees | `Long` |
| 8 | `summingInt()` | Calculate total salary (₹11,27,000) | `Integer` |
| 9 | `averagingInt()` | Find average salary (₹75,133.33) | `Double` |
| 10 | `summarizingInt()` | Get all salary statistics at once | `IntSummaryStatistics` |
| 11 | `groupingBy()` | Group employees by department | `Map<String, List<Employee>>` |
| 12 | `groupingBy()` + `counting()` | Count employees per department | `Map<String, Long>` |
| 13 | `partitioningBy()` | Split into high/low salary groups | `Map<Boolean, List<Employee>>` |
| 14 | `groupingBy()` | Group employees by gender | `Map<String, List<Employee>>` |
| 15 | `groupingBy()` + `maxBy()` | Highest-paid per department | `Map<String, Optional<Employee>>` |