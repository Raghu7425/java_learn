# Java from the Ground Up
### A complete course built from the BEL C23 class notes and code repository

---

## How to use this document

This is a full Java course, built from your class notes and your instructor's code repository. It is designed for **active study**, not reading.

- **Work through it in order.** Each module assumes the ones before it. Prerequisites are taught before the concepts that depend on them, even when the classes covered them in a different order.
- **Do the "Check yourself" questions before moving on.** For "predict the output" questions, write your prediction down *first*, then run the code to check yourself. A wrong prediction is the most valuable thing that can happen to you, because it shows exactly where your mental model is broken.
- **Exercises have no solutions here, on purpose.** Most have progressive hints in collapsible blocks. Open one hint at a time, and only after a genuine attempt. For the four big practice problems, your instructor's solutions are in the repository's `solution/` folders. Compare *after* you finish.
- **Bring your answers back to me.** I'll review your reasoning, find exactly where it went wrong, and re-teach that piece. Section ★.9 explains how your weak areas get tracked.

### Source legend
Every important claim is tagged so you know where it came from:

| Tag | Meaning |
|---|---|
| 📝 | From your AI-generated class notes |
| 📁 | From the instructor's code repository (`BEL C23 - Java code repository`) |
| ➕ | Added by me, to fill a gap, correct an error, or complete the topic |

### Priority legend

| Tag | Meaning |
|---|---|
| 🔴 | **Essential**: fundamental, used constantly, frequently interviewed. Master it. |
| 🟠 | **Important**: should understand well; useful in real work and interviews. |
| 🟡 | **Good to know**: useful but lower priority. |
| ⚪ | **Optional / Advanced**: learn later unless needed for something else. |

### How to run the code in this course
```bash
javac HelloWorld.java     # compile → produces HelloWorld.class (bytecode)
java HelloWorld           # run the bytecode on the JVM

java HelloWorld.java      # Java 11+: compile in memory and run a single-file program
```
➕ Every code example and every output shown in this course was compiled and run on Java 21 while writing it. Every compiler error message quoted is the real message.


## Contents
- Part 0 — What was in your material, and what was wrong with it
- Part 1 — Learning Roadmap
- Module 1 — How Java Runs & How Java Stores Data
- Module 2 — Typecasting & Operators
- Module 3 — Control Flow & Loops
- Module 4 — Methods, Arrays & Input (capstone: console Learner Manager)
- Module 5 — Classes & Objects
- Module 6 — Inside the JVM
- Module 7 — Packages, Access Modifiers & Encapsulation (practice: Report Card)
- Module 8 — Inheritance
- Module 9 — Polymorphism (practice: Payroll)
- Module 10 — Exception Handling
- Module 11 — Abstraction, Interfaces, Enums, Collections & Relationships (practice: LMS design)
- Module 12 — Design Principles (practice: Food Court Billing)
- Module 13 — Singleton & Thread-Safety Basics
- ★ Final Review — concept map, must-master list, checklist, cumulative exercises, mock interview, weak areas

---

# Part 0 — What was in your material, and what was wrong with it

## 0.1 Sources

**Your 11 note documents.** Nine cover the Java course. Two do not:
- **Document 1** (frontend/backend, client–server, DNS, hosts file, TTL, VPNs) is backend and internet background. It explains *where Java fits*: Java is mostly used to write the **server** in the *client → server → database* picture. It's used as context in Module 1 and Module 12. It is not otherwise part of this Java course.
- **Document 2** (a Zomato healthy-food feature, root-cause analysis of a 20% order drop, product interview feedback) is a product-management session that appears to come from a different track. It contains no Java and is **out of scope** here.

**The instructor's repository.** This has 133 files across classes 1–7: demo code, a markdown notes file per topic, four practice problems with solutions, and a class diagram. It covers several topics that were **taught in class but missing from your AI notes**:
- data types and variable scope
- methods
- `break`/`continue`
- JDK/JRE/JVM
- JVM architecture (PC register, native method stack)
- `try-catch-finally` and the exception hierarchy
- `Error` vs `Exception`
- method-overload resolution rules
- JVM inspection tools

## 0.2 Errors, outdated claims, and ambiguities found

Each one is corrected in detail inside the relevant module. Don't memorize the left-hand column.

| # | Where | What the material says | What's actually true | Fixed in |
|---|---|---|---|---|
| 1 | Doc 4 | The Application class loader "tries first" and delegates upward if it fails | The reverse. Each loader asks its **parent first** and loads the class itself only if the parents can't. | 6.3 |
| 2 | Doc 3 | Third-party libraries are loaded by the Extension class loader | Libraries on your classpath are loaded by the **Application** loader. "Extension" was renamed **Platform** in Java 9. | 6.3 |
| 3 | Docs 3, 4, repo | Static variables live in the method area | That's the conceptual JVM-specification model. In HotSpot since Java 8, class metadata lives in **Metaspace** and static field *values* live on the **heap**. | 6.6 |
| 4 | Doc 8 | Interfaces have no implementation and no shared code | Outdated since **Java 8**: `default` and `static` methods have bodies. Java 9 added `private` methods. | 11.3 |
| 5 | Doc 8 | Two interfaces with the same method never conflict | True for abstract methods. If both provide a **default** implementation, the class **must** override and resolve the conflict. | 11.3 |
| 6 | Doc 11, repo | Double-checked locking singleton | As written, it's **broken**: the field must be `volatile`. | 13.8 |
| 7 | Repo | `boolean` is 1 bit | The size is not defined by the Java specification. In practice it's usually 1 byte. | 1.4 |
| 8 | Repo | "Everything is an object" in Java | False. The 8 primitive types are not objects. | 1.4 |
| 9 | Repo | End users only need the JRE | Outdated since Java 11. There's no standalone JRE download; applications ship their own runtime. | 1.1 |
| 10 | Repo | Widening has "no data loss" | Usually true, but `long → float`, `int → float` and `long → double` can lose **precision**. | 2.1 |
| 11 | Doc 5 | Packages are "just strings, not folders" | Half right. The JVM treats package names as plain names, but class files are found through matching directory paths. | 7.1 |
| 12 | Doc 5 | Throwing a custom exception from a `catch` "replaces" the original | It replaces it **only if you discard the cause**. Best practice is to pass the original as the *cause*. | 10.7 |
| 13 | Doc 5 | Unchecked = "problems the compiler cannot predict" | That's intuition, not the rule. The real rule is the class hierarchy: `RuntimeException`, `Error` and their subclasses are unchecked. | 10.2 |
| 14 | Repo | Copy constructor demo | The body is commented out, so it copies **nothing**. Even when uncommented, it makes a **shallow** copy (the `Car` is shared). | 5.3 |
| 15 | Repo | `Dog(int a, String b, int c, String d)` contains `this.breed = breed;` | A bug: there's no parameter called `breed`, so the field is assigned to itself and stays `null`. | 5.4 |
| 16 | Repo LMS | The Cohort "composes" its learners | Debatable. The learners are created outside and passed in, which is aggregation by the course's own identification rule. | 11.8 |
| 17 | Doc 6 vs repo | Report card: invalid score is "reset to 0" | The repo's specification says *do not update* on invalid input. The repo version is correct. | 7.5 |
| 18 | Doc 7 vs repo | Payroll classes named PartTimeEmployee (bonus) / HourlyEmployee | The repo uses FullTimeEmployee (bonus) / PartTimeEmployee (hourly). Same design, different names. | 9.7 |
| 19 | Repo | Interfaces = "100% abstraction", abstract classes = "0–100%" | Old phrasing that's no longer accurate since Java 8 default methods. Use the decision rules in 11.4 instead. | 11.4 |

## 0.3 Prerequisites the course assumed but never taught (all added ➕)
- `String` internals: immutability, the string pool, `==` vs `.equals()` (5.7)
- **Pass-by-value**, the single most important idea for understanding references (4.2)
- Arrays in depth (4.4)
- Reading input with `Scanner`, and its famous trap (4.5)
- `final` (1.3, 7.4)
- The ternary operator (2.6)
- Arrow-style `switch` and switch expressions (3.2)
- Wrapper classes and autoboxing (5.8)
- `Object`, `toString()`, `equals()` (5.9)
- The full object-creation order (8.4)
- Overriding rules: visibility, return types, exceptions (9.3)
- Exception chaining and try-with-resources (10.7, 10.8)
- Enums (11.6)
- `List`/`Map`/`Set` with generics (11.7)
- Threads, `synchronized`, `volatile` (13.2–13.8)

## 0.4 What was intentionally dropped
- Program logistics (camera rules, muting, session structure)
- AI subscription pricing
- Document 2

One message from those sessions is kept as the guiding principle of this whole course: **AI can write code; your job is to know whether the code is correct, and why.**

---

# Part 1 — Learning Roadmap

```
PHASE A — Writing Java code             PHASE B — Objects & the JVM            PHASE C — Design
M1  How Java runs & storing data   →    M5  Classes & objects             →    M11 Abstraction, interfaces,
M2  Typecasting & operators        →    M6  Inside the JVM                      enums, collections, relationships
M3  Control flow & loops           →    M7  Packages, access, encapsulation →  M12 Design principles (SOLID +)
M4  Methods, arrays & input        →    M8  Inheritance                   →    M13 Singleton & thread safety
                                        M9  Polymorphism                       ★  Final review & mock interview
                                        M10 Exceptions
```

| # | Module | Main topics | Priority | Original class |
|---|---|---|---|---|
| 1 | How Java runs & storing data | Compile → bytecode → JVM, JDK/JRE/JVM, program anatomy, variables, primitives, references, local scope | 🔴 | Class 1, 2 |
| 2 | Typecasting & operators | Widening/narrowing, integer division, overflow, `++`, short-circuit, compound assignment, ternary | 🔴 | Class 1, 2 |
| 3 | Control flow & loops | if/else ladders, switch (classic and arrow), for/while/do-while, for-each, break/continue/labels | 🔴 | Class 1, 2 |
| 4 | Methods, arrays & input | Methods, **pass-by-value**, call stack, arrays, `Scanner`. **Capstone: console Learner Manager** | 🔴 | Class 1, 2 |
| 5 | Classes & objects | Fields, static vs instance, constructors, `this`, references/aliasing, Strings, wrappers, `Object` | 🔴 | Class 1, 2, 4 |
| 6 | Inside the JVM | Class loading, linking, initialization, memory areas, GC, JIT, performance under load, tools | 🟠 (parts 🔴) | Class 2 |
| 7 | Packages, access, encapsulation | FQCN, imports, `private`/default/`public`, getters/setters, behavior methods. **Practice: Report Card** | 🔴 | Class 3 |
| 8 | Inheritance | `extends`, is-a, `protected`, `super`, constructor chaining, object creation order, field hiding, diamond problem | 🔴 | Class 4 |
| 9 | Polymorphism | Overloading, overriding, `@Override`, upcasting/downcasting, dynamic dispatch, static hiding. **Practice: Payroll** | 🔴 | Class 4 |
| 10 | Exceptions | Hierarchy, checked/unchecked, try/catch/finally, throw/throws, custom exceptions, chaining, propagation | 🔴 | Class 2, 3 |
| 11 | Abstraction & relationships | Abstract classes, interfaces, enums, collections, association/aggregation/composition, LLD. **Practice: LMS design** | 🔴 | Class 5 |
| 12 | Design principles | DRY, KISS, YAGNI, SRP, OCP, LSP, ISP, DIP, composition over inheritance. **Practice: Food Court Billing** | 🔴/🟠 | Class 6 |
| 13 | Singleton & thread safety | Threads, `synchronized`, eager/lazy/DCL with `volatile`, holder idiom, pitfalls | 🟠 | Class 7 |
| ★ | Final review | Concept map, must-master list, relationships, interview topics, checklist, cumulative exercises, mock interview | — | — |

**Why this order differs from the classes:**
- **Exceptions come after inheritance.** Custom exceptions are created with `extends`, and catch-block ordering depends on class hierarchies.
- **The JVM internals module comes after classes and objects.** "Static", "heap", and "object" are meaningless until you know what an object is.
- **Instance and static variables move from class 1 to Module 5**, for the same reason.

---

# Module 1 — How Java Runs & How Java Stores Data 🔴

> **Prerequisites:** none.
> **You'll need this for:** everything. Especially §1.5 (references), which is the foundation for objects (M5), pass-by-value (M4), and memory (M6).

**Context (📝 Doc 1):** a typical application has three parts:
- a **client** (browser or mobile app) that sends requests,
- a **server** that runs the business logic, and
- a **database** that stores data.

The flow is *client asks → server processes → database stores → server replies.* Java is one of the most common languages for writing that server. Everything in this course, from variables to design patterns, is about writing server code that is correct and easy to change.

---

## 1.1 What Java is and how a program runs 🔴

### What is it?
Java is a programming language: a precise way of writing instructions. You write them as text in a `.java` file.

The problem is that a computer's processor cannot read that text. It only understands **machine code**, which is raw numeric instructions specific to that chip. Something has to translate.

### Why does it work the way it does?
Different computers speak different machine code. Intel/Windows, ARM/Mac, and Linux servers each need their own instructions.

Languages like C compile your code directly into one machine's language, so you recompile for each platform.

Java's designers at Sun Microsystems (1995) chose a two-step approach:

1. **`javac`** (the Java compiler) translates your `.java` file into **bytecode**, stored in a `.class` file. Bytecode is the machine language of a machine that doesn't physically exist: the **Java Virtual Machine (JVM)**.
2. Each real platform gets its *own* JVM, which turns bytecode into that platform's machine code while the program runs.

```
HelloWorld.java  ──javac──►  HelloWorld.class  ──JVM (java)──►  machine code runs
 (you write)                 (bytecode — same                  (a different JVM per OS
                              file for every OS)                does the last step)
```

This is **"Write Once, Run Anywhere"**: compile once, and the same `.class` file runs anywhere a JVM exists.

**Analogy:** bytecode is a recipe written in a universal cooking notation. Every kitchen (operating system) has its own chef (JVM) who reads the notation and cooks with that kitchen's equipment.

### JDK vs JRE vs JVM 📁 🔴

| Name | What it is | Who needs it |
|---|---|---|
| **JVM** (Java Virtual Machine) | The engine that executes bytecode. It also manages memory and garbage collection. | Anything running Java |
| **JRE** (Java Runtime Environment) | The JVM plus the standard library (`String`, `System`, `java.util`, …) | People who only *run* Java |
| **JDK** (Java Development Kit) | The JRE plus developer tools: `javac`, `javap`, debugger, `jar` | Developers (you) |

**JDK ⊃ JRE ⊃ JVM** — each one contains the next.

➕ **Correction (repo):** "end users only need the JRE" is historical. Since Java 11 there is no separate JRE download, and applications usually bundle a trimmed-down runtime. Know the three-layer model for interviews, but don't claim people download JREs today.

### Compile time vs runtime 🔴

This distinction recurs in almost every module, so learn it now.

- **Compile time** is when `javac` reads your code. It checks grammar (syntax) and types. Errors here are **compile errors**: no `.class` file is produced, and nothing runs.
- **Runtime** is when the JVM executes the bytecode. Some problems only appear here, such as dividing by a number a user typed that happens to be zero. These are **runtime errors (exceptions)**.

You'll see this split again in:
- checked vs unchecked exceptions (M10),
- overloading (the compiler chooses) vs overriding (the JVM chooses at runtime) (M9), and
- casting, which compiles fine but can fail at runtime (M9).

➕ **Modern shortcut:** `java HelloWorld.java` (Java 11+) compiles in memory and runs. It's still compile-then-run underneath.

---

## 1.2 Anatomy of your first program 🔴

📁 Repo `HelloWorld.java`:

```java
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello, World!");
        System.out.print("Java is ");
        System.out.print("fun!");
        System.out.println();
        System.out.println("Welcome to Backend Engineering Launchpad!");
    }
}
```
Output:
```
Hello, World!
Java is fun!
Welcome to Backend Engineering Launchpad!
```

**Line by line:**

- **`public class HelloWorld {`**
  All Java code lives inside a **class**. For now, think of a class as a named container for code. In M5 you'll learn it's a blueprint for objects.
  Rule: a `public` class must live in a file with *exactly* the same name, so this must be `HelloWorld.java`. Case matters.

- **`public static void main(String[] args) {`**
  This is the **entry point**. When you run the program, the JVM searches for a method with *exactly* this shape. A **method** is a named block of instructions. Each word has a job:
  - `public`: the JVM, which is outside your class, must be allowed to call it (M7).
  - `static`: the JVM can call it *without creating an object* of your class first (M5).
  - `void`: it returns nothing.
  - `main`: the exact name the JVM looks for.
  - `String[] args`: an array of text values passed on the command line (arrays: M4).

- **`System.out.println(...)`**
  Prints text, then moves to a new line. `print` prints without moving to a new line. An empty `println()` just ends the current line.

**Syntax rules:**
- Every statement ends in `;`.
- `{ }` group code into **blocks**.
- Java is **case-sensitive**: `String` ≠ `string`, `System` ≠ `system`.
- Comments: `// one line`, `/* several lines */`, `/** documentation (Javadoc) */`.

**Common mistakes:**
- Writing `string` instead of `String`. Even the instructor's quick sketches in the repo do this.
- A missing `;`.
- A file name that doesn't match the class name.
- **`Main` instead of `main`.** This one is sneaky: it **compiles fine**, because `Main` is a legal method name. At runtime you get:
  ```
  error: can't find main(String[]) method in class: Bank
  ```
  It's a perfect example of compile time vs runtime.

---

## 1.3 Variables 🔴

### What is it?
A **variable** is a named piece of memory that holds a value. Every variable has a **type**, which defines what it can hold.

```java
int age;          // declaration: reserve space for an int named age
age = 25;         // assignment
int xp = 120;     // declaration + initialization
xp = 150;         // re-assignment: the old value is replaced
```

### Why types?
Java is **statically typed**: each variable's type is fixed at compile time and never changes. `age = "hello";` is rejected *before* the program runs.

Dynamically typed languages like Python and JavaScript discover such mistakes only while running. Java catches whole categories of bugs before deployment, which matters enormously in large server codebases.

➕ `var count = 5;` (Java 10+) lets the compiler infer the type (`int`). The type is still fixed forever. `var` is allowed only for local variables.

### Naming
- **Rules** (enforced by the compiler):
  - letters, digits, `_`, `$`
  - can't start with a digit
  - can't be a keyword (`class`, `int`, …)
- **Conventions** (expected by every team):
  - `camelCase` for variables and methods (`totalStudents`)
  - `PascalCase` for classes (`StudentReportCard`)
  - `UPPER_SNAKE_CASE` for constants

### `final` ➕ 🔴
`final` means "can be assigned only once":
```java
final double GST_RATE = 0.18;
GST_RATE = 0.20;   // compile error: cannot assign a value to final variable GST_RATE
```
You'll use this for the DRY principle (M12: "one source of truth for the GST rate"), for immutable fields (M7), and for the eager singleton (M13).

---

## 1.4 The 8 primitive types 🔴

### What are they?
**Primitive** types are Java's built-in basic value types. A primitive variable holds the value *itself*. There are exactly eight (📁 repo `PrimitiveTypes.java`):

| Type | Size | Range / values | Typical use |
|---|---|---|---|
| `byte` | 1 byte | −128 to 127 | Raw file/network data |
| `short` | 2 bytes | −32,768 to 32,767 | Rare |
| `int` | 4 bytes | ≈ ±2.1 billion (max 2,147,483,647) | **Default whole number** |
| `long` | 8 bytes | ≈ ±9.2 × 10¹⁸ | IDs, timestamps, big counts |
| `float` | 4 bytes | ~7 significant digits | Rare |
| `double` | 8 bytes | ~15–16 significant digits | **Default decimal** |
| `char` | 2 bytes | One UTF-16 character, e.g. `'A'` | Single characters |
| `boolean` | unspecified ➕ | `true` / `false` | Conditions, flags |

(1 byte = 8 bits; a bit is a single 0 or 1. Each extra bit doubles the number of possible values.)

➕ **Corrections (repo):**
- `boolean` is listed as "1 bit". The specification deliberately doesn't define its size; JVMs typically use a byte.
- "Everything is an object in Java" is false. These eight types are *not* objects. They exist for speed and memory efficiency. This is why Java is called "not purely object-oriented".

### Why so many integer types?
Memory was expensive in the 1990s. Today, use:
- `int` by default,
- `long` when values can exceed about 2 billion, and
- `byte` for raw binary data.

`short` and `float` are rare in modern code.

### Literals (the source of many compile errors)
A **literal** is a value written directly in code. Java gives literals default types:
- **whole-number literals are `int`**, and
- **decimal literals are `double`**.

Verified:
```java
long big = 9999999999;     // error: integer number too large
long big = 9999999999L;    // OK: L makes it a long literal

float price = 3.14;        // error: possible lossy conversion from double to float
float price = 3.14f;       // OK: f makes it a float literal

long readable = 9_999_999_999L;  // underscores allowed for readability
```
Why does the first one fail even though a `long` can hold it? The compiler reads `9999999999` as an **`int` literal first**, and it's too big for an `int`.

### `char` is secretly a number ➕ 🟠
A `char` stores a Unicode code number. `'A'` = 65, `'B'` = 66, `'a'` = 97.
```java
char c = 'A';
c++;                          // c is now 'B'
System.out.println('A' + 1);  // 66: char + int is arithmetic, so the result is an int
```
- Single quotes `'A'` make a `char`. Double quotes `"A"` make a `String`.
- `'AB'` is a compile error.

### `boolean` is not a number
Only `true` and `false` exist. `boolean b = 1;` is a compile error. Unlike C, `0` doesn't mean false.

### Decimals are not exact 🔴 (frequently asked in interviews)
```java
System.out.println(0.1 + 0.2);   // 0.30000000000000004
```
**Why:** `double` stores numbers in binary. Some decimal fractions, like 0.1, have no exact binary form, the same way 1/3 has no exact decimal form (0.333…). What gets stored is a very close approximation.

**Consequence:** never store money in `double`. Real systems store money as a whole number of the smallest unit (paise, cents) in a `long`, or use `BigDecimal`. You'll see this again when reviewing the Food Court Billing solution (M12).

---

## 1.5 Reference types: String, arrays, `null` 🔴

This is the most important idea in Module 1. 📝 Doc 3's statement that "`B2 = B1` copies the reference, not the object" depends entirely on it.

### What is it?
Anything that isn't one of the 8 primitives is a **reference type**: `String`, arrays, and every class you will ever write.

A reference variable **does not hold the object**. It holds a **reference**, which you can think of as the object's address. The object lives elsewhere in memory (the *heap*, M6).

**Analogy:**
- A primitive variable is a **box with the value inside**.
- A reference variable is a **TV remote**. The TV (the object) is somewhere else.
- Two remotes can control the same TV.
- A remote can be paired with no TV at all. That's `null`.

### Why does Java do this?
Objects can be huge: a million orders, or a long document. Copying the whole object on every assignment or method call would be slow and wasteful. Copying an address is cheap. The price you pay is that you must understand **sharing**.

### See the difference (verified)
```java
// Primitives copy the VALUE
int x = 5;
int y = x;     // y gets its own copy of 5
y = 10;
System.out.println(x);   // 5

// References copy the ADDRESS
int[] a = {1, 2, 3};
int[] b = a;   // two remotes, ONE array
b[0] = 99;
System.out.println(a[0]);  // 99
```
```
 a ──────┐
         ├──►  [ 99 | 2 | 3 ]     ← one array object on the heap
 b ──────┘
```
📁 The instructor's comment in `NonPrimitiveTypes.java` shows the same effect through a method: a method receives a `User`, does `u2 = u; u2.name = "XYZ"`, and the caller then sees `"XYZ"`. Two references, one object. This becomes **pass-by-value** in M4.

### Arrays (intro; full treatment in M4)
```java
int[] scores = {95, 88, 72, 100};
System.out.println(scores[0]);                    // 95 (indexes start at 0)
System.out.println(scores.length);                // 4 (a property, so no parentheses)
System.out.println(scores[scores.length - 1]);    // 100: the last element
System.out.println(scores);                       // something like [I@40e6dfe1, NOT the contents
```
The last line surprises everyone. It prints the array object's default description: `[I` means "array of int", followed by an identifying number. To see the contents, use `java.util.Arrays.toString(scores)` ➕.

### String (intro; internals in M5)
`String` is a class (capital S) that holds text in double quotes and has methods:
```java
String name = "Airtribe";
System.out.println(name.length());       // 8
System.out.println(name.toUpperCase());  // AIRTRIBE
```
**`+` evaluates left to right** (verified):
```java
System.out.println('A' + 1);        // 66: arithmetic
System.out.println("" + 'A' + 1);   // A1: "" + 'A' gives "A", then "A" + 1 gives "A1"
```
Once the left operand of a `+` is a String, `+` means "join text". Before that point, it's arithmetic.

### `null`
```java
String city = null;                    // remote paired with no TV
System.out.println("City: " + city);   // City: null   (fine)
System.out.println(city.length());     // compiles, then crashes: NullPointerException
```
Primitives can **never** be `null`: `int n = null;` is a compile error. A box always contains *some* value.

### Wrapper classes (preview) ➕ 🟠
📁 The repo contains `Integer i1 = 4;`, which is never explained in your notes. Every primitive has an object version: `Integer`, `Long`, `Double`, `Character`, `Boolean`, … They exist because some parts of Java only work with objects, notably collections (M11). Java converts between the two automatically (**autoboxing**). A wrapper *can* be `null`. Details in M5.

### Default values 🔴
**Fields** (variables in a class, outside methods; M5) and **array elements** get automatic defaults:
- `0` for numbers
- `false` for `boolean`
- `'\u0000'` for `char`
- `null` for references

```java
int[] counts = new int[3];   System.out.println(counts[1]);  // 0
String[] names = new String[2]; System.out.println(names[0]); // null
```
**Local variables get no default.** See §1.6.

---

## 1.6 Local variables and scope 🔴

### What is it?
**Scope** is the region of code where a variable exists. A **local variable** is declared inside a method or a block `{ }`. It lives from its declaration to the closing `}` of that block.

### Three rules (all verified with real compiler errors)

**Rule 1: you can't use a local outside its block.**
```java
{ int inner = 5; }
System.out.println(inner);     // error: cannot find symbol
```

**Rule 2: a local must be definitely assigned before it's read.**
```java
int total;
System.out.println(total);     // error: variable total might not have been initialized
```
*Why do fields get defaults but locals don't?*
- When the JVM creates an object, it wipes the object's memory to zeros. That's cheap, and gives safe defaults.
- For locals, the compiler can analyze the single method and *prove* whether a value was assigned before use. So Java turns "you forgot to initialize this" into a compile error.

That's stricter and more helpful than silently giving you 0. C, by contrast, would hand you whatever leftover garbage was in memory.

**Rule 3: you can't redeclare a name that's still in scope, even in an inner block.**
```java
int x = 1;
{ int x = 2; }   // error: variable x is already defined in method main(String[])
```
C and JavaScript allow this; Java forbids it to prevent confusion. Two *separate, non-overlapping* blocks may each declare their own `x`.

**Preview:** a local variable **can** share a name with a *field*. The local then hides the field, which is why `this.name = name` exists (M5).

📁 The repo's `VariableScopeDemo` also introduces instance and static variables. They're taught in **M5**, because they need objects.

---

## 1.7 Comparisons at a glance

| | Primitive | Reference |
|---|---|---|
| Holds | The value itself | An address of an object |
| `=` copies | The value (independent copies) | The address (shared object) |
| Can be `null` | No | Yes |
| Has methods | No | Yes (`name.length()`) |
| Examples | `int`, `double`, `char`, `boolean` | `String`, `int[]`, `Integer`, your classes |

- `print` vs `println`: stays on the line vs moves to a new line.
- `float` vs `double`: ~7 digits and needs `f` vs ~15 digits and is the default.
- `char` vs `String`: one character, single quotes, really a number vs any text, double quotes, an object.

**Real-world usage:**
- `long` for database IDs and timestamps. `System.currentTimeMillis()` returns a `long`.
- `boolean` flags (`isActive`, `isPaid`).
- `String` for almost all external data (names, emails, JSON).
- `byte[]` for file uploads and network data.
- Money is never stored in `double` in production.

---

## 1.8 Interview preparation — Module 1

**Frequently asked**

1. **Why is Java platform-independent?**
   *Strong answer:* "javac compiles source into bytecode, which isn't tied to any CPU or OS. Each platform has its own JVM that translates bytecode to native code, so the bytecode is portable."
   *Follow-up trap:* **"Is the JVM platform-independent?"** No. The JVM is platform-*specific*, which is exactly *why* bytecode can be platform-independent.

2. **JDK vs JRE vs JVM?** Give the nesting and who needs what. Mention Java 11 to stand out.

3. **Primitive vs reference types?**
   *Strong answer:* storage (value vs address), what `=` copies, `null`, primitives aren't objects. Then the *why*: copying addresses is cheap, but it means objects can be shared.

4. **Is Java purely object-oriented?** No, because of the 8 primitives. Wrapper classes bridge the gap.

**"Why" questions**

5. **Why doesn't a local variable get a default value when a field does?**
   *Strong answer:* "The compiler can verify locals within one method, so Java makes 'read before assignment' a compile error. That catches bugs instead of hiding them behind a silent 0. Fields live in memory the JVM zeroes on object creation, and their assignment can't be tracked across all methods, so they get defaults."

6. **Why is `0.1 + 0.2` not `0.3`? What do you use for money?**
   Binary floating point can't represent 0.1 exactly. Use `BigDecimal`, or a `long` count of paise or cents.

**Tricky**

7. Why does `long x = 9999999999;` fail?
8. `Main` vs `main`: compile error or runtime error?
9. What does `System.out.println(new int[]{1,2})` print, and why?

---

## 1.9 Check yourself — Module 1

**Q1 (explain).** In 3–4 sentences, explain to a non-programmer why Java compiles to bytecode instead of machine code. Is the JVM itself platform-independent? Why or why not?

**Q2 (predict the output, then run it).**
```java
int[] scores = {10, 20, 30};
int[] backup = scores;
backup[1] = 0;
int total = scores[0];
int copy = total;
copy = 100;
System.out.println(scores[1] + " " + total);
System.out.println('A' + 'B');
System.out.println("A" + 'B' + 1);
System.out.println(1 + 2 + "3" + 4 + 5);
```

**Q3 (find the bugs).** The file is `Bank.java`. List every problem, say whether it's a compile error or a runtime error, and give the fix. There are at least 7.
```java
public class Bank {
    public static void Main(String[] args) {
        long accountNumber = 12345678901;
        float rate = 4.5;
        double balance;
        String owner = 'Ravi';
        boolean active = 1;
        if (true) {
            int bonus = 50;
        }
        System.out.println(owner + balance + bonus);
    }
}
```

**Q4 (choose and justify).** Choose a type for each of the following:
- (a) a user's age
- (b) milliseconds since 1970
- (c) whether an order is paid
- (d) a product price in rupees (careful)
- (e) a grade letter
- (f) Earth's population
- (g) a customer's name
- (h) a phone number (careful)

**Q5 (why).** Why can a `String` variable be `null` but an `int` cannot? Use the box/remote idea.

### Progress tracker — Module 1
- **You should now understand:**
  - the `.java` → bytecode → JVM pipeline
  - compile time vs runtime
  - the 8 primitives
  - value copying vs reference copying
  - `null`
  - defaults vs definite assignment
  - block scope
- **You should be able to code:** a runnable class with `main`; variables of every primitive with correct literal suffixes; Strings and arrays (indexing, `.length`); printing and concatenation.
- **Interview topics:** platform independence and the JVM trap; JDK/JRE/JVM; primitive vs reference; floating-point and money; default values.
- **Common mistakes:**
  - missing `L`/`f`
  - `'` vs `"`
  - `string`, `Main`
  - expecting `b = a` to copy an array
  - printing an array directly
  - calling a method on `null`
  - reading an uninitialized local
- **Prerequisites for M2:** type sizes; literal default types; `char` is a number; `+` works left to right.

### Exercises — Module 1

**🟢 Beginner — `Profile.java`.** Declare variables for:
- your name
- age
- height in metres
- first initial (`char`)
- whether you're a student
- phone number

Print a neat profile using both `print` and `println`.

<details><summary>Hint 1</summary>A phone number looks like a number but you never do arithmetic on it, and it can start with 0 or +. What does that suggest about its type?</details>

**🟡 Intermediate.** Create an `int[]` of 5 XP values. Without loops, print:
- the first value,
- the last value (using `.length`, so it still works if the size changes), and
- the sum.

Then make a second variable refer to the same array, double the first element through it, and print the first element via the *original* variable. Explain the result in a comment.

**🔴 Interview-level.** Without running it, draw the memory state as comments after each line (variables on the left, objects on the right, arrows for references). Then give the final `names[0]` and `other[0]`.
```java
int a = 5;
int b = a;
b++;
String[] names = {"Asha", "Bilal"};
String[] other = names;
other[0] = "Zara";
other = new String[]{"Quinn"};
other[0] = "Ravi";
```
<details><summary>Hint 1</summary>Distinguish "changing the TV a remote controls" from "re-pairing the remote to a new TV".</details>
<details><summary>Hint 2</summary>After <code>other = new String[]{"Quinn"}</code>, how many arrays exist, and which remote points to which?</details>

---

# Module 2 — Typecasting & Operators 🔴

> **Prerequisites:** M1 (type sizes, literal default types, `char` is a number, String `+`).
> **You'll need this for:** every calculation you ever write; M3 (conditions are built from these operators); M9 (casting *objects* reuses the same "widening = safe, narrowing = explicit" logic).

---

## 2.1 Type conversion: widening and narrowing 🔴

### What is it?
**Typecasting** (type conversion) means turning a value of one type into another type. 📝 For example, storing an `int` in a `double`, or turning a `double` into an `int`.

### Why does it exist?
Calculations mix types all the time. An average is a total (`int`) divided by a count (`int`), but the answer needs decimals. A price (`double`) sometimes needs to become a whole number of rupees (`int`).

Java must decide: when is a conversion obviously safe, and when could it destroy information? The rule it uses is based purely on **what the destination type can hold**.

### How does it work?

**Widening** (small → large) is **automatic**. 📝📁 The notes call it "parking a bike in a car-sized space":
```
byte → short → int → long → float → double
               char ↗
```
```java
int    count = 150;
long   asLong   = count;    // automatic
double asDouble = count;    // automatic → 150.0
int    code     = 'A';      // char → int: 65
```

**Narrowing** (large → small) **must be explicit**, because information may be lost: "parking a car in a bike spot". You write the target type in parentheses. That **cast** is you telling the compiler, "I know this might lose data, and I accept that."
```java
double price = 99.95;
int rupees = price;          // compile error: possible lossy conversion from double to int
int rupees = (int) price;    // 99
```

### What actually happens to the data (verified) 🔴

**1. Decimal → integer truncates toward zero. It does not round.**
```java
(int) 99.95   // 99
(int) -7.9    // -7   (toward zero, not -8)
Math.round(2.5)   // 3   ← use Math.round if you want rounding ➕
Math.round(-2.5)  // -2  (rounds half up, toward positive infinity)
```

**2. Integer → smaller integer keeps only the low bits (wrap-around).** 📁 From the repo:
```java
int bigInt = 300;
byte b = (byte) bigInt;   // 44
```
*Why 44?* 300 in binary is `1 0010 1100`. A `byte` keeps only the lowest 8 bits (`0010 1100`), which is 44. Equivalently, 300 − 256 = 44. There's no error and no warning. The value is just different.

**3. ➕ Correction: "widening never loses data" is not quite true.** `int → float`, `long → float` and `long → double` are automatic, but can lose **precision**, because `float` has only ~7 significant digits:
```java
long big = 123456789123456789L;
float f = big;                    // compiles silently
System.out.println(f);            // 1.2345679E17  ← the trailing digits are gone
```
The *magnitude* (range) always fits, but not every digit does.

**4. What you can't cast:**
- `boolean` to or from anything.
- `String` to `int` with a cast. `(int) "42"` is a compile error. ➕ Use `Integer.parseInt("42")` instead. That's a *parsing method*, not a cast, and it can throw `NumberFormatException` at runtime (M10).

### What happens internally?
The compiler decides every conversion at **compile time**, using the declared types. For a narrowing cast it emits a specific bytecode instruction; for example, `int → byte` becomes `i2b`, which you can see with `javap -c` (M6).

The cast creates a **new value**. The original variable is untouched:
```java
double price = 99.95;
int rupees = (int) price;
System.out.println(price);  // still 99.95
```

### Common mistakes
- Expecting `(int)` to round.
- Forgetting that narrowing integers silently wraps.
- Thinking a cast changes the original variable.
- Trying to cast a String into a number.

---

## 2.2 Arithmetic operators and the integer-division trap 🔴

📝📁 Operators: `+ - * / %` (`%` is the **remainder**).

### Integer division (the #1 beginner bug)
**If both operands are integers, the result is an integer**, and the fraction is discarded (verified):
```java
10 / 4              // 2
10 / 4.0            // 2.5   (one operand is double → double division)
(double) 10 / 4     // 2.5   (cast happens FIRST, then the division)
(double) (10 / 4)   // 2.0   (integer division happens first; the 2 is then widened)
7 / 2 * 2.0         // 6.0   (7/2 = 3 first, then 3 * 2.0)
```
📁 The Learner Manager exercise hint says "cast to double for the average". This is why: `sum / count` with two ints silently drops the decimals.

### How Java chooses the result type (➕ "binary numeric promotion")
For arithmetic, Java first promotes both operands:
- If either is `double`, both become `double`.
- else if either is `float`, both become `float`.
- else if either is `long`, both become `long`.
- **otherwise both become `int`, even if they were `byte`, `short`, or `char`.**

That last rule explains a famous compile error (verified):
```java
byte b = 10;
b = b + 1;      // error: possible lossy conversion from int to byte
```
`b + 1` is an `int`, and storing an `int` into a `byte` is narrowing. See §2.4 for why `b += 1` *does* compile.

### Remainder `%`
- `10 % 3` is 1.
- The sign follows the **left** operand: `-7 % 2` is −1, and `7 % -2` is 1.
- `-7 / 2` is −3 (truncation toward zero again).
- Typical uses:
  - even/odd: `n % 2 == 0`
  - cycling through positions: `i % size`
  - last digit: `n % 10`

### Division by zero (verified)
```java
5 / 0      // runtime: ArithmeticException: / by zero
5.0 / 0    // Infinity      (no exception for floating point!)
0.0 / 0    // NaN           ("not a number")
```

### Overflow ➕ 🔴
Integers **wrap around silently** when they exceed their range:
```java
Integer.MAX_VALUE + 1                // -2147483648
int ms  = 1000 * 60 * 60 * 24 * 30;  // -1702967296  ← 30 days in ms overflowed int!
long ms2 = 1000L * 60 * 60 * 24 * 30; // 2592000000   ← correct
```
**A trap that bites experienced developers:** `long ms = 1000 * 60 * 60 * 24 * 30;` is *still wrong*. The right-hand side is computed entirely in `int` and overflows *before* it's widened to `long`. Making the first operand a `long` (`1000L`) makes the whole calculation `long`. ⚪ `Math.addExact` and `Math.multiplyExact` throw on overflow instead of wrapping.

---

## 2.3 Increment and decrement: `++` and `--` 🔴

📝📁 `x++` (post-increment) and `++x` (pre-increment) **both add 1 to `x`**. The difference is the **value the expression produces**:

| Expression | Value produced | Effect on x |
|---|---|---|
| `x++` | the **old** value | x becomes x+1 |
| `++x` | the **new** value | x becomes x+1 |

Verified:
```java
int c = 5;
System.out.println(c++ + " " + c + " " + ++c);   // 5 6 7
```
- `c++` produces 5, then c becomes 6.
- `c` is 6.
- `++c` makes c 7 and produces 7.

📁 From the repo's `OperatorsDemo`:
```java
int x1 = 10, y1 = 5;
int result  = ++x1 + ++y1;   // 11 + 6 = 17
int result1 = ++x1 + ++y1;   // 12 + 7 = 19
```

### The classic trick question ➕
```java
int x = 5;
x = x++;
System.out.println(x);   // 5 (!)
```
Here's what happens:
1. `x++` saves the old value (5).
2. It increments x to 6.
3. The expression's value is the saved 5.
4. Then the assignment `x = 5` overwrites the 6.

**Lesson:** never use `++` inside a larger expression that also assigns the same variable. In real code, write `x++;` on its own line.

---

## 2.4 Assignment and compound assignment 🟠

📝📁 `=`, `+=`, `-=`, `*=`, `/=`, `%=`. `x += n` *roughly* means `x = x + n`.

➕ **The hidden detail:** a compound assignment **includes an automatic cast back to the variable's type** (verified):
```java
byte b = 10;
b += 1;          // compiles! Means b = (byte)(b + 1) → 11

int i = 3;
i += 2.7;        // compiles! Means i = (int)(3 + 2.7) → 5 (silent truncation)
```
This explains why `b += 1` works when `b = b + 1` doesn't. It also means `+=` can silently lose data, so watch out for it.

---

## 2.5 Relational and logical operators 🔴

### Relational (comparison)
`==`, `!=`, `>`, `<`, `>=`, `<=` all produce a `boolean`.

**`=` vs `==`:** `=` assigns; `==` compares.

**`==` on references compares addresses, not contents (verified):**
```java
String a1 = "hi", a2 = "hi", a3 = new String("hi");
a1 == a2        // true   (both literals point to the same pooled object; M5)
a1 == a3        // false  (different objects)
a1.equals(a3)   // true   (same characters)
```
**Rule: compare Strings (and objects in general) with `.equals()`, never `==`.** Full explanation in M5.7.

### Logical
📝 `&&` (AND: both true), `||` (OR: at least one true), `!` (NOT).

### Short-circuit evaluation ➕ 🔴
- `&&` stops as soon as the left side is `false`, because the result can't become true.
- `||` stops as soon as the left side is `true`.

This isn't just an optimization. Real code depends on it:
```java
String s = null;
if (s != null && s.length() > 0) { ... }   // safe: s.length() is never called when s is null
if (s.length() > 0 && s != null) { ... }   // NullPointerException: wrong order
```
⚪ The single `&` and `|` also work on booleans but **don't** short-circuit. Avoid them for conditions.

### Precedence (simplified)
From highest to lowest:
```
()   →   ++ -- !   →   * / %   →   + -   →   < > <= >=   →   == !=   →   &&   →   ||   →   ?:   →   = += ...
```
`a || b && c` means `a || (b && c)`. **When in doubt, use parentheses.** Readers will thank you (KISS, M12).

---

## 2.6 The ternary operator `?:` ➕ 🟠
A compact if/else that **produces a value**:
```java
int age = 20;
String label = age >= 18 ? "adult" : "minor";   // "adult"
```
Format: `condition ? valueIfTrue : valueIfFalse`. Use it for simple choices. Nested ternaries quickly become unreadable.

---

## 2.7 String concatenation, revisited 🔴
Remember from M1 that `+` works left to right and switches to text-joining once a String appears. Verified:
```java
1 + 2 + "" + 1 + 2      // "312"   (1+2=3, then "3"+1="31", then "31"+2="312")
'a' + 'b' + "c"         // "195c"  (97+98=195 first, then "195"+"c")
"Sum: " + 2 + 3         // "Sum: 23"
"Sum: " + (2 + 3)       // "Sum: 5"
```

---

## 2.8 Comparison summary

| Concept | Remember |
|---|---|
| Widening vs narrowing | Automatic and (usually) safe vs explicit cast and possibly lossy |
| `(int)` vs `Math.round` | Truncates toward zero vs rounds |
| `x++` vs `++x` | Produces the old value vs the new value; both increment |
| `b = b + 1` vs `b += 1` | Compile error for byte vs hidden cast |
| `==` vs `.equals()` | Same object? vs same content? |
| `&&` vs `&` | Short-circuits vs always evaluates both sides |
| int `/ 0` vs double `/ 0` | Exception vs `Infinity` |

**Real-world usage:**
- Averages (cast before dividing).
- Percentages in the report card (M7).
- Tax and discount math in billing (M12).
- Time math with `long`.
- Null-safe checks with `&&`, used constantly in server code.
- `%` for pagination and sharding (`userId % numberOfServers`).

---

## 2.9 Interview preparation — Module 2

1. **Widening vs narrowing, with examples. Why does narrowing need an explicit cast?**
   *Strong answer:* name the direction, the automatic vs explicit rule, and the *reason*: the destination may not fit the value. The cast is the programmer's explicit acceptance of possible loss. Bonus: mention that `long → float` widening can lose precision.
2. **What is `(byte) 300`? Explain.** (44: only the low 8 bits are kept.)
3. **Why does `byte b = 10; b = b + 1;` fail, but `b += 1` work?** (Arithmetic promotes to `int`; compound assignment casts back.)
4. **What does `x = x++` leave in x?** Explain step by step.
5. **What's the output of `5/2`, `5/2.0`, `(double)5/2`, `(double)(5/2)`?**
6. **What is short-circuit evaluation? Give a real case where the order of operands matters.** (Null check before method call.)
7. **Why is `long ms = 1000 * 60 * 60 * 24 * 30;` a bug?**
8. **Tricky:** `System.out.println('a' + 'b' + "c");`

---

## 2.10 Check yourself — Module 2

**Q1 (predict, then run).**
```java
int a = 7, b = 2;
double avg = a / b;
System.out.println(avg);
System.out.println((double) a / b);
System.out.println(a % b + " " + -a % b);
int i = 10;
int j = i++ + ++i;
System.out.println(i + " " + j);
System.out.println((int) 3.99 + (int) -3.99);
System.out.println(10 + 20 + "30" + 40);
```

**Q2 (bug hunt).** A teammate wrote this to compute a 30-day subscription expiry in milliseconds, and a percentage. Find both bugs and fix them.
```java
long now = System.currentTimeMillis();
long expiry = now + 30 * 24 * 60 * 60 * 1000;
int passed = 45, total = 60;
double percent = passed / total * 100;
```

**Q3 (explain).** Explain in your own words why `if (user != null && user.isActive())` is safe but `if (user.isActive() && user != null)` is not.

**Q4 (choose).** You need to convert `9.99` to a whole number of rupees for a display that should show the *nearest* rupee. Which do you use, `(int)` or `Math.round`? Why? What type does `Math.round(double)` return? Look it up; it matters for the assignment.

### Progress tracker — Module 2
- **Understand:** widening and narrowing, and what happens to the bits; numeric promotion; integer division; overflow; pre vs post increment; compound-assignment casts; short-circuiting; `==` on references.
- **Can code:** correct averages and percentages; safe conversions; null-safe conditions; ternaries.
- **Interview topics:** casting rules; `(byte)300`; `x = x++`; integer division; short-circuit; overflow.
- **Common mistakes:** `sum / count` with ints; `(double)(a/b)`; expecting `(int)` to round; `int`-overflow in `long` math; `==` on Strings.
- **Prerequisites for M3:** boolean expressions, `&&`/`||`, `%`, `++`.

### Exercises — Module 2

**🟢 Beginner.** Given `int totalSeconds = 98765;`, print it as `hours:minutes:seconds` using only `/` and `%`.
<details><summary>Hint 1</summary>One hour is 3600 seconds. What's left after removing whole hours? <code>%</code> gives you the leftover.</details>

**🟡 Intermediate.** A bill has subtotal `1249` (rupees, `int`). Tax is 5%, and a discount of 10% applies if the subtotal is at least 1000 (use a ternary). Compute the final amount **in paise as a `long`**, avoiding `double` entirely. Print it as rupees with exactly two decimals using only integer math, for example `1311.45`.
<details><summary>Hint 1</summary>Convert to paise first (×100). 5% of x is x*5/100. Watch the order of * and / to avoid losing precision.</details>
<details><summary>Hint 2</summary>For printing: rupees = paise / 100, and the remaining paise = paise % 100. How do you print 5 as "05"?</details>

**🔴 Interview-level.** Without running, predict the output and explain *every* step. Then run it.
```java
byte b = 127;
b++;
short s = 10;
s += 100000;
int big = Integer.MAX_VALUE;
long wrong = big + 1;
long right = big + 1L;
System.out.println(b + " " + s + " " + wrong + " " + right);
```
<details><summary>Hint 1</summary><code>b++</code> on a byte works like <code>b += 1</code>, including the hidden cast. What is one more than the byte maximum, after wrap-around?</details>
<details><summary>Hint 2</summary>For <code>s</code>: compute (10 + 100000) and keep only the low 16 bits (a short holds 65536 distinct values).</details>

---

# Module 3 — Control Flow & Loops 🔴

> **Prerequisites:** M2 (boolean expressions, `&&`, `||`, `%`, `++`).
> **You'll need this for:** every program; the M4 capstone (menu loop with `switch`); KISS and early returns (M12); OCP, which replaces long if/else chains (M12).

**Control flow** is how a program decides *which* statements run and *how many times*. Without it, code runs top to bottom exactly once.

---

## 3.1 `if`, `if-else`, and the else-if ladder 🔴

### What and why
📝📁
- `if` runs a block only when a condition is `true`.
- `if-else` chooses between two paths.
- An **else-if ladder** checks several conditions **in order**, and **the first match wins**.

```java
int score = 82;
String grade;
if (score >= 90) {
    grade = "A";
} else if (score >= 75) {
    grade = "B";
} else {
    grade = "C";
}
System.out.println(grade);   // B
```

### How it works
Java evaluates conditions top-down. As soon as one is true, it runs that block and **skips the rest of the ladder**. So ordering matters:
```java
// BUG: every score >= 75 (including 95) gets "B", because the >= 90 branch is never reached for them
if (score >= 75) grade = "B";
else if (score >= 90) grade = "A";
```
**Rule for range ladders:** check from the most restrictive condition to the least restrictive.

### Java-specific rules and traps ➕
- **The condition must be a `boolean`.** `if (x = 5)` is a compile error in Java (verified: `int cannot be converted to boolean`). That's a safety feature C doesn't have.
- **But** `if (flag = false)` *compiles*, because the assignment produces a boolean. It assigns `false`, so the if-body never runs (verified: prints the else branch). Write `if (!flag)` instead.
- **Braces are optional for a single statement, but dangerous:**
  ```java
  if (isAdmin)
      grantAccess();
      logAccess();      // NOT inside the if, despite the indentation. It always runs.
  ```
  Always use braces.

---

## 3.2 `switch` 🔴

### What is it?
📝📁 `switch` matches **one value** against a list of **exact constants**. It's a cleaner alternative to an else-if ladder made purely of `==` checks.

```java
int day = 2;
switch (day) {
    case 1:  System.out.println("Monday");  break;
    case 2:  System.out.println("Tuesday"); break;
    default: System.out.println("Other day");
}
```

### How it works: fall-through ➕ 🔴 (the classic trap)
In a classic `switch`, `case` labels are just **entry points**. Execution starts at the matching label and **keeps going** through the following cases until it hits a `break` or the end. Verified:
```java
int day = 2;
switch (day) {
    case 1: System.out.println("Mon");
    case 2: System.out.println("Tue");      // starts here...
    case 3: System.out.println("Wed");      // ...falls through into here
            break;                           // ...stops here
    default: System.out.println("Other");
}
// Output: Tue
//         Wed
```
Fall-through is occasionally intentional, for example grouping cases:
```java
case 6: case 7: System.out.println("Weekend"); break;
```

### What can be switched on?
- Allowed: `int`-family types (`byte`, `short`, `char`, `int` and their wrappers), `String` (Java 7+), and **enums** (M11).
- Not allowed: `long`, `float`, `double`, `boolean`.
- 📝 No ranges or comparisons: `case score >= 90:` is illegal.

### Modern switch (Java 14+) ➕ 🟠
Arrow labels have **no fall-through**, and a switch can **produce a value** (a *switch expression*):
```java
String cmd = "stop";
switch (cmd) {
    case "go"   -> System.out.println("going");
    case "stop" -> System.out.println("stopping");   // runs only this
    default     -> System.out.println("?");
}

int day = 6;
String type = switch (day) {
    case 1, 2, 3, 4, 5 -> "Weekday";
    case 6, 7          -> "Weekend";
    default            -> "Invalid";
};
```
In new code, prefer arrow syntax: it eliminates the forgotten-`break` bug class. You'll still meet classic switches in existing code and interviews.

### `if-else` vs `switch` 📝📁

| Use `if-else` when… | Use `switch` when… |
|---|---|
| Conditions are ranges (`>= 90`) | Matching one variable to exact values |
| Conditions involve several variables | Menu options, day numbers, command strings, enum states |
| Conditions are complex (`!= null && ...`) | There are many fixed options and readability matters |

➕ **Looking ahead:** when a switch or if-chain on a *type* keeps growing (`if (type.equals("UPI")) … else if ("CARD") …`), that's the signal to use polymorphism instead. That's the Open/Closed Principle (M12).

---

## 3.3 Loops: `for`, `while`, `do-while` 🔴

### Why loops?
To repeat work without repeating code: processing every learner, retrying an operation, keeping a menu running.

### `for`: initialization, condition, and update in one place
```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```
The order of execution:
1. `int i = 1` runs **once**.
2. The condition `i <= 5` is checked **before each iteration**. If it's false, the loop ends.
3. The body runs.
4. The update `i++` runs, then go back to step 2.

**Scope:** `i` exists only inside the loop.

📝 Best when the count is known, or when initialization, condition and update belong together.

### `while`: condition checked before each iteration
```java
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;                    // forget this and you have an infinite loop
}
```
📝 Best when you repeat *until something becomes true*, and the number of iterations isn't known in advance: reading input until the user types "exit", or polling until a job finishes.

### `do-while`: the body runs at least once
```java
int num = 10;
do {
    System.out.println("runs once even though num < 10 is false");
} while (num < 10);        // note the semicolon
```
📝 Best when you can't know whether to repeat until *after* the first attempt. The notes' example is generating a random ID, checking whether it already exists in the database, and repeating if it does:
```java
long id;
do {
    id = generateRandomId();
} while (idExistsInDatabase(id));   // you can't check before you've generated one
```
Another natural fit is a menu: show it at least once, then repeat until the user exits.

📁 Comparison:

| Loop | When to use | Runs at least once? |
|---|---|---|
| `for` | Known count / index-based | Only if the condition is initially true |
| `while` | Unknown count, check first | No |
| `do-while` | Unknown count, must run once | Yes |

📝 "The best loop is the one that matches the problem's structure." Any loop can be rewritten as another. Pick the most readable one.

### Common loop bugs ➕
- **Infinite loop:** the condition never becomes false (forgot `i++`, or updated the wrong variable).
- **Off-by-one:** `i <= arr.length` instead of `i < arr.length`. That gives `ArrayIndexOutOfBoundsException` on the last pass.
- **Stray semicolon** (verified):
  ```java
  int k;
  for (k = 0; k < 3; k++);          // ← this ; is an EMPTY loop body
  System.out.println("k=" + k);     // prints once: k=3
  ```

---

## 3.4 The enhanced for loop (for-each) 🔴

### What and why
📝 "For each item in the collection, do this." It's cleaner when you just want every element in order:
```java
int[] scores = {95, 88, 72};
for (int s : scores) {
    System.out.println(s);
}
```

### How it works and its limits
- The loop variable is a **copy** of each element. Assigning to it doesn't change the array (verified):
  ```java
  int[] arr = {1, 2, 3};
  for (int v : arr) { v = v * 10; }
  System.out.println(java.util.Arrays.toString(arr));   // [1, 2, 3]: unchanged
  ```
  For arrays of *objects*, the copy is a copy of the **reference** (M1.5). So you *can* change the object's fields (`learner.setXp(0)` affects the real learner), but you can't swap the element out.
- **No index.** You can't easily skip the first element, start in the middle, or know your position.
- **Forward only.** 📝 The notes said backward traversal "isn't natural" without explaining why. ➕ Here's the reason: for collections, for-each is compiled into code that uses an **Iterator**, an object whose only job is "give me the next element". It has no "previous". For arrays, it's compiled into an ordinary index loop going upward.

| Use for-each when… | Use an indexed `for` when… |
|---|---|
| You process every element, in order | You need the index (positions, parallel arrays) |
| You only read elements | You need to replace elements |
| | Custom start/end/step, backward, skipping |

---

## 3.5 Jump statements: `break`, `continue`, `return` 📁 🔴

| Statement | Effect |
|---|---|
| `break` | Exit the **innermost** loop (or switch) immediately |
| `continue` | Skip the rest of this iteration and go to the next one |
| `return` | Exit the **whole method**, optionally returning a value |

📁 From `JumpStatementsDemo`:
```java
int[] numbers = {1, 3, 7, 4, 9, 12};
for (int n : numbers) {
    if (n % 2 == 0) {
        System.out.println("Found even number: " + n);   // 4
        break;                                            // stop searching
    }
}

for (int i = 1; i <= 8; i++) {
    if (i % 2 != 0) continue;     // skip odd numbers
    System.out.println(i);        // 2 4 6 8
}

static String getDiscountLabel(int price) {
    if (price > 500) {
        return "Premium — 20% off!";   // early exit
    }
    return "Standard — no discount";
}
```
The last method is an **early return**. M12 shows how early returns replace deeply nested ifs (KISS).

### Labeled break and continue ➕ 🟡
`break` only exits the innermost loop. To exit an outer loop, label it (verified):
```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) continue outer;   // jump to the next i
        if (i == 2) break outer;      // leave both loops
        System.out.println(i + "," + j);
    }
}
// Output: 0,0
//         1,0
```
Labels are rare in clean code. Extracting a method and using `return` is usually clearer.

---

## 3.6 Nested loops 🟠
A loop inside a loop. The inner loop runs completely for **each** iteration of the outer loop, so the total work is outer × inner. That's why nested loops over large data get slow, which is a theme of your DSA track.
```java
for (int row = 1; row <= 3; row++) {
    for (int col = 1; col <= row; col++) {
        System.out.print("* ");
    }
    System.out.println();
}
// *
// * *
// * * *
```

---

## 3.7 Interview preparation — Module 3

1. **What is fall-through in `switch`? When is it useful, and when is it a bug?** Explain that cases are entry points, then give the grouped-cases example.
2. **What types can a `switch` use? Why not `double` or `boolean`?** (Exact matching on floating-point values is unreliable; a boolean only has two values, so `if` covers it.)
3. **`while` vs `do-while`: give a real use for `do-while`.** (Generate-then-check uniqueness; show the menu at least once.)
4. **When can't you use a for-each loop?** (Index needed, modifying elements, backward or partial traversal.)
5. **`break` vs `continue` vs `return`.**
6. **Tricky:** what does `for (int i = 0; i < 3; i++);` followed by a print do? What does `if (flag = false)` do?
7. **Arrow switch vs classic switch.** Why does the arrow form reduce bugs?
8. **Coding:** print FizzBuzz for 1–30 (multiples of 3 → "Fizz", of 5 → "Buzz", of both → "FizzBuzz"). Interviewers watch your **condition order**.

---

## 3.8 Check yourself — Module 3

**Q1 (predict).**
```java
int x = 3;
switch (x) {
    case 1: System.out.print("one ");
    case 3: System.out.print("three ");
    case 4: System.out.print("four ");
    default: System.out.print("default ");
}
```

**Q2 (predict).**
```java
int count = 0;
for (int i = 0; i < 5; i++) {
    if (i == 1) continue;
    if (i == 4) break;
    count += i;
}
System.out.println(count);
```

**Q3 (find the bug).** The goal is to print the largest value. Find **two** bugs.
```java
int[] xp = {40, 90, 15, 70};
int max = 0;
for (int i = 0; i <= xp.length; i++) {
    if (xp[i] > max) max = xp[i];
}
System.out.println(max);
```
(Hint: one bug crashes; the other only shows up for certain inputs. Which inputs?)

**Q4 (choose).** For each case, which loop would you choose, and why?
- (a) Print every learner's name.
- (b) Retry a network call until it succeeds or 3 attempts have happened.
- (c) Print an array in reverse.
- (d) Keep showing a menu until the user picks "Exit".

### Progress tracker — Module 3
- **Understand:** ladder ordering; switch fall-through and allowed types; arrow switch; the three loops and when each fits; for-each's copy semantics and limits; break/continue/return; labeled loops.
- **Can code:** grading ladders; menu switches; counting, searching, summing loops; early returns; nested loops.
- **Interview topics:** fall-through; do-while use cases; for-each limits; FizzBuzz-style condition ordering.
- **Common mistakes:** ladder in the wrong order; missing `break`; `<=` with `.length`; stray `;` after `for`/`if`; missing braces; `if (flag = false)`.
- **Prerequisites for M4:** loops over arrays; `return`.

### Exercises — Module 3

**🟢 Beginner.** Print the multiplication table of a number (1–10). Then print only the even multiples using `continue`.

**🟡 Intermediate.** Given `int[] xp = {120, 45, 300, 45, 90};`:
- print the max, the min, and the average (as a double);
- print the index of the *first* value above 100, using `break`; and
- count how many values are below the average. Think about why this needs two passes over the data.
<details><summary>Hint 1</summary>Initialize max and min from the first element, not from 0. Why does 0 fail for min?</details>

**🔴 Interview-level.** Write a `do-while` menu that repeatedly reads a number (use a fixed array of "inputs" for now, since input comes in M4) and uses an arrow-style switch expression to map 1→"Add", 2→"List", 3→"Exit", anything else→"Invalid". Stop when "Exit" appears. Explain why `do-while` fits better than `while` here, and what would change if you used a classic switch without `break`.

---

# Module 4 — Methods, Arrays & Input 🔴

> **Prerequisites:** M1–M3.
> **You'll need this for:** M5 (instance methods, constructors), M6 (the call stack is the JVM stack), M9 (overloading), and everything else. **§4.2 pass-by-value is one of the most important ideas in the course.**

---

## 4.1 Methods 🔴

### What is it?
📁 A **method** is a named, reusable block of code that performs one task. You *define* it once and *call* it whenever you need it.

### Why does it exist?
📁 The repo's `MethodsDemo` lists three reasons. Methods:
- avoid repeating code, which is the seed of the DRY principle (M12);
- break a big problem into small, nameable pieces; and
- make code easier to read and test.

📝 Doc 4's Learner Manager advice, "split logic into small methods, each with one responsibility", is the seed of SRP (M12).

### Anatomy
```java
static int add(int a, int b) {    // modifiers  returnType  name(parameters)
    return a + b;                 // body; return sends a value back
}
```
| Part | Meaning |
|---|---|
| `static` | Belongs to the class, so it can be called without an object (full story in M5) |
| return type | The type of value sent back; `void` means nothing is returned |
| name | camelCase verb: `calculateAverage`, `isEligible` |
| parameters | Input variables, each with a type; they're local variables of the method |
| `return` | Ends the method immediately and hands back the value |

**Parameter vs argument:** `a` and `b` are *parameters* (in the definition). In `add(10, 25)`, the values `10` and `25` are *arguments* (in the call).

📁 Examples from `MethodsDemo`:
```java
static void greet(String name) {                  // void: does something, returns nothing
    System.out.println("Hello, " + name + "!");
}
static boolean isEligible(int age) {              // returns a boolean
    return age >= 18;
}
public static void main(String[] args) {
    greet("Alice");
    int sum = add(10, 25);                        // 35
    boolean ok = isEligible(20);                  // true
}
```

### Rules worth knowing ➕
- A non-`void` method must `return` a value **on every path**, or it won't compile.
- **The method signature is the name plus the parameter types.** The return type is *not* part of it. Two methods that differ only in return type are illegal (verified: `method f(int) is already defined`). This becomes **overloading** in M9.
- **Why are the methods `static` for now?** `main` is static, and a static method can directly call only other static methods of its class. Calling instance methods requires an object (M5).

---

## 4.2 Pass-by-value 🔴 ➕ (the most important interview topic of Phase A)

### What is it?
**Java always passes a *copy* of each argument's value into the method.** No exceptions.

- For primitives, the value is the number itself.
- For references, **the value is the reference (the address)**.

So "Java passes objects by reference" is **wrong**. Java passes *references by value*.

### See it (all verified)
```java
static void swap(int a, int b) { int t = a; a = b; b = t; }
int p = 1, q = 2;
swap(p, q);
System.out.println(p + " " + q);      // 1 2: only the copies were swapped
```
📁 The repo's `TypeCastingDemo` has the same idea: `test(rounded)` does `a++` inside the method, and `rounded` is unchanged afterwards.

```java
static void zero(int[] arr)    { arr[0] = 0; }                          // uses the remote to change the TV
static void replace(int[] arr) { arr = new int[]{7, 7}; arr[0] = -1; }  // re-pairs the COPY of the remote

int[] data = {5, 6};
zero(data);
replace(data);
System.out.println(java.util.Arrays.toString(data));   // [0, 6]
```

### How it works (the remote analogy)
Calling a method hands it a **photocopy of your remote**.
- With the copy, it can press buttons: change the array's contents or an object's fields. You'll see that change, because it's the same TV.
- If it **re-pairs its copy** to a new TV (`arr = new int[]{...}`), your remote is unaffected.

```
before replace():   data ──► [0 | 6]   ◄── arr (copy)
inside replace():   data ──► [0 | 6]        arr ──► [-1 | 7]   (new array; data never sees it)
```

### Why Java chose this
It's simple and predictable: a method can **never** make your variable point somewhere else. It can only change the shared object your variable points to. M7 (encapsulation) is about controlling *that* sharing.

---

## 4.3 The call stack 🟠
Each method call gets its own **stack frame**: a workspace holding its parameters and local variables. When a method returns, its frame is removed, and execution resumes in the caller.

📝 Doc 4: the stack "remembers which method called which, and where to resume".
```
main() calls add(10, 25)
┌─────────────────┐
│ add: a=10 b=25  │  ← top (running now)
├─────────────────┤
│ main: sum=?     │  ← paused, resumes when add returns
└─────────────────┘
```
That's why locals vanish when a method ends, and why each call gets fresh copies of its parameters.

A method calling itself is **recursion**. Without a stopping condition (a *base case*), frames pile up until the stack runs out of space, giving `StackOverflowError`. More on the stack's place in JVM memory in M6.

---

## 4.4 Arrays in depth 🔴

### What and why
An **array** is a fixed-size, ordered container of elements **of one type**, accessed by index from `0` to `length - 1`. Arrays exist because programs handle many values of the same kind, such as 10 learners' XP. You can't create `xp1, xp2, … xp10` variables for that. 📝 Doc 8 makes the same point about `learner1, learner2, …`.

### Syntax
```java
int[] xp = {120, 45, 300};          // create with initial values
int[] marks = new int[5];           // create with defaults: [0, 0, 0, 0, 0]
String[] names = new String[10];    // all null
xp[1] = 50;                         // write
int first = xp[0];                  // read
int n = xp.length;                  // 3 (a field, not a method)
```

### How it works internally
- **An array is an object.** It lives on the heap, and the variable holds a reference to it (M1.5). Passing it to a method passes the reference by value (§4.2).
- **Its size is fixed at creation.** To "grow" an array, you create a bigger one and copy the elements over. That's exactly what `ArrayList` does for you (M11).
- **Every access is bounds-checked.** A bad index is a runtime error (verified):
  ```
  java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
  ```
  This protects memory. In C, a bad index silently reads or writes someone else's data.

### Standard patterns (memorize these shapes)
```java
int sum = 0;
for (int v : xp) sum += v;                      // sum
double avg = (double) sum / xp.length;          // average (cast first! See M2)

int max = xp[0];                                // max: start from the first element, not 0
for (int i = 1; i < xp.length; i++) if (xp[i] > max) max = xp[i];

int idx = -1;                                   // linear search: -1 means "not found"
for (int i = 0; i < xp.length; i++) if (xp[i] == 300) { idx = i; break; }
```

### Utilities ➕
```java
import java.util.Arrays;
Arrays.toString(xp);    // "[120, 50, 300]" (readable printing)
Arrays.sort(xp);        // sorts in place
```

### 2D arrays ➕ 🟡
A 2D array is an array of arrays (verified):
```java
int[][] grid = new int[2][3];   // 2 rows, 3 columns
grid[1][2] = 9;
System.out.println(Arrays.deepToString(grid));   // [[0, 0, 0], [0, 0, 9]]
System.out.println(grid.length + " " + grid[0].length);   // 2 3
```

### Parallel arrays (📁 the repo's hint) and their weakness
The Learner Manager exercise stores learners in three arrays: `String[] names`, `int[] ages`, `int[] xpValues`. Learner `i` is spread across the same index in all three.

This works, but it's fragile:
- sorting one array breaks the alignment;
- adding a field means adding another array; and
- nothing in the code says the three arrays belong together.

📝 Doc 4: "a more natural design would be a `Learner` object and a list of learners". That's exactly what M5 and M11 give you.

---

## 4.5 Reading input with `Scanner` ➕ 🔴

### What and how
```java
import java.util.Scanner;                 // Scanner lives in java.util (imports: M7)

Scanner sc = new Scanner(System.in);      // System.in = keyboard input
int age = sc.nextInt();                   // reads one int
double h = sc.nextDouble();               // reads one double
String word = sc.next();                  // reads one word (stops at a space)
String line = sc.nextLine();              // reads the rest of the current line
```

### The famous trap (verified)
```java
System.out.print("Age: ");
int age = sc.nextInt();          // user types 25 and presses Enter → input is "25\n"
System.out.print("Name: ");
String name = sc.nextLine();     // returns "" immediately!
```
Verified output: `Name: [] age 25`. The name is empty.

**Why:** `nextInt()` reads `25` but leaves the newline `\n` in the input. `nextLine()` then reads "everything up to the next newline", which is an empty string.

**Fix:** after reading a number, consume the leftover newline with an extra `sc.nextLine();`. Alternatively, read every line with `nextLine()` and convert with `Integer.parseInt(...)`.

### Bad input
If the user types `abc` when `nextInt()` expects a number, you get `InputMismatchException`. 📁 The class 2 version of the exercise asks you to catch it (exception handling is M10). ➕ You can also check first with `sc.hasNextInt()`.

---

## 4.6 Capstone — Airtribe Learner Manager (console) 📁 🔴

This is the class 1/class 2 exercise. It combines everything from Modules 1–4.

**Requirements** (from `PROBLEM_STATEMENT.md`):
```
===== Airtribe Learner Manager =====
1. Add Learner
2. Display All Learners
3. Calculate Average XP
4. Exit
Select: _
```
- Each learner has a name (String), an age (int), and XP (int).
- **Add:**
  - read name, age, and XP;
  - validate that age is between **18 and 100**;
  - if invalid, print an error and **do not add** (📝 "never store bad data halfway"); and
  - refuse to add if capacity (10) is full.
- **Display:** print all learners in a formatted list, or a friendly message if there are none.
- **Average:** print the average XP with 2 decimals, or a friendly message if there are none. Never divide by zero.
- **Exit:** print "Goodbye!" and stop.
- **Constraints:** a single class; arrays of max size 10; `Scanner`; `switch` for the menu; `if-else` for validation; a loop that keeps the menu running.
- **Class 2 addition:** handle non-numeric input without crashing. Come back to this after M10.

**Design guidance.** Split the work into small static methods, for example `printMenu()`, `addLearner(...)`, `displayLearners()`, `averageXp()`. The arrays and the `count` variable need to be visible to all of them. Think about what that implies (look up `static` fields in M5.2, or pass them as parameters).

<details><summary>Hint 1: structure</summary>A loop around: print menu → read choice → switch(choice). A boolean <code>running</code> flag, or a do-while, decides when to stop.</details>
<details><summary>Hint 2: storage</summary>Three parallel arrays plus <code>int count</code>. Adding writes at index <code>count</code>, then increments it. Display loops from 0 to <code>count - 1</code>, NOT to <code>names.length</code>. Why?</details>
<details><summary>Hint 3: the bug you will probably hit</summary>If the name comes out empty after you choose option 1, re-read §4.5.</details>
<details><summary>Hint 4: formatting</summary><code>String.format("%.2f", avg)</code> gives two decimals.</details>

When you're done, compare with `class1/s10_exercise/LearnerManagementSystem.java` and `class2/s11_exercise/LearnerManagementSystem.java` in the repo, or send me your code for review.

---

## 4.7 Interview preparation — Module 4

1. **Is Java pass-by-value or pass-by-reference?** 🔴 *(Asked constantly.)*
   *Strong answer:* "Always pass-by-value. For objects, the value copied is the reference. So a method can modify the object through its copy of the reference, and the caller sees that. But if the method reassigns its parameter to a new object, the caller's variable is unaffected." Then give the `zero` vs `replace` example. Saying "objects are passed by reference" is the common wrong answer.
2. **Why doesn't a `swap(int a, int b)` method work in Java?**
3. **Is the return type part of the method signature?** (No.)
4. **What happens when you access `arr[arr.length]`?** (Runtime `ArrayIndexOutOfBoundsException`, and why bounds checking exists.)
5. **Why are arrays fixed size? How does an `ArrayList` grow?** (Create bigger, copy, replace.)
6. **The `nextInt()` + `nextLine()` problem.** Explain the cause and the fix.
7. **What causes `StackOverflowError`?**
8. **Coding:** write `static int[] reverse(int[] a)` returning a *new* reversed array. Then write `static void reverseInPlace(int[] a)`. Explain why the second can be `void`.

---

## 4.8 Check yourself — Module 4

**Q1 (predict).**
```java
static void bump(int n, int[] arr) {
    n++;
    arr[0]++;
    arr = new int[]{100};
    arr[0]++;
}
// in main:
int n = 1;
int[] arr = {1};
bump(n, arr);
System.out.println(n + " " + arr[0]);
```

**Q2 (explain).** A teammate says: "My method `resetUser(User u) { u = new User(); }` doesn't reset the caller's user. Java must be broken." Explain what's happening, and write a version that *does* reset the user's fields.

**Q3 (find the bug).**
```java
static double average(int[] values) {
    int sum = 0;
    for (int i = 0; i < values.length; i++) sum += values[i];
    return sum / values.length;
}
```
There are two problems: one gives wrong answers, and one crashes for a certain input. What are they?

**Q4 (design).** Why do the instructor's notes say the parallel-array design is "not the natural design"? Name two concrete bugs it invites.

### Progress tracker — Module 4
- **Understand:** method anatomy; signature vs return type; **pass-by-value**; stack frames; arrays as fixed-size heap objects; bounds checking; Scanner and its newline trap.
- **Can code:** reusable static methods; sum/avg/max/search over arrays; menu-driven console programs with validation.
- **Interview topics:** pass-by-value (a must); swap; array bounds; growing arrays; StackOverflowError.
- **Common mistakes:** looping to `.length` instead of `count`; integer average; `nextInt`/`nextLine`; expecting reassignment inside a method to affect the caller.
- **Prerequisites for M5:** methods, references, `static` (seen but not yet explained).

### Exercises — Module 4

**🟢 Beginner.** Write `static boolean isPrime(int n)` and use it to print all primes below 50.

**🟡 Intermediate.** Write `static int countAbove(int[] values, int threshold)` and `static int[] filterAbove(int[] values, int threshold)`. The second must return an array of *exactly* the right size.
<details><summary>Hint 1</summary>You can't know the size up front. What if you called <code>countAbove</code> first?</details>

**🔴 Interview-level.** The Learner Manager capstone (§4.6), with all validation, then this extension: "Display learners sorted by XP, highest first, **without changing** the stored order." Explain why sorting the `xpValues` array alone would corrupt your data, and how you avoided that.
<details><summary>Hint 1</summary>Think about sorting <em>indexes</em> instead of values, or sorting copies of all three arrays in lockstep.</details>

---

# Module 5 — Classes & Objects 🔴

> **Prerequisites:** M1 (references), M4 (methods, pass-by-value).
> **You'll need this for:** literally everything after this. M6 explains where objects live; M7–M12 are all about designing classes well.

---

## 5.1 Class vs object 🔴

### What is it?
📁 (class 1, `s9_oop_intro`)
- A **class** is a **blueprint**. It describes what data a thing has (**state**, stored in *fields*) and what it can do (**behavior**, provided by *methods*).
- An **object** is one concrete thing built from that blueprint, also called an **instance**.

```java
class Car {
    String color;       // state (fields)
    int mileage;
    void start() { System.out.println(color + " car started"); }   // behavior
}

Car c1 = new Car();     // object 1
Car c2 = new Car();     // object 2: separate state
c1.color = "red";
c2.color = "blue";
c1.start();             // red car started
```
📁 The instructor's whiteboard sketch in `HelloWorld.java` models a `Movie` (title, duration, producer, director, genres) that refers to `Person` objects (name, age). Notice that a `Movie` *holds references* to `Person` objects. Objects pointing to other objects is how all real systems are modeled.

### Why does it exist?
Before object-oriented programming, data and the functions operating on it lived separately. You saw this with the parallel arrays in M4: nothing tied a learner's name to their age.

A class **bundles related data with the code that operates on it**. That makes the program mirror the problem domain (`Learner`, `Order`, `Payment`), and it makes protection possible (encapsulation, M7).

### What `new` actually does ➕ 🔴
`Car c1 = new Car();` happens in four steps:
1. **Allocates** memory for a `Car` object on the **heap** (M6).
2. **Zeroes** every field to its default (`0`, `false`, `null`). That's why fields get defaults (M1.6).
3. **Runs the constructor** (§5.3) to set up the object.
4. **Returns a reference** to the object, which is stored in the local variable `c1` (which lives on the **stack**).

```
STACK (main's frame)          HEAP
 c1 ─────────────────────►  Car { color="red",  mileage=0 }
 c2 ─────────────────────►  Car { color="blue", mileage=0 }
```

---

## 5.2 The three kinds of variables: local, instance, static 🔴

📁 Repo `VariableScopeDemo`:
```java
public class VariableScopeDemo {
    static int totalStudents = 0;    // STATIC: one copy, shared by the whole class
    String studentName;              // INSTANCE: each object gets its own copy
    int xp;

    public VariableScopeDemo(String name, int xp) {
        this.studentName = name;
        this.xp = xp;
        totalStudents++;             // the shared counter goes up for every object created
    }

    void printInfo() {
        String label = "Student";    // LOCAL: exists only during this method call
        System.out.println(label + ": " + studentName + " | XP: " + xp);
    }
}
```

| | Local | Instance | Static |
|---|---|---|---|
| Declared | Inside a method or block | In the class, no `static` | In the class, with `static` |
| How many copies | One per method *call* | One per **object** | **One for the whole class** |
| Accessed via | Its name, inside the block | `object.field` | `ClassName.field` |
| Default value | None (must assign) | Yes | Yes |
| Lives | While the method runs | As long as the object is reachable | As long as the class is loaded |
| Memory (M6) | Stack | Heap (inside the object) | Class-level storage |

### Why static exists
Some data belongs to the *concept*, not to any one object:
- a count of all students created,
- a shared constant like `TAX_RATE`, or
- a single shared resource (M13).

Storing it per object would duplicate it, and the copies could disagree.

### Trap (📁 the repo does this on purpose)
```java
s2.totalStudents = 10;   // compiles! But it changes THE ONE shared variable
System.out.println(VariableScopeDemo.totalStudents);   // 10, for everyone
```
Accessing a static through an object reference is legal but misleading: it *looks* per-object. **Always use the class name** for statics.

---

## 5.3 Constructors 🔴

### What is it?
📝 Doc 3: a **constructor** is a special block that **initializes a new object**. It:
- has **exactly the class's name**;
- has **no return type, not even `void`**; and
- runs automatically when `new` is used.

If you add a return type, it becomes an ordinary method that happens to share the class's name. That compiles, but it's a trap.

### Why does it exist?
To guarantee an object starts in a **valid state**. Without constructors, every caller would have to remember to set every required field, and some wouldn't.

📝 **Constructors are for initialization; methods are for behavior.** "Adding two numbers belongs in a method, not a constructor."

### The four kinds (📝 Doc 3, 📁 `ConstructorDemo`)

**1. Default constructor (generated by Java).** If you write **no constructor at all**, the compiler adds an invisible no-argument constructor that does nothing beyond the field defaults.

> 🔴 **If you write any constructor yourself, the default one is NOT generated.** Verified:
> ```java
> class Box { int w; Box(int w) { this.w = w; } }
> Box b = new Box();   // error: constructor Box in class Box cannot be applied to given types
> ```

**2. No-argument constructor (written by you).** It sets sensible preset values:
```java
Learner() {
    name = "Unknown";
    xp = 0;
}
```

**3. Parameterized constructor.** It takes values from the caller:
```java
Learner(String name, int xp) {
    this.name = name;
    this.xp = xp;
}
```
You can have several, as long as their parameter lists differ. That's **constructor overloading** (M9). 📝 This lets callers build objects "depending on how much data they already have".

**4. Copy constructor.** It builds a new object from an existing one:
```java
Learner(Learner other) {
    this.name = other.name;
    this.xp = other.xp;
}
```
📝 Java has no special "copy constructor" feature. It's just a constructor whose parameter happens to be the same class. Parameters also don't have to mirror fields one-to-one. They're simply inputs to initialization logic.

### Chaining constructors with `this(...)` 📁
Reuse one constructor from another instead of duplicating code (DRY):
```java
Learner(String name, int xp, int abc, Car c) {
    this(name, xp);      // must be the FIRST statement
    this.abc = abc;
    this.car = c;
}
```

### Shallow vs deep copy ➕ 🔴
📁 The repo's `ConstructorDemo` copy constructor has its body **commented out**, so as written it copies *nothing*: `l3.name` would be `null`. The instructor's comments sketch the intended version, including `this.car = other.car;`.

Here's what happens when you do copy like that (verified):
```java
Car c = new Car();                     // doors = 4
Learner a = new Learner("Alice", 120, c);
Learner b = new Learner(a);            // copy constructor: this.car = other.car
b.name = "Bob";
b.car.doors = 2;
System.out.println(a.name + " " + a.car.doors);   // Alice 2   ← a's car changed too!
```
```
a ──► Learner{name ─► "Alice", car ─┐}
                                     ├──► Car{doors=2}     ← ONE car, shared
b ──► Learner{name ─► "Bob",   car ─┘}
```
- A **shallow copy** copies the field values. For reference fields, that means it copies the *references*, so the nested objects are **shared**.
- A **deep copy** also creates new copies of the nested objects: `this.car = new Car(other.car);`.

Changing `b.name` didn't affect `a`. Why not? `b.name = "Bob"` re-pointed `b`'s field to a different String. It didn't modify the String `"Alice"` (Strings can't be modified at all; §5.7).

### Reference assignment is NOT copying 📝 🔴
```java
Book b2 = b1;   // NO new object. Two references, one Book.
```
Verified with learners: after `Learner alias = a; alias.xp = 0;`, `a.xp` is `0`, and `alias == a` is `true`. Only `new` creates objects.

---

## 5.4 `this` — the current object 🔴

### What is it?
📝 Doc 7: `this` is a reference to **the object whose method or constructor is currently running**. Every instance method secretly receives it.

### Why it's needed: shadowing
When a parameter has the same name as a field, the parameter **wins** inside the method (M1.6's name lookup: closest scope first).
```java
Student(String name, int age) {
    name = name;      // assigns the parameter to itself. The field stays null!
    age = age;
}
Student(String name, int age) {
    this.name = name; // this.name = the field; name = the parameter
    this.age = age;
}
```
If the names differ (`name1`), `this` is optional. The convention of matching names plus `this.` is universal, though.

📁 **A real bug in the repo** (`class4/b_protected_access/child/Dog.java`):
```java
public Dog(int a, String b, int c, String d) {
    super(a, b, c, "Dog");
    this.breed = breed;     // there's no parameter called breed, so this assigns the FIELD to itself → stays null
}
```
The intended line was `this.breed = d;`. This is exactly the class of bug this section is about.

### Other uses
- `this.display()`: call a method on the current object (usually optional).
- `this(...)`: constructor chaining (§5.3). It must be the first statement.
- Pass the current object somewhere: `registry.add(this);`.

### Where `this` cannot exist
**In a static context.** Verified:
```
error: non-static variable this cannot be referenced from a static context
```
A static method runs without any object, so there's no "current object". 📝 "No object means no `this`."

---

## 5.5 Instance methods vs static methods 🔴

| | Instance method | Static method |
|---|---|---|
| Belongs to | Each object | The class |
| Called via | `object.method()` | `ClassName.method()` |
| Has `this`? | Yes | **No** |
| Can use instance fields directly? | Yes | **No**; it needs an object reference |
| Typical use | Behavior of an object (`account.withdraw(500)`) | Utilities that need no object state (`Math.max`, `Integer.parseInt`) |

Verified error when a static method touches an instance field:
```
error: non-static variable count cannot be referenced from a static context
```
**Why:** if there are 1,000 objects, *whose* `count` would it be? The static method has no object to ask. 📝 "If there is no object, there is no instance state to read from." If you create an object inside the static method, you can use *that object's* fields: `ABC abc = new ABC(); abc.count++;`.

**This explains `main`.** The JVM calls `main` without creating your object (because it's static). That's why every method you called from `main` in M4 had to be static, until now.

---

## 5.6 References, `==`, `null`, and NullPointerException 🔴

- `a == b` on objects asks: **"are these the same object?"** That's address comparison.
- To compare *contents*, use `.equals()`, as long as the class defines what "equal" means (§5.9).
- A reference can be `null`. Calling a method on it throws **`NullPointerException`** (NPE) at runtime. This is the most common Java exception in production.
- Pass-by-value with objects (M4.2) works the same way for your own classes (verified):
  ```java
  static void move(Point p) { p.x = 99; p = new Point(-1); p.x = -5; }
  Point p = new Point(1);
  move(p);
  System.out.println(p.x);   // 99
  ```

**When objects die.** When no reference points to an object anymore, it becomes *unreachable*. The garbage collector may later reclaim its memory (M6.7).

---

## 5.7 Strings, properly ➕ 🔴

### Strings are immutable
Once created, a `String` object **can never change**. Methods that seem to modify it actually return a **new** String (verified):
```java
String s = "java";
s.toUpperCase();          // creates "JAVA", which is immediately thrown away
System.out.println(s);    // java
s = s.toUpperCase();      // re-point s to the new String
System.out.println(s);    // JAVA
```

### The string pool, and `==` vs `.equals()` (verified)
String **literals** are stored once in a shared **string pool** and reused.
```java
String a1 = "hi";
String a2 = "hi";               // same pooled object as a1
String a3 = new String("hi");   // new forces a separate object

a1 == a2          // true  (same object)
a1 == a3          // false (different objects)
a1.equals(a3)     // true  (same characters)
```
**Always use `.equals()` for Strings.** Code that uses `==` can appear to work in tests (where you use literals) and then fail in production, where the strings come from user input or a database.

### Why immutable?
- **Pooling is only safe if nobody can modify a shared string.**
- **Security:** a file path or URL can't be changed after it has been validated.
- **Thread safety** (M13).
- The hash code can be cached, which makes Strings fast as `HashMap` keys (M11).

### Building strings in loops
Each `+` creates a new String. In a loop, use `StringBuilder`, a mutable buffer:
```java
StringBuilder sb = new StringBuilder();
for (String name : names) sb.append(name).append(", ");
String result = sb.toString();
```
📁 The Food Court `BillFormatter` solution does exactly this.

---

## 5.8 Wrapper classes and autoboxing ➕ 🟠

Each primitive has an object counterpart:

| Primitive | Wrapper |
|---|---|
| `int` | `Integer` |
| `long` | `Long` |
| `double` | `Double` |
| `boolean` | `Boolean` |
| `char` | `Character` |
| … | … |

**Why they exist:** some parts of Java accept only objects. Most importantly, collections: `List<Integer>`, never `List<int>` (M11). Wrappers can also represent "no value" (`null`).

**Autoboxing / unboxing:** Java converts between the two automatically:
```java
Integer boxed = 4;   // autoboxing: int → Integer
int raw = boxed;     // unboxing: Integer → int
```

**Two traps (verified):**
```java
Integer i1 = 127, i2 = 127, i3 = 128, i4 = 128;
System.out.println(i1 == i2);        // true  ← values -128..127 are cached and shared
System.out.println(i3 == i4);        // false ← separate objects
System.out.println(i3.equals(i4));   // true

Integer nothing = null;
int z = nothing;                     // NullPointerException during unboxing
```
The same rule applies: **compare wrapper objects with `.equals()`.**

Useful helpers: `Integer.parseInt("42")`, `Integer.MAX_VALUE`, `Double.parseDouble("3.5")`.

---

## 5.9 `Object`, `toString()`, `equals()` ➕ 🟠

Every class automatically inherits from `java.lang.Object` (M8 explains how). `Object` provides methods every object has:

- **`toString()`** returns a text description. The default is `ClassName@hexHash`. That's why printing an object (or an array, in M1) shows gibberish (verified: `V5$Point@…`). Override it to print something useful:
  ```java
  @Override
  public String toString() { return "Point(" + x + ")"; }
  ```
  `System.out.println(obj)` and `"text" + obj` call `toString()` automatically.

- **`equals(Object o)`**: by default it's the same as `==`, an identity check. `String`, `Integer` and others override it to compare content. Override it in your own classes when "same content" should count as equal.

- **`hashCode()`**: if you override `equals`, you **must** override `hashCode` consistently, or `HashMap` and `HashSet` break (M11). The full contract: equal objects must have equal hash codes.

---

## 5.10 OOP in one picture 📁

The repo introduces OOP's **four pillars**. Here's where each is taught:

| Pillar | One-line meaning | Module |
|---|---|---|
| **Encapsulation** | Bundle data with methods; hide internals; control access | M7 |
| **Inheritance** | Reuse and extend another class's members (`extends`) | M8 |
| **Polymorphism** | One call, different behavior depending on the actual object | M9 |
| **Abstraction** | Expose *what* something does, hide *how* (abstract classes, interfaces) | M11 |

---

## 5.11 Interview preparation — Module 5

1. **Class vs object?** Blueprint vs instance. Strong candidates add what `new` does: allocate, zero, construct, return a reference.
2. **What happens if you define a parameterized constructor and then call `new X()`?** (Compile error: the default constructor disappears.)
3. **Constructor vs method?** (Name, no return type, called by `new`, initialization vs behavior; constructors aren't inherited, see M8.)
4. **Shallow vs deep copy?** 🔴 Give the shared-`Car` example, then explain how to make it deep.
5. **What is `this`? Why can't you use it in a static method?**
6. **Static vs instance variables and methods.** Why can't a static method access instance fields? Why is `main` static?
7. **`==` vs `.equals()`?** 🔴 Strings and wrappers; the pool; why `==` can "work" in tests and fail in production.
8. **Why are Strings immutable?** Give at least three reasons.
9. **Tricky:** `Integer a = 128, b = 128; a == b?`; `Integer x = null; int y = x;`.
10. **What does `Book b2 = b1;` do?** (Copies the reference; no new object.)
11. **Coding:** write a `Counter` class whose objects count their own `increment()` calls, while a static field counts calls across *all* counters.

---

## 5.12 Check yourself — Module 5

**Q1 (predict).**
```java
class Learner {
    static int total = 0;
    String name;
    Learner(String name) { name = name; total++; }
}
Learner a = new Learner("Asha");
Learner b = new Learner("Bilal");
Learner c = b;
c.name = "Chen";
System.out.println(a.name + " " + b.name + " " + Learner.total);
```

**Q2 (find the bugs).** Find three.
```java
class Account {
    String owner; double balance;
    Account(String owner) { this.owner = owner; }
    static void deposit(double amount) { this.balance += amount; }
}
Account acc = new Account();
```

**Q3 (explain).** A teammate writes `if (user.getRole() == "ADMIN")`. It passes every unit test but fails in production for some admins. Explain exactly why, and fix it.

**Q4 (design).** A `Course` has a title and a list of `Lesson` objects. Write a copy constructor for `Course` that makes a copy in which editing a lesson's title does **not** affect the original course. What kind of copy is that?

### Progress tracker — Module 5
- **Understand:** class vs object; what `new` does; local, instance and static variables; the 4 constructor kinds and the default-constructor rule; `this`; static vs instance methods; references and `==`; shallow vs deep copy; String immutability and the pool; wrappers; `Object` methods.
- **Can code:** classes with fields, constructors (including chaining), instance and static methods, `toString`; copy constructors (shallow and deep).
- **Interview topics:** constructors; `this`; static; `==` vs `equals`; String immutability; shallow vs deep copy; Integer caching.
- **Common mistakes:** `name = name`; losing the default constructor; statics accessed via objects; `==` on Strings and wrappers; thinking assignment copies objects.
- **Prerequisites for M6:** the stack vs heap picture from §5.1; static vs instance.

### Exercises — Module 5

**🟢 Beginner.** Write a `Book` class (title, author, price) with:
- a no-arg constructor,
- a parameterized constructor,
- a copy constructor,
- `toString()`, and
- a static counter of books created.

Create three books, two of them via copying, and print everything.

**🟡 Intermediate.** Rewrite the M4 Learner Manager using a `Learner` class and a `Learner[]` array instead of parallel arrays. Which bugs from M4 Q4 disappear? Which problem remains? (Hint: the array is still fixed-size; M11 fixes that.)

**🔴 Interview-level.** Implement `class Matrix` holding an `int[][]`, with:
- a constructor that takes the array,
- a copy constructor,
- `set(r, c, v)`, and
- `toString()`.

Demonstrate with code that a naive copy constructor shares data, then fix it. Also make the *original* constructor safe: if the caller later modifies the array they passed in, the `Matrix` must not change. This is called a **defensive copy**, and it previews encapsulation (M7).
<details><summary>Hint 1</summary>A 2D array is an array of arrays. Copying the outer array still shares the inner rows.</details>

---

# Module 6 — Inside the JVM 🟠 (parts 🔴)

> **Prerequisites:** M1 (bytecode), M4 (call stack), M5 (objects, static vs instance, `new`).
> **You'll need this for:** understanding performance and memory problems in real servers; interview rounds for backend roles; M8 (initialization order); M13 (class initialization makes the eager singleton thread-safe).

📝 Your notes focus on "vanilla Java", meaning the JVM itself, not frameworks. Priorities in this module:
- 🔴 stack vs heap, GC basics, static initialization
- 🟠 class loading and linking, JIT
- 🟡 tools

---

## 6.1 The big picture 📁

```
┌───────────────────────── JVM ─────────────────────────┐
│  Class Loader Subsystem:  Loading → Linking → Initialization │
│                         │                              │
│  Runtime Data Areas:  Method Area | Heap | Stacks | PC | Native stacks │
│                         │                              │
│  Execution Engine:    Interpreter | JIT Compiler | Garbage Collector │
│                         │                              │
│  Native Interface (JNI) — calls into C/C++ libraries   │
└───────────────────────────────────────────────────────┘
```

The journey of your program:
1. `javac` produces `.class` files.
2. The **class loader** reads them into memory.
3. **Linking** checks and prepares them.
4. **Initialization** runs static setup.
5. The **execution engine** runs the bytecode (interpreting it, JIT-compiling hot code, and collecting garbage), using the **runtime data areas** for memory.

📝 "The `.java` file itself is never executed. The JVM executes bytecode."

---

## 6.2 Bytecode 📁 🟡
Bytecode is a compact instruction set for the JVM. You can read it with the disassembler:
```bash
javap -c HelloWorld.class      # shows instructions like getstatic, ldc, invokevirtual, return
javap -verbose HelloWorld.class
```
Every valid `.class` file starts with the "magic number" `0xCAFEBABE`, which the verifier checks (§6.4).

---

## 6.3 Class loading 🟠

### What is it?
**Class loading** means finding a class's `.class` data and bringing it into the JVM's memory, so the class can be used.

### Loading is lazy (on demand)
📝 "Java does not load every class on the machine; it loads only what's needed."

Verified with `java -verbose:class`: a program's main class loads first, and `school.math.Teacher` loads only when the code actually reaches a line that uses it. A class you never use is never loaded.

### The three built-in loaders 📝📁

| Loader | Loads | Notes |
|---|---|---|
| **Bootstrap** | Core Java (`java.lang.String`, `java.util.List`, …) from the `java.base` module | Written in native code; `String.class.getClassLoader()` returns **`null`** (verified) |
| **Platform** (was **Extension** before Java 9) | Other JDK modules (e.g. `java.sql`) | Verified: `java.sql.Connection`'s loader is `PlatformClassLoader` |
| **Application** (System) | **Your classes and every library on your classpath** | Parent is the Platform loader (verified) |

➕ **Correction 1 (Doc 3):** third-party libraries (a caching library, a JSON library) are on your **classpath**, so the **Application** loader loads them, not the Extension loader.

### Parent delegation ➕ 🔴 (Correction 2, Doc 4)
📝 Doc 4 says the application loader "tries first" and delegates upward if it fails. **That's backwards.** The real rule, correctly stated in the 📁 repo notes:

1. When asked for a class, a loader **first asks its parent**.
2. The request travels all the way up to Bootstrap.
3. Bootstrap tries. If it can't find the class, control comes back **down**: Platform tries, then Application tries.
4. If nobody finds it, you get `ClassNotFoundException`.

```
Application ──asks──► Platform ──asks──► Bootstrap
     ▲  tries last        ▲ tries 2nd         │ tries FIRST
     └──────────────◄─────┴─────────◄─────────┘
```

**Why it's designed this way (security and consistency):** suppose you or a malicious library ship a class named `java.lang.String`. Bootstrap is asked first and always returns the *real* `String`, so core classes can't be replaced. It also guarantees each class is loaded once, by one loader.

### Dependencies
📝 Loading follows dependencies. If your class uses a library that uses core classes, all of them must be resolvable. The notes picture it as a dependency tree, with core classes at the bottom.

➕ 🟡 **Two errors often confused in interviews:**
- `ClassNotFoundException`: you asked for a class *by name* at runtime (for example `Class.forName("x.Y")`), and no loader found it.
- `NoClassDefFoundError`: the class existed at compile time but is missing (or failed to initialize) at runtime. This is often a missing JAR.

---

## 6.4 Linking: verification, preparation, resolution 🟠

After loading, a class is **linked** in three steps.

**1. Verification.** The bytecode verifier checks that the class is well-formed and safe:
- correct format (magic number),
- type-safe instructions, and
- no illegal stack operations.

📝 "Code can be buggy or malicious; Java blocks unsafe bytecode before it runs." This is part of why Java is considered secure.

**2. Preparation.** Memory is reserved for **static fields**, which are set to their **default values**, not the values you wrote:
```java
static int count = 10;
// after Preparation:   count == 0
// after Initialization: count == 10   (§6.5)
```
📝 "Preparation means reserving memory, not final initialization."

**3. Resolution.** Symbolic references in the bytecode (names like `"java/lang/System"`) are replaced by direct references (actual memory locations), so later uses don't need a name lookup. 📝 It's "an optimization to avoid repeated lookups".

➕ JVMs usually do resolution **lazily**: each symbolic reference is resolved the first time it's actually used, not all at once.

---

## 6.5 Initialization and static blocks 🔴

### What is it?
The **initialization** phase:
1. assigns static fields their **written values**, and
2. runs **static blocks**,

all **in textual order, top to bottom, exactly once** per class.

```java
class Demo {
    static int i = 5;
    static {                          // a static block (static initializer)
        System.out.println("hello");
    }
}
```

📁 The repo's example (verified output):
```java
static class Ex {
    static int x = 10;
    static { System.out.println("Static block 1, x=" + x); x = 20; }
    static int y = x + 5;
    static { System.out.println("Static block 2, y=" + y); }
}
System.out.println(Ex.y);
// Static block 1, x=10
// Static block 2, y=25
// 25
```

### Rules 📝📁
- It runs **once**, the first time the class is initialized. Creating 1,000 objects runs it once, not 1,000 times.
- **It can't use instance fields or `this`.** No object exists yet. 📝 "Static initialization happens before objects exist."
- A static block is **not a method**. It has no name and can't be called. A static *method* is called explicitly, as often as you like.
- **The parent class is initialized before the child class** (M8).

### When does initialization happen? 📁
The first time any of these happens:
- `new` creates an instance;
- a static method is called, or a static field is read or written (➕ except compile-time constants like `static final int X = 5`, which the compiler inlines);
- `Class.forName("...")` is called; or
- a subclass is initialized (the parent goes first).

### Real use: one-time startup work 📝
Doc 4 gives the reasoning: work like reading configuration files, loading credentials, or preparing database connection details should happen **once, at startup**. It shouldn't happen while a user waits for a login request. Static initialization is one place to do it. (In modern frameworks, configuration and dependency injection usually take over this role.)

➕ The JVM runs initialization **under a lock**, so two threads can't initialize the same class twice. That one fact is what makes the *eager singleton* thread-safe (M13).

---

## 6.6 Runtime data areas: where everything lives 🔴

📁 The JVM has five memory areas:

| Area | Per thread or shared | Holds | Error when exhausted |
|---|---|---|---|
| **Method area** | Shared | Class metadata (name, parent, method bytecode, constants) | `OutOfMemoryError` |
| **Heap** | Shared | **All objects** (and arrays) with their instance fields | `OutOfMemoryError` |
| **Java stack** | **One per thread** | Stack frames: local variables, parameters, return points | `StackOverflowError` |
| **PC register** | One per thread | Address of the current bytecode instruction | — |
| **Native method stack** | One per thread | Frames for native (C/C++) code called via JNI | `StackOverflowError` |

### Stack vs heap (🔴, asked in almost every Java interview)
📝 Doc 4:
```java
void handle() {
    int i = 5;               // local primitive → lives in handle()'s stack frame
    XYZ abc = new XYZ();     // the object → heap; the reference variable abc → stack frame
}
```
```
Thread's STACK                       HEAP (shared by all threads)
┌───────────────────────┐
│ handle(): i = 5        │
│           abc ─────────┼────────►  XYZ object { fields... }
├───────────────────────┤
│ main(): ...            │
└───────────────────────┘
```
- **Stack:** fast, automatic, tied to method calls. When `handle()` returns, its frame (with `i` and `abc`) disappears instantly. 📝 "The thread clears its own stack; GC is never involved."
- **Heap:** objects outlive the method that created them, as long as something still references them. They're cleaned up by the **garbage collector**.
- 📝 **Each thread has its own stack, and threads share one heap.** That's why local variables are naturally thread-safe, while shared objects need care (M13).

### ➕ Correction 3: "static variables live in the method area"
Your notes and the repo say this. That's the **conceptual model in the JVM specification**, and it's acceptable in most interviews. The precise modern answer, for HotSpot since Java 8:
- the old fixed-size *PermGen* area was replaced by **Metaspace** (native memory), which holds class metadata; and
- **static field values are stored on the heap**, inside the class's `java.lang.Class` object.

A strong interview answer is: "Conceptually the method area; in HotSpot since Java 8, class metadata is in Metaspace and static fields are on the heap."

📝 **References are "memory-address-like".** Java hides raw addresses from you (no pointer arithmetic), which removes a whole class of memory bugs. The native layer underneath still deals with real memory.

---

## 6.7 Garbage collection 🔴

### What and why
The **garbage collector (GC)** automatically frees heap memory used by objects that the program can no longer reach.

📝 Servers create huge numbers of short-lived objects: one request object, one result object, and so on, for every request. Without GC, you'd have to free each one manually, which is a constant source of crashes and leaks in C and C++.

### How it decides: reachability 🔴
An object is **alive** if it can be reached by following references from a **GC root**. ➕ GC roots include:
- local variables in active stack frames,
- static fields, and
- active threads.

Everything else is **eligible for collection**. 📝 Reachability is transitive: if object 1 → object 2 → object 3, and object 1 is reachable, then all three are alive.
```java
void process() {
    Order o = new Order();   // reachable via the local o
}                            // frame removed → no references → eligible for GC
```
Eligible doesn't mean collected immediately. The memory is reclaimed *whenever GC next runs*.

### Important truths from the notes 📝
- **GC works only on the heap.** The stack cleans itself.
- **GC is not on a timer.** It runs in response to **memory pressure**: more often on small heaps, less often on large ones.
- **GC doesn't care about business events** (Doc 9). Placing an order doesn't destroy the cart object. The cart is collected when nothing references it anymore. Your "orders" tab still shows completed orders because that data lives in the **database**, not in long-lived Java objects.
- **GC is not free.** It costs CPU, and some collections pause your application.

### Stop-the-world vs concurrent 📝
- **Stop-the-world** pauses all application threads, cleans up, then resumes. Requests stall during the pause, which shows up as **latency spikes**.
- **Concurrent/background** collection does much of its work while the app keeps running. Pauses are shorter, but it uses more CPU.

➕ 🟡 **Generational idea:** most objects die young. So the heap is split into a *young generation* (collected often and cheaply) and an *old generation* (long-lived objects, collected rarely and more expensively). A "full GC" of the old generation is the expensive one.

### Memory leaks in a garbage-collected language ➕ 🟠
GC can only free *unreachable* objects. If your code keeps references it no longer needs, those objects stay alive forever. Typical examples:
- a static `List` that keeps growing,
- a cache with no eviction, and
- listeners that are never removed.

📝 A **heap dump** (a snapshot of the heap) shows which objects pile up, which is how you find leaks.

➕ `System.gc()` is only a *hint*; the JVM may ignore it. The old `finalize()` method is deprecated, so never rely on it for cleanup (use try-with-resources, M10).

---

## 6.8 Execution: interpreter vs JIT 🟠

📝📁 The JVM can run bytecode in two ways:
- **Interpreter:** reads and executes bytecode one instruction at a time. **Fast startup**, but slow for code that runs repeatedly, because the same translation is redone every time.
- **JIT (Just-In-Time) compiler:** watches which methods and loops run often (**hot spots**) and compiles *those* into native machine code once, then reuses it. **Fast repeated execution**, but compiling costs CPU time and the compiled code costs memory.

📝 "Interpreter = translate each time; JIT = pay once, reuse many times."

**How they work together:**
1. Start by interpreting everything.
2. Count calls and loop iterations.
3. Compile the hot code.
4. Future calls run native code.

➕ This is why Java servers have a **warm-up period**: they're slower for the first few minutes after startup, until the hot paths are compiled.

📝 What's "hot" is decided **at runtime from actual behavior**, so you can't tell from source code which mode a method will run in, and the same code can be hot on one server and cold on another.

📁 Flags: `-Xint` (interpreter only), `-XX:+PrintCompilation` (show what gets JIT-compiled).

---

## 6.9 Putting it together: traffic spikes 📝 🟠
During a sale, traffic jumps from 1,000 to 10,000 queries per second (QPS). With the same number of servers, **each server does 10× the work**:
- **more CPU** for request handling;
- **more temporary objects** on the heap;
- so **more frequent GC**;
- which, together with CPU saturation, causes **higher latency**, plus occasional stop-the-world pauses that stall requests.

Adding servers (say 10 → 50) helps, but if traffic grows more than the server count does, each server is still busier than before.

📝 "Performance problems during spikes usually come from several costs stacking together: CPU, allocation, and GC." Reducing unnecessary object creation reduces GC pressure.

---

## 6.10 JVM tools 📁 🟡

| Tool | Purpose |
|---|---|
| `javap -c` | See bytecode |
| `java -verbose:class` | Trace class loading |
| `jps -l` | List running Java processes |
| `jcmd <pid> GC.heap_info` / `Thread.print` / `VM.flags` | Heap info, thread dump, flags |
| `jvisualvm` | GUI: memory, CPU, threads, GC activity |

---

## 6.11 Interview preparation — Module 6

1. **Explain the class loader hierarchy and parent delegation. Why delegate to the parent first?** 🔴 (Security: core classes can't be replaced; each class loaded once.) *Many candidates describe it backwards, as your notes did.*
2. **Loading → linking → initialization: what happens in each step?** Use `static int x = 5` going from 0 to 5 as your example.
3. **When are static blocks executed? How many times? Can they access instance variables?**
4. **Stack vs heap?** 🔴 What lives where in `XYZ abc = new XYZ();`; per-thread vs shared; `StackOverflowError` vs `OutOfMemoryError`.
5. **How does GC decide what to collect?** (Reachability from roots, not "no longer used".) **Can Java have memory leaks?** (Yes. Give an example.)
6. **What is stop-the-world? Why do servers see latency spikes during GC?**
7. **Interpreter vs JIT? What is warm-up?**
8. **Where are static variables stored?** (The two-level answer from §6.6.)
9. **`ClassNotFoundException` vs `NoClassDefFoundError`.**
10. **Scenario:** latency spikes every few minutes on an otherwise idle-looking server. What do you suspect, and which tool would you use first?

---

## 6.12 Check yourself — Module 6

**Q1 (predict).**
```java
class Config {
    static int port = 8080;
    static { System.out.println("loading config"); port = 9090; }
    Config() { System.out.println("new Config, port=" + port); }
}
// main:
System.out.println("start");
new Config();
new Config();
```

**Q2 (explain).** Draw the stack and heap after line 3:
```java
1  int count = 3;
2  String name = "Asha";
3  int[] xp = new int[count];
```
Then: which of these can the garbage collector ever touch?

**Q3 (spot the leak).**
```java
class RequestLog {
    static List<String> all = new ArrayList<>();
    static void record(String req) { all.add(req); }
}
```
Why will this server eventually crash, even though Java has GC?

**Q4 (correct the notes).** Rewrite this note so it's correct: "The application class loader tries first; if it can't load the class it asks the extension loader, which loads third-party libraries."

### Progress tracker — Module 6
- **Understand:** JVM components; lazy loading; the three loaders and correct parent delegation; verification, preparation and resolution; static initialization order and triggers; the five memory areas; stack vs heap; GC reachability, pauses and leaks; interpreter vs JIT; why spikes hurt.
- **Can do:** predict static-block output; draw stack/heap diagrams; explain a latency spike; use `-verbose:class` and `javap`.
- **Interview topics:** delegation; stack/heap; GC and leaks; static init; JIT warm-up.
- **Common mistakes:** delegation backwards; "GC cleans the stack"; "objects die when the method ends"; "GC runs on a schedule"; "static blocks run per object".
- **Prerequisites for M7:** the idea that the JVM finds classes by fully qualified name.

### Exercises — Module 6

**🟢 Beginner.** Write a class with two static blocks, a static counter, and a constructor. Predict the output of creating three objects, then run it.

**🟡 Intermediate.** Write a recursive method with no base case. Catch the `StackOverflowError` and print how deep the recursion got (keep a static depth counter). Run it with `java -Xss512k` and with `java -Xss4m`. Explain the difference using §6.6.

**🔴 Interview-level.** Run any program with `java -verbose:class` and count how many classes load before your `main` even starts. Then add an unused class reference inside an `if (false)` branch and an unused import. Do they load? Explain the result using lazy loading. Also explain why imports have no runtime effect (M7 confirms this).

---

# Module 7 — Packages, Access Modifiers & Encapsulation 🔴

> **Prerequisites:** M5 (classes, fields, methods, constructors), M6.3 (classes are found by name).
> **You'll need this for:** M8 (`protected`), M11–M12 (every design relies on hiding internals), and any real codebase.

---

## 7.1 Packages 🔴

### What is it?
📁 A **package** is a named group of related classes, like a folder for code. You declare it as the **first statement** of a file:
```java
package school.math;

public class Teacher { ... }
```

### Why does it exist?
📁📝 Three reasons:
1. **Avoiding name collisions.** Two libraries can both have a class called `Teacher`, `List`, or `Logger`.
2. **Access control.** Package-private visibility (§7.3).
3. **Organization.** Related classes live together.

### How it works: the fully qualified class name (FQCN) 🔴
📝 Java identifies a class by **package + class name**: `school.math.Teacher` and `school.science.Teacher` are **two different classes**. 📝 "Java identifies a class by its full name, not by its short name alone." That's what the class loader (M6) looks up.

**Rules:**
- Same short name in **different packages**: allowed.
- Same short name in the **same package**: not allowed, because the FQCN would be duplicated.
- 📝 A conflict between your class and a library class with the *same FQCN* only matters if both are actually on the runtime classpath.

### ➕ Correction: "packages are strings, not folders"
📝 Doc 5 says packages are just strings. That's **half right**:
- ✅ To the JVM, a package name is just part of the class's name. There's **no hierarchy**: `school`, `school.math`, and `school.mathx` are unrelated. A "sub-package" gets no special access to its "parent" package. 📝 "Prefixes don't imply closeness."
- ❌ But in practice, the compiler and the default class loader find `school.math.Teacher` by looking for the file `school/math/Teacher.class` under a classpath directory or JAR. So **the folder structure must match the package**. 📁 The repo's layout (`a_packages/school/math/Teacher.java`) follows this.

**Conventions:** package names are all lowercase. Companies use reversed domain names (`com.airtribe.lms.service`) for global uniqueness ➕.

---

## 7.2 Imports 🔴

### What is it?
An `import` lets you use a class's **short name** in your file:
```java
import school.math.Teacher;      // now "Teacher" means school.math.Teacher in this file
import java.util.Scanner;
import java.util.*;               // wildcard: every class DIRECTLY in java.util
```

### How it works ➕
**An import is purely a compile-time shortcut.** It doesn't load anything, doesn't copy code, and has zero runtime cost. The compiled bytecode always contains full names. 📝 "If you use the fully qualified name everywhere, you don't need imports at all."

➕ **`java.lang` is imported automatically.** That's why `String`, `System`, `Math` and `Integer` work without an import.

### Conflicts (verified)
```java
import school.math.Teacher;
import school.science.Teacher;
// error: a type with the same simple name is already defined by the single-type-import of Teacher
```
📝 Resolve it by importing one (or neither) and using the FQCN for the other. Verified working:
```java
import school.math.Teacher;

Teacher t1 = new Teacher();                                        // math
school.science.Teacher t2 = new school.science.Teacher();          // science
System.out.println(t1.subject() + " " + t2.subject());             // Math Science
```
📝 Java has **no import aliases**: you can't write `import x.Teacher as MathTeacher`.

### Wildcards are not recursive (verified) 📝📁
```java
import school.*;          // imports classes DIRECTLY in package "school" only
Teacher t;                // error: cannot find symbol
                          // (Teacher is in school.math, a different, unrelated package)
```
`*` is not a regex and doesn't reach into "sub-packages", because sub-packages don't really exist (§7.1).

➕ 🟡 **Static imports** let you use static members without the class name: `import static java.lang.Math.max;` then `max(a, b)`. Use them sparingly.

📝 Many red underlines in your IDE are editor assistance, not Java rules. Only the compiler and runtime enforce Java.

---

## 7.3 Access modifiers 🔴

### What is it?
📁 **Access modifiers** control *who can see and use* a class, field, method, or constructor.

### Why does it exist?
To enforce boundaries. If anything can reach any field, any piece of code can break any object (§7.4). Access modifiers let the compiler **guarantee** that only approved code touches internals.

### The levels (`protected` is completed in M8)

| Modifier | Same class | Same package | Subclass in another package | Anywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *(none)* = **default / package-private** | ✅ | ✅ | ❌ | ❌ |
| `protected` (M8) | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

📁 The repo's `Person` / `SamePackageDemo` / `DifferentPackageDemo` demonstrate exactly this:
- a `private` field can't be accessed even from another class in the *same* package;
- a default field works within the package but not outside it;
- `public` works everywhere.

Verified compiler messages for an access from another package:
```
error: id has private access in Animal
error: xyz is not public in Animal; cannot be accessed from outside package
```

### Rules worth knowing ➕
- A **top-level** class can only be `public` or default (not `private` or `protected`). A `.java` file may contain **at most one public top-level class**, and it must match the file name.
- Access is checked at **compile time**, per class, not per object. A `private` field is accessible from *any* `Account` code, including through another `Account` object (`other.balance` inside `Account` is fine).
- **Choose the most restrictive level that works.** Start with `private` and open up only when needed. 📝 Doc 6: "choose the least permissive access level that still supports the design."

---

## 7.4 Encapsulation 🔴

### What is it?
📁 **Encapsulation** has two parts:
1. **Bundling** data with the methods that operate on it, in one class.
2. **Hiding** the data, so it can only be changed through those methods.

**The recipe:**
- `private` fields,
- `public` methods that provide *controlled* access, and
- validation in those methods.

### Why does it exist? 📝📁
Without it, anyone can put an object into a nonsense state:
```java
student.age = -5;             // no validation
account.balance = 999999;     // anyone can create money
```
Every class has **invariants**: rules that must always hold, like "balance is never negative" or "age is between 0 and 150". Encapsulation means the class **enforces its own invariants**, in one place, instead of trusting every caller everywhere.

📁 Analogy (class 1 notes): an ATM. You press buttons; you don't reach into the cash drawer.

### Getters: read without write
```java
public class BankAccount {
    private double balance;
    public double getBalance() { return balance; }   // callers can look but not touch
}
```

### Setters: write with validation
```java
public void setAge(int age) {
    if (age > 0 && age < 150) {
        this.age = age;
    } else {
        System.out.println("Invalid age: " + age);   // (M10: throw an exception instead)
    }
}
```

### Better than setters: behavior methods 📝 🔴
For many fields, a raw setter is still too much power. `setBalance(anything)` is barely better than a public field. Expose **meaningful operations** instead:
```java
public void deposit(double amount) {
    if (amount <= 0) { System.out.println("Deposit must be positive"); return; }
    balance += amount;
}

public void withdraw(double amount) {
    if (amount <= 0) { System.out.println("Withdrawal must be positive"); return; }
    if (amount > balance) { System.out.println("Insufficient funds"); return; }
    balance -= amount;
}
```
📝 **The bug this prevents:** a naive `balance = balance - amount;` with `amount = -500` *increases* the balance. A withdrawal that creates money.

### Not everything needs a setter 📝
In the report card, the roll number is fixed after construction, so it gets **no setter**. ➕ Taking that further, **immutable objects** have all fields `private final`, set once in the constructor, with no setters. 📁 The Food Court solution's `MenuItem` and `Bill` are written this way. Immutable objects are simpler to reason about and automatically thread-safe (M13).

### ➕ Encapsulation leaks: returning internal mutable objects 🟠
📁 The Food Court `Bill` has `private final List<MenuItem> items` and `getItems() { return items; }`. `final` stops the *field* from being re-pointed, but the getter hands out a remote to the **same list**. Any caller can then do `bill.getItems().clear()`.

Fixes:
- return a copy, or an unmodifiable view (`List.copyOf(items)`, `Collections.unmodifiableList(items)`); and
- copy incoming mutable data in the constructor (a **defensive copy**, M5 exercise).

---

## 7.5 Practice problem — Student Report Card 📁 🔴

**Specification** (`class3/d_practice_problem`):
- **Private fields:** `name` (String), `rollNumber` (int), `mathScore`, `scienceScore`, `englishScore` (double, 0–100).
- **Constructor:** takes `name` and `rollNumber`.
- **Getters** for all fields; **setters only for the three scores**. If a score is outside 0–100, print an error and **do not update** it.
- `getPercentage()` returns the average of the three scores.
- `getGrade()` returns "A" if ≥ 80, "B" if ≥ 60, "C" if ≥ 40, otherwise "F".
- `printReportCard()` prints:
  ```
  === Report Card ===
  Name: Rahul
  Roll Number: 101
  Math: 85.0
  Science: 72.0
  English: 90.0
  Percentage: 82.33%
  Grade: A
  ===================
  ```
- **Test it from `Main`:**
  - Rahul (101): scores 85/72/90, print the report;
  - try `setMathScore(-10)` and confirm it's rejected;
  - Priya (102): set scores, print the report.

**Notes from class (📝 Doc 6):**
- The divisor stays 3, because the percentage is over the full subject set.
- Avoid repeating the 0–100 check in three setters. Use one private helper `isValidScore(double)` (DRY, M12).
- ➕ Doc 6 describes a version where an invalid score was "reset to 0". That **contradicts the specification** and corrupts the data (a real student's mark becomes 0). The repo's "don't update" rule is correct. 📝 The class also noted that a real application would re-prompt the user.

<details><summary>Hint 1</summary>Which fields need <code>final</code>? Which need setters? Answer this before writing code.</details>
<details><summary>Hint 2</summary><code>String.format("%.2f", value)</code> for 82.33.</details>

Compare with `class3/d_practice_problem/solution/` after you finish.

---

## 7.6 Interview preparation — Module 7

1. **What is encapsulation, and why not just make fields public?** 🔴
   *Strong answer:* "Encapsulation means a class owns its data and enforces its own invariants. Fields are private, and changes go through methods that validate them. Public fields mean any code anywhere can break the object, and validation would have to be duplicated at every call site." Then add: "I prefer behavior methods like `deposit`/`withdraw` over raw setters."
2. **Name the four access levels and what each allows.** Which applies when you write no modifier?
3. **Is a class with private fields and public getters/setters for every field encapsulated?** (Only nominally. A setter without validation is a public field with extra steps.)
4. **What is a fully qualified class name? How do you use two classes with the same short name in one file?**
5. **Does `import java.util.*` import `java.util.concurrent.ExecutorService`?** Why not?
6. **Do imports affect performance or class loading?** (No. Compile time only.)
7. **Can a top-level class be private?**
8. **Coding:** design a `Temperature` class that stores Celsius but can never go below absolute zero (−273.15), with `getCelsius()`, `getFahrenheit()`, and `increaseBy(double)`.

---

## 7.7 Check yourself — Module 7

**Q1 (find the problems).**
```java
public class Wallet {
    public double balance;
    public List<String> transactions = new ArrayList<>();
    public void setBalance(double b) { balance = b; }
    public void withdraw(double amt) { balance = balance - amt; transactions.add("W:" + amt); }
}
```
List at least four encapsulation problems, then rewrite the class.

**Q2 (compile or not?).** `Person` is in package `app.model` with `private int id; String nickname; public String name;`. For each line in `app.ui.Screen`, say whether it compiles:
- `p.id`
- `p.nickname`
- `p.name`

What changes if `Screen` moves into `app.model`?

**Q3 (explain).** A teammate says "packages are hierarchical, so code in `com.shop.order` can access package-private members of `com.shop`." Correct them.

### Progress tracker — Module 7
- **Understand:** packages and FQCNs; folder mapping; imports as compile-time shortcuts; conflicts and FQCN resolution; non-recursive wildcards; the access table; encapsulation, invariants, behavior methods, immutability, leaks.
- **Can code:** properly encapsulated classes with validated operations; multi-package projects; the Report Card.
- **Interview topics:** encapsulation (why, not just how); access levels; FQCN; imports.
- **Common mistakes:** setters without validation; returning internal mutable collections; package "hierarchy" assumptions; resetting bad input to 0.
- **Prerequisites for M8:** access levels (`protected` comes next); constructors.

### Exercises — Module 7

**🟢 Beginner.** The Report Card (§7.5).

**🟡 Intermediate.** Build a `BankAccount` with:
- `private` fields: an owner, a balance, and an account number that can never change;
- `deposit`/`withdraw` with validation;
- a transaction history, where `getHistory()` returns something the caller **cannot** modify; and
- an overdraft limit, set at construction, that allows the balance to go down to `-limit`.

Write a `main` that tries to break your class in five different ways.

**🔴 Interview-level.** Put `BankAccount` in package `bank.core` and a `Teller` class in `bank.ui`. Make `Teller` able to deposit and withdraw, while a helper method `applyMonthlyInterest()` stays usable only by classes inside `bank.core`. Which modifier did you choose for each member, and why? Then answer: what would go wrong with your design if `bank.ui` code could call `applyMonthlyInterest()`?

---

# Module 8 — Inheritance 🔴

> **Prerequisites:** M5 (constructors, `this`), M6.5 (static initialization), M7 (access modifiers).
> **You'll need this for:** M9 (polymorphism requires inheritance), M10 (exception hierarchies, custom exceptions), M11 (abstract classes), M12 (LSP; composition over inheritance).

---

## 8.1 What inheritance is and why it exists 🔴

### What is it?
📝📁 **Inheritance** lets a new class (the **child**, or *subclass*) automatically get the fields and methods of an existing class (the **parent**, or *superclass*). You write it with `extends`:
```java
class Vehicle {
    String brand;
    void move() { System.out.println(brand + " is moving"); }
}

class Car extends Vehicle {
    int numDoors;
    void drive() { System.out.println("Driving with " + numDoors + " doors"); }
}

Car c = new Car();
c.brand = "Maruti";   // inherited field
c.move();             // inherited method
c.drive();            // Car's own method
```

### Why does it exist? 📝
Without inheritance, `Car`, `Truck`, `Bike`, `Bus` and `Scooty` would each repeat `wheels`, `color`, `brand`, `model`, `engine`… That causes four problems:
- the same code is copied many times;
- a new shared field must be added in every class;
- it's easy to forget one class; and
- names drift apart (`colour` here, `color` there).

📝 "Put shared features in the parent class, define them once, reuse them everywhere."

### The is-a test 🔴
📝 Inheritance must model a real **is-a** relationship:
- ✅ a car *is a* vehicle; an electric car *is a* car; a circle *is a* shape.
- ❌ A human *is a* table? A table *is a* human? Both have `numberOfLegs`, but **shared fields alone never justify inheritance**.

A better hierarchy is `Furniture` → `Table`, `Chair`, `Wardrobe`.

➕ This test matters even more in M12. A child must also *behave* like its parent (the Liskov Substitution Principle). When the relationship is really **has-a** ("a car has an engine"), use a field instead (composition).

### Types of inheritance 📁

| Type | Shape | Java classes? |
|---|---|---|
| Single | `B extends A` | ✅ |
| Multilevel | `A → B → C` (e.g. `Vehicle → Car → ElectricCar`) | ✅ |
| Hierarchical | `A` is parent of `B` and `C` | ✅ |
| Multiple | `C extends A, B` | ❌ (§8.8) |
| Hybrid | A mix of the above | Only without the "multiple" part |

📝 **Multilevel inheritance accumulates members.** `ElectricCar` gets `brand`/`move()` from `Vehicle`, `numDoors`/`drive()` from `Car`, plus its own `chargeBattery()`.

---

## 8.2 What exactly is inherited? 🔴

### The "copy-paste" model vs reality
- 📝 Doc 6 offers a helpful *mental* model: imagine the parent's members pasted into the child.
- 📝 Doc 7 corrects it: **Java does not copy anything.** The child class stores only its own members. When code uses a name, Java looks it up along the inheritance chain (§8.6).
- Inside memory, a `Car` object contains **one set of fields**: Vehicle's fields *plus* Car's fields. The "parent part" isn't a separate object. 📁 The instructor's comment in `Dog.java` ("hidden reference of Animal") is a way of picturing this.

### The rules ➕
- `public` and `protected` members are accessible in the child; so are default members, *if the child is in the same package*.
- **`private` fields are inherited in the sense that they exist inside the child object**, but the child's code **cannot access them by name**. 📝 "Inheritance does not bypass access control." The child uses public or protected getters, or `protected` fields (§8.3).
- **Constructors are NOT inherited.** Each class declares its own, and they're connected by `super(...)` (§8.4).
- A child can **add** new fields and methods, and **override** methods (§8.5, M9).

---

## 8.3 `protected` 🔴

### What and why
📁 "If the parent's fields are `private`, the child can't touch them directly even though it inherited them. This is where `protected` comes in."

**`protected` = default (same package) + access from subclasses in other packages.**

| Modifier | Same class | Same package | Subclass, other package | Everywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| default | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

📁 The repo's `parent/Animal.java` and `child/Dog.java` (a different package): Dog can use the protected `name`, `age`, and `displayInfo()`, but not the private `id` or the default-access field.

### ➕ The subtle rule (verified, a favorite tricky interview question)
In a subclass in a *different* package, protected members are accessible **only through the subclass's own type**, meaning `this` or a `Dog` reference, **not** through a plain `Animal` reference:
```java
package zoo.child;
public class Dog extends Animal {
    void test(Animal other, Dog sibling) {
        System.out.println(name);          // ✅ inherited, via this
        System.out.println(sibling.name);  // ✅ via a Dog reference
        System.out.println(other.name);    // ❌ error: name has protected access in Animal
    }
}
```
**Why:** `protected` grants access to *your own inheritance relationship*, not a general licence to poke into any `Animal` anywhere (which could be a `Cat` in someone else's code).

### When to use it 📝
- Use `protected` when subclasses legitimately need direct access and unrelated classes shouldn't have it.
- Don't make something `public` just because a subclass needs it.
- ➕ Many teams still prefer `private` fields plus `protected`/`public` getters, because protected fields let every subclass break the parent's invariants.

---

## 8.4 `super` and constructor chaining 🔴

### What is `super`?
📝📁 `super` refers to the **immediate parent** part of the current object. It has three uses:
```java
super(name);          // 1. call the parent constructor
super.display();      // 2. call the parent's version of an overridden method
super.color;          // 3. access a parent field hidden by a same-named child field (§8.6)
```
📝 In a chain `A → B → C → D`, `super` inside `D` means `C`, and only `C`. ➕ `super.super.x` is not allowed. You reach `A` only through what `B` and `C` expose.

### Constructor chaining: why the parent must be built first
A child object *contains* the parent's fields, and the parent's constructor knows how to initialize them correctly. So **every constructor's first action is to call a parent constructor**.

📝📁 **The rules:**
1. If you don't write `super(...)`, Java **inserts `super();`** (the no-arg one) as the first line.
2. If the parent **has no no-arg constructor**, that automatic `super()` fails, so you **must** call `super(...)` explicitly. Verified:
   ```java
   class Animal { String name; Animal(String n) { name = n; } }
   class Dog extends Animal { Dog() { System.out.println("x"); } }
   // error: constructor Animal in class Animal cannot be applied to given types;
   ```
   The fix:
   ```java
   Dog(String name) { super(name); }
   ```
3. `super(...)` or `this(...)` must be the **first statement**, and a constructor can't contain **both**. (A `this(...)` call delegates to another constructor, which will call `super`.)
4. The chain continues all the way to **`Object`** (§8.7).

📝 "If there's no `extends`, there's no parent–child relationship." Creating `new Animal()` inside `Dog`'s code just makes an unrelated separate object; that isn't inheritance.

### The full object-creation order ➕ 🔴 (verified)
```java
class Parent {
    static { System.out.println("Parent static"); }
    { System.out.println("Parent instance block"); }        // instance initializer block
    Parent() { System.out.println("Parent constructor"); describe(); }
    void describe() { System.out.println("Parent.describe"); }
}
class Child extends Parent {
    static { System.out.println("Child static"); }
    String label = "set";
    Child() { super(); System.out.println("Child constructor"); }
    @Override void describe() { System.out.println("Child.describe label=" + label); }
}
new Child();
```
Output:
```
Parent static                  ← class initialization: parent class first (M6.5), once per class
Child static
Parent instance block          ← object construction: parent part first
Parent constructor
Child.describe label=null      ← !!! the overridden method ran before Child's fields were set
Child constructor
```
**The order:**
1. static initialization, parent then child (once);
2. the object is allocated with all fields zeroed;
3. the parent's field initializers, instance blocks, and constructor body;
4. the child's field initializers (`label = "set"` happens **here**), instance blocks, and constructor body.

**The trap:** `Parent`'s constructor called `describe()`, which is overridden in `Child`. Overriding uses the actual object (M9), so `Child.describe()` ran **before `label` was initialized**, and saw `null`.

**Rule: never call overridable methods from a constructor.**

---

## 8.5 Overriding: a first look (full rules in M9) 🔴

📝 A child can **override** a parent method by declaring a method with the **same signature**. The child keeps the inherited structure but changes the behavior.
```java
class Vehicle { void move() { System.out.println("Vehicle moves"); } }
class Car extends Vehicle {
    @Override
    void move() {
        super.move();                              // optional: reuse the parent's behavior
        System.out.println("Car drives on roads");
    }
}
```

### `@Override` 📝 🔴
`@Override` is an **annotation** (a marker for the compiler). It says "this method is *meant* to override something". It doesn't change runtime behavior; it adds a **compile-time check**.

If you mistype the name or parameters, for example `drive1()` instead of `drive()`:
- **without** `@Override`, it silently compiles as a *new* method, and the override is lost;
- **with** it, the compiler refuses. Verified:
  ```
  error: method does not override or implement a method from a supertype
  ```

**Always use `@Override`.** It turns a silent refactoring bug into a compile error.

---

## 8.6 Name lookup and field hiding 🟠

### Lookup order 📝
When code uses a simple name, Java searches:
1. local variables and parameters,
2. fields of the current class,
3. fields inherited from the parent (and up the chain).

The first match wins. That's why a parameter hides a field (hence `this.`), and why a child can use a parent field it never declared.

### Fields are hidden, not overridden ➕ 🔴 (verified)
📝 Doc 7: "methods are overridden; variables are not". Here's what that means concretely:
```java
class Parent { String name = "parent-field"; String getName() { return name; } }
class Child extends Parent {
    String name = "child-field";                       // a SECOND, separate field
    @Override String getName() { return name; }
}
Parent p = new Child();
System.out.println(p.name);        // parent-field  ← field chosen by the REFERENCE type (compile time)
System.out.println(p.getName());   // child-field   ← method chosen by the OBJECT type (runtime)
```
The `Child` object actually contains **both** fields. Declaring a same-named field in a child is almost always a mistake. It's confusing and rarely useful.

---

## 8.7 `Object`: the root of everything 🔴
📝 Every class without an explicit `extends` implicitly `extends Object`. So **every** inheritance chain ends at `java.lang.Object`:
```
Dog → Animal → Object
```
That's why every object has `toString()`, `equals()`, `hashCode()`, `getClass()` (M5.9). Every constructor chain ends with `Object()`, which Java handles implicitly. 📝 It's "the special base case underneath everything".

---

## 8.8 Why Java forbids multiple inheritance of classes 🔴

📝📁 If `C extends A, B` were allowed, and both `A` and `B` defined `xyz()` (or a field with the same name), which one does `C` get? The ambiguity becomes worst in the **diamond problem**:
```
        A   (defines xyz)
       / \
      B   C   (both override xyz differently)
       \ /
        D     ← D extends B, C: which xyz? Is there one copy of A's fields, or two?
```
Java's designers chose simplicity: **one parent class per class.** Verified: `class C extends A, B` doesn't even parse (`error: '{' expected`).

📝 There's a maintenance argument too: changes in either parent could unexpectedly break the child.

**Java's answer to "I need several types"** is **interfaces** (M11): a class can implement many of them. ➕ Since Java 8 interfaces can carry default implementations, and Java resolves the resulting conflicts by **forcing you to override** (M11.3).

---

## 8.9 When NOT to use inheritance (preview of M12) 🟠
- The relationship is has-a, not is-a → use a field (composition).
- The child can't honor everything the parent promises (an `Ostrich` that can't `fly()`) → the hierarchy is wrong (LSP).
- Behaviors combine in many ways (flying/non-flying × quacking/squeaking/silent) → subclass explosion → composition over inheritance.

---

## 8.10 Interview preparation — Module 8

1. **What is inheritance? What does a subclass inherit, and what doesn't it?** (Constructors aren't inherited; private members exist in the object but aren't accessible by name.)
2. **Why doesn't Java support multiple inheritance of classes? How does it compensate?** 🔴 Explain the diamond problem, then interfaces.
3. **`protected` vs default.** *Tricky follow-up:* can a subclass in another package access a protected field through a parent-type reference? (No. Explain why.)
4. **What does `super` do? Can you call `super.super.method()`?**
5. **What happens if the parent has only a parameterized constructor and the child constructor doesn't call `super(...)`?** 🔴
6. **Give the exact order of execution when creating a child object**, including static blocks, instance blocks, and constructors. 🔴
7. **Why is calling an overridable method from a constructor dangerous?** (Show the `label=null` example.)
8. **Are fields overridden?** What does `Parent p = new Child(); p.name` print?
9. **What does `@Override` do at runtime?** (Nothing. It's a compile-time check.)
10. **Is "Table extends Human because both have legs" good design? Why not?**

---

## 8.11 Check yourself — Module 8

**Q1 (predict).**
```java
class A { A() { System.out.println("A"); } }
class B extends A { B() { System.out.println("B"); } }
class C extends B { C() { super(); System.out.println("C"); } }
new C();
```

**Q2 (find the bug and fix it two ways).**
```java
class Employee {
    private String name;
    Employee(String name) { this.name = name; }
}
class Manager extends Employee {
    int reports;
    Manager(int reports) { this.reports = reports; }
    void show() { System.out.println(name + " manages " + reports); }
}
```

**Q3 (judge the design).** For each, decide is-a or has-a, and whether inheritance fits:
- (a) `Stack extends ArrayList`
- (b) `SavingsAccount extends BankAccount`
- (c) `Car extends Engine`
- (d) `Admin extends User`
- (e) `Square extends Rectangle` (save this one; M12 revisits it)

### Progress tracker — Module 8
- **Understand:** is-a; what is and isn't inherited; `protected` (including the subtle rule); `super`; implicit `super()`; full creation order; overriding with `@Override`; field hiding; `Object`; the diamond problem.
- **Can code:** class hierarchies with correct constructor chaining; overriding with `super.method()` reuse.
- **Interview topics:** multiple inheritance and the diamond; constructor chaining; creation order; protected; field hiding.
- **Common mistakes:** missing `super(...)`; calling overridable methods in constructors; same-named fields in the child; inheriting for code reuse without a true is-a.
- **Prerequisites for M9:** overriding; parent references.

### Exercises — Module 8

**🟢 Beginner.** `Vehicle` (brand, `move()`) → `Car` (numDoors, overrides `move()`) → `ElectricCar` (batteryKwh, `chargeBattery()`, overrides `move()` and calls `super.move()`). Give every class only parameterized constructors. Create an `ElectricCar` and trace which constructors run, in what order.

**🟡 Intermediate.** Put `Animal` (with `protected String name`, `private int id`, and a protected method `describe()`) in package `zoo.base`, and `Dog`/`Cat` in `zoo.pets`. In `Dog`, write code that tries to read `name` via `this`, via a `Dog` parameter, and via an `Animal` parameter. Predict which compile, then check.

**🔴 Interview-level.** Without running it, write the exact output of this program, then run it and explain any difference.
```java
class Base {
    static { System.out.print("1 "); }
    { System.out.print("2 "); }
    Base() { System.out.print("3 "); init(); }
    void init() { System.out.print("4 "); }
}
class Derived extends Base {
    static { System.out.print("5 "); }
    int value = 7;
    { System.out.print("6 "); }
    Derived() { System.out.print("7 "); }
    @Override void init() { System.out.print("init:" + value + " "); }
}
// main:
new Derived();
new Derived();
```
<details><summary>Hint 1</summary>Static blocks: how many times in total, across both objects?</details>
<details><summary>Hint 2</summary>When <code>init()</code> runs inside Base's constructor, has <code>value = 7</code> executed yet?</details>

---

# Module 9 — Polymorphism 🔴

> **Prerequisites:** M4 (method signatures), M5 (references), M8 (inheritance, overriding, `@Override`).
> **You'll need this for:** M10 (catching exceptions by parent type), M11 (programming against abstract types and interfaces), and M12, where **OCP, LSP, DIP and composition over inheritance are all applications of polymorphism**.

---

## 9.1 What is polymorphism? 🔴

📁 *Polymorphism* is Greek for "many forms". In Java it means **one name can mean different behavior depending on context**. There are two kinds, and the difference is **who decides which code runs, and when**:

| | Compile-time (static / early binding) | Runtime (dynamic / late binding) |
|---|---|---|
| Mechanism | **Overloading** | **Overriding** |
| Who decides | The **compiler** | The **JVM** |
| Based on | Argument types written in the call | The **actual object type** at runtime |
| Signatures | Same name, **different** parameters | Same name, **same** parameters |
| Needs inheritance? | No | Yes |

This is the compile-time vs runtime split from M1, applied to method calls.

---

## 9.2 Overloading (compile-time polymorphism) 🔴

### What and why
📝📁 Several methods share a **name** but have **different parameter lists**: a different number of parameters, different types, or different order. One concept ("add") can then accept different kinds of input without awkward names like `addInts` and `addDoubles`.

📁 `OverloadingDemo`:
```java
int    add(int a, int b)            { return a + b; }
int    add(int a, int b, int c)     { return a + b + c; }
double add(double a, double b)      { return a + b; }
String add(String text, int number) { return text + number; }
String add(int number, String text) { return number + text; }   // order differs → allowed
```
📝 Return types *may* differ between overloads, **but the return type alone can't distinguish them** (verified: `method f(int) is already defined`). The reason: a call like `f(5);` that ignores the result would give the compiler no way to choose.

**Constructor overloading** is the same idea (📁 `Employee()`, `Employee(String)`, `Employee(String, int, double)`; M5.3).

### How the compiler chooses (overload resolution) 📁 ➕
1. **Exact match.**
2. **Widening** of primitives (`byte → short → int → long → float → double`, `char → int`).
3. ➕ **Autoboxing** (`int → Integer`), then widening to a parent class (`Integer → Object`).
4. ➕ **Varargs** (`int... values`) as a last resort.
5. If two candidates are equally good, the call is ambiguous: **compile error**.

Verified, with `show(int)`, `show(double)`, `show(Object)`, `show(String)` available:
```java
t.show(5);                    // int     (exact)
t.show(5L);                   // double  (long widens to double; no show(long) exists)
t.show('a');                  // int     (char widens to int, preferred over boxing to Object)
t.show("hi");                 // String  (exact)
t.show(null);                 // String  (null fits String and Object; String is more specific)
t.show(Integer.valueOf(3));   // Object  (Integer is-an Object)
```
And with only `show(String)` and `show(Integer)`:
```java
show(null);   // error: reference to show is ambiguous   (neither is more specific)
```

---

## 9.3 Overriding (runtime polymorphism) 🔴

### What and why
📝 A child class provides its own implementation of a method inherited from its parent: same name, same parameters. Code written against the *parent* type then automatically runs the *child's* behavior.

📝 Doc 7's example:
```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override void makeSound() { System.out.println("Woof"); }
}
class Cat extends Animal {
    @Override void makeSound() { System.out.println("Meow"); }
}

Animal[] zoo = { new Dog(), new Cat(), new Animal() };
for (Animal a : zoo) a.makeSound();   // Woof, Meow, Some sound
```
**This is the payoff.** The loop knows only "Animal". It works with any current or future subclass **without modification**. That's exactly the Open/Closed Principle in M12.

### Overriding rules ➕ 🔴

| Rule | Example / reason |
|---|---|
| Same name and same parameter types | Otherwise it's an overload, not an override |
| Return type: same, or a **subtype** (covariant) | Parent returns `Animal`, child may return `Dog` |
| **Can't reduce visibility** | Verified: a `private void draw()` overriding a default-access `draw()` fails with `error: draw() in Circle cannot override draw() in Shape`. A child must be usable everywhere the parent is. |
| Can't throw **broader checked exceptions** (M10) | Callers coded against the parent must not be surprised |
| `private` methods aren't overridden | They're invisible to the child; a same-named child method is a brand-new method |
| `static` methods aren't overridden | They're *hidden* (§9.6) |
| `final` methods can't be overridden | The class author locked them |

Use `@Override` every time (M8.5).

---

## 9.4 Parent reference, child object (upcasting) 🔴

### What is it?
📝 A variable of a parent type can refer to a child object:
```java
Animal a1 = new Dog();      // ✅ a Dog IS an Animal: "upcasting" (automatic, always safe)
Dog d = new Animal();       // ❌ compile error: not every Animal is a Dog
```
This mirrors primitive widening (M2): child → parent is automatic and safe.

### The two-types rule (the heart of runtime polymorphism) 🔴
📝 Every such call involves **two types**:
- **The reference type** (`Animal` in `Animal a1`) decides **what you're allowed to call**. That's checked by the compiler.
- **The object type** (`Dog`, created by `new Dog()`) decides **which overridden version runs**. That's chosen by the JVM.

```java
Animal a1 = new Dog();
a1.makeSound();   // "Woof": the Dog version runs (object type)
a1.fetch();       // compile error if fetch() exists only in Dog (reference type)
```

### How the JVM does it ➕ 🟡
Each class has a method table (a *vtable*) mapping method signatures to implementations. Calling `a1.makeSound()` looks up `makeSound` in the table of `a1`'s *actual* class. This is **dynamic dispatch**, and it costs very little. Your repo notes call it "slightly slower" than overloading, and in practice JIT compilation (M6.8) mostly erases even that difference.

### Why program to the parent type?
Your code then depends on the *general* contract, not the specific class, so new subclasses work without changes. Real code writes `List<String> names = new ArrayList<>();` and `PaymentMethod pm = new UpiPayment();` for exactly this reason (M11, M12).

---

## 9.5 Downcasting and `instanceof` 🔴

### What is it?
To use child-only members through a parent reference, you **downcast**:
```java
Animal a1 = new Dog();
Dog d = (Dog) a1;     // explicit, like narrowing in M2
d.fetch();            // now allowed
```
📝 "Casting only changes how the compiler treats the reference. It does not change the real object."

### When it fails (verified)
If the object isn't really that type, the cast **compiles but throws at runtime**:
```java
Object o = "text";
Integer bad = (Integer) o;
// ClassCastException: class java.lang.String cannot be cast to class java.lang.Integer
```

### Check first with `instanceof`
```java
if (a1 instanceof Dog) {
    Dog d = (Dog) a1;
    d.fetch();
}
// ➕ Java 16+ pattern matching: test and cast in one step (verified)
if (o instanceof String str) {
    System.out.println(str.length());   // 4
}
```

**Design note ➕:** frequent `instanceof` checks plus downcasts usually mean a missing overridden method. If you're writing "if Dog do this, if Cat do that", put a method in `Animal` and override it instead.

---

## 9.6 What is NOT polymorphic 🔴 (favorite tricky questions)

Verified with `Parent p = new Child();`:

| Member | Which one is used? | Why |
|---|---|---|
| Instance method (overridden) | **Child's** (object type) | Dynamic dispatch |
| **Field** with the same name | **Parent's** (reference type) | Fields are hidden, not overridden (M8.6) |
| **Static method** with the same name | **Parent's** (`p.util()` printed `Parent.util`) | Static methods belong to the class and are bound at compile time: *method hiding* |
| `private` method | Never dispatched | Invisible to the child |
| **Which overload** gets called | Chosen from the **reference/argument types** at compile time | Overloading is compile-time |

📝 "Static methods belong to the class, not the object… call the version you want through the class name: `A.test()`, `B.test()`."

---

## 9.7 Practice problem — Employee Payroll 📁 🔴

**Specification** (`class4/e_practice_problem`):
1. **`Employee`:**
   - `private` fields `name`, `id`, `baseSalary`;
   - a constructor taking all three;
   - `calculatePay()` returns `baseSalary`;
   - `displayInfo()` prints name, id, and the calculated pay.
2. **`FullTimeEmployee extends Employee`:**
   - an extra field `bonus`;
   - a constructor using `super(...)`;
   - **override** `calculatePay()` to return base + bonus.
3. **`PartTimeEmployee extends Employee`:**
   - fields `hoursWorked`, `hourlyRate`;
   - a constructor that passes 0 as baseSalary to `super`;
   - **override** `calculatePay()` to return hours × rate.
4. In `FullTimeEmployee`, **overload** `displayInfo()`:
   - `displayInfo()` shows everything, including the bonus;
   - `displayInfo(boolean showBonus)` hides the bonus line when `false`.
5. **`Main`:** put both employees in an `Employee[]`, loop, and call `displayInfo()`. Observe polymorphism.

Expected output:
```
=== Employee Details ===
Name: Rahul | ID: 101
Total Pay: 85000.0

Name: Priya | ID: 102
Total Pay: 12000.0
```

📝 Your Doc 7 calls the classes `PartTimeEmployee` (base + bonus) and `HourlyEmployee` (hours × rate). The repo's naming is used here. Doc 7's worked numbers still make good test cases: base 1000 + bonus 100 = **1100**; 4 hours × 100 = **400**.

**Design questions to answer before coding:**
- `baseSalary` is `private`. How does `FullTimeEmployee.calculatePay()` read it? 📝 Use a getter in the parent (preferred), or make it `protected`.
- When the loop calls `displayInfo()` (from `Employee`), which `calculatePay()` runs inside it? Why?
- Can you call `displayInfo(false)` on an element of the `Employee[]`? What would you need to do?

<details><summary>Hint 1</summary><code>Employee.displayInfo()</code> calls <code>calculatePay()</code>. Remember the two-types rule: which type decides which <code>calculatePay</code> runs?</details>
<details><summary>Hint 2</summary>The overload <code>displayInfo(boolean)</code> exists only in FullTimeEmployee. The array's reference type is Employee. See §9.5.</details>

Compare with `class4/e_practice_problem/solution/` when you're done.

---

## 9.8 Interview preparation — Module 9

1. **Overloading vs overriding** 🔴 (who decides, when, signatures, inheritance). A strong answer adds *why* each exists, and that overload choice uses the **reference** type while override choice uses the **object** type.
2. **Can you overload by return type alone? Why not?**
3. **What does `Animal a = new Dog(); a.makeSound();` do, and how does the JVM choose?** (Dynamic dispatch.)
4. **Why can't `a.fetch()` compile when the object really is a Dog?**
5. **What's a `ClassCastException`? How do you prevent it?**
6. **Can you override a static method? A private method? A final method?** 🔴 Explain method hiding.
7. **Can an overriding method reduce visibility or widen checked exceptions? Why not?** (Substitutability; this leads into LSP in M12.)
8. **Tricky:** `Parent p = new Child();` with same-named fields and static methods in both. What do `p.name`, `p.getName()`, and `p.util()` print?
9. **Tricky overloads:** which method gets `show(null)`, `show('a')`, `show(5L)`?
10. **Design:** you see a method with ten `instanceof` checks. What would you refactor it into?

---

## 9.9 Check yourself — Module 9

**Q1 (predict).**
```java
class Shape {
    String name = "shape";
    static String kind() { return "static-shape"; }
    String describe() { return "I am a shape"; }
}
class Circle extends Shape {
    String name = "circle";
    static String kind() { return "static-circle"; }
    @Override String describe() { return "I am a circle"; }
}
Shape s = new Circle();
System.out.println(s.name + " | " + s.kind() + " | " + s.describe());
```

**Q2 (compile, runtime error, or fine?).**
```java
Object o = new Circle();
Shape s1 = (Shape) o;          // (a)
Circle c1 = (Circle) s1;       // (b)
String str = (String) o;       // (c)
Circle c2 = new Shape();       // (d)
```

**Q3 (refactor).** Rewrite this using overriding so that adding a new shape requires no changes to `area()`:
```java
static double area(Object shape) {
    if (shape instanceof Circle c) return Math.PI * c.r * c.r;
    if (shape instanceof Square s) return s.side * s.side;
    return 0;
}
```

### Progress tracker — Module 9
- **Understand:** the two kinds of polymorphism; overload resolution; overriding rules; reference type vs object type; dynamic dispatch; up/downcasting; `instanceof`; what isn't polymorphic.
- **Can code:** overload sets; overriding hierarchies; loops over parent-type arrays; safe downcasts; the Payroll system.
- **Interview topics:** overloading vs overriding; static/private/final overriding; field and static hiding; ClassCastException.
- **Common mistakes:** expecting fields or statics to dispatch dynamically; calling child-only methods through parent references; overloading only by return type; casting without checking.
- **Prerequisites for M10:** class hierarchies (exceptions form one); `catch (ParentType e)` relies on upcasting.

### Exercises — Module 9

**🟢 Beginner.** The Payroll problem (§9.7).

**🟡 Intermediate.** `Notification` with `send(String msg)`, plus subclasses `EmailNotification`, `SmsNotification`, `PushNotification` that override it. Write `static void broadcast(Notification[] channels, String msg)`. Then add `SlackNotification` and confirm that `broadcast` needs **zero** changes. In two sentences, explain why. (You've just demonstrated OCP from M12.)

**🔴 Interview-level.** Predict, then run:
```java
class A { void m(A a) { System.out.println("A.m(A)"); } }
class B extends A {
    void m(B b) { System.out.println("B.m(B)"); }
    @Override void m(A a) { System.out.println("B.m(A)"); }
}
A x = new B();
B y = new B();
x.m(y);
y.m(y);
y.m(x);
```
Explain each line in two steps: (1) which overload the **compiler** picks, from the reference types; (2) which override the **JVM** runs, from the object type.
<details><summary>Hint 1</summary>For <code>x.m(y)</code>, the compiler only sees class A's methods, because x's reference type is A. Which <code>m</code> exists in A?</details>

---

# Module 10 — Exception Handling 🔴

> **Prerequisites:** M1 (compile time vs runtime), M4 (call stack), M8 (inheritance, `extends`), M9 (upcasting: catching a parent type catches its children).
> **You'll need this for:** every real program; validation in M7/M11 designs (throw instead of printing); try-with-resources for files and database connections.

---

## 10.1 What exceptions are and why they exist 🔴

### What is it?
📁📝 An **exception** is an event that **interrupts the normal flow** of a program when something unexpected happens:
- dividing by zero,
- calling a method on `null`,
- a missing file, or
- bad user input.

In Java, an exception is an **object**: an instance of a class that describes the problem.

### Why does it exist?
The old approach (in C) was returning error codes like `-1`. That has three problems:
- callers forget to check;
- the error code mixes with real return values; and
- the information about what went wrong is minimal.

Exceptions solve all three:
- 📁 **Robustness and graceful recovery.** You can handle a problem and keep going instead of crashing.
- **Separation.** The normal-path code stays clean, and the error handling lives in one place.
- **Information.** You get the type, a message, and a stack trace showing exactly where it happened.
- **They can't be silently ignored.** An unhandled exception stops the thread loudly.

### How it works: throwing and stack unwinding
When something goes wrong, an exception object is **thrown**. The JVM then:
1. abandons the current method;
2. walks **back down the call stack** (M4.3), frame by frame, looking for a matching `catch`; and
3. if it reaches the bottom of the stack without finding one, the thread dies and prints the **stack trace**.

📝 "If nobody catches it, the JVM ends the program with the exception."

---

## 10.2 The exception hierarchy 🔴

📁
```
                    Throwable
                   /         \
               Error          Exception
          (don't catch)      /          \
     OutOfMemoryError    (checked)       RuntimeException  (unchecked)
     StackOverflowError  IOException     ├─ NullPointerException
                         FileNotFoundException ├─ ArithmeticException
                         SQLException    ├─ ArrayIndexOutOfBoundsException
                         InterruptedException ├─ ClassCastException
                                         ├─ NumberFormatException
                                         ├─ InputMismatchException
                                         └─ IllegalArgumentException / IllegalStateException
```

| | Checked | Unchecked |
|---|---|---|
| **The rule (exact)** | `Exception` and subclasses, **except** `RuntimeException`'s branch | `RuntimeException`, `Error`, and their subclasses |
| Compiler forces handling? | **Yes**: catch it or declare `throws` | No |
| Typical meaning | Foreseeable external problems a well-written program should plan for (missing file, network down) | Usually **programming bugs** (null, bad index, bad cast) or invalid arguments |

- **`Error`** (📁) means serious JVM-level problems (out of memory, stack overflow) that you normally don't catch and can't really recover from.

➕ **Correction (Doc 5):** the notes define unchecked exceptions as "problems the compiler cannot predict". That's a useful *intuition*, but the actual rule is **just the class hierarchy**. The compiler checks `throws` declarations; it doesn't predict anything. For example, `FileNotFoundException` is checked because of where it sits in the hierarchy.

### Why does Java have checked exceptions at all? ➕ 🟠
They make failure part of a method's **contract**. `readFile() throws FileNotFoundException` tells every caller, at compile time, "this can fail; decide what to do."

They're controversial: they can force verbose code. Many modern libraries (and languages like Kotlin) prefer unchecked exceptions. Know both sides; interviewers ask.

---

## 10.3 `try`, `catch`, `finally` 🔴

### Syntax and flow
📁
```java
try {
    // code that might throw
} catch (SpecificException e) {
    // runs only if a matching exception was thrown in the try block
} finally {
    // ALWAYS runs: after try, or after catch. Used for cleanup.
}
```
- If nothing is thrown: the whole `try` runs, then `finally`, then execution continues.
- If something is thrown: the rest of `try` is **skipped**, the first matching `catch` runs, then `finally`, then execution continues after the whole statement.

📝 Example from class: catching division by zero and a null pointer, printing a friendly message, and carrying on instead of crashing.

### Multiple catch blocks: order matters (verified)
Java tries the catch blocks **top to bottom**, and **a parent type catches all its subclasses** (M9 upcasting). So more specific types must come first:
```java
try { ... }
catch (Exception e) { }
catch (RuntimeException e) { }   // error: exception RuntimeException has already been caught
```

### Multi-catch ➕
Handle several unrelated types the same way (verified):
```java
catch (ArithmeticException | NullPointerException e) { ... }
```

### `finally` facts ➕ 🔴 (verified)
- `finally` runs **even when `try` returns**:
  ```java
  static int f() { try { return 1; } finally { System.out.println("finally runs"); } }
  // prints "finally runs", then f() returns 1
  ```
- A `return` *inside* `finally` **overrides** the try's return, and silently swallows exceptions:
  ```java
  static int g() { try { return 1; } finally { return 2; } }   // returns 2
  ```
  Never return from `finally`.
- `finally` does **not** run if the JVM exits (`System.exit`) or the process is killed.

---

## 10.4 `throw` vs `throws` 🔴

| | `throw` | `throws` |
|---|---|---|
| What it is | A **statement** that raises an exception *now* | Part of a **method declaration** |
| Where | Inside a method body | After the parameter list |
| Followed by | One exception **object** | One or more exception **types** |
| Meaning | "Something is wrong; stop here" | "This method may throw these; callers must deal with them" |

```java
static void validate(int age) throws InvalidAgeException {        // throws: a warning on the label
    if (age < 18) throw new InvalidAgeException("Age " + age + " < 18");   // throw: actually raise it
}
```

📝 Doc 5's summary is worth memorizing:
- **`try-catch`** = handle it here.
- **`throws`** = pass the responsibility upward.
- **`throw`** = create and raise an exception.

📝 "`throws` doesn't solve the problem; it moves responsibility to the caller. If nobody catches it, the program still crashes."

---

## 10.5 Handle or declare (the checked exception rule) 🔴

📝 For checked exceptions you have exactly two choices. Not choosing is a compile error (verified):
```
error: unreported exception FileNotFoundException; must be caught or declared to be thrown
```
```java
// Choice 1: catch it
try {
    readFile();
} catch (FileNotFoundException e) {
    System.out.println("Config missing, using defaults");
}

// Choice 2: declare it and propagate
public void loadConfig() throws FileNotFoundException {
    readFile();
}
```
Even `main` can declare `throws Exception`. Any exception that reaches it then crashes the program with a stack trace, which is acceptable for small tools but not for servers.

---

## 10.6 Propagation and reading a stack trace 🔴

An exception travels up through callers until something catches it (verified):
```java
static void level3() { int[] a = new int[1]; a[5] = 1; }   // thrown here
static void level2() { level3(); }                          // not caught here...
// main: try { level2(); } catch (ArrayIndexOutOfBoundsException e) { ... }  // ...caught here
```
A stack trace reads **top = where it was thrown, downward = who called it**:
```
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 1
    at Demo.level3(Demo.java:2)     ← the actual failure line
    at Demo.level2(Demo.java:3)
    at Demo.main(Demo.java:5)
```
**Debugging habit:** read the exception type and message first, then find the **first line that belongs to your own code**.

---

## 10.7 Custom exceptions 🔴

### What and why
📝 Exceptions are just classes, so you can create domain-specific ones: `InvalidAgeException`, `InsufficientFundsException`, `CourseFullException`.

They make failures **self-describing**, and they let callers catch **exactly** the business problem they care about.

### Creating one
```java
public class InvalidAgeException extends Exception {          // checked
    public InvalidAgeException(String message) { super(message); }
    public InvalidAgeException(String message, Throwable cause) { super(message, cause); }
}

public class InsufficientFundsException extends RuntimeException {   // unchecked
    public InsufficientFundsException(String message) { super(message); }
}
```
**Checked or unchecked?** ➕
- Extend `Exception` when the caller can reasonably **recover** and should be forced to think about it (for example, "seat no longer available, choose another").
- Extend `RuntimeException` for violated preconditions and programming errors. Much modern code uses unchecked custom exceptions by default.

### Translating exceptions, and the correction to Doc 5 ➕ 🔴
📝 Doc 5 shows catching a low-level exception and throwing your own from the `catch` block, saying the original is "effectively replaced". That's only true **if you throw away the cause**, which destroys debugging information. **Always pass the original as the cause** (verified):
```java
static int parseAge(String input) throws InvalidAgeException {
    try {
        return Integer.parseInt(input);
    } catch (NumberFormatException e) {
        throw new InvalidAgeException("bad age input: " + input, e);   // keep e as the cause
    }
}
// caller: e.getMessage() → "bad age input: abc"
//         e.getCause()   → NumberFormatException (the original is preserved)
```
The stack trace then shows both, with the original under "Caused by:". This is called **exception chaining**.

📝 A custom exception thrown from a `catch` is a new exception like any other: it must be caught higher up or declared with `throws`.

---

## 10.8 try-with-resources ➕ 🟠
Files, network sockets, and database connections must be **closed**, even when errors happen. Before Java 7 that meant clumsy `finally` blocks. Now:
```java
try (Scanner sc = new Scanner(new File("learners.txt"))) {   // declared in parentheses
    while (sc.hasNextLine()) System.out.println(sc.nextLine());
} catch (FileNotFoundException e) {
    System.out.println("File not found");
}   // sc.close() is called automatically, whether or not an exception happened
```
It works with any class that implements the `AutoCloseable` interface (M11). This is how real code handles database connections.

---

## 10.9 Best practices ➕ 🟠
- **Never swallow exceptions:** `catch (Exception e) {}` hides bugs. At least log them.
- **Catch specific types**, not `Exception`, unless you're at a top-level boundary such as a request handler.
- **Don't use exceptions for normal control flow.** Check `if (list.isEmpty())` rather than catching an exception.
- **Validate early (fail fast)** with clear messages: `throw new IllegalArgumentException("amount must be > 0")`. This ties into KISS early returns (M12).
- **Don't catch `Error`.**
- In layered servers, translate low-level exceptions into domain ones *at boundaries*, keeping the cause.

**Encapsulation upgrade:** in M7, setters *printed* an error for invalid input. In real code they **throw** (`IllegalArgumentException`), so the caller can't miss the failure.

---

## 10.10 Apply it: Learner Manager, class 2 version 📁
Back to the M4 capstone. The class 2 requirements add:
- handle `InputMismatchException` when the user types letters for a number. ➕ After catching it, discard the bad token with `sc.nextLine()`, or the same input will be read again in an infinite loop.
- handle a full array (the repo suggests catching `ArrayIndexOutOfBoundsException`). ➕ Better design: check `count == MAX` *before* writing. Use exceptions for unexpected cases, not predictable ones.
- ➕ Throw and catch a custom `InvalidAgeException` for ages outside 18–100.

---

## 10.11 Interview preparation — Module 10

1. **Checked vs unchecked exceptions?** 🔴 Give the exact rule (hierarchy), examples, and *when you'd choose each* for a custom exception.
2. **`Error` vs `Exception`?** Should you catch `OutOfMemoryError`?
3. **`throw` vs `throws`?** 🔴
4. **Does `finally` run if `try` has a `return`? If `catch` throws? After `System.exit()`?**
5. **What happens if you return from `finally`?** (It overrides the return and swallows exceptions.)
6. **Why does `catch (Exception e)` before `catch (IOException e)` fail to compile?**
7. **How do you create a custom exception? Why pass a cause?** 🔴
8. **What is try-with-resources, and why is it better than `finally`?**
9. **Can an overriding method throw a broader checked exception than the parent's method?** (No. Explain using substitutability, M9.)
10. **Scenario:** your API returns HTTP 500 with "NullPointerException" for a missing user. How should this be designed instead? (A custom `UserNotFoundException`, mapped to 404.)

---

## 10.12 Check yourself — Module 10

**Q1 (predict).**
```java
static String test(int n) {
    try {
        System.out.print("A");
        int x = 10 / n;
        System.out.print("B");
        return "R1";
    } catch (ArithmeticException e) {
        System.out.print("C");
        return "R2";
    } finally {
        System.out.print("D");
    }
}
System.out.println(test(1));
System.out.println(test(0));
```

**Q2 (compile or not?).** Explain each:
```java
void a() { throw new IllegalStateException("x"); }       // (a)
void b() { throw new Exception("x"); }                    // (b)
void c() throws Exception { throw new IOException("x"); } // (c)
```

**Q3 (improve).** What's wrong with this, and how would you rewrite it?
```java
try {
    account.withdraw(amount);
} catch (Exception e) { }
```

**Q4 (design).** For a ticket-booking app, decide checked or unchecked for each, and justify:
- `SeatAlreadyBookedException`
- `NegativeQuantityException`
- `PaymentGatewayTimeoutException`

### Progress tracker — Module 10
- **Understand:** throwing and unwinding; the hierarchy; checked vs unchecked (the exact rule); try/catch/finally semantics; throw vs throws; handle-or-declare; propagation and stack traces; custom exceptions and chaining; try-with-resources.
- **Can code:** robust input handling; validation that throws; custom exception classes; chained exceptions; resource-safe code.
- **Interview topics:** checked/unchecked; throw/throws; finally edge cases; custom exceptions; catch order.
- **Common mistakes:** empty catch blocks; catching `Exception` everywhere; losing the cause; returning from `finally`; catching instead of preventing predictable errors.
- **Prerequisites for M11:** interfaces appear in `AutoCloseable`; hierarchies and abstraction come next.

### Exercises — Module 10

**🟢 Beginner.** Read two integers from the user and print their quotient. Handle non-numeric input and division by zero with different messages, and keep asking until the input is valid.

**🟡 Intermediate.** Upgrade your M7 `BankAccount`:
- `withdraw` throws a checked `InsufficientFundsException` whose message includes the shortfall; and
- `deposit` throws `IllegalArgumentException` for amounts ≤ 0.

Write a `main` that handles each one differently.

**🔴 Interview-level.** Write `static int parsePositiveAge(String raw) throws InvalidAgeException` that:
- trims the input;
- converts `NumberFormatException` into `InvalidAgeException` **with the cause preserved**; and
- rejects values outside 18–100.

Then write a caller that prints the message *and* the cause's class name. Finally, explain what your stack trace would lose if you hadn't passed the cause.

---

# Module 11 — Abstraction, Interfaces, Enums, Collections & Object Relationships 🔴

> **Prerequisites:** M8 (inheritance), M9 (polymorphism), M7 (encapsulation).
> **You'll need this for:** M12. Every SOLID principle is expressed through abstract classes and interfaces, and every design exercise uses collections and relationships.

---

## 11.1 Abstraction 🔴

📁 **Abstraction** means exposing **what** something does while hiding **how** it does it. 📁 The analogy from class: you drive a car without knowing how the engine works.

In Java you achieve it with **abstract classes** and **interfaces**. Both let code depend on a *general type* while the details live in subclasses. That's exactly what polymorphism (M9) needs.

---

## 11.2 Abstract classes 🔴

### What is it?
📝 An **abstract class** is a class marked `abstract` that **cannot be instantiated directly**. It can contain **abstract methods**: methods declared without a body, which subclasses must implement.
```java
abstract class Shape {
    protected String color;                         // fields allowed
    Shape(String color) { this.color = color; }     // constructors allowed
    abstract double area();                          // abstract: no body; children MUST implement
    void displayInfo() {                             // concrete: shared by all children
        System.out.println(color + " shape, area = " + area());
    }
}
class Circle extends Shape {
    private double r;
    Circle(String color, double r) { super(color); this.r = r; }
    @Override double area() { return Math.PI * r * r; }
}
```

### Why does it exist? 📝
Sometimes the parent knows **what** must exist but not **how**. Every vehicle must `start()` and `stop()`; every shape has an `area()`. But there's no sensible "generic shape area".

Plain inheritance (M8) can't **force** subclasses to implement something. A parent method with an empty body or a dummy return is a trap, because a child can forget to override it and nothing complains. 📝 "If you want to force behavior, use abstraction; an empty body is still a concrete method."

An abstract class gives you **shared code and state** (like a normal parent) **plus enforced specialization**.

### Rules 📝📁
- **Can't be instantiated.** Verified: `error: S is abstract; cannot be instantiated`.
  📝 Why: an object must be able to respond to all its methods, and `area()` has no implementation.
- **Can have constructors.** They run when a subclass is created, via `super(...)`. 📝 This is a common point of confusion: "can't be instantiated" doesn't mean "has no constructor".
- **Can mix abstract and concrete methods**, and can have fields of any kind.
- A concrete subclass **must implement every inherited abstract method**, or be declared `abstract` itself.
- **Abstract classes can be chained:** `Vehicle` (abstract) → `Car` (abstract) → `MarutiSuzukiCar` (concrete). 📝 This is useful when an intermediate level is still too general. Only the first *concrete* class must supply all missing implementations.
- **An abstract class doesn't need any abstract methods.** 📝 It can be abstract simply because the base type should never exist on its own. In the LMS exercise, `Learner` is abstract: there are Java learners and Node.js learners, but no "generic learner".

---

## 11.3 Interfaces 🔴

### What is it?
📝 An **interface** is a **contract**: a named set of method signatures that any class can promise to implement, using `implements`.
```java
interface Drawable {
    void draw();          // implicitly public and abstract
    double area();
}
class Circle implements Drawable {
    public void draw() { ... }     // must be public (see rules)
    public double area() { ... }
}
```

### Why does it exist?
1. **Multiple types without the diamond problem.** A class can `extend` only one class but `implement` **many** interfaces (M8.8). 📝 Examples: `Flyable`, `Swimmable`.
2. **Contracts for unrelated classes.** A `Document`, an `Image` and a `Chart` share no sensible parent class, but all can be `Printable`.
3. **Decoupling.** Code depends on the contract, not the class (DIP, M12). This is what makes `List<String> list = new ArrayList<>();` and swappable payment providers possible.

📝 Why `implements` and not `extends`? Because an interface is a promise to fulfil, not (mainly) a parent class you inherit state from.

### Rules (corrected and completed ➕) 🔴
- **Fields:** only constants. Every interface field is implicitly `public static final` (verified: `Config.MAX` is accessible as a constant).
- **Abstract methods** are implicitly `public abstract`. **The implementing method must be `public`**: you can't reduce visibility (M9.3). Forgetting `public` is a classic compile error.
- **No constructors**, and no instance state.
- 📝 **A class can implement many interfaces.** When two interfaces declare the same *abstract* method, one implementation satisfies both. No conflict.
- 📝 A method **inherited from a parent class can satisfy** an interface requirement.
- ➕ An interface can `extend` **multiple** interfaces.

### ➕ Correction: interfaces *can* contain code (Java 8+)
📝 Doc 8 says interfaces have no implementation. That's been outdated since 2014:
- **`default` methods** (Java 8) have bodies, and implementing classes inherit them. They were added so the JDK could add methods (like `List.forEach`) to existing interfaces without breaking every class that implements them.
- **`static` methods** (Java 8) are utility methods called through the interface name.
- **`private` methods** (Java 9) are helpers for default methods.

### ➕ Correction: default methods *can* conflict (verified)
If two interfaces provide a **default** method with the same signature, the class **must override it** and can choose or combine:
```java
interface Flyable   { default String move() { return "fly"; } }
interface Swimmable { default String move() { return "swim"; } }

class Duck implements Flyable, Swimmable {}        // error: types Flyable and Swimmable are incompatible

class Duck implements Flyable, Swimmable {
    public String move() { return Flyable.super.move() + "+" + Swimmable.super.move(); }   // "fly+swim"
}
```
This is Java's controlled answer to the diamond problem: **the class must decide explicitly**.

### Functional interfaces and lambdas ⚪ ➕
An interface with exactly one abstract method can be implemented with a **lambda**: `Runnable task = () -> System.out.println("hi");`. You'll meet this with threads (M13) and streams later. It's not required for this course.

---

## 11.4 Abstract class vs interface 🔴

| | Abstract class | Interface |
|---|---|---|
| Instantiate? | No | No |
| State (instance fields) | ✅ | ❌ (constants only) |
| Constructors | ✅ | ❌ |
| Concrete methods | ✅ | ✅ `default`/`static`/`private` (Java 8+/9+) |
| A class can have… | **one** parent class | **many** interfaces |
| Method visibility | Any | Abstract methods are public |
| Models | "is-a kind of", with shared implementation | "can do", a capability or contract |

### Decision rules 📝
- **Only a contract**, no shared state or code → **interface**. 📝 "If a type contains only abstract methods and no shared state, it's more natural as an interface."
- **Shared fields or code, plus required overrides** → **abstract class**. 📝 Doc 9: if every payment type needs the same `calculateGST()`, put it in an abstract base class.
- **Flexibility:** prefer interfaces when unsure. A class can implement many, but it only gets one parent, and using it up limits future design.
- **Both together is common:** `interface PaymentMethod` + `abstract class BasePayment implements PaymentMethod` (shared GST and validation) + concrete classes.

➕ 📁 The repo describes interfaces as "100% abstraction" and abstract classes as "0–100%". That phrasing predates Java 8 default methods. Use the decision rules above instead.

---

## 11.5 Enums ➕ 🟠

### What and why
📝 Doc 8 mentions enums for "finite, controlled values" (Java, Node.js, Python…) to avoid "inconsistent strings and misspellings". An **enum** is a type with a **fixed set of named instances**:
```java
enum Specialization { JAVA, NODE }
Specialization s = Specialization.JAVA;
```
With Strings, `"java"`, `"Java"` and `"JAVA "` are all different, and a typo compiles fine. With an enum, `Specialization.JAVAA` is a **compile error**. 📁 The repo's LMS `Course` stores `type` as a String `"online"`/`"offline"`; an enum would be safer.

### Enums are classes (verified)
```java
enum Specialization {
    JAVA("Java"), NODE("Node.js");                 // calls the constructor
    private final String label;
    Specialization(String label) { this.label = label; }
    String label() { return label; }
}
for (Specialization s : Specialization.values())
    System.out.println(s + " " + s.label() + " " + s.ordinal());   // JAVA Java 0 / NODE Node.js 1
Specialization.valueOf("JAVA");                    // String → enum (throws if invalid)

switch (s) {                                       // enums work in switch (M3)
    case JAVA -> System.out.println("java track");
    case NODE -> System.out.println("node track");
}
```
- Enums can't be instantiated with `new`. Each constant exists exactly once, so comparing with `==` is safe.
- 📝 Doc 8: "If the only difference between types is a label, use a field or enum. If they differ in structure or behavior, make separate classes."

---

## 11.6 Collections: List, Map, Set ➕ 🔴

Your notes mention collections in passing (📝 Doc 6, 📝 Doc 10 call them "more DSA than OOP"). **You can't do LLD without them**, so here's the essential core.

### Why
Arrays are fixed-size (M4). Real systems add and remove items constantly, and need fast lookup by ID.

### The core types
| Interface | Common implementation | Holds | Use when |
|---|---|---|---|
| `List<E>` | `ArrayList` | Ordered elements, duplicates allowed, index access | A sequence of items (an order's items, a cohort's learners) |
| `Map<K,V>` | `HashMap` | Key → value pairs, unique keys | **Lookup by ID** (instructors by id) |
| `Set<E>` | `HashSet` | Unique elements, no index | Deduplication, "have I seen this?" |

Verified behavior:
```java
List<String> names = new ArrayList<>();
names.add("Asha"); names.add("Bilal"); names.add("Asha");
names.size();          // 3 (duplicates allowed)
names.get(1);          // "Bilal"

Map<Integer, String> byId = new HashMap<>();
byId.put(101, "Rahul"); byId.put(102, "Sneha");
byId.put(101, "Rahul2");    // same key → replaces the value
byId.get(101);              // "Rahul2"
byId.get(103);              // null (missing key)
byId.containsKey(103);      // false

Set<String> unique = new HashSet<>(names);   // size 2

List<Integer> fixed = List.of(1, 2);
fixed.add(3);               // UnsupportedOperationException: List.of is immutable
```

### Generics in one paragraph
`<String>` is a **type parameter**: `List<String>` is a list that the compiler guarantees contains only Strings. No casts are needed, and type mistakes are caught at compile time. Type parameters must be **reference types**: `List<Integer>`, never `List<int>`. Autoboxing (M5.8) converts automatically.

### Program to the interface
```java
List<Learner> learners = new ArrayList<>();   // the variable type is the interface
```
You can switch to another `List` implementation later by changing one line. This is DIP (M12) in miniature.

➕ `HashMap` and `HashSet` rely on `hashCode()` and `equals()` (M5.9). Using your own class as a key requires overriding both, consistently.

---

## 11.7 Object relationships: association, aggregation, composition 🔴

These describe **how objects relate** to each other.

### Association: "uses-a" 📝📁
The loosest relationship. One object **uses** another, often just as a method parameter. There's **no ownership**, and each can exist without the other.
- 📝 A doctor sees a patient; the patient can see any doctor.
- 📁 A driver and a car.
```java
class Doctor {
    void treat(Patient p) { ... }   // uses p temporarily; doesn't keep or own it
}
```

### Aggregation: "has-a", weak ownership 📝📁
One object **contains** others, but the parts **exist independently** and can outlive the whole.
- 📝 A cricket team has players; disband the team and the players still exist.
- 📁 A library has books.
- **Code signal (📁):** the part is **created outside and passed in**.
```java
class Team {
    private List<Player> players = new ArrayList<>();
    void addPlayer(Player p) { players.add(p); }   // the player existed before and survives after
}
```

### Composition: "has-a", strong ownership 📝📁
The parts **belong to** the whole and **have no independent existence** in the model. Destroy the whole, and the parts go with it.
- 📝 A house has rooms.
- 📁 A car has an engine; a human has a heart.
- **Code signal (📁):** the part is **created inside** the whole and never handed out.
```java
class House {
    private final List<Room> rooms = new ArrayList<>();
    House(int n) { for (int i = 0; i < n; i++) rooms.add(new Room()); }   // created here, owned here
}
```

| | Association | Aggregation | Composition |
|---|---|---|---|
| Phrase | uses-a | has-a (weak) | has-a (strong) |
| Ownership | None | Weak | Strong |
| Part survives the whole? | Yes | **Yes** | **No** |
| Typical code | Method parameter | Passed into a field | Created inside |

📝 **The deciding question:** *can the part exist without the whole?* Yes → aggregation. No → composition.

### ➕ Java nuance: "destroyed" means unreachable
Java never destroys objects explicitly. "The rooms die with the house" really means: once the `House` is unreachable, its privately held `Room` objects become unreachable too, so the GC can collect them (M6.7).

So composition is a **modeling and encapsulation claim**, enforced by *you*:
- create the parts inside;
- never leak references to them; and
- copy data rather than sharing objects.

If `House` had `getRooms()` returning its internal list, outside code could keep the rooms alive, and the "composition" would quietly be broken.

### ➕ A debatable point in the repo's LMS solution
📁 `Cohort` labels its learners as **composition**, but `enrollLearner(Learner l)` receives learners **created outside** in `Main`. By the course's own code signal, that's aggregation. Arguably that's also more realistic: a learner can move to another cohort. **In interviews, state your reasoning; there isn't always one right label.** What matters is that you can justify the lifecycle you chose.

---

## 11.8 Low-level design (LLD) mindset 📝 🔴

### What each interview round tests
📝 Doc 8 frames three kinds of rounds:
- **DSA** tests whether you can write complex logic.
- **LLD** tests whether you can create **the right structure**: classes, abstractions, contracts, relationships.
- **HLD** tests distributed architecture: services, databases, scaling. 📝 Doc 1's load balancers, caches and queues live here.

📝 "LLD is about 'can you model the system cleanly?', not 'can you code the XP formula fastest?'"

### What a good LLD answer shows
1. **Entities** (classes) and their **fields**.
2. **Contracts**: method signatures, interfaces, abstract methods.
3. **Relationships**: inheritance, association, aggregation, composition.
4. **Abstractions** at the right places: abstract bases, interfaces, enums.

📝 It's fine to leave business formulas minimal. Show that the model *supports* them.

### Design decisions from class 📝
- **Attribute or separate class?**
  - A cosmetic difference (just a label) → a field or **enum**.
  - A **structural** difference (different fields or behavior) → separate classes. `JavaLearner` (assignments, projects) and `NodeJSLearner` (modules, quiz score) compute XP differently, so they're separate subclasses of an abstract `Learner`.
- **Entity vs management layer.** `Instructor` is one instructor. An `InstructorManager` (or "service", or "repository") **stores and retrieves many** of them. 📝 "Don't create `learner1`, `learner2`, `learner3`…" Use a collection:
  ```java
  class InstructorManager {
      private final Map<Integer, Instructor> byId = new HashMap<>();
      void add(Instructor i) {
          if (byId.containsKey(i.getId())) throw new IllegalArgumentException("Duplicate id " + i.getId());
          byId.put(i.getId(), i);
      }
      Instructor find(int id) { return byId.get(id); }
  }
  ```
  📝 It handles creation, storage, lookup, and lifecycle. It's the **in-memory stand-in for a database** (M12.11).
- **Unique IDs.** An `int`, a `String`, or a `UUID` all work. What matters is enforcing uniqueness (the map check above). 📝 "Don't get distracted by database-grade ID generation in LLD."
- **Keep `main` thin.** It wires objects together and runs them. It doesn't hold design responsibilities.

---

## 11.9 Practice problem — Airtribe LMS design 📁 🔴

**Specification** (`class5/f_practice_problem`):
- **Courses:** type (**online** / **offline**) and specialization (**Node.js** / **Java**).
- **Cohorts:** enrolled learners plus one assigned instructor.
- **Instructors:** a unique ID and a name.
- **Learners:** Node.js learners or Java learners, each with a unique ID, a name, and course-specific XP. They can calculate and display their XP. Each is assigned to the matching cohort.
- **Functionality:** the average XP of a cohort.
- **Display:** courses, cohorts, instructors, learners.
- **Concepts to demonstrate:**
  - abstract `Learner` with an abstract XP method;
  - interfaces where appropriate;
  - encapsulation;
  - inheritance;
  - composition, aggregation and association: decide and **justify** each, given §11.7's caveat.

**Your improvements over a minimal solution:**
- enums for course type and specialization;
- a management layer with ID-uniqueness checks;
- validation through exceptions (M10); and
- a guard so a learner can only join a cohort of the matching specialization. Where should that check live?

<details><summary>Hint 1: class list</summary>Course, Instructor, abstract Learner → JavaLearner / NodeJSLearner, Cohort, two enums, LearnerManager/InstructorManager, Main.</details>
<details><summary>Hint 2: where does the specialization check go?</summary>Whoever owns the invariant "a cohort only contains matching learners" should enforce it. Which class is that? (Encapsulation, M7.)</details>
<details><summary>Hint 3: polymorphism</summary><code>Cohort.averageXP()</code> should call <code>learner.calculateXP()</code> on a <code>List&lt;Learner&gt;</code> without any <code>instanceof</code>.</details>

Compare with `class5/f_practice_problem/solution/` when you're done.

---

## 11.10 Interview preparation — Module 11

1. **Abstract class vs interface: when do you use each?** 🔴 (Shared state and code → abstract class; pure contract or a capability across unrelated classes → interface; mention Java 8 defaults; one parent vs many interfaces.)
2. **Can an abstract class have a constructor? Why, if it can't be instantiated?**
3. **Can an interface have method bodies? Fields? Constructors?** (Defaults/static/private; constants only; no.)
4. **Two interfaces have the same default method. What happens?** 🔴
5. **Why must an implementing method be `public`?**
6. **Why use an enum instead of String constants?**
7. **Association vs aggregation vs composition, with examples and code signals.** 🔴 *Follow-up:* "In Java, how is a composed object actually destroyed?"
8. **`List` vs `Set` vs `Map`. Why declare `List<X> x = new ArrayList<>()`?**
9. **What does an LLD round evaluate?** How is it different from DSA and HLD?
10. **Design question:** model a parking lot's vehicles and spots in 5 minutes. State your entities, contracts, and relationships.

---

## 11.11 Check yourself — Module 11

**Q1 (compile or not? Fix what's broken).**
```java
interface Payable { double MAX = 1000; void pay(double amt); }
abstract class Base implements Payable { abstract void log(); }
class Upi extends Base {
    void pay(double amt) { System.out.println("UPI " + amt); }
    void log() { }
}
Payable p = new Base();
Payable.MAX = 2000;
```

**Q2 (classify and justify).** Association, aggregation, or composition?
- (a) `Order` and its `OrderLine`s
- (b) `Department` and `Employee`s
- (c) a `Mechanic` who `repair(Car c)`s
- (d) a `Playlist` and `Song`s
- (e) a `Document` and its `Page`s

**Q3 (design).** The requirement is: "Shapes: circle, rectangle, triangle. All have a color and must report area and perimeter. Some shapes (but not all) can be rotated." Which abstract classes and interfaces would you create, and why?

### Progress tracker — Module 11
- **Understand:** abstraction; abstract classes (rules and purpose); interfaces (classic plus Java 8+ features); default-method conflicts; decision rules; enums; core collections and generics; the three relationship types and their Java reality; the LLD mindset; entity vs manager.
- **Can code:** abstract hierarchies; interface-based designs; enums with fields; List/Map/Set usage; the LMS design.
- **Interview topics:** abstract vs interface; default conflicts; relationship types; LLD expectations.
- **Common mistakes:**
  - non-public interface implementations;
  - instantiating abstract types;
  - String "enums";
  - leaking composed parts;
  - labeling relationships without justification;
  - `learner1, learner2` variables instead of collections.
- **Prerequisites for M12:** interfaces and abstract classes as tools for extensibility.

### Exercises — Module 11

**🟢 Beginner.** Abstract `Shape` (a color; abstract `area()`; concrete `describe()`), with `Circle` and `Rectangle`. Add an interface `Resizable { void resize(double factor); }` implemented only by `Rectangle`. Store all the shapes in a `List<Shape>` and print their descriptions.

**🟡 Intermediate.** Build an `InstructorManager` backed by a `Map<Integer, Instructor>` with `add` (rejecting duplicates via an exception), `findById` (throwing a custom `InstructorNotFoundException`), and `listAll()` (returning an unmodifiable list).

**🔴 Interview-level.** The full LMS (§11.9) with all the improvements listed. Then write a half-page design note covering:
- each class's single responsibility;
- each relationship and its justification;
- one decision you'd change if learners could switch cohorts mid-course.

---

# Module 12 — Design Principles 🔴/🟠

> **Prerequisites:** M7 (encapsulation), M8–M9 (inheritance, polymorphism), M10 (exceptions), M11 (abstract classes, interfaces, collections).
> **You'll need this for:** LLD interviews, code reviews, judging AI-generated code, and M13.

---

## 12.1 What design principles are (and aren't) 🔴

### What and why
📝📁 **Design principles** are guidelines for writing code that is easy to **understand, change, extend, and reuse**.

📝 Bad code can still *work*. The cost shows up later:
- only the author understands it, and the author forgets;
- teammates struggle to onboard; and
- every change gets slower and riskier.

📝 "OOP is the foundation; design principles are the rules of good construction on top of it." Abstraction, interfaces, classes and inheritance are the tools. Principles tell you how to use them well.

📁 **Principles vs patterns:**
- Principles are *general guidelines* (DRY, SOLID).
- **Design patterns** are *specific, named, reusable solutions* to recurring problems (Singleton in M13, Strategy in §12.10).

### Principles are guidelines, not laws 📝
- **Right-size your flexibility.** 📝 A real-estate app that will only ever support *buy* and *rent* for years doesn't need a plugin architecture for hypothetical new transaction types.
- **Apply cheap principles early.** If a principle costs almost nothing (extracting a constant), just do it.
- **The workflow:** 📝 *write the code → inspect it → refactor → name the principle.* Principles work best as a **review checklist**, not as a recipe you must satisfy before typing.
- **Guardrails, not locks.** 📝 You can't make bad changes impossible. **Unit tests, code review, and normal SDLC discipline** are what catch them.
- **No magic line counts.** 📝 "A method must be under 50 lines" isn't a rule. Optimize for single responsibility and readability.

---

## 12.2 DRY — Don't Repeat Yourself 🔴

### What is it?
📝 Every piece of **knowledge or business logic** should live in **one place**. Call it from everywhere that needs it.

### Why?
📝 Doc 9's billing example: `printInvoice()` and `printReceipt()` both compute tax with `0.18`. When GST becomes 20%, you must find every copy. Miss one, and invoices and receipts disagree.

### Before → after
```java
// BEFORE: logic and the magic number are duplicated
void printInvoice(double amount) { double total = amount + amount * 0.18; ... }
void printReceipt(double amount) { double total = amount + amount * 0.18; ... }

// AFTER: one source of truth
private static final double GST_RATE = 0.18;
double calculateTax(double amount)   { return amount * GST_RATE; }
double calculateTotal(double amount) { return amount + calculateTax(amount); }
void printInvoice(double amount) { double total = calculateTotal(amount); ... }
void printReceipt(double amount) { double total = calculateTotal(amount); ... }
```
📁 In the Food Court solution: `Order.getSubtotal()` is the only place prices are summed, `BillCalculator.TAX_RATE` is the only place the tax rate appears, and `BillFormatter.money()` is the only place `%.2f` appears.

### Constants vs configuration 📝
"Should GST go in a config file?" *It depends:*
- **Config files** suit values that vary by environment or deployment: file paths, secret locations, external URLs, runtime settings.
- **Constants** suit fixed domain values that are part of the program's logic.

### ➕ Don't over-apply DRY
Two pieces of code that *look* the same but represent **different knowledge** (and will change for different reasons) should stay separate. Merging them couples unrelated features. DRY is about duplicated *knowledge*, not similar-looking text.

📝 **Rule of thumb:** if you're copy-pasting business logic, it should probably be extracted.

---

## 12.3 KISS — Keep It Simple 🔴

📝 Prefer the **clearest** solution that meets the need. KISS is **not** "fewest lines" or clever syntax. It's minimum cognitive load for the reader.

### Early returns / fail fast 📝 (the class example)
```java
// BEFORE: nested pyramid
void ship(Item item, int qty, boolean paid) {
    if (item != null) {
        if (!item.isEmpty()) {
            if (qty > 0) {
                if (paid) {
                    doShip(item, qty);
                } else { System.out.println("Payment pending"); }
            } else { System.out.println("Invalid quantity"); }
        } else { System.out.println("Empty item"); }
    } else { System.out.println("No item"); }
}

// AFTER: guard clauses, then the happy path
void ship(Item item, int qty, boolean paid) {
    if (item == null)    { System.out.println("No item"); return; }
    if (item.isEmpty())  { System.out.println("Empty item"); return; }
    if (qty <= 0)        { System.out.println("Invalid quantity"); return; }
    if (!paid)           { System.out.println("Payment pending"); return; }
    doShip(item, qty);
}
```
📝 Benefits:
- each check is independent and readable;
- the **happy path** is obvious;
- adding a new rule means adding one line; and
- the nested structure doesn't repeat.

➕ In production code, the guards would usually `throw` (M10) rather than print.

📁 The Food Court solution's `BillCalculator` stays KISS: "biggest discount wins", with no stacking rules and no priority engine.

---

## 12.4 YAGNI — You Aren't Gonna Need It 🔴

📝 Build what is required **now**. Don't add features "in case".
- Asked for email notifications? Build email, **not** SMS and push too. But keep the design *open* so they're easy to add later (OCP).
- A calculator that needs addition gets addition, not power and modulo "for later".

**Why:** 📝 "Unused code is not free." It must be maintained, it confuses readers, and it adds risk to every change.

📁 Food Court YAGNI: no `Payment`, `Customer`, `Coupon`, or `Database` classes, because nothing asked for them.

📝 **YAGNI and scale:** designing for 10,000 users when you might reach 100,000 is a high-level design question. Don't build caches and Redis now, but don't paint yourself into a corner either.

📝 **"Build for the future" doesn't mean "build the future now".**

---

## 12.5 S — Single Responsibility Principle 🔴

### What is it?
📝 **A class should have exactly one reason to change.** In other words, it should serve one responsibility, or one stakeholder's concern.

**SRP is not "one method per class".** 📝 An `EmployeeRepository` with `save`, `update` and `delete` has one responsibility (persistence) and many methods.

### Why?
When one class does many things, a change to one concern can break an unrelated one, and every change requires understanding everything. SRP **localizes change**.

### The class example 📝
A "god" `Employee` class that calculates salary, applies tax slabs, saves to the database, generates payslips, and sends emails. It has five reasons to change. Split it:

| Class | Responsibility | Changes when… |
|---|---|---|
| `Employee` | Entity: fields, constructor, getters | The employee data shape changes |
| `PayCalculator` | Salary computation | Pay rules change |
| `IncomeTaxRules` | Tax slabs and tax logic | Tax law changes |
| `EmployeeRepository` | Database save/update/delete | Storage changes (📝 MySQL → PostgreSQL → MongoDB touches **only** this class) |
| `PayslipGenerator` | Payslip formatting | The payslip layout changes |

📁 Food Court: `BillCalculator` changes for pricing rules, `BillFormatter` for receipt layout, and `Main` for where output goes. 📝 Doc 10: *building* the receipt and *printing* it are different responsibilities, so the same receipt can go to the console today and to a file tomorrow.

### SRP evolves 📝
It's fine to start with a class doing two closely related things. Split it when new responsibilities arrive. 📝 Pay calculation plus payslips is fine at first; add SMS, email, final-settlement statements and offer letters, and it's time to split.

"Good design isn't set on day one; it evolves through refactoring."

---

## 12.6 O — Open/Closed Principle 🔴

### What is it?
📝 Software should be **open for extension, closed for modification**. You add new behavior by **adding new code** (a new class), not by editing working, tested code.

### The smell: a growing if/else (or switch) on type 📝
```java
class PaymentProcessor {
    void process(String type, double amount) {
        if (type.equals("UPI"))          { /* UPI logic */ }
        else if (type.equals("CREDIT"))  { /* card logic */ }
        else if (type.equals("DEBIT"))   { /* debit logic */ }
        // every new payment type reopens this tested class
    }
}
```

### The fix: abstraction plus polymorphism (M9)
```java
interface PaymentMethod {
    void pay(double amount);
}
class UpiPayment implements PaymentMethod {
    public void pay(double amount) { System.out.println("UPI: " + amount); }
}
class CreditCardPayment implements PaymentMethod {
    public void pay(double amount) { System.out.println("Card: " + amount); }
}

class PaymentProcessor {                                   // closed: never edited again
    void process(PaymentMethod method, double amount) { method.pay(amount); }
}
// Tomorrow: class CryptoPayment implements PaymentMethod { ... }   ← extension only
```
📝 "Depend on the interface, not the concrete payment classes." The same shape fits booking providers (MakeMyTrip, Goibibo, Cleartrip), notification channels, and discount rules.

### Shared logic → an abstract class 📝
If every payment type needs the same GST calculation or the same validation (method allowed? valid in this country? card format OK?), put it in an abstract base class:
```java
abstract class BasePayment implements PaymentMethod {
    protected double calculateGst(double amount) { return amount * 0.18; }   // shared, written once (DRY)
}
class UpiPayment extends BasePayment {
    public void pay(double amount) { double total = amount + calculateGst(amount); ... }
}
```
📝 "No shared code → interface. Shared code → abstract class (often implementing the interface)."

### 📁 Food Court discounts: OCP in practice
`DiscountRule` is an interface with `getName()` and `discountFor(subtotal)`. `BillCalculator` loops over a `List<DiscountRule>` and keeps the biggest discount. Adding `LoyaltyDiscount` is **one new file plus one line in `Main`**, with **zero edits to `BillCalculator`**.

📝 Doc 10 also notes that the calculator must **evaluate every offer dynamically**. Never hard-code "use the bulk offer", because when the business changes the numbers, a different offer may win.

---

## 12.7 L — Liskov Substitution Principle 🔴

### What is it?
📝 A subtype must be usable **anywhere its parent type is expected without breaking the program's correctness**. It's not enough to compile. The child must **honor the parent's behavioral promise**.

### The classic violation 📝
```java
class Bird { void fly() { System.out.println("Flying"); } }
class Sparrow extends Bird { }
class Ostrich extends Bird {
    @Override void fly() { throw new UnsupportedOperationException("Ostriches can't fly"); }
}

void migrate(List<Bird> birds) { for (Bird b : birds) b.fly(); }   // breaks when an Ostrich is in the list
```
Code written against `Bird` trusted that `fly()` means flying. An `Ostrich` that throws, or silently does nothing, breaks that trust. 📝 The database analogy: an interface promises `delete()`. An implementation whose `delete()` does nothing throws no exception, but it's still a violation, because callers believe the data is gone.

📝 "Overriding isn't the problem. The problem is when a subclass can't honor what the parent promised."

### Fixes 📝
1. **Broaden the abstraction:** replace `fly()` with `move()`. Sparrows fly, ostriches run, penguins swim. Every bird can honor `move()`.
2. **Split the hierarchy by capability:** `Bird` → `FlyingBird` (has `fly()`) and `NonFlyingBird`. `Sparrow extends FlyingBird`; `Ostrich extends NonFlyingBird`. ➕ Or use a `Flyable` interface only flying birds implement (ISP, below).

### ➕ Other LSP smells
- An overriding method that **throws** `UnsupportedOperationException`.
- An empty override that does nothing.
- Callers that check `instanceof` before calling.
- A child that **requires more** (stricter input) or **promises less** (weaker output) than the parent.

The classic interview example is `Square extends Rectangle`: setting a square's width also changes its height, which breaks code that expects to set them independently. Java's own rules enforce part of LSP: you can't reduce visibility or widen checked exceptions when overriding (M9.3).

---

## 12.8 I — Interface Segregation Principle 🔴

### What is it?
📝 **Clients shouldn't be forced to depend on methods they don't use.** Prefer several small, role-specific interfaces to one "fat" interface.

### The class example 📝
```java
// BEFORE: a fat interface
interface Worker { void work(); void eat(); void sleep(); }
class RobotWorker implements Worker {
    public void work()  { ... }
    public void eat()   { }                                            // meaningless
    public void sleep() { throw new UnsupportedOperationException(); } // LSP violation, too
}

// AFTER: segregated
interface Workable  { void work(); }
interface Eatable   { void eat(); }
interface Sleepable { void sleep(); }
class HumanWorker implements Workable, Eatable, Sleepable { ... }
class RobotWorker implements Workable { ... }
```

### LSP vs ISP 📝
They're closely related but apply at different levels:
- **LSP** is about **inheritance and substitutability**: a child must honor the parent's behavior.
- **ISP** is about **interface size**: don't force implementers to promise behavior they can't provide.

A fat interface *causes* LSP violations (empty or throwing methods). 📝 Both aim for the same thing: "a class only commits to behavior it can genuinely fulfil."

---

## 12.9 D — Dependency Inversion Principle 🔴

### What is it?
📝 High-level modules (business logic) should not depend on low-level concrete modules (specific devices, databases, APIs). **Both should depend on abstractions.**

### The class example 📝
```java
// BEFORE: tightly coupled
class LightBulb { void turnOn() { ... } void turnOff() { ... } }
class Switch {
    private LightBulb bulb = new LightBulb();     // hard-wired to one concrete class
    void press(boolean on) { if (on) bulb.turnOn(); else bulb.turnOff(); }
}
// A switch for a fan? Edit Switch. For an AC? Edit it again.

// AFTER: both depend on an abstraction
interface Switchable { void turnOn(); void turnOff(); }
class SmartBulb implements Switchable { ... }
class Fan       implements Switchable { ... }
class Switch {
    private final Switchable device;
    Switch(Switchable device) { this.device = device; }      // dependency INJECTED from outside
    void press(boolean on) { if (on) device.turnOn(); else device.turnOff(); }
}
new Switch(new Fan()).press(true);
```
📝 The home-switch analogy: a real wall switch doesn't care whether a bulb or a fan is wired to it. It just sends on/off.

### Two terms ➕ (📁 the repo notes mention them)
- **Dependency Injection (DI):** pass dependencies in (usually through the constructor) instead of creating them inside with `new`. DI is the *technique*; DIP is the *principle*. Frameworks like Spring automate DI.
- **Why "inversion"?** Normally, high-level code depends *downward* on low-level details. With DIP, the low-level classes depend *upward* on an abstraction defined for the high-level code's needs. The direction of dependency is inverted.

**Huge practical benefit:** testing. You can inject a fake `Switchable` or a fake repository in unit tests.

📝 **Watch out:** with exactly one concrete implementation and no realistic need for more, an interface "for elegance" adds indirection without value. Abstract **when coupling actually hurts.**

---

## 12.10 Composition over inheritance 🔴

### The problem 📝
Inheritance models is-a and fixes structure top-down. When behaviors vary **independently**, the hierarchy explodes. 📝 The duck example:
- start with `Duck` having `swim()`, `quack()`, `fly()`;
- then a rubber duck squeaks and can't fly;
- a decoy duck is silent and can't fly; and so on.

swim/no-swim × quack/squeak/silent × fly/no-fly means up to 12 subclasses, plus overrides that disable inherited behavior (an LSP smell). 📝 "If new use cases keep forcing you to reshape the tree, inheritance is the wrong tool."

### The fix: has-a behavior objects 📝
```java
interface FlyBehavior   { void fly(); }
interface SoundBehavior { void makeSound(); }
class CanFly    implements FlyBehavior   { public void fly() { System.out.println("Flapping"); } }
class CannotFly implements FlyBehavior   { public void fly() { System.out.println("Can't fly"); } }
class Quack     implements SoundBehavior { public void makeSound() { System.out.println("Quack"); } }
class Squeak    implements SoundBehavior { public void makeSound() { System.out.println("Squeak"); } }
class Silent    implements SoundBehavior { public void makeSound() { } }

class Duck {
    private final FlyBehavior flyBehavior;          // has-a
    private final SoundBehavior soundBehavior;      // has-a
    Duck(FlyBehavior f, SoundBehavior s) { flyBehavior = f; soundBehavior = s; }
    void fly()       { flyBehavior.fly(); }          // delegate
    void makeSound() { soundBehavior.makeSound(); }
}
Duck mallard = new Duck(new CanFly(), new Quack());
Duck rubber  = new Duck(new CannotFly(), new Squeak());
```
New behavior means a new small class. **No change to `Duck`**, and no subclass explosion. This is the **Strategy pattern** (📁 the repo names it). Behavior can even be swapped at runtime if you provide a setter.

### Missing behavior 📝
A duck with no sound can be modeled as:
- a **`Silent` behavior** (➕ the *Null Object* pattern, preferred: no null checks anywhere);
- a separate class without that slot; or
- `null`, which forces null checks everywhere and is error-prone. Avoid it.

### When inheritance is still fine 📝
"Composition over inheritance doesn't mean *never* use inheritance." For a simple, stable is-a hierarchy, inheritance is clean.

📝 **Decision rules:**
- All variants share the **same shape** of operations (every payment method has validate/pay/refund) → a simple **interface** is enough.
- Behaviors vary **independently** and combine (the ducks) → **composition**.
- The hierarchy is simple and stable → **inheritance** is fine.

The behavior classes themselves can use interfaces or inheritance internally. The principle is about not forcing a messy *top-level* tree.

---

## 12.11 Objects vs databases 📝 🟠
- **Objects live in RAM** (the heap, M6). They exist for the duration of a request or process.
- **Persistent data lives in a database**, and survives restarts, other servers, and user sessions. 📝 If request 1 hits server A and request 2 hits server B, server B can only see your cart because it's *stored*, not because an object survived.
- **A typical request:** load data from the database → build or update objects → do the work → persist the changes.
- 📝 LLD focuses on objects and their behavior; persistence design belongs mostly to HLD. A management layer with a `HashMap` (M11.8) is the in-memory stand-in.
- 📝 GC frees objects based on **reachability**, never on business events (M6.7).

---

## 12.12 Practice problem — Food Court Billing 📁 📝 🔴

**Specification** (`class6/i_practice_problem`, 📝 Doc 10):
- A **menu item** has a name and price.
- An **order** is a list of menu items; 3 pizzas means the pizza is added 3 times.
- **Tax:** a flat **5% of the subtotal, before discount**.
- **Discounts:**
  - "Festival Offer": 10% off;
  - "Bulk Offer": flat 150 off when the subtotal is ≥ 1000;
  - **apply only the single best offer**;
  - **new offers must be addable without editing the bill calculator**.
- **Receipt:** every item, subtotal, discount (with the offer's name), tax, and total, all to **two decimals**.
- 📝 **Printing:** keep *building* the receipt separate from *outputting* it. Console today, file tomorrow; ➕ a `ReceiptPrinter` interface.

**Test cases (📁 from the repo's `Main`):**
- 2 burgers (150) + 3 colas (60): subtotal 480 → Festival (48) wins.
- 3 pizzas (400): subtotal 1200 → Bulk (150) beats Festival (120).
- 5 pizzas: subtotal 2000 → Festival (200) beats Bulk (150).

**Principles to show:**
- DRY (subtotal, tax rate, money formatting each in one place);
- KISS (best single discount wins);
- YAGNI (no unrequested classes);
- SRP (calculator / formatter / printer);
- OCP (the `DiscountRule` interface).

<details><summary>Hint 1: classes</summary>MenuItem, Order, DiscountRule (interface) with PercentageDiscount and BulkDiscount, Bill (a result holder), BillCalculator, BillFormatter, ReceiptPrinter (interface) with ConsolePrinter, Main.</details>
<details><summary>Hint 2: "best offer"</summary>Each rule returns 0 when it doesn't apply. Then the calculator only needs "keep the maximum", with no special cases.</details>
<details><summary>Hint 3: review your own solution</summary>Does any class both calculate AND print? Does adding a LoyaltyDiscount force an edit anywhere except Main? Does the subtotal get computed in two places?</details>

**➕ Review points on the repo's solution** (for after you've finished yours). Spotting these is exactly the "judge the code" skill:
- money is held in `double` (M1.4); a production system would use `long` paise or `BigDecimal`;
- `Bill.getItems()` returns the internal mutable list (an encapsulation leak, M7.4); and
- the tax rate is a constant inside `BillCalculator`. The repo's own stretch goal (per-category tax via a `TaxPolicy` interface) shows how OCP would handle tax changes, too.

---

## 12.13 AI and fundamentals 📝
From Docs 1 and 9:
- AI-assisted coding is the permanent new baseline.
- The scarce skill is **judging the output**: is the design extensible, are the abstractions right, does it violate SRP or OCP or LSP?
- If you can't explain the architecture of code you "wrote", you don't own it. You can't debug, extend, or maintain it.
- Interviews increasingly test fundamentals and design judgment, precisely because AI writes the boilerplate.

This module is the checklist you apply to every AI-generated class.

---

## 12.14 Interview preparation — Module 12

1. **Explain SOLID with one real example each.** 🔴 Interviewers want *examples and trade-offs*, not acronyms.
2. **Does SRP mean one method per class?** (No. One reason to change.)
3. **How does OCP relate to polymorphism? Show how you'd remove a growing if/else on payment type.** 🔴
4. **Give an LSP violation and two ways to fix it.** (Bird/Ostrich; Square/Rectangle.)
5. **LSP vs ISP.**
6. **DIP vs dependency injection.** Why does DIP make testing easier?
7. **Composition over inheritance: when, and why?** 🔴 Duck/Strategy; when inheritance is still fine.
8. **DRY vs over-abstraction.** When is duplication acceptable?
9. **YAGNI vs OCP: aren't they contradictory?** (No. YAGNI: don't build features you don't need. OCP: structure the code so features you *will* need are easy to add. Build for change, not for guesses.)
10. **Code review question:** given a 300-line `OrderService` that validates, prices, saves, emails, and logs, how would you refactor it, and in what order?

---

## 12.15 Check yourself — Module 12

**Q1 (name the violation).** Which principle is violated, and how would you fix it?
- (a) `ReportService` builds a report, formats it as PDF, and emails it.
- (b) `class Penguin extends Bird { void fly() { } }`.
- (c) `interface Printer { print(); scan(); fax(); }` implemented by a basic printer.
- (d) `OrderService` does `new MySqlOrderRepository()` in its constructor.
- (e) `if (shipping.equals("FEDEX")) … else if ("DHL") … else if ("BLUEDART")`.
- (f) The 18% GST rate appears in 9 files.

**Q2 (judge).** A teammate proposes an interface plus a factory for a utility that formats Indian phone numbers, "in case we go international". What do you say? Which principles are in tension?

**Q3 (design).** Notifications must go out via Email now; SMS is "maybe next quarter". Sketch a design that respects **both** YAGNI and OCP.

### Progress tracker — Module 12
- **Understand:** why principles exist; DRY/KISS/YAGNI; each SOLID principle with its smell and fix; composition over inheritance and Strategy; objects vs persistence.
- **Can code:** refactor if/else chains into interfaces; split god classes; constructor injection; behavior composition; the Food Court system.
- **Interview topics:** SOLID with examples; composition over inheritance; YAGNI vs OCP; DIP vs DI.
- **Common mistakes:**
  - "SRP = one method";
  - interfaces with one implementation "for the future";
  - DRY-merging unrelated code;
  - LSP violations through throwing overrides;
  - fat interfaces.
- **Prerequisites for M13:** static fields, private constructors, class initialization (M6.5), and the idea of shared state.

### Exercises — Module 12

**🟢 Beginner.** Refactor the nested `ship()` pyramid (§12.3) yourself, using exceptions (M10) instead of prints.

**🟡 Intermediate.** Here's a god class. Split it by SRP and apply DIP so the storage can be swapped:
```java
class LearnerService {
    void register(String name, int age) {
        if (age < 18) { System.out.println("too young"); return; }
        // save to a HashMap
        // send a welcome email via SMTP
        // print a welcome certificate
    }
}
```

**🔴 Interview-level.** The Food Court Billing system (§12.12), including a `ReceiptPrinter` with console and file implementations. Then **prove OCP**: add a `StudentDiscount` (15% capped at 100) and a `FilePrinter` **without editing** `BillCalculator` or `BillFormatter`. List the files you touched, and explain why that list demonstrates OCP and SRP.

---

# Module 13 — Design Patterns: Singleton & Thread-Safety Basics 🟠

> **Prerequisites:** M5 (static, private constructors), M6.5–6.6 (class initialization, shared heap, per-thread stacks), M12 (DIP).
> **You'll need this for:** understanding shared resources in servers (connection pools, configuration, loggers); concurrency topics later in your backend track.

---

## 13.1 What design patterns are 📝 🟠
📝 Design patterns are **standard, reusable solutions to recurring design problems**. They're different from:
- **OOP fundamentals**: access modifiers, inheritance, overloading… (the *building blocks*); and
- **design principles**: DRY, SOLID… (the *guidelines*).

📝 The analogy with DSA: once you recognize the *shape* of a problem, you reuse a known solution shape instead of inventing one. You've already met one pattern: **Strategy** (M12.10). This module covers **Singleton**.

---

## 13.2 Prerequisite: threads in 10 minutes ➕ 🟠

Your notes jump straight to "thread-safe singleton". Here's the missing foundation.

### What is a thread?
A **thread** is an independent path of execution inside a program. A server handles many requests **at the same time** by running them on different threads.

Recall M6.6:
- each thread has its **own stack** (its own locals); and
- **all threads share one heap** (the same objects, the same static fields).

```java
Thread t = new Thread(() -> System.out.println("in thread: " + Thread.currentThread().getName()));
t.run();     // in thread: main      ← just a normal method call on the current thread!
t.start();   // in thread: Thread-0  ← actually starts a new thread
t.join();    // wait for it to finish
```
(Verified. The `() -> ...` is a lambda implementing `Runnable`, M11.3 ⚪.) A classic interview trap: **`run()` doesn't create a thread; `start()` does.**

### Race conditions
When two threads read and write **shared** data without coordination, the result depends on unlucky timing. Here's a "check-then-act" race:
```
Thread 1: if (instance == null)   → true
Thread 2: if (instance == null)   → true    (Thread 1 hasn't created it yet!)
Thread 1: instance = new X();
Thread 2: instance = new X();               → TWO objects created
```

### `synchronized`: mutual exclusion 📝
A `synchronized` block lets **only one thread at a time** run the code inside it, for a given **lock object**:
```java
synchronized (SomeClass.class) {
    // critical section: one thread at a time
}
```
📝 The washroom-key analogy: one key; whoever holds it is inside; everyone else waits.
- The **lock object defines the boundary**: threads using the *same* lock exclude each other, and threads using *different* locks don't.
- 📝 The class object (`X.class`) is a common lock for static data.
- ➕ A `synchronized` block also guarantees **visibility**: changes made inside it are seen by the next thread that takes the same lock.

### `volatile` ➕ (needed for §13.7)
Two things threads normally *don't* guarantee:
1. **Visibility:** a write by one thread may not be immediately visible to another thread, because of CPU caches.
2. **Ordering:** the JVM and the CPU may reorder instructions for speed.

Marking a field **`volatile`** guarantees that every read sees the latest write, and it prevents the dangerous reorderings around that field.

---

## 13.3 Singleton: intent and motivation 🔴

### What is it?
📁📝 **Ensure a class has exactly one instance, and provide one global access point to it.**

### Why? 📝📁
Some objects represent **one shared resource or one shared state**:
- a database connection or connection pool,
- configuration settings,
- a logger, or
- an in-memory `UserService` holding all users.

📝📁 The class example (from the instructor's comments in `SingletonDemo.java`):
```
UserService us1 = new UserService();   // us1's list: u1, u2, u3, u4, u5
UserService us2 = new UserService();   // us2's list: u6, u7
→ the "single source of truth" is split in two. Lookups give inconsistent results.
```

### The three rules 📁
| Rule | How |
|---|---|
| Nobody outside can call `new` | **`private` constructor** |
| The one instance lives in the class | **`private static`** field |
| Everyone gets it the same way | **`public static getInstance()`** |

Why must `getInstance()` be **static**? 📝 To call an instance method you need an object, and getting the object is the very thing you're trying to do. A static method needs no object (M5.5). 📝 "A static `getInstance()` doesn't mean the object is created at load time; it only means you can call the method without an instance."

---

## 13.4 Variant 1 — Eager initialization 🔴
```java
public class AppConfig {
    private static final AppConfig INSTANCE = new AppConfig();   // created during class initialization
    private AppConfig() { }
    public static AppConfig getInstance() { return INSTANCE; }
}
```
- ✅ **Simple and thread-safe.** ➕ The reason (M6.5): the JVM runs class initialization **exactly once, under a lock**, so no two threads can create two instances.
- ❌ **Created even if never used.** That wastes memory or startup time if it's expensive.
- ❌ **Can't take runtime parameters**, because the instance is built at class initialization.

📝 The uppercase name `INSTANCE` is only a convention; `final` is what makes it a constant (M1.3).

---

## 13.5 Variant 2 — Lazy initialization (NOT thread-safe) 🔴
```java
public class LazySingleton {
    private static LazySingleton instance;          // null until first requested
    private LazySingleton() { }
    public static LazySingleton getInstance() {
        if (instance == null) {                     // check...
            instance = new LazySingleton();         // ...then act: a RACE
        }
        return instance;
    }
}
```
📝 Why lazy? Sometimes creation must wait for **runtime data**: user input, an API response, configuration loaded later. And it avoids creating unused objects.

**Proof that it breaks (verified).** 10 threads called `getInstance()` at the same time, with the constructor sleeping 50 ms to widen the race window:
```
naive lazy: constructor ran 10 times, distinct objects 10
```
Ten "singletons". 📝 The bug isn't just that one overwrites another. **Multiple objects were created at all**, and different threads may now hold different ones.

---

## 13.6 Variant 3 — Synchronized access 🟠
```java
public static LazySingleton getInstance() {
    synchronized (LazySingleton.class) {
        if (instance == null) instance = new LazySingleton();
    }
    return instance;
}
```
(📁 The repo's `getInstance1()`. Declaring the method `synchronized` is equivalent.)
- ✅ Correct.
- ❌ 📝 **Every call takes the lock, forever.** Creation happens once, but thousands of later calls still queue up one at a time, which hurts latency and throughput under load.

---

## 13.7 Variant 4 — Double-checked locking (DCL) 🔴 ➕ (correction)
📝 The idea: **lock only during creation.** Check without the lock first; if the instance is still null, take the lock and check **again**.

```java
public class ThreadSafeSingleton {
    private static volatile ThreadSafeSingleton instance;   // ← volatile is REQUIRED

    private ThreadSafeSingleton() { }

    public static ThreadSafeSingleton getInstance() {
        if (instance == null) {                              // 1st check: fast path, no lock
            synchronized (ThreadSafeSingleton.class) {
                if (instance == null) {                      // 2nd check: inside the lock
                    instance = new ThreadSafeSingleton();
                }
            }
        }
        return instance;
    }
}
```
Verified with the same 10-thread test:
```
DCL+volatile: constructor ran 1 times, distinct objects 1
```

**Why the second check?** Two threads can both pass the first check. One enters the lock and creates the instance. The second thread then enters, and **must** see that the instance now exists, or it would create a second one.

**Why `volatile`? ➕ (missing from your notes and from the repo's code: the classic interview trap)**
`instance = new X()` is really three steps:
1. allocate memory;
2. run the constructor; and
3. store the reference in `instance`.

Without `volatile`, steps 2 and 3 may be **reordered**. Another thread then hits the *unlocked* first check, sees a non-null `instance`, and uses an object **whose constructor hasn't finished**. `volatile` forbids that reordering and guarantees visibility.

**DCL without `volatile` is broken.** A rare, timing-dependent bug of the worst kind.

📝 **Expected behavior:** the very first burst of calls may briefly queue on the lock while the instance is created (a one-time latency blip). After that, every call takes the lock-free fast path.

---

## 13.8 Variant 5 — The holder idiom ➕ 🟠 (often the best answer)
```java
public class Registry {
    private Registry() { }
    private static class Holder {                       // not loaded until first used
        static final Registry INSTANCE = new Registry();
    }
    public static Registry getInstance() { return Holder.INSTANCE; }
}
```
It's **lazy** and **thread-safe** without `synchronized` or `volatile`. `Holder` is initialized only when `getInstance()` first touches it (lazy loading, M6.3), and class initialization is locked by the JVM (M6.5).

Verified: calling another static method of `Registry` didn't create the instance; the first `getInstance()` did.

➕ 🟡 **Enum singleton:** `enum Config { INSTANCE; ... }` is thread-safe, and it's also safe against serialization and reflection attacks (§13.9). It's recommended by *Effective Java*.

---

## 13.9 Pitfalls and criticism 📁 ➕ 🟡

**Ways a singleton can be broken** (📁 repo notes):
- **Serialization:** deserializing creates a new object. Fix it with `readResolve()` returning the instance.
- **Cloning:** override `clone()` to throw.
- **Reflection:** code can call the private constructor reflectively. ➕ Enums are immune to this.
- **Multiple class loaders** (rare; application servers): each loader has its own copy of the class, and therefore its own "singleton".

**Runtime parameters ➕:** 📁 the repo's `getInstance(int i)` takes a parameter. What if a second caller passes a *different* value? It's silently ignored, because the instance already exists. That's surprising API behavior. Prefer configuring once at startup, or reconsider whether it should be a singleton at all.

**Criticism ➕ (be ready to discuss):**
- A singleton is **global mutable state**: any code can reach it, so dependencies are hidden.
- It's **hard to unit test**: you can't easily swap in a fake. That tension is with DIP (M12.9).
- Modern frameworks get "one instance" without the pattern. In Spring, beans are **singleton-scoped by default** and **injected**, giving you single instances *and* DIP-friendly code.

📁 **Real-world uses:** logger, database connection pool, application configuration, thread-pool manager, cache.

---

## 13.10 Interview preparation — Module 13

1. **What is the Singleton pattern? How do you implement it?** (The three rules.)
2. **Eager vs lazy: trade-offs?** Why is eager thread-safe without `synchronized`? (Class initialization is locked, M6.5.)
3. **Show that naive lazy initialization is not thread-safe.** Walk through the interleaving.
4. **Why is synchronizing the whole `getInstance()` method inefficient?**
5. **Write double-checked locking. Why two checks? Why `volatile`?** 🔴 *Strong candidates explain the reordering of allocate / construct / assign.*
6. **What is the holder idiom, and why is it lazy and thread-safe?**
7. **How can a singleton be broken? How does an enum singleton prevent that?**
8. **Is Singleton an anti-pattern?** Discuss testability and DIP; mention Spring's singleton scope with DI.
9. **`run()` vs `start()` on a Thread?**
10. **What does `synchronized` guarantee? What does `volatile` guarantee? Is `volatile` alone enough for `count++`?** (No. `count++` is read-modify-write, which isn't atomic. ➕ Use `AtomicInteger` or `synchronized`.)

---

## 13.11 Check yourself — Module 13

**Q1 (spot the bugs).**
```java
public class Logger {
    public static Logger instance;
    public Logger() { }
    public Logger getInstance() {
        if (instance == null) instance = new Logger();
        return instance;
    }
}
```
There are at least three problems. Find them.

**Q2 (explain).** In DCL, a teammate removes the inner `if (instance == null)` "because we already checked". Describe the exact sequence of two threads that now creates two instances.

**Q3 (choose).** Which singleton variant would you pick in each case, and why?
- (a) A tiny, always-needed config object.
- (b) An expensive connection pool that some command-line tools never use.
- (c) A case where you must also survive serialization.

### Progress tracker — Module 13
- **Understand:** what patterns are; threads and shared heap; race conditions; `synchronized`; `volatile`; the singleton intent; eager, lazy, synchronized, DCL, holder and enum variants and their trade-offs; ways to break it; criticism.
- **Can code:** every variant; a multi-threaded test that exposes the race.
- **Interview topics:** DCL with `volatile`; why eager is safe; holder idiom; singleton vs DI.
- **Common mistakes:** public constructor; non-static `getInstance`; DCL without `volatile`; synchronizing every call; parameterized `getInstance`.

### Exercises — Module 13

**🟢 Beginner.** Implement an eager `AppConfig` singleton holding an app name and version. Prove `getInstance() == getInstance()`.

**🟡 Intermediate.** Implement a naive lazy singleton whose constructor sleeps 50 ms. Start 10 threads that each call `getInstance()` and collect the results into a `Set`. Print the set's size. Then fix it with DCL and with the holder idiom, and show that the size is 1 in both cases.
<details><summary>Hint 1</summary>A thread-safe set: <code>Collections.synchronizedSet(new HashSet&lt;&gt;())</code>. Start every thread first, then <code>join()</code> them all.</details>

**🔴 Interview-level.** Turn the in-memory `InstructorManager` from M11 into a singleton using the holder idiom. Then argue the *opposite* position: rewrite it as a normal class injected into the services that need it (DIP), and list two concrete testing benefits. Which would you choose in a Spring application, and why?

---

# ★ Final Review

## ★.1 Complete Java concept map

```
JAVA
├── PLATFORM ───────────────────────────────────────────────────────────── (M1, M6)
│   ├── .java ──javac──► bytecode (.class) ──JVM──► machine code
│   ├── JDK ⊃ JRE ⊃ JVM
│   ├── Compile time vs runtime ◄──────── reused by: exceptions, overloading/overriding, casting
│   └── JVM
│       ├── Class loading (Bootstrap ◄ Platform ◄ Application, parent-first delegation, lazy)
│       ├── Linking (verify → prepare [defaults] → resolve)
│       ├── Initialization (static fields + static blocks, once, parent first, locked) ──► eager/holder singleton
│       ├── Memory: Method area/Metaspace │ Heap (objects, shared) │ Stack (frames, per thread) │ PC │ Native
│       ├── GC (reachability from roots; pauses; leaks via lingering references)
│       └── Interpreter + JIT (hot spots, warm-up)
│
├── LANGUAGE BASICS ────────────────────────────────────────────────────── (M1–M4)
│   ├── Types: 8 primitives (values) │ references (addresses; null)
│   ├── Variables: local (stack, must init) │ instance (per object) │ static (per class)
│   ├── Conversion: widening (auto) │ narrowing (cast; truncation, wrap-around)
│   ├── Operators: integer division, %, overflow, ++/--, compound (+= hidden cast), &&/|| short-circuit, ?:
│   ├── Control flow: if/else ladders │ switch (fall-through; arrow form) │ for/while/do-while/for-each │ break/continue/return
│   ├── Methods: signature = name + params │ PASS-BY-VALUE (copies of values/references) │ call stack
│   └── Arrays (fixed size, heap objects, bounds-checked) │ Scanner input
│
├── OBJECTS ────────────────────────────────────────────────────────────── (M5)
│   ├── Class (blueprint) vs object (instance); new = allocate, zero, construct, return reference
│   ├── Constructors: default (vanishes if you write one) │ no-arg │ parameterized │ copy (shallow vs deep) │ this(...)
│   ├── this (current object) │ static vs instance methods (no `this` in static)
│   ├── == (identity) vs equals (content) │ String immutability + pool │ wrappers, autoboxing, Integer cache
│   └── Object: toString / equals / hashCode
│
├── THE FOUR PILLARS ───────────────────────────────────────────────────── (M7–M11)
│   ├── ENCAPSULATION (M7): packages/FQCN, imports, private/default/protected/public, invariants, behavior methods
│   ├── INHERITANCE (M8): extends, is-a, protected, super, constructor chaining, creation order, Object, no multiple inheritance
│   ├── POLYMORPHISM (M9): overloading (compiler) │ overriding (JVM, dynamic dispatch) │ reference type vs object type │ casting
│   └── ABSTRACTION (M11): abstract classes │ interfaces (+ default methods) │ enums
│
├── EXCEPTIONS ─────────────────────────────────────────────────────────── (M10)
│   └── Throwable → Error │ Exception → (checked) │ RuntimeException (unchecked); try/catch/finally; throw vs throws; custom + chaining; try-with-resources
│
├── MODELING ───────────────────────────────────────────────────────────── (M11)
│   ├── Collections: List / Map / Set + generics
│   ├── Relationships: association (uses-a) │ aggregation (weak has-a) │ composition (strong has-a)
│   └── LLD: entities, contracts, relationships, management layers
│
└── DESIGN ─────────────────────────────────────────────────────────────── (M12, M13)
    ├── DRY │ KISS │ YAGNI
    ├── SOLID: SRP │ OCP │ LSP │ ISP │ DIP (+ dependency injection)
    ├── Composition over inheritance (Strategy pattern)
    └── Patterns: Singleton (eager, lazy, sync, DCL + volatile, holder, enum) + threads, synchronized, volatile
```

## ★.2 Concepts you must master (🔴)
1. Primitive vs reference; what `=` copies; `null`.
2. **Pass-by-value** (including for object references).
3. Integer division, casting rules, overflow.
4. Control flow, including switch fall-through and loop choice.
5. Classes, objects, constructors (including the default-constructor rule), `this`, static vs instance.
6. `==` vs `.equals()`; String immutability.
7. Stack vs heap; GC reachability.
8. Encapsulation done properly: invariants and behavior methods.
9. Access modifiers, including `protected`.
10. Inheritance, `super`, constructor chaining, object creation order.
11. Overloading vs overriding; reference type vs object type; what isn't polymorphic.
12. Checked vs unchecked; try/catch/finally; throw vs throws; custom exceptions.
13. Abstract class vs interface (with Java 8+ features).
14. Association, aggregation, composition.
15. SOLID, DRY/KISS/YAGNI, composition over inheritance.
16. Singleton variants, especially DCL with `volatile`.

## ★.3 Concepts you can postpone (⚪/🟡)
- JVM tools (`jcmd`, `jvisualvm`)
- Generational GC details
- Symbolic resolution details
- `ClassNotFoundException` vs `NoClassDefFoundError` nuances
- Labeled breaks
- 2D arrays beyond the basics
- Static imports
- Lambdas and functional interfaces
- The enum singleton
- Serialization and reflection attacks on singletons
- Bitwise operators (`&`, `|` on ints)

## ★.4 Important relationships between concepts
| This… | …explains or enables this |
|---|---|
| References (M1) | Pass-by-value (M4), aliasing and copying (M5), encapsulation leaks (M7), GC reachability (M6) |
| Compile time vs runtime (M1) | Checked/unchecked exceptions (M10); overloading vs overriding (M9); casts that compile but fail (M9) |
| Widening/narrowing of primitives (M2) | Upcasting/downcasting of objects (M9): the same "safe = automatic, risky = explicit" logic |
| Default values (M1) | Zeroed objects in `new` (M5); preparation phase (M6); `label=null` in the creation-order trap (M8) |
| Call stack (M4) | JVM stack (M6); exception propagation and stack traces (M10); StackOverflowError |
| Static (M5) | Class initialization (M6); singleton (M13); why `main` is static; why statics aren't polymorphic (M9) |
| Access modifiers (M7) | Encapsulation (M7); `protected` in inheritance (M8); interface methods must be public (M11) |
| Inheritance (M8) | Polymorphism (M9); exception hierarchy and catch order (M10); abstract classes (M11); LSP (M12) |
| Polymorphism (M9) | OCP, DIP, Strategy/composition (M12); collections programmed to interfaces (M11) |
| Diamond problem (M8) | Interfaces and default-method conflict rules (M11) |
| Class initialization lock (M6) | Thread-safe eager and holder singletons (M13) |
| Shared heap, per-thread stacks (M6) | Race conditions, `synchronized`, `volatile` (M13) |
| Encapsulation (M7) | Composition's lifecycle guarantee (M11); SRP (M12) |

## ★.5 Most important interview topics (ranked by how often they come up)
1. Pass-by-value
2. `==` vs `equals`; String immutability and the pool
3. Overloading vs overriding; static/private/final methods; dynamic dispatch
4. Abstract class vs interface (including Java 8 defaults)
5. Checked vs unchecked exceptions; `finally` edge cases; throw vs throws
6. Stack vs heap; GC basics; memory leaks in Java
7. SOLID with examples; composition over inheritance
8. Constructor chaining, object creation order, `super`
9. Access modifiers (the `protected` subtlety)
10. Singleton with DCL and `volatile`; the holder idiom
11. Class loaders and parent delegation; static blocks
12. Association, aggregation, composition; LLD approach
13. Casting (primitive and object); integer division; overflow
14. Encapsulation: *why*, not just getters/setters

## ★.6 Revision checklist
Tick each item only when you can **explain it aloud without notes and write code for it**.

**Phase A**
- [ ] I can explain the compile → bytecode → JVM pipeline, and why the JVM is platform-specific.
- [ ] I know all 8 primitives, the literal suffix rules, and why `0.1 + 0.2 != 0.3`.
- [ ] I can draw what `int[] b = a;` does in memory.
- [ ] I can predict casts: `(int)-7.9`, `(byte)300`, `long → float` precision loss.
- [ ] I can predict `x++` vs `++x`, `x = x++`, and integer division results.
- [ ] I use `&&` short-circuiting for null checks correctly.
- [ ] I can explain switch fall-through and write an arrow switch expression.
- [ ] I can choose between for, while, do-while and for-each, and justify the choice.
- [ ] I can explain pass-by-value with `swap`, `zero`, and `replace`.
- [ ] I can write the Scanner menu loop without the `nextLine` bug.

**Phase B**
- [ ] I can list what `new` does, in order.
- [ ] I know when the default constructor disappears.
- [ ] I can write shallow and deep copy constructors and explain the difference.
- [ ] I can explain `this`, and why it's unavailable in static methods.
- [ ] I can explain the string pool and Integer caching traps.
- [ ] I can describe class loading, linking and initialization, with correct parent delegation.
- [ ] I can draw stack vs heap for a method that creates objects.
- [ ] I can explain GC reachability and give a Java memory-leak example.
- [ ] I can design a properly encapsulated class with behavior methods.
- [ ] I can explain all four access levels, including the protected-via-parent-reference rule.
- [ ] I can write the full object creation order for a parent/child pair with static and instance blocks.
- [ ] I can explain why calling overridable methods in constructors is dangerous.
- [ ] I can predict field hiding vs method overriding vs static hiding.
- [ ] I can trace overload resolution, including `null` and `char` arguments.
- [ ] I can write custom checked and unchecked exceptions with cause chaining.
- [ ] I know `finally`'s behavior with `return`.

**Phase C**
- [ ] I can choose between abstract class and interface, and resolve default-method conflicts.
- [ ] I use enums for closed sets.
- [ ] I can use `List`, `Map` and `Set` appropriately, programming to interfaces.
- [ ] I can classify relationships and justify the lifecycle I chose.
- [ ] I can explain each SOLID principle with a smell and a fix.
- [ ] I can refactor an if/else type chain into polymorphism.
- [ ] I can apply composition over inheritance (Strategy).
- [ ] I can write DCL with `volatile` and explain why it's needed, and write the holder idiom.

## ★.7 Cumulative coding exercises
These combine many modules. Attempt them after finishing the course, in order.

**C1 — Library Management (M1–M10).**
- Entities: `Book` (immutable ISBN, title, author), `Member`, `Loan`.
- Operations:
  - lend a book: fails if it's already lent, or the member has 3 loans (custom exceptions);
  - return a book;
  - list overdue loans.
- Store data in arrays first, then refactor to collections.
- Requirements: correct encapsulation, `equals`/`toString`, a console menu with robust input handling.

**C2 — Ride Fare Engine (M9, M11, M12).**
- Vehicle types: Bike, Auto, Cab, with different base fares and per-km rates.
- Surge pricing applies at certain hours.
- Promo codes: percentage and flat, best one wins.
- Adding a new vehicle type or promo must require **no edits** to the fare calculator. Prove it.
- Model the vehicle types, then justify enum vs subclasses vs strategy objects.

**C3 — Notification Service (M10–M13).**
- Channels: Email, SMS, Push behind an interface.
- A `NotificationService` with an injected channel list (DIP).
- A retry policy as a strategy.
- A singleton `NotificationConfig` using the holder idiom.
- Failures raise a custom exception with the cause chained.
- Write a short note explaining your SRP splits and why `NotificationService` is *not* a singleton.

**C4 — Mini Learner Platform, the capstone (everything).**
- Rebuild the LMS:
  - abstract `Learner` with Java/Node subclasses;
  - enums for course type and specialization;
  - `Cohort` enforcing matching specializations;
  - managers with ID uniqueness;
  - XP strategies that can be swapped per cohort;
  - a console menu;
  - custom exceptions throughout;
  - a report printer interface (console and file).
- Then write a one-page design review of your own code against DRY, KISS, YAGNI and SOLID, listing two things you'd change.

## ★.8 Mock Java interview (concepts from this course only)
Simulate it properly: set a timer, answer **out loud** or in writing **before** opening the "what the interviewer listens for" notes. Then send me your answers for detailed feedback.

**Round 1 — Fundamentals (15 min)**
1. Java is called platform-independent. Is every part of Java platform-independent?
   <details><summary>Listens for</summary>Bytecode is portable; the JVM is platform-specific; that's what makes it work.</details>
2. What's printed, and why? `int a = 7, b = 2; double d = a / b; System.out.println(d);`
   <details><summary>Listens for</summary>3.0: integer division happens before widening; fix with a cast on an operand.</details>
3. Is Java pass-by-reference for objects? Prove your answer with a short example.
   <details><summary>Listens for</summary>"Always pass-by-value; the reference is copied"; modifying through it is visible, reassigning isn't.</details>
4. `String s1 = "a"; String s2 = new String("a");`. What are `s1 == s2` and `s1.equals(s2)`, and why do companies ban `==` on Strings?
   <details><summary>Listens for</summary>Pool vs heap object; false/true; tests pass with literals, production input fails.</details>

**Round 2 — OOP (20 min)**
5. Walk me through exactly what happens, in order, when `new Child()` runs, where both classes have static blocks, instance blocks, and constructors.
   <details><summary>Listens for</summary>Parent static → child static → parent instance init + constructor → child field init + constructor; the overridable-call trap.</details>
6. `Parent p = new Child();` where both declare a field `name`, a static method `util()`, and an instance method `show()`. What runs for `p.name`, `p.util()`, `p.show()`?
   <details><summary>Listens for</summary>Parent field, Parent static, Child method, explained by binding time.</details>
7. Abstract class or interface for (a) a `PaymentMethod` contract, (b) a base class that computes GST for all payments? Could you use both together?
   <details><summary>Listens for</summary>Interface for the contract; abstract base implementing it for shared code; mentions Java 8 defaults and the single-parent limit.</details>
8. Explain encapsulation without using the words "getter" or "setter".
   <details><summary>Listens for</summary>Invariants, owning state, controlled operations, a single point of validation.</details>
9. A subclass in another package tries `otherAnimal.name` where `name` is protected and `otherAnimal` is typed `Animal`. Compiles?
   <details><summary>Listens for</summary>No; protected access across packages only via the subclass's own type.</details>

**Round 3 — Exceptions and the JVM (15 min)**
10. Checked vs unchecked: what decides it, and how do you choose for a custom `SeatUnavailableException`?
11. `try { return 1; } finally { return 2; }`: what's returned, and why is it bad practice?
12. Explain parent delegation in class loading and the security problem it solves.
13. Your service's latency spikes every few minutes under load. Explain the likely JVM-level cause and how you'd confirm it.
    <details><summary>Listens for</summary>GC pauses from allocation pressure; heap info and GC logs; reducing allocation and tuning.</details>

**Round 4 — Design (25 min)**
14. Here's `if (type.equals("UPI")) … else if ("CARD") … else if ("WALLET")`. Refactor it live, and name the principles.
15. `Penguin extends Bird` throws from `fly()`. What's wrong? Give two fixes.
16. Model ducks that may fly or not, and quack, squeak, or stay silent, without a subclass explosion.
17. LLD: design the core classes for a cohort-based learning platform. State entities, contracts, relationships (with justification), and where validation lives.
18. Implement a lazily created, thread-safe configuration singleton. Why is `volatile` needed in your version (or why isn't it)?
    <details><summary>Listens for</summary>DCL + volatile with the reordering explanation, or the holder idiom with the class-initialization guarantee; bonus for mentioning DI as an alternative.</details>

**Scoring yourself:** for each answer, ask:
- (1) Did I give the *mechanism*, not just the definition?
- (2) Did I give an example?
- (3) Did I mention a trade-off or edge case?

Three yeses is a strong answer.

## ★.9 Your weak areas
**I can't assess this yet:** you haven't answered any questions so far, and I won't guess. Here's how this section gets filled in:
1. Work through each module's **Check yourself** questions and the **mock interview**.
2. Send me your answers, module by module. Wrong answers are especially useful.
3. For each mistake, I'll identify the exact faulty reasoning, re-teach the underlying concept, and record it in a tracker like this one:

| Module | Question | What went wrong | Underlying concept to revisit | Re-tested? |
|---|---|---|---|---|
| | | | | |

**Areas most learners find weak** (watch these in yourself):
- pass-by-value with objects;
- object creation order;
- static vs dynamic binding (fields, statics, overload choice);
- `protected` across packages;
- `finally` with `return`;
- DCL with `volatile`; and
- justifying aggregation vs composition.

---
*Built from: 11 AI-generated note documents (9 in scope) and the BEL C23 Java code repository (133 files, classes 1–7). Every code sample and output was verified on Java 21.*

