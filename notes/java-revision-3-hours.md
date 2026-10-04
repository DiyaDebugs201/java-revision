# ☕ Java in 3 Hours: Rebuild, Practice, Retain

> A self-contained revision course for someone who has studied Java but forgot most of it.
> **Goal:** read Java again, write basic-to-intermediate Java, use it for DSA, understand OOP and Collections, and have a base for Spring Boot.

**How to use this file**

1. Follow the timetable. Use a timer.
2. Every section has: **Intuition → Technical → Code → Predict/Write → Answer**.
3. **Never open an answer before you attempt it.** Answers are inside `<details>` blocks. Write your guess first, even a wrong one.
4. Type the code in a real IDE (IntelliJ / VS Code). Do not just read it.
5. Revisit later using the **Cheat Sheet** (Section 15) and the **Revision Checklist** (Section 17).

**Bridge for ServiceNow people:** wherever helpful, a `🔗 ServiceNow bridge` box maps the idea to something you already know from ITSM / scripting.

---

## 📅 The 180-Minute Timetable

| Time | Min | Topic | Priority |
|---|---:|---|---|
| 0:00–0:15 | 15 | 1. Fundamentals | 🔴 Must |
| 0:15–0:28 | 13 | 2. Methods + core concepts (pass-by-value) | 🔴 Must |
| 0:28–1:00 | 32 | 3. OOP | 🔴 Must |
| 1:00–1:09 | 9 | 4. Access modifiers + Object class (`equals`/`hashCode`) | 🔴 Must |
| 1:09–1:20 | 11 | 5. Strings | 🔴 Must |
| 1:20–1:29 | 9 | 6. Arrays | 🔴 Must |
| 1:29–2:02 | 33 | 7. Collections | 🔴 Must |
| 2:02–2:07 | 5 | 8. Generics | 🟡 Important |
| 2:07–2:17 | 10 | 9. Exceptions | 🔴 Must |
| 2:17–2:30 | 13 | 10. Lambdas, Streams, Optional | 🟡 Important |
| 2:30–2:35 | 5 | 11. Comparable / Comparator | 🟡 Important |
| 2:35–2:38 | 3 | 12. Memory + JVM | 🟢 Overview |
| 2:38–2:43 | 5 | 13. Java for DSA toolkit | 🔴 Must |
| 2:43–2:45 | 2 | 14. Java → Spring Boot map | 🟢 Overview |
| 2:45–3:00 | 15 | 16. Final assessment | 🔴 Must |
| **Total** | **180** | | |

**Checkpoints**

- **60 min** (end of OOP): you should have finished Parts 1–3. If not, skip the 🟢/🟡 parts later, never Strings/Arrays/Collections/Exceptions.
- **120 min** (just before Exceptions): you should be done with Collections. If behind, shorten Generics, Streams, Comparator.
- **165 min:** stop learning. Only assessment.

Priority if time runs out: Fundamentals > Methods > OOP > Strings > Arrays > Collections > Exceptions > Generics > Lambda/Streams > DSA toolkit > JVM.

---

# 1. Java Fundamentals (15 min)

## 1.1 What Java is

**Intuition.** Java is a strict, statically typed language. Every variable has a declared type and the compiler checks it **before** running. In JavaScript you can write `let x = 5; x = "hi";`. In Java that is a compile error.

🔗 **ServiceNow bridge:** like field types on a table. An Integer field refuses text. Java enforces that rule at compile time.

## 1.2 JDK vs JRE vs JVM

```text
Hello.java  --javac-->  Hello.class (bytecode)  --JVM-->  runs on any OS
```

| Term | Meaning |
|---|---|
| **JVM** | The engine that executes bytecode. One per OS. |
| **JRE** | JVM + standard libraries. Enough to *run* Java. |
| **JDK** | JRE + compiler (`javac`) + tools. Needed to *develop*. |

**Why platform independent?** The compiler outputs **bytecode**, not machine code. Each OS has its own JVM that understands the same bytecode. "Write once, run anywhere."

## 1.3 Types

```java
// Primitives: store the value itself
byte b = 10;
short s = 100;
int age = 25;                 // default for whole numbers
long big = 10_000_000_000L;   // needs L
float f = 3.5f;               // needs f
double price = 99.5;          // default for decimals
char grade = 'A';             // single quotes
boolean done = true;

// Reference types: store an address to an object
String name = "Diya";
int[] marks = {80, 90, 70};
```

Only 8 primitives. Everything else is a reference type (String, arrays, your classes, collections).

**Defaults** (for fields and array elements, not local variables): numbers `0`, boolean `false`, char `'\u0000'`, references `null`. **Local variables have no default. You must assign them.**

## 1.4 Casting

```java
int x = 7;
double y = x;          // widening: automatic, safe
int z = (int) 9.8;     // narrowing: explicit, truncates -> 9
```

Direction: `byte → short → int → long → float → double`. Going right is automatic, going left needs a cast and may lose data.

## 1.5 Operators

- Arithmetic: `+ - * / %`. **Integer division truncates:** `10 / 4 == 2`, `10 / 4.0 == 2.5`, `-7 % 3 == -1`.
- Comparison: `== != < > <= >=`. Logical: `&& || !` (short-circuit).
- Increment: `i++` (use then add), `++i` (add then use).
- `==` on **primitives** compares values. On **references** it compares addresses (Section 5).

## 1.6 Input / Output

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String word = sc.next();
        sc.nextLine();                // consume leftover newline
        String line = sc.nextLine();
        System.out.println("n=" + n);
        System.out.printf("%d %s%n", n, word);
    }
}
```

Every Java program starts at `public static void main(String[] args)`.

## 1.7 Control flow

```java
if (x > 0) { ... } else if (x == 0) { ... } else { ... }

switch (day) {                 // classic
    case 1: System.out.println("Mon"); break;   // forget break => fall-through
    default: System.out.println("Other");
}

String r = switch (day) {      // modern (Java 14+): no fall-through, returns a value
    case 1, 7 -> "Weekend-ish";
    case 2 -> "Tue";
    default -> "Other";
};

for (int i = 0; i < 5; i++) { }
while (cond) { }
do { } while (cond);           // runs at least once
// break exits the loop, continue skips to the next iteration
```

## 1.8 Predict

**Q1.** What does this print?

```java
int x = 7;
double y = x;
int z = (int) 9.8;
System.out.println(y);
System.out.println(z);
System.out.println(10 / 4);
System.out.println(10 / 4.0);
```

<details><summary>Answer</summary>

```text
7.0
9
2
2.5
```
`(int)` truncates (does not round). `10 / 4` is integer division.
</details>

**Q2.** What does this print?

```java
int i = 5;
System.out.println(i++ + ++i);
```

<details><summary>Answer</summary>

`i++` gives 5 (then i=6), `++i` makes i=7 and gives 7. Result: `5 + 7 = 12`.
</details>

## 1.9 Write these (no peeking)

1. Even or odd for a number.
2. Largest of three numbers.
3. Factorial of `n` using a loop.
4. Reverse a number (`1234 → 4321`).
5. Print a right triangle pattern of `*` with `n` rows.

<details><summary>Solutions</summary>

```java
// 1
static boolean isEven(int n) { return n % 2 == 0; }

// 2
static int max3(int a, int b, int c) { return Math.max(a, Math.max(b, c)); }

// 3  (use long: factorial overflows int at 13!)
static long fact(int n) {
    long r = 1;
    for (int i = 2; i <= n; i++) r *= i;
    return r;
}

// 4
static int reverse(int n) {
    int rev = 0;
    while (n != 0) {
        rev = rev * 10 + n % 10;   // take last digit, append
        n /= 10;                   // drop last digit
    }
    return rev;
}

// 5
static void triangle(int n) {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) System.out.print("*");
        System.out.println();
    }
}
```
</details>

---

# 2. Methods + Core Concepts (13 min)

## 2.1 Methods

```java
// modifiers  returnType  name(parameters)
public static int add(int a, int b) {   // a, b = parameters
    return a + b;
}
int r = add(3, 4);                       // 3, 4 = arguments
```

- `void` = returns nothing.
- **Overloading:** same name, **different parameter list** (return type alone does not count). The compiler picks at compile time.

```java
static int area(int s) { return s * s; }
static int area(int l, int w) { return l * w; }
static double area(double r) { return Math.PI * r * r; }
```

## 2.2 Scope and variable kinds

| Kind | Where | Lives | Default value |
|---|---|---|---|
| **Local** | inside a method/block | until block ends | none, must assign |
| **Instance field** | inside class, no `static` | per object | yes |
| **Static field** | inside class, `static` | one copy per class | yes |

```java
class Counter {
    static int total = 0;   // shared by all Counter objects
    int count = 0;          // each object has its own

    void inc() { count++; total++; }
    static void reset() { total = 0; }  // no 'this', cannot touch instance fields directly
}
```

- `static` = belongs to the **class**, not an object. Call it as `Counter.reset()`.
- **A static method cannot access instance fields/methods directly** (no object to refer to). `main` is static for this reason: the JVM calls it before any object exists.
- `this` = reference to the **current object**. Used to separate field vs parameter names and to chain constructors.

```java
class Person {
    String name;
    Person(String name) { this.name = name; }   // this.name = field, name = parameter
}
```

## 2.3 ⭐ Java is ALWAYS pass-by-value

**Intuition.** When you call a method, Java **copies** whatever is in the variable. For a primitive, the copy is the value. For an object variable, the thing inside is an **address**, so the copy is a **copy of the address**. Two variables now point to the same object, but they are two separate variables.

Photocopying a house key: the copy opens the same house (you can rearrange the furniture), but if you throw away your copy and get a different key, the original owner's key is unchanged.

**Predict the output:**

```java
public class PassDemo {
    static void change(int n, int[] arr, StringBuilder sb, String s) {
        n = 99;
        arr[0] = 99;
        sb.append("!");
        s = "new";
    }

    static void reassign(int[] arr) {
        arr = new int[]{7, 7, 7};
        arr[0] = 1000;
    }

    public static void main(String[] args) {
        int n = 1;
        int[] arr = {1, 2, 3};
        StringBuilder sb = new StringBuilder("hi");
        String s = "old";

        change(n, arr, sb, s);
        System.out.println(n);
        System.out.println(arr[0]);
        System.out.println(sb);
        System.out.println(s);

        int[] other = {5, 5, 5};
        reassign(other);
        System.out.println(other[0]);
    }
}
```

<details><summary>Answer</summary>

```text
1        <- primitive copied; original unchanged
99       <- copy of the address, same array, so the change is visible
hi!      <- same StringBuilder object mutated
old      <- s = "new" only repoints the LOCAL copy; (and String is immutable anyway)
5        <- reassigning the local copy of the reference does nothing to the caller's variable
```
**Rule:** you can **mutate** an object through a passed reference, but you can never make the **caller's variable** point somewhere else.
</details>

---

# 3. OOP (32 min) 🔴

## 3.1 Class and object

**Intuition.** A **class** is a blueprint. An **object** is a thing built from it. Class `Student` = the form template, each `new Student(...)` = a filled-in record.

🔗 **ServiceNow bridge:** class ≈ table definition (dictionary); object ≈ one record (row); fields ≈ columns; methods ≈ behavior you attach (business rules / script includes).

```java
public class Student {
    // fields (state)
    private String name;
    private int marks;

    // constructor: runs on `new`, same name as class, no return type
    public Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }

    public Student(String name) {          // constructor overloading
        this(name, 0);                     // this(...) calls another constructor; must be 1st line
    }

    // methods (behavior)
    public String getName() { return name; }
    public int getMarks() { return marks; }
    public void setMarks(int marks) {
        if (marks < 0 || marks > 100) throw new IllegalArgumentException("bad marks");
        this.marks = marks;
    }
}

Student s = new Student("Asha", 88);
```

If you write **no** constructor, Java gives a free no-arg one. The moment you write any constructor, the free one disappears.

## 3.2 Encapsulation

**Intuition.** Hide the data, expose controlled doors. Fields `private`, access via `public` getters/setters, so the class can **validate** and later change its internals without breaking callers.

Above, `setMarks` refuses invalid input. If `marks` were `public`, anyone could set `marks = -500`.

## 3.3 Inheritance

**Intuition.** "IS-A": a Dog **is an** Animal, so it reuses Animal's features.

```java
class Animal {
    protected String name;
    Animal(String name) { this.name = name; }
    void sound() { System.out.println("..."); }
    void eat()   { System.out.println(name + " eats"); }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);               // call parent constructor (first line)
    }
    @Override
    void sound() { System.out.println("Woof"); }   // overriding
    void fetch() { System.out.println(name + " fetches"); }
}
```

- `extends` = inherit. Java allows **one** parent class (single inheritance).
- `super(...)` calls the parent constructor. `super.method()` calls the parent's version.
- **Override** = child gives its own version of a parent method (same name + same parameters). Use `@Override`: the compiler catches typos.
- Methods marked `final` can't be overridden. A `final` class can't be extended (`String` is final).

## 3.4 ⭐ Polymorphism

```java
Animal a = new Dog("Bruno");
a.sound();     // ???
a.eat();       // ???
a.fetch();     // ???
```

**Think first.** Two things exist here:

- **Reference type** (left side): `Animal`. Decides **what you are allowed to call** (compile time).
- **Object type** (right side): `Dog`. Decides **which version actually runs** (runtime).

<details><summary>Answer</summary>

- `a.sound()` → prints **Woof**. The method exists on `Animal` (compile OK), and at runtime the real object is a `Dog`, so Dog's override runs. This is **runtime polymorphism / dynamic dispatch**.
- `a.eat()` → "Bruno eats" (inherited, not overridden).
- `a.fetch()` → **compile error**. `Animal` doesn't have `fetch`, and the compiler only looks at the reference type. To call it: `((Dog) a).fetch();`
</details>

**Why it matters:**

```java
static void makeSound(Animal a) { a.sound(); }   // works for Dog, Cat, anything that IS-A Animal
List<Animal> zoo = List.of(new Dog("A"), new Cat("B"));
for (Animal x : zoo) x.sound();                  // each prints its own sound
```

One piece of code, many behaviors. This is how Spring Boot injects different implementations behind one interface.

**Compile-time vs runtime:**

| | Overloading | Overriding |
|---|---|---|
| Resolved | **compile time** (by argument types) | **runtime** (by actual object) |
| Where | same class (or subclass) | parent → child |
| Signature | **different** parameters | **same** name + parameters |

Fields and `static` methods are **not** polymorphic: they follow the reference type.

## 3.5 Abstraction: abstract class and interface

**Intuition.** Show *what*, hide *how*.

```java
abstract class Shape {                       // cannot do `new Shape()`
    String color;
    Shape(String color) { this.color = color; }
    abstract double area();                  // no body, children MUST implement
    void describe() { System.out.println(color + " area " + area()); }  // normal method
}

class Circle extends Shape {
    double r;
    Circle(String c, double r) { super(c); this.r = r; }
    @Override double area() { return Math.PI * r * r; }
}
```

```java
interface Payable {                          // a contract
    double pay();                            // implicitly public abstract
    default String currency() { return "INR"; }   // default method (Java 8)
    static void info() { System.out.println("Payable"); }
}

class Salaried implements Payable {
    public double pay() { return 50000; }    // must be public
}

class Person2 implements Payable, Comparable<Person2> { ... }   // many interfaces allowed
```

🔗 **ServiceNow bridge:** an interface is like an agreed Script Include API: callers depend on the method names, not on who implements them.

## 3.6 The comparison table

| Compare | Left | Right |
|---|---|---|
| **Abstract class vs interface** | Can have fields, constructors, state, concrete + abstract methods. A class extends **one**. Use for a shared base with common code ("is a kind of"). | A contract (mostly behavior). No instance state (only constants). A class implements **many**. Use for capabilities ("can do": `Comparable`, `Runnable`). |
| **Overloading vs overriding** | Same name, different params, compile time | Same signature in child, runtime |
| **Encapsulation vs abstraction** | **Hiding data** (private + getters) to protect it | **Hiding complexity**: expose only essentials (interfaces/abstract classes) |
| **Inheritance vs composition** | IS-A. `Dog extends Animal`. Tight coupling. | HAS-A. `Car` has an `Engine` field. Flexible, preferred. "Favor composition over inheritance." |
| **IS-A vs HAS-A** | `extends` / `implements` | A field holding another object |
| **Static vs instance** | One per class, no `this` | One per object |
| **`this` vs `super`** | Current object / its constructors | Parent part of the object / parent constructor |

## 3.7 Predict and write

**Q1.** Output?

```java
class A {
    void show() { System.out.println("A"); }
    static void hello() { System.out.println("A-hello"); }
}
class B extends A {
    @Override void show() { System.out.println("B"); }
    static void hello() { System.out.println("B-hello"); }
}
A obj = new B();
obj.show();
obj.hello();
```

<details><summary>Answer</summary>

`B` then `A-hello`. `show()` is overridden → runtime type. `static` methods are hidden, not overridden → decided by the **reference type** (A).
</details>

**Q2.** Will this compile? Why?

```java
abstract class X { abstract void f(); }
X x = new X();
```

<details><summary>Answer</summary>

No. Abstract classes cannot be instantiated. You could do `X x = new X() { void f() { } };` (anonymous subclass) or use a concrete child.
</details>

**Exercise.** Write an interface `Notifier` with `void send(String msg)`, and two classes `EmailNotifier`, `SmsNotifier`. Write a method `alertAll(List<Notifier> list, String msg)` that calls each one.

<details><summary>Solution</summary>

```java
interface Notifier { void send(String msg); }

class EmailNotifier implements Notifier {
    public void send(String msg) { System.out.println("Email: " + msg); }
}
class SmsNotifier implements Notifier {
    public void send(String msg) { System.out.println("SMS: " + msg); }
}

static void alertAll(List<Notifier> list, String msg) {
    for (Notifier n : list) n.send(msg);   // polymorphism
}
```
</details>

## 3.8 Interview questions (OOP)

1. Can an abstract class have a constructor? (Yes, called via `super(...)` from the child.)
2. Can you override a `private` or `static` method? (No. Private is invisible, static is hidden.)
3. Why no multiple class inheritance? (Diamond problem ambiguity. Interfaces allow multiple *type* inheritance.)
4. When do you use an interface over an abstract class? (Unrelated classes sharing a capability; need multiple.)
5. What does `final` do on a variable / method / class? (Can't reassign / can't override / can't extend.)

---

# 4. Access Modifiers + Object Class (9 min) 🔴

## 4.1 Access modifiers

| Modifier | Same class | Same package | Subclass (other package) | Everywhere |
|---|:-:|:-:|:-:|:-:|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *(default / package-private)* | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

**`final`:** variable can't be reassigned (a `final` reference can still have its **object mutated**), method can't be overridden, class can't be extended.

```java
final int[] a = {1, 2};
a[0] = 9;          // OK, object mutated
// a = new int[3]; // ERROR, reassigning the final reference
```

## 4.2 The `Object` class

Every class secretly `extends Object`. Three methods you must know:

- `toString()`: text form. Default prints `ClassName@hash`, useless. Override it.
- `equals(Object o)`: logical equality. Default is the same as `==` (same object).
- `hashCode()`: an int "bucket number" used by hash-based collections.

## 4.3 ⭐ equals() → hashCode() → HashMap/HashSet

**Intuition.** A `HashMap` is a building with numbered rooms (**buckets**).

```text
put/get(key):
  1. hashCode(key)  -> picks the ROOM        (fast, narrows the search)
  2. equals(key)    -> checks WHICH item in that room is really the same key
```

If two objects are `equals()` but have **different** `hashCode()`s, they land in **different rooms**, and the map never compares them, so you get duplicates and failed lookups.

**The contract:** `a.equals(b)` ⇒ `a.hashCode() == b.hashCode()`. (The reverse is not required: different objects may share a hash, called a collision.) **Override both together, using the same fields.**

```java
class Point {
    final int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Point)) return false;
        Point p = (Point) o;
        return x == p.x && y == p.y;
    }

    @Override
    public int hashCode() { return java.util.Objects.hash(x, y); }

    @Override
    public String toString() { return "(" + x + "," + y + ")"; }
}
```

**Predict:** With only `equals` overridden (no `hashCode`), what is `set.size()`?

```java
Set<Point> set = new HashSet<>();
set.add(new Point(1, 2));
set.add(new Point(1, 2));
System.out.println(set.size());
```

<details><summary>Answer</summary>

Most likely **2** (bug): the default `hashCode` differs per object, so they go in different buckets and `equals` is never consulted. With both overridden, it's **1**. Records (`record Point(int x, int y) {}`, Java 16+) generate `equals/hashCode/toString` for you.
</details>

**Also:** mutable fields used in `hashCode` + changing them after inserting into a HashSet/HashMap key = the element becomes "lost". Use immutable keys (`String`, `Integer`, records).

---

# 5. Strings (11 min) 🔴

## 5.1 Immutability and the pool

**Intuition.** A `String` never changes. Operations like `toUpperCase()` return a **new** String. Because strings are immutable, Java can safely **share** identical literals in a **String pool** (a cache inside the heap).

```java
String a = "hello";            // pool
String b = "hello";            // same pooled object
String c = new String("hello");// new object on the heap, outside the pool
System.out.println(a == b);        // ???
System.out.println(a == c);        // ???
System.out.println(a.equals(c));   // ???
System.out.println(a == c.intern());// ???
```

<details><summary>Answer</summary>

`true`, `false`, `true`, `true`.
`==` compares **addresses**; `equals` compares **content**. **Always use `.equals()` for Strings.** `intern()` returns the pooled instance.
</details>

## 5.2 Concatenation traps

```java
System.out.println("a" + 1 + 2);   // ???
System.out.println(1 + 2 + "a");   // ???
String s = "x";
s.concat("y");
System.out.println(s);             // ???
```

<details><summary>Answer</summary>

`a12`, `3a`, `x`. Evaluation is left to right. And `concat` returned a new String that was thrown away (immutability).
</details>

## 5.3 StringBuilder / StringBuffer

String `+` in a loop creates a new object every time → O(n²). Use **StringBuilder** (mutable, not thread-safe, fast). **StringBuffer** = synchronized (thread-safe) version, slower, rarely needed.

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 5; i++) sb.append(i).append(',');
sb.setLength(sb.length() - 1);   // remove trailing comma
sb.reverse();
sb.insert(0, "[").append("]");
sb.toString();
```

## 5.3 Common methods

```java
s.length()  s.charAt(i)  s.substring(a, b)   // b exclusive
s.indexOf("x")  s.contains("x")  s.startsWith("x")  s.endsWith("x")
s.toUpperCase()  s.toLowerCase()  s.trim()  s.strip()  s.isEmpty()  s.isBlank()
s.replace("a","b")  s.split(",")  String.join("-", list)
s.toCharArray()   String.valueOf(charArray)   s.equalsIgnoreCase(t)   s.compareTo(t)
```

## 5.4 Write these

1. `isPalindrome(String s)`
2. Count vowels in a string.
3. `isAnagram(String a, String b)`

<details><summary>Solutions</summary>

```java
static boolean isPalindrome(String s) {
    int i = 0, j = s.length() - 1;
    while (i < j) {
        if (s.charAt(i++) != s.charAt(j--)) return false;
    }
    return true;
}

static int countVowels(String s) {
    int count = 0;
    for (char c : s.toLowerCase().toCharArray())
        if ("aeiou".indexOf(c) >= 0) count++;
    return count;
}

static boolean isAnagram(String a, String b) {
    if (a.length() != b.length()) return false;
    int[] freq = new int[26];                  // assumes lowercase a-z
    for (int i = 0; i < a.length(); i++) {
        freq[a.charAt(i) - 'a']++;
        freq[b.charAt(i) - 'a']--;
    }
    for (int f : freq) if (f != 0) return false;
    return true;
}
```
`c - 'a'` converts a char to an index 0–25. Useful constantly in DSA.
</details>

---

# 6. Arrays (9 min) 🔴

## 6.1 Basics

```java
int[] a = new int[5];           // [0,0,0,0,0], size fixed forever
int[] b = {3, 1, 4, 1, 5};
int[][] grid = new int[3][4];   // 3 rows, 4 cols
int[][] m = {{1,2},{3,4}};
a.length                         // field, no ()   (String uses length())
m.length  m[0].length            // rows, cols

for (int i = 0; i < b.length; i++) { }
for (int x : b) { }              // enhanced for, read-only view of each element
```

Out of range → `ArrayIndexOutOfBoundsException`. Printing an array directly gives garbage (`[I@1b6d3586`). Use `Arrays.toString(b)` / `Arrays.deepToString(m)`.

## 6.2 `Arrays` utility

```java
import java.util.Arrays;
Arrays.sort(b);                          // ascending, in place (dual-pivot quicksort for primitives)
Arrays.sort(b, 1, 4);                    // range
Arrays.fill(a, -1);
int[] copy  = Arrays.copyOf(b, b.length);
int[] part  = Arrays.copyOfRange(b, 1, 4);
Arrays.equals(x, y);                     // content equality (x.equals(y) is address equality!)
Arrays.binarySearch(b, 4);               // array must be sorted
Arrays.asList(1,2,3);                    // fixed-size list view (works on Integer[] not int[])
Arrays.stream(b).sum();
```

**Common mistakes:** `int[] c = b;` does **not** copy, both point to the same array. Sorting primitives with a comparator doesn't compile: use `Integer[]` for that.

## 6.3 Write these

1. Max of array. 2. Reverse in place. 3. Frequency count. 4. Second largest. 5. Two-pointer: in a **sorted** array, find a pair with sum = target.

<details><summary>Solutions</summary>

```java
static int max(int[] a) {
    int m = a[0];
    for (int x : a) if (x > m) m = x;
    return m;
}

static void reverse(int[] a) {
    for (int i = 0, j = a.length - 1; i < j; i++, j--) {
        int t = a[i]; a[i] = a[j]; a[j] = t;
    }
}

static Map<Integer,Integer> freq(int[] a) {
    Map<Integer,Integer> f = new HashMap<>();
    for (int x : a) f.put(x, f.getOrDefault(x, 0) + 1);
    return f;
}

static int secondLargest(int[] a) {             // one pass
    int first = Integer.MIN_VALUE, second = Integer.MIN_VALUE;
    for (int x : a) {
        if (x > first) { second = first; first = x; }
        else if (x > second && x < first) second = x;
    }
    return second;
}

static int[] twoSumSorted(int[] a, int target) {  // O(n)
    int i = 0, j = a.length - 1;
    while (i < j) {
        int s = a[i] + a[j];
        if (s == target) return new int[]{i, j};
        if (s < target) i++; else j--;
    }
    return new int[]{-1, -1};
}
```
</details>

---

# 7. Collections (33 min) 🔴🔴

## 7.1 Big picture

```text
Iterable
  └── Collection
        ├── List   (ordered, duplicates OK, index access)   ArrayList, LinkedList
        ├── Set    (no duplicates)                           HashSet, LinkedHashSet, TreeSet
        └── Queue  (processing order)                        LinkedList, ArrayDeque, PriorityQueue
              └── Deque (both ends)                          ArrayDeque, LinkedList

Map<K,V> is SEPARATE (key → value)                           HashMap, LinkedHashMap, TreeMap
```

Always **program to the interface**: `List<Integer> list = new ArrayList<>();`. You can swap implementations later.

🔗 **ServiceNow bridge:** a `List` is like iterating a GlideRecord result set; a `Map` is like a lookup by `sys_id`; a `Set` is "give me the unique values".

## 7.2 List

```java
List<Integer> list = new ArrayList<>();
list.add(5);            list.add(0, 9);        // add at index (shifts)
list.get(0);            list.set(0, 7);
list.remove(0);                                 // by INDEX
list.remove(Integer.valueOf(5));                // by VALUE (trap!)
list.size();  list.isEmpty();  list.contains(5);  list.indexOf(5);
list.clear();
List<Integer> fixed = List.of(1, 2, 3);         // immutable
Collections.sort(list);  Collections.reverse(list);
Collections.max(list);   Collections.min(list);
```

| | **ArrayList** | **LinkedList** |
|---|---|---|
| Inside | resizable array | doubly linked nodes |
| `get(i)` | **O(1)** | O(n) |
| add at end | O(1) amortized | O(1) |
| insert/remove middle | O(n) (shift) | O(1) *once you're at the node*, but finding it is O(n) |
| Memory | compact | extra pointers per node |
| Use | **default choice 95% of the time** | rarely; maybe as a Queue/Deque, but `ArrayDeque` is usually better |

## 7.3 Set

| | **HashSet** | **LinkedHashSet** | **TreeSet** |
|---|---|---|---|
| Order | none guaranteed | **insertion order** | **sorted** |
| add/contains/remove | O(1) avg | O(1) avg | **O(log n)** |
| Needs | `equals` + `hashCode` | same | `Comparable` or `Comparator` |
| Extra | | | `first()`, `last()`, `floor()`, `ceiling()`, `headSet()` |

```java
Set<String> users = new HashSet<>();
boolean isNew = users.add("asha");   // false if already present -> great for duplicate detection
```

## 7.4 Map

```java
Map<String, Integer> m = new HashMap<>();
m.put("a", 1);
m.get("a");                       // null if missing
m.getOrDefault("z", 0);
m.containsKey("a");  m.containsValue(1);
m.remove("a");
m.putIfAbsent("b", 2);
m.merge("a", 1, Integer::sum);    // frequency counting one-liner
m.computeIfAbsent("k", k -> new ArrayList<>()).add(5);   // grouping pattern (value is a List)
for (Map.Entry<String,Integer> e : m.entrySet()) {
    e.getKey(); e.getValue();
}
m.keySet();  m.values();
```

| | **HashMap** | **LinkedHashMap** | **TreeMap** |
|---|---|---|---|
| Order | none | insertion (or access order → LRU cache) | sorted by key |
| get/put | O(1) avg | O(1) avg | **O(log n)** |
| Null key | 1 allowed | 1 allowed | not allowed |
| Extra | | | `firstKey()`, `floorKey()`, `ceilingKey()`, `headMap()` |

`HashMap` is not thread-safe (use `ConcurrentHashMap` in multi-threaded code). Keys must have proper `equals/hashCode` (Section 4.3).

## 7.5 Queue, Deque, PriorityQueue

```java
Queue<Integer> q = new LinkedList<>();   // or ArrayDeque (faster)
q.offer(1);  q.poll();  q.peek();        // add, remove head (null if empty), look at head

Deque<Integer> dq = new ArrayDeque<>();
dq.addFirst(1); dq.addLast(2); dq.pollFirst(); dq.pollLast(); dq.peekFirst(); dq.peekLast();

// Stack (DON'T use the old Stack class):
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);  stack.pop();  stack.peek();   // LIFO

// Heap
PriorityQueue<Integer> minHeap = new PriorityQueue<>();                          // smallest first
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());// largest first
minHeap.offer(5); minHeap.poll(); minHeap.peek();   // offer/poll O(log n), peek O(1)
```

| | Behavior |
|---|---|
| **Queue** | FIFO line |
| **Deque** | add/remove at **both** ends → works as stack AND queue |
| **PriorityQueue** | always gives the **smallest** (or by comparator) element first, not sorted when iterated |

🔗 **ServiceNow bridge:** a PriorityQueue is how a ticket queue sorted by priority behaves: you always pick up the highest-priority one next.

## 7.6 Which one? Decide before opening the answers

1. You need **unique usernames**, order doesn't matter.
2. **key → value** lookup (employeeId → name).
3. Elements **automatically sorted** with no duplicates.
4. A **stack** for matching brackets.
5. Process tasks in **arrival order** (BFS).
6. Always need the **smallest** of a changing set (Dijkstra, top-K).
7. Keep unique items in **insertion order**.
8. Count how many times each word appears.
9. Frequent **random access by index**, adding at the end.
10. A **sorted map** where you need "next higher key".

<details><summary>Answers</summary>

1. `HashSet` 2. `HashMap` 3. `TreeSet` 4. `ArrayDeque` (as stack) 5. `ArrayDeque`/`LinkedList` as `Queue` 6. `PriorityQueue` (min-heap) 7. `LinkedHashSet` 8. `HashMap<String,Integer>` 9. `ArrayList` 10. `TreeMap` (`higherKey`/`ceilingKey`)
</details>

## 7.7 Iteration gotchas

- Removing from a list inside a for-each throws `ConcurrentModificationException`. Use an `Iterator` with `it.remove()`, or `list.removeIf(x -> x < 0)`.
- `List<int>` is illegal: collections hold objects. Use wrappers (`Integer`) with **autoboxing**.
- `Integer a = 127, b = 127; a == b` is `true` (cache), `128 == 128` is `false`. **Compare wrappers with `.equals()`.**

---

# 8. Generics (5 min) 🟡

**Intuition.** Generics let you say "this list holds **Strings**", so the compiler catches mistakes **before** runtime and you stop casting.

```java
List raw = new ArrayList();           // old style
raw.add("hi"); raw.add(5);
String s = (String) raw.get(1);       // ClassCastException at RUNTIME

List<String> safe = new ArrayList<>();
safe.add("hi");
// safe.add(5);                        // COMPILE error
String t = safe.get(0);               // no cast
```

```java
class Box<T> {                          // generic class; T = type placeholder
    private T value;
    Box(T value) { this.value = value; }
    T get() { return value; }
}
Box<Integer> b = new Box<>(10);

static <T> void printAll(List<T> list) {   // generic method: <T> before return type
    for (T x : list) System.out.println(x);
}

static <T extends Comparable<T>> T maxOf(T a, T b) {   // bounded type
    return a.compareTo(b) >= 0 ? a : b;
}
```

**Wildcards** (reading code):

- `List<?>`: list of unknown type (read-only-ish).
- `List<? extends Number>`: "some list of Number or its subclasses". **Read** safely, can't add.
- `List<? super Integer>`: "list of Integer or its parents". Can **add** Integers.

Memory aid: **P**roducer **E**xtends, **C**onsumer **S**uper.

Generics exist only at compile time (**type erasure**): at runtime `List<String>` and `List<Integer>` are the same class. Primitives cannot be type arguments.

**Quick check:** Is `List<Object> x = new ArrayList<String>();` legal?

<details><summary>Answer</summary>

No. Generics are **invariant**: `List<String>` is **not** a `List<Object>` even though String is an Object (otherwise you could add an Integer to it). Use `List<? extends Object>`.
</details>

---

# 9. Exception Handling (10 min) 🔴

## 9.1 Hierarchy

```text
Throwable
 ├── Error                 (JVM problems: OutOfMemoryError, StackOverflowError). Don't catch.
 └── Exception
      ├── Checked          (compiler FORCES you to handle): IOException, SQLException
      └── RuntimeException (UNCHECKED, programming bugs): NullPointerException,
                           ArrayIndexOutOfBoundsException, IllegalArgumentException,
                           ArithmeticException, ClassCastException
```

| | **Checked** | **Unchecked** |
|---|---|---|
| Parent | `Exception` (not Runtime) | `RuntimeException` |
| Compiler | must `catch` or declare `throws` | no requirement |
| Typical meaning | external failure you can recover from (file missing, network) | bug / invalid input |

## 9.2 try / catch / finally

```java
try {
    int x = Integer.parseInt("abc");
} catch (NumberFormatException e) {          // most specific first
    System.out.println("bad number: " + e.getMessage());
} catch (Exception e) {                      // general last
    e.printStackTrace();
} finally {
    System.out.println("always runs (cleanup)");
}
```

Multi-catch: `catch (IOException | SQLException e)`.

## 9.3 ⭐ `throw` vs `throws`

| | **`throw`** | **`throws`** |
|---|---|---|
| What | **Statement**: actually raises an exception object | **Declaration** on a method signature: "I might pass this on" |
| Where | inside method body | after the parameter list |
| Count | one exception object | can list several types |

```java
static void withdraw(int balance, int amt) throws InsufficientFundsException {   // declare
    if (amt > balance)
        throw new InsufficientFundsException("Short by " + (amt - balance));       // raise
}
```

## 9.4 Custom exceptions

```java
class InsufficientFundsException extends Exception {          // checked
    public InsufficientFundsException(String msg) { super(msg); }
}
class UserNotFoundException extends RuntimeException {        // unchecked
    public UserNotFoundException(String msg) { super(msg); }
}
```

Backend convention (Spring Boot): mostly **unchecked** custom exceptions, translated into HTTP responses by an exception handler.

## 9.5 try-with-resources

```java
try (BufferedReader br = new BufferedReader(new FileReader("a.txt"))) {   // AutoCloseable
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}   // br.close() is called automatically, even on error
```

## 9.6 Predict

```java
static int f() {
    try { return 1; }
    finally { System.out.println("finally"); }
}
System.out.println(f());
```

<details><summary>Answer</summary>

`finally` then `1`. `finally` runs before the method actually returns. (Returning inside `finally` overrides everything. Never do that.)
</details>

**Which is checked?** `NullPointerException`, `IOException`, `IllegalArgumentException`, `SQLException`.

<details><summary>Answer</summary>

`IOException` and `SQLException`. The others extend `RuntimeException`.
</details>

---

# 10. Lambdas, Streams, Optional (13 min) 🟡

## 10.1 Lambda

**Intuition.** A lambda is a **short anonymous function** you can pass around. `(params) -> expression`. You already know this from JavaScript arrow functions.

```java
x -> x * 2                      // takes x, returns x*2
(a, b) -> a + b
(String s) -> { return s.trim(); }
```

A lambda can be used wherever a **functional interface** is expected: an interface with exactly **one abstract method**.

```java
Runnable r = () -> System.out.println("run");
Comparator<String> byLen = (a, b) -> a.length() - b.length();
```

## 10.2 The four you must know (`java.util.function`)

| Interface | Method | Meaning | Example |
|---|---|---|---|
| `Predicate<T>` | `boolean test(T)` | **yes/no test** | `x -> x > 5` |
| `Function<T,R>` | `R apply(T)` | **transform** | `s -> s.length()` |
| `Consumer<T>` | `void accept(T)` | **do something**, return nothing | `x -> System.out.println(x)` |
| `Supplier<T>` | `T get()` | **produce** a value | `() -> new ArrayList<>()` |

Variables captured from outside must be **effectively final** (not reassigned).

## 10.3 Method references

```java
list.forEach(System.out::println);      // x -> System.out.println(x)
.map(String::length)                    // s -> s.length()
.map(Student::getName)
.collect(Collectors.toCollection(ArrayList::new))
```

## 10.4 Streams

**Intuition.** A stream is a **pipeline**: source → intermediate steps (lazy) → terminal step (triggers everything). It doesn't modify the original collection.

```java
List<Integer> nums = List.of(5, 2, 8, 2, 7, 4);

nums.stream()
    .filter(n -> n % 2 == 0)     // intermediate
    .map(n -> n * n)             // intermediate
    .sorted()                    // intermediate
    .distinct()
    .limit(3)
    .forEach(System.out::println);   // terminal
```

| Intermediate | Terminal |
|---|---|
| `filter, map, sorted, distinct, limit, skip, flatMap` | `forEach, collect, reduce, count, sum*, min, max, anyMatch, allMatch, findFirst, toList()` |

(*`sum` and `average` exist on primitive streams `IntStream`, via `mapToInt`.)

### Convert these loops into streams (write first)

For `List<Integer> nums = List.of(5, 2, 8, 2, 7, 4);` find: **a)** even numbers, **b)** squares, **c)** maximum, **d)** sorted unique values, **e)** sum.

<details><summary>Solutions</summary>

```java
List<Integer> evens   = nums.stream().filter(n -> n % 2 == 0).collect(Collectors.toList());
List<Integer> squares = nums.stream().map(n -> n * n).toList();                 // Java 16+
int max               = nums.stream().max(Integer::compare).get();              // or Collections.max(nums)
List<Integer> uniq    = nums.stream().distinct().sorted().toList();
int sum               = nums.stream().mapToInt(Integer::intValue).sum();
int sum2              = nums.stream().reduce(0, Integer::sum);                  // reduce: fold to ONE value
```
</details>

### Grouping / collecting

```java
List<String> words = List.of("apple", "bat", "cat", "avocado");

Map<Character, List<String>> byFirst =
    words.stream().collect(Collectors.groupingBy(w -> w.charAt(0)));
// {a=[apple, avocado], b=[bat], c=[cat]}

Map<Integer, Long> byLen =
    words.stream().collect(Collectors.groupingBy(String::length, Collectors.counting()));

String joined = words.stream().collect(Collectors.joining(", "));
Map<String,Integer> m = words.stream().collect(Collectors.toMap(w -> w, String::length));
```

🔗 **ServiceNow bridge:** `filter` ≈ an encoded query / `addQuery`, `groupingBy` ≈ GlideAggregate `groupBy`.

### Stream vs loop: when?

- **Stream:** filter/transform/aggregate/group collections: reads declaratively.
- **Loop:** early exits with complex logic, index math, mutating state, nested logic, tight DSA code, performance-critical paths, or when a stream would be harder to read.

A stream can be consumed **once**. Don't mutate external state in a pipeline.

## 10.5 Optional

**Why:** avoid `null` and `NullPointerException` by making "might be empty" explicit in the type.

```java
Optional<String> o1 = Optional.of("x");              // value must be non-null
Optional<String> o2 = Optional.ofNullable(maybeNull);// null-safe
Optional<String> empty = Optional.empty();

o2.orElse("default");                 // always evaluates the default expression
o2.orElseGet(() -> compute());        // computes only if empty (prefer for expensive defaults)
o2.ifPresent(v -> System.out.println(v));
o2.isPresent();  o2.map(String::trim);  o2.orElseThrow(() -> new RuntimeException("none"));
```

Use `Optional` as a **return type**. Not for fields or parameters.

---

# 11. Comparable and Comparator (5 min) 🟡

| | `Comparable<T>` | `Comparator<T>` |
|---|---|---|
| Where | **inside** the class | **separate** object/lambda |
| Method | `int compareTo(T other)` | `int compare(T a, T b)` |
| Orders | **one natural order** | **many** custom orders |

Return **negative** if first < second, **0** if equal, **positive** if first > second.

```java
class Student implements Comparable<Student> {
    String name; int age; int marks;
    Student(String n, int a, int m) { name = n; age = a; marks = m; }
    String getName() { return name; }  int getAge() { return age; }  int getMarks() { return marks; }

    @Override
    public int compareTo(Student o) { return Integer.compare(this.marks, o.marks); }  // natural: by marks
}
```

**Write:** sort a `List<Student> list` by **marks desc**, by **name**, and by **age** then **name**.

<details><summary>Solutions</summary>

```java
list.sort(Comparator.comparingInt(Student::getMarks).reversed());   // marks, high to low
list.sort(Comparator.comparing(Student::getName));                  // name A-Z
list.sort(Comparator.comparingInt(Student::getAge)
                    .thenComparing(Student::getName));              // age, tie-break by name

Collections.sort(list);                          // uses compareTo (natural order)
Arrays.sort(arr, (a, b) -> Integer.compare(b, a)); // Integer[] only, descending
```

**Pitfall:** `(a, b) -> a - b` can **overflow** for large/negative values. Use `Integer.compare(a, b)`.
</details>

`PriorityQueue<Student> pq = new PriorityQueue<>(Comparator.comparingInt(Student::getMarks));` also takes a comparator, as do `TreeSet` and `TreeMap`.

---

# 12. Memory + JVM (3 min) 🟢

```text
STACK (per thread)                 HEAP (shared)
─────────────────                  ─────────────────────────
main() frame                        objects live here
  int n = 5        (value)          ┌───────────────┐
  Student s ─────────────────────▶  │ Student{...}  │
  int[] arr ─────────────────────▶  │ int[]{1,2,3}  │
method frames pushed/popped        └───────────────┘
                                    String pool (inside heap)
```

- **Stack:** one per thread. Holds **method frames**: local variables, primitives, and **references**. Fast, auto-cleaned when a method returns. Too deep recursion → `StackOverflowError`.
- **Heap:** holds **all objects** (including arrays). Shared. Cleaned by the GC. Full → `OutOfMemoryError`.
- **Reference:** a variable holding the address of an object. `null` = points to nothing; calling a method on it → `NullPointerException`.
- **Garbage collection:** the JVM automatically frees objects that are **no longer reachable** from any live reference. You don't `free()` manually. You can't force it (`System.gc()` is only a hint).
- **String pool:** shared cache of string literals (Section 5).
- **JVM:** loads classes, verifies bytecode, runs it (interpreter + JIT compiler turns hot code into machine code), manages memory/GC.

**Interview one-liner:** "Primitives and references live on the stack. Objects live on the heap. When no reference can reach an object, the garbage collector reclaims it."

---

# 13. Java for DSA: The Toolkit (5 min) 🔴

## 13.1 Import line you'll use constantly

```java
import java.util.*;
```

## 13.2 Cheat toolkit

```java
int[] a = new int[n];            Arrays.sort(a);          Arrays.fill(a, -1);
String s;   s.charAt(i)   s.toCharArray()   new String(chars)   String.valueOf(chars)
StringBuilder sb = new StringBuilder();   sb.append(x); sb.reverse(); sb.toString();
List<Integer> list = new ArrayList<>();   Collections.sort(list);   list.get(i)
Map<Integer,Integer> map = new HashMap<>();   Set<Integer> set = new HashSet<>();
Deque<Integer> stack = new ArrayDeque<>();    Queue<int[]> q = new ArrayDeque<>();
PriorityQueue<Integer> minH = new PriorityQueue<>();
PriorityQueue<Integer> maxH = new PriorityQueue<>(Collections.reverseOrder());
Integer.MAX_VALUE   Integer.MIN_VALUE   Long.MAX_VALUE   Math.abs   Math.max   Math.min   Math.pow
```

## 13.3 Patterns

**Frequency counting**

```java
Map<Character,Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);
```

**Duplicate detection**

```java
Set<Integer> seen = new HashSet<>();
for (int x : a) if (!seen.add(x)) return true;   // add() returns false if already present
return false;
```

**Two Sum (HashMap)**

```java
Map<Integer,Integer> idx = new HashMap<>();
for (int i = 0; i < a.length; i++) {
    int need = target - a[i];
    if (idx.containsKey(need)) return new int[]{idx.get(need), i};
    idx.put(a[i], i);
}
```

**Stack: valid parentheses**

```java
Deque<Character> st = new ArrayDeque<>();
for (char c : s.toCharArray()) {
    if (c == '(' || c == '[' || c == '{') st.push(c);
    else {
        if (st.isEmpty()) return false;
        char open = st.pop();
        if ((c == ')' && open != '(') || (c == ']' && open != '[') || (c == '}' && open != '{'))
            return false;
    }
}
return st.isEmpty();
```

**Queue: BFS skeleton**

```java
Queue<Integer> q = new ArrayDeque<>();
boolean[] visited = new boolean[n];
q.offer(start); visited[start] = true;
while (!q.isEmpty()) {
    int u = q.poll();
    for (int v : graph.get(u)) if (!visited[v]) { visited[v] = true; q.offer(v); }
}
```

**Heap: K largest**

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();   // min-heap of size k
for (int x : a) { pq.offer(x); if (pq.size() > k) pq.poll(); }
// pq.peek() is the k-th largest
```

**Sorting 2D (intervals)**

```java
Arrays.sort(intervals, (x, y) -> Integer.compare(x[0], y[0]));
```

**Sorting a String's chars (anagram key)**

```java
char[] c = s.toCharArray(); Arrays.sort(c); String key = new String(c);
```

## 13.4 Gotchas that cost points

- `==` on `Integer`/`String` objects → use `.equals()`.
- `int` overflow: use `long` when sums/products can exceed ~2.1 billion.
- Don't use `Stack` or `Vector`: use `ArrayDeque`.
- `List<Integer>` → `int[]`: `list.stream().mapToInt(Integer::intValue).toArray()`. `int[]` → `List<Integer>`: `Arrays.stream(a).boxed().toList()`.
- `s.length()` vs `a.length` vs `list.size()`.
- Modifying a collection while iterating it.

## 13.5 Quick decisions

Which structure? (Answer first, then check.)

1. Count characters → ?  2. Check if seen before → ?  3. Undo/last-in-first-out → ?  4. Level-order traversal → ?  5. Top-K / running min → ?  6. Sorted keys with floor/ceiling → ?

<details><summary>Answers</summary>

1. `HashMap` (or `int[26]`) 2. `HashSet` 3. `ArrayDeque` as stack 4. `ArrayDeque` as queue 5. `PriorityQueue` 6. `TreeMap`/`TreeSet`
</details>

---

# 14. Java → Spring Boot Map (2 min) 🟢

You're not learning Spring Boot now. This is *why today's Java matters*.

| Java concept | Where it shows up in Spring Boot |
|---|---|
| **OOP / classes** | Controllers, Services, Repositories, Entities are all classes with layered responsibilities. |
| **Interfaces + polymorphism** | You code to an interface (`UserService`); Spring injects the actual implementation. Swap implementations without changing callers. |
| **Dependency injection** | Instead of `new Service()` inside a class, Spring creates objects (**beans**) and passes them to your constructor. It's just "composition (HAS-A) with someone else doing the `new`". 🔗 Like calling a shared Script Include instead of rewriting logic. |
| **Annotations** (`@Override` is one) | `@RestController`, `@Service`, `@Autowired`, `@Entity`, `@GetMapping` are metadata that Spring reads to wire things. |
| **Collections** | Endpoints return `List<UserDto>`, maps for config, sets for unique roles. |
| **Generics** | `Repository<User, Long>`, `ResponseEntity<T>`, `Optional<User>`: you must be able to read these. |
| **Exceptions** | Custom unchecked exceptions + `@ControllerAdvice` map to HTTP 404/400/500. |
| **Streams / lambdas** | Map entities to DTOs, filter results, group data. |
| **Optional** | `repository.findById(id)` returns `Optional<User>`. |
| **Immutability / records** | DTOs as `record`s or `final` fields; safer shared state (beans are singletons shared across requests). |
| **equals/hashCode** | JPA entities and caching/sets of entities. |

**Next step after Java:** a basic Spring Boot REST API (Controller → Service → Repository) with a database.

---

# 15. One-Page Cheat Sheet

```text
Types:   byte short int long float double char boolean | everything else = reference
Casting: widening auto; narrowing explicit (int)9.8 -> 9
Pass:    ALWAYS by value (references copied, objects shared)
OOP:     class=blueprint, object=instance; encapsulation=private+getters;
         inheritance=extends (IS-A); polymorphism: reference type decides WHAT you can call,
         object type decides WHICH runs; abstract class=partial base, interface=contract
Overload: same name, different params, compile-time | Override: same signature, runtime
equals/hashCode: equal objects MUST have equal hashCodes; override both together
String:  immutable, pool, equals() not ==, StringBuilder for loops
Array:   fixed size, .length, Arrays.sort/toString/copyOf/fill
List: ArrayList (default) | Set: HashSet / Linked (order) / Tree (sorted)
Map: HashMap / LinkedHashMap / TreeMap | Stack: ArrayDeque | Queue: ArrayDeque
Heap: PriorityQueue (min by default; reverseOrder for max)
Big-O: ArrayList get O(1); Hash* O(1) avg; Tree* O(log n); PriorityQueue offer/poll O(log n)
Exceptions: Checked (must handle) vs Unchecked (RuntimeException). throw=raise, throws=declare
Functional: Predicate(test) Function(apply) Consumer(accept) Supplier(get)
Streams: source -> filter/map/sorted/distinct/limit -> collect/forEach/reduce/count
Comparator: comparingInt(X::getA).thenComparing(X::getB).reversed()
Memory: stack = frames + references; heap = objects; GC frees unreachable objects
```

---

# 16. 🧪 Final Assessment (15 min)

**Rules:** timer on, **close everything above**, answer on paper or in an IDE, then check answers. Score honestly.

## A. Rapid-fire conceptual (20)

1. JDK vs JRE vs JVM?
2. Why is Java platform independent?
3. Name 5 primitive types.
4. Why does `10 / 4` give `2`?
5. Is Java pass-by-value or pass-by-reference? Explain.
6. Can a static method use `this`? Why?
7. What is constructor overloading?
8. What does `super()` do?
9. Overloading vs overriding?
10. Reference type vs object type in `Animal a = new Dog();`?
11. Abstract class vs interface (2 differences)?
12. What is encapsulation?
13. What is the `equals`/`hashCode` contract?
14. Why is `String` immutable? (2 reasons)
15. `==` vs `.equals()` for Strings?
16. ArrayList vs LinkedList: when each?
17. HashMap vs TreeMap?
18. Checked vs unchecked exception?
19. `throw` vs `throws`?
20. What is a functional interface?

<details><summary>Answers</summary>

1. JVM runs bytecode; JRE = JVM + libraries; JDK = JRE + compiler/tools. 2. Compiled to bytecode, run by a per-OS JVM. 3. byte, short, int, long, float, double, char, boolean. 4. Both operands are `int` → integer division truncates. 5. Always by value; for objects the *reference* is copied, so mutation is visible but reassignment isn't. 6. No: static belongs to the class, no current object. 7. Multiple constructors with different params. 8. Calls the parent constructor (must be the first statement). 9. Overload: same name, different params, compile-time. Override: same signature in child, runtime. 10. Reference = Animal (what you can call); object = Dog (which override runs). 11. Abstract class: state/constructors, single inheritance. Interface: contract, many allowed, no instance state. 12. Hiding fields (`private`) and exposing controlled access. 13. Equal objects ⇒ equal hash codes. 14. Security/safe sharing, enables string pool, thread-safety, safe as hash keys. 15. `==` compares references; `.equals()` compares content. 16. ArrayList by default (fast index access); LinkedList rarely (use ArrayDeque for queue/deque). 17. HashMap O(1) unordered; TreeMap O(log n) sorted keys. 18. Checked: compiler forces handling (IOException). Unchecked: RuntimeException subclasses (NPE). 19. `throw` raises one exception object; `throws` declares possible exceptions in the signature. 20. Interface with exactly one abstract method (usable with lambdas).
</details>

## B. Predict the output (5)

```java
// B1
String a = "hi"; String b = new String("hi");
System.out.println((a == b) + " " + a.equals(b));

// B2
static void f(int[] arr, int x) { arr[0] = 10; x = 10; arr = new int[]{99}; }
int[] arr = {1}; int x = 1; f(arr, x);
System.out.println(arr[0] + " " + x);

// B3
class P { void hi() { System.out.println("P"); } }
class C extends P { void hi() { System.out.println("C"); } }
P p = new C(); p.hi();

// B4
List<Integer> l = new ArrayList<>(List.of(10, 20, 30));
l.remove(1);
System.out.println(l);

// B5
System.out.println(1 + 2 + "3" + 4 + 5);
```

<details><summary>Answers</summary>

B1: `false true` · B2: `10 1` · B3: `C` · B4: `[10, 30]` (`remove(int)` removes the **index**) · B5: `3345` (1+2=3 first, then `3 + "3"` = "33", then "334", then "3345").
</details>

## C. Coding problems (4)

1. Write `static int[] twoSum(int[] a, int target)` returning indices (unsorted array) in O(n).
2. Write `static boolean isValid(String s)` for balanced `()[]{}`.
3. Given `List<String> words`, return a `Map<String,Integer>` of word frequencies. Then print the top 3 by count using a stream.
4. Sort `List<Student>` (name, marks) by marks descending, then name ascending. Then write a custom unchecked `InvalidMarksException` and throw it from a setter when marks are outside 0–100.

<details><summary>Answers</summary>

1 and 2: Section 13.3.

3:

```java
Map<String,Integer> freq = new HashMap<>();
for (String w : words) freq.merge(w, 1, Integer::sum);

freq.entrySet().stream()
    .sorted(Map.Entry.<String,Integer>comparingByValue().reversed())
    .limit(3)
    .forEach(e -> System.out.println(e.getKey() + " " + e.getValue()));
```

4:

```java
list.sort(Comparator.comparingInt(Student::getMarks).reversed()
                    .thenComparing(Student::getName));

class InvalidMarksException extends RuntimeException {
    InvalidMarksException(String m) { super(m); }
}
void setMarks(int m) {
    if (m < 0 || m > 100) throw new InvalidMarksException("Marks out of range: " + m);
    this.marks = m;
}
```
</details>

## D. Collection-selection scenarios (5)

1. Remove duplicate emails from a list, keep original order.
2. Cache with O(1) lookup by user id.
3. Always process the highest-priority task first.
4. Undo feature (last action undone first).
5. A leaderboard sorted by score where you also need "who is just above score X".

<details><summary>Answers</summary>

1. `LinkedHashSet` 2. `HashMap<Integer, User>` 3. `PriorityQueue` with a priority comparator 4. `ArrayDeque` used as a stack 5. `TreeMap<Integer, ...>` (`higherKey` / `ceilingKey`)
</details>

## E. Interview questions (5)

1. Why must you override `hashCode` when you override `equals`?
2. Explain runtime polymorphism with an example.
3. Is Java pass-by-value? Prove it with the swap-method example.
4. Difference between `ArrayList` and `LinkedList`, and which would you choose by default?
5. When would you prefer a loop over a stream?

<details><summary>Model answers</summary>

1. Hash collections pick the bucket with `hashCode` and then confirm with `equals`. If equal objects have different hashes they land in different buckets → duplicates/lost lookups. 2. `Animal a = new Dog(); a.sound();` calls Dog's override: method chosen at runtime from the actual object type. 3. A `swap(Student x, Student y)` that swaps the local references has no effect on the caller, because the references were copied. Only mutating the object (`x.name = ...`) is visible. 4. ArrayList: array-backed, O(1) get; LinkedList: node-based, O(n) get; default ArrayList. 5. Complex control flow/early exit, index-based logic, mutation, simple loops where the stream is harder to read, and hot performance paths.
</details>

## F. Score it

| Section | Max | Your score |
|---|---:|---:|
| A. Conceptual | 20 | |
| B. Output prediction | 5 | |
| C. Coding | 4 | |
| D. Collections | 5 | |
| E. Interview | 5 | |

---

# 17. 📊 Evaluation + Revision Checklist

## 17.1 Self-rate each topic

Mark each as 🟢 **Strong (I can use it)**, 🟡 **Understood, needs coding practice**, or 🔴 **Forgot / relearn**. Rule: if you couldn't **write it from scratch** in the assessment, it's at most 🟡.

| Topic | 🟢/🟡/🔴 |
|---|---|
| Fundamentals, types, casting, control flow | |
| Methods, static, `this`, pass-by-value | |
| Classes, constructors, encapsulation | |
| Inheritance, overriding, polymorphism | |
| Abstract class vs interface | |
| `equals` / `hashCode` / `toString` | |
| Strings, pool, StringBuilder | |
| Arrays + `Arrays` utility | |
| List / ArrayList / LinkedList | |
| Set (Hash / Linked / Tree) | |
| Map (Hash / Linked / Tree) | |
| Queue / Deque / PriorityQueue | |
| Generics | |
| Exceptions (`throw` vs `throws`, checked) | |
| Lambdas + functional interfaces | |
| Streams + Collectors | |
| Optional | |
| Comparable / Comparator | |
| Memory / JVM | |
| Java DSA patterns | |

## 17.2 My Java Revision Checklist

Tick when you can do it **without looking**.

**Fundamentals**
- [ ] Explain JDK/JRE/JVM and bytecode in 30 seconds
- [ ] Write factorial, reverse number, pattern, even/odd from scratch
- [ ] Explain pass-by-value with the array example

**OOP**
- [ ] Write a class with private fields, constructor, getters/setters
- [ ] Explain `Animal a = new Dog()` (reference vs object type)
- [ ] State 3 differences: abstract class vs interface
- [ ] Write `equals`, `hashCode`, `toString` for a class
- [ ] Explain why `equals`/`hashCode` matter for HashMap

**Strings & Arrays**
- [ ] Predict `==` vs `.equals()` outputs
- [ ] Palindrome, anagram, vowel count
- [ ] Max, reverse, frequency, second-largest, two-pointer

**Collections**
- [ ] Choose correctly across the 10 scenarios in 7.6
- [ ] Use `merge`, `getOrDefault`, `computeIfAbsent`, `entrySet`
- [ ] Use `ArrayDeque` as stack and queue, `PriorityQueue` as min/max heap

**Exceptions / Modern Java**
- [ ] Write a custom exception and use `throw`/`throws`
- [ ] Write try-with-resources
- [ ] Convert 5 loops into streams; write a `groupingBy`
- [ ] Sort objects with `Comparator.comparing...thenComparing`

**DSA in Java**
- [ ] Solve 5 easy problems: Two Sum, Valid Parentheses, Duplicates, Top-K, BFS: in Java without reference

## 17.3 Revision plan

| When | What |
|---|---|
| **Tomorrow (30 min)** | Redo every 🔴/🟡 item from the checklist **from scratch** in an IDE. |
| **Day 3** | Re-attempt the final assessment cold. |
| **Day 7** | 5 DSA problems in Java using only the Section 13 toolkit. |
| **Weekly** | Re-read the Cheat Sheet (Section 15). Skim code of one real project. |

## 17.4 What to learn NEXT

1. **Java + DSA practice (1–2 weeks):** 40–50 easy/medium problems (arrays, strings, hashing, stack/queue, two pointers, sorting). This cements Collections.
2. **Small plain-Java project (3–4 days):** e.g., a library or ticket-tracker console app with classes, interfaces, collections, custom exceptions, streams.
3. **Git + Maven/Gradle basics (1 day).**
4. **SQL + JDBC concept (3–4 days).**
5. **Spring Boot REST API (1–2 weeks):** Controller → Service → Repository, validation, exception handler, JPA.
6. Later: unit testing (JUnit/Mockito), concurrency basics (`Thread`, `ExecutorService`, `synchronized`), Java memory/GC in depth.

> If your assessment shows 🔴 in **OOP, Collections or Exceptions**, finish those first, because Spring Boot depends on them heavily.

---

*End of file. Revise in short sessions: attempt first, check second.*
