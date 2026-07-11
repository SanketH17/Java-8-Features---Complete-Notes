# Functional Interfaces and Lambda Expressions in Java 8

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
15. [Built-in Functional Interfaces — `Predicate<T>` ✅](#15-built-in-functional-interfaces--predicatet)
16. [Built-in Functional Interfaces — `Consumer<T>` 📥](#16-built-in-functional-interfaces--consumert)
17. [Built-in Functional Interfaces — `Supplier<T>` 📤](#17-built-in-functional-interfaces--suppliert)

## 1. What is a Functional Interface?🧩

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

## 2. Why Were Functional Interfaces Introduced?🟢

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

## 3. The `@FunctionalInterface` Annotation🔴

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

## 4. Rules for Functional Interfaces🟧

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

## 5. What is a Lambda Expression?🟣

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

## 6. Why Were Lambda Expressions Introduced?⚪

Before Java 8, passing behavior to a method required verbose code. Lambda Expressions were introduced to solve this by:

- **Reducing boilerplate code** — write only the logic, not the structure
- **Making code more readable** — focus on *what* is happening, not *how*
- **Supporting functional programming** style in Java
- **Simplifying collection operations** — `forEach`, `sort`, `filter`, etc.
- **Improving Stream API usage** — lambdas and streams go hand-in-hand

---

## 7. Lambda Expression Syntax🟨

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

## 8. Lambda Syntax Rules🟣

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

## 9. Type Inference in Lambda Expressions🟡

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

## 10. Lambda vs Anonymous Inner Class🔶

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

## 11. Lambda Expressions with Collections🔷💠

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

## 12. Practical Code Examples🟧

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

## 14. Built-in Functional Interfaces — `Function<T, R>`🔷🟦

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

## 15. Built-in Functional Interfaces — `Predicate<T>` ✅

In the previous section, we learned about `Function<T, R>` which transforms data. Now let us learn about another very commonly used built-in Functional Interface: **`Predicate<T>`**.

While `Function` is about **transforming** data, `Predicate` is about **testing** data.

---

### 15.1 What is `Predicate<T>`?

`Predicate<T>` is a built-in Functional Interface that:

- **Takes one input** of type `T`
- **Tests it** against a condition
- **Returns a `boolean`** — either `true` or `false`

In simple words:

> `Predicate<T>` is like a **question that can only be answered with Yes or No**.

It is defined in the `java.util.function` package like this:

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

Here:

| Symbol | Meaning |
|---|---|
| `T` | The type of input data that will be **tested** |
| `boolean` | The output is always `true` or `false` |

> **Key Point:** Unlike `Function<T, R>` where both input and output types can vary, `Predicate<T>` **always returns a `boolean`**. You only specify the input type `T`.

---

### 15.2 Why Was `Predicate<T>` Introduced?

Before Java 8, if you wanted to check a condition (like "is this number even?" or "is this string empty?"), you would write an `if` statement or create a separate method every time.

The problem was:
- The same kind of checking logic was written **again and again** in different places.
- There was **no standard way** to pass a condition as a parameter to a method.

`Predicate<T>` was introduced so that you can:
- **Define a condition once** using a lambda expression.
- **Pass that condition** to methods that need to filter, validate, or test data.
- **Reuse and combine** conditions easily.

**Without `Predicate` (the old way):**

```java
public class Main {
    // A separate method just to check even
    static boolean isEven(int number) {
        return number % 2 == 0;
    }

    public static void main(String[] args) {
        System.out.println(isEven(10)); // true
    }
}
```

**With `Predicate` (the Java 8 way):**

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isEven = number -> number % 2 == 0;

        System.out.println(isEven.test(10)); // true
    }
}
```

The condition is defined **inline**, can be **stored in a variable**, and can be **passed to other methods** — all in one clean line.

---

### 15.3 When Should You Use `Predicate<T>`?

Use `Predicate<T>` whenever you need to **check a condition** and get a **yes/no answer**.

Common scenarios:

| Scenario | Input (`T`) | Question Being Asked |
|---|---|---|
| Is the number even? | `Integer` | `number % 2 == 0` |
| Is the string empty? | `String` | `str.isEmpty()` |
| Is the person an adult? | `Person` | `person.getAge() >= 18` |
| Is the price within budget? | `Double` | `price <= 1000.0` |
| Does the name start with "S"? | `String` | `name.startsWith("S")` |

The simple rule is:

> If you have **one input** and want to get a **true/false answer** — use `Predicate<T>`.

---

### 15.5 How the `test()` Method Works

`test()` is the **single abstract method** of the `Predicate<T>` interface.

**Signature:**

```java
boolean test(T t);
```

**How it works — step by step:**

1. You create a `Predicate` using a lambda expression — this defines the condition.
2. You call `test()` and pass an input value of type `T`.
3. The lambda evaluates the condition on that input.
4. It returns `true` if the condition is satisfied, `false` otherwise.

**Example:**

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        // Step 1: Define the condition — is the string empty?
        Predicate<String> isEmpty = str -> str.isEmpty();

        // Step 2: Call test() with an input
        System.out.println(isEmpty.test(""));      // true
        System.out.println(isEmpty.test("Hello")); // false
    }
}
```

**Output:**
```
true
false
```

**Flow of data:**

```
Input (T = String):    ""
Condition (test):      str -> str.isEmpty()
Output (boolean):      true

Input (T = String):    "Hello"
Condition (test):      str -> str.isEmpty()
Output (boolean):      false
```

---

### 15.6 Default and Static Methods of `Predicate<T>`

`Predicate<T>` is more powerful than it first appears. Beyond `test()`, it provides several **default and static methods** that let you **combine and modify** predicates.

| Method | Type | What It Does |
|---|---|---|
| `test(T t)` | Abstract | Evaluates the condition and returns `true` or `false` |
| `and(Predicate other)` | Default | Combines two predicates with **logical AND** (`&&`) |
| `or(Predicate other)` | Default | Combines two predicates with **logical OR** (`\|\|`) |
| `negate()` | Default | Returns the **opposite** of the current predicate |
| `isEqual(Object target)` | Static | Creates a predicate that checks **equality** with the given object |

---

#### `and()` — Both Conditions Must Be True

The `and()` method combines two predicates. The result is `true` **only if both** conditions are satisfied.

Think of it as: *"Is the number greater than 10 **AND** less than 50?"*

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> greaterThan10 = n -> n > 10;
        Predicate<Integer> lessThan50   = n -> n < 50;

        // Combine with AND
        Predicate<Integer> between10And50 = greaterThan10.and(lessThan50);

        System.out.println(between10And50.test(25));  // true  (25 > 10 AND 25 < 50)
        System.out.println(between10And50.test(5));   // false (5 is NOT > 10)
        System.out.println(between10And50.test(60));  // false (60 is NOT < 50)
    }
}
```

**Output:**
```
true
false
false
```

---

#### `or()` — At Least One Condition Must Be True

The `or()` method combines two predicates. The result is `true` **if at least one** condition is satisfied.

Think of it as: *"Is the number negative **OR** greater than 100?"*

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> lessThan0      = n -> n < 0;
        Predicate<Integer> greaterThan100 = n -> n > 100;

        // Combine with OR
        Predicate<Integer> outOfRange = lessThan0.or(greaterThan100);

        System.out.println(outOfRange.test(150));  // true  (150 > 100)
        System.out.println(outOfRange.test(-5));   // true  (-5 < 0)
        System.out.println(outOfRange.test(50));   // false (neither condition met)
    }
}
```

**Output:**
```
true
true
false
```

---

#### `negate()` — Reverse the Result

The `negate()` method returns a new predicate that gives the **opposite** result of the original.

Think of it as: *"If the original says true, negate says false — and vice versa."*

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isPositive = n -> n > 0;

        // Negate — now it checks for NOT positive
        Predicate<Integer> isNotPositive = isPositive.negate();

        System.out.println(isNotPositive.test(-10)); // true  (original would say false, negate flips it)
        System.out.println(isNotPositive.test(5));   // false (original would say true, negate flips it)
    }
}
```

**Output:**
```
true
false
```

> **Tip:** `negate()` is useful when you already have a predicate and need the opposite condition without writing a new lambda.

---

#### `isEqual()` — Check Equality with a Target Value

The `isEqual()` is a **static method** that creates a predicate to check whether the input is **equal** to a given target object. It uses `Objects.equals()` internally, making it **null-safe**.

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<String> isJava = Predicate.isEqual("Java");

        System.out.println(isJava.test("Java"));   // true
        System.out.println(isJava.test("Python")); // false
        System.out.println(isJava.test(null));      // false (null-safe)
    }
}
```

**Output:**
```
true
false
false
```

> **Tip:** `isEqual()` is useful when you need a quick equality check without writing `str -> str.equals("Java")` manually. It also handles `null` safely, avoiding `NullPointerException`.

---

### 15.7 Summary Table

| Aspect | Detail |
|---|---|
| **Package** | `java.util.function` |
| **Interface name** | `Predicate<T>` |
| **Abstract method** | `boolean test(T t)` |
| **Input** | One input of type `T` |
| **Output** | Always `boolean` (`true` or `false`) |
| **Purpose** | Test / validate / check a condition |
| **Default methods** | `and()`, `or()`, `negate()` |
| **Static method** | `isEqual()` |
| **Analogy** | A security guard — checks a rule and says yes or no |

---

### 15.8 `Predicate<T>` — Code Examples

Below are **5 examples**, arranged from basic to practical, to help you master `Predicate<T>`.

---

#### Example 1: Check if a Number is Even

The simplest use of `Predicate` — test a single condition on a number.

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isEven = num -> num % 2 == 0;

        System.out.println(isEven.test(20)); // true
        System.out.println(isEven.test(15)); // false
    }
}
```

**Output:**
```
true
false
```

**Explanation:**
- `T` = `Integer` (the number being tested)
- The lambda `num -> num % 2 == 0` checks if the number is divisible by 2.
- `test(20)` returns `true` because 20 is even. `test(15)` returns `false` because 15 is odd.

---

#### Example 2: Check if a String Starts with a Specific Letter

Using `Predicate` with `String` operations.

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<String> startsWithS = name -> name.startsWith("S");

        System.out.println(startsWithS.test("Sanket")); // true
        System.out.println(startsWithS.test("Rahul"));  // false
        System.out.println(startsWithS.test("Sneha"));  // true
    }
}
```

**Output:**
```
true
false
true
```

**Explanation:**
- The lambda checks if the input string starts with the letter "S".
- This is reusable — the same predicate can test any number of strings.

---

#### Example 3: Combine Conditions Using `and()` and `or()`

Demonstrating how to chain multiple predicates together.

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isEven        = n -> n % 2 == 0;
        Predicate<Integer> isGreaterThan10 = n -> n > 10;

        // AND — both conditions must be true
        Predicate<Integer> isEvenAndGreaterThan10 = isEven.and(isGreaterThan10);
        System.out.println("12 -> " + isEvenAndGreaterThan10.test(12)); // true
        System.out.println("8  -> " + isEvenAndGreaterThan10.test(8));  // false (not > 10)

        // OR — at least one condition must be true
        Predicate<Integer> isEvenOrGreaterThan10 = isEven.or(isGreaterThan10);
        System.out.println("8  -> " + isEvenOrGreaterThan10.test(8));   // true (is even)
        System.out.println("15 -> " + isEvenOrGreaterThan10.test(15));  // true (is > 10)
        System.out.println("7  -> " + isEvenOrGreaterThan10.test(7));   // false (neither)
    }
}
```

**Output:**
```
12 -> true
8  -> false
8  -> true
15 -> true
7  -> false
```

**Explanation:**
- `and()` returns `true` only when **both** predicates pass. 12 is even AND greater than 10, so it passes. 8 is even but NOT greater than 10, so it fails.
- `or()` returns `true` when **at least one** predicate passes. 7 fails both, so it returns `false`.

---

#### Example 4: Filter a List Using `Predicate`

A practical example — using `Predicate` to filter elements from a collection.

```java
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Sanket", "Rahul", "Sneha", "Amit", "Swati");

        Predicate<String> startsWithS = name -> name.startsWith("S");

        List<String> filtered = filterList(names, startsWithS);

        System.out.println("Names starting with S: " + filtered);
    }

    // A reusable method that accepts any Predicate
    static List<String> filterList(List<String> list, Predicate<String> condition) {
        List<String> result = new ArrayList<>();
        for (String item : list) {
            if (condition.test(item)) {
                result.add(item);
            }
        }
        return result;
    }
}
```

**Output:**
```
Names starting with S: [Sanket, Sneha, Swati]
```

**Explanation:**
- The `filterList()` method accepts **any** `Predicate<String>`. It does not know what condition will be checked — it just calls `test()` on each element.
- This makes the method **reusable**. You could pass a different predicate (e.g., `name -> name.length() > 4`) without changing the method itself.
- This is the same pattern used internally by Java's Stream API `filter()` method.

---

#### Example 5: Filter Users by Role Using `Predicate`

A real-world example — filtering a list of custom objects.

```java
import java.util.function.Predicate;
import java.util.ArrayList;
import java.util.List;

class User {
    private String name;
    private String role;

    public User(String name, String role) {
        this.name = name;
        this.role = role;
    }

    public String getName() { return name; }
    public String getRole() { return role; }

    @Override
    public String toString() {
        return "User{name='" + name + "', role='" + role + "'}";
    }
}

public class Main {
    public static void main(String[] args) {
        List<User> users = new ArrayList<>();
        users.add(new User("Sanket", "admin"));
        users.add(new User("Rahul", "member"));
        users.add(new User("Neha",  "admin"));
        users.add(new User("Amit",  "member"));

        // Predicate to check if user is an admin
        Predicate<User> isAdmin = user -> user.getRole().equals("admin");

        // Filter admins
        List<User> admins = filterUsers(users, isAdmin);
        System.out.println("Admins: " + admins);

        // Filter non-admins using negate()
        List<User> nonAdmins = filterUsers(users, isAdmin.negate());
        System.out.println("Non-Admins: " + nonAdmins);
    }

    static List<User> filterUsers(List<User> users, Predicate<User> condition) {
        List<User> result = new ArrayList<>();
        for (User user : users) {
            if (condition.test(user)) {
                result.add(user);
            }
        }
        return result;
    }
}
```

**Output:**
```
Admins: [User{name='Sanket', role='admin'}, User{name='Neha', role='admin'}]
Non-Admins: [User{name='Rahul', role='member'}, User{name='Amit', role='member'}]
```

**Explanation:**
- `Predicate<User>` takes a `User` object and checks whether their role is `"admin"`.
- The same `filterUsers()` method is reused for both filtering admins **and** non-admins.
- For non-admins, we used `isAdmin.negate()` instead of writing a new lambda — this shows the real power of the `negate()` method.
- This pattern is extremely common in real-world Java applications for filtering data from databases, APIs, and collections.

---

## 16. Built-in Functional Interfaces — `Consumer<T>` 📥

### 16.1 What is `Consumer<T>`?

`Consumer<T>` is a built-in Functional Interface that:

- **Takes one input** of type `T`
- **Performs an action** on it
- **Returns nothing** (`void`)

In simple words:

> `Consumer<T>` is like a **black hole** — it takes something in, does something with it, but gives nothing back.

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

| Symbol | Meaning |
|---|---|
| `T` | The type of input the consumer accepts |
| `void` | It never returns a value |

---

### 16.2 When to Use `Consumer<T>`?

Use it whenever you want to **do something** with a value but **don't need a result back**.

| Scenario | Input (`T`) | Action |
|---|---|---|
| Print a value | `String` | `System.out.println(...)` |
| Log a message | `String` | Write to log file |
| Save to database | `Employee` | Insert into DB |
| Send a notification | `User` | Send email/SMS |

> **Simple rule:** If you want to **use** the data but not **return** anything — use `Consumer<T>`.

---

### 16.3 How `accept()` Works

`accept()` is the single abstract method. You pass the input, the lambda runs, and nothing is returned.

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> printName = name -> System.out.println("Hello, " + name);

        printName.accept("Sanket"); // Hello, Sanket
        printName.accept("Rahul");  // Hello, Rahul
    }
}
```

**Flow:**
```
Input (T = String):  "Sanket"
Action (accept):     name -> System.out.println("Hello, " + name)
Output:              nothing (void)
```

---

### 16.4 Default Method — `andThen()`

`Consumer` has one default method: `andThen()`. It lets you **chain** two consumers — the first runs, then the second runs on the **same input**.

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> toUpperCase = s -> System.out.println(s.toUpperCase());
        Consumer<String> toLowerCase = s -> System.out.println(s.toLowerCase());

        // Chain: first print uppercase, then print lowercase
        Consumer<String> both = toUpperCase.andThen(toLowerCase);

        both.accept("Sanket");
    }
}
```

**Output:**
```
SANKET
sanket
```

> Both consumers receive the **same original input** `"Sanket"`.

---

### 16.5 Summary Table

| Aspect | Detail |
|---|---|
| **Package** | `java.util.function` |
| **Interface** | `Consumer<T>` |
| **Abstract method** | `void accept(T t)` |
| **Input** | One input of type `T` |
| **Output** | None (`void`) |
| **Default method** | `andThen()` |
| **Purpose** | Perform an action on input without returning anything |

---

### 16.6 `Consumer<T>` — Code Examples

---

#### Example 1: Print a String

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> print = msg -> System.out.println(msg);

        print.accept("Welcome to Java 8");
    }
}
```

**Output:** `Welcome to Java 8`

The simplest use — takes a string, prints it, returns nothing.

---

#### Example 2: Print Each Element of a List

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Sanket", "Rahul", "Amit");

        Consumer<String> printName = name -> System.out.println("Name: " + name);

        names.forEach(printName);
    }
}
```

**Output:**
```
Name: Sanket
Name: Rahul
Name: Amit
```

`forEach()` internally uses `Consumer` — this is why they work together naturally.

---

#### Example 3: Modify and Print a Number

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<Integer> printSquare = n -> System.out.println("Square: " + (n * n));

        printSquare.accept(5);  // Square: 25
        printSquare.accept(9);  // Square: 81
    }
}
```

**Output:**
```
Square: 25
Square: 81
```

It computes and prints the square, but **returns nothing**.

---

#### Example 4: Print Employee Details (Using Object)

```java
import java.util.function.Consumer;

class Employee {
    String name;
    int salary;

    Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee("Sanket", 50000);

        Consumer<Employee> printEmployee = e ->
            System.out.println(e.name + " earns " + e.salary);

        printEmployee.accept(emp);
    }
}
```

**Output:** `Sanket earns 50000`

`Consumer<Employee>` reads: *"Give me an Employee, I will do something with it but return nothing."*

---

#### Example 5: Apply Salary Bonus to a List of Employees (Using Object)

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

class Employee2 {
    String name;
    double salary;

    Employee2(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Employee2> employees = Arrays.asList(
            new Employee2("Sanket", 50000),
            new Employee2("Neha", 45000),
            new Employee2("Amit", 60000)
        );

        // Consumer that gives a 10% bonus
        Consumer<Employee2> applyBonus = e -> e.salary = e.salary * 1.10;

        // Consumer that prints details
        Consumer<Employee2> printDetails = e ->
            System.out.println(e.name + " -> " + e.salary);

        // Chain: first apply bonus, then print
        Consumer<Employee2> bonusAndPrint = applyBonus.andThen(printDetails);

        employees.forEach(bonusAndPrint);
    }
}
```

**Output:**
```
Sanket -> 55000.0
Neha -> 49500.0
Amit -> 66000.0
```

This shows `andThen()` in a real-world scenario — first modify the data, then display it, all using consumers.

---

## 17. Built-in Functional Interfaces — `Supplier<T>` 📤

### 17.1 What is `Supplier<T>`?

`Supplier<T>` is a built-in Functional Interface that:

- **Takes no input**
- **Returns one output** of type `T`

In simple words:

> `Supplier<T>` is like a **factory** — you ask for something, and it gives it to you without needing anything from you.

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

| Symbol | Meaning |
|---|---|
| `T` | The type of value the supplier **produces** |
| No input | The `get()` method takes **zero parameters** |

> **Key difference from other interfaces:** `Predicate`, `Function`, and `Consumer` all take input. `Supplier` takes **nothing** — it only **gives**.

---

### 17.2 When to Use `Supplier<T>`?

Use it whenever you need to **generate or provide a value** without any input.

| Scenario | Output (`T`) | What it Supplies |
|---|---|---|
| Get current date/time | `LocalDate` | Today's date |
| Generate a random number | `Integer` | A random value |
| Create a default object | `Employee` | A new Employee with default values |
| Return a constant message | `String` | A welcome message |

> **Simple rule:** If you want to **produce** a value without needing any input — use `Supplier<T>`.

---

### 17.3 How `get()` Works

`get()` is the single abstract method. You call it with no arguments, and it returns a value.

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> greeting = () -> "Welcome to Java 8";

        System.out.println(greeting.get()); // Welcome to Java 8
    }
}
```

**Flow:**
```
Input:              nothing
Action (get):       () -> "Welcome to Java 8"
Output (T = String): "Welcome to Java 8"
```

> Notice `()` — empty parentheses because `Supplier` takes no input.

---

### 17.4 Summary Table

| Aspect | Detail |
|---|---|
| **Package** | `java.util.function` |
| **Interface** | `Supplier<T>` |
| **Abstract method** | `T get()` |
| **Input** | None |
| **Output** | One output of type `T` |
| **Default methods** | None |
| **Purpose** | Produce / supply a value without needing input |

---

### 17.5 `Supplier<T>` — Code Examples

---

#### Example 1: Supply a Simple String

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<String> message = () -> "Hello from Supplier!";

        System.out.println(message.get());
    }
}
```

**Output:** `Hello from Supplier!`

The simplest use — no input, just returns a string.

---

#### Example 2: Generate a Random Number

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<Integer> randomNumber = () -> (int) (Math.random() * 100);

        System.out.println("Random: " + randomNumber.get());
        System.out.println("Random: " + randomNumber.get());
    }
}
```

**Output (varies):**
```
Random: 42
Random: 87
```

Each call to `get()` produces a **new random number**. The supplier generates a fresh value every time.

---

#### Example 3: Get Current Date

```java
import java.util.function.Supplier;
import java.time.LocalDate;

public class Main {
    public static void main(String[] args) {
        Supplier<LocalDate> today = () -> LocalDate.now();

        System.out.println("Today: " + today.get());
    }
}
```

**Output:** `Today: 2026-07-11`

Useful when you need to supply the current date dynamically.

---

#### Example 4: Supply a Default Employee Object (Using Object)

```java
import java.util.function.Supplier;

class Employee {
    String name;
    int salary;

    Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return name + " - " + salary;
    }
}

public class Main {
    public static void main(String[] args) {
        Supplier<Employee> defaultEmployee = () -> new Employee("Unknown", 0);

        Employee emp = defaultEmployee.get();

        System.out.println(emp);
    }
}
```

**Output:** `Unknown - 0`

`Supplier<Employee>` acts as a **factory** — every time you call `get()`, it creates a new default `Employee` object.

---

#### Example 5: Factory Method for Creating Multiple Students (Using Object)

```java
import java.util.function.Supplier;
import java.util.List;
import java.util.ArrayList;

class Student {
    private static int counter = 0;
    String name;
    int rollNo;

    Student() {
        this.rollNo = ++counter;
        this.name = "Student-" + rollNo;
    }

    @Override
    public String toString() {
        return name + " (Roll: " + rollNo + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        Supplier<Student> studentFactory = () -> new Student();

        // Create 3 students using the supplier
        List<Student> students = new ArrayList<>();
        students.add(studentFactory.get());
        students.add(studentFactory.get());
        students.add(studentFactory.get());

        students.forEach(s -> System.out.println(s));
    }
}
```

**Output:**
```
Student-1 (Roll: 1)
Student-2 (Roll: 2)
Student-3 (Roll: 3)
```

Each `get()` call creates a **new Student** with an auto-incremented roll number. This is a common **factory pattern** using `Supplier`.

---

### Quick Comparison: All Four Built-in Functional Interfaces

| Interface | Input | Output | Method | One-line Description |
|---|---|---|---|---|
| `Predicate<T>` | ✅ Yes (`T`) | `boolean` | `test()` | Checks a condition |
| `Function<T, R>` | ✅ Yes (`T`) | ✅ Yes (`R`) | `apply()` | Transforms input to output |
| `Consumer<T>` | ✅ Yes (`T`) | ❌ None | `accept()` | Uses input, returns nothing |
| `Supplier<T>` | ❌ None | ✅ Yes (`T`) | `get()` | Produces a value, needs nothing |

---

