# Functional Interfaces and Lambda Expressions in Java 8

> These notes cover one of the most important and foundational topics of Java 8.
> Master this topic to understand Streams, Method References, and modern Java coding style.

---
## Table of Contents

1. [What is a Functional Interface? 🧩](#1-what-is-a-functional-interface)
2. [Why Were Functional Interfaces Introduced? 🎯](#2-why-were-functional-interfaces-introduced)
3. [The `@FunctionalInterface` Annotation 🏷️](#3-the-functionalinterface-annotation)
4. [Rules for Functional Interfaces 📜](#4-rules-for-functional-interfaces)
5. [What is a Lambda Expression? ⚡](#5-what-is-a-lambda-expression)
6. [Why Were Lambda Expressions Introduced? 🚀](#6-why-were-lambda-expressions-introduced)
7. [Lambda Expression Syntax 📝](#7-lambda-expression-syntax)
8. [Lambda Syntax Rules 📏](#8-lambda-syntax-rules)
9. [Type Inference in Lambda Expressions 🔍](#9-type-inference-in-lambda-expressions)
10. [Lambda vs Anonymous Inner Class ⚔️](#10-lambda-vs-anonymous-inner-class)
11. [Lambda Expressions with Collections 📂](#11-lambda-expressions-with-collections)
12. [Practical Code Examples 💡](#12-practical-code-examples)
13. [Key Points to Remember 🔑](#13-key-points-to-remember)
14. [Built-in Functional Interfaces — `Function<T, R>` 🧰](#14-built-in-functional-interfaces--functiont-r)

## 1. What is a Functional Interface? 🧩

A **Functional Interface** is an interface that contains **exactly one abstract method**.

> A Functional Interface represents **one single action or task**.

### Simple Example

```java
@FunctionalInterface
interface Greeting {
    void sayHello();
}
```

Here:
- `Greeting` is a functional interface.
- It has only **one abstract method**: `sayHello()`.
- This method represents a single task — saying hello.

### The Foundation of Java 8 Features

Functional Interfaces are the **backbone** of many powerful Java 8 features:

| Feature | Why Functional Interfaces Matter |
|---|---|
| Lambda Expressions | A lambda provides the implementation for the single abstract method |
| Method References | Method references are a shorthand for lambda expressions |
| Stream API | Stream operations use built-in functional interfaces under the hood |
| Cleaner Code | Eliminates verbose anonymous class boilerplate |

---

## 2. Why Were Functional Interfaces Introduced? 🟢

Before Java 8, implementing an interface required writing a separate class or an anonymous inner class, which resulted in a lot of unnecessary boilerplate code. 
Functional Interfaces were introduced to support Lambda Expressions, allowing us to provide implementations in a much shorter, cleaner, and more readable way.

### The Problem: Before Java 8

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        // Anonymous inner class — a lot of code for a simple task
        Greeting greeting = new Greeting() {
            public void sayHello() {
                System.out.println("Hello");
            }
        };

        greeting.sayHello();
    }
}
```

The actual logic is just one line: `System.out.println("Hello")`.
But we had to write **many extra lines** of boilerplate code.

> **Boilerplate code** = Extra repeated code that you have to write even though the actual logic is very small.

### The Solution: After Java 8 (with Lambda Expression)

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting greeting = () -> System.out.println("Hello");

        greeting.sayHello();
    }
}
```

**Much shorter and cleaner.** The same result with far less code.

### Summary of Goals

Functional Interfaces were introduced to support:
- Lambda Expressions
- Method References
- Functional programming style in Java
- Cleaner, more readable code

---

## 3. The `@FunctionalInterface` Annotation 🔴

Java provides the `@FunctionalInterface` annotation to **explicitly declare** that an interface is intended to be a functional interface.

```java
@FunctionalInterface
interface MyInterface {
    void show();
}
```

### What Does It Do?

It tells the **compiler** to enforce the one-abstract-method rule.

If you accidentally add a second abstract method, the compiler immediately gives an **error**:

```java
@FunctionalInterface
interface MyInterface {
    void show();
    void display(); // Compile-time error!
}
```

### Is `@FunctionalInterface` Mandatory?

**No.** An interface with a single abstract method is still a functional interface even without the annotation:

```java
interface MyInterface {
    void show(); // Still a valid functional interface
}
```

However, using `@FunctionalInterface` is **strongly recommended** because:

| Benefit | Explanation |
|---|---|
| Readability | Clearly shows the intent of the interface |
| Error prevention | Compiler catches accidental second abstract methods |
| Documentation | Other developers immediately understand the purpose |
| Best practice | Shows professional awareness in interviews and real projects |

---

## 4. Rules for Functional Interfaces 🟧

### Rule 1: Exactly One Abstract Method

A functional interface **must** have exactly one abstract method. No more, no less.

```java
@FunctionalInterface
interface Payment {
    void pay(); // Exactly one abstract method — valid
}
```

```java
@FunctionalInterface
interface Payment {
    void pay();
    void refund(); // Invalid — two abstract methods
}
```

### Rule 2: `Object` Class Methods Do Not Count

Methods inherited from `java.lang.Object` (like `toString()`, `equals()`, `hashCode()`) do **not** count as abstract methods of a functional interface.

```java
@FunctionalInterface
interface MyInterface {
    void show();        // The one required abstract method

    String toString();  // Belongs to Object class — does not count
}
```

This is still a valid functional interface because `toString()` is part of `Object`.

### Rule 3: Default and Static Methods Are Allowed

A functional interface **can** have any number of `default` and `static` methods. They do not violate the one-abstract-method rule.

```java
@FunctionalInterface
interface Vehicle {
    void start();           // Abstract method — the one required method

    default void stop() {   // Default method — allowed
        System.out.println("Vehicle stopped");
    }

    static void serviceInfo() { // Static method — allowed
        System.out.println("Service every 6 months");
    }
}
```

### Complete Example with Default and Static Methods

```java
@FunctionalInterface
interface Vehicle {
    void start();

    default void stop() {
        System.out.println("Vehicle stopped");
    }

    static void serviceInfo() {
        System.out.println("Service every 6 months");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = () -> System.out.println("Car started");

        car.start();           // Calls the lambda
        car.stop();            // Calls the default method
        Vehicle.serviceInfo(); // Calls the static method
    }
}
```

**Output:**
```
Car started
Vehicle stopped
Service every 6 months
```

---

## 5. What is a Lambda Expression? 🟣

A **Lambda Expression** is an **anonymous function** — a function without a name.

> A Lambda Expression is a short, concise way to write the implementation of a Functional Interface's abstract method.

Normally in Java, we write methods inside a class with a proper name:

```java
public void sayHello() {
    System.out.println("Hello");
}
```

With a Lambda Expression, the same logic can be written as:

```java
() -> System.out.println("Hello");
```

### Breaking Down the Lambda

```java
() -> System.out.println("Hello")
```

| Part | Meaning |
|---|---|
| `()` | No input parameters |
| `->` | The **lambda arrow operator** (separates parameters from body) |
| `System.out.println("Hello")` | The logic to execute |

### Real-Life Analogy

**Without Lambda:**
> "Create a worker named John. Tell John that whenever I ask him to greet, he should say Hello."

**With Lambda:**
> "Whenever asked, just say Hello."

Lambda removes unnecessary extra details and focuses only on the actual work.

---

## 6. Why Were Lambda Expressions Introduced? ⚪

Before Java 8, passing behavior to a method required verbose code. Lambda Expressions were introduced to solve this by:

- **Reducing boilerplate code** — write only the logic, not the structure
- **Making code more readable** — focus on *what* is happening, not *how*
- **Supporting functional programming** style in Java
- **Simplifying collection operations** — `forEach`, `sort`, `filter`, etc.
- **Improving Stream API usage** — lambdas and streams go hand-in-hand

---

## 7. Lambda Expression Syntax 🟨

### Basic Syntax

```java
(parameters) -> { body }
```

### Syntax Variations

#### No parameters, single statement

```java
() -> System.out.println("Hello");
```

#### One parameter, single statement (parentheses optional)

```java
name -> System.out.println("Hello " + name);

// Also valid:
(name) -> System.out.println("Hello " + name);
```

#### Multiple parameters, single expression

```java
(a, b) -> a + b;
```

The result of `a + b` is automatically returned — no `return` keyword needed.

#### Multiple parameters, multiple statements (braces required)

```java
(a, b) -> {
    int sum = a + b;
    return sum;
};
```

When using `{}` braces with multiple statements, you **must** use the `return` keyword explicitly.

---

## 8. Lambda Syntax Rules 🟣

Understanding these rules will help you write lambdas correctly and confidently.

| Rule | Description | Example |
|---|---|---|
| **Zero parameters** | Parentheses are **required** | `() -> ...` |
| **One parameter** | Parentheses are **optional** | `name -> ...` or `(name) -> ...` |
| **Multiple parameters** | Parentheses are **required** | `(a, b) -> ...` |
| **Single statement body** | Braces are **optional** | `(a, b) -> a + b` |
| **Multiple statement body** | Braces are **required** | `(a, b) -> { ... }` |
| **Return (no braces)** | `return` keyword is **optional** | `(a, b) -> a + b` (returns automatically) |
| **Return (with braces)** | `return` keyword is **required** | `(a, b) -> { return a + b; }` |

---

## 9. Type Inference in Lambda Expressions 🟡

In Lambda Expressions, Java can **automatically infer** the data type of parameters from the Functional Interface method signature.

### Example

Suppose you have:

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

You can write the lambda without specifying types:

```java
Calculator calculator = (a, b) -> a + b;
```

Even though we did not write `(int a, int b)`, Java knows from the `Calculator` interface that `a` and `b` are `int`. This is called **type inference**.

This keeps lambda expressions concise and clean.

---

## 10. Lambda vs Anonymous Inner Class 🔶

| Feature | Anonymous Inner Class | Lambda Expression |
|---|---|---|
| Code length | More verbose | Short and concise |
| Readability | Harder to read | Easy to read |
| Usage | Works with any interface or abstract class | Works **only** with Functional Interfaces |
| Boilerplate | A lot of extra boilerplate | Eliminates boilerplate |
| Introduced | Before Java 8 | Java 8 onwards |

### Side-by-Side Comparison

**Anonymous Inner Class:**

```java
Greeting greeting = new Greeting() {
    public void sayHello() {
        System.out.println("Hello");
    }
};
```

**Lambda Expression:**

```java
Greeting greeting = () -> System.out.println("Hello");
```

Both do the same thing. Lambda is the preferred modern approach.

> **Important:** Lambda Expressions can be used **only** with Functional Interfaces. This is because a lambda provides the implementation for exactly **one** abstract method. If an interface has multiple abstract methods, the lambda would not know which one to implement.

---

## 11. Lambda Expressions with Collections 🔷💠

Lambda Expressions work seamlessly with Java Collections, making operations like iteration and sorting much cleaner.

### Iterating a List

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Sanket", "Rahul", "Amit", "Priya");

        names.forEach(name -> System.out.println(name));
    }
}
```

**Output:**
```
Sanket
Rahul
Amit
Priya

            } else {
                return "Fail";
            }
        };

        System.out.println("95 marks -> Grade: " + getGrade.apply(95));
        System.out.println("78 marks -> Grade: " + getGrade.apply(78));
        System.out.println("55 marks -> Grade: " + getGrade.apply(55));
        System.out.println("30 marks -> Grade: " + getGrade.apply(30));
    }
}
```

**Output:**
```
95 marks -> Grade: A+
78 marks -> Grade: A
55 marks -> Grade: B
30 marks -> Grade: Fail
```

**Explanation:**
- `T` = `Integer` (marks scored)
- `R` = `String` (the grade)
- The lambda uses `{}` braces because it contains multiple statements (if-else logic).
- The `return` keyword is required inside braces.
- This shows that `Function` is not limited to simple one-line transformations — it can handle **complex logic** too.

---

## 12. Practical Code Examples 🟧

### Example 1: No Parameter, No Return Value

```java
@FunctionalInterface
interface Message {
    void showMessage();
}

public class Main {
    public static void main(String[] args) {
        Message message = () -> System.out.println("Welcome to Java 8");

        message.showMessage();
    }
}
```

**Output:** `Welcome to Java 8`

The lambda `()` means no input is needed, and the body just prints a message.

---

### Example 2: One Parameter

```java
@FunctionalInterface
interface Printer {
    void print(String message);
}

public class Main {
    public static void main(String[] args) {
        Printer printer = message -> System.out.println(message);

        printer.print("Hello Sanket");
    }
}
```

**Output:** `Hello Sanket`

With a single parameter, parentheses around `message` are optional.

---

### Example 3: Two Parameters, Return Value

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        Calculator calculator = (a, b) -> a + b;

        int result = calculator.add(10, 20);

        System.out.println(result);
    }
}
```

**Output:** `30`

The expression `a + b` is automatically returned — no `return` keyword needed for a single expression.

---

### Example 4: Multiple Statements in Lambda Body

```java
@FunctionalInterface
interface Calculator {
    int multiply(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        Calculator calculator = (a, b) -> {
            int result = a * b;
            return result;
        };

        System.out.println(calculator.multiply(5, 4));
    }
}
```

**Output:** `20`

When the lambda body has multiple lines, curly braces `{}` are required, and `return` must be used explicitly.

---

### Example 5: Lambda with the `Runnable` Interface

`Runnable` is a **built-in Functional Interface** with one abstract method: `void run()`. It is commonly used for threads.

**Before Java 8:**

```java
public class Main {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            public void run() {
                System.out.println("Thread is running");
            }
        };

        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```

**Using Lambda Expression:**

```java
public class Main {
    public static void main(String[] args) {
        Runnable runnable = () -> System.out.println("Thread is running");

        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```

**Output:** `Thread is running`

The lambda replaces the entire anonymous inner class with a single line.


---

## 13. Key Points to Remember

- Lambda Expression was introduced in **Java 8**.
- It is an **anonymous function** — a function without a name.
- A Lambda Expression can be used **only** with a **Functional Interface**.
- A **Functional Interface** has **exactly one abstract method**.
- The lambda operator is `->` (arrow operator).
- Basic syntax: `(parameters) -> { body }`
- **Parentheses** are optional for **one parameter**, required for zero or multiple parameters.
- **Braces** `{}` are optional for a **single statement**, required for multiple statements.
- The `return` keyword is **optional** for a single expression, **required** inside `{}` braces when returning a value.
- Java uses **type inference** to automatically determine parameter types in lambdas.
- The `@FunctionalInterface` annotation is **not mandatory** but is a **best practice**.
- Methods from `java.lang.Object` (like `toString()`) do **not** count as abstract methods of a functional interface.
- A functional interface can have **multiple default and static methods** — they do not violate the rule.
- Lambda Expressions eliminate boilerplate code and make code **shorter, cleaner, and more readable**.
- Commonly used with: **Collections, Streams, `Runnable`, `Comparator`, `Predicate`, `Function`, `Consumer`, `Supplier`**.

---

## 14. Built-in Functional Interfaces — `Function<T, R>` 🔷🟦

Java 8 introduced several **built-in Functional Interfaces** in the `java.util.function` package. These are ready-made interfaces that cover the most common use cases, so you do not have to create your own every time.

In this section, we will learn about the most versatile one: **`Function<T, R>`**.

---

### 14.1 What is `Function<T, R>`?

`Function<T, R>` is a built-in Functional Interface that:

- **Takes one input** of type `T`
- **Processes it** (applies some logic)
- **Returns one output** of type `R`

In simple words:

> `Function<T, R>` is like a **machine** — you put something in, and it gives you something back.

It is defined in the `java.util.function` package like this:

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

Here:

| Symbol | Full Name | Meaning |
|---|---|---|
| `T` | **Type** (Input) | The type of data that goes **into** the function |
| `R` | **Result** (Output) | The type of data that comes **out** of the function |

> **Key Point:** The input type `T` and the output type `R` can be the **same** type or **different** types. That is what makes `Function` so flexible.

---

### 14.2 Why Was `Function<T, R>` Introduced?

Function<T, R> was introduced in Java 8 to avoid creating separate interfaces and classes for simple data transformation logic. It allows us to write the transformation directly using a lambda expression, making the code shorter, cleaner, and easier to read.



**Without `Function` (the old way):**

```java
// Step 1: Create an interface
interface StringConverter {
    String convert(String input);
}

// Step 2: Implement it
class UpperCaseConverter implements StringConverter {
    public String convert(String input) {
        return input.toUpperCase();
    }
}

// Step 3: Use it
public class Main {
    public static void main(String[] args) {
        StringConverter converter = new UpperCaseConverter();
        System.out.println(converter.convert("hello")); // HELLO
    }
}
```

**With `Function` (the Java 8 way):**

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> toUpperCase = input -> input.toUpperCase();

        System.out.println(toUpperCase.apply("hello")); // HELLO
    }
}
```

The entire transformation is written in **one line**. No extra interfaces, no extra classes.

---

### 14.3 When Should You Use `Function<T, R>`?

Use `Function<T, R>` whenever you need to **transform** or **convert** data from one form to another.

Common scenarios:

| Scenario | Input (`T`) | Output (`R`) |
|---|---|---|
| Convert text to uppercase | `String` | `String` |
| Find the length of a string | `String` | `Integer` |
| Square a number | `Integer` | `Integer` |
| Get the name from a Person object | `Person` | `String` |
| Calculate salary after tax | `Double` | `Double` |

The simple rule is:

> If you have **one input** and want to get **one output** after applying some logic — use `Function<T, R>`.

---

### 14.4 Understanding T and R with a Real-World Analogy

#### The Juice Machine Analogy

Imagine a **juice machine**:

- **You put in** a fruit (e.g., an orange) → This is `T` (Input)
- **The machine processes it** (squeezes the fruit) → This is the `apply()` method
- **You get back** juice (e.g., orange juice) → This is `R` (Output)

```
[Orange]  →  [ Juice Machine ]  →  [Orange Juice]
   T              apply()               R
```

- The **input type** (`T`) is `Fruit`
- The **output type** (`R`) is `Juice`
- The **machine** is the `Function`
- **Pressing the button** is calling `apply()`

In code, this would look like:

```java
Function<String, String> juiceMachine = fruit -> fruit + " Juice";

System.out.println(juiceMachine.apply("Orange"));  // Orange Juice
System.out.println(juiceMachine.apply("Apple"));   // Apple Juice
System.out.println(juiceMachine.apply("Mango"));   // Mango Juice
```

The **same machine** (same `Function`) can process **different inputs** and give the **corresponding output** every time.

---

### 14.5 How the `apply()` Method Works

`apply()` is the **single abstract method** of the `Function<T, R>` interface.

**Signature:**

```java
R apply(T t);
```

**How it works — step by step:**

1. You create a `Function` using a lambda expression — this defines the logic.
2. You call `apply()` and pass an input value of type `T`.
3. The lambda logic runs on that input.
4. The result of type `R` is returned.

**Example:**

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Step 1: Define the logic — multiply the number by 2
        Function<Integer, Integer> doubleIt = number -> number * 2;

        // Step 2: Call apply() with an input
        Integer result = doubleIt.apply(5);

        // Step 3: Use the result
        System.out.println(result); // 10
    }
}
```

**Output:**
```
10
```

**Flow of data:**

```
Input (T = Integer):   5
      ↓
Logic (apply):         number -> number * 2
      ↓
Output (R = Integer):  10
```

> **Remember:** You define the logic **once** when creating the `Function`, and then you can call `apply()` **as many times** as you want with different inputs.

```java
System.out.println(doubleIt.apply(5));   // 10
System.out.println(doubleIt.apply(15));  // 30
System.out.println(doubleIt.apply(100)); // 200
```

---

### 14.6 Summary Table

| Aspect | Detail |
|---|---|
| **Package** | `java.util.function` |
| **Interface name** | `Function<T, R>` |
| **Abstract method** | `R apply(T t)` |
| **Input** | One input of type `T` |
| **Output** | One output of type `R` |
| **Purpose** | Transform / convert input into output |
| **Analogy** | A machine — put something in, get something out |

---

### 14.7 `Function<T, R>` — Code Examples

Below are **5 examples**, arranged from basic to practical, to help you master `Function<T, R>`.

---

#### Example 1: Convert a String to Uppercase

This is the simplest possible use — both input and output are `String`.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> toUpperCase = str -> str.toUpperCase();

        String result = toUpperCase.apply("hello world");

        System.out.println(result);
    }
}
```

**Output:**
```
HELLO WORLD
```

**Explanation:**
- `T` = `String` (the input text)
- `R` = `String` (the uppercase version of the text)
- The lambda `str -> str.toUpperCase()` takes a string and returns its uppercase form.
- `apply("hello world")` passes the input and triggers the transformation.

---

#### Example 2: Find the Length of a String

Here, the input type and output type are **different** — input is `String`, output is `Integer`.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, Integer> findLength = str -> str.length();

        Integer length = findLength.apply("Sanket");

        System.out.println("Length: " + length);
    }
}
```

**Output:**
```
Length: 6
```

**Explanation:**
- `T` = `String` (the input word)
- `R` = `Integer` (the length of that word)
- The lambda `str -> str.length()` converts a `String` into its length.
- This is a great example of how `T` and `R` can be completely different types.

---

#### Example 3: Square an Integer

Both input and output are `Integer`, but the value is transformed.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<Integer, Integer> square = number -> number * number;

        System.out.println("Square of 5: " + square.apply(5));
        System.out.println("Square of 9: " + square.apply(9));
        System.out.println("Square of 12: " + square.apply(12));
    }
}
```

**Output:**
```
Square of 5: 25
Square of 9: 81
Square of 12: 144
```

**Explanation:**
- `T` = `Integer`, `R` = `Integer`
- The lambda `number -> number * number` multiplies the number by itself.
- Notice how we call `apply()` multiple times with different values — the same function is **reusable**.

---

#### Example 4: Extract a Property from an Object

This is a very common real-world use case — extracting a specific field from a custom object.

```java
import java.util.function.Function;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge()     { return age; }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person("Sanket", 25);

        // Extract name from Person
        Function<Person, String> getName = p -> p.getName();
        System.out.println("Name: " + getName.apply(person));

        // Extract age from Person
        Function<Person, Integer> getAge = p -> p.getAge();
        System.out.println("Age: " + getAge.apply(person));
    }
}
```

**Output:**
```
Name: Sanket
Age: 25
```

**Explanation:**
- `Function<Person, String>` means: *"Give me a `Person`, and I will give you back a `String`."*
- `Function<Person, Integer>` means: *"Give me a `Person`, and I will give you back an `Integer`."*
- This pattern is extremely useful when working with lists of objects and the Stream API.

---

#### Example 5: Calculate the Grade Based on Marks

A practical example that uses logic inside the lambda body to return a result.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<Integer, String> getGrade = marks -> {
            if (marks >= 90) {
                return "A+";
            } else if (marks >= 75) {
                return "A";
            } else if (marks >= 60) {
                return "B";
            } else if (marks >= 40) {
                return "C";
            } else {
                return "Fail";
            }
        };

        System.out.println("95 marks -> Grade: " + getGrade.apply(95));
        System.out.println("78 marks -> Grade: " + getGrade.apply(78));
        System.out.println("55 marks -> Grade: " + getGrade.apply(55));
        System.out.println("30 marks -> Grade: " + getGrade.apply(30));
    }
}
```

**Output:**
```
95 marks -> Grade: A+
78 marks -> Grade: A
55 marks -> Grade: B
30 marks -> Grade: Fail
```

**Explanation:**
- `T` = `Integer` (marks scored)
- `R` = `String` (the grade)
- The lambda uses `{}` braces because it contains multiple statements (if-else logic).
- The `return` keyword is required inside braces.
- This shows that `Function` is not limited to simple one-line transformations — it can handle **complex logic** too.

---

#### Example 6 : `Function` with an Employee Object

```java
import java.util.function.Function;

class Employee {
    private String name;
    private Long phoneNumber;
    private String gender;

    public Employee(String name, Long phoneNumber, String gender) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.gender = gender;
    }

    String getName()        { return this.name; }
    Long getPhoneNumber()   { return this.phoneNumber; }
    String getGender()      { return this.gender; }
}

public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee("Sanket", 9887766554L, "Male");

        // Extract phone number
        Function<Employee, Long> getPhoneNumber = e -> e.getPhoneNumber();
        Long empPhoneNumber = getPhoneNumber.apply(emp);
        System.out.println("Phone number of Employee is : " + empPhoneNumber);

        // Extract gender
        Function<Employee, String> genderOfEmployee = e -> e.getGender();
        String gender = genderOfEmployee.apply(emp);
        System.out.println("Gender : " + gender);
    }
}
```

**Output:**
```
Phone number of Employee is : 9887766554
Gender : Male
```

`Function<Employee, Long>` reads: *"Take an `Employee` as input, return a `Long`."*

---

#### Example 7 : `Function` to Calculate Average Salary

```java
import java.util.function.Function;

class Employee2 {
    private String name;
    private int salary;

    public Employee2(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    public String getName()  { return name; }
    public int getSalary()   { return salary; }
}

public class Main3 {
    public static void main(String[] args) {
        Employee2[] employees = {
                new Employee2("Sanket", 50000),
                new Employee2("Ravi",   60000),
                new Employee2("Neha",   55000),
                new Employee2("Amit",   65000)
        };

        Function<Employee2[], Double> getAverageSalary = emps -> {
            int sum = 0;
            for (Employee2 e : employees) {
                sum = sum + e.getSalary();
            }
            return (double) sum / employees.length;
        };

        double avg = getAverageSalary.apply(employees);
        System.out.println("Average Salary = " + avg);
    }
}
```

**Output:** `Average Salary = 57500.0`

This demonstrates using `Function` with a multi-statement lambda body to process an array of objects.

---

#### Example 8 : `Function` to Filter Female Employees

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;

class Employee3 {
    private String name;
    private String gender;
    private int salary;

    public Employee3(String name, String gender, int salary) {
        this.name = name;
        this.gender = gender;
        this.salary = salary;
    }

    public String getName()   { return name; }
    public String getGender() { return gender; }
    public int getSalary()    { return salary; }
}

public class Main4 {
    public static void main(String[] args) {
        List<Employee3> employees = new ArrayList<>();
        employees.add(new Employee3("Sanket", "Male",   50000));
        employees.add(new Employee3("Neha",   "Female", 45000));
        employees.add(new Employee3("Ravi",   "Male",   55000));
        employees.add(new Employee3("Priya",  "Female", 65000));

        Function<List<Employee3>, List<Employee3>> getFemaleEmployees = emps -> {
            List<Employee3> females = new ArrayList<>();
            for (Employee3 e : emps) {
                if ("Female".equalsIgnoreCase(e.getGender())) {
                    females.add(e);
                }
            }
            return females;
        };

        List<Employee3> femaleList = getFemaleEmployees.apply(employees);
        System.out.println("Female Employees:");

        for (Employee3 f : femaleList) {
            System.out.println(f.getName() + " - " + f.getSalary());
        }
    }
}
```

**Output:**
```
Female Employees:
Neha - 45000
Priya - 65000
```

`Function<List<Employee3>, List<Employee3>>` reads: *"Take a list of employees, return a filtered list of employees."*

---

#### Example 9 : `Function` to Get Total Marks of a Student

```java
import java.util.function.Function;

class Student {
    private int Physics;
    private int Maths;
    private int Chemistry;

    public Student(int Physics, int Maths, int Chemistry) {
        this.Physics = Physics;
        this.Maths = Maths;
        this.Chemistry = Chemistry;
    }

    public Integer getSum() {
        return this.Physics + this.Chemistry + this.Maths;
    }
}

public class Main2 {
    public static void main(String[] args) {
        Student std = new Student(56, 50, 87);

        Function<Student, Integer> getSum = s -> s.getSum();

        Integer result = getSum.apply(std);

        System.out.println("Sum of Marks of Student : " + result);
    }
}
```

**Output:** `Sum of Marks of Student : 193`

---

