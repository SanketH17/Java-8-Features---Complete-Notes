# Collector Class in Java Stream API

## What is a Collector?

A **Collector** is an object that **collects the elements of a Stream and converts them into a final result**.

The final result can be:

- A `List`
- A `Set`
- A `Map`
- A `String`
- A Count
- Statistics (sum, average, min, max)
- Grouped data
- Partitioned data
- And much more...

Simply put,

> **Collector = A tool that tells Stream how to gather the processed data.**

---

## Why do we need Collectors?

Suppose we filter some data from a stream.

```java
List<String> names = List.of("Amit", "Rahul", "Priya", "Rohit");

names.stream()
     .filter(name -> name.startsWith("R"));
```

After filtering...

```
Rahul
Rohit
```

But where should these values go?

- Into a List?
- Into a Set?
- Into a Map?
- Into a String?

A **Collector** answers this question.

---

## Without Collector

Imagine writing this manually.

```java
List<String> result = new ArrayList<>();

for (String name : names) {
    if (name.startsWith("R")) {
        result.add(name);
    }
}
```

---

## With Collector

```java
List<String> result = names.stream()
                           .filter(name -> name.startsWith("R"))
                           .collect(Collectors.toList());
```

Much shorter and easier.

---

# Where is Collector used?

Collectors are mainly used with

```java
collect()
```

method.

General syntax:

```java
stream.collect(Collectors.someMethod());
```

Example

```java
List<Integer> list = numbers.stream()
                            .collect(Collectors.toList());
```

---

# How collect() Works

```
Stream
   │
   ▼
Process Elements
(filter/map/etc.)
   │
   ▼
Collector
   │
   ▼
Final Result
(List / Set / Map / String / Statistics ...)
```

---

# Collectors Utility Class

Java provides a utility class

```java
java.util.stream.Collectors
```

which contains many ready-made collectors.

Example

```java
Collectors.toList()

Collectors.toSet()

Collectors.toMap()

Collectors.joining()

Collectors.groupingBy()

Collectors.partitioningBy()

Collectors.counting()

Collectors.summingInt()

Collectors.averagingDouble()

Collectors.summarizingInt()
```

---

# Most Important Collector Methods

## 1. Collectors.toList()

Collects stream elements into a List.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class ToListExample {
    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rohit"
        );

        List<String> result = names.stream()
                                   .collect(Collectors.toList());

        System.out.println(result);
    }
}
```

### Output

```
[Rahul, Amit, Rohit]
```

### Explanation

The stream elements are collected into a **List** while preserving the order.

---

## 2. Collectors.toSet()

Collects stream elements into a Set.

### Example

```java
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;

public class ToSetExample {
    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rahul",
                "Rohit"
        );

        Set<String> result = names.stream()
                                  .collect(Collectors.toSet());

        System.out.println(result);
    }
}
```

### Output

```
[Amit, Rahul, Rohit]
```

### Explanation

A `Set` stores only unique elements, so duplicate `"Rahul"` is removed.

---

## 3. Collectors.toCollection()

Collects elements into a specific collection type chosen by you.

### Example

```java
import java.util.LinkedList;
import java.util.List;
import java.util.stream.Collectors;

public class ToCollectionExample {

    public static void main(String[] args) {

        List<Integer> numbers = List.of(10, 20, 30);

        LinkedList<Integer> list = numbers.stream()
                                          .collect(Collectors.toCollection(LinkedList::new));

        System.out.println(list);
    }
}
```

### Output

```
[10, 20, 30]
```

### Explanation

Instead of the default collection, you can choose your own collection type like `LinkedList`, `TreeSet`, or `ArrayDeque`.

---

## 4. Collectors.toMap()

Converts stream elements into a Map.

### Example

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class ToMapExample {

    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rohit"
        );

        Map<String, Integer> map = names.stream()
                                        .collect(Collectors.toMap(
                                                name -> name,
                                                name -> name.length()
                                        ));

        System.out.println(map);
    }
}
```

### Output

```
{Rahul=5, Amit=4, Rohit=5}
```

### Explanation

- Key → Name
- Value → Length of the name

The collector creates a `Map<Key, Value>` from the stream elements.

> **Note:** Keys must be unique. Duplicate keys will throw an exception unless a merge function is provided.

---

## 5. Collectors.joining()

Combines all String elements into one String.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class JoiningExample {

    public static void main(String[] args) {

        List<String> words = List.of(
                "Java",
                "Stream",
                "API"
        );

        String result = words.stream()
                             .collect(Collectors.joining(" "));

        System.out.println(result);
    }
}
```

### Output

```
Java Stream API
```

### Explanation

All strings are joined into one string using the specified delimiter (`" "`).

---

### Joining with Prefix and Suffix

```java
String result = words.stream()
                     .collect(Collectors.joining(", ", "[", "]"));

System.out.println(result);
```

### Output

```
[Java, Stream, API]
```

---

## 6. Collectors.counting()

Counts the number of elements.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class CountingExample {

    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rohit"
        );

        long count = names.stream()
                          .collect(Collectors.counting());

        System.out.println(count);
    }
}
```

### Output

```
3
```

### Explanation

Returns the total number of elements in the stream.

---

## 7. Collectors.summingInt()

Calculates the sum.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class SummingExample {

    public static void main(String[] args) {

        List<Integer> numbers = List.of(
                10,
                20,
                30,
                40
        );

        int sum = numbers.stream()
                         .collect(Collectors.summingInt(n -> n));

        System.out.println(sum);
    }
}
```

### Output

```
100
```

### Explanation

Adds all integer values and returns the total sum.

---

## 8. Collectors.averagingInt()

Calculates the average.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class AverageExample {

    public static void main(String[] args) {

        List<Integer> marks = List.of(
                80,
                90,
                70
        );

        double average = marks.stream()
                              .collect(Collectors.averagingInt(m -> m));

        System.out.println(average);
    }
}
```

### Output

```
80.0
```

### Explanation

Returns the average of all integer values as a `double`.

---

## 9. Collectors.summarizingInt()

Returns multiple statistics in one object.

### Example

```java
import java.util.IntSummaryStatistics;
import java.util.List;
import java.util.stream.Collectors;

public class SummaryExample {

    public static void main(String[] args) {

        List<Integer> numbers = List.of(
                10,
                20,
                30,
                40,
                50
        );

        IntSummaryStatistics stats = numbers.stream()
                                            .collect(Collectors.summarizingInt(n -> n));

        System.out.println(stats);
    }
}
```

### Output

```
IntSummaryStatistics{
count=5,
sum=150,
min=10,
average=30.0,
max=50
}
```

### Explanation

One collector gives you:

- Count
- Sum
- Minimum
- Maximum
- Average

---

## 10. Collectors.mapping()

Transforms elements before collecting them.

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class MappingExample {

    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rohit"
        );

        List<Integer> lengths = names.stream()
                                     .collect(Collectors.mapping(
                                             String::length,
                                             Collectors.toList()
                                     ));

        System.out.println(lengths);
    }
}
```

### Output

```
[5, 4, 5]
```

### Explanation

Each name is converted into its length before collecting into a list.

---

# Grouping Data using Collectors

One of the most powerful features of Collectors is **grouping**.

Instead of storing everything in one list, grouping organizes elements into different groups based on a condition or property.

---

## Collectors.groupingBy()

Groups elements based on a classifier function.

### Example

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class GroupingExample {

    public static void main(String[] args) {

        List<String> names = List.of(
                "Rahul",
                "Amit",
                "Rohit",
                "Raj",
                "Priya"
        );

        Map<Integer, List<String>> grouped = names.stream()
                                                  .collect(Collectors.groupingBy(String::length));

        System.out.println(grouped);
    }
}
```

### Output

```
{
3=[Raj],
4=[Amit],
5=[Rahul, Rohit, Priya]
}
```

### Explanation

Names are grouped based on their length.

---

## Grouping Employees by Department

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

class Employee {

    String name;
    String department;

    Employee(String name, String department) {
        this.name = name;
        this.department = department;
    }

    public String getDepartment() {
        return department;
    }

    @Override
    public String toString() {
        return name;
    }
}

public class GroupEmployeeExample {

    public static void main(String[] args) {

        List<Employee> employees = List.of(
                new Employee("Rahul", "IT"),
                new Employee("Amit", "HR"),
                new Employee("Rohit", "IT"),
                new Employee("Priya", "HR")
        );

        Map<String, List<Employee>> map = employees.stream()
                .collect(Collectors.groupingBy(Employee::getDepartment));

        System.out.println(map);
    }
}
```

### Output

```
{
IT=[Rahul, Rohit],
HR=[Amit, Priya]
}
```

### Explanation

Employees are grouped based on their department.

---

# Partitioning Data using Collectors

Partitioning divides data into **exactly two groups**.

- true
- false

---

## Collectors.partitioningBy()

### Example

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class PartitionExample {

    public static void main(String[] args) {

        List<Integer> numbers = List.of(
                10,
                15,
                22,
                31,
                40
        );

        Map<Boolean, List<Integer>> map = numbers.stream()
                .collect(Collectors.partitioningBy(n -> n % 2 == 0));

        System.out.println(map);
    }
}
```

### Output

```
{
true=[10, 22, 40],
false=[15, 31]
}
```

### Explanation

The collector creates two groups:

- `true` → Even numbers
- `false` → Odd numbers

Unlike `groupingBy()`, `partitioningBy()` always creates exactly two groups.

---

# Reducing Data using Collectors

Sometimes we want a **single result** from many values.

Examples:

- Total salary
- Total marks
- Product of numbers
- Maximum value

Collectors provide `reducing()` for such operations.

---

## Collectors.reducing()

### Example

```java
import java.util.List;
import java.util.stream.Collectors;

public class ReducingExample {

    public static void main(String[] args) {

        List<Integer> numbers = List.of(
                10,
                20,
                30,
                40
        );

        int sum = numbers.stream()
                .collect(Collectors.reducing(
                        0,
                        Integer::intValue,
                        Integer::sum
                ));

        System.out.println(sum);
    }
}
```

### Output

```
100
```

### Explanation

The collector combines all values into a single result (their sum). While `Stream.reduce()` is often preferred for simple reductions, `Collectors.reducing()` is useful inside collectors such as `groupingBy()`.

---

# groupingBy() vs partitioningBy()

| Feature | groupingBy() | partitioningBy() |
|----------|--------------|------------------|
| Number of groups | Any number | Exactly 2 |
| Return Type | `Map<K, List<T>>` | `Map<Boolean, List<T>>` |
| Based On | Any key | Boolean condition |
| Example | Group by department | Even vs Odd |

---

# Commonly Used Collectors

| Collector | Purpose |
|-----------|---------|
| `toList()` | Collect into List |
| `toSet()` | Collect into Set |
| `toCollection()` | Collect into a specific collection |
| `toMap()` | Convert to Map |
| `joining()` | Join Strings |
| `counting()` | Count elements |
| `mapping()` | Transform while collecting |
| `groupingBy()` | Group elements |
| `partitioningBy()` | Split into true/false groups |
| `summingInt()` | Sum values |
| `averagingInt()` | Average values |
| `summarizingInt()` | Count, sum, min, max, average |
| `reducing()` | Reduce to a single value |

---

# Collector Flow Diagram

```
                Stream
                   │
                   ▼
        filter() / map() / sorted()
                   │
                   ▼
              collect()
                   │
                   ▼
            Collectors Method
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
     List       Set        Map
        │
        ├──────────────► joining()
        ├──────────────► counting()
        ├──────────────► groupingBy()
        ├──────────────► partitioningBy()
        ├──────────────► summarizingInt()
        └──────────────► reducing()
```

---

# Quick Summary

- A **Collector** defines **how stream elements should be gathered into a final result**.
- The `collect()` method works together with the `Collectors` utility class.
- `Collectors.toList()` creates a `List`.
- `Collectors.toSet()` creates a `Set` with unique elements.
- `Collectors.toMap()` creates a `Map` using key-value mappings.
- `Collectors.joining()` combines strings into one string.
- `Collectors.counting()` counts stream elements.
- `Collectors.summingInt()` and `Collectors.averagingInt()` calculate numeric results.
- `Collectors.summarizingInt()` provides count, sum, min, max, and average together.
- `Collectors.mapping()` transforms elements before collecting.
- `Collectors.groupingBy()` organizes data into multiple groups.
- `Collectors.partitioningBy()` splits data into exactly two groups based on a boolean condition.
- `Collectors.reducing()` combines multiple values into a single result.
- Collectors make Stream code shorter, cleaner, and easier to read.