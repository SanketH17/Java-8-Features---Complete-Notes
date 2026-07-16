# Optional Class in Java

`Optional` is one of the useful features introduced in **Java 8**. It helps us handle values that **may or may not be present** and reduces the chances of getting a `NullPointerException`.

---

## 1. Why Do We Need Optional?

Imagine that you order a product online. The delivery box may contain:

- A product
- Nothing, because the product is unavailable

An `Optional` is like that delivery box.

```text
Optional Box
    |
    +---- Contains a value
    |
    +---- Empty
```

In Java, an `Optional` object can either:

```text
Optional[value]  → Value is available
Optional.empty   → Value is not available
```

---

## 2. Problem Without Optional

Consider the following method:

```java
public static String getUserName() {
    return null;
}
```

If we use the returned value directly:

```java
String name = getUserName();

System.out.println(name.toUpperCase());
```

This will throw:

```text
NullPointerException
```

That is because `name` contains `null`, and we are trying to call `toUpperCase()` on it.

### Traditional Solution

```java
String name = getUserName();

if (name != null) {
    System.out.println(name.toUpperCase());
} else {
    System.out.println("Name is not available");
}
```

This works, but we must repeatedly write `null` checks.

### Solution Using Optional

```java
public static Optional<String> getUserName() {
    return Optional.ofNullable(null);
}
```

Now we can handle the missing value safely:

```java
Optional<String> name = getUserName();

name.ifPresent(value -> System.out.println(value.toUpperCase()));
```

There is no `NullPointerException`.

---

# 3. What Is Optional?

`Optional` is a container object that may or may not contain a value.

It belongs to the `java.util` package.

```java
import java.util.Optional;
```

Syntax:

```java
Optional<DataType> variableName;
```

Example:

```java
Optional<String> name;
Optional<Integer> age;
Optional<Employee> employee;
```

---

# 4. How to Create an Optional Object

There are three main methods used to create an `Optional`:

1. `Optional.of()`
2. `Optional.ofNullable()`
3. `Optional.empty()`

---

## 4.1 `Optional.of()`

`Optional.of()` creates an `Optional` containing a **non-null value**.

```java
Optional<String> name = Optional.of("Sanket");

System.out.println(name);
```

Output:

```text
Optional[Sanket]
```

### Important Rule

The value passed to `Optional.of()` must not be `null`.

```java
Optional<String> name = Optional.of(null);
```

Output:

```text
NullPointerException
```

### When Should We Use `of()`?

Use `Optional.of()` when you are completely sure that the value is not `null`.

```java
String company = "Microsoft";

Optional<String> optionalCompany = Optional.of(company);
```

---

## 4.2 `Optional.ofNullable()`

`Optional.ofNullable()` creates an `Optional` that can accept either:

- A non-null value
- A null value

### Example with a Non-null Value

```java
String name = "Sanket";

Optional<String> optionalName = Optional.ofNullable(name);

System.out.println(optionalName);
```

Output:

```text
Optional[Sanket]
```

### Example with a Null Value

```java
String name = null;

Optional<String> optionalName = Optional.ofNullable(name);

System.out.println(optionalName);
```

Output:

```text
Optional.empty
```

It does not throw a `NullPointerException`.

### When Should We Use `ofNullable()`?

Use it when you are not sure whether the value is available.

```java
String email = getEmailFromDatabase();

Optional<String> optionalEmail = Optional.ofNullable(email);
```

---

## 4.3 `Optional.empty()`

`Optional.empty()` creates an empty `Optional`.

```java
Optional<String> optionalName = Optional.empty();

System.out.println(optionalName);
```

Output:

```text
Optional.empty
```

It means that no value is available inside the `Optional`.

---

## 4.4 Difference Between Creation Methods

| Method | Accepts Non-null Value? | Accepts Null? | Result for Null |
|---|---:|---:|---|
| `Optional.of(value)` | Yes | No | Throws `NullPointerException` |
| `Optional.ofNullable(value)` | Yes | Yes | Returns `Optional.empty` |
| `Optional.empty()` | No value required | Not applicable | Creates an empty Optional |

---

# 5. Most Important Optional Methods

The most commonly used methods are:

1. `isPresent()`
2. `isEmpty()`
3. `get()`
4. `ifPresent()`
5. `ifPresentOrElse()`
6. `orElse()`
7. `orElseGet()`
8. `orElseThrow()`
9. `filter()`
10. `map()`
11. `flatMap()`
12. `or()`
13. `stream()`

---

# 6. `isPresent()`

The `isPresent()` method checks whether a value is available inside the `Optional`.

- Returns `true` if a value is present
- Returns `false` if the `Optional` is empty

### Example

```java
Optional<String> name = Optional.ofNullable("Sanket");

if (name.isPresent()) {
    System.out.println("Name is available");
} else {
    System.out.println("Name is not available");
}
```

Output:

```text
Name is available
```

### Empty Optional Example

```java
Optional<String> name = Optional.empty();

System.out.println(name.isPresent());
```

Output:

```text
false
```

### Important Note

Although `isPresent()` is useful, this code:

```java
if (name.isPresent()) {
    System.out.println(name.get());
}
```

looks similar to a traditional `null` check:

```java
if (name != null) {
    System.out.println(name);
}
```

In many situations, `ifPresent()`, `map()`, `orElse()`, or `orElseThrow()` provides a cleaner solution.

---

# 7. `isEmpty()`

The `isEmpty()` method checks whether the `Optional` does not contain a value.

It was introduced in **Java 11**.

- Returns `true` if the `Optional` is empty
- Returns `false` if a value is present

```java
Optional<String> name = Optional.empty();

if (name.isEmpty()) {
    System.out.println("Name is not available");
}
```

Output:

```text
Name is not available
```

### Java 8 Alternative

Java 8 does not have `isEmpty()`. In Java 8, we can write:

```java
if (!name.isPresent()) {
    System.out.println("Name is not available");
}
```

---

# 8. `get()`

The `get()` method returns the value stored inside the `Optional`.

```java
Optional<String> name = Optional.of("Sanket");

String result = name.get();

System.out.println(result);
```

Output:

```text
Sanket
```

## Problem with `get()`

If the `Optional` is empty, `get()` throws `NoSuchElementException`.

```java
Optional<String> name = Optional.empty();

System.out.println(name.get());
```

Output:

```text
NoSuchElementException: No value present
```

## Recommendation

Avoid using `get()` directly unless you have already checked that the value is present.

Instead of this:

```java
String result = name.get();
```

Prefer:

```java
String result = name.orElse("Default Name");
```

Or:

```java
String result = name.orElseThrow();
```

---

# 9. `ifPresent()`

The `ifPresent()` method performs an operation only when a value is available.

It accepts a `Consumer<T>` functional interface.

### Syntax

```java
optional.ifPresent(value -> {
    // Operation
});
```

### Example

```java
Optional<String> name = Optional.ofNullable("Sanket");

name.ifPresent(value -> System.out.println(value));
```

Output:

```text
Sanket
```

### If the Optional Is Empty

```java
Optional<String> name = Optional.empty();

name.ifPresent(value -> System.out.println(value));
```

Nothing will be printed, and no exception will occur.

### Example: Convert Name to Uppercase

```java
Optional<String> name = Optional.of("Sanket");

name.ifPresent(value ->
        System.out.println(value.toUpperCase())
);
```

Output:

```text
SANKET
```

### Method Reference Version

```java
Optional<String> name = Optional.of("Sanket");

name.ifPresent(System.out::println);
```

---

# 10. `ifPresentOrElse()`

`ifPresentOrElse()` allows us to define:

- One action when the value is present
- Another action when the value is absent

It was introduced in **Java 9**.

### Syntax

```java
optional.ifPresentOrElse(
    value -> {
        // Execute when value is present
    },
    () -> {
        // Execute when value is absent
    }
);
```

### Example with a Value

```java
Optional<String> name = Optional.of("Sanket");

name.ifPresentOrElse(
    value -> System.out.println("Name: " + value),
    () -> System.out.println("Name is not available")
);
```

Output:

```text
Name: Sanket
```

### Example Without a Value

```java
Optional<String> name = Optional.empty();

name.ifPresentOrElse(
    value -> System.out.println("Name: " + value),
    () -> System.out.println("Name is not available")
);
```

Output:

```text
Name is not available
```

---

# 11. `orElse()`

The `orElse()` method returns:

- The actual value if it is present
- A default value if the `Optional` is empty

### Syntax

```java
optional.orElse(defaultValue);
```

### Example with a Value

```java
Optional<String> name = Optional.of("Sanket");

String result = name.orElse("Guest");

System.out.println(result);
```

Output:

```text
Sanket
```

### Example Without a Value

```java
Optional<String> name = Optional.empty();

String result = name.orElse("Guest");

System.out.println(result);
```

Output:

```text
Guest
```

### Real-World Example

```java
public static Optional<String> getUserCity() {
    return Optional.empty();
}

public static void main(String[] args) {
    String city = getUserCity().orElse("Pune");

    System.out.println("City: " + city);
}
```

Output:

```text
City: Pune
```

---

# 12. `orElseGet()`

`orElseGet()` also provides a default value if the `Optional` is empty.

The difference is that it uses a `Supplier<T>` to generate the default value.

### Syntax

```java
optional.orElseGet(() -> defaultValue);
```

### Example

```java
Optional<String> name = Optional.empty();

String result = name.orElseGet(() -> "Guest");

System.out.println(result);
```

Output:

```text
Guest
```

### Example with a Method

```java
public static String getDefaultName() {
    System.out.println("Creating default name...");
    return "Guest";
}

public static void main(String[] args) {
    Optional<String> name = Optional.empty();

    String result = name.orElseGet(() -> getDefaultName());

    System.out.println(result);
}
```

Output:

```text
Creating default name...
Guest
```

Using a method reference:

```java
String result = name.orElseGet(ClassName::getDefaultName);
```

---

# 13. Difference Between `orElse()` and `orElseGet()`

This is an important interview question.

## `orElse()`

The default value is created even when the `Optional` already contains a value.

## `orElseGet()`

The default value is created only when the `Optional` is empty.

### Example

```java
public class OptionalExample {

    public static String getDefaultName() {
        System.out.println("getDefaultName() method executed");
        return "Guest";
    }

    public static void main(String[] args) {

        Optional<String> name = Optional.of("Sanket");

        String result = name.orElse(getDefaultName());

        System.out.println("Result: " + result);
    }
}
```

Output:

```text
getDefaultName() method executed
Result: Sanket
```

Even though `"Sanket"` is present, `getDefaultName()` was still executed.

Now use `orElseGet()`:

```java
Optional<String> name = Optional.of("Sanket");

String result = name.orElseGet(() -> getDefaultName());

System.out.println("Result: " + result);
```

Output:

```text
Result: Sanket
```

Here, `getDefaultName()` is not executed because the value is already available.

## Simple Rule

```text
Default value is simple and already available
                ↓
             orElse()

Default value requires calculation or method execution
                ↓
            orElseGet()
```

### Examples

Use `orElse()` for a simple value:

```java
String name = optionalName.orElse("Guest");
```

Use `orElseGet()` for an expensive operation:

```java
User user = optionalUser.orElseGet(() -> loadDefaultUserFromDatabase());
```

---

# 14. `orElseThrow()`

The `orElseThrow()` method returns the value if it is present. If the value is absent, it throws an exception.

### Example with a Value

```java
Optional<String> name = Optional.of("Sanket");

String result = name.orElseThrow();

System.out.println(result);
```

Output:

```text
Sanket
```

### Example with an Empty Optional

```java
Optional<String> name = Optional.empty();

String result = name.orElseThrow();
```

This throws:

```text
NoSuchElementException
```

## Throwing a Custom Exception

```java
Optional<String> name = Optional.empty();

String result = name.orElseThrow(
    () -> new RuntimeException("Name is not available")
);
```

Output:

```text
RuntimeException: Name is not available
```

### Real-World Employee Example

```java
public static Optional<Employee> findEmployeeById(int id) {
    return Optional.empty();
}
```

```java
Employee employee = findEmployeeById(101)
        .orElseThrow(
            () -> new RuntimeException("Employee not found with ID: 101")
        );
```

A better custom exception can be created:

```java
public class EmployeeNotFoundException extends RuntimeException {

    public EmployeeNotFoundException(String message) {
        super(message);
    }
}
```

Usage:

```java
Employee employee = findEmployeeById(101)
        .orElseThrow(
            () -> new EmployeeNotFoundException(
                "Employee not found with ID: 101"
            )
        );
```

---

# 15. `filter()`

The `filter()` method checks whether the value inside the `Optional` satisfies a condition.

It uses the `Predicate<T>` functional interface.

- If the condition is true, it returns the Optional containing the value
- If the condition is false, it returns `Optional.empty`

### Example: Check Age

```java
Optional<Integer> age = Optional.of(25);

Optional<Integer> result = age.filter(value -> value >= 18);

System.out.println(result);
```

Output:

```text
Optional[25]
```

### When the Condition Is False

```java
Optional<Integer> age = Optional.of(15);

Optional<Integer> result = age.filter(value -> value >= 18);

System.out.println(result);
```

Output:

```text
Optional.empty
```

### Practical Example

```java
Optional<String> name = Optional.of("Sanket");

name.filter(value -> value.length() >= 5)
    .ifPresent(value -> System.out.println("Valid name: " + value));
```

Output:

```text
Valid name: Sanket
```

### Filter and Default Value

```java
String result = Optional.of("Java")
        .filter(language -> language.length() > 10)
        .orElse("Condition not satisfied");

System.out.println(result);
```

Output:

```text
Condition not satisfied
```

---

# 16. `map()`

The `map()` method transforms the value inside an `Optional` from one form into another.

It uses the `Function<T, R>` functional interface.

### Simple Example

```java
Optional<String> name = Optional.of("Sanket");

Optional<String> uppercaseName =
        name.map(value -> value.toUpperCase());

System.out.println(uppercaseName);
```

Output:

```text
Optional[SANKET]
```

### Shorter Version

```java
Optional<String> uppercaseName =
        name.map(String::toUpperCase);
```

## Transform String into Integer

```java
Optional<String> name = Optional.of("Sanket");

Optional<Integer> nameLength =
        name.map(value -> value.length());

System.out.println(nameLength);
```

Output:

```text
Optional[6]
```

Here:

```text
Optional<String>
       |
       | map()
       ↓
Optional<Integer>
```

## What Happens If the Optional Is Empty?

```java
Optional<String> name = Optional.empty();

Optional<String> uppercaseName =
        name.map(String::toUpperCase);

System.out.println(uppercaseName);
```

Output:

```text
Optional.empty
```

The mapping operation is not executed.

---

# 17. Practical `map()` Example with Employee

```java
class Employee {

    private int id;
    private String name;
    private double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }
}
```

Create an employee:

```java
Employee employee = new Employee(101, "Sanket", 50000);

Optional<Employee> optionalEmployee =
        Optional.of(employee);
```

Extract the employee name using `map()`:

```java
Optional<String> employeeName =
        optionalEmployee.map(emp -> emp.getName());

System.out.println(employeeName);
```

Output:

```text
Optional[Sanket]
```

Using a method reference:

```java
Optional<String> employeeName =
        optionalEmployee.map(Employee::getName);
```

Get the name directly with a default value:

```java
String employeeName = optionalEmployee
        .map(Employee::getName)
        .orElse("Employee name not available");

System.out.println(employeeName);
```

Output:

```text
Sanket
```

---

# 18. `flatMap()`

`flatMap()` is used when the mapping function itself returns an `Optional`.

Suppose an `Employee` class contains an Optional email:

```java
class Employee {

    private String name;
    private Optional<String> email;

    public Employee(String name, Optional<String> email) {
        this.name = name;
        this.email = email;
    }

    public Optional<String> getEmail() {
        return email;
    }
}
```

Create an employee:

```java
Employee employee = new Employee(
    "Sanket",
    Optional.of("sanket@example.com")
);

Optional<Employee> optionalEmployee =
        Optional.of(employee);
```

## Problem with `map()`

```java
Optional<Optional<String>> email =
        optionalEmployee.map(Employee::getEmail);
```

The result becomes a nested Optional:

```text
Optional<Optional<String>>
```

It is like putting one box inside another box:

```text
Optional[
    Optional[
        sanket@example.com
    ]
]
```

## Solution with `flatMap()`

```java
Optional<String> email =
        optionalEmployee.flatMap(Employee::getEmail);
```

Result:

```text
Optional[sanket@example.com]
```

### Complete Example

```java
String email = optionalEmployee
        .flatMap(Employee::getEmail)
        .orElse("Email not available");

System.out.println(email);
```

Output:

```text
sanket@example.com
```

## Difference Between `map()` and `flatMap()`

Use `map()` when the method returns a normal value:

```java
public String getName() {
    return name;
}
```

```java
Optional<String> name =
        optionalEmployee.map(Employee::getName);
```

Use `flatMap()` when the method already returns an `Optional`:

```java
public Optional<String> getEmail() {
    return email;
}
```

```java
Optional<String> email =
        optionalEmployee.flatMap(Employee::getEmail);
```

---

# 19. `or()`

The `or()` method provides another `Optional` if the original `Optional` is empty.

It was introduced in **Java 9**.

```java
Optional<String> primaryEmail = Optional.empty();
Optional<String> secondaryEmail =
        Optional.of("backup@example.com");

Optional<String> result =
        primaryEmail.or(() -> secondaryEmail);

System.out.println(result);
```

Output:

```text
Optional[backup@example.com]
```

### Multiple Alternatives

```java
Optional<String> result = primaryEmail
        .or(() -> secondaryEmail)
        .or(() -> Optional.of("default@example.com"));
```

Unlike `orElse()`:

- `orElse()` returns a normal value
- `or()` returns another `Optional`

---

# 20. `stream()`

The `stream()` method converts an `Optional` into a Stream.

It was introduced in **Java 9**.

- A non-empty Optional produces a Stream containing one element
- An empty Optional produces an empty Stream

```java
Optional<String> name = Optional.of("Sanket");

name.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

Output:

```text
SANKET
```

It is particularly useful when working with a collection of Optional objects.

```java
List<Optional<String>> names = List.of(
    Optional.of("Sanket"),
    Optional.empty(),
    Optional.of("Rahul")
);

List<String> availableNames = names.stream()
        .flatMap(Optional::stream)
        .toList();

System.out.println(availableNames);
```

Output:

```text
[Sanket, Rahul]
```

> `Stream.toList()` is available from Java 16. In Java 8, use `collect(Collectors.toList())`.

Java 8 version:

```java
List<String> availableNames = names.stream()
        .filter(Optional::isPresent)
        .map(Optional::get)
        .collect(Collectors.toList());
```

---

# 21. Combining Multiple Optional Methods

The real power of `Optional` becomes clear when we combine its methods.

## Example

```java
Optional<String> name = Optional.ofNullable("Sanket");

String result = name
        .filter(value -> value.length() >= 5)
        .map(String::toUpperCase)
        .orElse("INVALID NAME");

System.out.println(result);
```

Output:

```text
SANKET
```

## Step-by-Step Working

```text
Optional.ofNullable("Sanket")
            |
            | filter(name length >= 5)
            ↓
     Optional["Sanket"]
            |
            | map(to uppercase)
            ↓
     Optional["SANKET"]
            |
            | orElse("INVALID NAME")
            ↓
          "SANKET"
```

If the name does not satisfy the condition:

```java
String result = Optional.ofNullable("Ram")
        .filter(value -> value.length() >= 5)
        .map(String::toUpperCase)
        .orElse("INVALID NAME");

System.out.println(result);
```

Output:

```text
INVALID NAME
```

---

# 22. Real-World Repository Example

Optional is frequently used in repository methods where a record may or may not be found.

```java
class EmployeeRepository {

    public Optional<Employee> findById(int id) {

        if (id == 101) {
            return Optional.of(
                new Employee(101, "Sanket", 50000)
            );
        }

        return Optional.empty();
    }
}
```

Using the repository:

```java
EmployeeRepository repository =
        new EmployeeRepository();

Employee employee = repository.findById(101)
        .orElseThrow(
            () -> new RuntimeException("Employee not found")
        );

System.out.println(employee.getName());
```

Output:

```text
Sanket
```

This clearly tells the calling code:

```text
An employee may or may not be found.
```

---

# 23. Common Mistakes to Avoid

## Mistake 1: Using `Optional.of()` with Null

Incorrect:

```java
String name = null;

Optional<String> optionalName =
        Optional.of(name);
```

This throws `NullPointerException`.

Correct:

```java
Optional<String> optionalName =
        Optional.ofNullable(name);
```

---

## Mistake 2: Calling `get()` Without Checking

Incorrect:

```java
Optional<String> name = Optional.empty();

System.out.println(name.get());
```

Correct:

```java
String result = name.orElse("Guest");
```

Or:

```java
String result = name.orElseThrow(
    () -> new RuntimeException("Name not found")
);
```

---

## Mistake 3: Using `isPresent()` Followed by `get()`

This works:

```java
if (name.isPresent()) {
    System.out.println(name.get());
}
```

But this is usually cleaner:

```java
name.ifPresent(System.out::println);
```

---

## Mistake 4: Returning Null from an Optional Method

Incorrect:

```java
public Optional<String> getName() {
    return null;
}
```

An Optional-returning method should never return `null`.

Correct:

```java
public Optional<String> getName() {
    return Optional.empty();
}
```

Or:

```java
public Optional<String> getName() {
    return Optional.of("Sanket");
}
```

---

## Mistake 5: Using Optional for Every Variable

Avoid unnecessary usage like this:

```java
Optional<String> firstName = Optional.of("Sanket");
Optional<String> lastName = Optional.of("Harvande");
Optional<Integer> age = Optional.of(25);
```

Optional is mainly helpful as a **method return type** when a result may not be available.

---

## Mistake 6: Using Optional as a Method Parameter

Avoid:

```java
public void displayName(Optional<String> name) {
}
```

Usually, it is simpler to accept the actual value:

```java
public void displayName(String name) {
}
```

Optional is primarily designed to represent an absent return value, not as a replacement for every nullable parameter.

---

## Mistake 7: Creating an Optional Containing Another Optional

Avoid:

```java
Optional<Optional<String>> email;
```

Use `flatMap()` to convert it into:

```java
Optional<String> email;
```

---

# 24. Functional Interfaces Used by Optional Methods

| Optional Method | Functional Interface | Purpose |
|---|---|---|
| `ifPresent()` | `Consumer<T>` | Performs an operation on the value |
| `ifPresentOrElse()` | `Consumer<T>` and `Runnable` | Handles both present and empty cases |
| `filter()` | `Predicate<T>` | Checks a condition |
| `map()` | `Function<T, R>` | Transforms the value |
| `flatMap()` | `Function<T, Optional<R>>` | Transforms and removes nested Optional |
| `orElseGet()` | `Supplier<T>` | Lazily creates a default value |
| `orElseThrow()` | `Supplier<Exception>` | Lazily creates an exception |
| `or()` | `Supplier<Optional<T>>` | Supplies another Optional |

---

# 25. Java Version Summary

| Method | Available Since |
|---|---|
| `of()` | Java 8 |
| `ofNullable()` | Java 8 |
| `empty()` | Java 8 |
| `isPresent()` | Java 8 |
| `get()` | Java 8 |
| `ifPresent()` | Java 8 |
| `orElse()` | Java 8 |
| `orElseGet()` | Java 8 |
| `orElseThrow(Supplier)` | Java 8 |
| `filter()` | Java 8 |
| `map()` | Java 8 |
| `flatMap()` | Java 8 |
| `ifPresentOrElse()` | Java 9 |
| `or()` | Java 9 |
| `stream()` | Java 9 |
| `orElseThrow()` without arguments | Java 10 |
| `isEmpty()` | Java 11 |

---

# 26. Important Interview Questions

## What is Optional in Java?

`Optional` is a container object introduced in Java 8. It may contain a non-null value or may be empty. It helps represent missing return values more clearly and reduces unsafe null handling.

## Can an Optional Object Contain Null?

No. An Optional is either:

```text
Optional[value]
```

or:

```text
Optional.empty
```

When `Optional.ofNullable(null)` is used, Java creates an empty Optional.

## What Is the Difference Between `of()` and `ofNullable()`?

- `of()` does not accept `null`
- `ofNullable()` accepts `null` and creates an empty Optional

## What Is the Difference Between `orElse()` and `orElseGet()`?

- `orElse()` creates or evaluates its default value immediately
- `orElseGet()` creates the default value only when the Optional is empty

## What Is the Difference Between `map()` and `flatMap()`?

- `map()` is used when the mapping function returns a normal value
- `flatMap()` is used when the mapping function returns an Optional

## Is Optional a Complete Replacement for Null?

No. Optional helps manage potentially absent values, especially method return values. It does not mean that `null` should or can be completely removed from Java.

## Should We Call `get()` Directly?

Generally, no. Calling `get()` on an empty Optional throws `NoSuchElementException`.

Prefer:

```java
orElse()
orElseGet()
orElseThrow()
ifPresent()
```

---

# 27. Easy Method Selection Guide

```text
Do you want to create an Optional?
    |
    +-- Value is definitely not null → of()
    |
    +-- Value may be null → ofNullable()
    |
    +-- No value is available → empty()
```

```text
Do you want to use an Optional?
    |
    +-- Perform an action if present → ifPresent()
    |
    +-- Handle present and empty cases → ifPresentOrElse()
    |
    +-- Return a simple default value → orElse()
    |
    +-- Generate a default value lazily → orElseGet()
    |
    +-- Throw an exception if empty → orElseThrow()
    |
    +-- Check a condition → filter()
    |
    +-- Transform the value → map()
    |
    +-- Transform a method returning Optional → flatMap()
```

---

# 28. Final Complete Example

```java
import java.util.Optional;

class Employee {

    private int id;
    private String name;
    private double salary;
    private Optional<String> email;

    public Employee(
            int id,
            String name,
            double salary,
            Optional<String> email) {

        this.id = id;
        this.name = name;
        this.salary = salary;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public Optional<String> getEmail() {
        return email;
    }
}

public class OptionalExample {

    public static Optional<Employee> findEmployeeById(int id) {

        if (id == 101) {
            Employee employee = new Employee(
                101,
                "Sanket",
                50000,
                Optional.of("sanket@example.com")
            );

            return Optional.of(employee);
        }

        return Optional.empty();
    }

    public static void main(String[] args) {

        String employeeName = findEmployeeById(101)
                .map(Employee::getName)
                .map(String::toUpperCase)
                .orElse("Employee not found");

        System.out.println("Name: " + employeeName);

        String employeeEmail = findEmployeeById(101)
                .flatMap(Employee::getEmail)
                .orElse("Email not available");

        System.out.println("Email: " + employeeEmail);

        double salary = findEmployeeById(101)
                .filter(emp -> emp.getSalary() >= 40000)
                .map(Employee::getSalary)
                .orElse(0.0);

        System.out.println("Salary: " + salary);
    }
}
```

Output:

```text
Name: SANKET
Email: sanket@example.com
Salary: 50000.0
```

---

# 29. Final Summary

`Optional` is a container that represents whether a value is available.

```text
Value available     → Optional[value]
Value not available → Optional.empty
```

Remember these most-used methods:

```java
Optional.of(value);             // Value must not be null

Optional.ofNullable(value);     // Value may be null

Optional.empty();               // Creates an empty Optional

optional.ifPresent(action);      // Performs action if value exists

optional.orElse(defaultValue);   // Returns value or default value

optional.orElseGet(supplier);    // Lazily creates default value

optional.orElseThrow();          // Returns value or throws exception

optional.filter(condition);      // Checks a condition

optional.map(function);          // Transforms the contained value

optional.flatMap(function);      // Handles a function returning Optional
```

The most important rule is:

> Use `Optional` mainly as a method return type when a result may or may not be available. Avoid calling `get()` directly. Prefer `orElse()`, `orElseGet()`, `orElseThrow()`, `ifPresent()`, `map()`, and `flatMap()`.
