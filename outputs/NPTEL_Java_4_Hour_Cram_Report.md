# NPTEL Programming in Java: Four-Hour Exam Cram Report

## How to use this report

You said you know Java basics but are not confident at calculating outputs. That is exactly the skill this report prioritizes. Do not try to reread all of Java in four hours. Spend the first hour on the rules and traps, the second hour on the high-probability topics, and the final two hours solving the practice questions without looking at the answers.

The local folder contains six year-wise merged PDFs: 2020, 2022, 2023, 2024, 2025, and 2026. Its README describes them as containing 12-week assignments, previous-year practice, repeated questions, and exam-preparation material. The PDFs are not all text-readable: the 2022 and 2023 files behave like scanned/image-heavy documents, while the other files yield partial or noisy text extraction. Therefore, exact word-for-word duplicate detection is not reliable for every year. The patterns below are stated as recurring **question families** rather than as a claim that every sentence is duplicated.

The official IIT Kharagpur course page confirms the 12-week sequence: Java introduction and tools; encapsulation and static scope; inheritance and information hiding; packages and interfaces; exception handling; multithreading and I/O; applets; AWT and Swing; networking and JDBC; and case studies [1]. The current SWAYAM/NPTEL preview identifies the course as “Programming In Java” by Prof. Debasis Samanta of IIT Kharagpur and lists programming and data-structures familiarity as prerequisites [2].

## The most important conclusion

If time is short, prioritize these in this order:

1. **Output tracing:** pre/post-increment, short-circuit operators, loops, arrays, strings, method calls, constructors, static fields, and reference aliasing.
2. **OOP dispatch:** overloading versus overriding, `this` versus `super`, constructor order, static method hiding, dynamic method dispatch, abstract classes, and interfaces.
3. **Exceptions:** checked versus unchecked exceptions, `try`/`catch`/`finally`, `throw` versus `throws`, catch ordering, and return-versus-finally behavior.
4. **Threads:** `start()` versus `run()`, `sleep`, `join`, synchronization, race conditions, and legal thread states.
5. **Collections and generics:** `ArrayList`, `HashSet`, `HashMap`, equality, hashing, iterators, and type safety.
6. **Strings and arrays:** immutability, `==` versus `.equals()`, string pool, `StringBuilder`, array covariance, and default values.
7. **I/O, networking, GUI, and JDBC:** know the purpose and order of the important classes and methods even if you cannot memorize every API detail.

A reasonable prediction is that the exam will continue to mix direct concept MCQs with small code fragments whose answer depends on one or two Java rules. It is much more valuable to trace ten short programs carefully than to memorize a long list of definitions.

# Four-hour plan

## 0:00–0:25 — Build the output-tracing method

For every code question, write a tiny state table. Track the variable values after each statement. Mark whether a variable is a primitive value or a reference. For a method call, write the actual argument values and the selected method signature. For inheritance, write the constructor order and the runtime class of each reference.

Never evaluate a complicated expression mentally when increments are involved. Split it into steps. For example, in `x++ + ++x`, the first operand uses the old value and then increments; the second operand increments first and uses the new value. Parentheses and Java's left-to-right operand evaluation matter.

## 0:25–1:10 — OOP and language traps

Study the sections on `static`, `final`, constructors, overloading, overriding, `this`, `super`, access modifiers, abstract classes, interfaces, packages, and strings. Solve Practice Questions 1–10 below.

## 1:10–1:45 — Exceptions, threads, and collections

Study the exception rules, thread lifecycle, synchronization, and collection behavior. Solve Practice Questions 11–18.

## 1:45–2:15 — I/O, networking, GUI, JDBC

Learn the architecture and common class/method purposes in the quick-reference sections. These questions are often recognition or ordering questions rather than long calculations. Solve Practice Questions 19–24.

## 2:15–3:30 — Timed practice

Attempt all 24 questions without looking at the solutions. Give yourself about three minutes per output-tracing question and one minute per direct-concept question. Put a star beside every question where you guessed.

## 3:30–4:00 — Error review

Review only your wrong and guessed questions. Read the explanation, then create one sentence in your own words. Examples: “`start()` creates/schedules a new thread; `run()` is just an ordinary method call.” “Overloading is selected mainly from the compile-time types; overriding is selected from the runtime object.”

# Recurring question patterns

## 1. Output tracing is the central pattern

The usual structure is a short program followed by four possible outputs. The code may be Java, or a language-fragment question used to test general programming reasoning. The most valuable Java variants are:

- pre-increment and post-increment in expressions;
- `&&` and `||` short-circuiting;
- nested loops and `break`/`continue`;
- array references and aliasing;
- string concatenation and evaluation from left to right;
- overloaded methods;
- constructor chaining;
- `static` fields shared across objects;
- exceptions interrupting normal control flow;
- thread output where only a partial ordering is guaranteed.

The safe method is always the same: evaluate one operand at a time, record state changes, and do not assume that two thread outputs have a fixed order unless synchronization creates that order.

## 2. OOP questions recur in connected chains

NPTEL-style questions often combine several OOP rules in one small program. A typical chain is:

1. a base-class constructor runs;
2. it calls or initializes a field;
3. a derived-class constructor runs;
4. a reference typed as the base class points to a derived object;
5. an overloaded or overridden method is invoked;
6. the answer depends on compile-time selection versus runtime dispatch.

Learn the distinction precisely:

- **Overloading:** same method name, different parameter list. Selection is resolved at compile time.
- **Overriding:** subclass supplies a compatible implementation of an inherited instance method. Selection is dynamic at runtime.
- **Static methods:** hidden, not overridden. Selection uses the reference/class type.
- **Private methods:** not inherited in the normal overriding sense.
- **Fields:** hidden, not dynamically dispatched. Field access uses the reference type.

## 3. Constructors and initialization order are high-yield

For `new Child()`, Java initializes the object in this broad order: superclass static initialization when the class is initialized; subclass static initialization when applicable; superclass instance field initializers and instance initializer blocks; superclass constructor; subclass instance field initializers and instance initializer blocks; subclass constructor body. Exact class-initialization timing depends on whether the class is being initialized for the first active use, but the exam usually tests the superclass-before-subclass rule.

A constructor cannot be inherited. If a subclass constructor does not explicitly call `super(...)`, Java inserts a call to the accessible no-argument superclass constructor. If no such constructor exists, compilation fails.

## 4. Exceptions are tested through control flow

Know these rules exactly:

- A checked exception must be caught or declared with `throws`.
- `RuntimeException` and its common subclasses are unchecked.
- `throw` actually throws one exception object.
- `throws` declares that a method may pass exceptions to its caller.
- Catch blocks are tested top to bottom. A broader catch before a narrower catch makes the narrower catch unreachable.
- A `finally` block normally runs whether the `try` completes normally or abruptly, except for process termination such as `System.exit(...)`.
- If both `try` and `finally` return, the `finally` return wins. This is legal but bad style.
- An exception thrown in a `try` skips the remaining statements in that `try` and searches for a matching catch.

## 5. Threads produce “what is guaranteed?” questions

The most common traps are:

- `start()` requests a new thread of execution and eventually invokes `run()`.
- Calling `run()` directly does not create a new thread; it runs on the current thread.
- `sleep(...)` pauses the current thread and does not release a monitor lock held by that thread.
- `join()` makes the calling thread wait for another thread to finish.
- `synchronized` protects a critical section by using an object's monitor lock.
- A race condition occurs when the result depends on unsynchronized timing of shared mutable state.
- Thread scheduling is not a valid reason to claim a single exact output order unless the code enforces it.

A question may ask which outputs are possible rather than which one output always occurs. Separate **mandatory ordering** from **possible ordering**.

## 6. Strings and arrays are reliable marks if you know the traps

`String` objects are immutable. Methods such as `concat`, `replace`, and `toUpperCase` return a new string; they do not mutate the original string. `==` compares references, while `.equals()` compares content for `String`.

String literals may be interned in the string pool, so two identical literals can have the same reference. A string created with `new String("x")` is a distinct object even if its content is equal to the literal.

For arrays, remember:

- arrays are objects;
- indexing starts at zero;
- `length` is a field, not a method;
- a newly allocated array contains default values;
- primitive array elements store values, while an object array initially stores `null`;
- two variables can refer to the same array, so changing through one changes what the other sees;
- `array.clone()` makes a shallow copy for an object array.

## 7. Collections and generics are common conceptual targets

Know the rough behavior:

- `List` preserves order and permits duplicates.
- `Set` models unique elements.
- `Map` stores key-value associations and does not implement `Collection`.
- `ArrayList` gives fast indexed access in typical use; insertion in the middle can shift elements.
- `LinkedList` supports efficient insertion/removal when the position/node is known, but indexed access is not its strength.
- `HashSet` and `HashMap` depend on correct `equals`/`hashCode` behavior.
- A `TreeSet`/`TreeMap` uses ordering and normally rejects elements that cannot be compared under the chosen ordering.
- Generics provide compile-time type checking and normally use type erasure at runtime.
- `List<Integer>` stores `Integer` objects; autoboxing converts an `int` to `Integer` when needed.

## 8. Packages and access control are easy direct questions

Visibility from most restrictive to broadest is: `private`, default/package-private, `protected`, `public`, although `protected` has special cross-package rules. A top-level class can be public or package-private, not private or protected.

A package helps organize classes and control access. An `import` statement makes names easier to use; it does not copy classes or grant extra permissions. `java.lang` is imported automatically; other packages are not.

## 9. Interfaces, abstract classes, and modern Java features

An abstract class may contain state, constructors, concrete methods, and abstract methods. It cannot be instantiated directly. An interface defines a contract; a class can implement multiple interfaces. Modern interfaces may contain `default`, `static`, and private helper methods, but interface fields are implicitly `public static final`.

If a class implements an interface method, the implementation must be compatible and cannot reduce visibility. If two interfaces provide conflicting default methods, the implementing class must resolve the conflict, unless a more specific inherited default already resolves it.

## 10. I/O, networking, GUI, and JDBC are breadth topics

The official syllabus places these in the later weeks, so expect recognition questions and small API-order questions:

- **Byte streams:** `InputStream`/`OutputStream` and subclasses handle raw bytes.
- **Character streams:** `Reader`/`Writer` handle character data.
- **Buffered streams:** add buffering and often improve efficiency.
- **Serialization:** `ObjectOutputStream` writes objects; the class normally implements `Serializable`.
- **Networking:** `Socket` is commonly the client-side endpoint; `ServerSocket` listens for incoming connections on the server side.
- **AWT/Swing:** AWT uses heavyweight native components; Swing provides lightweight components and follows a richer component model. Event handling uses listeners and event objects.
- **JDBC:** the broad flow is obtain a `Connection`, create a `Statement`/`PreparedStatement`, execute SQL, process a `ResultSet` for queries, and close resources. `PreparedStatement` is preferred for parameters and helps avoid SQL injection when used correctly.

# Predicted question families

These are predictions, not guarantees. They are based on the official course sequence and the recurring families visible in the local material.

1. Trace an expression mixing `++x`, `x++`, and method calls.
2. Determine whether a program compiles when a constructor calls `super(...)` or `this(...)` incorrectly.
3. Find constructor and initializer-block output order in an inheritance hierarchy.
4. Distinguish overloaded, overridden, hidden, and private methods.
5. Determine the value of a static field after several objects are created.
6. Compare `==`, `.equals()`, and string-pool behavior.
7. Identify whether a checked exception must be caught or declared.
8. Choose the valid order of catch blocks.
9. Determine the output when `finally` contains a return.
10. Decide whether calling `run()` creates a new thread.
11. Identify which shared counter updates are unsafe without synchronization.
12. Select the collection that preserves duplicates/order or enforces uniqueness.
13. Predict behavior when `equals` is overridden without a compatible `hashCode`.
14. Determine whether a generic assignment compiles, especially involving `List<Integer>` and `List<Number>`.
15. Identify the correct byte/character stream class.
16. Match `Socket`, `ServerSocket`, `Connection`, `PreparedStatement`, and `ResultSet` to their roles.
17. Identify a Swing event listener or the correct event-dispatch concept.
18. Determine default values or aliasing behavior for arrays.

# Practice questions

Try these before reading the solutions. Unless a question says “possible output,” assume the code is run as a standalone valid Java fragment with the obvious class wrapper.

### 1. Increment tracing

```java
int x = 3;
int y = x++ + ++x;
System.out.println(x + " " + y);
```

What is printed?

### 2. Short-circuiting

```java
int x = 0;
boolean b = (x++ > 0) && (++x > 0);
System.out.println(x + " " + b);
```

What is printed?

### 3. String references

```java
String a = "java";
String b = "ja" + "va";
String c = new String("java");
System.out.println((a == b) + " " + (a == c) + " " + a.equals(c));
```

What is printed?

### 4. String immutability

```java
String s = "abc";
s.concat("def");
System.out.println(s);
```

What is printed, and why?

### 5. Static field

```java
class Counter {
    static int n = 0;
    Counter() { n++; }
}
Counter a = new Counter();
Counter b = new Counter();
System.out.println(Counter.n);
```

What is printed?

### 6. Overloading versus overriding

```java
class Parent {
    void show(Object x) { System.out.print("P-O"); }
    void show(String x) { System.out.print("P-S"); }
}
class Child extends Parent {
    @Override void show(Object x) { System.out.print("C-O"); }
}
Parent p = new Child();
p.show("x");
Object o = "x";
p.show(o);
```

What is printed?

### 7. Fields are not dynamically dispatched

```java
class A { int x = 1; }
class B extends A { int x = 2; }
A ref = new B();
System.out.println(ref.x);
```

What is printed?

### 8. Constructor order

```java
class A {
    A() { System.out.print("A"); }
}
class B extends A {
    B() { System.out.print("B"); }
}
new B();
```

What is printed?

### 9. Exception and finally

```java
static int test() {
    try { return 1; }
    finally { return 2; }
}
System.out.println(test());
```

What is printed? Is this good style?

### 10. Catch ordering

Which version compiles?

A.
```java
try { throw new Exception(); }
catch (Exception e) { }
catch (RuntimeException e) { }
```

B.
```java
try { throw new RuntimeException(); }
catch (RuntimeException e) { }
catch (Exception e) { }
```

### 11. `throw` and `throws`

In one sentence each, explain the difference between `throw` and `throws`.

### 12. Checked exception

```java
void read() {
    throw new java.io.IOException();
}
```

Will this compile? If not, give two standard repairs.

### 13. `start()` versus `run()`

```java
class T extends Thread {
    public void run() { System.out.print("R"); }
}
T t = new T();
t.run();
System.out.print("M");
```

What is guaranteed about the output and why?

### 14. `join()` concept

Thread A calls `b.join()`. What does A wait for?

### 15. Array aliasing

```java
int[] a = {1, 2};
int[] b = a;
b[0] = 9;
System.out.println(a[0]);
```

What is printed?

### 16. Collection choice

Choose the best general interface for each requirement: (i) preserve insertion order and allow duplicates; (ii) enforce uniqueness; (iii) map keys to values.

### 17. Hashing rule

If two objects are equal according to `equals`, what must be true about their `hashCode()` values?

### 18. Generics

Does this compile?

```java
java.util.List<Integer> a = new java.util.ArrayList<>();
java.util.List<Number> b = a;
```

Why or why not?

### 19. Stream type

Which family is appropriate for raw binary data: `Reader`/`Writer` or `InputStream`/`OutputStream`? Which family is appropriate for character data?

### 20. JDBC order

Put these in a sensible order for a parameterized query: execute, obtain connection, create prepared statement, bind parameters, process result set, close resources.

### 21. Networking

What is the usual role of `ServerSocket`, and what is the usual role of `Socket`?

### 22. Interface field

```java
interface I { int X = 10; }
```

What modifiers are implicitly associated with `X`?

### 23. Default array values

What are the initial values of `int[]`, `boolean[]`, and `String[]` elements after allocation with `new`?

### 24. Thread safety

Why can `count++` be unsafe when several threads share `count`, even though it looks like one statement?

# Answers and worked solutions

## 1. `4 6`

`x++` contributes 3 and changes `x` to 4. Then `++x` changes it to 5 and contributes 5. Therefore `y = 3 + 5 = 8`—wait: the final value is actually 5, so the correct output is **`5 8`**. This is a good example of why the state must be written after each operand.

## 2. `1 false`

`x++ > 0` tests 0, then increments `x` to 1. The comparison is false, so `&&` short-circuits and `++x` is not evaluated. The result is false and `x` remains 1.

## 3. `true false true`

`"ja" + "va"` is a compile-time constant expression, so it is the same pooled literal as `"java"`. The `new String` expression creates a different object. `.equals()` compares content and returns true.

## 4. `abc`

`String` is immutable. `concat` returns a new string, but the returned value is ignored. The variable `s` still refers to `"abc"`.

## 5. `2`

`n` belongs to the class, not separately to each object. Each constructor increments the same static field.

## 6. `P-SC-O`

For `p.show("x")`, overload resolution sees the compile-time type `Parent` and chooses the more specific `show(String)`. That method is inherited and prints `P-S`. For `p.show(o)`, the compile-time type of `o` is `Object`, so `show(Object)` is selected; that instance method is overridden, so the `Child` implementation prints `C-O`.

## 7. `1`

Fields are resolved using the reference type, not dynamic dispatch. The reference is declared as `A`, so `ref.x` accesses `A.x`.

## 8. `AB`

A subclass constructor begins by invoking the superclass constructor, explicitly or implicitly. The `A` body prints first, then the `B` body prints.

## 9. `2`; it is poor style

The `finally` return replaces the pending return from the `try`. Returning from `finally` can hide exceptions or earlier return values, so it should normally be avoided.

## 10. Version B only

In A, `Exception` catches `RuntimeException` already, so the later `RuntimeException` catch is unreachable. In B, the narrow `RuntimeException` catch comes first and the broader `Exception` catch comes later, so the code compiles.

## 11. `throw` versus `throws`

`throw` is a statement that actually throws a particular exception object. `throws` is part of a method declaration and lists exceptions that the method may pass to its caller.

## 12. It does not compile as written

`IOException` is checked. The method must either catch it:

```java
void read() {
    try { throw new java.io.IOException(); }
    catch (java.io.IOException e) { }
}
```

or declare it:

```java
void read() throws java.io.IOException {
    throw new java.io.IOException();
}
```

## 13. `RM` is guaranteed

Calling `run()` directly is an ordinary method call on the current thread. It prints `R`, then the next statement prints `M`. If the code used `t.start()`, a new thread would be scheduled and the relative order would not generally be fixed without synchronization.

## 14. A waits for B to terminate

`b.join()` causes the thread that calls it to wait until thread `b` has finished, subject to the overload and interruption behavior of the particular `join` call.

## 15. `9`

`b` is another reference to the same array object as `a`; it is not a copy. Updating through `b` changes the element seen through `a`.

## 16. `List`, `Set`, `Map`

Use `List` for ordered duplicates, `Set` for uniqueness, and `Map` for key-value associations. Concrete choices such as `ArrayList`, `LinkedHashSet`, or `HashMap` depend on additional ordering and performance requirements.

## 17. Equal objects must have equal hash codes

The contract requires that if `a.equals(b)` is true, then `a.hashCode() == b.hashCode()` must also be true. The reverse is not required: equal hash codes do not prove that objects are equal.

## 18. No

Java generics are invariant. `List<Integer>` is not a subtype of `List<Number>`, even though `Integer` is a subtype of `Number`. A wildcard such as `List<? extends Number>` can express a read-oriented relationship.

## 19. Binary data uses byte streams; character data uses character streams

`InputStream`/`OutputStream` and their subclasses are for bytes. `Reader`/`Writer` and their subclasses are for characters and text encodings.

## 20. Obtain connection → create prepared statement → bind parameters → execute → process result set → close resources

For an update, there may be no `ResultSet`. In modern Java, try-with-resources is the preferred way to ensure JDBC resources are closed.

## 21. `ServerSocket` listens; `Socket` communicates

A server commonly creates a `ServerSocket` bound to a port and accepts connections. The resulting `Socket` represents a connected endpoint used to exchange data. A client normally creates a `Socket` to connect to a server address and port.

## 22. `public static final`

Interface fields are implicitly public, static, and final. `I.X` is a constant and cannot be reassigned.

## 23. `0`, `false`, and `null`

New `int` elements are zero, new `boolean` elements are false, and new reference-type elements such as `String` are null.

## 24. `count++` is a read-modify-write sequence

Conceptually it reads the current value, adds one, and writes the result. Two threads can read the same old value and both write the same incremented value, losing one update. Synchronization, an atomic variable, or another concurrency design is needed when the increment must be thread-safe.

# Final one-page memory sheet

- `==` compares primitive values or references; `.equals()` is a method for logical equality.
- `String` is immutable; ignored transformation results do not change the original.
- Overloading: compile time. Overriding: runtime instance dispatch.
- Static methods and fields are hidden/resolved by type; instance methods can override.
- Fields are not dynamically dispatched.
- `this(...)` or `super(...)` must be the first constructor statement, and both cannot appear together.
- A subclass constructor implicitly calls accessible `super()` if no explicit constructor call appears.
- Checked exceptions must be caught or declared.
- Catch narrow exceptions before broad exceptions.
- `finally` usually runs; a return there overrides earlier returns.
- `start()` creates/schedules a new thread; `run()` alone does not.
- `sleep()` does not release a monitor; `join()` waits for termination.
- `List` allows duplicates; `Set` enforces uniqueness; `Map` maps keys to values.
- Equal objects must have equal hash codes.
- `InputStream`/`OutputStream` are bytes; `Reader`/`Writer` are characters.
- `ServerSocket` listens; `Socket` connects/communicates.
- JDBC: connection → statement → bind → execute → result → close.
- Interface fields are `public static final`.
- Arrays use zero-based indexing, `length` field, and default initialization.

# References

[1]: https://nptel.ac.in/courses/106105191 "NPTEL Programming in Java course details and weekly lecture sequence"

[2]: https://onlinecourses.nptel.ac.in/noc26_cs36/preview "NPTEL Programming In Java current course preview"

[3]: https://swayam.gov.in/nc_details/NPTEL "SWAYAM NPTEL national coordinator and certification information"

[4]: https://github.com/Kartik0033/NPTEL-Programming-In-Java-Previous-Year-Questions "Public repository containing the year-wise local question-paper collection"
