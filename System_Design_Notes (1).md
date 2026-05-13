# System Design - Complete Notes

> Structured notes for cracking System Design interviews at top product-based companies.
> Target Level: SDE-2 / SDE-3

---

## Table of Contents

### Low-Level Design (LLD)
- [Lecture 1: Foundations of LLD](#lecture-1-foundations-of-lld)
  - [SDLC (Software Development Life Cycle)](#1-sdlc---software-development-life-cycle)
  - [OOP Concepts](#2-oop-concepts)
  - [SOLID Principles](#3-solid-principles)
  - [SOLID Principles - Comparison Table](#4-solid-principles---comparison-table)
  - [LLD vs HLD - When to Apply What](#5-lld-vs-hld---when-to-apply-what)
  - [The Diamond Problem](#6-the-diamond-problem)
  - [UML Diagrams & Relationships](#7-uml-diagrams--relationships)
  - [Relationships in OOP](#8-relationships-in-oop)
- [Lecture 2: SOLID Deep Dive + Creational Design Patterns](#lecture-2-solid-deep-dive--creational-design-patterns)
  - [Interface Segregation Principle (ISP)](#9-interface-segregation-principle-isp)
  - [Dependency Inversion Principle (DIP)](#10-dependency-inversion-principle-dip)
  - [DRY Principle](#11-dry---dont-repeat-yourself)
  - [KISS Principle](#12-kiss---keep-it-simple-stupid)
  - [Design Patterns - Introduction](#13-design-patterns---introduction)
  - [Threading & Synchronization Primer](#14-threading--synchronization-primer-prerequisite-for-singleton)
  - [Singleton Pattern](#15-singleton-design-pattern)
  - [Factory Pattern](#16-factory-design-pattern)
  - [Abstract Factory Pattern](#17-abstract-factory-design-pattern)
  - [Builder Pattern (in depth)](#18-builder-design-pattern-in-depth)
- [Lecture 3: Structural & Behavioral Design Patterns](#lecture-3-structural--behavioral-design-patterns)
  - **Structural Patterns**
    - [Structural Design Patterns - Introduction](#19-structural-design-patterns--introduction)
    - [Adapter Pattern](#20-adapter-design-pattern)
    - [Facade Pattern](#21-facade-design-pattern)
    - [Decorator Pattern](#22-decorator-design-pattern)
    - [Flyweight Pattern](#23-flyweight-design-pattern)
  - **Behavioral Patterns**
    - [Behavioral Design Patterns - Introduction](#24-behavioral-design-patterns--introduction)
    - [Strategy Pattern](#25-strategy-design-pattern)
    - [Observer Pattern](#26-observer-design-pattern)
    - [Chain of Responsibility Pattern](#27-chain-of-responsibility-design-pattern)
    - [Template Method Pattern](#28-template-method-design-pattern)
  - [Lecture 3 — Patterns at a Glance (Cheat Sheet)](#29-lecture-3--patterns-at-a-glance)

### Revision & Reference
- [Design Patterns — Revision Cheat Sheet (ALL 12 Patterns)](#30-design-patterns--revision-cheat-sheet-all-12-patterns)
  - [30.1 The 12 Patterns in One Line Each](#301-the-12-patterns-in-one-line-each)
  - [30.2 The 4-Slot Recall Card (per pattern)](#302-the-4-slot-recall-card-per-pattern)
  - [30.3 The Sibling-Pairs Trap](#303-the-sibling-pairs-trap-this-is-where-points-are-lost)
  - [30.4 Problem Statement → Pattern (keyword lookup)](#304-problem-statement--pattern-the-keyword-lookup)
  - [30.5 The 30-Second Interview Pitch Template](#305-the-30-second-interview-pitch-template)
  - [30.6 Decision Tree — Which Pattern Should I Suggest?](#306-decision-tree--which-pattern-should-i-suggest)
  - [30.7 SOLID Principles — One-Line Cheat Sheet](#307-solid-principles--one-line-cheat-sheet)
  - [30.8 Quick-Fire Interview Q&A](#308-quick-fire-interview-qa-the-questions-you-will-be-asked)
  - [30.9 The "Final Mile" Checklist](#309-the-final-mile-checklist-15-mins-before-the-interview)

---

# Lecture 1: Foundations of LLD

---

## 1. SDLC - Software Development Life Cycle

SDLC is a structured process that defines the stages involved in building software from inception to deployment and maintenance. Every piece of production software at companies like Google, Amazon, or Flipkart follows some variant of SDLC.

### Phases of SDLC (with Role Mapping)

Each phase is typically owned by a specific role in the organization:

```
┌──────────────────────┐
│  1. Planning          │──► PM (Product Manager)
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  2. Requirement      │──► PM / Business Analyst
│     Gathering &      │
│     Analysis         │
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  3. Design           │──► Tech Architect / Sr. Developer
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  4. Development      │──► Developers (SDE / SDE-2)
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  5. Testing          │──► Full Dev / QA Engineers
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  6. Deployment       │──► Full Stack Dev / DevOps / SRE
└──────┬───────────────┘
       ▼
┌──────────────────────┐
│  7. Maintenance      │──► DRI (Directly Responsible Individual) / On-call
└──────────────────────┘
```

### SDLC Models

The lecture covered two primary SDLC models:

### (1) Waterfall SDLC Model

A **sequential** approach where each phase must be **100% complete** before the next phase begins. No going back.

```
Planning (100%) ──► Req Analysis (100%) ──► Design (100%)
    ──► Development (100%) ──► Testing (100%)
        ──► Deployment (100%) ──► Maintenance (100%)
```

**Key characteristics:**
- Each phase is fully completed before moving on
- Rigid — no going back to a previous phase
- Works well when requirements are crystal clear upfront
- Not ideal for evolving requirements

### (2) Iterative SDLC Model

An **incremental** approach where the product is built in multiple iterations. Each iteration goes through all phases and delivers a working release.

```
         ┌──────────────────────────────────────────────────┐
         │  Planning (done once at the start)                │
         └──────────────────────┬───────────────────────────┘
                                ▼
    ┌─────────────────────────────────────────────────┐
    │  Iteration 1:                                    │
    │  Req Analysis → Design/Dev → Test/Deploy → Maint │──► Release 1 (30%)
    ├─────────────────────────────────────────────────┤
    │  Iteration 2:                                    │
    │  Req Analysis → Design/Dev → Test/Deploy → Maint │──► Release 2 (+30% = 60%)
    ├─────────────────────────────────────────────────┤
    │  Iteration 3:                                    │
    │  Req Analysis → Design/Dev → Test/Deploy → Maint │──► Release 3 (+20% = 80%)
    ├─────────────────────────────────────────────────┤
    │  ...                                             │──► Release N (100%)
    └─────────────────────────────────────────────────┘
```

**Key characteristics:**
- Planning is done once; then each iteration cycles through the remaining phases
- Each iteration delivers a working, deployable release
- New features are added incrementally with each release
- Much more flexible than waterfall — can adapt to changing requirements
- Most modern companies (Agile, Scrum) follow iterative approaches

### Real-World Example

Building an e-commerce platform like Amazon:
1. **Planning** — Decide to build a marketplace with seller onboarding
2. **Requirement Analysis** — Users can browse, search, add to cart, checkout; system must handle 10K concurrent users
3. **Design** — HLD: Microservices (User, Product, Order, Payment services); LLD: Class design for Cart, Order, Payment
4. **Implementation** — Code the services
5. **Testing** — Test checkout flow, payment edge cases, load testing
6. **Deployment** — Deploy via CI/CD to Kubernetes clusters
7. **Maintenance** — Add new payment methods, fix bugs, scale for sale events

### Interview Tip
> When asked "How would you build X?", mentally walk through SDLC stages. Start with requirements (functional + non-functional), then move to design. This shows structured thinking.

---

## 2. OOP Concepts

Object-Oriented Programming is the backbone of Low-Level Design. Every LLD interview question expects you to model the problem using OOP.

### The 4 Pillars of OOP

### 2.1 Encapsulation

**Definition:** Bundling data (fields) and methods that operate on that data into a single unit (class), and restricting direct access to internal state.

**Why it matters:** Protects object integrity. External code can't put the object into an invalid state.

```java
public class BankAccount {
    private double balance;  // Hidden from outside

    public BankAccount(double initialBalance) {
        if (initialBalance < 0) throw new IllegalArgumentException("Negative balance");
        this.balance = initialBalance;
    }

    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Invalid deposit");
        this.balance += amount;
    }

    public void withdraw(double amount) {
        if (amount > balance) throw new InsufficientFundsException();
        this.balance -= amount;
    }

    public double getBalance() {
        return this.balance;  // Read-only access
    }
}
```

**Key point:** `balance` is `private`. You can't do `account.balance = -500`. The class enforces its own **invariants**.

> **What is an invariant?**
> An invariant is a **condition that must ALWAYS be true** for an object to be in a valid state. It's a rule that should never be violated throughout the lifetime of the object.
>
> **Examples:**
> - `BankAccount` → balance must **never be negative** (invariant: `balance >= 0`)
> - `Stack` → size must **never exceed capacity** (invariant: `size <= capacity`)
> - `Age` field → must **always be between 0 and 150** (invariant: `0 <= age <= 150`)
> - `Email` field → must **always contain @** (invariant: valid format)
>
> **"Protecting invariants"** means using `private` fields + validation in setters/methods so that **no external code can put the object into an illegal state**. That's exactly what encapsulation does — the `withdraw()` method checks `amount <= balance` before allowing the operation, so the invariant `balance >= 0` can never be broken from outside.

### 2.2 Abstraction

**Definition:** Hiding the complex implementation details and showing only the essential features.

**Why it matters:** Reduces complexity. Users of a class don't need to know HOW it works, just WHAT it does.

**Real-world examples from lecture:**
- **CAR Dashboard** — You see Speed and Fuel level. You don't see the complex engine mechanics, fuel injection system, or sensor wiring behind it.
- **ATM Machine** — You see Deposit and Withdraw buttons. You don't see the banking network calls, encryption, vault mechanisms happening behind the scenes.

**Code example (from lecture) — Abstract class:**

```java
abstract class Shape {
    abstract void draw();
}

class Circle extends Shape {
    void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle extends Shape {
    void draw() {
        System.out.println("Drawing Rectangle");
    }
}
```

**How the caller uses it (you can't instantiate `Shape` directly):**

```java
public class Main {
    public static void main(String[] args) {
        // Shape s = new Shape();  ← COMPILE ERROR! Cannot instantiate abstract class

        Shape s1 = new Circle();      // Parent reference, child object
        Shape s2 = new Rectangle();

        s1.draw();  // Output: "Drawing Circle"
        s2.draw();  // Output: "Drawing Rectangle"

        // The caller just calls draw() — doesn't care HOW each shape draws itself
        // This is abstraction: the "what" (draw) is defined, the "how" is hidden
    }
}
```

**Key point:** You always use the **parent type as the reference** (`Shape s1`) but assign a **child object** (`new Circle()`). This is also polymorphism in action — the correct `draw()` is called at runtime based on the actual object type.

**Another example — Interface-based abstraction:**

```java
public interface NotificationService {
    void send(String recipient, String message);
}

public class EmailNotification implements NotificationService {
    @Override
    public void send(String recipient, String message) {
        // Complex SMTP setup, authentication, HTML formatting...
        // All hidden behind a simple send() call
    }
}

public class SMSNotification implements NotificationService {
    @Override
    public void send(String recipient, String message) {
        // Twilio API integration, phone number validation...
        // All hidden behind the same simple interface
    }
}
```

**Encapsulation vs Abstraction:**
- **Encapsulation** = hiding *data* (making fields private)
- **Abstraction** = hiding *complexity* (exposing simple interfaces)

### 2.3 Inheritance

**Definition:** Creating a new class based on an existing class. The child class inherits properties and behaviors from the parent class.

**Why it matters:** Avoids code duplication. Establishes IS-A relationships.

```java
class Animal {          // Parent
    void eat() { System.out.println("Eating..."); }
}

class Dog extends Animal {   // Child — Dog IS-A Animal
    void bark() { System.out.println("Bark!"); }
}

class Cat extends Animal {   // Child — Cat IS-A Animal
    void meow() { System.out.println("Meow!"); }
}
```

### Types of Inheritance (5 Types)

**1. Single Inheritance** — Child class inherits from one parent class only.

```
  ┌──────────┐
  │  Parent   │       Class A
  └─────┬────┘          │
        ▼               ▼
  ┌──────────┐       Class B extends A
  │  Child   │
  └──────────┘
```

**2. Multilevel Inheritance** — A class inherits from a parent, which itself inherits from another class. Forms a chain.

```
  ┌──────────┐       A
  │ Class A  │       │
  └─────┬────┘       ▼
        ▼         B extends A
  ┌──────────┐       │
  │ Class B  │       ▼
  └─────┬────┘    C extends B
        ▼
  ┌──────────┐
  │ Class C  │
  └──────────┘
```

**3. Hierarchical Inheritance** — Multiple child classes inherit from one parent class.

```
              ┌──────────┐
              │ Class A  │   ← One parent
              └─────┬────┘
              ┌─────┴─────┐
              ▼           ▼
        ┌──────────┐ ┌──────────┐
        │ Class B  │ │ Class C  │   ← Multiple children
        └──────────┘ └──────────┘
```

**4. Multiple Inheritance** — A single class inherits from multiple parent classes.

```
  ┌──────────┐  ┌──────────┐
  │ Class A  │  │ Class B  │
  └─────┬────┘  └────┬─────┘
        └──────┬─────┘
               ▼
         ┌──────────┐
         │ Class C  │   ← Inherits from BOTH A and B
         └──────────┘
```

**Java does NOT support multiple inheritance via classes** (to avoid the Diamond Problem). But it **supports it via interfaces:**

```java
interface A { void methodA(); }
interface B { void methodB(); }

class C implements A, B {
    public void methodA() { /* ... */ }
    public void methodB() { /* ... */ }
}
```

**5. Hybrid Inheritance** — Combination of two or more types of inheritance (e.g., Hierarchical + Multiple).

```
Hybrid = Hierarchical + Multiple (or any combination)
```

### 2.4 Polymorphism

**Definition:** Multiple forms — a method can behave differently depending on the context.

**UML visualization (from lecture):**

```
            ┌──────────────┐
            │    Shape     │
            ├──────────────┤
            │ + draw()     │
            └──────┬───────┘
          ┌────────┼────────┐
          ▼        ▼        ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Circle  │ │Rectangle │ │ Triangle │
├──────────┤ ├──────────┤ ├──────────┤
│ +draw()  │ │ +draw()  │ │ +draw()  │
└──────────┘ └──────────┘ └──────────┘
```

Same method `draw()`, different behavior in each class.

**Two types:**

### (1) Compile-Time Polymorphism (Static Polymorphism / Method Overloading)

- Decision made at **compile time**
- Same method name but **different parameters**
- Also called **Method Overloading**

```java
class Calculator {
    int add(int a, int b) {           // Version 1: two ints
        return a + b;
    }
    int add(int a, int b, int c) {    // Version 2: three ints
        return a + b + c;
    }
    double add(double a, double b) {  // Version 3: two doubles
        return a + b;
    }
}
```

**Important — Role of Return Type in Overloading:**

| Scenario | Valid Overload? | Why? |
|----------|:-:|-------|
| `int add(int a, int b)` vs `int add(int a, int b, int c)` | Yes | Different number of parameters |
| `int add(int a, int b)` vs `double add(double a, double b)` | Yes | Different parameter types |
| `int add(int a, int b)` vs `double add(int a, int b)` | **No** | Only return type differs — **COMPILE ERROR** |

> **Rule:** Changing **only the return type** does NOT count as overloading. The compiler decides which method to call based on the **arguments passed**, not what you do with the result. Since `add(5, 3)` could match both `int add(int, int)` and `double add(int, int)`, the compiler can't distinguish them — so it's ambiguous and illegal.

```java
// This will NOT compile:
class Bad {
    int add(int a, int b) { return a + b; }
    double add(int a, int b) { return a + b; }  // ← COMPILE ERROR: already defined
}
```

**Overloading is valid when methods differ in:**
1. **Number** of parameters — `add(a, b)` vs `add(a, b, c)`
2. **Type** of parameters — `add(int, int)` vs `add(double, double)`
3. **Order** of parameter types — `add(int, double)` vs `add(double, int)`

### (2) Run-Time Polymorphism (Dynamic Polymorphism / Method Overriding)

- Decision made at **runtime**
- Requires **Inheritance + Method Overriding**
- Depends on the **actual object type**, not the reference type

```java
class Animal {
    void sound() {
        System.out.println("Animal Sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();   // Reference: Animal, Object: Dog
        a.sound();              // Output: "Dog barks" — calls Dog's method!
    }
}
```

The JVM looks at the **actual object type** (`Dog`) at runtime, not the reference type (`Animal`).

### OOP Summary Table

| Pillar | What it Does | Key Mechanism | Interview Signal |
|--------|-------------|---------------|-----------------|
| **Encapsulation** | Hides data | `private` fields + getters/setters | "How do you protect invariants?" |
| **Abstraction** | Hides complexity | Interfaces, abstract classes | "Design an interface for X" |
| **Inheritance** | Reuses code | `extends` / `implements` | "Model the IS-A relationship" |
| **Polymorphism** | Same interface, different behavior | Overriding / Overloading | "How would different types behave?" |

---

## 3. SOLID Principles

SOLID principles are the foundation of writing maintainable, extensible, and clean code. They are crucial for LLD interviews.

### 3.1 S — Single Responsibility Principle (SRP)

> **"A class should have one, and only one, reason to change."**

Each class should do exactly one thing. If a class has multiple responsibilities, changes to one responsibility can break the other.

**Bad Example (violates SRP):**

```java
public class Employee {
    public void calculateSalary() { /* salary logic */ }
    public void generateReport() { /* PDF generation */ }
    public void saveToDatabase() { /* DB operations */ }
}
```

This class has THREE reasons to change: salary rules change, report format changes, or database schema changes.

**Good Example (follows SRP):**

```java
public class Employee {
    private String name;
    private double baseSalary;
    // Only holds employee data
}

public class SalaryCalculator {
    public double calculate(Employee emp) { /* salary logic */ }
}

public class ReportGenerator {
    public void generate(Employee emp) { /* PDF generation */ }
}

public class EmployeeRepository {
    public void save(Employee emp) { /* DB operations */ }
}
```

**Real-world analogy:** A chef cooks food. A waiter serves food. A cashier handles billing. You don't want one person doing all three — if billing rules change, it shouldn't affect cooking.

**Lecture Example — Snakes and Ladders Game:**

**Approach 1 (Violates SRP):** One massive class doing everything.

```java
class SnakesAndLadders {
    - List<Integer> snakes;
    - List<Integer> ladders;
    - Integer[][] board;
    - List<String> players;
    + movePlayer();
    + isWinner();
}
```

**Does this actually violate SRP?** Here's the nuance (from lecture):

Technically, you could argue it does NOT violate **Single Responsibility** in the strict sense — the class is called `SnakesAndLadders` and ALL of its fields (snakes, ladders, board, players) are related to the class name. Everything belongs to the context of the game.

**But it violates "Smallest Responsibility Principle"** — which is what SRP is really about in practice. SRP is also called the **Smallest Responsibility Principle** because the goal is to give each class the **smallest meaningful responsibility** it can have. Just because everything is "related to the game" doesn't mean it should all be in one class.

> **Key Insight:** "Single responsibility" doesn't mean "everything related to X goes in class X." It means "what is the **smallest, most focused** job this class can have?"

**Approach 2 (Follows SRP + Smallest Responsibility):** Break into smaller classes, each with one focused job.

```java
class Snake {
    - Integer start, end;
}

class Ladder {
    - Integer start, end;
}

class Board {
    - List<Snake> snakes;
    - List<Ladder> ladders;
    - Integer[][] cells;
    + move();
}

class Game {
    + isWinner();
}
```

**Why Approach 2 is better:**

| | Approach 1 (SnakesAndLadders) | Approach 2 (Separated) |
|-|-----|------|
| **Single Responsibility** | Debatable — all fields relate to the game name | Each class has exactly one job |
| **Smallest Responsibility** | Violates — one class doing everything | Follows — smallest meaningful unit |
| **Reason to change** | Many: snake rules, ladder rules, board layout, game logic | Each class has only ONE reason to change |
| **Reusability** | Can't reuse `Snake` logic elsewhere | `Snake`, `Ladder` are reusable components |

**Bottom line:** When someone says "SRP," think **smallest responsibility**, not just "single responsibility." If you can split a class further and each piece still makes sense on its own — split it.

### 3.2 O — Open/Closed Principle (OCP)

> **"Software entities should be open for extension but closed for modification."**

You should be able to add new functionality WITHOUT modifying existing, tested code.

**Bad Example (violates OCP):**

```java
public class DiscountCalculator {
    public double calculate(String customerType, double amount) {
        if (customerType.equals("REGULAR")) return amount * 0.1;
        if (customerType.equals("PREMIUM")) return amount * 0.2;
        if (customerType.equals("VIP")) return amount * 0.3;
        // Every new customer type = modify this class
        return 0;
    }
}
```

**Good Example (follows OCP):**

```java
public interface DiscountStrategy {
    double calculate(double amount);
}

public class RegularDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.1; }
}

public class PremiumDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.2; }
}

// Adding a new type? Just create a new class. No existing code touched.
public class EmployeeDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.4; }
}

public class DiscountCalculator {
    public double calculate(DiscountStrategy strategy, double amount) {
        return strategy.calculate(amount);
    }
}
```

**Key technique:** Use **Strategy Pattern** or **Polymorphism** to achieve OCP.

**Lecture Example — Invoice Discount System:**

**Real-world analogy:** Think of an **Electric Adapter** — you extend it by plugging in an adapter for different plug types. You don't modify the original socket.

**Bad (violates OCP):** Using if/else that needs modification for every new invoice type.

```java
public enum InvoiceType {
    ProposedInvoice,   // 50 INR discount
    FinalInvoice       // 100 INR discount
}

public class Invoice {
    public double getInvoiceDiscount(double amount, InvoiceType type) {
        double finalAmount = 0;
        if (type == InvoiceType.FinalInvoice) {
            finalAmount = amount - 100;
        } else if (type == InvoiceType.ProposedInvoice) {
            finalAmount = amount - 50;
        }
        // Adding a new type? Must modify this class! ← Violates OCP
        return finalAmount;
    }
}
```

**Good (follows OCP):** Use inheritance — extend with new classes, never modify existing ones.

```java
public class Invoice {
    public double getInvDiscount(double amount) {
        return amount - 10;  // Default discount
    }
}

public class FinalInvoice extends Invoice {
    @Override
    public double getInvDiscount(double amount) {
        return amount - 100;
    }
}

public class ProposedInvoice extends Invoice {
    @Override
    public double getInvDiscount(double amount) {
        return amount - 50;
    }
}

// NEW type? Just add a new class. No existing code touched!
public class RecurringInvoice extends Invoice {
    @Override
    public double getInvDiscount(double amount) {
        return amount - 30;
    }
}
```

**Caller code (the OCP win in action):**

The caller writes code **once** against the parent type `Invoice`. It doesn't know or care which subclass it's holding — Java's polymorphism picks the right `getInvDiscount()` at runtime.

```java
public class BillingService {

    // Accepts ANY Invoice — present or future
    public void printDiscount(Invoice invoice, double amount) {
        double finalPrice = invoice.getInvDiscount(amount);
        System.out.println("Final price: " + finalPrice);
    }

    public static void main(String[] args) {
        BillingService service = new BillingService();

        // Reference type = parent, object type = child (polymorphism)
        Invoice inv1 = new FinalInvoice();      // actual object: FinalInvoice
        Invoice inv2 = new ProposedInvoice();   // actual object: ProposedInvoice
        Invoice inv3 = new RecurringInvoice();  // actual object: RecurringInvoice

        service.printDiscount(inv1, 1000);  // 900  (FinalInvoice's method runs)
        service.printDiscount(inv2, 1000);  // 950  (ProposedInvoice's method runs)
        service.printDiscount(inv3, 1000);  // 970  (RecurringInvoice's method runs)
    }
}
```

**Adding a new type later — zero changes to caller:**

```java
// 1. Just add a new file:
public class TaxInvoice extends Invoice {
    @Override
    public double getInvDiscount(double amount) {
        return amount - 200;
    }
}

// 2. Use it — BillingService didn't change at all:
Invoice inv4 = new TaxInvoice();
service.printDiscount(inv4, 1000);   // 800 — works automatically
```

> **Why it works:** When you call `invoice.getInvDiscount(...)`, Java looks at the **actual object** in memory (`FinalInvoice`, `TaxInvoice`, etc.) — not the reference type — and runs *that* class's overridden method. This is called **dynamic dispatch** / **runtime polymorphism**, and it's the engine that makes OCP possible.

```
         ┌──────────────┐
         │   Invoice    │  ← Open for extension (new subclasses)
         │              │  ← Closed for modification (don't touch this)
         └──────┬───────┘
       ┌────────┼──────────┐
       ▼        ▼          ▼
┌──────────┐ ┌─────────┐ ┌───────────┐
│  Final   │ │Proposed │ │Recurring  │  ← Just extend!
│ Invoice  │ │ Invoice │ │ Invoice   │
└──────────┘ └─────────┘ └───────────┘
```

### 3.3 L — Liskov Substitution Principle (LSP)

> **"Objects of a superclass should be replaceable with objects of a subclass without breaking the application."**

If `B` extends `A`, then anywhere you use `A`, you should be able to drop in `B` and everything still works correctly.

**Bad Example (violates LSP):**

```java
public class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// Code that expects Bird to fly will BREAK with Penguin
public void makeBirdFly(Bird bird) {
    bird.fly();  // Throws exception if bird is a Penguin!
}
```

**Good Example (follows LSP):**

```java
public abstract class Bird {
    public abstract void move();
}

public class FlyingBird extends Bird {
    @Override
    public void move() {
        System.out.println("Flying...");
    }
}

public class Penguin extends Bird {
    @Override
    public void move() {
        System.out.println("Swimming...");
    }
}

// Now any Bird can be used safely
public void makeBirdMove(Bird bird) {
    bird.move();  // Works for ALL birds
}

// How it's called:
makeBirdMove(new FlyingBird());  // Output: "Flying..."
makeBirdMove(new Penguin());     // Output: "Swimming..."
// Both work perfectly — LSP satisfied!
```

**"But Bird is abstract — how can we replace parent with child?"**

This is a common confusion. LSP does NOT mean you literally create a `Bird` object and then swap it. It means:

> **Anywhere the code expects a `Bird` reference, you should be able to pass ANY child (`FlyingBird`, `Penguin`) and the program should still work correctly.**

```java
// The method signature says "give me a Bird"
public void makeBirdMove(Bird bird) {   // ← accepts parent TYPE
    bird.move();
}

// LSP says ALL of these should work without breaking:
makeBirdMove(new FlyingBird());   // ✅ child 1 works
makeBirdMove(new Penguin());      // ✅ child 2 works
// No if/else, no type checking, no surprises
```

**The old Penguin problem (before fix) violated LSP because:**
```java
class Bird {
    void fly() { ... }  // concrete method — "all birds fly"
}
class Penguin extends Bird {
    void fly() { throw new UnsupportedOperationException(); }  // 💥 BREAKS!
}

makeBirdFly(new Penguin());  // 💥 Crashes! Caller expected fly() to work
```

Here, replacing `Bird` with `Penguin` **breaks the program** — that's an LSP violation. The fix was to use `abstract void move()` instead of `void fly()`, so every child defines its OWN valid behavior.

**Key takeaway:** LSP is about the **reference type** (parameter, variable), not about instantiating the parent. Abstract classes actually HELP enforce LSP because they force every child to provide an implementation — you can't accidentally inherit a method that doesn't make sense.

**Lecture Example — PaymentMethod:**

If you have a parent class `PaymentMethod` with subclasses `CreditCard`, `PhoneBanking`, `UPI`, and `MobileWallet`, every subclass should be usable wherever `PaymentMethod` is expected — without the caller needing to check the type.

```
         ┌──────────────────┐
         │  PaymentMethod   │
         └────────┬─────────┘
     ┌────────┬───┴───┬──────────┐
     ▼        ▼       ▼          ▼
┌────────┐ ┌────┐ ┌─────┐ ┌──────────┐
│Credit  │ │ PB │ │ UPI │ │  Mobile  │
│Card    │ │    │ │     │ │  Wallet  │
└────────┘ └────┘ └─────┘ └──────────┘
```

If any subclass breaks when substituted in (throws an exception, behaves unexpectedly), it violates LSP.

**Deep Dive — When does the application actually break? (All scenarios)**

Let's walk through every case:

**Scenario 1: Parent is ABSTRACT, child implements properly — LSP SAFE**
```java
abstract class PaymentMethod {
    abstract void pay(double amount);  // every child MUST implement
}

class UPI extends PaymentMethod {
    void pay(double amount) { /* UPI logic */ }   // ✅ works
}

class CreditCard extends PaymentMethod {
    void pay(double amount) { /* card logic */ }  // ✅ works
}

// Caller:
void checkout(PaymentMethod pm) {
    pm.pay(500);  // Always works — abstract forces all children to implement
}
checkout(new UPI());         // ✅
checkout(new CreditCard());  // ✅
```
**Why it's safe:** Abstract class forces every child to implement `pay()`. You literally can't create a child that "forgets" to implement it — compiler won't allow it.

**Scenario 2: Parent is CONCRETE (not abstract), child inherits but OVERRIDES badly — LSP BREAKS**
```java
class PaymentMethod {
    void pay(double amount) {
        // default: deducts from account
        processPayment(amount);
    }
}

class CryptoCoin extends PaymentMethod {
    @Override
    void pay(double amount) {
        throw new UnsupportedOperationException("Crypto not supported yet!");
        // 💥 BREAKS the contract — caller expected pay() to work
    }
}

// Caller:
void checkout(PaymentMethod pm) {
    pm.pay(500);  // 💥 Crashes if pm is CryptoCoin!
}
checkout(new CryptoCoin());  // 💥 LSP VIOLATION
```
**Why it breaks:** Parent promises "call `pay()` and payment happens." Child says "nope, I throw an exception." The caller trusted the parent's contract but the child broke it.

**Scenario 3: Parent is CONCRETE, child inherits but CHANGES behavior silently — LSP BREAKS (subtle)**
```java
class PaymentMethod {
    void pay(double amount) {
        deduct(amount);  // deducts exact amount
    }
}

class ShadyWallet extends PaymentMethod {
    @Override
    void pay(double amount) {
        deduct(amount * 1.1);  // silently charges 10% extra! 💀
    }
}

// Caller:
checkout(new ShadyWallet());  // Doesn't crash, but WRONG behavior
```
**Why it breaks:** No exception, but the child **silently violates the parent's contract** (parent says "deduct exact amount", child deducts more). The caller can't tell something is wrong — this is the most dangerous LSP violation.

**Scenario 4: A completely unrelated class — WON'T EVEN COMPILE**
```java
class Pizza { }  // not a PaymentMethod

void checkout(PaymentMethod pm) {
    pm.pay(500);
}
checkout(new Pizza());  // ❌ COMPILE ERROR — Pizza is not a PaymentMethod
```
**Why it's safe:** Java's type system prevents this at compile time. Only subclasses of `PaymentMethod` can be passed.

**Summary — When does the app break?**

| Scenario | Parent Type | Child Behavior | Result |
|----------|-----------|---------------|--------|
| Abstract parent, child implements | Abstract | Implements correctly | ✅ **Safe** — compiler forces implementation |
| Concrete parent, child inherits as-is | Concrete | Doesn't override | ✅ **Safe** — uses parent's logic |
| Concrete parent, child throws exception | Concrete | Overrides with `throw` | 💥 **LSP violation** — runtime crash |
| Concrete parent, child changes logic silently | Concrete | Overrides with wrong behavior | 💀 **LSP violation** — silent bug (worst kind) |
| Unrelated class passed | Any | N/A | ❌ **Won't compile** — type safety |

> **Key Takeaway:** Abstract classes are actually your BEST FRIEND for LSP — they make it impossible to accidentally inherit a default behavior that doesn't apply. Concrete parent classes are riskier because a child can override methods in ways that violate the contract without the compiler catching it.

**Interview tip:** LSP violations often show up as `if (obj instanceof SpecificType)` checks scattered through code. That's a code smell.

### 3.4 I — Interface Segregation Principle (ISP)

> **Note:** ISP was not covered in this lecture. It will be covered in an upcoming lecture. The content below is provided for completeness.

> **"No client should be forced to depend on methods it does not use."**

Don't create fat, bloated interfaces. Break them into smaller, focused ones.

**Bad Example (violates ISP):**

```java
public interface Worker {
    void work();
    void eat();
    void sleep();
}

public class Robot implements Worker {
    public void work() { /* works */ }
    public void eat()  { /* Robots don't eat! */ }   // Forced to implement
    public void sleep() { /* Robots don't sleep! */ } // Forced to implement
}
```

**Good Example (follows ISP):**

```java
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public interface Sleepable {
    void sleep();
}

public class Human implements Workable, Eatable, Sleepable {
    public void work()  { /* works */ }
    public void eat()   { /* eats */ }
    public void sleep() { /* sleeps */ }
}

public class Robot implements Workable {
    public void work() { /* works */ }
    // No forced empty implementations!
}
```

**Real-world analogy:** A restaurant menu shouldn't force vegetarians to read through all non-veg items. Separate menus (interfaces) for different needs.

### 3.5 D — Dependency Inversion Principle (DIP)

> **Note:** DIP was not covered in this lecture. It will be covered in an upcoming lecture. The content below is provided for completeness.

> **"High-level modules should not depend on low-level modules. Both should depend on abstractions."**

Don't hardcode dependencies to concrete classes. Depend on interfaces/abstractions instead.

**Bad Example (violates DIP):**

```java
public class MySQLDatabase {
    public void save(String data) { /* MySQL specific save */ }
}

public class UserService {
    private MySQLDatabase db = new MySQLDatabase();  // Tightly coupled!

    public void createUser(String name) {
        db.save(name);  // What if we switch to PostgreSQL? Change this class.
    }
}
```

**Good Example (follows DIP):**

```java
public interface Database {
    void save(String data);
}

public class MySQLDatabase implements Database {
    public void save(String data) { /* MySQL save */ }
}

public class PostgreSQLDatabase implements Database {
    public void save(String data) { /* PostgreSQL save */ }
}

public class UserService {
    private Database db;  // Depends on abstraction!

    public UserService(Database db) {  // Injected via constructor
        this.db = db;
    }

    public void createUser(String name) {
        db.save(name);  // Works with ANY database
    }
}

// Usage — easy to swap implementations
UserService service = new UserService(new PostgreSQLDatabase());
```

**Key technique:** **Dependency Injection** (constructor injection, setter injection) is how you implement DIP in practice. Spring Framework is built on this principle.

---

## 4. SOLID Principles - Comparison Table

| Principle | Focus | Problem it Solves | Key Question to Ask |
|-----------|-------|-------------------|---------------------|
| **SRP** | One class = one job | Classes doing too many things | "Does this class have more than one reason to change?" |
| **OCP** | Extend, don't modify | Adding features breaks existing code | "Can I add new behavior without touching existing code?" |
| **LSP** | Substitutability | Subclasses breaking parent contracts | "Can I replace parent with child without issues?" |
| **ISP** | Small interfaces | Classes forced to implement irrelevant methods | "Is any implementor forced to have empty/dummy methods?" |
| **DIP** | Depend on abstractions | Tight coupling to concrete classes | "Am I directly `new`-ing dependencies inside a class?" |

### How They Relate

```
SRP ──► Keeps classes focused
 │
 ▼
OCP ──► Focused classes are easier to extend without modification
 │
 ▼
LSP ──► Extensions (subclasses) must honor parent contracts
 │
 ▼
ISP ──► Contracts (interfaces) should be small and specific
 │
 ▼
DIP ──► Depend on those small interfaces, not concrete implementations
```

---

## 5. What is System Design?

### 5.1 Friendly Definition First

> **Friendly definition:** System Design is the **blueprint stage** of building software. Before anyone writes a line of code, you decide **what big pieces exist, how they're shaped, how they fit together, and how data flows through them** — so the final software actually does what it's supposed to do.

#### Real-world analogy — building a house

Before construction starts, an architect produces:

| Document | What it shows | Software equivalent |
|---|---|---|
| Floor plan | Rooms, their sizes, where doors are | **Architecture** — major pieces and their layout |
| Room labels (kitchen, bath, bedroom) | What each room is for | **Components / modules** — what each piece does |
| Doors and hallways | How rooms connect, who can walk where | **Interfaces** — how pieces talk to each other |
| Plumbing & wiring diagrams | How water/electricity flows | **Data flow** — how information moves |

You don't start laying bricks until those drawings exist. Same with software — System Design is the drawings phase.

### 5.2 Formal Definition (for interviews)

> **System Design is a process of defining the architecture, components, modules, interfaces, and data for a system to satisfy specified requirements.**

This sentence is dense. Let's unpack each of the **5 elements** in plain English with concrete examples.

### 5.3 The 5 Elements of System Design — broken down

Every word in that one-liner means something specific. Here's what each one is **and a concrete example for both LLD and HLD** so you can recognize them in any question.

#### Element 1 — Architecture

> *"What's the overall shape of the system?"*

**In plain English:** The big-picture layout — how the major pieces are organized at the top level. Architecture answers "**what is the overall structure**?"

| Level | Example |
|---|---|
| **HLD** | Twitter is split into User Service, Tweet Service, Timeline Service, Notification Service, all behind an API Gateway, talking to a SQL DB + a Redis cache + a Kafka bus. |
| **LLD** | Inside the **Order Module**, the architecture is: `OrderController → OrderService → OrderRepository → DB`, with helper utilities (`PriceCalculator`, `DiscountEngine`) on the side. |

Think of architecture as the **skeleton diagram** before any details are filled in.

#### Element 2 — Components

> *"What are the major buildings on the campus?"*

**In plain English:** A **component** is a self-contained, reusable piece of the system that does one thing well. It's bigger than a class but smaller than the whole system.

| Level | Example |
|---|---|
| **HLD** | An "Authentication Service" is a component. A "Payment Gateway" is a component. A "Notification Service" is a component. |
| **LLD** | Inside an Order module, the `PriceCalculator` is a component. The `DiscountEngine` is a component. The `OrderValidator` is a component. They're each a focused, reusable piece. |

> **In LLD specifically, "component" usually means: a class or small group of classes that delivers ONE clear capability.** That's why the lecture uses the word — components in LLD are the building blocks like `Cart`, `Inventory`, `User`, `PaymentProcessor`, etc.

A useful test: *"Could I lift this out and reuse it elsewhere?"* If yes → it's a component. If it's tangled with everything else → it's not yet a clean component.

#### Element 3 — Modules

> *"How do I group related stuff into folders?"*

**In plain English:** A **module** is a logical grouping of related components/classes that belong together. Modules are the **folders** of a software system.

| Level | Example |
|---|---|
| **HLD** | Zomato has a `restaurant-service` module, a `delivery-service` module, an `analytics-service` module. Each module is independently deployable. |
| **LLD** | A Java project might have `com.shop.order`, `com.shop.payment`, `com.shop.user` — each is a module containing related classes. |

> **Component vs Module — easy distinction:** A **component** does ONE thing (`PriceCalculator`). A **module** is a *group* of related components (`OrderModule` containing PriceCalculator + DiscountEngine + TaxComputer + OrderValidator). Module = parent folder; components = files inside.

#### Element 4 — Interfaces

> *"How do these pieces talk to each other?"*

**In plain English:** An **interface** is a *contract* — it says *"if you want to talk to me, you must use these specific methods/endpoints with these inputs and outputs."* It hides internal details.

There are two flavors depending on the layer:

| Level | What "interface" usually means |
|---|---|
| **HLD** | A **REST API** (`POST /orders`, `GET /users/{id}`) or a **gRPC contract**. Service A talks to Service B by sending HTTP requests defined in an OpenAPI spec. |
| **LLD** | A **Java `interface`** (`PaymentProcessor`, `Comparator`) or method signatures of a class. Client classes only know the interface, not the concrete implementation. |

> Interfaces are the **doors** of components. They define *what comes in and what goes out* without exposing the messy internals.

**Example — same idea at both levels:**
- HLD interface: `POST /payments {amount, method}` — client doesn't know if Stripe or Razorpay handles it.
- LLD interface: `interface PaymentProcessor { void pay(double amount); }` — caller doesn't know if it's `UPIPayment` or `CardPayment`.

#### Element 5 — Data

> *"What information does the system remember and shuffle around?"*

**In plain English:** Data refers to **what's stored, where it's stored, what shape it has, and how it flows**. This includes:

| Aspect | What it covers |
|---|---|
| **Data model** | The shape — fields, types, relationships (e.g., `Order has many OrderItems`) |
| **Data storage** | Where it lives — SQL DB, NoSQL, cache, file, message queue |
| **Data flow** | How info moves between components — request/response, event publishing, etc. |

| Level | Example |
|---|---|
| **HLD** | "User profile data lives in PostgreSQL. Session tokens live in Redis. Tweets are stored in Cassandra. Events flow through Kafka." |
| **LLD** | "An `Order` has fields `id, customerId, items[], total, status`. `OrderItem` has `productId, quantity, price`. We persist via JPA into a `MySQL` `orders` table joined with `order_items`." |

### 5.4 Putting the 5 Elements Together — a worked example

Suppose you're asked: *"Design the LLD for an Online Shopping Cart."*

Walking through the 5 elements gives you a complete answer:

| Element | What you'd say |
|---|---|
| **Architecture** | A `CartController` exposes endpoints; it calls a `CartService` for business logic; `CartService` uses `ProductRepository` and `CartRepository` for persistence. |
| **Components** | `Cart`, `CartItem`, `Product`, `DiscountEngine`, `TaxCalculator`, `CartValidator`. |
| **Modules** | `com.shop.cart`, `com.shop.product`, `com.shop.pricing`. |
| **Interfaces** | `interface DiscountStrategy { double apply(Cart cart) }`, `interface CartRepository { Cart findById(...) }`. |
| **Data** | `Cart {id, userId, items: List<CartItem>, total}`; `CartItem {productId, quantity, unitPrice}`; persisted in MySQL `carts` + `cart_items` tables. |

Now you've answered the question completely — every word in the formal definition has a real anchor in your design. **This is the recipe for any LLD interview.**

### 5.5 LLD vs HLD — Where Each Element Lives

```
       SYSTEM DESIGN
           │
   ┌───────┴───────┐
   ▼               ▼
 HLD              LLD
("the whole     ("the inside of
  campus")       one building")

 Architecture   Architecture
 = services     = classes &
   & DBs          their layers
 Components     Components
 = services     = classes
 Modules        Modules
 = deployable   = packages /
   services       folders
 Interfaces     Interfaces
 = REST APIs    = Java interfaces
                  & method sigs
 Data           Data
 = DB choices,  = entity classes
   schemas,       & their fields
   data flow
```

Same vocabulary, different zoom level.

### 5.6 The Two Major Areas

It splits into two major areas:

```
                    System Design
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       ┌─────────────┐      ┌─────────────┐
       │     LLD     │      │     HLD     │
       │  Low-Level  │      │  High-Level │
       │   Design    │      │   Design    │
       └─────────────┘      └─────────────┘
```

### LLD vs HLD — Key Differences

| | LLD (Low-Level Design) | HLD (High-Level Design) |
|-|----------------------|------------------------|
| **What** | Detail design of individual components | Overall system architecture |
| **When** | During later stages of development | During initial stages of development |
| **View** | Low-level (zoomed in) | High-level (zoomed out) |
| **Example** | Order Management module of Zomato | Zomato as a whole platform |

### What LLD Covers (Topics to Learn)

```
LLD
 ├── OOPs                    ← (1) Foundation
 ├── SOLID Principles        ← (2) Design guidelines
 ├── Design Patterns         ← (3) Reusable solutions
 ├── UML Diagrams            ← (4) Visual communication
 ├── Class Diagrams          ← (5) Structure of classes
 ├── Data Modeling           ← Database schema for a component
 ├── API Design              ← REST endpoints for a module
 └── Data Structures         ← Choosing right DS for the problem
```

### What HLD Covers (Topics to Learn)

```
HLD
 ├── Architecture Design  ┐
 │                        ├── FR (Functional Requirements)
 ├── Component Design     ┘    "What should the system DO?"
 │
 ├── Scalability          ┐
 ├── Availability         │
 │                        ├── NFR (Non-Functional Requirements)
 ├── Performance          │    "How should the system PERFORM?"
 └── Consistency          ┘
```

**FR (Functional Requirements)** — What the system does (features, user flows)
**NFR (Non-Functional Requirements)** — How well the system does it (speed, uptime, scale)

### Recognizing LLD Questions in Interviews

**Interview signals — the question is LLD when:**
- "Design a parking lot system"
- "Design a chess game"
- "Design an elevator system"
- "Design a library management system"
- "Design a vending machine"
- Keywords: *classes, objects, methods, patterns*

**What's expected:**
1. Identify entities (classes)
2. Define attributes and methods
3. Establish relationships (IS-A, HAS-A)
4. Apply design patterns (Strategy, Observer, Factory, etc.)
5. Draw UML class diagrams
6. Write code skeleton

**Example — "Design a Parking Lot":**
- Classes: `ParkingLot`, `Floor`, `ParkingSpot`, `Vehicle`, `Car`, `Truck`, `Ticket`
- Patterns: Strategy (pricing), Factory (vehicle creation)
- Focus: Class relationships, method signatures, state management

### Recognizing HLD Questions in Interviews

**Interview signals — the question is HLD when:**
- "Design Twitter/Instagram"
- "Design a URL shortener"
- "Design a chat application"
- "Design a notification system"
- Keywords: *scalability, availability, databases, APIs, load balancing, microservices*

**What's expected:**
1. Requirements gathering (functional + non-functional)
2. API design
3. Database schema (SQL vs NoSQL choice)
4. Service architecture (monolith vs microservices)
5. Scaling strategies (horizontal, caching, CDN)
6. Trade-offs discussion

**Example — "Design Twitter":**
- Services: User Service, Tweet Service, Timeline Service, Notification Service
- Databases: User data (SQL), Tweets (NoSQL), Timeline cache (Redis)
- Focus: How services communicate, how to handle 500M tweets/day

### Quick Decision Matrix

| Signal | LLD | HLD |
|--------|-----|-----|
| "Design a game / system component" | Yes | |
| "Design a web-scale application" | | Yes |
| "Focus on classes and objects" | Yes | |
| "Focus on scalability and APIs" | | Yes |
| "How would you implement this feature?" | Yes | |
| "How would you architect this platform?" | | Yes |
| Mentions concurrent users / throughput | | Yes |
| Mentions design patterns | Yes | |

---

## 6. The Diamond Problem

The Diamond Problem is a classic OOP interview question that explains why languages like Java don't support multiple inheritance (with classes).

### What is it?

When a class inherits from two classes that both inherit from a common ancestor, it creates a diamond-shaped inheritance hierarchy.

```
        ┌─────────┐
        │  Animal  │
        │ +speak() │
        └────┬────┘
           ┌─┴──┐
           ▼    ▼
     ┌─────────┐  ┌───────────┐
     │   Dog   │  │    Cat    │
     │+speak() │  │ +speak()  │
     └────┬────┘  └─────┬─────┘
          └──────┬──────┘
                 ▼
          ┌────────────┐
          │   DogCat   │
          │  speak()=? │  ◄── AMBIGUITY: Which speak()?
          └────────────┘
```

### The Problem

```java
// Hypothetical (NOT valid Java)
class Animal {
    void speak() { System.out.println("..."); }
}

class Dog extends Animal {
    void speak() { System.out.println("Bark!"); }
}

class Cat extends Animal {
    void speak() { System.out.println("Meow!"); }
}

// If Java allowed this:
class DogCat extends Dog, Cat {   // COMPILATION ERROR in Java
    // Which speak() is inherited? Dog's or Cat's?
    // Which Animal constructor runs? Once or twice?
}
```

### Issues Caused

1. **Method Ambiguity:** If both parents override the same method, the compiler can't determine which version the child should inherit.
2. **State Duplication:** The grandparent's fields might get initialized twice (once through each parent path), leading to inconsistent state.
3. **Constructor Chain Confusion:** Which parent's constructor runs first? Does the grandparent constructor run once or twice?

### Why Java Doesn't Support Multiple Inheritance (with classes)

Java's designers made a deliberate choice: **simplicity > power**. Multiple class inheritance introduces complexity that's hard to resolve cleanly.

**Java's solution — Interfaces:**

```java
interface Barkable {
    void bark();
}

interface Meowable {
    void meow();
}

class DogCat implements Barkable, Meowable {
    @Override
    public void bark() { System.out.println("Bark!"); }

    @Override
    public void meow() { System.out.println("Meow!"); }
}
```

Since interfaces (prior to Java 8) had no implementation, there was no ambiguity. With Java 8+ default methods, if two interfaces provide the same default method, the implementing class **must** explicitly override it:

```java
interface A {
    default void greet() { System.out.println("Hello from A"); }
}

interface B {
    default void greet() { System.out.println("Hello from B"); }
}

class C implements A, B {
    @Override
    public void greet() {
        A.super.greet();  // Explicitly choose, or provide own logic
    }
}
```

### How Other Languages Handle It

| Language | Approach |
|----------|----------|
| **Java** | No multiple class inheritance; multiple interface implementation allowed |
| **C++** | Allows multiple inheritance; uses virtual inheritance to resolve diamond |
| **Python** | Allows multiple inheritance; uses MRO (Method Resolution Order / C3 linearization) |
| **Kotlin** | Same as Java; interfaces with default methods, explicit conflict resolution |

---

## 7. UML Diagrams & Relationships

UML (Unified Modeling Language) class diagrams are the standard way to communicate LLD in interviews. You must be able to draw and read them.

### Why UML?

- **Complexity** — Systems are too complex to describe in words alone
- **Non-Programmers** — PMs, architects, stakeholders can understand visual diagrams without reading code
- **Visualization** — Easier to spot design flaws, missing relationships, and redundancies visually

### What UML Covers

- **Structure** — Classes, objects, modules and how they are organized
- **Behavior** — How components interact with each other
- **Relationships** — How classes relate (inheritance, composition, etc.)

### UML Diagram Types — Which One for What?

UML is **not just one diagram** — it's a family of ~14 diagram types, split into two big groups: **Structure diagrams** (what the system *is*) and **Behavior diagrams** (what the system *does*).

For LLD, you mainly use **5 of them**. Here's the full landscape so you know which one to draw for which question.

#### The two families

```
                     UML Diagrams
                          │
        ┌─────────────────┴─────────────────┐
        ▼                                   ▼
  STRUCTURE (the "is")              BEHAVIOR (the "does")
   what the system                  how the system reacts,
   contains, statically              flows, and changes
        │                                   │
   ┌────┼────┐                ┌────┬────┬───┴───┬────┐
   ▼    ▼    ▼                ▼    ▼    ▼       ▼    ▼
 Class Object Component      Use   Sequence  Activity State
       diagram diagram      Case   diagram   diagram  diagram
       (etc.)              diagram
```

#### The 7 most-used UML diagrams (with their job)

| # | Diagram | Family | Answers the question | Use when |
|---|---|---|---|---|
| 1 | **Class diagram** | Structure | *"What classes exist and how are they related?"* | Designing the static skeleton of code (LLD's #1 diagram). |
| 2 | **Object diagram** | Structure | *"What does the system look like at one point in time?"* | Showing a concrete snapshot — e.g., "this Order has 3 OrderItems and 1 Customer." |
| 3 | **Component diagram** | Structure | *"What are the deployable building blocks of the system?"* | Higher-level than classes — e.g., AuthService, PaymentService, NotificationService boxes. |
| 4 | **Sequence diagram** | Behavior | *"In what order do objects send messages to each other?"* | Showing a single workflow step-by-step (e.g., the login flow). |
| 5 | **Use case diagram** | Behavior | *"What can a user do with the system?"* | Capturing requirements / actors / system boundaries. |
| 6 | **Activity diagram** | Behavior | *"What's the flow of work / decisions?"* | Modeling a process or business workflow with branches. |
| 7 | **State diagram** | Behavior | *"What states can an object be in, and how does it transition?"* | Modeling lifecycle of an entity (e.g., Order: PLACED → SHIPPED → DELIVERED). |

#### Quick "which diagram for which question" cheat sheet

> *Train yourself to map an interview prompt to the right diagram instantly.*

| Interview prompt fragment | Reach for this diagram |
|---|---|
| *"Show me the class hierarchy."* | **Class diagram** |
| *"Show how Login works step by step."* | **Sequence diagram** |
| *"Show what an Order looks like with 3 items right now."* | **Object diagram** |
| *"Show all the actors and what they can do."* | **Use case diagram** |
| *"Walk me through the checkout flow with branches."* | **Activity diagram** |
| *"What states does an Order go through?"* | **State diagram** |
| *"What are the major services in the system?"* | **Component diagram** |
| *"How would you deploy this to AWS?"* | **Deployment diagram** |

#### Specifically for the user's two questions

> **Q: *"Which diagram is used to illustrate interactions between components?"*** → **Sequence diagram** (the most common). For richer "who-talks-to-whom" views you can also use **Communication diagrams** (older, less popular). Component diagrams show *which* components exist; sequence diagrams show *how they talk* over time.
>
> **Q: *"What diagram is useful for visualizing the structure of a system?"*** → **Class diagram** (for code-level structure in LLD) and **Component diagram** (for service-level structure in HLD). Together they form the "static structure" view of the system.

#### Tiny visual examples — what each diagram actually looks like

**Class diagram** (you've seen lots of these in Lecture 3):
```
┌──────────┐       ┌────────────┐
│  Order   │◆──────│ OrderItem  │
│  -id     │ 1   * │  -product  │
│  +pay()  │       │  -qty      │
└──────────┘       └────────────┘
```

**Sequence diagram** (interactions over time, top-to-bottom):
```
Client      OrderService     Payment    Inventory
  │              │              │           │
  │─placeOrder─▶│              │           │
  │              │──validate──▶│           │
  │              │◀─────ok─────│           │
  │              │──charge────▶│           │
  │              │◀────done────│           │
  │              │──reserve──────────────▶│
  │◀─orderConfirmed──            │           │
```

**Use case diagram** (actors and their interactions with the system):
```
       ┌──────── E-commerce System ────────┐
                  ○ Browse Products
   👤            ○ Add to Cart
 Customer        ○ Checkout
                  ○ Track Order
       └──────────────────────────────────┘
```

**State diagram** (lifecycle of an Order):
```
   ●──▶ [PLACED] ──pay──▶ [PAID] ──ship──▶ [SHIPPED]
                              │              │
                              │           deliver
                              ▼              ▼
                          [CANCELLED]    [DELIVERED]
```

**Activity diagram** (workflow with decisions):
```
   ● ── login ── (auth ok?) ──Yes──▶ show dashboard ──▶ ●
                     │
                     No
                     ▼
                show error
```

**Component diagram** (deployable services):
```
┌─ Frontend ─┐    ┌─ API Gateway ─┐    ┌─ Auth Service ─┐
│   React     │──▶│               │───▶│                │
└────────────┘    │               │    └────────────────┘
                   │               │    ┌─ Order Service ┐
                   │               │───▶│                │
                   └───────────────┘    └────────────────┘
```

#### What you'll typically draw in an LLD interview

| Order of importance | Diagram | Why |
|---|---|---|
| 1 (must) | **Class diagram** | Shows the code structure — interviewer's primary checkpoint. |
| 2 (very common) | **Sequence diagram** | Shows that you understand the runtime flow. |
| 3 (when relevant) | **State diagram** | If the entity has clear states (Order, Job, Document, etc.). |
| 4 (sometimes) | **Use case diagram** | Up front to confirm requirements & actors. |
| 5 (rare in LLD) | Activity / Component / Deployment | Mostly HLD territory. |

> **Memory aid:** If the question is about **what's there**, draw a **structure** diagram (class / object / component). If the question is about **how it behaves**, draw a **behavior** diagram (sequence / state / activity / use case).

### UML Class Diagram — Box Format

Every class in a UML diagram is represented as a **3-section box**:

```
┌─────────────────────────┐
│       Class Name         │  ← Section 1: NAME of the class
├─────────────────────────┤
│ - attribute1 : Type      │  ← Section 2: ATTRIBUTES (fields/properties)
│ - attribute2 : Type      │     - means private
│ + attribute3 : Type      │     + means public
├─────────────────────────┤
│ + method1() : ReturnType │  ← Section 3: METHODS (behaviors)
│ + method2() : ReturnType │
│ - helperMethod() : void  │
└─────────────────────────┘
```

**Access Modifier Notation:**

| Symbol | Meaning | Java Keyword |
|--------|---------|-------------|
| `-` | Private | `private` |
| `+` | Public | `public` |
| `#` | Protected | `protected` |
| `~` | Package/Default | (no keyword) |

### Example — Animal Hierarchy (from lecture)

```
                  ┌────────────────────┐
                  │       Animal       │
                  ├────────────────────┤
                  │ - age : int        │
                  │ - gender : String  │
                  ├────────────────────┤
                  │ + isMammal()       │
                  │ + mate()           │
                  └─────────┬──────────┘
                            △
            ┌───────────────┼───────────────┐
            │               │               │
┌───────────┴──────┐ ┌─────┴──────────┐ ┌──┴───────────────┐
│      Duck        │ │     Fish       │ │      Zebra       │
├──────────────────┤ ├────────────────┤ ├──────────────────┤
│ - beakColor :    │ │ - sizeInFt :   │ │ - isWild : bool  │
│     String       │ │     int        │ │                  │
│                  │ │ - canEat : bool│ │                  │
├──────────────────┤ ├────────────────┤ ├──────────────────┤
│ + swim()         │ │ + swim()       │ │ + run()          │
│ + quack()        │ │                │ │                  │
└──────────────────┘ └────────────────┘ └──────────────────┘
```

**Reading this diagram:**
- `Animal` is the parent class with private fields (`-age`, `-gender`) and public methods (`+isMammal()`, `+mate()`)
- `Duck`, `Fish`, `Zebra` inherit from `Animal` (△ arrow = generalization)
- Each child adds its own attributes and methods
- `-beakColor` is private to Duck; `+swim()` is public

### Arrow Types — Quick Reference

```
─────────────────────────────────────────────────────────
 Arrow Type          │ Symbol              │ Meaning
─────────────────────────────────────────────────────────
 Solid line + open   │ ────────▷           │ Inheritance
   triangle          │                     │ (Generalization / IS-A)
─────────────────────────────────────────────────────────
 Dashed line + open  │ - - - - ▷           │ Implementation
   triangle          │                     │ (Realization)
─────────────────────────────────────────────────────────
 Solid line + open   │ ────────>           │ Association
   arrowhead         │                     │ (HAS-A, uses)
─────────────────────────────────────────────────────────
 Dashed line + open  │ - - - - >           │ Dependency
   arrowhead         │                     │ (temporary use)
─────────────────────────────────────────────────────────
 Solid line +        │ ────────◆           │ Composition
   filled diamond    │                     │ (strong HAS-A, owns)
─────────────────────────────────────────────────────────
 Solid line +        │ ────────◇           │ Aggregation
   empty diamond     │                     │ (weak HAS-A, has)
─────────────────────────────────────────────────────────
```

### Solid Arrow vs Dashed Arrow vs Filled Arrow

| Arrow | Style | Represents | Strength |
|-------|-------|-----------|----------|
| **Solid line + hollow triangle** `────▷` | Solid | **Generalization** (class extends class) | Permanent, structural |
| **Dashed line + hollow triangle** `- - -▷` | Dashed | **Realization** (class implements interface) | Contractual |
| **Solid line + filled diamond** `◆────` | Filled diamond | **Composition** (part can't exist without whole) | Strongest ownership |
| **Solid line + hollow diamond** `◇────` | Hollow diamond | **Aggregation** (part CAN exist without whole) | Weak ownership |
| **Dashed line + arrow** `- - ->` | Dashed | **Dependency** (temporary/method-level use) | Weakest |

### Weak vs Strong Association

**Strong Association (Composition):**
- Part CANNOT exist without the whole
- If the whole is destroyed, parts are destroyed too
- Example: `Room` inside a `House` — destroy the house, rooms are gone

**Weak Association (Aggregation):**
- Part CAN exist independently
- If the whole is destroyed, parts survive
- Example: `Student` in a `University` — university closes, students still exist

```
 STRONG (Composition)              WEAK (Aggregation)
 ┌─────────┐  ◆──── ┌────────┐   ┌────────────┐  ◇──── ┌──────────┐
 │  House   │       │  Room  │   │ University │       │ Student  │
 └─────────┘       └────────┘   └────────────┘       └──────────┘
  Destroy House                   Close University
  → Rooms destroyed               → Students still exist
```

### Multiplicity — Showing "How Many" on a Relationship

In a class diagram, the line connecting two classes is **labeled at each end with a number** that says *"how many objects of this class participate in this relationship."* This number is called **multiplicity**.

#### Real-world analogy

Imagine a wedding hall seating chart. The hall manager writes:
> *"Each table seats 6–8 people."*

That `6..8` is multiplicity for the *table-to-chairs* relationship.

In UML, the *exact same idea* applies between classes.

#### The notation

| Symbol | Means | Plain English |
|---|---|---|
| `1` | Exactly one | "Exactly 1" |
| `0..1` | Zero or one | "At most 1, may be missing" |
| `*` or `0..*` | Zero or more | "Any number, including none" |
| `1..*` | One or more | "At least 1, possibly many" |
| `n..m` | Between `n` and `m` | "Between n and m, inclusive" |

#### How to read multiplicity on a diagram

```
┌──────────┐  1            1..*  ┌──────┐
│  Order   │──────────────────────│ Item │
└──────────┘                      └──────┘
```

Read **the number near a class as "how many of this class participate"**:

- *"Each `Order` has `1..*` (one or more) `Items`."*
- *"Each `Item` belongs to `1` (exactly one) `Order`."*

So multiplicities go in **pairs**, one at each end. Always read them from "this end" to "the class at the other end".

#### Easy domain examples

```
┌──────────┐  1           1     ┌──────────┐
│   User   │─────────────────────│ Profile  │     "User has exactly 1 Profile"
└──────────┘                     └──────────┘

┌──────────┐  1           0..1  ┌──────────┐
│  Driver  │─────────────────────│ License  │     "Driver may or may not have a License"
└──────────┘                     └──────────┘

┌──────────┐  1           1..*  ┌──────────┐
│  Course  │─────────────────────│ Student  │     "Course has at least 1 Student"
└──────────┘                     └──────────┘

┌──────────┐  *             *   ┌──────────┐
│  Author  │─────────────────────│   Book   │     "Many-to-many: an Author writes
└──────────┘                     └──────────┘      many Books; a Book can have
                                                   many Authors"
```

#### Industry examples — the same idea in real systems

| Real system | Relationship | Multiplicity |
|---|---|---|
| Spring `JpaRepository` | A `User` entity → many `Order` entities | `1` to `*` |
| Twitter | A `Tweet` → 0..1 retweet of another tweet | `*` to `0..1` |
| GitHub | A `User` → many `Repositories` they can read | `*` to `*` |
| Banking | A `Customer` → 1..* `Account`s | `1` to `1..*` |
| Uber | A `Ride` → exactly 1 `Driver` and exactly 1 `Rider` | `1` to `1` (twice) |

#### Why multiplicity matters in interviews

If your class diagram has *no* multiplicity labels, an interviewer immediately spots it as incomplete — they won't know whether your `Order` has one item or thousands. Always label both ends. **A diagram without multiplicity is a half-finished diagram.**

### Arrow Direction — Who Knows Whom

The *direction* of an arrow on a relationship line tells you a specific thing: **which class can navigate to the other in code**. In other words, *who has whom as a field*.

#### Three cases

```
1) Unidirectional      Customer ────────────────► Order
   (one-way)           Customer can find its Orders.
                       Order does NOT know its Customer.

2) Bidirectional       Customer ◄───────────────► Order
   (two-way)           Both sides know each other.

3) Undirected          Customer ──────────────── Order
   (no arrowhead)      Just says "they are related" without
                       saying who navigates to whom.
```

#### Why this matters in code

The direction is **literally about which class has a field referencing the other**:

```java
// Unidirectional — Customer ──> Order
class Customer {
    List<Order> orders;     // Customer holds Orders
}
class Order {
    // No reference to Customer — Order doesn't know who placed it
}

// Bidirectional — Customer <──> Order
class Customer {
    List<Order> orders;     // Customer holds Orders
}
class Order {
    Customer customer;      // Order also holds its Customer
}
```

In databases this maps to *"which side has the foreign key, or do both sides?"*

#### Real-world analogy

Imagine a teacher and a student.

- *"The teacher has a list of all students in the class"* → `Teacher ──> Student` (teacher knows students).
- *"Each student also has a reference to their assigned teacher"* → `Teacher <──> Student` (now both know each other).

Both are valid designs. The arrow direction documents which one you chose.

### How to Read Any UML Class Diagram (a 5-step recipe)

Whenever you encounter an unfamiliar class diagram (in a book, in a code review, in an interview), read it in this exact order:

1. **Find the classes.** Every rectangle is one class. Note the names.
2. **Read each class's contents** — its fields and methods (top section = fields, bottom section = methods).
3. **Identify the relationships.** Look at the *type of arrow* between each pair (solid vs dashed; triangle vs diamond vs simple arrowhead vs filled diamond).
4. **Read the multiplicity** at both ends of every line. *"How many of this class participate?"*
5. **Read the direction.** *"Who knows whom?"*

After 10–20 diagrams, you'll do this in ~10 seconds without thinking. **This is exactly the skill interviewers test for in LLD rounds.**

### UML Class Diagram Example

```
┌─────────────────────────┐
│       <<interface>>      │
│     PaymentProcessor     │
├─────────────────────────┤
│                          │
├─────────────────────────┤
│ + processPayment(amt)    │
│ + refund(transactionId)  │
└───────────┬─────────────┘
            △ (dashed - realization)
    ┌───────┴───────────┐
    │                    │
┌───┴──────────┐  ┌─────┴──────────┐
│ CreditCard   │  │    UPI         │
│ Processor    │  │  Processor     │
├──────────────┤  ├────────────────┤
│ -apiKey      │  │ -vpa           │
├──────────────┤  ├────────────────┤
│ +process()   │  │ +process()     │
│ +refund()    │  │ +refund()      │
└──────────────┘  └────────────────┘

┌──────────────┐       ┌──────────────┐
│    Order     │◆─────│  OrderItem   │   ← Composition
├──────────────┤       ├──────────────┤     (items die with order)
│ -orderId     │       │ -productName │
│ -total       │       │ -quantity    │
└──────────────┘       └──────────────┘

┌──────────────┐       ┌──────────────┐
│  Department  │◇─────│  Employee    │   ← Aggregation
├──────────────┤       ├──────────────┤     (employees survive dept closure)
│ -name        │       │ -name        │
│ -budget      │       │ -role        │
└──────────────┘       └──────────────┘
```

---

## 8. Relationships in OOP

### 8.1 IS-A Relationship (Inheritance / Generalization)

**What:** A child class IS-A type of parent class. Represents inheritance.

#### Real-world analogy

Think of a **biology classification**:
- A *Tesla Model 3* IS-A *Car*. A *Car* IS-A *Vehicle*. A *Vehicle* IS-A *MachinerY*.
- A *Dog* IS-A *Mammal*. A *Mammal* IS-A *Animal*.

Each step down the tree adds something specific (a Tesla has battery details a generic Car doesn't), but the core *"is a kind of"* relationship is what makes it inheritance.

**UML:** Solid line with hollow triangle pointing to parent.

```java
class Animal { }
class Dog extends Animal { }  // Dog IS-A Animal
class Cat extends Animal { }  // Cat IS-A Animal
```

```
    ┌──────────┐
    │  Animal   │
    └─────△────┘
      ┌───┴────┐
┌─────┴──┐  ┌──┴──────┐
│   Dog   │  │   Cat   │
└─────────┘  └─────────┘
```

#### Industry examples

| System | IS-A relationship |
|---|---|
| Java standard library | `ArrayList extends AbstractList extends AbstractCollection` (every collection IS-A AbstractCollection) |
| Spring | `RuntimeException extends Exception extends Throwable` (every runtime error IS-A Throwable) |
| Android | `Activity extends Context` (every Activity IS-A Context) |
| Banking app | `SavingsAccount extends Account`, `CheckingAccount extends Account` |
| E-commerce | `PhysicalProduct extends Product`, `DigitalProduct extends Product` |

**When to use:** When there's a true taxonomic relationship and the child genuinely IS a specialized version of the parent. **If you find yourself saying "the child *uses* the parent" rather than "the child *is* a parent", you should be using composition, not inheritance.**

### 8.2 HAS-A Relationship (Association)

**What:** Any time **one class holds a reference to another**, it's an association. This is the umbrella term for "object A has, uses, or knows about object B."

There are three flavors of HAS-A, ranked from **weakest** (no ownership) to **strongest** (full lifecycle ownership):

```
   PLAIN              AGGREGATION             COMPOSITION
   ─────              ───────────             ───────────
   "knows of"         "loosely owns"          "strictly owns"
   ────────►          ◇──────►                ◆──────►
   no diamond         hollow diamond          filled diamond
   weakest            weak ownership          strongest ownership
```

We'll cover each of these in detail. Plain association is here in §8.2; aggregation and composition get their own deep dives below (composition in §8.5).

---

#### 8.2.1 Plain Association — "A just knows about B"

This is the **simplest, weakest** form of HAS-A. Class A has a field referencing class B, but A doesn't *own* B's lifecycle. B existed before A came along, and B will continue to exist after A is gone.

##### Real-world analogy

Think of you and your **friend**. You both know each other (each of you has the other in your "list of friends"), but neither of you owns the other. If you stop being friends, both of you continue to exist independently.

##### Code example

```java
class Teacher {
    private List<Student> studentsITeach;   // ← association: teacher knows about students
}

class Student {
    private List<Course> coursesIamEnrolledIn; // ← association: student knows courses
}
```

A `Teacher` has references to `Student`s. The teacher didn't *create* those students. If the teacher quits, the students don't disappear. That's plain association.

##### UML drawing

```
┌─────────┐  1     *  ┌─────────┐
│ Teacher │──────────►│ Student │
└─────────┘           └─────────┘
   (no diamond — just an arrowhead = plain association)
```

##### Industry examples

| System | Example of plain association |
|---|---|
| Twitter | A `User` follows other `User`s — no ownership, both exist independently |
| LinkedIn | An `Employee` has connections to other `Employee`s |
| Spotify | A `Playlist` references `Song`s — songs exist on their own; deleting a playlist doesn't delete songs |
| Spring app | A `LoginService` has a reference to a `UserRepository` — neither owns the other |
| Amazon | A `Customer` has a list of `WishList` items pointing at `Product`s — products exist independently |

> **Memory aid:** plain association = *"A and B are friends."* Neither created the other; neither dies for the other.

---

#### 8.2.2 Aggregation — "A loosely owns B"

Aggregation is **stronger than plain association** but still weak: A *contains* B, but B can exist independently of A. If A goes away, B keeps living.

The key word is **"part of, but not bound to"**.

##### Real-world analogy

Think of a **football team and its players**. The team *has* players (they're part of the team), but the players exist as independent humans. If the team disbands, the players are still alive — they just go play for other teams or live their lives.

```
   Team                       After team disbands
   ┌───────────────┐         ┌───────┐ ┌───────┐ ┌───────┐
   │ ◇ Player A    │         │Player │ │Player │ │Player │
   │ ◇ Player B    │   →     │   A    │ │   B    │ │   C   │
   │ ◇ Player C    │         └───────┘ └───────┘ └───────┘
   └───────────────┘         (Players survive independently)
```

##### Code example

```java
class FootballTeam {
    private List<Player> players;     // ← aggregation: team has players
                                       //   players already exist; team just holds references

    public FootballTeam(List<Player> existingPlayers) {
        this.players = existingPlayers;   // team did NOT create the players
    }
}

class Player {
    String name;
    int age;
}

// Usage:
Player ronaldo = new Player("Ronaldo", 39);   // player exists on their own
Player messi   = new Player("Messi", 36);
FootballTeam team = new FootballTeam(List.of(ronaldo, messi));
// If `team` is garbage-collected, ronaldo and messi still exist.
```

The `Player` objects exist *before* and *outside* the team — the team merely **references** them.

##### UML drawing

```
┌──────────┐  1     *  ┌──────────┐
│   Team   │◇──────────│  Player  │
└──────────┘           └──────────┘
   (hollow diamond at the OWNER end = aggregation)
```

The hollow diamond is on the side of the **container** (the team), pointing toward the **part** (the player).

##### Industry examples

| System | Example of aggregation |
|---|---|
| Slack | A `Channel` contains `User` references; users exist outside any specific channel |
| Spotify | A `Playlist` aggregates `Song`s — deleting the playlist doesn't delete songs |
| University | A `Department` aggregates `Professor`s — closing a department doesn't fire the professors |
| Spring | A `Configuration` class aggregates beans, but the beans live in the application context |
| GitHub | A `Project` aggregates `Issue`s for visibility, but issues can be moved between projects |

> **Memory aid:** aggregation = *"Team has players, but players exist on their own."* You can move them to another team.

---

#### 8.2.3 Composition (preview — full deep-dive in §8.5)

Composition is the **strongest** form of HAS-A: not just *"A has B"*, but *"A owns B's life — when A dies, B dies with it."*

##### Quick preview

```java
class House {
    private List<Room> rooms;

    public House(int numRooms) {
        this.rooms = new ArrayList<>();
        for (int i = 0; i < numRooms; i++) {
            this.rooms.add(new Room());   // ← House CREATES its own rooms
        }
    }
    // When this House is garbage-collected, the Rooms go with it.
    // Rooms cannot meaningfully exist outside this house.
}
```

When the `House` object is destroyed, the `Room` objects inside it are destroyed too. They were *created by* the house and have no independent existence.

The full lifecycle test, code walkthrough, and comparison with aggregation are in **§8.5 Composition** below.

### 8.3 Generalization

**What:** Extracting shared behavior from multiple classes into a common parent. It's the "bottom-up" way of thinking about IS-A.

#### How it differs from §8.1

§8.1 (IS-A) describes the *result* — once you have `Dog extends Animal`, that's an IS-A relationship.

Generalization describes the *thinking process* that leads there: you spot common features across multiple classes and **extract** them into a parent. It's the same UML arrow — just looked at from the other direction.

| Direction of thinking | Name |
|---|---|
| Top-down: "I have Animal; I'll specialize it as Dog and Cat." | **Specialization** (synonym for IS-A) |
| Bottom-up: "I notice Dog and Cat share fields and methods; let me extract them into Animal." | **Generalization** |

#### Real-world analogy

Imagine you're a librarian. You start by labeling individual books: *"Pride and Prejudice — Romance, English, Hardcover"*, *"Sense and Sensibility — Romance, English, Hardcover"*. After labeling 50 books, you realize *"these all share fields like genre, language, format."* So you create a parent category *"Book"* with those shared fields, and each specific book just records its title. **You generalized.**

**UML:** Solid line with hollow triangle.

```
You notice Dog, Cat, and Parrot all have name, age, eat(), sleep().
    ↓
Generalize into Animal superclass.
```

```java
// Before: Code duplication
class Dog { String name; int age; void eat() {} void bark() {} }
class Cat { String name; int age; void eat() {} void meow() {} }

// After: Generalized
class Animal { String name; int age; void eat() {} }
class Dog extends Animal { void bark() {} }
class Cat extends Animal { void meow() {} }
```

#### Industry examples

| System | What was generalized |
|---|---|
| Java | `Number` is the generalization of `Integer`, `Long`, `Float`, `Double` (all share `intValue()`, `doubleValue()`) |
| Spring | `RuntimeException` is the generalization of all unchecked exceptions |
| Banking app | `Account` is the generalization extracted from `SavingsAccount`, `CheckingAccount`, `LoanAccount` |
| E-commerce | `Product` is the generalization extracted from `Book`, `Electronic`, `Clothing` |
| Game | `GameObject` generalizes `Player`, `Enemy`, `NPC`, `Item`, `Projectile` |

#### The 6 Advantages of Generalization

People usually think generalization is just *"so I don't repeat code in subclasses."* That's only the *smallest* benefit. Here are all six, ranked by importance.

##### Advantage 1 — Code reuse (DRY)

Subclasses don't have to redefine common fields/methods.

```java
class Animal {
    String name;
    int age;
    void eat()   { System.out.println(name + " is eating"); }
    void sleep() { System.out.println(name + " is sleeping"); }
}

class Dog extends Animal {
    void bark() { System.out.println("Woof!"); }
    // No need to redefine eat / sleep / name / age — inherited free
}

class Cat extends Animal {
    void meow() { System.out.println("Meow!"); }
    // Same — inherits everything from Animal
}
```

##### Advantage 2 — Polymorphism (the BIG one)

This is the *real* reason generalization exists. With a common parent, you can write code that works on **any** subclass without knowing the specific kind.

```java
// Method that takes ANY Animal
public static void feedAnimal(Animal a) {
    a.eat();
}

feedAnimal(new Dog());      // ✅
feedAnimal(new Cat());      // ✅
feedAnimal(new Parrot());   // ✅
```

Or store mixed types in one collection:

```java
List<Animal> zoo = new ArrayList<>();
zoo.add(new Dog());
zoo.add(new Cat());
zoo.add(new Parrot());

for (Animal a : zoo) {
    a.eat();   // calls Dog.eat() / Cat.eat() / Parrot.eat() at runtime
}
```

> Without generalization you'd need `List<Dog>`, `List<Cat>`, `List<Parrot>` separately and a `feedDog()`, `feedCat()`, `feedParrot()` method each. Generalization gives you **one method that handles all subtypes.**

This is what makes Strategy, Observer, Adapter, Decorator (and most of design patterns) possible.

##### Advantage 3 — Open-Closed Principle (free for you)

Adding a *new* subclass doesn't require touching any existing code:

```java
// New requirement: add Tiger.
class Tiger extends Animal {
    void roar() { System.out.println("ROAR!"); }
}

// Existing code keeps working — feedAnimal(new Tiger()) just works.
// We didn't modify Animal, Dog, Cat, Parrot, or feedAnimal().
// We only ADDED a new class.
```

> **Subtle but critical point:** `feedAnimal(Animal a)` only calls methods that exist on `Animal` (like `eat()`, `sleep()`). It does **not** call `roar()` — and never tries to. So when we add Tiger, we do **NOT** need to add `roar()` to `Animal`. Tiger-specific methods stay on Tiger; the parent only holds what *every* animal shares. (See "Advantage 5 — LSP" below for why this matters.)

##### Advantage 4 — Single source of truth

When something common changes, you change it in **one place** — the parent.

```java
// Suddenly the business says: "log every meal for analytics"

// Without generalization (3 classes have eat()):
class Dog    { void eat() { /* ...add logging... */ } }   // edit
class Cat    { void eat() { /* ...add logging... */ } }   // edit
class Parrot { void eat() { /* ...add logging... */ } }   // edit
// Forget one? Bug.

// With generalization:
class Animal {
    void eat() {
        System.out.println(name + " is eating");
        Analytics.logMeal(this);   // ← add ONCE, all subclasses inherit it
    }
}
```

This is **DRY at the class level** — change once, propagate to all subclasses.

##### Advantage 5 — Substitutability (LSP)

Because every Dog IS-A Animal, you can pass a Dog anywhere an Animal is expected — and the **compiler enforces it**.

```java
public void registerInZoo(Animal a) {
    a.eat();
    a.sleep();
    // works for any Animal subtype
}

registerInZoo(new Dog());      // ✅ compiles
registerInZoo(new Tiger());    // ✅ compiles
registerInZoo(new Customer()); // ❌ compile error — Customer is not an Animal
```

The type system uses generalization to **enforce correctness** — you can't accidentally pass the wrong thing.

###### LSP companion rule — what NOT to put in the parent

> **Put on the parent only what ALL children genuinely do. Subclass-specific methods stay on the subclass.**

If you put `roar()` on `Animal`:
- Every `Animal` subtype (Dog, Cat, Parrot, Fish) would have to implement `roar()`.
- Most would either throw `UnsupportedOperationException` or do nothing — both violate LSP.
- The `Animal` contract becomes a lie ("every animal can roar" — false).

**What to do when multiple subclasses share a method but not all** (e.g., Tiger, Lion, and Leopard all roar):

```java
class Animal { void eat(); void sleep(); }            // only universal stuff

class BigCat extends Animal {                          // ← introduced middle layer
    void roar() { System.out.println("ROAR!"); }       // ← shared by big cats only
}

class Tiger   extends BigCat { /* tiger-specific */ }
class Lion    extends BigCat { /* lion-specific */ }
class Leopard extends BigCat { /* leopard-specific */ }

class Dog extends Animal { void bark() {} }            // Dog still doesn't get roar()
```

Or use an **interface** (cleaner if "roaring" is a capability, not a hierarchy):

```java
interface Roarable { void roar(); }

class Tiger extends Animal implements Roarable { public void roar() {...} }
class Lion  extends Animal implements Roarable { public void roar() {...} }

public void makeRoar(Roarable r) { r.roar(); }   // works for any roaring thing
```

##### Advantage 6 — Default behavior + selective override

Subclasses can choose to **inherit as-is** OR **override** when they need different behavior — best of both worlds.

```java
class Animal {
    void eat() { System.out.println(name + " eats grass"); }   // default
}

class Dog extends Animal {
    // Doesn't override — Dog uses Animal's default eat()
}

class Tiger extends Animal {
    @Override
    void eat() { System.out.println(name + " hunts and eats meat"); }   // tiger overrides
}
```

So generalization gives you **reuse for free**, but lets each subclass customize the bits that genuinely differ.

#### Side-by-side — Without vs With Generalization

```
WITHOUT generalization                  WITH generalization
─────────────────────────                ──────────────────────────
class Dog {                             class Animal {       ← parent
  String name;                            String name;
  int age;                                int age;
  void eat() { ... }                      void eat() { ... }
  void sleep() { ... }                    void sleep() { ... }
  void bark() { ... }                   }
}
class Cat {                             class Dog extends Animal {
  String name;        ← duplicated         void bark() { ... }
  int age;            ← duplicated      }
  void eat() { ... }  ← duplicated
  void sleep() { ... }← duplicated      class Cat extends Animal {
  void meow() { ... }                      void meow() { ... }
}                                        }

❌ Duplicated code                      ✅ DRY
❌ List<Dog>, List<Cat> separately       ✅ List<Animal> holds both
❌ feedDog(), feedCat() separately       ✅ feedAnimal(Animal a) works
❌ Add Tiger → modify many places        ✅ Add Tiger → just new class
❌ Hard to extend                        ✅ OCP friendly
```

#### TL;DR — the mental model in one line

> **Generalization = "common skeleton + uniform handling."** You factor out shared bits (DRY) AND let the rest of the program treat all subtypes uniformly through the parent type (polymorphism). The first benefit is what most beginners notice; the second is what makes most of OOP and design patterns possible at all.

### 8.4 Realization

**What:** A class implements an interface, promising to provide concrete behavior for the interface's contract.

#### Real-world analogy

Think of a **job description** at a company. The job description says: *"You must be able to writeReport(), attendStandup(), and shipCode()."* It doesn't say *how* you do those — that's up to you. Different employees fulfill the same job description in their own way (some pair-program, some work solo, some write reports in Word, others in Markdown).

The job description is the **interface**. The employee fulfilling it **realizes** the interface.

```
   <<interface>>             Real employee A    Real employee B
   ┌──────────────┐          ┌──────────────┐   ┌──────────────┐
   │ JobRole       │          │   Alice       │   │   Bob          │
   │ + writeReport │  ◄──┐    │ writeReport() │   │ writeReport()  │
   │ + standup     │     ├────│ standup()     │   │ standup()      │
   │ + shipCode    │     │    │ shipCode()    │   │ shipCode()     │
   └──────────────┘     │    └──────────────┘   └──────────────┘
                        │                Both REALIZE the same JobRole
                       (dashed line + hollow triangle in UML)
```

Two employees, same contract, different implementations — exactly how realization works.

**UML:** Dashed line with hollow triangle.

```java
interface Flyable {
    void fly();
}

class Airplane implements Flyable {   // Airplane REALIZES Flyable
    @Override
    public void fly() {
        System.out.println("Flying with engines");
    }
}

class Bird implements Flyable {       // Bird REALIZES Flyable
    @Override
    public void fly() {
        System.out.println("Flying with wings");
    }
}
```

```
    ┌─────────────────┐
    │  <<interface>>   │
    │    Flyable       │
    └───────△──────────┘  (dashed line)
        ┌───┴────┐
  ┌─────┴───┐  ┌─┴────────┐
  │ Airplane │  │   Bird   │
  └──────────┘  └──────────┘
```

#### Generalization vs Realization

| | Generalization | Realization |
|-|---------------|-------------|
| **Keyword** | `extends` | `implements` |
| **Parent type** | Class (concrete or abstract) | Interface |
| **Inherits** | State + behavior | Only contract (method signatures) |
| **Child must...** | Optionally override; gets defaults | Provide concrete bodies for **all** methods |
| **Mental model** | "Dog **is a kind of** Animal" | "Bird **fulfills the contract** of Flyable" |
| **UML arrow** | Solid line + hollow triangle | Dashed line + hollow triangle |

#### Industry examples

| System | Realization example |
|---|---|
| Java collections | `ArrayList implements List`, `HashSet implements Set` (every collection class realizes a contract) |
| Java I/O | `FileInputStream` realizes the `InputStream` contract (`extends` from abstract class, but the *contract* part is realization-style) |
| Spring Data | `UserRepositoryImpl implements UserRepository` |
| JDBC | `MysqlConnection implements java.sql.Connection`, `PostgresConnection implements java.sql.Connection` |
| Logging | `LogbackLogger implements Logger`, `Log4jLogger implements Logger` (the SLF4J pattern from §10.5) |
| Strategy pattern | All concrete strategies realize a `Strategy` interface |

### 8.5 Composition

#### What it really means

Composition is the **strongest** form of HAS-A. The container doesn't just *reference* its parts — it **owns their entire lifecycle**:

1. The container **creates** the parts.
2. The parts **cannot exist** outside the container in any meaningful way.
3. When the container is destroyed, the parts are **destroyed with it**.

It's a true ownership relationship — closer to "the parts are *built into* the whole" than "the whole *uses* the parts."

#### Real-world analogy

Think of a **house and its rooms**.

- The rooms didn't exist before the house was built — they were created when the house was constructed.
- A "Room #3" doesn't make sense outside of *this specific house*.
- When the house is demolished, the rooms cease to exist with it. You can't pick up Room #3 and carry it to a different house.

```
   House                       House demolished
   ┌─────────────────────┐         💥 (everything gone)
   │ ◆ Living Room       │   →
   │ ◆ Kitchen            │     No more rooms; they had no
   │ ◆ Bedroom            │     existence outside the house.
   └─────────────────────┘
```

This is composition: the parts are *born* with the whole and *die* with it.

#### Code example

```java
class Room {
    private String type;
    public Room(String type) { this.type = type; }
}

class House {
    private List<Room> rooms;

    public House() {
        this.rooms = new ArrayList<>();
        rooms.add(new Room("Living"));    // ← House creates its own rooms
        rooms.add(new Room("Kitchen"));
        rooms.add(new Room("Bedroom"));
    }
}

// Usage:
House h = new House();   // 3 Room objects come into existence
h = null;                // House becomes unreachable; Rooms become unreachable too
                          // Garbage collector destroys all 4 objects together
```

The `Room` objects were *created inside the constructor* of `House` and are held only by `House`. No external code has references to them. When the `House` object becomes unreachable, the `Room` objects do too.

#### UML drawing

```
┌──────────┐  1     1..*  ┌──────────┐
│  House   │◆──────────────│   Room   │
└──────────┘               └──────────┘
   (FILLED diamond at the OWNER end = composition)
```

The **filled** diamond marks the strong ownership. Compare to aggregation, which uses a *hollow* diamond.

#### The lifecycle test — Composition vs Aggregation

This is the **single most important distinction** in OOP relationships, and the most-asked interview question. Use this test:

> **"If I destroy the container, does the part still exist somewhere meaningful?"**
>
> - **YES, the part survives** → **Aggregation** (hollow diamond ◇)
> - **NO, the part is gone too** → **Composition** (filled diamond ◆)

##### Side-by-side comparison

| Aspect | Aggregation (◇) | Composition (◆) |
|---|---|---|
| **Lifecycle** | Part exists independently of container | Part's lifecycle is bound to the container |
| **Created by** | Someone *else* creates the part; container just holds a reference | Container creates the part itself |
| **Survives container death?** | ✅ Yes | ❌ No |
| **Can be shared?** | ✅ Yes — same part can be in multiple containers | ❌ Usually no — part belongs to exactly one container |
| **Ownership** | Loose — like having a tenant | Strong — like having a body part |
| **Example phrase** | *"Team **has** players"* | *"House **owns** rooms"* |
| **UML diamond** | Hollow ◇ | Filled ◆ |

##### Concrete example pairs

| Pair | Type | Why |
|---|---|---|
| `Team` and `Player` | **Aggregation** | Players exist outside the team |
| `House` and `Room` | **Composition** | Rooms only make sense inside *this* house |
| `Department` and `Employee` | **Aggregation** | Employees can be moved to another department |
| `Order` and `OrderItem` | **Composition** | Order items are created with the order; deleting the order deletes items |
| `Person` and `Heart` | **Composition** | The heart is part of *that person*; can't move it casually |
| `Library` and `Book` | **Aggregation** | Books can move between libraries; they exist independently |
| `Chess Game` and `Move` | **Composition** | Moves are tied to one specific game's history |
| `User` and `Address` | Could be either, depending on design — *"I'm using a saved address"* (aggregation) vs *"I created this billing address just for this checkout"* (composition) |

#### Industry examples — composition in real systems

| System | Composition relationship | Why it's composition |
|---|---|---|
| Spring Boot | A `@RestController` *composes* its private fields and helper objects | Helpers exist only to serve this controller |
| E-commerce app | An `Invoice` composes `LineItem`s | Line items are created with the invoice and meaningful only inside it |
| Banking app | A `Transaction` composes its `JournalEntry` records | Entries are written when the transaction is created and tied to that transaction |
| Android UI | An `Activity` composes its `View` objects | Views live only within the lifetime of their hosting Activity |
| Compiler | An `AST` (Abstract Syntax Tree) composes its child nodes | Child nodes are part of *this specific* tree; deleting the tree deletes them |

> **Memory aid:** composition = *"the parts are *built into* the whole, like rooms in a house. Demolish the house, the rooms are gone."*

#### One-line takeaway for interviews

> *"If the part can't exist outside the container — meaning the container creates the part and the part dies with the container — that's composition. Otherwise, it's aggregation."*

### 8.6 Dependency — "A uses B briefly"

#### What it really means

Dependency is the **weakest** kind of relationship between two classes. Class A doesn't *contain* B as a field — A merely **uses** B for a moment, usually inside a single method call. As soon as the method returns, A no longer references B.

#### Real-world analogy

Think of yourself and an **Uber**. You don't *own* the Uber. You don't even keep a reference to it after the trip. You use it for one short journey, pay, and walk away.

```
You ────uses────► Uber (just for this trip)
   (no field, no ownership, no long-term relationship)
```

That's a dependency. You needed the Uber temporarily, used it, and forgot about it.

#### Code example

```java
class EmailValidator {
    public boolean isValid(String email) {
        return email.contains("@");
    }
}

class SignupService {
    public void signup(String email, String password) {
        EmailValidator validator = new EmailValidator();   // ← created locally, used briefly
        if (validator.isValid(email)) {
            // ... save user ...
        }
        // After this method returns, the validator is gone.
        // SignupService does NOT have an EmailValidator field.
    }
}
```

`SignupService` **uses** `EmailValidator` inside the `signup` method, but doesn't *hold* it as a field. As soon as `signup` returns, the validator is gone. That's a dependency, not an association.

#### How dependency differs from association

| Aspect | Dependency | Association (HAS-A) |
|---|---|---|
| Held as a **field**? | ❌ No — used inside a method only | ✅ Yes — long-lived field reference |
| Lifetime | Short — one method call | Long — as long as the container exists |
| Coupling | Loosest possible (besides not knowing each other) | Stronger |
| In code | `Foo foo = ...; foo.bar();` *inside a method* | `private Foo foo;` *as a field* |

A simple way to spot dependency in code: the class B's name appears **inside method bodies** but **never as a field declaration** in class A.

#### UML drawing

```
┌────────────────┐  - - - - - - - ►  ┌─────────────────┐
│ SignupService  │                    │ EmailValidator  │
└────────────────┘                    └─────────────────┘
       (DASHED line + open arrowhead = dependency)
```

The arrow says *"SignupService **uses** EmailValidator (briefly)."*

#### Industry examples

| System | Example of dependency |
|---|---|
| Spring | A `@Service` calls a `Mapper.toDto(...)` inside a method; doesn't store a `Mapper` field |
| Java logging | A method calls `LoggerFactory.getLogger(MyClass.class)` once, no long-term field |
| HTTP client | `OrderService.placeOrder()` instantiates a temporary `RetryHelper` inside the method |
| Validation | `UserController` calls `ValidationUtils.validate(user)` inline — utils class is a dependency |
| Math | A method does `Math.sqrt(x)` — the class temporarily depends on `Math` |

> **Memory aid:** dependency = *"I called you once for help, but I don't remember your number."* No field, no long-term relationship.

#### Tip for class diagrams

If you sketch a relationship and aren't sure whether it's an association or a dependency, ask:

> *"Will I write `private B b;` as a field in class A?"*

- **Yes** → association (solid arrow, optionally with a diamond if there's ownership)
- **No, just `B b = ...; b.do();` inside a method** → dependency (dashed arrow)

---

### 8.7 Common Confusions & How to Avoid Them

Some pairs of relationships get confused over and over by beginners (and even mid-level engineers). Here are the most common ones and how to keep them straight.

#### Confusion 1 — Aggregation vs. Composition

**Why it's confusing:** both are HAS-A; both have a diamond on the UML; both look almost identical in code.

**The litmus test:**

> *"If the container disappears, does the part still exist somewhere meaningful?"*

```java
// Aggregation — Team has Players, Players are passed in from outside
class Team {
    private List<Player> players;
    public Team(List<Player> players) { this.players = players; }   // ← reference passed in
}

// Composition — House owns Rooms, Rooms created inside
class House {
    private List<Room> rooms;
    public House() {
        this.rooms = List.of(new Room(), new Room(), new Room());   // ← created here
    }
}
```

| Test | Team / Player | House / Room |
|---|---|---|
| Created externally? | ✅ Yes (passed into constructor) | ❌ No (created inside the container) |
| Part survives container death? | ✅ Yes (players still exist) | ❌ No (rooms gone with house) |
| Diamond | Hollow ◇ | Filled ◆ |
| Verdict | **Aggregation** | **Composition** |

#### Confusion 2 — Association vs. Dependency

**Why it's confusing:** both can show "A uses B" in some sense.

**The litmus test:**

> *"Does class A keep a reference to class B as a **field**?"*

- **Yes, as a field** → association
- **No, only briefly inside a method** → dependency

```java
// Association — service HOLDS a reference to repository
class OrderService {
    private OrderRepository repo;          // ← field
}

// Dependency — service uses validator briefly inside a method
class OrderService {
    void placeOrder(Order o) {
        OrderValidator v = new OrderValidator();   // ← method-local, no field
        v.validate(o);
    }
}
```

#### Confusion 3 — Generalization vs. Realization

**Why it's confusing:** both are "is-a"-ish; both use a triangle arrowhead in UML.

**The litmus test:**

> *"Is class B a regular class or an interface?"*

- **B is a class** → use `extends` → **Generalization** (solid line + hollow triangle)
- **B is an interface** → use `implements` → **Realization** (dashed line + hollow triangle)

```java
// Generalization (extends a CLASS)
class Animal { ... }
class Dog extends Animal { ... }            // Dog IS-A Animal — Generalization

// Realization (implements an INTERFACE)
interface Flyable { void fly(); }
class Bird implements Flyable { ... }       // Bird REALIZES Flyable — Realization
```

| | Generalization | Realization |
|---|---|---|
| Java keyword | `extends` | `implements` |
| Parent type | concrete or abstract class | interface |
| What's inherited | state + behavior | only the contract (signatures) |
| UML arrow | solid — — — — ▷ | dashed - - - - ▷ |

#### Confusion 4 — Association vs. Aggregation

**Why it's confusing:** both involve A having B as a field.

**The litmus test:**

> *"Does A meaningfully **'own' or 'contain'** B in a structural sense, or is A just keeping a **reference** to a peer?"*

- **A is just keeping a reference (no ownership)** → plain Association
- **A is a container that 'has' B as part of its structure (but B can still survive on its own)** → Aggregation
- **A creates B and B dies with A** → Composition

| Example | Type | Why |
|---|---|---|
| `User` follows other `User`s | Plain Association | Peers, no ownership |
| `Spotify Playlist` contains `Song`s | Aggregation | Playlist contains songs, but songs exist on their own |
| `Order` contains `OrderItem`s | Composition | Items are part of the order; deleting order deletes items |

#### Confusion 5 — UML Direction vs. Multiplicity

**Why it's confusing:** they're both labels on the same line.

**The rule:**

- **Multiplicity** is *how many* — written **at each end** of the line as a number (`1`, `*`, `1..*`).
- **Direction** is *who knows whom* — shown by the **arrowhead** (none, one-way, or two-way).

```
   ┌────────┐  1                  1..*  ┌──────┐
   │ Order  │──────────────────────────►│ Item │
   └────────┘                           └──────┘
              Direction (arrow): "Order navigates to Items"
              Multiplicity (1 / 1..*): "1 Order has 1 or more Items"
```

You can have any combination — direction and multiplicity are independent labels.

---

### Complete Relationship Summary

```
┌─────────────────────────────────────────────────────────────────┐
│                    OOP Relationships                             │
├─────────────┬──────────────┬──────────┬────────────────────────┤
│ Relationship│ UML Arrow    │ Keyword  │ Example                 │
├─────────────┼──────────────┼──────────┼────────────────────────┤
│ IS-A        │ ────▷ solid  │ extends  │ Dog IS-A Animal         │
│ Realization │ - - ▷ dashed │implements│ Bird REALIZES Flyable   │
│ Aggregation │ ◇──── hollow │ has-a    │ University HAS Students │
│ Composition │ ◆──── filled │ owns-a   │ House OWNS Rooms        │
│ Dependency  │ - - > dashed │ uses     │ Service USES Logger     │
│ Association │ ───── solid  │ knows    │ Teacher KNOWS Student   │
└─────────────┴──────────────┴──────────┴────────────────────────┘

Strength (weakest → strongest):
  Dependency < Association < Aggregation < Composition < Inheritance
```

### When to Use What — Decision Guide

```
Q: Does Class A need Class B?
│
├── Only in a method temporarily? ──► DEPENDENCY (dashed arrow)
│   e.g., OrderService uses EmailSender in sendConfirmation()
│
├── As a field/member? ──► ASSOCIATION
│   │
│   ├── Can B exist without A? ──► AGGREGATION (hollow diamond)
│   │   e.g., Team has Players; players exist without team
│   │
│   └── B dies when A dies? ──► COMPOSITION (filled diamond)
│       e.g., Invoice has LineItems; delete invoice, items gone
│
└── Is A a specialized version of B? ──► INHERITANCE
    │
    ├── B is a class? ──► GENERALIZATION (solid triangle)
    │   e.g., ElectricCar extends Car
    │
    └── B is an interface? ──► REALIZATION (dashed triangle)
        e.g., PayPalProcessor implements PaymentGateway
```

---

### 8.8 Case-Study Phrase → Relationship — Speed Drill

In LLD case studies, the interviewer **describes** entities in plain English and you have to instantly map the description to the right relationship. Train this mapping until it's reflexive.

#### The 4 phrases that decide everything

| When you hear / read… | Relationship | UML | Java keyword |
|---|---|---|---|
| *"X **is a kind of** Y"* / *"X **is a special** Y"* | **Inheritance (IS-A)** | `────▷` solid triangle | `extends` |
| *"X **fulfils the contract of** Y"* / *"X **provides** Y"* | **Realization** | `- - -▷` dashed triangle | `implements` |
| *"X **is part of** Y, **created and destroyed with** Y"* | **Composition** | `◆────` filled diamond | container creates the part |
| *"X **belongs to** Y, but **exists on its own**"* | **Aggregation** | `◇────` hollow diamond | container holds external part |
| *"X **has** Y / **knows** Y"* (peers, no ownership) | **Plain association** | `─────►` solid arrow | field reference |
| *"X **uses** Y briefly to do something"* | **Dependency** | `- - -►` dashed arrow | local var inside a method |

#### Extended phrase dictionary — what specific words signal

| Phrase fragment | Likely relationship | Reasoning |
|---|---|---|
| *"…is a part of…"* | **Composition** | "Part of" implies the part lives inside the whole's lifecycle. |
| *"…belongs to…"* / *"…is owned by…"* | Composition (usually) | Ownership wording. |
| *"…is composed of…"* | **Composition** | Literally the keyword. |
| *"…has many…"* / *"…has a list of…"* | Aggregation OR Composition | Decide by lifecycle test (next subsection). |
| *"…contains…"* | Composition (usually) | Strong containment. |
| *"…references…"* / *"…holds a reference to…"* | **Plain association** | Just keeps a pointer. |
| *"…knows about…"* / *"…follows…"* / *"…is friends with…"* | **Plain association** | Peer-level, no ownership. |
| *"…uses…"* / *"…calls…"* / *"…depends on…"* | **Dependency** (usually) | Verb-only, transient use. |
| *"…inherits from…"* / *"…is a type of…"* | **Inheritance** | Direct keyword match. |
| *"…implements…"* / *"…provides…"* / *"…fulfils…"* | **Realization** | Interface contract. |
| *"…created when…and destroyed when…"* | **Composition** | Lifecycle binding. |
| *"…can be moved/transferred/reassigned to another…"* | **Aggregation** | Independent existence — can switch containers. |
| *"…shared between multiple…"* | **Aggregation** | Composition usually means single owner. |

#### The deciding lifecycle test (when "has" is ambiguous)

When you hear *"X has Y"*, the lecture says it's HAS-A — but you still need to decide which kind. Apply this 2-question test:

```
Q1: Where does Y come from?
   ├── Created INSIDE X (in X's constructor)         → leans toward Composition
   └── Passed INTO X from outside                    → leans toward Aggregation

Q2: When X is destroyed, does Y still exist?
   ├── No, Y is destroyed too                         → COMPOSITION (◆)
   └── Yes, Y survives independently                  → AGGREGATION (◇)
```

Both questions usually agree. If they disagree, **trust Q2** — lifecycle is the truth-teller.

#### Speed-drill examples — read the prompt, name the relationship

| Case-study prompt | Relationship | Why |
|---|---|---|
| "An **Order has multiple OrderItems**; if the order is cancelled, the items are gone." | **Composition (◆)** | Lifecycle binding — items die with order. |
| "A **Library has many Books**; books can be moved to another library." | **Aggregation (◇)** | Books exist independently of any one library. |
| "A **Manager forwards an unhandled request to the Director**." | **Plain association** | Both peers; Manager just holds a `next` reference. |
| "**ElectricCar is a kind of Car**." | **Inheritance** | IS-A direct match (`extends`). |
| "**UPIPayment, CardPayment fulfil the PaymentStrategy interface**." | **Realization** | `implements` an interface contract. |
| "**OrderService uses an EmailValidator inside its createOrder method**." | **Dependency** | Local variable, no field. |
| "A **Department has Employees**; if the department closes, employees move elsewhere." | **Aggregation** | Employees survive department closure. |
| "An **Invoice has LineItems**, generated automatically when the invoice is created." | **Composition** | Items born and bound to the invoice. |
| "A **User follows other Users**." | **Plain association** | Peer relationship, no ownership. |
| "A **House has Rooms**; rooms exist only inside this house." | **Composition** | Classic textbook composition. |
| "A **Team has Players**; players can join other teams." | **Aggregation** | Players are independent humans. |
| "**SavingsAccount and CheckingAccount both extend Account**." | **Inheritance** | Hierarchy with shared fields/methods. |
| "**Logger.info(...)** is called inside `process()` to log progress." | **Dependency** | Single-method use, no field. |
| "A **Cart references Products**; deleting the cart doesn't delete products." | **Aggregation / plain association** | Loose containment, products independent. |

#### Tip — drawing relationships in a case study, fast

When you hear the entities in a case study, draw boxes for each, then ask **for every pair**:

1. **Is one a kind of the other?** → triangle arrow (inheritance/realization).
2. **Does one hold the other as a field?** → solid arrow.
   - **Owns its lifecycle?** → filled diamond ◆ (composition).
   - **Just contains it but it's external?** → hollow diamond ◇ (aggregation).
   - **Just refers to a peer?** → no diamond, plain arrow.
3. **Does one only USE the other inside a method?** → dashed arrow (dependency).

Five seconds per pair. After 10 case studies you'll do this without thinking.

#### Why the relationships section is enough for case studies

Yes — what we've covered (§8.1 to §8.7) gives you **all 6 relationships** + the lifecycle test + the 5 common confusions + the phrase-to-relationship dictionary above. Most LLD case studies will only test these. The rest is practice — apply the dictionary to as many problems as you can (Parking Lot, Movie Booking, Splitwise, Tic-Tac-Toe, Library, Elevator, Cab-booking, Pizza order).

> **One-liner for interviews:** *"Six relationships, decided by two questions — does it inherit, and does it own the lifecycle?"*

---

## Key Takeaways for Interviews

1. **Always start with requirements** — Don't jump into design. Ask clarifying questions.
2. **Identify entities first** — What are the nouns in the problem? Those become classes.
3. **Define relationships** — IS-A vs HAS-A drives your entire class structure.
4. **Apply SOLID** — Interviewers actively look for SRP and OCP. Mention them explicitly.
5. **Draw UML** — Even a rough class diagram shows structured thinking.
6. **Prefer composition over inheritance** — This is a well-known design principle. Use inheritance only for true IS-A relationships.
7. **Code to interfaces** — Depend on abstractions (DIP), not concrete classes.

---

**Lecture 1 Complete.**

---

# Lecture 2: SOLID Deep Dive + Creational Design Patterns

> **Date:** 26th April 2026 — LLD-2
> **Topics:** Interface Segregation, Dependency Inversion, DRY, KISS, Design Patterns Intro, Singleton (with threading), Factory, Abstract Factory, Builder

---

## 9. Interface Segregation Principle (ISP)

> **The "I" in SOLID.**
> *"Clients should not be forced to depend on methods they do not use."* — Robert C. Martin

### 9.1 The Naive (One-Line) Definition

A **larger interface should be split into smaller, more specific interfaces** so that classes only implement what they actually need.

```
            ❌ BAD                              ✅ GOOD
  ┌──────────────────────┐         ┌─────────┐  ┌─────────┐  ┌─────────┐
  │   FatInterface       │         │  IFace1 │  │  IFace2 │  │  IFace3 │
  │   - method1()        │         │ method1 │  │ method2 │  │ method3 │
  │   - method2()        │         └─────────┘  └─────────┘  └─────────┘
  │   - method3()        │              ▲             ▲           ▲
  │   - method4()        │              │             │           │
  │   - method5()        │         (only implement what you need)
  └──────────────────────┘
```

### 9.2 Why It Matters (The Problem)

When you have **one bloated interface** with many unrelated methods, every class implementing it is forced to provide implementations for methods it doesn't care about. The usual hacks are:

1. **Throwing `UnsupportedOperationException`** — defers the bug to runtime instead of compile time.
2. **Empty/no-op methods** — silently breaks Liskov Substitution.
3. **Useless coupling** — changing one method's signature breaks ten classes that don't even use it.

ISP is the principle that **prevents "fat" interfaces**.

### 9.3 Restaurant Staff Example (from lecture)

Imagine you model a restaurant `Staff` interface that has every action in the restaurant on it:

```java
// ❌ Violates ISP — fat interface
interface Staff {
    void cookFood();      // 1
    void takeOrder();     // 2
    void cleanTable();    // 3
    void serveDishes();   // 4
    void handleBills();   // 5
    void payment();
}
```

Now look at concrete roles:

| Role     | What they actually do          |
|----------|--------------------------------|
| Cook     | `cookFood()`                   |
| Waiter   | `takeOrder()`, `serveDishes()` |
| Cleaner  | `cleanTable()`                 |
| Cashier  | `handleBills()`, `payment()`   |

If `Cook implements Staff`, the cook is now forced to implement `cleanTable()` and `handleBills()` — methods that have **nothing to do with cooking**.

**ISP-compliant redesign — split by responsibility:**

```java
interface ICook   { void cookFood(); }
interface IWaiter { void takeOrder(); void serveDishes(); }
interface ICleaner{ void cleanTable(); }
interface ICashier{ void handleBills(); void payment(); }

class Cook    implements ICook    { public void cookFood()   { /* ... */ } }
class Waiter  implements IWaiter  {
    public void takeOrder()   { /* ... */ }
    public void serveDishes() { /* ... */ }
}
class Cleaner implements ICleaner { public void cleanTable() { /* ... */ } }
class Cashier implements ICashier {
    public void handleBills() { /* ... */ }
    public void payment()     { /* ... */ }
}
```

Now each class implements **only the contract relevant to it**.

### 9.4 Payment Service Example (from lecture)

A more realistic, code-level example.

```java
// ❌ Violates ISP
interface PaymentService {
    void pay();
    void refund();
    void generateReport();
}

class CreditCardPayment implements PaymentService {
    @Override public void pay()    { /* implementation */ }
    @Override public void refund() { /* implementation */ }
    @Override public void generateReport() {
        throw new UnsupportedOperationException("Not Required");  // 🚩 RED FLAG
    }
}

class ReportService implements PaymentService {
    @Override public void pay()    { throw new UnsupportedOperationException("Not Required"); }
    @Override public void refund() { throw new UnsupportedOperationException("Not Required"); }
    @Override public void generateReport() { /* implementation */ }
}
```

**Smell:** Throwing `UnsupportedOperationException` is a giant ISP violation flag. The interface is forcing classes into a contract they cannot honour.

**ISP-compliant redesign — split into role interfaces:**

```java
interface IPaymentProcessor { void pay(); }
interface IRefundProcessor  { void refund(); }
interface IReportGenerator  { void generateReport(); }

// CreditCard does payments AND refunds — implement both
class CreditCardPayment implements IPaymentProcessor, IRefundProcessor {
    @Override public void pay()    { /* ... */ }
    @Override public void refund() { /* ... */ }
}

// Reporting service only generates reports
class ReportingService implements IReportGenerator {
    @Override public void generateReport() { /* ... */ }
}
```

```
                    ✅ ISP applied
   ┌────────────────────┐    ┌────────────────────┐    ┌────────────────────┐
   │ IPaymentProcessor  │    │ IRefundProcessor   │    │ IReportGenerator   │
   │   + pay()          │    │   + refund()       │    │   + generateReport()│
   └─────────△──────────┘    └─────────△──────────┘    └─────────△──────────┘
             │                          │                         │
             └─────────┬────────────────┘                         │
                       │                                          │
              ┌────────┴─────────┐                       ┌────────┴────────┐
              │ CreditCardPayment│                       │ ReportingService│
              └──────────────────┘                       └─────────────────┘
```

### 9.5 Real-World Industry Examples

1. **Java's `Collection` family.** Java doesn't have one giant `Collection` interface. It splits into `List`, `Set`, `Queue`, `Deque`, `Map` — each adding only the operations relevant to that data structure. `LinkedList` implements `List` AND `Deque`, but `HashSet` only implements `Set` because queue semantics make no sense for a hash set.

2. **AWS SDK clients.** AWS doesn't expose one massive `AwsClient` interface. Each service has its own client (`S3Client`, `DynamoDbClient`, `SqsClient`). A microservice that only writes to S3 doesn't have to depend on DynamoDB classes at all.

3. **Spring Data repositories.** `CrudRepository`, `PagingAndSortingRepository`, `JpaRepository` form a hierarchy. A read-only repository can implement just `Repository<T, ID>` and pick the find methods it needs without inheriting `save()`/`delete()`.

4. **Printer/Scanner devices.** A classic textbook example: a basic printer should not be forced to implement `scan()` or `fax()`. Use `IPrinter`, `IScanner`, `IFax`. A multi-function device implements all three; a cheap one implements just `IPrinter`.

### 9.6 ISP Smells (How to Spot Violations in PRs)

- A class throws `UnsupportedOperationException` from an overridden method.
- A class has empty `{}` overrides for methods it doesn't care about.
- An interface name ends in `Service` or `Manager` and has more than ~5 unrelated methods.
- Mocking the interface in tests requires stubbing 10 methods you'll never call.
- Adding a method to the interface causes you to update 20 classes that don't use it.

### 9.7 ISP — Interview Cheat Sheet

| Question | One-liner answer |
|---|---|
| What is ISP? | Clients shouldn't be forced to depend on methods they don't use. |
| Why? | Avoids fat interfaces, reduces coupling, prevents `UnsupportedOperationException`. |
| Relation to SRP? | SRP is about classes; ISP is the **same idea applied to interfaces**. |
| Java example? | `List`, `Set`, `Queue` instead of one giant `Collection`. |
| How to fix violation? | Split the fat interface into smaller, role-specific interfaces. |

---

## 10. Dependency Inversion Principle (DIP)

> **The "D" in SOLID.**
> *"High-level modules must not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions."* — Robert C. Martin

### 10.1 The Naive Definition

- **High-level module** = business logic (e.g., `PasswordReminder`, `OrderService`).
- **Low-level module** = infrastructure / detail (e.g., `MySQLConnection`, `EmailSender`).
- **Don't** let your business logic `new` up concrete infrastructure classes.
- **Do** make both depend on an interface (abstraction). Then the concrete implementation is **injected** at runtime.

```
   ❌ Without DIP                       ✅ With DIP
  ┌──────────────────┐                ┌──────────────────┐
  │ PasswordReminder │                │ PasswordReminder │ (high-level)
  │  (high-level)    │                └────────┬─────────┘
  └────────┬─────────┘                         │ depends on
           │ new MySQLConnection()             ▼
           ▼                          ┌──────────────────┐
  ┌──────────────────┐                │  <<interface>>   │ ← abstraction
  │ MySQLConnection  │                │  DBConnection    │
  │  (low-level)     │                └────────△─────────┘
  └──────────────────┘                         │
                                       ┌───────┼───────┐
                                       │       │       │
                                  ┌────┴───┐ ┌─┴────┐ ┌┴───────┐
                                  │ MySQL  │ │MongoDB│ │XYZ-SQL │ (low-level)
                                  └────────┘ └──────┘ └────────┘
```

### 10.2 The Problem (from lecture, in Java)

Use case: a `PasswordReminder` class needs a DB connection.

```java
// ❌ Violates DIP — high-level depends directly on low-level
class MySQLConnection {
    public String connect() {
        return "Database connection string";
    }
}

class PasswordReminder {
    private MySQLConnection dbConnection;

    public PasswordReminder(MySQLConnection dbConnection) {  // ← concrete type
        this.dbConnection = dbConnection;
    }
}
```

**What's wrong?**
- If tomorrow we want to switch to **MongoDB**, we have to **change every class** that took `MySQLConnection`.
- Ask the team to migrate to `XYZSQL`? Same painful refactor.
- **Cannot mock** `MySQLConnection` in unit tests easily — we end up needing a real DB.

### 10.3 The Fix — Depend on an Abstraction

```java
// ✅ Abstraction
interface DBConnection {
    String connect();
}

// ✅ Low-level details — implement the abstraction
class MySQLConnection  implements DBConnection {
    public String connect() { return "Connected to MySQL"; }
}
class MongoDBConnection implements DBConnection {
    public String connect() { return "Connected to MongoDB"; }
}
class XYZSQLConnection  implements DBConnection {
    public String connect() { return "Connected to XYZ-SQL"; }
}

// ✅ High-level module — depends only on the abstraction
class PasswordReminder {
    private DBConnection dbConnection;        // ← interface, not concrete

    public PasswordReminder(DBConnection dbConnection) {
        this.dbConnection = dbConnection;
    }
}

// ✅ Caller (composition root) decides which implementation to use
public class Main {
    public static void main(String[] args) {
        DBConnection conn1 = new MySQLConnection();
        DBConnection conn2 = new MongoDBConnection();
        DBConnection conn3 = new XYZSQLConnection();

        PasswordReminder reminder = new PasswordReminder(conn1);  // swap freely
    }
}
```

### 10.4 DIP vs. Dependency Injection (DI) — Don't Confuse Them

This trips up most candidates. They are **related but different**:

| Term | What it is | Layer |
|---|---|---|
| **DIP** | A **principle** — "depend on abstractions, not concretions". | Design |
| **DI** | A **technique** — passing dependencies in (via constructor / setter / framework). | Implementation |
| **IoC container** (Spring, Guice) | A **tool** that automates DI. | Framework |

DI is the most common **mechanism** to implement DIP, but you can follow DIP without a framework — just hand-wire constructors at the application's composition root (the `main` method).

### 10.5 Real-World Industry Examples

All five of these are **the same pattern as `PasswordReminder` + `DBConnection`** from §10.3, just with different names. In each case:

- A **high-level module** (your business logic) depends only on an **interface**.
- The **low-level module** (the concrete tool / driver / library) implements that interface.
- The actual concrete class to use is decided **at runtime, in one place** — never inside the business logic.

Walk through each one slowly.

---

#### 1. Spring Framework

**What is Spring?**
A popular Java framework for building web apps. One of its main jobs is to **create your objects for you and wire them together** so you don't have to write `new SomeClass()` everywhere.

**Without DIP** — your service hard-codes the implementation:

```java
class OrderService {
    private OrderRepository repo = new JpaOrderRepository();   // tightly coupled
}
```

`OrderService` is now married to JPA. To swap to MongoDB or to use a mock in tests, you'd have to *change `OrderService` itself*. That's exactly the `PasswordReminder` problem from §10.2.

**With Spring (DIP)** — you declare the interface and Spring fills it in:

```java
class OrderService {
    @Autowired
    private OrderRepository repo;        // says "I need an OrderRepository"
}
```

`OrderService` never writes `new JpaOrderRepository()` anywhere. Instead, Spring scans the project at startup, finds classes annotated as components (`@Repository`, `@Service`, `@Component`, etc.), and "injects" the right one into the field.

##### How does Spring decide which implementation to pick when multiple exist?

This is the part that often feels like magic. Several mechanisms — the two most common are **profiles** and **test overrides**:

###### Mechanism A — `@Profile` for prod vs. test

Each implementation declares which environment it belongs to:

```java
// Real database-backed implementation — only loaded in prod
@Repository
@Profile("prod")
class JpaOrderRepository implements OrderRepository { ... }

// HashMap-backed fake — only loaded in tests
@Repository
@Profile("test")
class InMemoryOrderRepository implements OrderRepository { ... }
```

When you start the app, you set the active profile:

```
# Production startup
java -jar app.jar -Dspring.profiles.active=prod

# Test startup
java -jar app.jar -Dspring.profiles.active=test
```

- With `prod` active, Spring creates only `JpaOrderRepository` and injects that into `OrderService.repo`.
- With `test` active, Spring creates only `InMemoryOrderRepository` and injects *that* into the same field.
- `OrderService` is unchanged. It just sees an `OrderRepository`.

###### Mechanism B — `@MockBean` for unit tests (Spring Boot)

For unit tests, Spring Boot offers an even simpler way: mock the dependency with one annotation.

```java
@SpringBootTest
class OrderServiceTest {

    @MockBean
    private OrderRepository repo;        // Spring replaces the real bean with a Mockito mock

    @Autowired
    private OrderService service;        // service is built using the mocked repo

    @Test
    void placeOrder_savesViaRepository() {
        Order order = new Order(...);
        service.placeOrder(order);

        verify(repo).save(order);        // assertion using Mockito on the mock
    }
}
```

Spring sees the `@MockBean` and substitutes a Mockito-generated fake for `OrderRepository` in the application context. `OrderService` is wired with that fake — no real database is involved, the test runs in milliseconds.

##### Why it matters in practice

- **Production** uses `JpaOrderRepository` (real database).
- **Integration tests** use `InMemoryOrderRepository` (a `HashMap`, no database needed at all).
- **Unit tests** use a Mockito mock via `@MockBean`.
- All three scenarios run **the same `OrderService` code** with **zero changes** — only the wiring at the edges differs.

This is the `PasswordReminder` + `DBConnection` pattern from §10.3 in modern industrial form. The only difference is that Spring handles the wiring automatically (so you don't have to do `new` calls in `Main` yourself), and gives you a few annotations (`@Profile`, `@MockBean`, `@Primary`, `@Qualifier`) to control which implementation gets chosen when there are several.

---

#### 2. Logging — SLF4J

**What is SLF4J?**
A standard library Java apps use to write log messages (the lines like `User signed in`, `Payment failed`, etc. that you see in server logs). SLF4J is just an **interface** — it has no actual logging code by itself.

**Why does that even exist?**
Java has many real logging libraries — **Logback**, **Log4j2**, the JDK's built-in `java.util.logging`. They all do the same job (write log lines), but their classes have different names. Without DIP, your business code would import one of them directly:

```java
// Without DIP — coupled to Logback specifically
import ch.qos.logback.classic.Logger;
private Logger log = new LogbackLoggerImpl(MyClass.class);
```

Now if the company decides to switch from Logback to Log4j2, **every file in the codebase** needs editing.

**How SLF4J fixes it (DIP)** — your code only ever sees an interface:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private static final Logger log = LoggerFactory.getLogger(MyClass.class);
log.info("User signed in");
```

`Logger` here is the SLF4J **interface**. The actual implementation (Logback, Log4j2, etc.) is decided by which logging JAR is on the classpath at runtime — your business code doesn't reference it anywhere.

**Why it matters in practice:**
- Switch the entire app from Logback to Log4j2 → just change one line in `pom.xml` (the dependency JAR). **Zero code changes.**
- Open-source libraries publish against SLF4J so the *application* can pick whichever real logger it wants.

Same pattern: high-level code depends only on `Logger` (abstraction); the real implementation is the swappable detail.

---

#### 3. JDBC (Java Database Connectivity)

**What is JDBC?**
Java's **built-in standard** for talking to databases. It's a set of interfaces (`Connection`, `Statement`, `ResultSet`) shipped inside Java itself, in the `java.sql` package.

**Why is it interesting?**
There are many databases (MySQL, Postgres, Oracle, SQL Server). Each one speaks its own network protocol. If Java had only MySQL-specific classes, you couldn't switch databases without rewriting all your code.

**How JDBC follows DIP** — your code only uses the interfaces:

```java
import java.sql.Connection;
import java.sql.ResultSet;

Connection conn = DriverManager.getConnection(url);          // interface
ResultSet rs = conn.createStatement().executeQuery("...");   // interface
```

Each database vendor (MySQL, Postgres, Oracle) ships a **driver JAR** — a set of concrete classes that implement these interfaces. At startup, the driver registers itself with `DriverManager`. Your code never names a vendor's class directly.

**Why it matters in practice:**
- Your DAO code looks the same whether you're using MySQL, Postgres, or Oracle.
- To migrate databases: change the connection URL (e.g. `jdbc:mysql://...` → `jdbc:postgresql://...`) and swap the driver JAR. Code unchanged.
- This is DIP **at the JDK level** — Java ships with the interfaces; vendors plug in the details.

---

#### 4. Payment Gateways (Stripe / Razorpay / PayPal)

**What is a payment gateway?**
A third-party service that processes credit-card payments for an e-commerce app. **Stripe**, **Razorpay**, **PayPal** are competing providers — they all do the same job (charge a card, refund, etc.) but each has its own API.

**Without DIP** — your `OrderService` hard-wires Stripe:

```java
class OrderService {
    void placeOrder(Order o) {
        StripeClient stripe = new StripeClient();    // ← Stripe-specific
        stripe.charge(o.total());
        ...
    }
}
```

Now your *business logic* is married to Stripe. Real-world consequences:
- The CEO says "we want to also support Razorpay for India" → you must edit `OrderService` and add `if-else` branches.
- A developer in your unit test wants to test "order placement" without actually charging a card → impossible without real money.

**With DIP** — define your own abstraction:

```java
// Your domain (high-level)
interface PaymentGateway {
    void charge(BigDecimal amount);
}

class OrderService {
    private final PaymentGateway gateway;
    OrderService(PaymentGateway gateway) { this.gateway = gateway; }

    void placeOrder(Order o) {
        gateway.charge(o.total());     // doesn't know if it's Stripe or PayPal
    }
}

// Infrastructure (low-level adapters — one per provider)
class StripeAdapter   implements PaymentGateway { /* calls Stripe SDK */ }
class PayPalAdapter   implements PaymentGateway { /* calls PayPal SDK */ }
class FakePayment     implements PaymentGateway { /* records calls in memory, for tests */ }
```

The decision of which adapter to use lives in `Main` (or Spring config), not inside `OrderService`.

**Why it matters in practice:**
- Add a new payment provider → write one adapter class. `OrderService` is untouched.
- Unit tests use `FakePayment` → no real card is charged, tests run instantly.
- Integration tests use Stripe's sandbox → the same `OrderService` runs against a real provider.

Identical pattern to `PasswordReminder` + `DBConnection`. The only difference is the domain.

---

#### 5. Repository Pattern

**What is a repository?**
A class whose job is to save and load entities (objects like `Order`, `User`) from a data store. Methods look like `findById(123)`, `save(order)`, `delete(order)`. The repository **hides whether** the data lives in MySQL, MongoDB, an in-memory cache, or somewhere else.

**Without DIP:**

```java
class OrderService {
    private final EntityManager em;        // JPA-specific
    void save(Order o) { em.persist(o); }  // talks to JPA directly
}
```

Now `OrderService` knows about JPA. Switching to MongoDB later means rewriting `OrderService`. Tests against an in-memory store are awkward.

**With DIP** — repository as an interface:

```java
// Domain layer (high-level)
interface OrderRepository {
    Order findById(Long id);
    void  save(Order order);
}

class OrderService {
    private final OrderRepository repo;
    OrderService(OrderRepository repo) { this.repo = repo; }
}

// Infrastructure layer (low-level implementations)
class JpaOrderRepository      implements OrderRepository { /* uses JPA */ }
class MongoOrderRepository    implements OrderRepository { /* uses MongoDB */ }
class InMemoryOrderRepository implements OrderRepository { /* HashMap, for tests */ }
```

**Why it matters in practice:**
- **Domain layer stays clean** — no `EntityManager`, no `MongoClient`, no SQL strings cluttering the business logic.
- **Different stores for different needs** — production uses `JpaOrderRepository`, fast unit tests use `InMemoryOrderRepository` (no DB needed at all).
- **Database migration is local** — switching from MySQL to MongoDB only changes the repository implementation; `OrderService` doesn't notice.

---

#### Common pattern across all 5 — same idea, 5 different problems

| Example | High-level module | Abstraction (interface) | Low-level details (implementations) |
|---|---|---|---|
| Spring `@Autowired` | `OrderService` | `OrderRepository` | `JpaOrderRepository`, `InMemoryOrderRepository`, … |
| SLF4J | Your business class | `Logger` (SLF4J) | Logback, Log4j2, `java.util.logging` |
| JDBC | Your DAO code | `Connection`, `ResultSet` | MySQL driver, Postgres driver, Oracle driver |
| Payment gateways | `OrderService` | `PaymentGateway` (yours) | `StripeAdapter`, `PayPalAdapter`, `FakePayment` |
| Repository pattern | `OrderService` | `OrderRepository` (yours) | `JpaOrderRepository`, `MongoOrderRepository`, `InMemoryOrderRepository` |

In each row, you can substitute the names back into the §10.3 diagram and it still makes sense. **DIP is everywhere in real Java systems** — once you spot the "high-level depends on interface; low-level implements it; composition root picks one" pattern, you'll see it constantly.

### 10.6 DIP — Interview Cheat Sheet

| Question | One-liner answer |
|---|---|
| What is DIP? | Depend on abstractions, not concretions. Both high- and low-level modules depend on interfaces. |
| Why? | Decouples business logic from infrastructure; allows swapping implementations without changes; makes unit testing easy via mocks. |
| Inversion of what? | Inversion of the **direction of dependency** — instead of high-level depending on low-level, both depend on an interface. |
| Different from DI? | DIP is a principle; DI is a technique to achieve it. |
| Spring example? | `@Autowired` on an interface type; Spring picks the implementation. |

---

## 11. DRY — Don't Repeat Yourself

> *"Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."* — *The Pragmatic Programmer*

### 11.1 The Naive Definition

**Avoid duplicating logic in code. Keep it centralized and reusable.**

If you find yourself copy-pasting a block of code more than twice, **extract it into a method, class, or module**.

### 11.2 The Lecture Example — Email Validation

Imagine in a project you have three features:

- **Login** — validate the email before authenticating.
- **Signup** — validate the email before creating the account.
- **Notifications** — validate the email before sending.

A naïve developer copy-pastes the validation everywhere:

```java
// ❌ DRY violation — same logic in 3 places (login, signup, notifications)
class LoginService {
    void login(String email, String password) {
        if (email.contains("@") && email.contains(".")) {  // duplicated logic
            // proceed
        }
    }
}

class SignupService {
    void signup(String email) {
        if (email.contains("@") && email.contains(".")) {  // duplicated logic
            // proceed
        }
    }
}

class NotificationService {
    void send(String email) {
        if (email.contains("@") && email.contains(".")) {  // duplicated logic
            // proceed
        }
    }
}
```

**Problem:** If tomorrow the validation rule changes (e.g., regex check, must end with a TLD, no `+` aliases), you need to find and update **every copy**. Miss one — bug in production.

**DRY-compliant fix — extract to one place:**

```java
public class EmailValidator {
    public static boolean validateEmail(String email) {
        return email != null
            && email.contains("@")
            && email.contains(".");
    }
}

class LoginService        { void login (String e) { if (EmailValidator.validateEmail(e)) { /* ... */ } } }
class SignupService       { void signup(String e) { if (EmailValidator.validateEmail(e)) { /* ... */ } } }
class NotificationService { void send  (String e) { if (EmailValidator.validateEmail(e)) { /* ... */ } } }
```

Now changing the rule means changing **one method**, not 100 sprinkled `if` statements.

### 11.3 DRY in Practice — Where to Apply

| Layer | Apply DRY by |
|---|---|
| **Methods** | Extract repeated blocks into private helpers. |
| **Classes** | Pull common fields/methods into a base class or composition. |
| **Modules** | Common utilities into a shared library (`commons-utils`). |
| **Database** | Normalize tables; one source of truth for entities. |
| **Configuration** | Centralized config (Spring `@Value`, environment variables, config service). |
| **Documentation** | Generate docs from code (Javadoc, OpenAPI) instead of maintaining duplicates. |

### 11.4 When DRY Becomes Harmful (The "WET" Counter-Argument)

So far DRY has only sounded like a good thing. But there's a famous trap: **applying DRY too eagerly can actually make the code *worse* than the duplicated version.** Let's see exactly how.

#### The trap: code that *looks* the same but isn't really the same thing

Suppose your e-commerce app has two features that both compute a "total":

```java
// 1) Customer's shopping cart total
double customerCartTotal(List<Item> items) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    return subtotal + subtotal * 0.18;          // 18% sales tax
}

// 2) Vendor invoice total
double vendorInvoiceTotal(List<Item> items) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    return subtotal + subtotal * 0.18;          // 18% sales tax
}
```

A junior developer reads this, says *"these are identical — DRY violation!"*, and extracts a shared helper:

```java
// "DRY" extraction — both use this now
double total(List<Item> items) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    return subtotal + subtotal * 0.18;
}
```

Looks great. Until six months later, when product requirements arrive…

#### Six months later: requirements diverge

- **Customer team** says: *"Customers with a coupon should get a 10% discount before tax is applied."*
- **Vendor team** says: *"Vendor tax is now 12% (different agreement), and we need to deduct a 5% withholding from the final total."*

These two rules have *nothing to do with each other* — they're driven by completely different concerns (consumer marketing vs. vendor accounting). But the shared `total()` method serves both. So now it has to grow:

```java
// The "DRY" method, after a few rounds of conflicting requirements
double total(List<Item> items,
             boolean isCustomer, double customerDiscount,
             boolean isVendor) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    if (isCustomer) subtotal -= subtotal * customerDiscount;
    double taxRate = isCustomer ? 0.18 : 0.12;
    double total = subtotal + subtotal * taxRate;
    if (isVendor) total -= total * 0.05;        // withholding
    return total;
}
```

Look at what just happened:
- Every caller now has to pass a pile of flags they don't care about.
- Reading the function, you see logic for *both* domains tangled together.
- If a third use case appears (e.g. *internal employee discount*), you add yet another flag and yet another `if`.
- A bug fix for the customer rule risks breaking the vendor rule, because they share the same function.

This is what **"premature abstraction"** means: you abstracted two things into one *before you knew whether they would stay the same*. They didn't. And now the abstraction is fighting you.

#### What "different domain concepts" means

`customerCartTotal` and `vendorInvoiceTotal` *looked* identical, but they answer **different business questions**:

| | Customer cart total | Vendor invoice total |
|---|---|---|
| Who pays? | The shopper | Your company (paying the vendor) |
| Driven by | Marketing rules (coupons, loyalty) | Accounting rules (tax, withholding) |
| Likely to evolve in lockstep? | **No** — they will diverge as each side gets its own rules |

Two methods that compute the same arithmetic *today* are not the same concept. The arithmetic was a coincidence. The **purpose** is what matters when deciding whether to merge them.

#### What the right answer would have been

Keep the two methods separate from the start. Even though they look identical right now:

```java
double customerCartTotal(List<Item> items) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    return subtotal + subtotal * 0.18;
}

double vendorInvoiceTotal(List<Item> items) {
    double subtotal = items.stream().mapToDouble(Item::price).sum();
    return subtotal + subtotal * 0.18;
}
```

Now when the customer rule changes, you only edit `customerCartTotal`. When the vendor rule changes, only `vendorInvoiceTotal`. Each function stays small, clear, and focused on one purpose. No flags. No tangled logic.

> **Comparing it back to §11.2 (email validation):** there, all three callers (login, signup, notifications) were doing the *same conceptual thing* — checking that a string is a valid email. They were *expected* to share rules forever. Extracting was the right move. Here, customer total and vendor total *look* the same but are doing two different conceptual things, and they're *expected* to drift apart. Extracting is the wrong move. **What separates the two situations is the meaning, not the syntax.**

#### The "Rule of Three"

A simple practical guideline used by experienced engineers:

> **Don't extract on the first duplicate. Don't extract on the second. Wait for the third.**

The reasoning:
- **First copy** — there's only one place; no duplication problem yet.
- **Second copy** — *might* be a true duplicate, *might* be coincidence. Hard to tell.
- **Third copy** — by now you can see the *real shape* of what's being duplicated. Now you have enough evidence to abstract correctly.

Waiting until the third occurrence costs you very little (a tiny bit of typing) and protects you from the much more expensive mistake of building the wrong abstraction.

#### WET — "Write Everything Twice"

A counter-acronym used in microservices and other distributed systems:

> **WET = Write Everything Twice — a little duplication is cheaper than the wrong abstraction.**

The idea isn't that duplication is *good*. It's that the cost of duplication is well-understood (a bit of extra typing and the risk of inconsistent edits), but the cost of a wrong shared abstraction can be enormous (tangled code across teams, painful migrations, hard-to-fix bugs that span multiple unrelated features).

When in doubt — especially across **different teams**, **different bounded contexts**, or **different microservices** — duplicate first, extract later when the right shape becomes obvious.

#### Summary — when DRY helps and when it hurts

| Situation | What to do | Why |
|---|---|---|
| Same logic, **same concept**, called in 3+ places (e.g. email validation) | **Extract** — apply DRY | The rules will evolve together; one place to change is a win |
| Logic that *looks* identical but represents **different concepts** (e.g. customer total vs vendor total) | **Don't extract** — leave it duplicated | The rules will evolve apart; merging creates a flag-ridden mess |
| Only seen the duplication twice and unsure | **Wait for the third** | Two might still be coincidence; gather more evidence first |

DRY is a heuristic, not a law. The real principle behind it is:

> **One source of truth per concept.** If two pieces of code represent one concept, merge them. If they represent two concepts that just happen to look alike, leave them alone.

### 11.5 Real-World Industry Examples

1. **Validation utilities.** Spring's `org.springframework.util.StringUtils`, Apache Commons' `StringUtils.isBlank()` — every project re-uses one centralized helper.
2. **Constants files.** Status codes, error messages, API paths in one `Constants.java` instead of magic strings sprinkled around.
3. **Database migrations.** Tools like Flyway/Liquibase — one source of truth for schema, applied consistently to every environment.
4. **OpenAPI / Swagger.** API contract defined once; client SDKs and server stubs generated from it.

---

## 12. KISS — Keep It Simple, Stupid

> *"Simplicity is the ultimate sophistication."* — Leonardo da Vinci

### 12.1 The Naive Definition

**Avoid over-engineering. Pick the simplest design that solves the problem.**

A junior engineer's instinct is to apply every pattern they just learned. KISS says: if a `for` loop solves it, don't introduce a strategy pattern + a factory + a registry + a dependency-injected service.

### 12.2 The Lecture Example — `isEven(int n)`

You need to check if a number is even. An over-eager developer might do this:

```java
// ❌ Over-engineered — unnecessary abstractions for a one-liner
class NumberProcessor {
    public boolean isEven(int number) {
        EvenChecker checker = new EvenCheckerFactory().getChecker();
        return checker.check(number);
    }
}

interface EvenChecker {
    boolean check(int number);
}

class EvenCheckerImpl implements EvenChecker {
    @Override
    public boolean check(int number) {
        return number % 2 == 0;
    }
}

class EvenCheckerFactory {
    public EvenChecker getChecker() {
        return new EvenCheckerImpl();
    }
}
```

**What's wrong?** The actual logic is a single modulo. We've built a factory + interface + impl class for `n % 2 == 0`. This is what's called **Pattern-itis** — applying patterns where they aren't needed.

**KISS-compliant version:**

```java
// ✅ Simple, readable, exactly as much complexity as the problem warrants
public class NumberUtils {
    public static boolean isEven(int num) {
        return num % 2 == 0;
    }
}
```

### 12.3 When to Add Complexity (and When Not)

The hardest part of KISS isn't *spotting* over-engineering — it's deciding **at the time you write the code** whether a given abstraction is worth adding. Here's a decision flow you can actually use.

#### The decision flowchart

```
Q1: Do I have a REAL, DEMONSTRATED need for this abstraction RIGHT NOW?
│
├── No  → STOP. Keep it simple. Inline the logic. Delete the interface.
│         (You can always refactor LATER when a real need shows up.)
│
└── Yes → Q2: Is the abstraction earning its keep?
              (Does it serve at least one of these concrete purposes?)
              │
              │   ✓ Multiple real implementations exist or are imminent
              │   ✓ I need to mock this in unit tests
              │   ✓ I need a published extension point for plugins
              │   ✓ It enforces a contract across team boundaries
              │
              ├── Yes → KEEP IT. The abstraction pays rent.
              │
              └── No  → REMOVE IT. It's complexity without benefit.
                        (Mark a TODO and tear it out in the next refactor.)
```

#### What "real, demonstrated need" means (Q1)

This is where most beginner mistakes happen. The word **"real"** is doing the heavy lifting here. Compare:

| Real need (build it) | Hypothetical need (don't build it) |
|---|---|
| *"We have two payment providers in production today (Stripe + Razorpay) so we genuinely need a `PaymentGateway` interface."* | *"We use Stripe today but might switch to PayPal someday — let's add a `PaymentGateway` interface just in case."* |
| *"Our test suite needs to run without a real DB, so we need an `OrderRepository` interface to mock."* | *"We might want to switch databases someday — let's wrap JPA in 4 layers just in case."* |
| *"Marketing wants to support 3 different discount strategies starting next sprint, so the Strategy pattern is justified."* | *"We have one discount calculation now, but maybe later we'll have more — let's add Strategy now."* |

The pattern: a real need is a **need you have today** (or one that's already on a confirmed near-term roadmap, like "approved for next sprint"). A hypothetical need is *"maybe someday"* — which is a category that almost never materializes the way you imagined.

#### What "earning its keep" means (Q2)

An abstraction "earns its keep" if you can point to **at least one concrete benefit** it provides *right now*. The four most common reasons:

1. **Multiple real implementations exist.** Two or more concrete classes implement the interface today. (One implementation = the interface is just ceremony.)
2. **You need to mock it in unit tests.** A `FakeXxxRepository` or `Mockito` mock is being used in tests right now. (The interface lets the mock substitute for the real thing.)
3. **It's a published extension point.** Other teams or plugins implement the interface. (E.g. SLF4J, JDBC, Spring's `BeanPostProcessor`.)
4. **It enforces a contract across team boundaries.** Frontend and backend have agreed on a shape; the interface is the contract. (E.g. an OpenAPI spec generating Java interfaces.)

If none of these apply, you're paying a tax (extra files, extra cognitive load) for nothing — tear the interface out and code directly against the implementation.

#### Re-examining our KISS example (`isEven`)

Run the over-engineered version through the flowchart:

- **Q1:** Is there a real, demonstrated need for an interface, an impl class, and a factory around `n % 2 == 0`?
  → **No.** There's only one possible "implementation" of evenness. We're not mocking modular arithmetic in tests. There's no plugin system. *Stop here, inline it.*

- For the simple version (`return num % 2 == 0`): nothing to question. Done.

That's the entire decision tree applied. Two questions, both answered honestly, and the right design falls out automatically.

---

### 12.3.1 YAGNI — *You Aren't Gonna Need It*

YAGNI is KISS's twin sibling. Where KISS says *"keep what you build simple"*, YAGNI says *"don't build it at all until you need it"*.

#### The principle

> *"Always implement things when you actually need them, never when you just foresee that you might need them."* — Ron Jeffries (XP / Extreme Programming, 1999)

The rule is short, but it cuts deep. YAGNI says **a feature, abstraction, or configuration knob you "might need someday" should not exist in the codebase today** — even if it's small, even if it's "free", even if you're sure you'll use it later.

#### Why "we'll need it someday" is a trap

Whenever you add a feature you don't yet need, you pay these costs **immediately**, every day, until you remove it:

| Cost | Why it matters |
|---|---|
| **Cognitive load** | Every reader of the code has to understand the unused code path and wonder *"why is this here?"* |
| **Maintenance burden** | When you upgrade a dependency, refactor a class, or change the language version, the unused code has to be migrated too. |
| **Bug surface** | Untested or rarely-used code paths grow bugs in silence. |
| **Test cost** | Pretty much by definition, code that "isn't used yet" doesn't have realistic tests, so the *first* real user of it discovers all the bugs. |
| **Wrong abstraction risk** | When the real need finally arrives, it's almost always *slightly different* from what you imagined — and now you have to refactor your speculative code anyway, often with more pain than if you'd built it fresh. |

In other words: **speculative code costs you today, and usually has to be rewritten when you actually need the feature.** You pay twice and benefit once — *if you ever get the feature at all*.

#### Concrete YAGNI violations you'll see in real code

| Speculative thing in the code | What was the actual need at the time? |
|---|---|
| `interface PaymentGateway` with one impl (`StripeAdapter`) | "We use Stripe today; *maybe* PayPal later." |
| A `RetryPolicy` configuration with 3 strategies | "We retry once today; *maybe* exponential backoff later." |
| A `MultiTenant` flag on every entity | "We have one customer today; *maybe* SaaS later." |
| `private` setters that nobody calls | "*Someone* might want to change it later." |
| A pluggable cache layer wrapping a `HashMap` | "We use an in-memory map; *maybe* Redis later." |
| 4 layers (Controller → Service → Manager → DAO) when the work is one DB query | "Maybe we'll add more business logic between layers later." |

In every case, the *italicized* future use was speculative. **Most of those somedays never come.** And for the ones that do come, the speculative code is almost always thrown out and rewritten because the real requirement turned out to be different.

#### How YAGNI plays with KISS and DRY

| Principle | Says | Trigger |
|---|---|---|
| **KISS** | Keep what you do build simple | Applied while you're designing/coding right now |
| **YAGNI** | Don't build what you don't need yet | Applied to the *decision* of whether to add a feature/abstraction at all |
| **DRY** | Don't duplicate concepts | Applied when the same logic appears in 3+ places (with the WET caveat from §11.4) |

YAGNI and KISS are best friends — both push you toward less code. DRY can sometimes pull the other way (toward more abstraction). The tie-breaker is the §11.4 *Rule of Three* and the *"different concepts vs same concept"* test.

#### A practical decision rule when you're tempted to over-engineer

Ask yourself these three questions, in order:

1. **Do I have a confirmed real need in this sprint or the next one?**
   - **No** → don't build it.
   - **Yes** → continue.

2. **Is the simple version close to "good enough" for the real need I have today?**
   - **Yes** → build the simple version.
   - **No** → continue.

3. **Will the abstraction earn its keep on day one?** (Use the four criteria from §12.3.)
   - **Yes** → build the abstraction.
   - **No** → build the simple version *and add a TODO* describing the future scenario, so the next person knows where to refactor when the real need arrives.

#### One-line takeaway

> **KISS says "make what you build simple". YAGNI says "don't build things you don't yet need". Together they're the most reliable defense against over-engineering — the failure mode that makes codebases slow, fragile, and hard to read. Build for the requirement on your desk today; let tomorrow's requirement justify tomorrow's complexity.**

### 12.4 Real-World Industry Examples

1. **Go's standard library.** Famously avoids over-abstraction; many things are concrete structs and functions, not deep interface hierarchies.
2. **`grep`, `sed`, `awk`.** Each does one thing well. Composition via pipes — no plug-in architecture, no DI container.
3. **Stripe's API.** Every endpoint returns predictable JSON. No GraphQL gymnastics where REST works fine.
4. **Bug-fix PRs.** A 5-line fix is better than a 500-line refactor — even if the refactor is "cleaner" — because it's reviewable, revertible, and ships today.

### 12.5 DRY vs. KISS — They Can Conflict

| | DRY | KISS |
|---|---|---|
| **Goal** | Eliminate duplication | Keep things simple |
| **Risk if abused** | Premature abstraction | Copy-paste sprawl |
| **Resolution** | Use the *Rule of Three* — extract on the third duplication, not the first |

---

## 13. Design Patterns — Introduction

> A **design pattern** is a reusable, named solution to a commonly-occurring problem in software design. Patterns are *templates*, not *libraries* — you adapt them to your code.

#### A real-world analogy first

Forget code for a moment. Think of a **chef** learning to cook.

When a beginner chef wants to make a dish, they often start from scratch every time — figuring out how to mix sauce, when to add salt, how to plate. After years of cooking, they realize that *certain combinations of techniques keep coming back*: "make a roux to thicken sauces", "marinate proteins in acid + salt overnight", "blanch vegetables before stir-frying". These aren't recipes for specific dishes — they're **reusable techniques** that show up in many recipes.

If a senior chef tells a junior, *"start with a roux,"* the junior immediately knows: "heat butter, whisk in flour, cook until golden." A whole sequence of steps compressed into one named technique.

**Design patterns are exactly this — but for code.** They're proven, named techniques that experienced engineers reach for over and over when they encounter familiar problem shapes. When a senior dev says *"let's use a Strategy here,"* every other senior dev in the room immediately knows the structure they have in mind, without having to describe it.

So a design pattern is **not**:
- A library you import.
- Working code you copy-paste.
- A tool that solves a specific business problem.

A design pattern **is**:
- A *template* — a class structure you adapt to your particular case.
- A *vocabulary* — a shared name engineers use to talk about a recurring solution shape.
- A *thinking tool* — a way of recognizing "I've seen this kind of problem before; here's the proven shape."

---

### 13.1 Why Do Design Patterns Exist?

Patterns emerged because experienced engineers, working independently in different companies on different products, kept solving the same problems in the same ways.

##### The same problems kept appearing

- *"I need to make sure only one instance of a configuration object exists in the whole app."*
- *"I have many ways to compute a discount and I want to swap them at runtime without `if-else` chains."*
- *"I have an old library and a new library that do similar things but have different method names — how do I make my code work with both?"*
- *"How do I notify many parts of the system when one piece of data changes?"*

Each of these problems has a **proven shape** — a way to organize classes and methods that works well, that handles edge cases, that future-proofs the code. Naming these shapes gives engineers a way to **share solutions across the entire industry**.

##### What patterns give you in practice

| Benefit | What it means concretely |
|---|---|
| **Battle-tested solutions** | These have been used by millions of developers across decades. The bugs and edge cases have already been found and ironed out. |
| **Shared vocabulary** | Saying *"let's use a Strategy here"* in a code review takes 2 seconds. Describing the same structure in plain words might take a paragraph. |
| **Faster design** | When a familiar problem shows up, you don't reinvent the solution — you reach for the matching pattern and adapt it. |
| **Easier code reviews** | When the reviewer recognizes a pattern, they can focus on whether you applied it correctly instead of trying to understand the structure from scratch. |
| **Easier onboarding** | A new team member who knows the patterns can read your codebase faster — they recognize the shapes. |
| **Improves design instincts** | After learning a few patterns, you start seeing them everywhere — and your own designs naturally improve. |

##### Why a beginner specifically should learn patterns

Patterns are also **interview gold** — both LLD (low-level design) and even some HLD (high-level design) interviews routinely ask:

- *"Design a parking lot."* (uses Strategy + Factory + Singleton)
- *"Design Splitwise."* (uses Observer + Composite)
- *"Design a logging library."* (uses Singleton + Strategy + Chain of Responsibility)
- *"Implement an LRU cache."* (uses Adapter / Composite, depending on style)

Without a vocabulary of patterns, you'll *re-derive* solutions on the whiteboard from scratch — slower, error-prone, and harder to communicate. With patterns, you map the problem to a known shape and demonstrate experience.

---

### 13.2 The Origin Story — The "Gang of Four" (GoF)

In 1994, four computer scientists — **Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides** — published a famous book:

> ***Design Patterns: Elements of Reusable Object-Oriented Software***

This book is universally referred to as the **"GoF book"** ("Gang of Four"). It documented **23 patterns** they had observed across many real codebases — patterns that experienced engineers were independently arriving at over and over.

##### Why this book mattered

- It gave the patterns **standard names** (Singleton, Factory, Strategy, Observer, etc.). Before the book, different engineers used different names for the same idea, making cross-team communication painful.
- It gave each pattern a **standard structure** with class diagrams.
- It explained **when to use** and **when not to use** each one (the trade-offs).
- It became *the* reference book for a generation of OOP engineers.

The 23 patterns are still the foundation of the field. Newer patterns have been added since (like Repository or Dependency Injection), but the original 23 remain the core curriculum.

##### How many patterns do you actually need to know?

You **don't** need all 23 to do well in interviews or in real work. Most professional engineers deeply know maybe 8–10 patterns and recognize the rest. For a beginner, focus on a much smaller set first:

| Priority | Patterns to learn first |
|---|---|
| **Must know** | Singleton, Factory, Builder, Strategy, Observer |
| **Important** | Adapter, Decorator, Template Method, Composite |
| **Useful** | Facade, Proxy, State, Iterator, Command |
| **Niche** | Bridge, Flyweight, Prototype, Mediator, Memento, Visitor, Chain of Responsibility, Abstract Factory, Interpreter |

We'll work through the *must-know* and *important* ones in detail later in this guide. The rest you'll naturally pick up as you encounter them.

---

### 13.3 Patterns Are Templates, Not Code You Copy

This is the single most common confusion for beginners.

##### Quick reference — what "shape" means in design-pattern talk

> A pattern's **"shape"** = the structural arrangement of classes and their relationships (which classes exist, what their roles are, how they connect), **independent of any specific business domain**. The shape is what stays constant across every application of the pattern; the *names* and *business logic* inside the classes are yours to fill in. Think of it as the floor plan of a house — same plan, different furniture.

##### What "template" means

A pattern describes the **shape** of a solution: which classes exist, what their roles are, how they relate to each other. But the **specific names** and **specific business logic** are entirely yours.

For example, the *Strategy pattern* says:

> *"Define a family of algorithms behind one interface, and let the caller pick which one to use at runtime."*

That description doesn't mention any specific business domain. You can apply it to:

- **Discount calculation** — `DiscountStrategy` interface; `PercentDiscount`, `FlatDiscount`, `NoDiscount` implementations.
- **Shipping cost** — `ShippingStrategy` interface; `StandardShipping`, `ExpressShipping`, `FreeShipping` implementations.
- **Sorting** — `SortStrategy` interface; `QuickSort`, `MergeSort`, `BubbleSort` implementations.
- **Authentication** — `AuthStrategy` interface; `PasswordAuth`, `OAuth`, `BiometricAuth` implementations.

All four use *the same Strategy pattern shape*, with completely different domain code inside. **The pattern is the shape. The code is yours.**

##### Why this matters

Beginners sometimes search GitHub for "Singleton class Java" and copy-paste a class verbatim. That **misses the point**. You need to:

1. Recognize that your problem fits the pattern's shape.
2. Adapt the pattern to your specific classes, fields, and methods.
3. Decide which trade-offs of the pattern apply to your situation.

Patterns are the *plan*; the *building* is still up to you.

---

### 13.4 The Three Categories

The 23 GoF patterns are grouped into **three categories**, based on what kind of problem they solve. Knowing which category a pattern belongs to is *itself* a useful intuition.

```
                       ┌─────────────────────────┐
                       │     Design Patterns      │
                       └────────────┬────────────┘
                                    │
        ┌───────────────────────────┼─────────────────────────┐
        │                           │                          │
        ▼                           ▼                          ▼
┌──────────────────┐       ┌──────────────────┐       ┌────────────────────┐
│   Creational     │       │    Structural    │       │    Behavioural     │
│ "How to create   │       │ "How classes &   │       │ "How objects       │
│  objects"        │       │  objects are     │       │  interact &        │
│                  │       │  composed"       │       │  communicate"      │
├──────────────────┤       ├──────────────────┤       ├────────────────────┤
│ • Singleton      │       │ • Adapter        │       │ • Strategy         │
│ • Factory        │       │ • Decorator      │       │ • Observer         │
│ • Abstract       │       │ • Facade         │       │ • Command          │
│   Factory        │       │ • Composite      │       │ • State            │
│ • Builder        │       │ • Proxy          │       │ • Iterator         │
│ • Prototype      │       │ • Bridge         │       │ • Template Method  │
│                  │       │ • Flyweight      │       │ • Chain of Resp.   │
└──────────────────┘       └──────────────────┘       └────────────────────┘
```

##### Category 1 — **Creational Patterns** ("How to create objects")

These patterns deal with the question: *"how should I instantiate objects in a flexible, reusable way?"*

In simple programs, you just call `new Foo()` and you're done. But sometimes that's not flexible enough:

- *"I want to make sure only one instance ever exists."* → **Singleton**
- *"I want to decide which subclass to instantiate based on a runtime value."* → **Factory**
- *"I have an object with 10 optional fields and constructor calls have become unreadable."* → **Builder**
- *"I want to clone an existing object cheaply."* → **Prototype**
- *"I want to create families of related objects (Mac vs Windows widgets)."* → **Abstract Factory**

All five answer the same kind of question: *"what's a better way to make objects than just `new`?"*

##### Category 2 — **Structural Patterns** ("How classes and objects are composed")

These patterns deal with the question: *"how should I compose existing classes into bigger structures?"* They're about **the relationships between classes**, not about creation or behavior.

- *"I have an old API that doesn't fit the interface my code expects — bridge them."* → **Adapter**
- *"I want to add features to an object dynamically without modifying it."* → **Decorator**
- *"I want to hide a complicated subsystem behind one simple interface."* → **Facade**
- *"I want to treat individual items and groups of items uniformly."* → **Composite**
- *"I want a stand-in object that controls access to the real one."* → **Proxy**

All five answer: *"how do I plug existing pieces together into a useful structure?"*

##### Category 3 — **Behavioural Patterns** ("How objects interact and communicate")

These patterns deal with the question: *"how should objects collaborate and pass responsibilities around?"* They're about **runtime collaboration**, not creation or static structure.

- *"I have a family of algorithms; let the caller pick one at runtime."* → **Strategy**
- *"When one object's state changes, automatically notify many others."* → **Observer**
- *"I want to encapsulate a request as an object (so it can be queued, undone, logged)."* → **Command**
- *"An object's behavior changes depending on its internal state (e.g. order is `New` vs `Shipped`)."* → **State**
- *"I want to step through a collection without exposing its internal structure."* → **Iterator**
- *"I want to define the skeleton of an algorithm, but let subclasses fill in specific steps."* → **Template Method**

All answer: *"how do these objects interact at runtime?"*

##### Quick reference

| Category       | Question it answers                  | Patterns we'll cover here     |
|----------------|--------------------------------------|-------------------------------|
| **Creational** | "How are objects **created**?"       | Singleton, Factory, Abstract Factory, Builder |
| **Structural** | "How are objects **composed**?"      | (next lecture)                |
| **Behavioural**| "How do objects **interact**?"       | (later lecture)               |

---

### 13.5 When NOT to Use Design Patterns

This is where KISS and YAGNI from §12 come back. **Patterns are not magic** and not every problem deserves one.

##### Common pitfalls

1. **Pattern-itis** — applying patterns just because you learned them.
   - "I learned Strategy yesterday. Let me wrap my single discount calculation in a Strategy pattern."
   - The result: 4 classes for `n * 0.10`, just like the `isEven` over-engineering in §12.2.

2. **Premature abstraction** — applying a pattern *before* you have evidence you need its flexibility.
   - "We might support multiple payment providers someday." → don't add a Strategy yet (YAGNI from §12.3.1).

3. **Wrong pattern for the problem** — forcing a pattern that doesn't quite fit, instead of writing simpler code.
   - Using Observer for a simple synchronous flow that has only one listener.

##### A simple decision rule

> **First, write the simplest code that solves the problem.** If it works, ship it. If you later find yourself adding flag parameters, copy-pasting, or struggling to extend it, then ask *"is there a pattern that solves exactly this kind of mess?"* If yes, refactor toward the pattern. If no, keep going.

Patterns earn their keep when they replace something painful (`if-else` chains, hardcoded coupling, brittle code). They become a tax when they replace something that was already simple.

---

### 13.6 How Patterns Connect to What You've Learned

You might worry that design patterns are some completely new thing. They're not — they're built directly on the OOP atoms and SOLID principles you already understand.

| Concept you already know | How patterns use it |
|---|---|
| **Interfaces / abstract classes** | Almost every pattern uses an interface to decouple the caller from the implementation. (DIP from §10.) |
| **Polymorphism** | Strategy, Observer, State, Template Method all rely on the same `interface` reference holding different implementations. |
| **Composition** | Decorator and Composite are textbook examples of "favor composition over inheritance". |
| **Single Responsibility (SRP)** | Each class in a pattern usually has exactly one reason to change. |
| **Open/Closed (OCP)** | Strategy, Decorator, Visitor — all let you *add* new behavior by adding new classes, without modifying existing ones. |

So a design pattern is essentially a **specific recipe** that combines OOP atoms (classes, interfaces, polymorphism) with SOLID principles (DIP, OCP, SRP) to solve a recurring problem. **You already have the ingredients.** Patterns just teach you the recipes.

---

### 13.7 What's Coming Up

The remainder of this notes file walks through the most important patterns one at a time, in this order:

1. A quick **threading primer** (§14) — needed before we discuss Singleton's thread-safety.
2. **Singleton** — your first creational pattern, with all its concurrency subtleties.
3. **Factory** and **Abstract Factory** — flexible object creation.
4. **Builder** — clean construction of complex objects.
5. (Then structural and behavioural patterns in subsequent lectures.)

By the time you finish all of them, you'll have ~10 patterns in your toolbox and the instinct to recognize when each one applies. That's enough for almost any LLD interview.

---

---

## 14. Threading & Synchronization Primer (Prerequisite for Singleton)

> *Read this section carefully before Singleton — the Singleton implementation we'll discuss only makes sense after you understand why threads need synchronization.*

### 14.1 Process vs. Thread (the 30-second mental model)

- A **process** is a running program (e.g., one JVM, one Chrome tab). It owns its own memory, file handles, etc.
- A **thread** is a single line of execution **inside** a process. Threads inside the same process **share memory** (same heap).
- Modern apps are **multithreaded** — your Spring Boot service runs hundreds of threads (one per HTTP request).

```
┌────────────────── PROCESS (one JVM) ──────────────────┐
│   ┌──────── shared HEAP (objects, statics) ────────┐  │
│   │   instance: DBConnManager  (only ONE!)          │ │
│   └──────────────────────────────────────────────────┘ │
│   ┌──────── Thread T1 ────────┐  ┌── Thread T2 ──┐    │
│   │  call getInstance()        │  │ call          │    │
│   │  read instance == null     │  │ getInstance() │    │
│   │  about to new()...         │  │ read instance │    │
│   └────────────────────────────┘  └───────────────┘    │
└────────────────────────────────────────────────────────┘
```

### 14.2 Why Multithreading Causes Bugs — the Race Condition

Threads execute **interleaved**. When two threads read and write the **same shared variable** without coordination, you get a **race condition** — the result depends on timing.

**Classic bank account example:**

```java
class Account {
    int balance = 100;

    void withdraw(int amount) {
        if (balance >= amount) {        // ← Thread-1 reads balance = 100
            balance = balance - amount; // ← Thread-2 also reads balance = 100
        }                               //   Both succeed; balance ends at 0 instead of -100
    }
}
```

If two threads both call `withdraw(100)` at the same instant, **both can pass the `if` check** before either updates `balance`. End result: account overdrawn — a real-world catastrophe.

### 14.3 What is the `synchronized` Keyword?

`synchronized` in Java is the basic **mutual exclusion (mutex) lock** built into every Java object.

> When a thread enters a `synchronized` block, it **acquires the lock** on a target object (the *monitor*). No other thread can enter ANY `synchronized` block on the same monitor until the first thread exits.

Two flavours:

#### (a) **Method-level** synchronization

```java
public synchronized void withdraw(int amount) {
    // entire method body is the critical section
    // lock is on `this`
}
```

Equivalent to wrapping the whole method in `synchronized(this) { ... }`.

#### (b) **Block-level** synchronization

```java
public void withdraw(int amount) {
    // non-critical work here (NO lock yet)
    synchronized (this) {     // ← only THIS block holds the lock
        if (balance >= amount) {
            balance -= amount;
        }
    }
    // more non-critical work (lock already released)
}
```

For static methods / class-level locks:

```java
synchronized (DBConnManager.class) { ... }   // lock on the Class object
```

### 14.4 Why Use a `synchronized` *Block* Instead of the Whole Method?

**Performance.** A synchronized method makes **every** caller wait, even when the work doesn't need protection. A synchronized block lets you protect *only the critical section*. This is exactly the trick used in the **Double-Checked Locking** Singleton in §15.

```
Method-level (lecture: "each request needs to wait")    Block-level (only critical section locked)
───────────────────────────────────────────────────     ─────────────────────────────────────────────
T1 ───[ entire method LOCKED ]─────►                    T1 ──work──[ tiny LOCK ]──work──►
T2     ▒▒▒▒▒▒▒▒ blocked ▒▒▒▒▒▒▒▒───[ now runs ]──►      T2 ──work──[ tiny LOCK ]──work──►
T3     ▒▒▒▒▒▒▒▒ blocked ▒▒▒▒▒▒▒▒▒▒▒▒▒▒───[ now ]►       T3 ──work──[ tiny LOCK ]──work──►
        (high contention, slow)                          (low contention, fast)
```

### 14.5 The `volatile` Keyword (you'll see it in Singleton)

`volatile` ensures that:
1. **Every read** of the variable reads from main memory (not the CPU cache).
2. **Every write** is flushed to main memory immediately.
3. Prevents the compiler/CPU from **reordering** instructions around the volatile access.

Without `volatile`, Thread-2 might read a half-constructed object that Thread-1 just `new`-ed up, because the JVM can reorder the assignment vs. the constructor calls. `volatile` says: *"hands off, do this in order, and make it visible to everyone."*

### 14.6 Quick Mental Model

| Tool          | Solves                                | Cost                |
|---------------|---------------------------------------|---------------------|
| `synchronized` (method) | Race condition, big & blunt | High (lock held the whole call) |
| `synchronized` (block)  | Race condition, fine-grained| Lower (lock held briefly) |
| `volatile`              | Visibility & ordering only  | Low (no lock); doesn't fix compound ops like `i++` |
| `AtomicInteger` etc.    | Lock-free atomic ops        | Low; CAS-based      |
| `ReentrantLock`         | Advanced locking (tryLock, fairness) | Higher (manual unlock required) |

> **You'll see `volatile` + `synchronized` block working together in the Double-Checked Locking Singleton below — that's the pattern's whole point.**

---

## 15. Singleton Design Pattern

> **Category:** Creational
> **Intent:** Ensure a class has **exactly one instance** in the entire JVM and provide a global access point to it.

### 15.1 When to Use

- **Database connection pools / managers** — you don't want each thread opening its own `Connection` factory.
- **Configuration / properties** — a single read of `application.yml`.
- **Loggers** — one central log writer.
- **Caches** — one in-memory cache shared across the app.
- **Thread pools / Executors** — one shared `ExecutorService`.

> **Lecture quote:** *"Only one object of class exists across the entire system."*

### 15.2 The Three Building Blocks of a Singleton

```
┌───────────────────────────────────────────────────────────┐
│                      DBConnManager                         │
├───────────────────────────────────────────────────────────┤
│  ① private static volatile DBConnManager instance;         │ ← one slot
│  ② private DBConnManager() { ... }                         │ ← can't `new` from outside
│  ③ public static DBConnManager getInstance() { ... }       │ ← only way to get it
└───────────────────────────────────────────────────────────┘
```

### 15.3 V1 — The Naïve (NOT Thread-Safe) Singleton

```java
// ❌ Broken under multithreading
public class DBConnManager {
    private static DBConnManager instance;          // (1)

    private DBConnManager() {}                      // (2)

    public static DBConnManager getInstance() {     // (3)
        if (instance == null) {
            instance = new DBConnManager();
        }
        return instance;
    }
}
```

**Why it's broken:** Two threads can both pass the `if (instance == null)` check at the same time → both call `new` → **two instances exist**. Bug.

#### Why doesn't the JVM's class-init lock save us here?

You might think *"but the JVM puts a lock around static initialization — won't that protect this?"* Look carefully at where `new DBConnManager()` actually lives:

```java
private static DBConnManager instance;        // ← static field, but its initializer is just "set to null"
                                               //   This is what runs inside the JVM's class-init lock.

public static DBConnManager getInstance() {
    if (instance == null) {
        instance = new DBConnManager();         // ← THIS runs inside a regular method body —
    }                                            //   the JVM class-init lock is long gone by now.
    return instance;
}
```

When `DBConnManager` is loaded, the JVM class-init lock fires and runs **only one thing**: *"set field `instance` to its default value (`null`)"*. Then the lock is released. The actual `new DBConnManager()` runs **later, inside `getInstance()` — at runtime, in a regular method, with no JVM lock anywhere near it**. That's why two threads can race.

> **Rule:** *The JVM gives you a free thread-safety lock — but only for code that lives inside a static initializer expression (`static X = ...` or a `static { }` block). Code inside method bodies doesn't get that lock.*

### 15.3.5 Bonus — The Eager Singleton (the simplest thread-safe version)

If we just **move the `new DBConnManager()` into a static initializer**, the JVM's class-init lock now covers it — and we get a thread-safe singleton with **no `synchronized`, no `volatile`, no double-checks**. This is called the **eager singleton**.

```java
public class DBConnManager {
    // ✅ The 'new' is now INSIDE a static initializer expression
    private static final DBConnManager INSTANCE = new DBConnManager();

    private DBConnManager() {}

    public static DBConnManager getInstance() {
        return INSTANCE;
    }
}
```

**Why this is thread-safe:** the line `private static final DBConnManager INSTANCE = new DBConnManager();` is a static initializer. The JVM runs it inside its class-init lock the first time `DBConnManager` is loaded — exactly once, no matter how many threads race in.

**Why it's called "eager":** the `INSTANCE` is created the **moment the outer class is first loaded**, even if no one ever calls `getInstance()`. That's the trade-off vs lazy versions.

#### Eager vs Lazy — the *real* reason V3 (DCL) and V4 (Bill Pugh) exist

The eager singleton is already thread-safe. So why do we need V3 and V4?

> **The reason V3 and V4 exist isn't *thread-safety* — it's *laziness*.**

| | Eager singleton (this section) | V3 DCL / V4 Bill Pugh |
|---|---|---|
| When is the singleton constructed? | The moment the outer class is first loaded — **even if `getInstance()` is never called** | Only when `getInstance()` is **first called** |
| Cost paid up-front? | Yes — at class-load time | No — deferred until needed |
| Code complexity | Lowest (1 line, no lock keywords) | Medium (DCL) / Low (Bill Pugh) |
| Thread-safety mechanism | JVM class-init lock | JVM class-init lock (V4) / explicit `volatile` + `synchronized` (V3) |

So eager is "simpler but pays the cost up-front." Lazy versions defer that cost.

#### When does laziness actually matter?

Laziness is only worth the extra code when **construction is expensive AND the singleton might never be needed.**

| Scenario | Eager OK? | Why |
|---|---|---|
| Singleton is a small object (config holder, registry, formatter) | ✅ Yes | Construction is microseconds; eager is fine |
| Singleton is a `DBConnManager` that opens 50 connections | ❌ Prefer lazy | Don't open 50 connections at JVM startup if no one's calling it yet |
| Singleton is a heavy in-memory cache loaded from disk | ❌ Prefer lazy | Don't slow JVM startup unless someone needs it |
| Singleton is used on **every** code path anyway | ✅ Eager fine | Laziness gains you nothing |

#### How to choose — decision rule

| If you need... | Use |
|---|---|
| Simplest possible thread-safe singleton, eager init is OK | **Eager Singleton** (this §15.3.5) |
| Lazy + thread-safe + clean modern code | **V4 Bill Pugh** (§15.6) |
| Lazy + thread-safe — explaining DCL in interviews, or working in C++/older Java codebases | **V3 DCL** (§15.5) |
| Absolute serialization-safety + simplest lazy | **V5 Enum** (§15.7) |

#### So why does V3 (DCL) exist at all in modern Java?

Honestly, **mostly for historical and educational reasons**:
1. **Historical** — before Bill Pugh's pattern (§15.6) became popular (~2003), DCL was the standard lazy thread-safe singleton. Plenty of older Java codebases still use it.
2. **Educational** — DCL is what teaches you about *partial construction*, the *Java memory model*, and why `volatile` exists. Once you've seen DCL, you actually understand why these things matter.
3. **Cross-language** — C++ and other languages don't have an equivalent of Java's class-loading mechanism, so DCL (or its variants) is still the canonical lazy thread-safe pattern there.

In **modern Java code that you write today**: prefer eager singleton when laziness isn't needed, and Bill Pugh when it is. DCL is mostly an interview/legacy-codebase pattern.

#### One-line takeaway

> **"If you can afford eager init, use a static initializer — it's the simplest thread-safe singleton. Reach for Bill Pugh (V4) when you need laziness. V3 DCL is mostly an interview/historical pattern in modern Java."**

### 15.4 V2 — Synchronized Method (works, but slow)

```java
public class DBConnManager {
    private static DBConnManager instance;

    private DBConnManager() {}

    public static synchronized DBConnManager getInstance() {  // ← lock the whole method
        if (instance == null) {
            instance = new DBConnManager();
        }
        return instance;
    }
}
```

**Pros:** Thread-safe.
**Cons:** Every call to `getInstance()` acquires the lock — even after the singleton is initialised. *"Each request needs to wait."* In a service handling 10k req/sec, that's a serious bottleneck.

### 15.5 V3 — Double-Checked Locking (Thread-Safe + Performant) ⭐ Lecture Version

This is the version drawn on the board — **the textbook industrial Singleton**.

```java
public class DBConnManager {
    // ① private static volatile instance (volatile is critical!)
    private static volatile DBConnManager instance;

    // ② private constructor
    private DBConnManager() {}

    // ③ public method to return one instance
    public static DBConnManager getInstance() {
        if (instance == null) {                            // 1st check (no lock — fast path, 99% of calls)
            synchronized (DBConnManager.class) {            // lock only when instance might be null
                if (instance == null) {                    // 2nd check (inside lock — actual safety)
                    instance = new DBConnManager();        // create exactly once
                }
            }
        }
        return instance;
    }
}
```

#### Why two `null` checks? — the lecture's "double lock"

Imagine threads `T1` and `T2` both arrive when `instance == null`:

```
Step  T1                                T2
────  ─────────────────────────────     ─────────────────────────────
 1    if (instance == null)  ✓          if (instance == null)  ✓
 2    synchronized(...)  ← acquires      synchronized(...)  ← waits ⏸
 3    if (instance == null) ✓
 4    instance = new DBConnManager()
 5    exit synchronized → release lock   ──────► acquires lock
 6                                        if (instance == null)  ✗  ← saved!
 7                                        return existing instance
```

The **second** `null` check is what saves you. Without it, `T2` would `new` a second instance even though `T1` already made one.

#### Why `volatile`? — and what it does that `synchronized` does NOT

A common confusion: *"isn't `synchronized` enough? It already prevents two threads from creating two instances."*

It does — for the **inner** check. But there's a **second, completely separate problem** that `synchronized` alone does NOT fix, and that's what `volatile` is for.

##### Two different problems, two different fixes

| Problem | Fixed by |
|---|---|
| Two threads both creating an instance (race at the inner null check) | `synchronized` (the inner block serializes creation) |
| A reader thread on the **fast path** seeing a non-null but **half-constructed** object | `volatile` (no-reordering + visibility) |

So **both** are needed. They solve **different** bugs.

##### Why does a "half-constructed object" even exist?

`instance = new DBConnManager()` looks like one statement, but at the JVM/CPU level it's three sub-steps:

```
Step A:  allocate memory for a DBConnManager      (raw block, fields = defaults)
Step B:  run the constructor → initialize fields    (object now valid)
Step C:  publish the reference into 'instance'      (instance now points to it)
```

The JIT compiler and CPU are allowed to **reorder** these steps as long as the *single thread doing the work* can't tell the difference. The reordered execution **A → C → B** is legal:

```
Step A:  allocate memory                          ✅ object exists, fields = defaults
Step C:  publish reference into 'instance'        ⚠️ instance is NOW non-null, but...
Step B:  run constructor → fields initialized      ⏳ ...still running!
```

For a tiny window between Step C and Step B, **`instance` is non-null but the object is not fully built.**

##### Disaster timeline — Thread 2 hits the OUTER (lock-free) check in that window

```
Time  | Thread 1 (creating)               | Thread 2 (reading)
──────┼──────────────────────────────────┼──────────────────────────────────
 t1   | allocate memory (Step A)          |
 t2   | publish reference (Step C — REORDERED!)
 t3   |                                    | outer if (instance == null) → sees NON-null
 t4   |                                    | returns instance (fast path, no lock)
 t5   |                                    | calls instance.openConnection() 💥
 t6   | run constructor (Step B)          |
```

At `t5`, Thread 2 holds a reference to an object whose constructor has not finished. Its fields are still default values (null, 0, false). Calling any method can:

- Throw `NullPointerException` (fields not initialized yet)
- Use uninitialized config (default ports, null URLs, empty collections)
- Behave unpredictably depending on how the JIT scheduled the writes

This is the **"partially constructed object"** bug — also called **unsafe publication**.

##### What `volatile` guarantees

Marking the field `volatile` adds two things:

1. **No-reordering across the volatile write** — a *release barrier* before writing to `instance` forces Steps A and B to complete first. So `instance` only becomes non-null **after** the object is fully built.
2. **Visibility / no-stale-reads** — an *acquire barrier* on every read of `instance` means when Thread 2 sees a non-null `instance`, it is guaranteed to also see all of the constructor's writes (no caching, no register hoisting, no stale view).

In one sentence:

> **`volatile` makes "publish the reference" the LAST step in `new DBConnManager()`, and ensures every other thread that reads a non-null `instance` also sees a fully-constructed object.**

##### Pin this in memory

| Bug | Fixed by |
|---|---|
| Two threads both create instances | `synchronized` (specifically the inner null check) |
| A reader sees a non-null but half-built object | `volatile` (no reordering + visibility) |

That's why DCL needs **`volatile` + `synchronized`** — neither one alone is sufficient.

### 15.6 V4 — Bill Pugh / Initialization-on-Demand Holder (Modern Java Best Practice)

```java
public class DBConnManager {
    private DBConnManager() {}

    // The Holder class is loaded only when getInstance() is called
    private static class Holder {
        private static final DBConnManager INSTANCE = new DBConnManager();
    }

    public static DBConnManager getInstance() {
        return Holder.INSTANCE;
    }
}
```

**Beauty of this approach:**
- The JVM's **classloading guarantees** make initialisation thread-safe — no `synchronized`, no `volatile`.
- **Lazy** — `Holder` isn't loaded until someone calls `getInstance()`.
- **Fast** — no locking on subsequent calls.

This is the version most senior engineers prefer in Java today.

#### Why is this thread-safe? (the JVM does the locking *for you*)

A natural beginner question: *"if many threads call `getInstance()` at the same time, can the `Holder` class be loaded twice and create two instances?"*

**No.** The Java Language Specification (JLS §12.4.2) guarantees:

> **A class is initialized exactly ONCE per `ClassLoader`, no matter how many threads concurrently trigger its initialization.**

When the **first** thread accesses `Holder.INSTANCE`, the JVM does this **internally, on your behalf**:

1. Acquires an **internal lock on the `Class<Holder>` object**.
2. Runs the static initializer — `INSTANCE = new DBConnManager()`.
3. Marks `Holder` as **initialized**.
4. Releases the lock.

Any **other thread** arriving during step 2 doesn't run the initializer — it **blocks on the JVM's internal lock** until step 4. Then it just reads the already-initialized `INSTANCE`.

This lock lives **inside the JVM, not in your application code.** You don't write it, can't see it, can't accidentally remove it. Every `static final` initialization gets it for free.

##### Real-world analogy — one librarian, many readers

Imagine a brand-new book that just arrived at the library, still in shrink-wrap. The librarian's rule:
- The first reader who asks for it triggers the librarian to **unwrap it** (one-time setup).
- While the librarian is unwrapping, **all other readers wait at the desk**.
- Once it's unwrapped, every reader (including the first one) gets the same book.

The **JVM is the librarian**. The **static initializer is the unwrapping**. The **`Holder` class is the book**. No matter how many readers (threads) show up at the same moment, the unwrapping happens **once**.

##### What's a "static initializer"? — and where the lock actually lives

Before we go further, two things need to be crystal clear, because the term *"static initializer"* is doing a lot of heavy lifting in this discussion.

###### (1) A static initializer is just **code that runs exactly once when the class is first used**

Java has **two forms** of it:

```java
public class Holder {
    // FORM A — a static field with an initializer expression
    private static final DBConnManager INSTANCE = new DBConnManager();
                                              // ↑ "= new DBConnManager()" is the static initializer

    // FORM B — an explicit static block
    static {
        System.out.println("Holder being initialized");
    }
}
```

Both forms run during the same JVM-internal phase called **class initialization**, in source-code order. In Bill Pugh, **Form A** is what creates our singleton.

> **Mental model:** *"static initializer = the one-time setup code that runs the first time someone touches the class."*

###### (2) ClassLoader vs JVM — who actually does the locking

It's tempting to think *"the ClassLoader is what makes this thread-safe."* That's the part to **unlearn**. There are two distinct actors doing different jobs:

| Actor | Phase | What it does | Thread-safety role |
|---|---|---|---|
| **ClassLoader** | **Loading** | Finds the `.class` file on disk, reads its bytes into memory | Just loads each class once per ClassLoader; **does NOT lock for static initialization** |
| **JVM** | **Linking** | Verifies bytecode, gives static fields *default* values (0 / `null` / `false`), resolves references | No application-visible locking |
| **JVM** | **Initialization** | **Runs static initializers** (Forms A and B above) | **Locks on the `Class` object → this is where Bill Pugh's thread-safety comes from** |

So the precise statement is:

> **The JVM (not the ClassLoader) holds an internal lock on the `Class<Holder>` object during the *initialization* phase. That lock is what guarantees the static initializer `INSTANCE = new DBConnManager()` runs exactly once, even if 1,000 threads trigger it simultaneously.**

The ClassLoader only finds and reads the bytecode. It doesn't run your `new DBConnManager()` — that's a job for the initialization phase, which is the JVM's job, which is locked.

###### (3) Why is the lock *specifically* tied to static initializers?

Because **static state must exist exactly once, ever** — there's only one copy per class. If the JVM didn't serialize the initialization phase, here's what would happen with two threads:

```
Time | Thread A                                  | Thread B
─────┼───────────────────────────────────────────┼───────────────────────────────────────────
 t1  | reads Holder.INSTANCE                      | reads Holder.INSTANCE
 t2  | sees "Holder not initialized"              | sees "Holder not initialized"
 t3  | runs INSTANCE = new DBConnManager()       | runs INSTANCE = new DBConnManager()
                                                    ↑↑↑
                                            TWO instances created.
                                            Singleton broken.
```

That's exactly the bug the JVM's class-init lock prevents. The lock turns the dangerous race above into the **safe timeline** you see at the end of this section: the first thread does the one-time work, every other thread parks on the JVM lock until that work is done, then they all read the same `INSTANCE`.

> **One-line takeaway:** *Static initializers need a lock because they create one-per-class state, and the JVM provides that lock automatically during the initialization phase — so you, the programmer, never have to.*

##### Two phases people confuse

When people say *"class loading is thread-safe,"* they usually mean **class initialization**, which is a later phase. The JVM has three steps:

| Phase | What happens | Thread-safety |
|---|---|---|
| **Loading** | `.class` file found and bytecode read into memory | Once per `ClassLoader` |
| **Linking** | Bytecode is verified, prepared, resolved | Once per `ClassLoader` |
| **Initialization** | Static initializers run (including `static final INSTANCE = new ...`) | **Locked by the JVM** — runs exactly once |

For Bill Pugh, the phase that matters is **initialization** — and that's the one the JVM puts a lock around.

##### Concurrent timeline — 3 threads racing into `getInstance()` simultaneously

```
Time | Thread A                       | Thread B                  | Thread C
─────┼────────────────────────────────┼──────────────────────────┼─────────────────────────
 t1  | calls getInstance()             |                          |
 t2  | reads Holder.INSTANCE           |                          |
 t3  | JVM: Holder not initialized     |                          |
 t4  | JVM acquires Holder.class lock  |                          |
 t5  | runs new DBConnManager()...     | calls getInstance()       | calls getInstance()
 t6  |   (constructing...)              | reads Holder.INSTANCE     | reads Holder.INSTANCE
 t7  |   (constructing...)              | JVM: lock held → BLOCK    | JVM: lock held → BLOCK
 t8  | INSTANCE assigned                |                          |
 t9  | JVM marks Holder initialized    |                          |
 t10 | JVM releases Holder.class lock  |                          |
 t11 | returns INSTANCE                 | unblocked → reads INSTANCE| unblocked → reads INSTANCE
 t12 |                                  | returns INSTANCE          | returns INSTANCE
```

- **A** is the first thread to touch `Holder` — it triggers initialization.
- **B and C** arrive while A is mid-initialization. The JVM **automatically blocks them** on its internal class-init lock.
- When A finishes, B and C are released. They just **read `INSTANCE`** — no constructing, no `new`.

So `new DBConnManager()` runs **exactly once**, even with 1,000 threads racing in at the same moment.

##### Why this is *better* than Double-Checked Locking (V3)

| | DCL (V3) | Bill Pugh Holder (V4) |
|---|---|---|
| Lazy initialization | ✅ | ✅ (`Holder` loads on first `getInstance()`) |
| Thread-safe | ✅ (needs `volatile` + `synchronized`) | ✅ (JVM-guaranteed; no app-level locks) |
| App-level lock on each call | Brief lock check | **Zero** — pure field read |
| `volatile` needed? | Yes | No |
| Code complexity | Medium | Low |

Once `Holder` is initialized, every `getInstance()` call collapses to `return Holder.INSTANCE;` — a **single field read**. The fastest possible thread-safe lazy singleton in Java.

##### One-line summary

> **"Bill Pugh is thread-safe because the JVM puts a lock around class initialization for you — so you don't have to write `volatile` or `synchronized` yourself."**

### 15.7 V5 — Enum Singleton (Joshua Bloch's recommendation, *Effective Java*)

```java
public enum DBConnManager {
    INSTANCE;

    public void connect() { /* ... */ }
}

// Usage:
DBConnManager.INSTANCE.connect();
```

- Thread-safe **for free** (the JVM guarantees enum constants are constructed once).
- **Serialization-safe** (other Singleton implementations can be broken by deserialization creating a 2nd instance).
- **Reflection-safe** (you can't `setAccessible(true)` your way to a 2nd enum instance).

**Caveat:** Enums can't extend a class (only implement interfaces), so this isn't always usable.

### 15.8 Caller Code (proves "one instance")

```java
public class Main {
    public static void main(String[] args) {
        // Two different threads / call sites
        DBConnManager db1 = DBConnManager.getInstance();
        DBConnManager db2 = DBConnManager.getInstance();

        if (db1 == db2) {
            System.out.println("Same instance ✓");   // always true
        }
    }
}
```

### 15.9 Real-World Industry Examples

1. **`java.lang.Runtime`** — `Runtime.getRuntime()` returns the JVM's only Runtime. Pure Singleton.
2. **Spring beans (default scope)** — every `@Service`, `@Repository`, `@Component` is a singleton **inside the Spring container** (one instance per `ApplicationContext`).
3. **`Logger` instances (SLF4J / Log4j)** — typically one per class, shared across threads.
4. **JDBC `DriverManager`** — global registry of JDBC drivers; effectively a singleton.
5. **Connection pools (HikariCP, c3p0)** — one pool per data source; the pool object itself is a singleton.

### 15.10 Anti-Patterns & Pitfalls

- **Hidden global state.** Singletons are essentially globals. They make unit testing harder (can't mock easily). Prefer DI-managed singletons (Spring) over hand-rolled ones.
  - *In simple words:* If a class calls `getInstance()` inside itself, it's hard-wired to the real object — your test can't sneak in a fake. With Spring's `@Service` (DI), the dependency is **passed in** via the constructor, so production gets the real one and tests can pass a fake one.

- **God objects.** A `Singleton` that grows to 50 methods becomes a code smell.
  - *In simple words:* Because a Singleton is easy to grab from anywhere, devs keep dumping unrelated stuff into it (DB + email + tax + UI in one class). It violates Single Responsibility — keep each class focused on one job.

- **Multiple classloaders / OSGi / containers.** "One per JVM" can become "one per classloader" — surprising in WAR/EAR deployments.
  - *In simple words:* A classloader loads `.class` files into memory. Big enterprise servers (Tomcat, JBoss, OSGi) can run **multiple classloaders** side-by-side, each loading its own copy of your Singleton class — so you actually end up with several "singletons." Surprise!

- **Reflection attack.** Anyone with `setAccessible(true)` can call your private constructor — use enum to guard against this.
  - *In simple words:* Java reflection can bypass `private` and call your constructor anyway, creating a 2nd instance. `enum` Singletons are JVM-protected — even reflection can't create a duplicate.

- **Serialization breaks Singleton.** You need to override `readResolve()` to return the existing instance.
  - *In simple words:* Saving an object to disk/network (serialization) and reading it back creates a **brand new** object — so now you have two. Override `readResolve()` to say "return my existing singleton instead," or just use `enum` (it handles this for free).

### 15.11 Singleton — Interview Cheat Sheet

| Q | A |
|---|---|
| Is the basic `if (instance == null) { instance = new ...; }` thread-safe? | **No.** Use synchronized + double-check, or Bill Pugh, or enum. |
| Why is `volatile` needed in DCL? | Prevents reordering during object construction; ensures visibility across threads. |
| Why two null checks in DCL? | First check (no lock) for fast path; second check (inside lock) for correctness. |
| Best modern Java Singleton? | Bill Pugh (initialization-on-demand holder) or **enum** for absolute safety. |
| When NOT to use Singleton? | When you need testability and isolation — prefer Dependency Injection from a container. |

---

## Java Aside — Lambdas, Functional Interfaces & Method References

> **Why this aside?** Modern design patterns rely heavily on passing **behavior** (not just data) — strategies, callbacks, factory recipes. Java 8 added three intertwined features for this: **lambdas**, **functional interfaces**, and **method references**. Learn them once; they appear in every pattern from here on.

### A.1 The Big Picture

Before Java 8, passing a piece of behavior meant writing a whole anonymous class:

```java
// Old way — 5 lines for 1 line of real logic
button.addClickListener(new ClickListener() {
    @Override
    public void onClick() {
        System.out.println("clicked");
    }
});
```

Java 8 fixed this with **lambdas**:

```java
button.addClickListener(() -> System.out.println("clicked"));   // 1 line
```

And **method references** are an even shorter form when the lambda just calls an existing method:

```java
button.addClickListener(System.out::println);   // shortest
```

The progression: anonymous class → lambda → method reference. Same behavior, less ceremony.

### A.2 Lambda Recap

A **lambda** is a **short anonymous function**. Syntax:

```java
(parameters) -> body
```

Examples:

```java
() -> 42                          // no params, returns 42
x -> x * 2                        // one param, returns x*2
(x, y) -> x + y                   // two params, returns sum
(String s) -> s.length()          // explicit param type
s -> { return s.length(); }       // explicit return + braces (multi-statement)
```

Syntax rules:
- One param → parens optional: `x -> ...` or `(x) -> ...`
- Multiple params → parens required: `(x, y) -> ...`
- Multiple statements → braces + explicit `return`: `x -> { ... ; return r; }`

But where do lambdas "live"? You can't write a lambda floating on its own. It must fit a **functional interface** type — that's how Java knows what method the lambda represents.

### A.3 Functional Interfaces — the "shape" lambdas plug into

A **functional interface** is an interface with **exactly one abstract method**. Think of it as a "shape" with one empty slot.

A lambda is just a quick way to fill that slot.

```java
@FunctionalInterface
interface Greeter {
    String greet(String name);   // ONE abstract method
}

// Fill the slot with a lambda:
Greeter g = name -> "Hello, " + name;
String result = g.greet("Zhunzar");   // "Hello, Zhunzar"
```

The `@FunctionalInterface` annotation is optional but useful — the compiler errors if you accidentally add a second abstract method.

#### The 4 built-in functional interfaces (memorize these)

Java's `java.util.function` package gives you ready-made functional interfaces. These four cover ~80% of real-world cases:

| Interface | Abstract method | Takes | Returns | Mnemonic |
|---|---|---|---|---|
| `Supplier<T>` | `T get()` | nothing | `T` | "**supplies** a value" |
| `Consumer<T>` | `void accept(T)` | `T` | nothing | "**consumes** a value" |
| `Function<T, R>` | `R apply(T)` | `T` | `R` | "**transforms** T into R" |
| `Predicate<T>` | `boolean test(T)` | `T` | `boolean` | "**tests**, returns yes/no" |

Examples of each:

```java
Supplier<String> s        = () -> "Hello";
String hello              = s.get();                     // "Hello"

Consumer<String> c        = msg -> System.out.println(msg);
c.accept("hi");                                          // prints "hi"

Function<String, Integer> f = str -> str.length();
int len                   = f.apply("abc");              // 3

Predicate<Integer> p      = n -> n > 0;
boolean isPositive        = p.test(5);                   // true
```

### A.4 Method References — even shorter than lambdas

When your lambda **only calls an existing method**, you can replace it with a **method reference** using `::`.

```java
// These are EQUIVALENT:
Consumer<String> c1 = x -> System.out.println(x);   // lambda
Consumer<String> c2 = System.out::println;          // method reference (shorter)
```

Both produce the same `Consumer<String>`.

#### The 4 forms of method references

| # | Form | Syntax | Example | Equivalent lambda |
|---|---|---|---|---|
| 1 | **Static method** | `ClassName::staticMethod` | `Integer::parseInt` | `s -> Integer.parseInt(s)` |
| 2 | **Instance method of a SPECIFIC object** | `instance::method` | `System.out::println` | `x -> System.out.println(x)` |
| 3 | **Instance method of an ARBITRARY object** | `ClassName::instanceMethod` | `String::length` | `s -> s.length()` |
| 4 | **Constructor** | `ClassName::new` | `UPIPayment::new` | `() -> new UPIPayment()` |

#### Form 2 vs Form 3 — the confusing one

This is where most people stumble. The distinction:

**Form 2 (specific instance):** You already have an object; the reference is **bound** to it.
```java
String greeting = "Hello";
Supplier<Integer> len = greeting::length;   // bound to THIS specific "Hello"
len.get();   // 5  (always returns length of "Hello")
len.get();   // 5  (same string, same answer)
```

**Form 3 (arbitrary instance):** No specific object — the method will be called on whatever you pass in.
```java
Function<String, Integer> len = String::length;   // unbound — works on ANY String
len.apply("Hello");  // 5
len.apply("Hi");     // 2
len.apply("Java");   // 4
```

Key insight for Form 3: **the input parameter becomes the receiver.** `String::length` is read as "take a string `s` and call `s.length()` on it" — `s` is both the input AND the object the method runs on.

### A.5 Master Rule — Picking the Right Functional Interface

> **The functional interface is decided by the SHAPE of what's left to plug in:**
> 1. **How many inputs** does the resulting function still need?
> 2. **What does it return** (a value, void, or boolean)?

Once you internalize this question, you'll never get stuck again. Apply it in two steps.

#### Step 1 — Count the inputs you still need to provide

This depends on the form of the method reference:

| Form | Inputs already given | Inputs still needed |
|---|---|---|
| `Class::staticMethod` (Form 1) | None | Whatever the static method takes |
| `obj::method` (Form 2) | The object is already bound | Whatever the instance method takes |
| `Class::method` (Form 3) | None | The object **+** whatever the instance method takes |
| `Class::new` (Form 4) | None | Whatever the constructor takes |

Concrete examples — focus on the "still needed" column:

```java
Integer::parseInt    →  needs 1 input (a String)        →  returns Integer
System.out::println  →  needs 1 input (anything)        →  returns void
String::length       →  needs 1 input (a String)        →  returns int
ArrayList::new       →  needs 0 inputs                  →  returns ArrayList
list::add            →  needs 1 input (the element)     →  returns boolean (often ignored)
Person::getName      →  needs 1 input (a Person)        →  returns String
Math::max            →  needs 2 inputs (a, b)           →  returns int
```

#### Step 2 — Match `(#inputs, return type)` to a functional interface

This is the **lookup table** to memorize:

| #Inputs | Returns | Functional Interface | Method |
|---|---|---|---|
| 0 | a value | `Supplier<R>` | `R get()` |
| 0 | nothing (void) | `Runnable` | `void run()` |
| 1 | a value | `Function<T, R>` | `R apply(T)` |
| 1 | nothing (void) | `Consumer<T>` | `void accept(T)` |
| 1 | boolean | `Predicate<T>` | `boolean test(T)` |
| 2 | a value | `BiFunction<T, U, R>` | `R apply(T, U)` |
| 2 | nothing (void) | `BiConsumer<T, U>` | `void accept(T, U)` |
| 2 | boolean | `BiPredicate<T, U>` | `boolean test(T, U)` |
| 1 (T → T, same type) | same type | `UnaryOperator<T>` | `T apply(T)` |
| 2 (T,T → T, same type) | same type | `BinaryOperator<T>` | `T apply(T, T)` |

> Note: `UnaryOperator<T>` is just `Function<T, T>` and `BinaryOperator<T>` is just `BiFunction<T, T, T>` — convenience subtypes when input and output types match.

#### Decision flow you can use forever

When you see ANY method reference, ask:

```
1. What method will get called?
2. How many arguments do I (the caller) still need to provide?
   - Form 1 (static):        same as static method's params
   - Form 2 (obj::method):   same as instance method's params (object is bound)
   - Form 3 (Class::method): instance method's params + 1 (the object itself)
   - Form 4 (Class::new):    same as constructor's params
3. What's returned?
   - void          → Consumer / BiConsumer / Runnable
   - boolean       → Predicate / BiPredicate
   - any other val → Supplier / Function / BiFunction
4. Look up the table → that's your functional interface.
```

#### Worked examples — verify the rule

| Method reference | Inputs needed | Returns | Functional Interface |
|---|---|---|---|
| `Person::getName` | 1 (Person) | String | `Function<Person, String>` |
| `alice::getName` | 0 (alice bound) | String | `Supplier<String>` |
| `list::add` | 1 (element) | void* | `Consumer<String>` |
| `Math::sqrt` | 1 (double) | double | `Function<Double, Double>` or `UnaryOperator<Double>` |
| `String::isEmpty` | 1 (String) | boolean | `Predicate<String>` |
| `LocalDate::now` | 0 | LocalDate | `Supplier<LocalDate>` |
| `String::concat` | 2 (Strings) | String | `BiFunction<String,String,String>` or `BinaryOperator<String>` |
| `HashMap::new` | 0 | HashMap | `Supplier<HashMap<K,V>>` |
| `myList::contains` | 1 (Object) | boolean | `Predicate<Object>` |
| `Math::max` | 2 (int, int) | int | `BinaryOperator<Integer>` |

\* `list::add` actually returns `boolean`, but in `Consumer` context Java accepts ignoring the return value.

#### One-sentence summary

> **Look at what the method needs and what it returns AFTER accounting for what's already bound; match that "(inputs, return)" shape to the lookup table.**

### A.6 Quick Exercises

Try these mentally before peeking at the answer.

**Exercise 1:** Convert this lambda to a method reference.
```java
Function<String, String> f = s -> s.toUpperCase();
```

> **Answer:** `Function<String, String> f = String::toUpperCase;` — Form 3 (arbitrary instance method).

**Exercise 2:** What does this print?
```java
Supplier<List<String>> recipe = ArrayList::new;
List<String> a = recipe.get();
List<String> b = recipe.get();
System.out.println(a == b);
```

> **Answer:** `false` — `recipe.get()` calls `new ArrayList<>()` each time, creating **two distinct lists**. Form 4 (constructor reference) is a *recipe*, not a stored object.

**Exercise 3:** Convert to a lambda:
```java
Predicate<String> p = String::isEmpty;
```

> **Answer:** `Predicate<String> p = s -> s.isEmpty();` — Form 3, returning boolean.

**Exercise 4:** Pick the right functional interface:
> "I want a thing I can call to give me the current timestamp, taking no arguments."

> **Answer:**
> ```java
> Supplier<Long> clock = System::currentTimeMillis;
> long now = clock.get();
> ```
> Takes nothing, returns a value → `Supplier`. The method reference is Form 1 (static method).

**Exercise 5:** Which form is each of these?
```java
a)  Math::max
b)  list::add
c)  Person::getName
d)  HashMap::new
```

> **Answers:**
> a) Form 1 — static method on `Math`
> b) Form 2 — instance method bound to specific `list` object
> c) Form 3 — arbitrary `Person` instance method
> d) Form 4 — constructor reference

### A.7 Where Method References Show Up in Design Patterns

This is the payoff — these forms appear in nearly every pattern:

```java
// FACTORY PATTERN — registry of constructor references (Form 4)
Map<String, Supplier<Payment>> registry = Map.of(
    "CC",  CreditCardPayment::new,
    "UPI", UPIPayment::new
);
Payment p = registry.get("CC").get();   // builds a new CreditCardPayment

// STRATEGY PATTERN — pass behavior as a function (Form 1)
calculator.compute(10, 20, Math::max);      // strategy: take the max
calculator.compute(10, 20, Math::min);      // strategy: take the min
calculator.compute(10, 20, Integer::sum);   // strategy: sum

// OBSERVER PATTERN — register callbacks (Form 2)
eventBus.subscribe("login", logger::logEvent);
eventBus.subscribe("login", auditService::record);

// COMMAND PATTERN — undo/redo stack of references (Form 2)
undoStack.push(canvas::undo);

// COLLECTION OPS — filter/map/forEach (mix of forms)
names.stream()
     .filter(Objects::nonNull)        // Form 1: static method
     .map(String::toUpperCase)        // Form 3: arbitrary instance
     .forEach(System.out::println);   // Form 2: specific instance
```

### A.8 Mental Cheat Sheet

When you SEE this in code, read it as:

| What you see | Read it as |
|---|---|
| `Foo::new` | "a recipe to **make** a Foo" |
| `Foo::staticMethod` | "a function that calls **Foo.staticMethod(...)**" |
| `obj::method` | "a function that calls **method()** on this **specific** obj" |
| `Foo::instanceMethod` | "a function: input is a **Foo**, call **method()** on it" |

When you WRITE your own:
1. Start with the lambda: `x -> doSomething(x)`.
2. If the lambda body is **just one method call**, try replacing with a method reference.
3. The compiler will tell you if it doesn't fit the target functional interface.

### A.9 Common Pitfalls

- **Form 2 captures the reference.** `obj::method` remembers `obj` *at that moment*. If `obj` is reassigned later, the method reference still uses the old object.
- **A method reference is NOT a method call.** `Math::max` is a *value* (a `BinaryOperator<Integer>`); `Math.max(1, 2)` is a *call* (returns `2`). Don't add parentheses!
- **Functional interfaces only work if the shape matches.** A lambda that takes 2 args can't fit a `Supplier`. The compiler enforces this.
- **`var` doesn't work directly with lambdas.** `var x = () -> 42;` is illegal — Java needs the target type. Use the explicit interface: `Supplier<Integer> x = () -> 42;`.

---

## 16. Factory Design Pattern

> **Category:** Creational
> **Intent:** Provide a method for creating objects **without exposing the instantiation logic** to the client. The client asks the factory for *what* it wants; the factory decides *which concrete class* to return.

### 16.1 Naïve Definition

> *"Without specifying the class name, it will create the object."* — lecture

The client should not say `new CreditCardPayment()`. The client says *"give me a payment processor of type 'UPI'"* and the factory hands back the right object.

### 16.2 Why? (Purpose & Real Need)

- **Decouples creation from usage.** The client doesn't know (or care) about concrete classes.
- **Centralizes the `new` calls.** Adding a new type → change the factory only, not 50 callers.
- **Hides complex construction.** Maybe the object needs config lookup, validation, caching — the factory handles it.
- **Enables polymorphism naturally.** Clients code against the interface; the factory returns whichever implementation fits.

### 16.3 The Lecture Example — Payment Processing System

A payments system supports **Credit Card**, **UPI**, **Net Banking**.

```
                ┌─────────────────────┐
                │   <<interface>>     │
                │      Payment        │
                │   + process()       │
                └──────────△──────────┘
                           │
        ┌──────────────────┼─────────────────────┐
        │                  │                     │
┌───────┴──────┐   ┌──────┴───────┐    ┌───────┴────────┐
│ CreditCard   │   │   UPIPayment  │    │ NetBanking     │
│  Payment     │   │               │    │   Payment      │
│ +process()   │   │  +process()   │    │  +process()    │
└──────────────┘   └───────────────┘    └────────────────┘
                           ▲
                           │ creates
                ┌──────────┴───────────┐
                │   PaymentFactory     │
                │ + getPayType(String) │
                └──────────────────────┘
```

#### Step 1 — Common interface

```java
interface Payment {
    void process();
}
```

#### Step 2 — Concrete products

```java
class CreditCardPayment implements Payment {
    @Override
    public void process() {
        System.out.println("Processing CC Payment");
    }
}

class UPIPayment implements Payment {
    @Override
    public void process() {
        System.out.println("Processing UPI Payment");
    }
}

class NetBankingPayment implements Payment {
    @Override
    public void process() {
        System.out.println("Processing Net Banking Payment");
    }
}
```

#### Step 3 — The Factory

```java
class PaymentFactory {
    public static Payment getPayType(String type) {
        switch (type) {
            case "CC":
                return new CreditCardPayment();
            case "UPI":
                return new UPIPayment();
            case "NB":
                return new NetBankingPayment();
            default:
                throw new IllegalArgumentException("Unknown type: " + type);
        }
    }
}
```

#### Step 4 — The Caller

```java
public class Main {
    public static void main(String[] args) {
        Payment payment = PaymentFactory.getPayType("UPI");
        payment.process();   // → "Processing UPI Payment"
    }
}
```

The caller **never** writes `new UPIPayment()`. To add a 4th payment method (`PayPal`), you (a) create a new `PayPalPayment` class and (b) edit **only** `PaymentFactory` — every existing caller keeps working unchanged.

> **OCP nuance (important):** Strictly speaking, the *factory itself* is **NOT** OCP-compliant — its `switch` must be modified for every new type. But the **callers are**. The pattern's value is to **localize the violation in one file** instead of letting it scatter across every caller in the codebase.

#### Why "every caller" matters — the scattered if/else problem

Without a factory, every place that creates a `Payment` repeats the same `if/else`:

```java
// File 1: CheckoutController.java
public void checkout(String type) {
    Payment p;
    if (type.equals("CC"))       p = new CreditCardPayment();
    else if (type.equals("UPI")) p = new UPIPayment();
    else if (type.equals("NB"))  p = new NetBankingPayment();
    p.process();
}

// File 2: RefundService.java  — same if/else copy-pasted
public void refund(String type) {
    Payment p;
    if (type.equals("CC"))       p = new CreditCardPayment();
    else if (type.equals("UPI")) p = new UPIPayment();
    else if (type.equals("NB"))  p = new NetBankingPayment();
    p.refund();
}

// File 3: RecurringBillingJob.java  — same again ...
```

Adding `PayPal` now means hunting down **every file** with this pattern and editing each one (you WILL miss one). The factory replaces all of those with:

```java
Payment p = PaymentFactory.getPayType(type);   // one line, every caller
```

→ Adding `PayPal` now requires editing **only the factory**, not every caller. That's the trade-off win.

#### Variants of Factory by OCP-strictness

| Variant | OCP-strict? | Cost | When to use |
|---|---|---|---|
| `switch`/if-else factory (lecture example) | ❌ Factory itself violates | Low complexity | Default — small/medium apps |
| Registry/Map factory (self-registering) | ✅ Yes | Medium | Plugin systems, dynamic types |
| Spring DI (auto-discover) | ✅ Yes | High (needs framework) | Enterprise apps |

#### Variant A — Registry-based Factory (true OCP, no framework needed)

```java
class PaymentFactory {
    // "type" → "how to build that type"
    private static final Map<String, Supplier<Payment>> registry = new HashMap<>();

    static {
        registry.put("CC",  CreditCardPayment::new);   // constructor reference
        registry.put("UPI", UPIPayment::new);
        registry.put("NB",  NetBankingPayment::new);
    }

    public static void register(String type, Supplier<Payment> creator) {
        registry.put(type, creator);
    }

    public static Payment getPayType(String type) {
        Supplier<Payment> creator = registry.get(type);
        if (creator == null) throw new IllegalArgumentException("Unknown: " + type);
        return creator.get();   // builds the object NOW
    }
}

// Adding PayPal — factory is NEVER edited:
public class PayPalPayment implements Payment {
    public void process() { /* ... */ }

    static {   // self-registration on class load
        PaymentFactory.register("PAYPAL", PayPalPayment::new);
    }
}
```

#### Variant B — Spring DI Factory (true OCP, framework-managed)

Spring auto-discovers every `Payment` implementation at startup, so the "factory" is the Spring container itself.

```java
// Tag each implementation with a bean name:
@Component("CC")
public class CreditCardPayment implements Payment { /* ... */ }

@Component("UPI")
public class UPIPayment implements Payment { /* ... */ }

@Component("NB")
public class NetBankingPayment implements Payment { /* ... */ }

// Spring auto-fills the Map with every Payment implementation it finds
@Service
public class PaymentService {
    private final Map<String, Payment> paymentMap;

    public PaymentService(Map<String, Payment> paymentMap) {
        this.paymentMap = paymentMap;   // { "CC": ..., "UPI": ..., "NB": ... }
    }

    public void process(String type) {
        Payment p = paymentMap.get(type);
        if (p == null) throw new IllegalArgumentException("Unknown: " + type);
        p.process();
    }
}

// Adding PayPal — ZERO edits to existing code:
@Component("PAYPAL")
public class PayPalPayment implements Payment {
    public void process() { /* ... */ }
}
```

> **Why Spring is fully OCP-compliant:** No existing file is modified. Spring scans the classpath, finds every `@Component` that implements `Payment`, and rebuilds the map automatically. Adding a new type = adding one new file.

#### Quick note: what is `ClassName::new`?

This is a **constructor reference** (Java 8+). It's shorthand for a lambda that creates an object:

```java
Supplier<UPIPayment> a = () -> new UPIPayment();   // lambda
Supplier<UPIPayment> b = UPIPayment::new;          // identical, shorter
```

Both give you a "recipe" for building a `UPIPayment`. Calling `.get()` on it actually creates one:

```java
Supplier<Payment> recipe = UPIPayment::new;   // doesn't create yet — just stores HOW
Payment p1 = recipe.get();   // NOW creates: new UPIPayment()
Payment p2 = recipe.get();   // creates ANOTHER one
```

Common method-reference forms:

| Form | Equivalent lambda |
|---|---|
| `ClassName::new` | `() -> new ClassName()` |
| `String::length` | `s -> s.length()` |
| `System.out::println` | `x -> System.out.println(x)` |
| `Math::max` | `(a, b) -> Math.max(a, b)` |

That's why the registry factory uses `UPIPayment::new` — a clean way to hand the factory a "recipe" instead of a fully-built object.

### 16.4 Real-World Industry Examples

1. **`java.util.Calendar.getInstance()`** — returns a `GregorianCalendar`, `BuddhistCalendar`, or `JapaneseImperialCalendar` based on the locale. Classic factory.
2. **`java.text.NumberFormat.getInstance(Locale)`** — gives you the right formatter for the locale.
3. **Logger frameworks** — `LoggerFactory.getLogger(MyClass.class)`.
4. **JDBC `DriverManager.getConnection(url)`** — parses the URL and returns the driver-specific `Connection`.
5. **Spring's `BeanFactory`** — returns beans by type/name.
6. **Notification systems** — `NotificationFactory.get("EMAIL" | "SMS" | "PUSH")`.

### 16.5 Factory — Interview Cheat Sheet

| Q | A |
|---|---|
| What problem does Factory solve? | Removes `new` from client code; client depends on interface, not implementation. |
| Difference vs. Abstract Factory? | Factory creates **one** kind of object; Abstract Factory creates **families** of related objects. |
| Where in the JDK? | `Calendar.getInstance()`, `NumberFormat.getInstance()`, `LoggerFactory.getLogger()`. |
| OCP relation? | Adding a new product → modify only the factory; client code is closed for modification. |

---

## 17. Abstract Factory Design Pattern

> **Category:** Creational
>
> **Friendly definition:** A regular Factory hands you **one thing**. An Abstract Factory hands you a **whole matching set** — pick the family first, and you automatically get every piece from that family. You can't accidentally mix pieces from different families.
>
> **Formal definition (for reference):** Provide an interface for creating **families of related or dependent objects** without specifying their concrete classes.

### 17.1 The Problem It Solves — From Scratch

#### First, build the intuition with a real-world example

Imagine you walk into a furniture store to buy a **chair, a sofa, and a table**. The store doesn't sell random mixed pieces — it organizes them into two **collections**:

```
  ┌─────────── MODERN COLLECTION ───────────┐    ┌──── VICTORIAN COLLECTION ────┐
  │  Modern Chair  +  Modern Sofa  +  Modern Table │  │  Vict. Chair + Vict. Sofa + Vict. Table │
  └────────────────── all match ────────────────┘    └────────── all match ──────────┘
                                                                             
  ❌ Modern Chair  with  Victorian Sofa   →   looks ridiculous
```

**Rules of the store:**
1. Pick **one collection** at a time (Modern OR Victorian).
2. Once you pick, you get the **matching chair + sofa + table** from that collection.
3. **No mixing across collections** — that's forbidden by design.

The "**collection**" here is the **family**. You never pick individual pieces directly — you pick a family, and the family gives you matching pieces.

That's Abstract Factory. The "abstract factory" is the **furniture store** itself; "Modern" and "Victorian" are **concrete factories**; chair/sofa/table are **products**.

#### Now translate to code — the cloud example

You're building an app that needs **two cloud services** working together:
- **Storage** (for saving files)
- **Database** (for saving data)

Two cloud providers offer both:
- **AWS:** AWS Storage + AWS Database
- **Azure:** Azure Storage + Azure Database

**Rule (just like the furniture store):**
- Once you pick a provider, you MUST use **its** storage AND **its** database together.
- You **cannot** mix AWS Storage with Azure Database — they speak different SDKs, different auth, different APIs. The app would break.

```
   ┌──── AWS FAMILY ─────┐         ┌──── AZURE FAMILY ────┐
   │ AWS Storage          │         │ Azure Storage         │
   │       +              │         │       +               │
   │ AWS Database         │         │ Azure Database        │
   └──────────────────────┘         └───────────────────────┘
                ❌ Don't pair AWS Storage with Azure Database
```

#### Why a regular Factory can't solve this

A regular factory has one method — "give me a product of type X":

```java
Storage  s = factory.create("AWS_STORAGE");
Database d = factory.create("AZURE_DB");      // ❌ Oops! Mixed providers!
```

Nothing stops you from making this mistake — the factory doesn't know that storage and database must come from the same family.

#### How Abstract Factory fixes it

**The trick: make the FACTORY itself the choice.** Instead of one factory that creates everything, you have **one factory per family**. Each family-factory only knows how to make its own family's products.

```java
CloudFactory factory = new AWSFactory();   // Pick the AWS family ONCE
Storage  s = factory.createStorage();      // → AWS Storage
Database d = factory.createDatabase();     // → AWS Database (guaranteed match!)
```

To switch to Azure, you change **one line**:

```java
CloudFactory factory = new AzureFactory();   // ← only this line changes
Storage  s = factory.createStorage();        // → Azure Storage
Database d = factory.createDatabase();       // → Azure Database (still consistent)
```

The rest of your code (`createStorage()`, `createDatabase()`) doesn't change at all.

#### One-line summary

> **Regular Factory:** "Give me a Payment of type UPI." (one product)
> **Abstract Factory:** "Give me the AWS factory — now hand me Storage and Database from your family." (a matching set, no mixing possible)

### 17.2 Structure

```
                    ┌───────────────────────────┐
                    │   <<interface>>           │
                    │   CloudFactory            │ ← Abstract Factory
                    │  +createStorage()         │
                    │  +createDatabase()        │
                    └────────────△──────────────┘
                                 │
                ┌────────────────┴────────────────┐
                │                                 │
       ┌────────┴────────┐                ┌──────┴──────────┐
       │   AWSFactory     │                │   AzureFactory  │ ← Concrete factories
       │ +createStorage() │                │ +createStorage()│
       │ +createDatabase()│                │+createDatabase()│
       └────────┬─────────┘                └────────┬────────┘
                │ produces                          │ produces
       ┌────────┴────────────┐         ┌───────────┴──────────────┐
       │ AWSStorage, AWSDB    │        │ AzureStorage, AzureDB    │
       └─────────────────────┘         └──────────────────────────┘

       (Abstract products: Storage, Database — interfaces)
```

### 17.3 The Lecture Code (cleaned up to compile)

#### Step 1 — Abstract products

```java
// Abstract Product 1
interface Storage {
    void upload(String file);
}

// Abstract Product 2
interface Database {
    void connect();
}
```

#### Step 2 — Concrete products (AWS family)

```java
class AWSStorage implements Storage {
    @Override
    public void upload(String file) {
        System.out.println("Uploading to AWS S3: " + file);
        // AWSSDK.connect(file);
    }
}

class AWSDB implements Database {
    @Override
    public void connect() {
        System.out.println("Connecting to AWS RDS");
    }
}
```

#### Step 3 — Concrete products (Azure family)

```java
class AzureStorage implements Storage {
    @Override
    public void upload(String file) {
        System.out.println("Uploading to Azure Blob: " + file);
    }
}

class AzureDB implements Database {
    @Override
    public void connect() {
        System.out.println("Connecting to Azure SQL");
    }
}
```

#### Step 4 — Abstract factory + concrete factories

```java
// Abstract Factory
interface CloudFactory {
    Storage  createStorage();
    Database createDatabase();
}

class AWSFactory implements CloudFactory {
    @Override public Storage  createStorage()  { return new AWSStorage(); }
    @Override public Database createDatabase() { return new AWSDB(); }
}

class AzureFactory implements CloudFactory {
    @Override public Storage  createStorage()  { return new AzureStorage(); }
    @Override public Database createDatabase() { return new AzureDB(); }
}
```

#### Step 5 — High-level client

```java
class Application {
    private Storage  storage;
    private Database database;

    public Application(CloudFactory factory) {       // ← takes the FACTORY, not products
        this.storage  = factory.createStorage();
        this.database = factory.createDatabase();
    }

    public void run() {
        database.connect();
        storage.upload("file.txt");
    }
}
```

#### Step 6 — Composition root (`main`)

```java
public class Main {
    public static void main(String[] args) {
        CloudFactory factory;
        String provider = "AWS";       // could come from config / env

        if (provider.equals("AWS")) {
            factory = new AWSFactory();
        } else {
            factory = new AzureFactory();
        }

        Application app = new Application(factory);
        app.run();
    }
}
```

**The whole point:** `Application` doesn't know whether it's running on AWS or Azure. The decision lives in **one place** (the `if` in `main`). To add **GCP**, write `GCPFactory`, `GCPStorage`, `GCPDB` and add one `else if` — nothing else changes.

### 17.4 Factory vs. Abstract Factory — The Crisp Distinction

| | Factory | Abstract Factory |
|---|---|---|
| **Creates** | A single product | A **family** of related products |
| **Lecture example** | `PaymentFactory` returns Payment | `CloudFactory` returns Storage + DB together |
| **Choice point** | `factory.get("CC")` (string param) | `new AWSFactory()` (which factory you instantiate) |
| **Use when** | One axis of variation | Multiple correlated axes (Storage *and* DB must match) |
| **Number of classes** | 1 factory | N factories + N×M products |

### 17.5 Real-World Industry Examples

1. **GUI toolkits.** AWT/Swing's "Look and Feel" — one `WindowsLookAndFeel` factory creates Windows-style buttons + menus + scrollbars; `MacOSLookAndFeel` creates the matching Mac family.
2. **Cross-cloud abstraction layers (Terraform, Pulumi, Spring Cloud).** A `CloudResources` abstract factory hides AWS vs. Azure vs. GCP.
3. **Cross-DB drivers.** Hibernate dialects produce SQL families specific to MySQL / Postgres / Oracle while the application code stays the same.
4. **Theming systems.** A `Theme` abstract factory creates `Button`, `Card`, `TextField` skinned consistently for `LightTheme` vs. `DarkTheme`.
5. **Mocking infra in tests.** A `TestFactory` produces in-memory `Storage` + in-memory `Database`; production uses `AWSFactory`.

### 17.6 Abstract Factory — Interview Cheat Sheet

| Q | A |
|---|---|
| One-liner intent? | Create families of related objects without specifying concrete classes. |
| When over Factory? | When products must be **used together** and mixing families is forbidden. |
| Adding a new family? | Add a new concrete factory + concrete products. **Existing client code unchanged.** |
| Adding a new *product* (e.g., Cache)? | Must update **every** factory. (This is Abstract Factory's main weakness.) |

---

## 18. Builder Design Pattern (in depth)

> **Category:** Creational
>
> **Friendly definition:** When an object has **too many pieces, many of them optional**, Builder lets you assemble it **step by step** — pick only what you need, in any order — and click "Done" at the end to get the final, ready-to-use object.
>
> **Formal definition (for reference):** Construct a **complex object step by step**, decoupling the construction process from the final object's representation. Lets you build the same object in many configurations using a fluent API.

### 18.1 What Problem Does Builder Solve? — From Scratch

#### First, build the intuition — Subway sandwich analogy

Imagine ordering a Subway sandwich. You walk up to the counter, and the worker asks you step by step:

```
1. Bread?      →  "Wheat"
2. Meat?       →  "Turkey"
3. Cheese?     →  "American"   (optional)
4. Veggies?    →  "Tomato, lettuce, onion"   (multiple, optional)
5. Sauces?     →  "Mayo, mustard"             (multiple, optional)
6. Toasted?    →  "Yes"                        (optional)
7. Done?       →  "Yes!"  ← the sandwich is FINALLY assembled and handed to you
```

**Notice three things:**
1. You build the sandwich **one step at a time** — never in one giant request.
2. Many steps are **optional** (cheese, sauces, toasted).
3. The sandwich isn't yours until you say **"Done"** — only at the end do you get the final object.

That's exactly Builder Pattern. The Subway worker is the **Builder**, the sandwich is the **target object**, and "Done!" is the `build()` method.

#### Now translate to code — the problems with normal approaches

Imagine you're modeling a `Pizza` class. A pizza has **many optional toppings** — let's see what happens with the usual ways of constructing objects.

##### Problem 1: The "Telescoping Constructor" Anti-Pattern

When an object has many optional parameters, you end up writing **one constructor for every possible combination** — they "telescope" out longer and longer:

```java
class Pizza {
    Pizza(String size) { ... }
    Pizza(String size, boolean cheese) { ... }
    Pizza(String size, boolean cheese, boolean pepperoni) { ... }
    Pizza(String size, boolean cheese, boolean pepperoni, boolean olives) { ... }
    Pizza(String size, boolean cheese, boolean pepperoni, boolean olives,
          boolean mushrooms, boolean basil, String crust, String sauce) { ... }
    // ... and on, and on
}

// Caller — what does this even mean?
new Pizza("L", true, false, true, true, false, false, "thin", "tomato");
//                ↑     ↑      ↑     ↑     ↑      ↑
//             cheese? pep?  olives? mush? basil? ???
```

**Why it's bad:**
- **Unreadable** — you have to count commas to know what each `true` / `false` means.
- **Bug-prone** — swap two booleans by mistake, the compiler **can't catch it** (both are `boolean`).
- **Constructor explosion** — adding one new topping can mean adding 10 new constructors.

It's like ordering a Subway sandwich by yelling: *"Footlong, wheat, true, false, true, true, false, mayo, no mustard, toasted!"* — chaos.

##### Problem 2: Mutable JavaBeans (the "setter swamp")

A common alternative is empty constructor + setters:

```java
Pizza p = new Pizza();          // empty pizza — what does that even mean?
p.setSize("L");
p.setCheese(true);
p.setPepperoni(true);
//   ... did I forget setSauce()?  setCrust()?
//   The pizza exists in an INCONSISTENT half-built state during all this.
```

**Why it's bad:**
- The object exists in a **broken intermediate state** between `new Pizza()` and the last setter call.
- **No way to make the object immutable** — anyone can call setters later and change it.
- **Easy to forget a required field** — and your bug only surfaces at runtime.

###### What's the actual risk of a "half-built" object?

The deepest reason JavaBeans setters are bad: between `new Pizza()` and the last setter call, a **real, incomplete Pizza object exists in memory** — and any other code can grab it.

```java
Pizza p = new Pizza();      // ← STEP 1: a real Pizza EXISTS NOW (with no fields set)
p.setSize("L");             // ← STEP 2: still a real Pizza (size set, nothing else)
deliverToCustomer(p);       // ← Compiler is HAPPY to let this happen!
                            //   You just delivered a Pizza with no sauce, no crust,
                            //   and missing toppings — to a paying customer.
p.setCheese(true);          // ← STEP 3: too late — the broken Pizza already left.
```

**Concrete risks of letting half-built state escape:**

1. **Half-built objects can leak into other methods.**
   `deliverToCustomer(p)` accepts a `Pizza` — it doesn't know (or care) that you weren't done. The compiler sees a `Pizza` with all its fields and waves it through.

2. **Multi-threaded code sees corrupt state.**
   If thread A is mid-construction and thread B reads `p` at the same moment, B sees an inconsistent pizza (size set but no sauce, etc.). This causes flaky bugs that are nearly impossible to reproduce.

3. **No central validation point.**
   With Builder, `build()` is the ONE place to check "is this object valid?" With JavaBeans, every setter is independent — they can't enforce rules like "if cheese=true, sauce must not be null."

4. **Forgetting a required setter is a runtime time-bomb.**
   You write `setSize("L")` and forget `setSauce(...)`. The code compiles. The bug only shows up later — maybe in production — when something tries to use the missing field and crashes with a `NullPointerException`.

5. **The object is mutable FOREVER, not just during construction.**
   Even after construction is "done," the setters remain public. Six months later, some random code somewhere can call `p.setSize("XS")` and change your pizza out from under you. The compiler can't stop it.

6. **"Done-ness" is invisible.**
   Looking at any `Pizza` variable in the debugger or in code, you can't tell if it's "fully built and ready to use" or "still being filled in." There's no marker. With Builder, the type itself tells you: `Pizza.Builder` means in-progress; `Pizza` means finished.

###### Builder eliminates all six risks at once

| Risk with JavaBeans | How Builder prevents it |
|---|---|
| Half-built object leaks to other methods | The "in-progress" thing is a `Builder`, not a `Pizza` — wrong type, won't compile |
| Threads see corrupt state | The Pizza is created atomically in `build()` — once visible, fully complete |
| No central validation | `build()` is the single validation point |
| Forgetting required fields | `build()` can throw if required fields are missing |
| Object mutable forever | Pizza's fields are `final`, no setters exist |
| "Done-ness" invisible | The type itself signals state: `Builder` (in-progress) vs `Pizza` (done) |

> **The deeper principle:** never let an object exist in a state where it's **callable** but **not yet valid**. Builder enforces this by physically separating "the in-progress notepad" from "the final object."

##### Problem 3: Dynamic, conditional construction

Some objects literally **can't fit in a constructor** because their shape changes from one call to the next. Let's build the intuition step by step using SQL queries (the lecture's example).

###### Step A — A 30-second SQL refresher

A SQL query is just a sentence in the database language that says "fetch me some data":

```sql
SELECT * FROM users
```

That means: "give me everything from the users table." Simple.

###### Step B — SQL queries can have OPTIONAL extra parts

You can tack on extra clauses to filter, sort, or limit the result:

```sql
SELECT * FROM users
WHERE age > 25            -- optional: filter
ORDER BY name ASC         -- optional: sort
LIMIT 10                  -- optional: how many rows
```

Each of `WHERE`, `ORDER BY`, `LIMIT` is **optional** — you might use 0, 1, or all of them in any given query.

###### Step C — `WHERE` can appear MULTIPLE TIMES

Here's the kicker. You can have several `WHERE` conditions joined with `AND`:

```sql
SELECT * FROM users
WHERE age > 25
  AND status = 'active'
  AND country = 'India'
```

So `WHERE` can appear **0, 1, or many** times. The number changes per query.

###### Step D — Try to model this with a Java constructor

Imagine a `SQLQuery` class. Constructor approach:

```java
class SQLQuery {
    SQLQuery(String select, String from, String where, String orderBy, Integer limit) {
        // ...
    }
}
```

**Bug 1 — representing "no WHERE":** I have to pass `null` for the parts I don't want.

```java
new SQLQuery("*", "users", null, null, null);   // ugly but doable
```

**Bug 2 — representing MULTIPLE WHERE clauses:** This is where the constructor *breaks*. The constructor only takes ONE `String where`. So now I'd need:

```java
class SQLQuery {
    SQLQuery(String select, String from, String where1) { ... }
    SQLQuery(String select, String from, String where1, String where2) { ... }
    SQLQuery(String select, String from, String where1, String where2, String where3) { ... }
    // ... how many overloads do I need?  Infinite!
}
```

You **literally cannot** capture *"sometimes 0 WHEREs, sometimes 1, sometimes 50"* in a fixed constructor signature.

###### Step E — How Builder fixes it

The trick: instead of a single `String where` field, the Builder holds a **List of where clauses**, and each `.where(...)` call **appends** to the list.

```java
public static class Builder {
    private String       select;
    private String       from;
    private List<String> whereClauses = new ArrayList<>();   // ← a LIST, not a single String
    private String       orderBy;
    private Integer      limit;

    public Builder where(String clause) {
        this.whereClauses.add(clause);    // ← APPEND on every call
        return this;
    }
    // ... other setters ...
}
```

Then in `assemble()` (the method that builds the final SQL string), we use `if`s to add each optional clause **only if it was set**:

```java
public String assemble() {
    StringBuilder sb = new StringBuilder();
    sb.append("SELECT ").append(select)
      .append(" FROM ").append(from);

    if (!whereClauses.isEmpty()) {                  // include WHERE only if list has items
        sb.append(" WHERE ").append(String.join(" AND ", whereClauses));
    }
    if (orderBy != null) {                          // include ORDER BY only if set
        sb.append(" ORDER BY ").append(orderBy);
    }
    if (limit != null) {                            // include LIMIT only if set
        sb.append(" LIMIT ").append(limit);
    }
    return sb.toString();
}
```

###### Step F — Three different queries from the SAME Builder class

Watch how the same class produces three completely different queries, depending on which methods the caller chains:

**Query 1 — simple, no extras:**
```java
String q = new SQLQuery.Builder()
        .select("*")
        .from("users")
        .build()
        .getQuery();
// → "SELECT * FROM users"
```
The `assemble()` runs but skips WHERE/ORDER BY/LIMIT (their `if`s evaluate `false`).

**Query 2 — one WHERE, with sort and limit:**
```java
String q = new SQLQuery.Builder()
        .select("*")
        .from("users")
        .where("age > 25")
        .orderBy("name ASC")
        .limit(10)
        .build()
        .getQuery();
// → "SELECT * FROM users WHERE age > 25 ORDER BY name ASC LIMIT 10"
```

**Query 3 — multiple WHERE clauses:**
```java
String q = new SQLQuery.Builder()
        .select("*")
        .from("users")
        .where("age > 25")
        .where("status = 'active'")
        .where("country = 'India'")
        .build()
        .getQuery();
// → "SELECT * FROM users WHERE age > 25 AND status = 'active' AND country = 'India'"
```

**Same class, three completely different queries.** No constructor explosion. No infinite overloads. The Builder gracefully handles "0, 1, or many" of any optional piece.

###### Why this is called "dynamic conditional construction"

| Term | Meaning here |
|---|---|
| **Dynamic** | The shape of the final string is decided **at runtime** based on which methods you called. |
| **Conditional** | `if` checks inside `assemble()` decide which clauses appear. |
| **Construction** | We're creating an object (the SQLQuery). |

A normal constructor is **static** — fixed signature, fixed shape. Builder lets construction be **dynamic** because:
- The Builder collects pieces into mutable state (lists, optional fields).
- The final shape is decided in `build()`/`assemble()`, **after** all input is gathered.

> **One-line summary:** When the parts you assemble change from call to call (some optional, some repeated), a constructor can't capture it — Builder can, because it gathers state first and decides the final shape last.

### 18.2 The Builder Solution

The fix is exactly what the Subway worker does — **separate the assembly from the final product**:

1. Create a separate **Builder class** that holds the in-progress state.
2. Each optional piece becomes a **fluent setter** that returns `this` (so you can chain calls — like the worker asking the next question).
3. The customer signals "Done!" by calling `build()`, which:
   - Validates the result is sensible (e.g., must have bread + meat).
   - Returns the **final, immutable** target object.

**The result — readable, safe, flexible:**

```java
Pizza p = new Pizza.Builder("L")
    .cheese(true)
    .pepperoni(true)
    .crust("thin")
    .sauce("tomato")
    .build();
```

You read this top-to-bottom like English: *"a Large pizza, with cheese, with pepperoni, thin crust, tomato sauce — done."* Compare to `new Pizza("L", true, false, true, true, false, false, "thin", "tomato")` — there's no contest.

#### One-line summary

> **Telescoping constructor:** "Tell me everything in one breath." → unreadable.
> **Setters:** "Build me piece by piece, but I'm broken until you finish." → unsafe.
> **Builder:** "Tell me what you want step by step, then I'll hand you the finished object." → readable + safe.

### 18.3 The Lecture Example — SQL Query Builder

The lecture uses dynamic SQL queries with optional `WHERE`, `JOIN`, `ORDER BY`, `LIMIT`. Here's the full implementation cleaned up to compile.

#### Step 1 — The target class with an inner static `Builder`

```java
import java.util.ArrayList;
import java.util.List;

public class SQLQuery {
    private final String query;     // immutable final string

    private SQLQuery(Builder builder) {
        this.query = builder.buildQuery();   // built from builder's state
    }

    public String getQuery() {
        return query;
    }

    // ────────────────────────────────────────────────────────────────────
    //  Inner static Builder class
    // ────────────────────────────────────────────────────────────────────
    public static class Builder {
        private String       select;
        private String       from;
        private List<String> whereClause = new ArrayList<>();
        private String       orderBy;
        private Integer      limit;

        // ── Fluent setters: each returns `this` for chaining ──
        public Builder select(String select) {
            this.select = select;
            return this;
        }

        public Builder from(String from) {
            this.from = from;
            return this;
        }

        public Builder where(String clause) {
            this.whereClause.add(clause);   // can be called multiple times
            return this;
        }

        public Builder orderBy(String orderBy) {
            this.orderBy = orderBy;
            return this;
        }

        public Builder limit(int limit) {
            this.limit = limit;
            return this;
        }

        // ── The actual SQL assembly (the lecture's `buildQuery()`) ──
        public String buildQuery() {
            StringBuilder sb = new StringBuilder();
            sb.append("SELECT ").append(select)
              .append(" FROM ").append(from);

            if (!whereClause.isEmpty()) {
                sb.append(" WHERE ")
                  .append(String.join(" AND ", whereClause));
            }
            if (orderBy != null) {
                sb.append(" ORDER BY ").append(orderBy);
            }
            if (limit != null) {
                sb.append(" LIMIT ").append(limit);
            }
            return sb.toString();
        }

        // ── The terminal `build()` ──
        public SQLQuery build() {
            // (optional) validation:
            if (select == null || from == null) {
                throw new IllegalStateException("SELECT and FROM are required");
            }
            return new SQLQuery(this);
        }
    }
}
```

#### Step 2 — The client (lecture's `// Client Side`)

```java
public class Main {
    public static void main(String[] args) {
        SQLQuery query = new SQLQuery.Builder()
                .select("*")
                .from("users")
                .where("age > 25")
                .where("status = 'Active'")     // multiple WHEREs are fine
                .orderBy("name ASC")
                .limit(10)
                .build();

        System.out.println(query.getQuery());
        // → SELECT * FROM users WHERE age > 25 AND status = 'Active' ORDER BY name ASC LIMIT 10
    }
}
```

That **fluent chain** is the whole user experience of Builder. Compare it to a 7-argument constructor with positional booleans — it's not even close.

### 18.4 UML Diagram

```
        ┌──────────────────────┐
        │       Client          │
        └──────────┬───────────┘
                   │ uses
                   ▼
        ┌──────────────────────┐         creates       ┌────────────────────┐
        │  SQLQuery.Builder    │ ─────────────────────►│      SQLQuery      │
        │  - select            │      (build())        │  - query (final)   │
        │  - from              │                        │  + getQuery()      │
        │  - whereClause: List │                        └────────────────────┘
        │  - orderBy           │
        │  - limit             │
        │ +select(s)  : Builder│
        │ +from(s)    : Builder│
        │ +where(s)   : Builder│
        │ +orderBy(s) : Builder│
        │ +limit(n)   : Builder│
        │ +build()    : SQLQuery│
        └──────────────────────┘
```

### 18.5 Why "Inner Static" Builder?

- **Static** — you can build a `Builder` without first having a `SQLQuery` instance (`new SQLQuery.Builder()`).
- **Inner (nested)** — keeps the Builder logically tied to its target class (`SQLQuery.Builder`); the target's private constructor is accessible to the Builder.
- The target's constructor is **private** so the **only** way to create a `SQLQuery` is through the Builder. You cannot bypass it.

### 18.6 Variations & Real-World Patterns

#### (a) **Lombok's `@Builder`**

In real Java codebases nobody hand-writes 200 lines of Builder anymore — they use Lombok:

```java
@Builder
public class User {
    private String name;
    private int    age;
    private String email;
}

// Generated automatically:
User u = User.builder().name("Alice").age(30).email("a@b.com").build();
```

Lombok generates the inner `Builder` class at compile time.

#### (b) **Records + Builder (Java 17+)**

```java
public record User(String name, int age, String email) {
    public static class Builder { /* … */ }
}
```

#### (c) **StringBuilder & java.lang.ProcessBuilder**

The JDK already uses Builder all over the place. `ProcessBuilder` is the canonical example:

```java
Process p = new ProcessBuilder()
    .command("ls", "-la")
    .directory(new File("/tmp"))
    .redirectErrorStream(true)
    .start();
```

#### (d) **HTTP clients**

```java
HttpRequest request = HttpRequest.newBuilder()        // JDK 11+
    .uri(URI.create("https://api.example.com"))
    .header("Authorization", "Bearer xyz")
    .timeout(Duration.ofSeconds(10))
    .GET()
    .build();
```

#### (e) **Test data builders**

```java
User u = aUser().withName("Alice").withAge(30).asAdmin().build();
```

A widely-used testing pattern — readable test fixtures with sensible defaults.

### 18.7 Real-World Industry Examples

1. **`StringBuilder` / `StringBuffer`** in `java.lang`.
2. **`ProcessBuilder`** for OS process spawning.
3. **`HttpRequest.Builder`**, **`HttpClient.Builder`** in `java.net.http` (JDK 11+).
4. **AWS SDK v2.** Every request is `S3PutObjectRequest.builder().bucket("…").key("…").build()`.
5. **Lombok `@Builder`** on millions of Java classes in production.
6. **Protocol Buffers / gRPC** — generated message builders.
7. **MongoDB Java driver** — `Aggregates.lookup(...)`, `Filters.and(...)` use builder-like fluent APIs.
8. **JOOQ / QueryDSL** — entire fluent SQL-like DSLs built on the Builder pattern.

### 18.8 Builder vs. Other Creational Patterns

| Pattern          | What you control            | Result               |
|------------------|-----------------------------|----------------------|
| **Singleton**    | *Number* of instances (one) | One shared object    |
| **Factory**      | *Which* class to instantiate| One product          |
| **Abstract Factory** | Family of related products | Several products  |
| **Builder**      | *How* to assemble step-by-step | One complex object |
| **Prototype**    | Cloning an existing object  | Copy of a template   |

### 18.9 When to Use Builder (and When Not)

✅ **Use Builder when:**
- Object has **4+ constructor arguments**, especially several optional.
- You want the result to be **immutable** (no setters on the final object).
- Construction is **multi-step or conditional** (the SQL example).
- You want **readable** call sites.

❌ **Avoid Builder when:**
- Object has 1–3 simple required fields (a regular constructor is clearer).
- Performance-critical hot path (Builder allocates an extra object).
- You have setters anyway and don't care about immutability.

### 18.10 Builder — Interview Cheat Sheet

| Q | A |
|---|---|
| One-liner intent? | Build a complex object step by step using a fluent API; keep the final object immutable. |
| Anti-pattern it replaces? | **Telescoping constructor** + **mutable JavaBean setters**. |
| Why inner *static* Builder? | Logical grouping with target; can call private constructor; doesn't need an enclosing instance. |
| JDK examples? | `StringBuilder`, `ProcessBuilder`, `HttpRequest.Builder`, `Stream.Builder`. |
| Lombok? | `@Builder` generates the inner builder for you. |
| Why immutable result? | Once `build()` returns, no caller can mutate the SQLQuery — safe to share between threads, predictable. |

---

## Lecture 2 — Key Takeaways for Interviews

1. **ISP:** *"clients shouldn't be forced to depend on methods they don't use."* When you see `throw new UnsupportedOperationException`, that's an ISP red flag.
2. **DIP:** *"high-level modules and low-level modules both depend on abstractions."* DI is the technique; DIP is the principle. Spring is built on it.
3. **DRY:** Extract on the **third** duplication, not the first. Premature abstraction is worse than duplication.
4. **KISS / YAGNI:** Pick the simplest solution that solves *today's* problem. `n % 2 == 0` is a one-liner — don't build a factory for it.
5. **Singleton:** Always use **Double-Checked Locking + `volatile`**, **Bill Pugh holder**, or **enum**. Naïve Singleton is a thread-safety bug.
6. **`synchronized` block vs. method:** Use a block to keep the critical section as small as possible; locks have a real performance cost.
7. **Factory vs. Abstract Factory:** Factory = one product; Abstract Factory = a **family** of products that must match.
8. **Builder:** The go-to pattern when an object has **many parameters** or needs **step-by-step construction**. Combine with `final` fields → immutable result.
9. **Patterns are vocabulary, not goals.** Don't apply them just because you can. Solve the problem; recognize when it matches a known pattern.

---

**Lecture 2 Complete.**

---

# Lecture 3: Structural & Behavioral Design Patterns

> **Lecture date:** 3rd May
> **Topics covered:** Adapter · Facade · Decorator · Flyweight (Structural) · Strategy · Observer · Chain of Responsibility · Template Method (Behavioral)

---

## 19. Structural Design Patterns — Introduction

> **Friendly definition:** Structural patterns are about **how classes and objects are wired together** to build bigger, more flexible structures. Think of them as "connectors" — they help mismatched parts work together, hide complexity behind a simpler door, add features on top of existing parts without modifying them, and share resources to save memory.
>
> **Formal definition (for reference):** Structural patterns deal with object composition — they describe ways to compose interfaces or implementations to obtain larger structures while keeping these structures flexible and efficient.

### 19.1 The 4 Structural Patterns We'll Cover

| # | Pattern | One-liner |
|---|---|---|
| 1 | **Adapter** | Make incompatible interfaces work together (translator). |
| 2 | **Facade** | Provide a single, simple front door to a complex subsystem. |
| 3 | **Decorator** | Add new behavior to an object at runtime without changing its class. |
| 4 | **Flyweight** | Share common parts among many similar objects to save memory. |

### 19.2 Mental Model — How They Differ

```
ADAPTER       FACADE        DECORATOR        FLYWEIGHT
─────────     ─────────     ──────────       ──────────
[Old plug]    [Front]       [Plain]          [1 shared
   ↓          desk]            ↓               red ●]
[Adapter]     hides         [Wrapped+        used by
   ↓          → Inv          icing]            1000s of
[New plug]    → Pay            ↓             ● ● ● ●
              → Ship        [Wrapped+
                            icing+layer]

Translates    Simplifies    Stacks         Shares
interfaces    the API       behavior       memory
```

Each one solves a different "structural" problem:
- **Adapter** → "These two don't talk. Make them talk."
- **Facade** → "This subsystem is too messy for clients. Hide it."
- **Decorator** → "I want to add features without subclassing."
- **Flyweight** → "I have a million objects but only a few unique ones. Don't waste RAM."

We'll do each one with the same depth: real-world analogy → bad way → pattern → code → UML → trade-offs → cheat sheet.

---

## 20. Adapter Design Pattern

> **Category:** Structural
>
> **Friendly definition:** When two pieces of code don't speak the same language, an **Adapter** is a translator that sits between them. The client speaks language A, the legacy system speaks language B — the Adapter takes A's words and converts them into B's words on the fly.
>
> **Formal definition (for reference):** Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
>
> **Lecture quote:** *"Allow incompatible interfaces to work together."*

### 20.1 The Problem It Solves — From Scratch

#### Real-world analogy: the travel power adapter

You're traveling from India to the US. Your laptop has an Indian plug (Type D, three round pins). The wall socket in the US is Type B (two flat pins + one round). They are **physically incompatible** — your plug won't fit.

You don't want to:
- Buy a new laptop (rewriting your code).
- Modify the wall socket (modifying the legacy system you don't own).

Solution: a **travel adapter** — a small device that has Type-D holes on one side (where YOU plug in) and Type-B prongs on the other (which goes into the wall). It physically translates between the two formats.

That's an Adapter.

```
   Your laptop          [ADAPTER]              US wall socket
   (Type D plug)   →   D socket  →  B prongs   ←  (Type B)
                   ─→  ───────────────────────  ←─
                       client side   wall side
```

#### Translate to code: payment processing

The lecture's exact example. Your application uses a clean modern interface:

```java
// Your app's standard interface — what your code expects everywhere
public interface PaymentProcessor {
    void pay(double amount);   // a clean, named method
}
```

But you also need to integrate **Razorpay** — a third-party payment gateway that has its own legacy class with its own API:

```java
// Legacy system — you can't change this; it's a third-party library
public class RazorpayGateway {
    public void makePayment(double amount) {
        // ... actual razorpay logic ...
        System.out.println("Paid " + amount + " using Razorpay");
    }
}
```

The names are different (`pay()` vs `makePayment()`). The client wants to call `pay(amount)` everywhere; Razorpay only knows `makePayment(amount)`. They don't speak the same language.

#### The bad way — modify the client to know about Razorpay directly

```java
// Client code — directly aware of every payment provider's quirks:
if (provider.equals("razorpay")) {
    new RazorpayGateway().makePayment(1000);     // legacy method name
} else if (provider.equals("stripe")) {
    new StripeAPI().chargeCard(1000);             // yet another name
} else if (provider.equals("paypal")) {
    new PayPalSDK().sendMoney(1000);              // yet another name
}
```

**Why it's bad:**
- Client code is now **tightly coupled** to every gateway's API quirks.
- Adding a new gateway = modifying every place that handles payments.
- Code is unreadable — every gateway speaks differently.

#### The Adapter solution

Wrap the legacy class in a new class that **implements your standard interface** but **delegates** the work to the legacy object inside.

### 20.2 Code Walkthrough — Step by Step

#### Step 1 — The standard interface (what client expects)

```java
public interface PaymentProcessor {
    void pay(double amount);
}
```

#### Step 2 — The legacy class (what we can't modify)

```java
public class RazorpayGateway {
    public void makePayment(double amount) {
        System.out.println("Paid " + amount + " using Razorpay");
    }
}
```

#### Step 3 — The Adapter (the translator)

```java
public class RazorpayAdapter implements PaymentProcessor {
    private final RazorpayGateway razorpay;   // holds a reference to the legacy object

    public RazorpayAdapter(RazorpayGateway razorpay) {
        this.razorpay = razorpay;              // get the legacy object via constructor
    }

    @Override
    public void pay(double amount) {           // implements the STANDARD interface
        razorpay.makePayment(amount);          // delegates to the LEGACY method
    }
}
```

This is the entire "magic." `RazorpayAdapter`:
- **Implements** `PaymentProcessor` → so the client can use it just like any other PaymentProcessor.
- **Holds** a `RazorpayGateway` → the actual worker.
- **Translates**: when `pay(amount)` is called on the adapter, it internally calls `razorpay.makePayment(amount)`.

#### Step 4 — The client (the caller code)

```java
public class AdapterDemo {
    public static void main(String[] args) {
        // Wrap the legacy gateway in our adapter
        PaymentProcessor processor = new RazorpayAdapter(new RazorpayGateway());

        // Now the client uses ONLY the standard interface — no idea Razorpay is inside!
        processor.pay(1000);
    }
}
```

The client never sees `RazorpayGateway` or `makePayment`. It only knows `PaymentProcessor.pay(...)`. Adding Stripe later means writing a `StripeAdapter` — the client code doesn't change.

### 20.3 UML Diagram

```
            ┌───────────────────────┐
            │   <<interface>>        │
            │  PaymentProcessor      │  ← Target (what client expects)
            │   + pay(double)        │
            └─────────△──────────────┘
                      ╎ realizes (dashed line + hollow triangle)
                      ╎
            ┌─────────┴──────────────┐    aggregates  ┌──────────────────────┐
            │  RazorpayAdapter       │◇──────────────►│   RazorpayGateway    │ ← Adaptee
            │   - razorpay           │ (hollow diamond│   + makePayment(amt) │   (legacy class,
            │   + pay(amount) {      │  + open arrow) │                      │    passed in via
            │     razorpay           │                └──────────────────────┘    constructor)
            │      .makePayment(amt) │
            │   }                    │
            └────────────────────────┘
                      ▲
                      │ uses (solid line + open arrow ▲)
                      │
            ┌─────────┴──────────────┐
            │       Client            │
            │  (knows ONLY the        │
            │   PaymentProcessor      │
            │   interface)            │
            └────────────────────────┘
```

> **Notes on the arrows in this diagram:**
> 1. `△` (hollow triangle) is reserved for **inheritance/realization** only. For "uses / association / dependency", use an **open arrowhead** (drawn as `▲` for vertical, `►` / `>` for horizontal). So `Client → RazorpayAdapter` is a plain **association** (Client holds a reference of type `PaymentProcessor`), not inheritance.
> 2. The `RazorpayAdapter`–`RazorpayGateway` link is **aggregation** (`◇`), not plain "uses". Why? `RazorpayAdapter` stores the gateway as a `private final` field, but it does **not create** the gateway — the client passes a pre-existing `new RazorpayGateway()` into the adapter's constructor. The gateway exists *before* the adapter and could survive *after* it. If the adapter created the gateway itself (e.g. `this.razorpay = new RazorpayGateway()`), it would be **composition** (`◆`).

The 3 roles:
- **Target** = `PaymentProcessor` interface (what client wants).
- **Adaptee** = `RazorpayGateway` (legacy class that doesn't fit).
- **Adapter** = `RazorpayAdapter` (the bridge).

### 20.4 Two Flavors — Object Adapter vs Class Adapter

| Flavor | How it works | Example we used |
|---|---|---|
| **Object Adapter** | Adapter HAS-A reference to the adaptee (composition) | Yes — we stored `RazorpayGateway razorpay` |
| **Class Adapter** | Adapter EXTENDS the adaptee (inheritance) — needs multiple inheritance | Rare in Java (no multi-inheritance) |

**Java strongly prefers Object Adapter** because:
- No multi-inheritance limitations.
- Can adapt subclasses of the adaptee at runtime.
- Composition over inheritance (good design principle).

So when someone says "Adapter pattern in Java," 99% of the time it's the object adapter.

### 20.5 Address Common Misconceptions

| Question | Answer |
|---|---|
| Is Adapter just a wrapper? | Yes — but specifically a wrapper that **converts one interface to another**. Not every wrapper is an adapter. |
| What if I can modify the legacy class? | Then you don't need an adapter — just rename the method. Adapter is for cases where you can't (third-party JARs, databases, etc.) |
| Does Adapter add new functionality? | No, it just translates. If you're adding new behavior, that's **Decorator**, not Adapter. |
| Can one adapter wrap multiple adaptees? | Usually it wraps one. If you're combining many subsystems behind one front, that's **Facade**, not Adapter. |
| What's the difference between Adapter and Bridge? | Adapter solves an existing incompatibility. Bridge is designed up front to decouple abstraction from implementation. |

### 20.6 Trade-offs

✅ **Pros:**
- Lets your code use legacy/third-party APIs without changing your code's interface.
- Open-Closed friendly — adding a new gateway = new adapter, no client changes.
- Single Responsibility — translation logic lives in one place.

⚠️ **Cons:**
- Extra layer of indirection (slight performance/complexity cost).
- If the adaptee's interface changes drastically, the adapter needs maintenance.
- Easy to overuse — if you can change the legacy code, do that instead.

### 20.7 Real-World Industry Examples

1. **`java.util.Arrays.asList(T[])`** — adapts an array (`T[]`) into a `List<T>`.
2. **`java.io.InputStreamReader`** — adapts byte streams (`InputStream`) into character streams (`Reader`).
3. **Spring's `HandlerAdapter`** — adapts different Spring controller types to the dispatcher's expected interface.
4. **JDBC drivers** — each database vendor's driver adapts the standard `java.sql.Connection` interface to their proprietary protocols.
5. **Payment SDKs** (Stripe, PayPal, Razorpay) — large fintech apps wrap each one in an adapter so the rest of the code uses one consistent interface.

### 20.8 Adapter — Interview Cheat Sheet

| Q | A |
|---|---|
| When would you use Adapter? | When you need to integrate a class whose interface doesn't match what your code expects, AND you can't modify the source. |
| Object vs class adapter? | Object adapter (composition). Class adapter (inheritance) is rarely used in Java due to single-inheritance. |
| Difference vs Decorator? | Adapter changes the **interface**; Decorator adds **behavior** (same interface, more capability). |
| Difference vs Facade? | Adapter wraps **one** thing to fix an incompatibility. Facade wraps **many** things to simplify usage. |
| Real Java example? | `java.util.Arrays.asList(...)`, `InputStreamReader`. |

### 20.9 TL;DR

> **Adapter = translator.** When the client speaks one interface and the existing class speaks another, slot an Adapter in between. The Adapter implements the client's interface and internally calls the existing class. Use composition (object adapter) — that's the Java way.

---

## 21. Facade Design Pattern

> **Category:** Structural
>
> **Friendly definition:** When you have a complex subsystem with many moving parts, **Facade** is a single, simple "front desk" that the client talks to. The client says one polite request; the front desk handles all the messy coordination behind the scenes.
>
> **Formal definition (for reference):** Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
>
> **Lecture quote:** *"Complex classes, libraries or subsystem → simplified / unified interface."*

### 21.1 The Problem It Solves — From Scratch

#### Real-world analogy: hotel front desk

You arrive at a fancy hotel. To enjoy your stay, you need:
- A room booked
- Bags carried up
- Restaurant reservations
- Wake-up call
- Valet parking
- Hot water arranged
- Wifi password

Imagine if you had to walk to **7 different departments** every time you needed something — finance, housekeeping, kitchen, valet, front-of-house, IT, security. Exhausting!

Instead, the hotel has a **front desk**. You say one thing — *"I'm checking in."* — and the front desk **coordinates** with all the departments behind the scenes. You only ever talk to the front desk.

That's a Facade.

#### Translate to code: order management system

The lecture's exact example. To place an online order, the system must:

1. Check inventory (Inventory Service)
2. Process payment (Payment Service)
3. Schedule shipping (Shipping Service)

Three different subsystems, each with its own API.

#### The bad way — client manages all 3 subsystems

```java
public class CheckoutController {
    public void placeOrder(String item, double amount) {
        // The CLIENT has to know about all 3 services and orchestrate them itself:
        InventoryService inventory = new InventoryService();
        PaymentService payment     = new PaymentService();
        ShippingService shipping   = new ShippingService();

        if (inventory.checkStock(item)) {
            payment.processPayment(amount);
            shipping.shipItem(item);
            System.out.println("Order placed");
        } else {
            System.out.println("Out of stock");
        }
    }
}
```

**Why it's bad:**
- Every place that places an order has to **repeat** this 3-step orchestration.
- Client is **tightly coupled** to all 3 service APIs.
- If a 4th step is added (e.g., notification email), you must edit every caller.
- Hard to test — too many dependencies in client.
- Violates **encapsulation** — clients shouldn't need to know the inner steps.

#### The Facade solution

Create one class — `OrderFacade` — that owns the 3 services and exposes ONE simple method: `placeOrder(item, amount)`. Clients only talk to the Facade.

### 21.2 Code Walkthrough — Step by Step

#### Step 1 — The subsystem services (already exist)

```java
public class InventoryService {
    public boolean checkStock(String item) {
        System.out.println("Checking inventory for " + item);
        return true;
    }
}

public class PaymentService {
    public void processPayment(double amount) {
        System.out.println("Payment processed: " + amount);
    }
}

public class ShippingService {
    public void shipItem(String item) {
        System.out.println("Shipping " + item);
    }
}
```

#### Step 2 — The Facade class — one front door

```java
public class OrderFacade {
    private final InventoryService inventory = new InventoryService();
    private final PaymentService   payment   = new PaymentService();
    private final ShippingService  shipping  = new ShippingService();

    // ── ONE simple method that hides all the coordination ──
    public void placeOrder(String item, double amount) {
        try {
            if (inventory.checkStock(item)) {
                payment.processPayment(amount);
                shipping.shipItem(item);
                System.out.println("Order placed successfully");
            } else {
                System.out.println("Item out of stock");
            }
        } catch (Exception e) {
            System.out.println("Order failed: " + e.getMessage());
        }
    }
}
```

#### Step 3 — The client — beautifully simple now

```java
public class Main {
    public static void main(String[] args) {
        OrderFacade facade = new OrderFacade();
        facade.placeOrder("Laptop", 50000);
        // → Checking inventory for Laptop
        // → Payment processed: 50000.0
        // → Shipping Laptop
        // → Order placed successfully
    }
}
```

The client has NO idea about `InventoryService`, `PaymentService`, or `ShippingService`. It just calls `facade.placeOrder(...)`.

### 21.3 UML Diagram

```
                 ┌────────────────────────────┐
                 │           Client            │
                 │  (only knows OrderFacade)   │
                 └─────────────┬──────────────┘
                               │ uses (solid + open arrow ▼)
                               ▼
                 ┌────────────────────────────┐
                 │       OrderFacade           │   ← THE FRONT DOOR
                 │   - inventory               │
                 │   - payment                 │
                 │   - shipping                │
                 │   + placeOrder(item, amt)   │   ← ONE simple method
                 └────◆─────◆─────◆──────────┘
                      │     │     │  composition (filled diamond)
                      │     │     │  OrderFacade CREATES & OWNS each service
              ┌───────┘     │     └────────┐
              ▼             ▼              ▼
   ┌──────────────────┐ ┌──────────────┐ ┌─────────────────┐
   │ InventoryService │ │PaymentService│ │ ShippingService │   ← Subsystem services
   │ +checkStock()    │ │ +processPayment │ │ + shipItem()  │     (client never sees these)
   └──────────────────┘ └──────────────┘ └─────────────────┘
```

> **Why composition (◆), not aggregation (◇) or plain "uses"?** Look at the code: `private final InventoryService inventory = new InventoryService();` — the services are **created inside** OrderFacade and never escape (they're `private`). Their lifecycle is bound to OrderFacade — born with it, die with it. That's the textbook definition of composition. **Plain "uses" or aggregation would be wrong here.**
>
> ⚠️ **If you change the code to dependency injection** (services passed in via the constructor — the modern Spring style), the relationship becomes **aggregation (◇)** instead, because the services would now be created externally and merely *borrowed* by the facade. Same Facade pattern, different lifecycle relationship — the UML must reflect what the code actually does.

### 21.4 Address Common Misconceptions

| Question | Answer |
|---|---|
| Does Facade hide the subsystem completely? | No — clients **can** still use the subsystem directly if they need to. Facade is optional. It's a convenience layer, not a wall. |
| Is Facade just a "wrapper"? | It's a wrapper, but specifically one that **hides complexity**. Adapter is also a wrapper but for a different reason (incompatibility). |
| Can a Facade do more than just delegate? | Yes — Facades often add validation, logging, exception handling, transaction management. As long as the *client view* stays simple. |
| One Facade or many? | Depends on cohesion. One Facade per "high-level use case" is common — `OrderFacade`, `UserOnboardingFacade`, `ReportingFacade`. |
| Difference vs Mediator? | Mediator coordinates **peer objects** that talk to each other. Facade simplifies access to a **subsystem** for outside clients. |

### 21.5 Trade-offs

✅ **Pros:**
- **Simplifies usage** — clients deal with one method instead of 5 services.
- **Decouples** clients from subsystem details.
- **Centralizes orchestration** — adding a new step (e.g., email notification) means editing only the Facade.
- Great for legacy systems — wrap an ugly old API in a Facade and migrate gradually.

⚠️ **Cons:**
- Can become a **god object** if you keep adding methods. Keep facades focused.
- Hides flexibility — power users may need direct access to subsystem.
- Doesn't reduce the underlying complexity, just hides it.

### 21.6 Real-World Industry Examples

1. **`javax.faces.context.FacesContext`** — front for the JSF subsystem.
2. **Spring's `JdbcTemplate`** — facade over the JDBC API (connection, statement, result-set, exception handling).
3. **SLF4J logger** — facade over different logging implementations (Log4j, Logback, java.util.logging).
4. **`java.net.URL`** — facade over the URL connection / stream / parsing internals.
5. **Hibernate `Session`** — facade over JDBC, transaction, caching, and SQL generation.

### 21.7 Facade — Interview Cheat Sheet

| Q | A |
|---|---|
| When would you use Facade? | When clients face a complex subsystem and you want to give them a simple, unified entry point. |
| Difference vs Adapter? | Adapter changes one interface to another. Facade defines a NEW simpler interface over many internal APIs. |
| Does Facade make subsystem inaccessible? | No — it's optional. Power users can still bypass the Facade. |
| Can a Facade be a Singleton? | Often yes — one Facade per use case is common. Be careful about coupling and testability though. |
| Real Java example? | `JdbcTemplate`, SLF4J, `JOptionPane`. |

### 21.8 TL;DR

> **Facade = front desk.** When clients face a multi-step subsystem, give them ONE class with simple methods that internally orchestrate the messy details. Reduces coupling, simplifies the client's life, and centralizes orchestration logic in one place.

---

## 22. Decorator Design Pattern

> **Category:** Structural
>
> **Friendly definition:** **Decorator** lets you add new features to an object by wrapping it in another object — like adding icing on a plain cake. The original object stays untouched; the wrapper adds behavior on top. You can stack multiple wrappers to combine features.
>
> **Formal definition (for reference):** Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
>
> **Lecture quote:** *"Add new behavior or responsibilities to object dynamically at runtime, without altering their original code."*

### 22.1 The Problem It Solves — From Scratch

#### Real-world analogy: layers on a plain cake

Start with a **plain cake**. You want to add features:
- Add a **sponge layer** in the middle.
- Add **toppings** (cherries, nuts).
- Add **icing** on top.

Each addition is **on top of** the previous one. The plain cake itself never changes. Each layer wraps what's below.

```
Layer 4: ICING            ┐
Layer 3: TOPPINGS         │  ← decorations stacked
Layer 2: SPONGE LAYER     │     on top of each other
Layer 1: PLAIN CAKE       ┘  ← original object, untouched
```

Each layer **delegates** to what's below, then adds its own thing. That's Decorator.

#### Translate to code: a drawing application

The lecture's exact example. You have shapes (Circle, Rectangle, Square) and you want to **decorate** some of them with extras like:
- A **red border**
- A **shadow**
- A **fill color**

You don't want every combination as a separate class (`RedBorderCircle`, `ShadowedCircle`, `RedShadowedCircle`, `FilledRedShadowedCircle`...) — that's class explosion.

#### The bad way — subclass for every combination

```java
class Circle { void draw() { ... } }
class CircleWithRedBorder extends Circle { void draw() { /* draw + add border */ } }
class CircleWithShadow extends Circle { void draw() { /* draw + add shadow */ } }
class CircleWithRedBorderAndShadow extends Circle { /* both! */ }
class RectangleWithRedBorder extends Rectangle { ... }
class RectangleWithShadow extends Rectangle { ... }
// ... etc., for 3 shapes × 3 decorations × all combos = 27+ classes!
```

**Why it's bad:**
- **Class explosion** — N shapes × M decorations × all combinations = exponential growth.
- **Static** — once you make a `CircleWithBorder`, you can't add a shadow at runtime.
- **Repetition** — every shape needs its own RedBorder version.

#### The Decorator solution

Make each "decoration" itself a class that **wraps a shape** and adds its bit. Stack them at runtime in any combination.

```java
Shape s1 = new Circle();
Shape s2 = new RedBorderDecorator(new Circle());                 // circle + border
Shape s3 = new ShadowDecorator(new RedBorderDecorator(new Circle()));  // border + shadow
```

3 classes for shapes + 3 classes for decorators = 6 classes, infinite combinations.

### 22.2 Code Walkthrough — Step by Step

#### Step 1 — The base interface

```java
public interface Shape {
    void draw();
}
```

#### Step 2 — Concrete shapes (the "plain cake")

```java
public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}
```

#### Step 3 — The abstract Decorator (the "wrapper template")

This is the secret sauce. The decorator IS-A Shape (so clients can use it as one) AND HAS-A Shape (so it can wrap any shape).

```java
public abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;          // ← the shape we're wrapping

    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }

    @Override
    public void draw() {
        decoratedShape.draw();                // ← delegate to the wrapped shape
    }
}
```

> **Why abstract?** Because the base decorator just delegates — it has no specific decoration. Concrete decorators (RedBorderDecorator, ShadowDecorator) extend this and add their own behavior on top.

##### Why do we even NEED `ShapeDecorator`? (the deep "why")

This is the most-skipped explanation in design pattern tutorials. Let's do it properly.

###### Friendly analogy — the gift-wrapping kit

Imagine you run a gift-wrapping shop and offer 10 styles: ribbon-wrap, bow-wrap, name-tag-wrap, glitter-wrap, etc. Every style needs the **same boring base setup**: take the gift, hold it, put paper around it. The *only* thing that differs between styles is the final flourish on top.

You have two options:
1. **No kit (no abstract class)** — every wrapper does the boring base setup themselves. Lots of repeated work, easy to mess up.
2. **Pre-made kit (abstract class)** — the kit comes with paper, tape, and a holder. Each wrapper just adds their unique flourish.

`ShapeDecorator` is the pre-made kit. Concrete decorators (RedBorder, Shadow, Blur, Watermark…) just add their flourish.

###### The 4 concrete jobs `ShapeDecorator` does for us

| Job | What it gives every concrete decorator | Why we want it centralized |
|---|---|---|
| 1. Implements `Shape` | Concrete decorators *automatically* IS-A `Shape` (they extend the abstract class which already implements `Shape`) | Keep concrete decorators focused on the *flourish*, not the contract paperwork |
| 2. Holds the wrapped `Shape` field | `protected Shape decoratedShape;` is declared once | Otherwise every concrete decorator declares & initializes the same field |
| 3. Constructor that takes a `Shape` | `super(decoratedShape)` is the standard call | Avoids repeating `this.x = x` boilerplate in every decorator |
| 4. Default delegation behavior for *all interface methods* | `draw()` defaults to forwarding to `decoratedShape.draw()` | **HUGE win** when the interface has many methods (see below) |

Now let's see what happens if we throw it away.

###### What happens WITHOUT the abstract class — duplication pain

If every concrete decorator implements `Shape` directly, look what each one becomes:

```java
// ── BAD: no abstract decorator ──
public class RedBorderDecorator implements Shape {
    private Shape decoratedShape;                                  // ← duplicated

    public RedBorderDecorator(Shape decoratedShape) {              // ← duplicated
        this.decoratedShape = decoratedShape;
    }

    @Override
    public void draw() {
        decoratedShape.draw();                                     // ← duplicated delegation
        setRedBorder();
    }
    private void setRedBorder() { System.out.println("Adding Red Border"); }
}

public class ShadowDecorator implements Shape {
    private Shape decoratedShape;                                  // ← duplicated AGAIN

    public ShadowDecorator(Shape decoratedShape) {                 // ← duplicated AGAIN
        this.decoratedShape = decoratedShape;
    }

    @Override
    public void draw() {
        decoratedShape.draw();                                     // ← duplicated AGAIN
        addShadow();
    }
    private void addShadow() { System.out.println("Adding Shadow"); }
}
```

For 10 decorators, you copy-paste the same 4 lines (field + constructor + delegation) **10 times**. That's WET (Write Everything Twice) — a pure DRY violation.

###### Where it gets *really* painful — interfaces with multiple methods

`Shape` only has `draw()`, so the duplication looks small. But **realistic interfaces have many methods**, and that's where the abstract class becomes essential.

Suppose `Shape` has 4 methods:

```java
public interface Shape {
    void draw();
    void resize(double factor);
    String getColor();
    double getArea();
}
```

A decorator typically wants to enhance only **one** of these (say, `draw()`) and **pass the rest through unchanged**.

**With the abstract class — easy:**

```java
public abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;

    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }

    @Override public void draw()                  { decoratedShape.draw(); }
    @Override public void resize(double factor)   { decoratedShape.resize(factor); }
    @Override public String getColor()            { return decoratedShape.getColor(); }
    @Override public double getArea()             { return decoratedShape.getArea(); }
}

// Concrete decorator only overrides the ONE method it cares about
public class RedBorderDecorator extends ShapeDecorator {
    public RedBorderDecorator(Shape s) { super(s); }

    @Override
    public void draw() {
        super.draw();                  // delegate first
        System.out.println("Adding Red Border");
    }
    // resize, getColor, getArea inherited as-is — perfect pass-through
}
```

`RedBorderDecorator` is **8 lines** and does exactly what it should.

**Without the abstract class — repetitive misery:**

```java
public class RedBorderDecorator implements Shape {
    private Shape decoratedShape;
    public RedBorderDecorator(Shape s) { this.decoratedShape = s; }

    @Override public void draw()                {
        decoratedShape.draw();
        System.out.println("Adding Red Border");
    }
    // ↓ FORCED to re-implement these even though we don't decorate them ↓
    @Override public void resize(double factor) { decoratedShape.resize(factor); }
    @Override public String getColor()          { return decoratedShape.getColor(); }
    @Override public double getArea()           { return decoratedShape.getArea(); }
}
```

`RedBorderDecorator` balloons to **15+ lines**, and every other decorator (`ShadowDecorator`, `BlurDecorator`, …) repeats those 3 pass-through methods. **That's the real cost.**

###### Side-by-side comparison

| Concern | Without abstract class | With abstract class |
|---|---|---|
| Field `decoratedShape` | Declared in every decorator | Declared once in parent |
| Constructor wiring | Repeated in every decorator | Standard `super(s)` call |
| Pass-through methods | Re-implemented in every decorator | Inherited from parent |
| Lines per decorator (4-method interface) | ~15 | ~6 |
| Adding a new method to `Shape` | **Touch every concrete decorator** ❌ | **Touch only `ShapeDecorator`** ✅ |
| Risk of forgetting to delegate a method | High — easy to miss one | None — inherited automatically |
| Reads as "this is a decorator" | Not obvious — it's just `implements Shape` | Crystal clear via `extends ShapeDecorator` |

The last row is sneaky-important: **`extends ShapeDecorator` documents intent.** Anyone reading `class WatermarkDecorator extends ShapeDecorator` knows instantly "this wraps another Shape and tweaks behavior." `class WatermarkDecorator implements Shape` doesn't tell you that.

###### When CAN you skip it?

You can drop the abstract class only when **all three** are true:

1. The component interface has **exactly 1 method** (so no pass-through methods to centralize), AND
2. You'll only ever have **1 or 2 concrete decorators** (so duplication won't grow), AND
3. You don't care about reading-clarity ("this class is a decorator, that one isn't").

In real projects this is almost never true — interfaces grow, decorators multiply, and clarity matters. So **default to including the abstract class.** Pretty much every JDK example follows this rule (see `BufferedReader`, `BufferedInputStream`, etc. — they all extend `FilterReader` / `FilterInputStream`, which are exactly this kind of abstract decorator).

###### TL;DR — why the abstract class exists

> **`ShapeDecorator` is the place where the *common wiring* of wrapping lives:** the field, the constructor, and the default "do whatever the wrapped shape does" delegation. Without it, every concrete decorator becomes a tax form full of boilerplate. With it, each decorator becomes a one-liner addition to existing behavior.

That's why it's there — and that's why the JDK, Spring, every UI framework, and every IO stream library all use this exact pattern.

#### Step 4 — A concrete decorator (the "icing")

```java
public class RedBorderDecorator extends ShapeDecorator {

    public RedBorderDecorator(Shape decoratedShape) {
        super(decoratedShape);                 // pass the shape to parent
    }

    @Override
    public void draw() {
        decoratedShape.draw();                 // first: do whatever the wrapped shape does
        setRedBorder();                         // then: add OUR decoration
    }

    private void setRedBorder() {
        System.out.println("Adding Red Border");
    }
}
```

The order matters: `decoratedShape.draw()` runs first, then `setRedBorder()` adds the border on top. That's the "stacking" effect.

#### Step 5 — The client — stack decorators in any order

```java
public class DecoratorDemo {
    public static void main(String[] args) {
        Shape circle        = new Circle();
        Shape redCircle     = new RedBorderDecorator(new Circle());
        Shape redRectangle  = new RedBorderDecorator(new Rectangle());

        // Plain circle
        circle.draw();
        // → Drawing Circle

        // Circle + red border
        redCircle.draw();
        // → Drawing Circle
        // → Adding Red Border

        // Rectangle + red border
        redRectangle.draw();
        // → Drawing Rectangle
        // → Adding Red Border
    }
}
```

Notice: the same `RedBorderDecorator` works on both Circle and Rectangle. **One decorator, infinite shapes it can decorate.** Same for vice versa.

#### Step 6 — Stacking decorators (the real power)

##### 6a — First, here's a second decorator (`ShadowDecorator`)

Adding a new decoration is **literally a copy of `RedBorderDecorator` with the action swapped**. That's the OCP magic.

```java
public class ShadowDecorator extends ShapeDecorator {

    public ShadowDecorator(Shape decoratedShape) {
        super(decoratedShape);                 // pass the shape to parent
    }

    @Override
    public void draw() {
        decoratedShape.draw();                 // first: draw the wrapped shape
        addShadow();                           // then: add OUR decoration on top
    }

    private void addShadow() {
        System.out.println("Adding Shadow");
    }
}
```

Compare with `RedBorderDecorator` — **only 3 things differ** (class name, action method name, print line). Everything else is identical:

| Part | `RedBorderDecorator` | `ShadowDecorator` |
|---|---|---|
| Class name | `RedBorderDecorator` | `ShadowDecorator` |
| `draw()` body | `super.draw(); setRedBorder();` | `super.draw(); addShadow();` |
| Action method | `setRedBorder()` → "Adding Red Border" | `addShadow()` → "Adding Shadow" |

> **Important:** to add `ShadowDecorator`, we made **zero changes** to `Shape`, `Circle`, `Rectangle`, `ShapeDecorator`, or `RedBorderDecorator`. That's pure Open–Closed Principle: open for extension, closed for modification.

##### 6b — Now stack them

```java
Shape fancy = new ShadowDecorator(
                  new RedBorderDecorator(
                      new Circle()));

fancy.draw();
// → Drawing Circle
// → Adding Red Border
// → Adding Shadow
```

##### 6c — Why does the output come out in *that* exact order?

Picture the wrapping like Russian dolls:

```
   ShadowDecorator ─┐
   ┌────────────────┘
   │  RedBorderDecorator ─┐
   │  ┌────────────────────┘
   │  │  Circle
   │  │
   │  └─→ draw(): "Drawing Circle"
   │
   └─→ super.draw() unwinds Red, then adds Shadow
```

Trace `fancy.draw()` step by step:

| Step | Whose `draw()` runs | What it does |
|---|---|---|
| 1 | `ShadowDecorator.draw()` | calls `decoratedShape.draw()` first → goes into `RedBorderDecorator` |
| 2 | `RedBorderDecorator.draw()` | calls `decoratedShape.draw()` first → goes into `Circle` |
| 3 | `Circle.draw()` | prints **"Drawing Circle"** ← innermost wins |
| 4 | back in `RedBorderDecorator.draw()` | now runs `setRedBorder()` → prints **"Adding Red Border"** |
| 5 | back in `ShadowDecorator.draw()` | now runs `addShadow()` → prints **"Adding Shadow"** |

**Rule:** the **innermost shape draws first**, then each wrapper layer adds its decoration on the way out. Order of wrapping = order of layers from inside to outside.

If you flip the wrapping order:

```java
Shape alsoFancy = new RedBorderDecorator(
                      new ShadowDecorator(
                          new Circle()));
alsoFancy.draw();
// → Drawing Circle
// → Adding Shadow
// → Adding Red Border         ← now the border is on the OUTSIDE
```

Same components, different order, different visual result — exactly like real-world layering (you don't put icing under the cake).

### 22.3 UML Diagram

```
                ┌────────────────────────┐
                │     <<interface>>       │
                │       Shape             │  ← Component (the contract)
                │   + draw()              │
                └──────────△──────────────┘
                           │
                ┌──────────┴──────┬─────────────────┐
                │                 │                 │
        ┌───────┴────────┐ ┌─────┴────────┐  ┌─────┴────────────┐
        │     Circle     │ │   Rectangle   │  │ ShapeDecorator    │ ← ABSTRACT decorator
        │  + draw()      │ │   + draw()    │  │  - decoratedShape │   (HAS-A & IS-A Shape)
        └────────────────┘ └───────────────┘  │  + draw() {       │
                                              │     decoratedShape│
                                              │       .draw();    │
                                              │  }                │
                                              └────────△──────────┘
                                                       │
                                              ┌────────┴───────────────┐
                                              │  RedBorderDecorator    │  ← Concrete decorator
                                              │  + draw() {            │
                                              │     super.draw();      │
                                              │     setRedBorder();    │
                                              │  }                     │
                                              └────────────────────────┘
```

### 22.4 Address Common Misconceptions

| Question | Answer |
|---|---|
| Is Decorator the same as Adapter? | No. Adapter changes the **interface** to fit a different one. Decorator keeps the **same interface** but adds **new behavior**. |
| Why do decorators implement the same interface as the wrapped object? | So you can use a decorated object **anywhere** the original could go (polymorphism). The client doesn't need to know it's been decorated. |
| Why does the wrapped object live inside the decorator? | So we can **delegate** (`decoratedShape.draw()`) and stack behavior — without modifying the original class. |
| Can I stack decorators in any order? | Yes — that's the power. But order can affect output (border-then-shadow vs shadow-then-border). |
| Why is the wrapped Shape `protected`? | So subclasses (concrete decorators) can access it. Could also be private with a getter. |
| Decorator vs Inheritance? | Inheritance is **static** (decided at compile time). Decorator is **dynamic** (decided at runtime — you can wrap differently each time). |

### 22.5 Trade-offs

✅ **Pros:**
- **Open-Closed Principle compliant** — add behavior without modifying existing classes.
- **Compositional flexibility** — combine N decorators in any order, infinite variations.
- **Single Responsibility** — each decorator does one thing.
- **Runtime decisions** — you choose decorations based on user input, config, etc.

⚠️ **Cons:**
- **Complex object graphs** — debugging is harder when you have a chain of 5 wrappers.
- **Identity issues** — `wrappedShape != originalShape` (same content, different object); `equals()` may behave unexpectedly.
- **Many small classes** — each decoration is its own class.
- **Order matters** — decorating in a different order can give different results, which can be subtle.

### 22.6 Real-World Industry Examples

#### 22.6.0 Quick list — where you'll spot Decorator in the wild

1. **`java.io` streams** — the canonical example (deep dive in §22.6.2 below). `BufferedReader` wraps `InputStreamReader` wraps `FileInputStream`. Each layer adds buffering, charset conversion, file access.
2. **Text formatting** — Markdown renderers, ANSI terminal codes, Slack/Discord, HTML rich text (deep dive in §22.6.1 below).
3. **Spring `@Transactional`** — wraps your method with transaction begin/commit/rollback (uses dynamic decorators / proxies).
4. **HTTP filters/middleware** — Spring's `OncePerRequestFilter`, Servlet `Filter` chain, Express.js middleware — each adds a behavior (auth, logging, compression).
5. **Java Collections** — `Collections.unmodifiableList(...)`, `Collections.synchronizedList(...)` — wrap a list to add immutability or thread-safety without changing it.
6. **GUI libraries (Swing)** — `JScrollPane` decorates any component with scrollbars.

The Shape example in §22.2 taught you the **mechanics**. The two deep dives below teach you **recognition** — *"oh, that thing I write every day at work is actually Decorator."*

---

#### 22.6.1 Deep Dive #1 — Text Formatting (the cleanest real-world fit)

**Where you've seen this:** Markdown editors (`**bold**` → `<b>bold</b>`), ANSI terminal colors (`\033[1m...\033[0m`), Slack/Discord rich text, log4j formatting patterns, React/Vue text components.

##### The 4-piece setup — same shape as our Shape example

```java
// ── 1. Component (the contract) ──
public interface Text {
    String render();   // returns the formatted output
    int length();      // returns RAW character count (no formatting)
}

// ── 2. Concrete component (the "plain cake") ──
public class PlainText implements Text {
    private final String content;
    public PlainText(String content) { this.content = content; }

    @Override public String render() { return content; }
    @Override public int length()    { return content.length(); }
}

// ── 3. Abstract decorator (the "wrapper template") ──
public abstract class TextDecorator implements Text {
    protected Text wrapped;
    public TextDecorator(Text wrapped) { this.wrapped = wrapped; }

    @Override public String render() { return wrapped.render(); }   // pass-through
    @Override public int length()    { return wrapped.length(); }   // pass-through
}
```

> **🔑 Notice the abstract class earning its keep:** *both* `render()` and `length()` default to pass-through. Concrete decorators below override `render()` but **let `length()` inherit untouched** — exactly the pain point we discussed in §22.2 Step 3.

##### The concrete decorators

```java
public class Bold extends TextDecorator {
    public Bold(Text t) { super(t); }

    @Override
    public String render() {
        return "<b>" + super.render() + "</b>";
    }
    // length() inherited — formatting tags don't change raw length
}

public class Italic extends TextDecorator {
    public Italic(Text t) { super(t); }

    @Override
    public String render() {
        return "<i>" + super.render() + "</i>";
    }
}

public class Underline extends TextDecorator {
    public Underline(Text t) { super(t); }

    @Override
    public String render() {
        return "<u>" + super.render() + "</u>";
    }
}

// A "parameterized" decorator — has its OWN state (the color string)
public class Color extends TextDecorator {
    private final String color;

    public Color(Text t, String color) {
        super(t);
        this.color = color;
    }

    @Override
    public String render() {
        return "<span style=\"color:" + color + "\">" + super.render() + "</span>";
    }
}
```

> **Java fundamental — parameterized decorators:** `Color` shows that decorators can carry **their own constructor params** beyond the wrapped object. You'll see this all over real code (e.g., `BufferedReader(reader, bufferSize)`).

##### Usage — stacking for an alert message

```java
public class TextDecoratorDemo {
    public static void main(String[] args) {
        Text msg = new Color(
                       new Underline(
                           new Italic(
                               new Bold(
                                   new PlainText("Critical Alert!")))),
                       "red");

        System.out.println(msg.render());
        // → <span style="color:red"><u><i><b>Critical Alert!</b></i></u></span>

        System.out.println(msg.length());
        // → 15   (just "Critical Alert!" — formatting tags don't change raw length)
    }
}
```

##### Why this example beats Shape for "real-world feel"

| Concept | Shape demo shows | Text demo shows additionally |
|---|---|---|
| Stacking | `super.draw()` order | **Output you can SEE**: nested HTML tags |
| Multi-method abstract class | Hypothetical (`Shape` only has `draw()`) | **Real**: `length()` is genuinely a pass-through; only `render()` is decorated |
| Order matters | Subtle (border-then-shadow vs reverse) | **Concrete**: `<b><i>` vs `<i><b>` — visible structural difference |
| Parameterized decorator | Not shown | `Color(text, "red")` — has its own state |
| Industry parallel | Toy | **Markdown, ANSI, JSX, log4j** — daily code |

##### Trace — how `msg.render()` unwinds (good interview follow-up)

| Step | Whose `render()` runs | What it returns |
|---|---|---|
| 1 | `Color.render()` | calls `super.render()` first → goes into `Underline` |
| 2 | `Underline.render()` | calls `super.render()` first → goes into `Italic` |
| 3 | `Italic.render()` | calls `super.render()` first → goes into `Bold` |
| 4 | `Bold.render()` | calls `super.render()` first → goes into `PlainText` |
| 5 | `PlainText.render()` | returns `"Critical Alert!"` ← innermost wins |
| 6 | back in `Bold` | wraps → `"<b>Critical Alert!</b>"` |
| 7 | back in `Italic` | wraps → `"<i><b>Critical Alert!</b></i>"` |
| 8 | back in `Underline` | wraps → `"<u><i><b>Critical Alert!</b></i></u>"` |
| 9 | back in `Color` | wraps → `"<span style=\"color:red\">...</span>"` |

Same Russian-doll unwinding as the Shape example, but now the output **literally shows you the layers**.

---

#### 22.6.2 Deep Dive #2 — `java.io` Streams (the most famous Java Decorator)

This is the **highest-value recognition example** for Java interviews. You've written this code 100 times without realizing it's the Decorator pattern in action:

```java
BufferedReader br = new BufferedReader(             // ← decorator 2
                       new InputStreamReader(        // ← decorator 1
                           new FileInputStream("data.txt")));   // ← concrete component
```

##### Map it to the pattern

| `java.io` class | Role | What it does |
|---|---|---|
| `InputStream` | **Component** (abstract) | Defines `read()` contract |
| `FileInputStream` | **Concrete component** | Reads raw bytes from disk |
| `FilterInputStream` | **Abstract decorator** | Holds a wrapped `InputStream`, defaults all methods to pass-through |
| `BufferedInputStream`, `DataInputStream`, `GZIPInputStream`, `CipherInputStream` | **Concrete decorators** | Each adds one capability (buffering, primitives, decompression, decryption) |

Notice — JDK uses **the exact same 4-piece structure** we used for `Shape` and `Text`. This isn't coincidence; it's the pattern.

##### Look — the JDK source has our exact `TextDecorator`

Here's `FilterInputStream` from the JDK (slightly trimmed):

```java
public class FilterInputStream extends InputStream {
    protected volatile InputStream in;            // ← the wrapped stream

    protected FilterInputStream(InputStream in) {
        this.in = in;
    }

    @Override
    public int read() throws IOException {
        return in.read();                         // pass-through
    }

    @Override
    public int read(byte[] b, int off, int len) throws IOException {
        return in.read(b, off, len);              // pass-through
    }

    @Override public long skip(long n)        { return in.skip(n); }       // pass-through
    @Override public int available()          { return in.available(); }   // pass-through
    @Override public void close()             { in.close(); }              // pass-through
    @Override public synchronized void mark(int r)   { in.mark(r); }       // pass-through
    @Override public synchronized void reset()       { in.reset(); }       // pass-through
    @Override public boolean markSupported()         { return in.markSupported(); } // pass-through
}
```

That is **literally** our `TextDecorator` template — the field, the constructor, the pass-through default for every interface method. Compare:

| Our code | JDK code |
|---|---|
| `protected Text wrapped;` | `protected volatile InputStream in;` |
| `public TextDecorator(Text wrapped) { this.wrapped = wrapped; }` | `protected FilterInputStream(InputStream in) { this.in = in; }` |
| `public String render() { return wrapped.render(); }` | `public int read() { return in.read(); }` |
| (8 methods to pass through if `Text` had them) | 8 methods passed through |

The only difference: JDK adds `volatile` for thread-safety and uses `protected` constructor (so only subclasses instantiate it).

##### Concrete decorator from the JDK — `BufferedInputStream`

```java
public class BufferedInputStream extends FilterInputStream {
    protected byte[] buf;            // ← its OWN state (the buffer)
    protected int pos, count;

    public BufferedInputStream(InputStream in) {
        super(in);                    // ← exactly like Bold(Text t) { super(t); }
        this.buf = new byte[8192];
    }

    @Override
    public synchronized int read() throws IOException {
        if (pos >= count) {
            fill();                   // refill buffer from underlying stream
            if (pos >= count) return -1;
        }
        return buf[pos++] & 0xff;
    }
    // ... overrides only the read() methods to add buffering ...
    // ... close(), available(), mark(), reset() inherited from FilterInputStream ...
}
```

This is the **same shape** as our `Bold extends TextDecorator` — overrides only the method it decorates (`read()`), inherits everything else. Perfect application of the abstract-class wisdom from §22.2 Step 3.

##### Production-grade stack — the WHY of stacking

Read a **gzipped, encrypted, line-oriented log file**:

```java
BufferedReader reader = new BufferedReader(             // ← provides readLine()
    new InputStreamReader(                              // ← bytes → chars (UTF-8)
        new GZIPInputStream(                            // ← decompress
            new CipherInputStream(                      // ← decrypt
                new FileInputStream("logs.gz.enc"),     // ← raw bytes from disk
                aesCipher))));

String line;
while ((line = reader.readLine()) != null) {
    process(line);
}
```

5 layers. Each independent. **Order matters** — you must decrypt before decompressing (the disk has encrypted gzipped bytes; you can't decompress encrypted data). Sound familiar? Exactly the `<b><i>` vs `<i><b>` discussion from text formatting.

##### Output side — same pattern, mirrored

| Output side | Mirror role |
|---|---|
| `OutputStream` | Component |
| `FileOutputStream` | Concrete component (writes raw bytes) |
| `FilterOutputStream` | Abstract decorator |
| `BufferedOutputStream`, `GZIPOutputStream`, `CipherOutputStream`, `DataOutputStream` | Concrete decorators |

##### The SDE3 "aha" sentence

> **Whenever you write `new X(new Y(new Z(...)))` in `java.io`, you're using the Decorator pattern.** The chained constructors are decorators wrapping each other; each layer adds a capability without modifying the underlying stream. This is the canonical Java example interviewers expect you to recognize.

---

#### 22.6.3 Quick mention — Java Collections wrappers

```java
List<String> list = new ArrayList<>();
List<String> safe = Collections.synchronizedList(list);     // adds thread-safety
List<String> ro   = Collections.unmodifiableList(list);     // adds read-only
List<String> both = Collections.unmodifiableList(
                        Collections.synchronizedList(list)); // stack them!
```

Each `Collections.xxxList(...)` returns a wrapper that **implements `List`** and **wraps the original list**, intercepting calls to add behavior (lock/throw `UnsupportedOperationException`). Same pattern, slightly hidden by the static factory method.

### 22.7 Decorator — Interview Cheat Sheet

| Q | A |
|---|---|
| When use Decorator? | When you want to add behavior to objects without subclassing, and want the flexibility to combine behaviors at runtime. |
| Difference vs Inheritance? | Inheritance is compile-time/static; Decorator is runtime/dynamic. Decorator avoids class explosion. |
| Difference vs Adapter? | Adapter converts one interface to another. Decorator keeps the same interface and adds responsibilities. |
| Why "abstract" decorator? | Provides the wrapping/delegation skeleton; concrete subclasses add specific decorations on top. Centralizes the field, constructor, and pass-through methods so concrete decorators only override what they enhance. |
| Real Java example? | `new BufferedReader(new InputStreamReader(new FileInputStream(...)))` — three decorators stacked. The abstract decorator is `FilterInputStream` / `FilterReader`. Also `Collections.unmodifiableList()`, Spring `@Transactional`. |
| How does it relate to the Text/HTML formatting you've used? | Markdown → HTML, ANSI terminal codes, JSX rich text — same pattern. Each formatter wraps the inner text and adds tags around it. See §22.6.1. |
| Order of stacking — does it matter? | Yes. `<b><i>X</i></b>` ≠ `<i><b>X</b></i>` structurally. In `java.io`, you must decrypt before decompressing — wrong order = broken stream. |

### 22.8 TL;DR

> **Decorator = layered icing.** Wrap an object in another object that implements the same interface. The wrapper delegates to the wrapped object, then adds its own behavior. Stack as many wrappers as you want — combinations are infinite without class explosion.

---

## 23. Flyweight Design Pattern

> **Category:** Structural
>
> **Friendly definition (no jargon):** When you need to make A LOT of similar objects, **make ONE master copy and reuse it everywhere.** Each user just tells the master "where to apply me" or "how to use me." You save a ton of memory because you're not creating thousands of duplicates.
>
> **One-line:** *one shared copy, many reuses.*
>
> **Formal definition (for interviews — come back to this LATER):** Use sharing to support large numbers of fine-grained objects efficiently.
>
> **Lecture quote:** *"Minimize memory use by sharing objects that are similar."*

### 23.1 The Problem It Solves — From Scratch

#### Easy analogy #1 — the rubber stamp 🟥

Picture an office accountant approving invoices.

```
   ┌──────────┐                    ┌─────────┐
   │   PAID   │  →  stamp page 1   │ Invoice │
   │  ▓▓▓▓▓   │  →  stamp page 2   │   #001   │
   │  ▓ XX ▓  │  →  stamp page 3   └─────────┘
   │  ▓▓▓▓▓   │  →  ... 500 stamps today
   └──────────┘
   ONE rubber stamp                500 invoices stamped
```

How many rubber stamps does the accountant own? **Just one.** They use that one stamp 500 times. What's different per use? Just the **paper** they stamp on (and maybe ink color or position).

If they bought 500 separate stamps, that would be wasteful. Same idea applies in software: **why create 500 identical objects when one shared object can serve all 500 uses?**

That's the entire Flyweight pattern. **One stamp, 500 uses.**

#### Easy analogy #2 — WhatsApp emojis 💬

When you type ❤️❤️❤️❤️❤️, does WhatsApp store 5 separate heart images on your phone?

**No.** WhatsApp has **ONE** heart image baked into the app. Every time you send a heart, the app just *draws* that ONE image at a different position in the message.

| What's stored once (shared) | What's per-use (different each time) |
|---|---|
| The heart image (the shape, the red color) | Where in the message it appears, font size |

If 1 billion users have 100 hearts on screen at any moment = 100 billion hearts visible. **Number of heart images actually stored?** Still **just one** per app install. That's a billion-to-one memory win.

#### Now translate to code — drawing 10,000 red and blue circles

Suppose a drawing app needs to draw 10,000 RED circles and 10,000 BLUE circles. Each circle has:
- A **color** (`"Red"` or `"Blue"`)
- A **radius** (let's say always 10)
- A **position** (`x, y`) — different for every circle

#### The wasteful way

```java
// ❌ Bad: every circle stores everything itself
class Circle {
    String color;    // "Red" or "Blue"
    int radius;      // always 10
    int x, y;        // changes per circle
}

for (int i = 0; i < 10000; i++) {
    new Circle("Red",  10, i, i * 2);     // ← brand new object every time
    new Circle("Blue", 10, i, i * 2);     // ← brand new object every time
}
// → 20,000 separate objects in memory
// → "Red" / "Blue" / 10 are duplicated 10,000 times each
```

Notice the waste: 10,000 red circles all carry their own copy of `"Red"` and `radius = 10`. Identical info, repeated 10,000 times.

#### The Flyweight idea (in plain English first)

Look at what's repeated vs. what's unique:

| In all red circles | "Red" — same. Radius 10 — same. | ← repeated stuff = make ONE master copy |
| In each red circle | (x, y) position — different | ← per-use stuff = pass it in when needed |

So the plan is:
1. Make **one** "Red Circle template" (a single object holding `color="Red"`).
2. Make **one** "Blue Circle template".
3. Whenever we need to draw a circle, ask the template to draw itself **at a given (x, y)** — we pass position to its `draw()` method.

Result: only **2 Circle objects** in memory total, even if we draw 20,000 times. Same correctness, 10,000× less memory.

### 23.2 Code Walkthrough — Step by Step

The whole pattern needs **3 pieces**:

| Piece | What it is in plain English | Maps to in our code |
|---|---|---|
| 1 | The contract / shared interface | `Shape` interface |
| 2 | The "master copy" object that stores the shared stuff | `Circle` class |
| 3 | A "warehouse" that hands out the master copies (creates one if missing, reuses if present) | `ShapeCache` class |

Let's build them.

#### Step 1 — The contract (`Shape` interface)

```java
public interface Shape {
    void draw(int x, int y);    // ← position is PASSED IN, not stored
}
```

> **Look at this carefully:** `draw()` takes `x` and `y` as **arguments**. We do NOT store the position inside the Circle. That's the entire Flyweight trick — keep what's shared inside the object, take what's per-use as parameters.

#### Step 2 — The master copy (`Circle`)

```java
public class Circle implements Shape {
    private final String color;     // ← the SHARED stuff (one Circle per color)

    public Circle(String color) {
        this.color = color;
    }

    @Override
    public void draw(int x, int y) {
        System.out.println("Drawing " + color + " Circle at (" + x + "," + y + ")");
    }
}
```

In words: a Circle holds only its **color** (the part that's the same for all red circles). When we want to draw it somewhere, we pass that "somewhere" as `(x, y)`.

> **Why `final color`?** Because this Circle will be SHARED across thousands of uses. If anyone could change `color` later, all 10,000 callers would suddenly see a different color. **Make shared things immutable.**

#### Step 3 — The warehouse (`ShapeCache`)

This is where the Flyweight magic happens. The warehouse keeps a `HashMap` of master copies. When someone asks for a "Red Circle":
- If we already made one before → hand them back the **same** one.
- If this is the first time → create one, store it, hand it back.

```java
import java.util.HashMap;

public class ShapeCache {
    private static final HashMap<String, Shape> circleMap = new HashMap<>();

    public static Shape getCircle(String color) {
        Shape circle = circleMap.get(color);

        if (circle == null) {                                  // first time?
            circle = new Circle(color);                         // make one
            circleMap.put(color, circle);                       // remember it
            System.out.println("Creating a new " + color + " Circle");
        }
        return circle;                                          // hand back the same one
    }
}
```

That's it. The whole "warehouse" is a HashMap lookup with auto-create. Simple.

> **Why is this needed?** Because if clients used `new Circle("Red")` directly, they'd each create their own Red Circle — duplicates everywhere. The warehouse forces "one Red, shared by all."

#### Step 4 — Use it (the client)

```java
public class FlyweightDemo {
    public static void main(String[] args) {
        String[] colors = {"Red", "Green", "Blue", "Red", "Green"};

        for (int i = 0; i < colors.length; i++) {
            Shape circle = ShapeCache.getCircle(colors[i]);  // ask warehouse
            circle.draw(i * 10, i * 20);                      // pass position
        }
    }
}
```

Output:
```
Creating a new Red Circle              ← first time we ever ask for Red
Drawing Red Circle at (0, 0)
Creating a new Green Circle            ← first Green
Drawing Green Circle at (10, 20)
Creating a new Blue Circle             ← first Blue
Drawing Blue Circle at (20, 40)
Drawing Red Circle at (30, 60)         ← Red asked for AGAIN — REUSED, no "Creating"!
Drawing Green Circle at (40, 80)       ← Green asked AGAIN — REUSED
```

**Total Circle objects created in memory: 3** (one Red, one Green, one Blue).
**Total Circle drawings: 5.**

Scale it up: 1 million draws with 3 unique colors? **Still just 3 objects in memory.** That's the win.

#### The full picture in your head

```
   ShapeCache (warehouse)
   ┌─────────────────────────┐
   │  "Red"   → ●  ◄─────────┼─── ALL red circles point to the same one
   │  "Green" → ●  ◄─────────┼─── ALL green circles point to the same one
   │  "Blue"  → ●  ◄─────────┼─── ALL blue circles point to the same one
   └─────────────────────────┘
                ▲
                │ getCircle(color)
                │
            [client]
              │
              │ circle.draw(x, y)   ← per-use info passed each time
              ▼
           (drawing happens at the given position)
```

**Three master copies in the warehouse, drawn at thousands of different positions.**

### 23.3 UML Diagram

```
                ┌───────────────────────────┐
                │      <<interface>>         │
                │         Shape              │
                │   + draw(int x, int y)     │   ← extrinsic state passed in
                └──────────△─────────────────┘
                           ╎ realizes (dashed + hollow triangle)
                ┌──────────┴───────────┐
                │       Circle          │   ← Concrete Flyweight
                │   - color: String     │   ← intrinsic state (shared)
                │   + draw(x, y)        │
                └───────────────────────┘
                           ▲
                           │ hands out shared instances
                           │ (solid + open arrow ▲)
                ┌──────────┴───────────────────────────┐
                │           ShapeCache                  │   ← Flyweight Factory
                │   - circleMap: Map<String, Shape>     │
                │   + getCircle(color): Shape           │
                └───────────────────────────────────────┘
                           ▲
                           │ uses (solid + open arrow ▲)
                ┌──────────┴───────────┐
                │        Client         │
                │   (calls ShapeCache,  │
                │    passes x,y to      │
                │    draw)              │
                └───────────────────────┘
```

### 23.4 Now meet the "official" interview terms (don't panic — you already get it)

You already understand the pattern. These are just the fancy names interviewers use.

#### Plain English ↔ Interview vocabulary

| What we said in plain English | What the interview calls it | In our Circle example |
|---|---|---|
| "The shared stuff stored inside the master copy" | **Intrinsic state** | `color = "Red"` |
| "The per-use stuff passed in as arguments" | **Extrinsic state** | `x`, `y` (position) |
| "The master copy itself" | **Concrete Flyweight** | The `Circle` object |
| "The warehouse / cache" | **Flyweight Factory** | `ShapeCache` |
| "The contract" | **Flyweight interface** | `Shape` |

That's it. The official names are scary; the ideas are simple.

#### How to remember which is which

| Word | Plain meaning | Memory hook |
|---|---|---|
| **In**trinsic | "**In**side" the object — built-in, shared | "**IN**built" |
| **Ex**trinsic | "**Ex**ternal" — passed in from outside per call | "**EX**ternal, **EX**tra info" |

Or even simpler:

> **Intrinsic = "always the same."**
> **Extrinsic = "changes each time."**

That's the whole vocabulary. You can stop worrying about it now.

#### How to find them in any problem (the bucket test)

Pick any property of an object you're modeling. Ask:

> *"If I use this object 1000 times, will this property be the same in all 1000 uses?"*

| Answer | Bucket |
|---|---|
| ✅ "Yes, always the same" | **Intrinsic** — store inside the master copy (share it) |
| ❌ "No, changes per use" | **Extrinsic** — pass in as method argument (don't store) |

Apply it to a few examples:

| Object | Intrinsic ("same for all uses") | Extrinsic ("changes per use") |
|---|---|---|
| Circles in a drawing app | color, radius | (x, y) position |
| Bullets in a video game | bullet model, damage value | current position, velocity |
| Trees in a forest scene | tree species, mesh, texture | position (x, y, z), height variation |
| Chess pieces | piece type (rook, pawn), how it moves | current square it sits on |
| Letters drawn on screen | the shape of the letter (e.g., shape of 'A'), font face | position, font size, color of THIS one |
| Emojis in WhatsApp | the emoji image | position in the message |

Once you can sort properties into the two buckets, you can apply Flyweight anywhere.

> **One footnote on terms you might hear elsewhere:** in some text-editor articles, the shape of a letter is called a *"glyph"* — that's just a fancy word for "the visual shape of a character." So **glyph = the intrinsic part of a character**. Don't let anyone use the word at you to make it sound complicated.

### 23.5 Common Confusions (Q&A)

| Question | Answer |
|---|---|
| Why is `color` stored inside Circle, but `x, y` is passed in? | Because `color` is the **same** for all 10,000 red circles (only 2 unique colors total). `x, y` is **different** for every single circle. Storing `x, y` would force one object per position → no sharing → defeats the whole point. |
| What if I just store `x, y` inside Circle anyway? | Then 10,000 red circles need 10,000 distinct Circle objects (each has its own position). You're back to the wasteful version with no Flyweight benefit. |
| Why do we need the warehouse (`ShapeCache`)? Can't clients do `new Circle("Red")` themselves? | They could, but then nothing forces them to share. Each `new Circle("Red")` creates a duplicate. The warehouse enforces "one Red, shared by everyone." |
| Is Flyweight just "object pooling"? | Related, not the same. Object pooling = reusing objects (like DB connections), and you usually return them after use. Flyweight = sharing **immutable** objects forever, based on what's identical between them. |
| Should the master copy (the flyweight) be immutable? | **Yes.** Since it's shared, if anyone modified it, EVERYONE using it would see the change. Always mark shared fields `final`. |
| When should I NOT use Flyweight? | When you only have a handful of unique objects (sharing saves nothing), or when objects aren't big enough to matter. Don't optimize for memory you weren't going to waste. |

### 23.6 Trade-offs

✅ **Pros:**
- **Massive memory savings** when you have many objects with shared data.
- **Faster object creation** — reuse existing instead of allocating new.
- **Cache locality** improves performance in some cases.

⚠️ **Cons:**
- **More complex code** — splitting state into intrinsic/extrinsic adds cognitive overhead.
- **Extrinsic state must be passed everywhere** — can clutter method signatures.
- **Thread-safety care** — shared flyweights mutated by multiple threads = chaos. Keep them immutable.
- **Premature optimization risk** — only worth it for thousands+ objects.

### 23.7 Real-World Industry Examples

These are all **just rubber stamps** — one shared master, many reuses.

1. **`Integer.valueOf(int)`** — Java pre-creates Integer objects for `-128` to `127` and reuses them. So:
   ```java
   Integer.valueOf(100) == Integer.valueOf(100)   // true — SAME shared object
   Integer.valueOf(200) == Integer.valueOf(200)   // false — outside the cache range, fresh objects
   ```
   The cached integers (-128..127) are flyweights. The "shared" part is the number value; there's no per-use info, so the master is the entire object.

2. **`Boolean.TRUE` and `Boolean.FALSE`** — Java has exactly **two** Boolean objects in the entire JVM. `Boolean.valueOf(true)` always returns the same one. The most extreme flyweight there is.

3. **String literal pool** — when you write `"hello"` in two places, Java reuses the **same** String object. That's why `"hello" == "hello"` is true. The string text is the shared part; nothing is per-use because Strings are immutable.

4. **Game development** — picture 10,000 trees in a forest scene. Each tree has a mesh (3D model) and texture, plus a position. The mesh and texture are shared (flyweight); only the position differs per tree. Same for bullets, particles, NPCs.

5. **Text editors / IDEs** — every visible 'A' on screen shares the same letter shape (officially called a "glyph"). Only the position, color, and font size are per-character. Without this, a 1-million-character file would need 1 million letter-shape copies in memory.

6. **Map tiles in Google Maps / OSM** — tiles at the same zoom level are reused across millions of users. The tile bitmap is the master; your screen position when viewing it is per-use.

### 23.8 Flyweight — Interview Cheat Sheet

| Q | A (in plain English first) |
|---|---|
| When should I use Flyweight? | When you'd otherwise create thousands of objects with mostly the same info. One master, many reuses. |
| What is "intrinsic state"? | The same-for-everyone stuff — stored inside the master copy (e.g. `color`). |
| What is "extrinsic state"? | The per-use stuff — passed in as method arguments (e.g. `x`, `y`). |
| Why do we need a factory/warehouse? | To force everyone to share. Without it, `new Circle("Red")` creates duplicates. |
| Common real-world Java example? | `Integer.valueOf()` reuses cached Integers for -128..127. Also `Boolean.TRUE`/`FALSE`, String literal pool. |
| Should flyweights be immutable? | Yes. Shared object + mutation = chaos. Always `final` the shared fields. |
| What's the elevator pitch? | "Make ONE master, reuse it everywhere; pass per-use info as arguments." |

### 23.9 TL;DR — the rubber stamp pattern

> **Flyweight = one rubber stamp, many uses.**
>
> 1. Find what's the same across all uses → store it ONCE in a shared "master copy".
> 2. Find what's different per use → pass it as method arguments.
> 3. Put the master copies in a warehouse (factory) so everyone gets the SAME copy when they ask.
>
> Result: thousands of "objects" backed by a handful of master copies. **Massive memory savings.**
>
> Fancy interview names: "shared stuff" = **intrinsic state**, "per-use stuff" = **extrinsic state**, "warehouse" = **flyweight factory**, "master copy" = **flyweight**. That's the whole vocabulary.

---

## 24. Behavioral Design Patterns — Introduction

> **Friendly definition:** Behavioral patterns are about **how objects talk to each other and divide work**. They define communication, responsibilities, and algorithms — answering questions like "who decides what?", "how do objects react to changes?", "how is work passed around?".
>
> **Formal definition (for reference):** Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects. They describe not just patterns of objects or classes but also the patterns of communication between them.

### 24.1 The 4 Behavioral Patterns We'll Cover

| # | Pattern | One-liner |
|---|---|---|
| 1 | **Strategy** | Encapsulate a family of algorithms; swap which one to use at runtime. |
| 2 | **Observer** | When one object changes, automatically notify many dependents. |
| 3 | **Chain of Responsibility** | Pass a request through a chain of handlers; each decides to handle or forward. |
| 4 | **Template Method** | Define an algorithm's skeleton in a base class; let subclasses fill in steps. |

### 24.2 Mental Model — How They Differ

```
STRATEGY            OBSERVER             CHAIN OF RESP          TEMPLATE METHOD
────────            ────────             ─────────────          ───────────────
Choose which        State change         Pass request           Algorithm skeleton
algorithm to run    → notify all         hop-by-hop             with overridable
                    listeners            until handled          steps
                                  
[client]            [pub] ●            [Handler1]              [skeleton: A→B→C→D]
   │                  │ \              [Handler2]                A,D are common
   ▼                  ●  ●             [Handler3]                B,C subclass-fills
[Strategy: UPI]    [obs][obs]           [Handler4]
[Strategy: Card]
[Strategy: NB]
```

The 4 themes:
- **Strategy** → "I have many ways to do the same thing — let me pick one."
- **Observer** → "I changed; tell whoever cares."
- **Chain of Responsibility** → "Try handler 1, if it can't, try handler 2, ..."
- **Template Method** → "I'll define the algorithm shape; subclasses fill in the gaps."

---

## 25. Strategy Design Pattern

> **Category:** Behavioral
>
> **Friendly definition:** When you have several different ways to do the same thing (different algorithms), **Strategy** wraps each algorithm in its own class. The client picks which one to use **at runtime** — and can swap it out anytime without modifying anything else.
>
> **Formal definition (for reference):** Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
>
> **Lecture quote:** *"Family of algorithms."*

### 25.1 The Problem It Solves — From Scratch

#### Real-world analogy: choosing how to commute

You need to travel from home to office. There are multiple **algorithms** (strategies) to do the same task:
- Car
- Bus
- Bike
- Walk

The **goal is the same** (get to office). The **details are different** (cost, time, environmental impact).

You shouldn't have to write *one giant function* with a giant `if/else` to handle every transport. Instead, treat each transport as a **swappable module**. Today you take the car; tomorrow the bike. The "commute" function doesn't change — only the strategy does.

#### Translate to code: Fintech payment system

The lecture's exact example. A fintech app supports many payment methods:
- UPI
- Debit/Credit Card
- Net Banking

The **goal is the same** (process a payment). The **details differ** (each payment provider has its own logic).

#### The bad way — giant `if/else` ladder

```java
public class PaymentService {
    public void processPayment(String paymentType, int amount) {
        if (paymentType.equals("UPI")) {
            // UPI logic — call UPI APIs, etc.
            System.out.println("Pay " + amount + " using UPI");
        } else if (paymentType.equals("CARD")) {
            // Card logic — call Visa/Mastercard APIs
            System.out.println("Pay " + amount + " using Card");
        } else if (paymentType.equals("NETBANKING")) {
            // NetBanking logic
            System.out.println("Pay " + amount + " using NetBanking");
        } else {
            throw new IllegalArgumentException("Unknown payment type");
        }
    }
}
```

**Why it's bad** (the lecture explicitly calls these out):

1. **OCP violation** — adding PayPal means **modifying** `PaymentService`. The existing class is not "closed for modification."
2. **Scalability issue** — the if/else grows with every new payment method. After 10 methods, this method is 100+ lines.
3. **Single Responsibility violation** — one class now knows about every payment provider's quirks.
4. **Hard to test** — to test UPI, you have to instantiate `PaymentService` and pass `"UPI"` string; you can't easily test UPI logic in isolation.
5. **Magic strings** — `"UPI"`, `"CARD"`, `"NETBANKING"` are typo-prone.

#### The Strategy solution

Make each payment method its own class. They all implement a common interface (`PaymentStrategy`). Inject the chosen strategy into the `PaymentService` at runtime.

### 25.2 Code Walkthrough — Step by Step

#### Step 1 — The Strategy interface

```java
public interface PaymentStrategy {
    void pay(int amount);
}
```

A common contract: every payment method has a `pay(amount)` operation. The interface doesn't care HOW.

#### Step 2 — Concrete Strategies (one per algorithm)

```java
public class UPIPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Pay " + amount + " using UPI");
    }
}

public class CardPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Pay " + amount + " using Card");
    }
}

public class NetBankingPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Pay " + amount + " using NetBanking");
    }
}
```

Each concrete strategy knows ONLY its own logic. To add PayPal: write a new `PayPalPayment` class. Existing classes don't change.

#### Step 3 — The Context (`PaymentService`) — holds and uses the strategy

```java
public class PaymentService {
    private PaymentStrategy strategy;          // ← holds whichever strategy was injected

    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;               // ← caller picks at runtime
    }

    public void processPayment(int amount) {
        System.out.println("Initiating payment");
        strategy.pay(amount);                   // ← delegates to the strategy
        System.out.println("Payment complete");
    }
}
```

`PaymentService` doesn't know HOW the payment is processed — it just delegates to whatever strategy is currently set.

#### Step 4 — The client — pick a strategy at runtime

```java
public class StrategyDemo {
    public static void main(String[] args) {
        PaymentService paymentService = new PaymentService();

        // Pay with UPI
        paymentService.setPaymentStrategy(new UPIPayment());
        paymentService.processPayment(1000);
        // → Initiating payment
        // → Pay 1000 using UPI
        // → Payment complete

        // Switch to Card at runtime — no change to PaymentService!
        paymentService.setPaymentStrategy(new CardPayment());
        paymentService.processPayment(2000);
        // → Initiating payment
        // → Pay 2000 using Card
        // → Payment complete
    }
}
```

The same `PaymentService` instance handled both UPI and Card payments. The strategy is just **swapped** — no `if/else`, no rewriting.

### 25.3 UML Diagram

```
                ┌─────────────────────────────┐
                │    <<interface>>             │
                │    PaymentStrategy           │  ← Strategy interface
                │    + pay(int amount)         │
                └──────────△───────────────────┘
                           ╎ realizes (dashed + hollow triangle)
            ┌──────────────┼─────────────────┐
            ╎              ╎                 ╎
   ┌────────┴───────┐ ┌────┴──────────┐ ┌────┴──────────────┐
   │  UPIPayment    │ │  CardPayment   │ │ NetBankingPayment │  ← Concrete strategies
   │  + pay(amount) │ │  + pay(amount) │ │ + pay(amount)     │
   └────────────────┘ └────────────────┘ └───────────────────┘
                           ▲
                           │ uses (solid line + open arrow ▲)
                           │   PaymentService holds a PaymentStrategy reference
                ┌──────────┴───────────────────┐
                │      PaymentService          │  ← Context
                │   - strategy: PaymentStrategy│
                │   + setPaymentStrategy()     │
                │   + processPayment(amount)   │
                └──────────────────────────────┘
                           ▲
                           │ uses (solid line + open arrow ▲)
                ┌──────────┴───────────────────┐
                │           Client              │
                │   (picks strategy at runtime) │
                └───────────────────────────────┘
```

### 25.4 Address Common Misconceptions

| Question | Answer |
|---|---|
| Difference vs Factory? | Factory **creates** the object you want. Strategy **uses** an algorithm; the strategy can be created via Factory if you wish. They're often combined. |
| Difference vs State pattern? | Both swap behavior at runtime. State changes its strategy **based on internal state transitions**; Strategy is **chosen by the client** explicitly. |
| Can the context have a default strategy? | Yes — set one in the constructor or via setter. |
| Can I pass strategy as a lambda? | Yes! If the strategy interface is a functional interface (one method), you can pass a lambda or method reference instead of a class. Very modern Java. |
| Does Strategy violate OCP? | No — adding a new strategy is a new class. The context doesn't change. ✅ |
| Does the client need to know strategies? | Yes — *some* code has to know which strategy to pick. Often hidden in a Factory based on user input or config. |

### 25.5 Modern Twist — Strategy with Lambdas

Since Java 8, if your strategy interface has just one method (it's a functional interface), you can skip writing concrete classes:

```java
@FunctionalInterface
public interface PaymentStrategy {
    void pay(int amount);
}

// Inline strategies — no class needed!
PaymentService service = new PaymentService();
service.setPaymentStrategy(amount -> System.out.println("UPI: " + amount));
service.processPayment(500);

service.setPaymentStrategy(amount -> System.out.println("Card: " + amount));
service.processPayment(1500);
```

The classic Strategy pattern + lambdas = a powerful, terse modern style.

### 25.6 Trade-offs

✅ **Pros:**
- **Open-Closed Principle** — adding new strategies = adding new classes; existing code untouched.
- **Single Responsibility** — each strategy class does one thing.
- **Testable** — each strategy is isolated and unit-testable.
- **Runtime flexibility** — switch strategies based on user choice, config, A/B test, etc.

⚠️ **Cons:**
- **More classes** — every algorithm becomes a class (or lambda).
- **Client has to know** about strategies (or use a factory to pick).
- **If strategies need shared state**, you have to pass it around as parameters.

### 25.7 Real-World Industry Examples

1. **`java.util.Comparator`** — sorting strategy. `Collections.sort(list, comparator)` accepts a strategy for ordering.
2. **`java.util.concurrent.ThreadPoolExecutor`** — `RejectedExecutionHandler` is a strategy for what to do when the queue is full (`AbortPolicy`, `CallerRunsPolicy`, etc.).
3. **Spring's `PasswordEncoder`** — `BCryptPasswordEncoder`, `Argon2PasswordEncoder`, etc. — strategies for hashing.
4. **Compression libraries** — different compression algorithms (Gzip, LZ4, Snappy) plug in as strategies.
5. **Payment gateways in apps** — UPI, Cards, Wallets, BNPL — each is a strategy.

### 25.8 Strategy — Interview Cheat Sheet

| Q | A |
|---|---|
| When use Strategy? | When you have multiple algorithms for the same task and want to pick at runtime. |
| Difference vs State? | Strategy: client picks. State: object changes its own strategy based on state transitions. |
| Difference vs Template Method? | Template Method uses inheritance and a fixed skeleton; Strategy uses composition and is fully replaceable. |
| Modern Java twist? | Use lambdas for functional-interface strategies — no need for concrete classes. |
| OCP-friendly? | Yes — adding new strategy = new class, no modifications to existing code. |
| Real Java example? | `Comparator`, `RejectedExecutionHandler`, Spring `PasswordEncoder`. |

### 25.9 TL;DR

> **Strategy = pluggable algorithms.** Wrap each algorithm in its own class implementing a common interface. The context holds a reference to a strategy and delegates to it. Swap strategies at runtime without modifying the context or existing strategies. Pairs perfectly with lambdas in modern Java.

---

## 26. Observer Design Pattern

> **Category:** Behavioral
>
> **Friendly definition:** When **one** object changes state and **many** other objects need to react, **Observer** is a "subscribe & notify" pattern. The publisher doesn't know who's listening; it just announces the change. Anyone interested registers as an observer and gets notified automatically.
>
> **Formal definition (for reference):** Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
>
> **Lecture quote:** *"State Change → notify Observers."*

### 26.1 The Problem It Solves — From Scratch

#### Real-world analogy: YouTube channel subscriptions

A YouTube channel (the **publisher**) uploads a new video.
- People who **subscribed** (the **observers**) get notified — push notification, email, in-app.
- People who didn't subscribe — silence.
- The channel doesn't keep a list of names and call each fan personally; it just hits "Publish" and YouTube broadcasts to all subscribers.

When a new subscriber joins → they're added to the list. Unsubscribe → they're removed. The publisher never has to know individuals personally.

That's Observer: a **one-to-many** push notification system.

#### Translate to code: Order Service in an e-commerce app

The lecture's exact example. When an order's status changes ("PLACED" → "SHIPPED" → "DELIVERED"), several services need to react:

```
                      ORDER STATUS CHANGED
                             │
        ┌────────────────┬──┴──┬──────────────┬──────────────┐
        ▼                ▼     ▼              ▼              ▼
  ┌────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
  │ Customer    │  │  Warehouse    │  │  Analytics    │  │  Email/SMS    │
  │ Notification│  │  Service       │  │  System       │  │  Notification │
  └────────────┘  └──────────────┘  └──────────────┘  └──────────────┘
```

All these services need to know "the order changed" — but the OrderService shouldn't know about each one specifically.

#### The bad way — OrderService directly calls each service

```java
public class OrderService {
    private EmailService email = new EmailService();
    private WarehouseService warehouse = new WarehouseService();
    private AnalyticsService analytics = new AnalyticsService();

    public void updateOrderStatus(String orderId, String status) {
        // ... update DB ...

        // Manually notify each service:
        email.sendEmail(orderId, status);
        warehouse.updateInventory(orderId, status);
        analytics.trackChange(orderId, status);
        // What if we add CustomerNotification? Edit this method!
        // What if Email is removed? Edit this method!
    }
}
```

**Why it's bad:**
- **Tight coupling** — OrderService knows about every dependent service.
- **OCP violation** — adding a new dependent (e.g., loyalty points) means editing `updateOrderStatus`.
- **Hard to test** — to test OrderService, you need email, warehouse, analytics all up.
- **No flexibility** — can't disable email at runtime; you'd need to edit code.

#### The Observer solution

OrderService keeps a `List<Observer>`. Anyone interested **subscribes**. When status changes, OrderService calls `notifyObservers(...)` which iterates and calls `update(...)` on each. OrderService never knows the concrete observers — only that they implement `OrderObserver`.

### 26.2 Code Walkthrough — Step by Step

#### Step 1 — The Observer interface

```java
public interface OrderObserver {
    void update(String orderId, String status);
}
```

A simple contract: every observer has an `update(orderId, status)` method that's called when the publisher's state changes.

#### Step 2 — The Publisher (Subject) — `OrderService`

```java
import java.util.ArrayList;
import java.util.List;

public class OrderService {
    private final List<OrderObserver> observers = new ArrayList<>();

    // ── Subscription management ──
    public void addObserver(OrderObserver observer) {
        observers.add(observer);
    }

    public void removeObserver(OrderObserver observer) {
        observers.remove(observer);
    }

    // ── State-changing operation ──
    public void updateOrderStatus(String orderId, String status) {
        System.out.println("Order status updated to status: " + status);
        notifyObservers(orderId, status);   // tell everyone
    }

    // ── Internal: notify all observers ──
    private void notifyObservers(String orderId, String status) {
        for (OrderObserver observer : observers) {
            observer.update(orderId, status);
        }
    }
}
```

The Publisher has 3 responsibilities:
1. **Manage subscriptions** (`addObserver`, `removeObserver`).
2. **Change state** (`updateOrderStatus`).
3. **Notify** (`notifyObservers`).

#### Step 3 — Concrete Observers — each with their own update logic

```java
public class EmailService implements OrderObserver {
    @Override
    public void update(String orderId, String status) {
        System.out.println("Email sent for order " + orderId + " with status " + status);
    }
}

public class WarehouseService implements OrderObserver {
    @Override
    public void update(String orderId, String status) {
        System.out.println("Warehouse update for order " + orderId + " " + status);
    }
}

public class AnalyticsService implements OrderObserver {
    @Override
    public void update(String orderId, String status) {
        System.out.println("Analytics track for order " + orderId);
    }
}
```

Each observer cares about the same event but reacts differently — the email service sends an email, the warehouse updates inventory, analytics logs metrics.

#### Step 4 — The Client — wire them up and trigger events

```java
public class ObserverDemo {
    public static void main(String[] args) {
        OrderService orderService = new OrderService();

        // Register observers
        orderService.addObserver(new EmailService());
        orderService.addObserver(new WarehouseService());
        orderService.addObserver(new AnalyticsService());

        // Trigger event
        orderService.updateOrderStatus("ORD123", "SHIPPED");
        // Output:
        // → Order status updated to status: SHIPPED
        // → Email sent for order ORD123 with status SHIPPED
        // → Warehouse update for order ORD123 SHIPPED
        // → Analytics track for order ORD123
    }
}
```

To add a new observer (e.g., customer SMS), just write a new class implementing `OrderObserver` and `addObserver()` it. **OrderService doesn't change.** ✅ OCP.

### 26.3 UML Diagram

```
                    ┌──────────────────────────┐
                    │     <<interface>>         │
                    │     OrderObserver         │   ← Observer contract
                    │  + update(id, status)     │
                    └─────────△─────────────────┘
                              ╎ realizes (dashed + hollow triangle)
            ┌─────────────────┼──────────────────┐
            ╎                 ╎                  ╎
   ┌────────┴───────┐ ┌───────┴────────┐ ┌──────┴──────────┐
   │  EmailService   │ │WarehouseService │ │AnalyticsService │
   │  +update(...)   │ │  +update(...)   │ │  +update(...)   │
   └─────────────────┘ └─────────────────┘ └─────────────────┘
                              ◇  *
                              │  aggregation "subscribes to"
                              │  (hollow diamond + solid line)
                              │  OrderService holds List<OrderObserver>,
                              │  but the observers are CREATED OUTSIDE
                              │  (by Client) and PASSED IN via addObserver().
                              │  Each observer has its own lifecycle.
                    ┌─────────┴─┐ 1
                    │       OrderService            │   ← Publisher (Subject)
                    │   - observers: List<…>        │
                    │   + addObserver(o)            │
                    │   + removeObserver(o)         │
                    │   + updateOrderStatus(id, st) │
                    │   - notifyObservers()         │
                    └───────────────────────────────┘
                                ▲
                                │ uses (solid + open arrow ▲)
                    ┌───────────┴───────────────────┐
                    │            Client              │
                    │ (creates, registers, triggers) │
                    └────────────────────────────────┘
```

> **Why aggregation (◇) not plain "uses"?** `OrderService` holds observers as a long-lived field (`private final List<OrderObserver> observers`), but the observers themselves are created by the Client (`new EmailService()`) and **passed in** via `addObserver(o)`. That fits the aggregation rule: container holds, but doesn't own the lifecycle.
>
> **Why aggregation (◇) and not composition (◆)?** Because the observers can exist independently — the same `EmailService` could be subscribed to multiple subjects, and observers continue to exist even after the subject is gone. If `OrderService` did `new EmailService()` internally and never let it escape, THEN it would be composition.
>
> **Multiplicity `1 ──◇ *`** means: one `OrderService` is associated with zero or more observers. That's the textbook Observer relationship.

### 26.4 Push vs Pull (an important variant)

There are two flavors of Observer based on **how data flows**:

| Flavor | Description | Example |
|---|---|---|
| **Push** | Publisher passes data directly to observers via `update(...)` arguments | `update(orderId, status)` ← we used this |
| **Pull** | Publisher only signals "I changed!"; observers pull what they need | `update(this)` and observer calls `subject.getOrderId()` etc. |

**Push** is simpler but couples observers to the data shape. **Pull** is more flexible but makes observers query the subject. The lecture used push.

### 26.5 Common Confusions — explained in plain English

#### Q1 — Is Observer just a fancy name for "event listener"?

**Short answer:** Yes, basically. Anytime "something happens → other things automatically react", that's Observer.

Examples from things you use every day:
- **Click the like button on Instagram** → the count goes up, you get a confetti animation, the person who posted gets a notification. Three different "observers" all reacted to one "like" event.
- **WhatsApp group**: when someone sends a message, every member of the group gets notified. The group is the *publisher*; each member is an *observer*.
- **YouTube subscribe button**: when your favorite channel uploads, every subscriber gets a notification. Same pattern.
- **Notification bell on Gmail/WhatsApp**: a new email/message arrives → your phone lights up, makes a sound, shows a banner. Three observers reacting to one event.

In code, this same idea shows up everywhere — button clicks, form submissions, anything that says "when X happens, do Y." The technical name for the pattern is Observer.

---

#### Q2 — Does `OrderService` know who its observers actually are?

**Short answer:** No — only by their *role*, not their *identity*.

```java
private final List<OrderObserver> observers;   // ← type is the INTERFACE
```

The list is typed as `OrderObserver`. So `OrderService` doesn't know *"this is EmailService"* or *"this is AnalyticsService"*. It only knows *"these are things that promised to implement `OrderObserver`"*.

**Why this matters:** Tomorrow when someone adds `SmsService`, you just register it. `OrderService` doesn't need a single line of change. That's the *Open-Closed Principle* — open for extension, closed for modification.

> **Analogy:** A teacher taking attendance doesn't need to know each student's life story — just their name on the roll. Add a new student? Just add the name to the roll. The "take attendance" routine stays the same.

---

#### Q3 — What happens if one observer is super slow?

**Short answer:** Everyone behind it has to wait.

When `OrderService.notifyObservers()` runs, it calls each observer **one at a time**, in a loop, on the same thread:

```java
for (OrderObserver o : observers) {
    o.update(orderId, status);     // ← runs synchronously, one after another
}
```

So if `EmailService.update()` takes 3 seconds (e.g., calls a slow email API), then `WarehouseService` and `AnalyticsService` are STUCK waiting those 3 seconds before they even start. And the user who triggered the order has been waiting all that time too.

**Fix:** Don't run observers in line — fire each one off in the background so they run in parallel and the publisher can return immediately:

```java
ExecutorService bgPool = Executors.newCachedThreadPool();    // "a pool of workers"

for (OrderObserver o : observers) {
    bgPool.submit(() -> o.update(orderId, status));          // hand off the task to a worker
}
// publisher returns RIGHT AWAY — doesn't wait for any observer to finish
```

`ExecutorService` is just Java's way of saying *"give me a pool of worker threads I can throw tasks at."* Instead of doing the work yourself, you hand each task to a free worker and move on.

> **Analogy:** Imagine a manager who needs to give 10 people the same news.
> - **Bad way (current code):** Walk to each desk one by one. If one person is in a 3-hour meeting, you stand at their door for 3 hours before moving to the next desk.
> - **Good way (the fix):** Send the news to all 10 inboxes at once. Each person reads it whenever they get to it. You're done in 1 second.

---

#### Q4 — What does "modifying the list mid-notification" even mean? When does that happen?

**You're 100% right to push back** — in the basic example, the client adds/removes observers at one point, and notify happens later. They don't overlap. So why does this Q matter?

Because in real Observer code, there's a very common case where an observer **unsubscribes itself from inside its own `update()` method**. Let me show you.

##### The "notify me ONCE" use case

Imagine an observer that only cares about **ONE specific event**:

> *"WhatsApp me when my order is DELIVERED. After that, stop bothering me."*

In code:

```java
public class OneTimeSmsObserver implements OrderObserver {
    private final OrderService source;          // ← it knows who to unsubscribe from

    public OneTimeSmsObserver(OrderService s) { this.source = s; }

    @Override
    public void update(int orderId, String status) {
        if (status.equals("DELIVERED")) {
            sendSms("Your order is here!");
            source.removeObserver(this);        // ← ★ self-unsubscribe HERE
        }
    }
}
```

Now look at what happens during a single notify call:

```java
// Inside OrderService:
public void notifyObservers() {
    for (OrderObserver o : observers) {         // ① iterate the list
        o.update(orderId, "DELIVERED");          // ② calls update()
                                                  //    which calls removeObserver(this)
                                                  //    which MODIFIES the list we're iterating! 💥
    }
}
```

So step ② is **inside** step ①. The publisher is mid-iteration when the observer mutates the list. That's the "mid-notification modification" scenario.

##### Why is that a problem?

A regular `ArrayList` in Java has a built-in safety check: *"if you modify me while someone is iterating through me, throw an error."* The error is called `ConcurrentModificationException` — translation: "you tried to change my contents while I was being walked through." Java does this because the alternative is silent bugs (skipping observers, calling the same one twice, etc.).

So with a plain `ArrayList`, the moment the observer self-unsubscribes, the publisher's loop **crashes**.

##### Two ways to fix it

**Fix A — iterate over a copy of the list:**
```java
for (OrderObserver o : new ArrayList<>(observers)) {    // ← take a snapshot first
    o.update(orderId, status);                           //    iterate the snapshot
}
// The original list can be modified safely during the loop.
```
Simple, easy to understand. The cost: every notify takes one extra `O(n)` copy.

**Fix B — use a special list class: `CopyOnWriteArrayList`:**
```java
private final List<OrderObserver> observers = new CopyOnWriteArrayList<>();
```
This is Java's "list that's safe to modify while being iterated." Internally it **automatically takes a snapshot every time you add or remove**, so anyone already iterating sees the old snapshot — no crash.

The trade-off: every `add`/`remove` is a bit slower (it copies). But iterations are fast. Perfect for "we notify many times, but rarely subscribe/unsubscribe" — which is the typical Observer workload.

> **Analogy:** Imagine you're reading a guest list at a party. If someone walks up and scratches their name off while you're reading, you'll lose your place or skip names. Two fixes:
> - **Fix A:** Photocopy the list first, read the copy. Originals can be edited without disturbing your read.
> - **Fix B:** Use a magic clipboard that always shows you a frozen-in-time copy whenever you start reading. New edits don't disturb your current read.

##### Bonus: when else does this happen?

Beyond self-unsubscribe, the same problem appears in **multi-threaded apps**: one thread is calling `notifyObservers()`, another thread (e.g., a different HTTP request) calls `removeObserver()` at the same moment. Same fix applies. (We'll cover threading in the multi-threading notes — for now, just know that `CopyOnWriteArrayList` covers both cases.)

---

#### Q5 — How is Observer different from "Pub/Sub"?

**Short answer:** Same core idea — *one publisher, many subscribers* — but they differ in **WHO holds the subscriber list and HOW messages are delivered.**

##### Two everyday analogies

**Observer = a WhatsApp group YOU created.**
- You (the publisher) personally keep the list of group members.
- When you want to say something, you go to your group and post — your phone sends the message to each member directly.
- If your phone dies, messages in mid-flight are lost.
- Only works for groups you're in (same app, same network).

**Pub/Sub = posting on Twitter / Instagram.**
- You don't keep any list of followers. Twitter does.
- You just post — Twitter delivers to whoever subscribed to your account.
- If your phone dies, Twitter still has your post. Followers see it whenever they open the app, even days later.
- Anyone in the world can subscribe — they don't have to know you personally.

That "Twitter" in the middle is called a **broker** (or "message broker"). It's a separate server whose only job is to:
1. Accept messages from publishers.
2. Remember who subscribed to what.
3. Deliver messages to subscribers (now or later).

##### The differences side-by-side

| Feature | Observer (WhatsApp group) | Pub/Sub (Twitter) |
|---|---|---|
| Who holds the subscriber list? | The publisher itself (`List<Observer>`) | A separate **broker** (Twitter server) |
| Where does it live? | Inside ONE running program | Across many separate services / machines |
| Who delivers messages? | The publisher, in a loop | The broker |
| What if publisher crashes? | In-flight notifications lost | Broker still has them — delivered later |
| What if subscriber is offline? | Notification is missed | Broker holds it; delivered when subscriber comes back |
| Speed | Microseconds (memory-only) | Milliseconds (network call) |
| Setup effort | None — just code | Need to set up the broker server |

##### When to use which?

- **Observer (WhatsApp group):** all the action is inside one running program. Small subscriber list. You want fast, simple, real-time.
  - Example: a button click handler in your app.
- **Pub/Sub (Twitter):** different services on different machines need to be notified. Want durability (don't lose messages if something crashes). Subscribers might be offline temporarily.
  - Example: when a user places an order on your e-commerce site, you need to notify the email service, warehouse service, analytics service, and recommendation service — and they're all separate apps running on separate servers.

> **Note on names you might hear:** The popular broker software in industry includes **Kafka**, **RabbitMQ**, **Amazon SNS**, **Google Pub/Sub**. Don't worry about the specific names yet — they're all just different brands of "Twitter for software services." Same idea, different implementations.

---

#### Q6 — What if one observer crashes? Do the others still get notified?

**Short answer:** By default, NO — and that's a bug waiting to happen. Always wrap each observer's call in try/catch.

**The problem:**
```java
for (OrderObserver o : observers) {
    o.update(orderId, status);    // if observer #3 throws, the loop dies here
}
// Observers #4, #5, ... are NEVER called.
```

If `EmailService.update()` throws (e.g., SMTP timeout), the unhandled exception kills the loop. `WarehouseService` and `AnalyticsService` never get notified. Now your warehouse doesn't know about the new order — silently — because of an email failure. That's a disaster.

**The fix — exception isolation:**
```java
for (OrderObserver o : observers) {
    try {
        o.update(orderId, status);
    } catch (Exception e) {
        log.error("Observer {} failed for order {}", o.getClass().getSimpleName(), orderId, e);
        // continue with the next observer
    }
}
```

Each observer's failure is logged but does not affect the others. **This is non-negotiable in production Observer code.**

> **Analogy:** A teacher reading attendance asks "Anna?" — no answer. Should the teacher freeze and refuse to continue? Of course not. They mark Anna absent and move on to Bob. Same rule for observers.

##### "But wait — if I'm using async (ExecutorService), don't separate threads already protect me?"

Great instinct — partially yes, but there's a sneaky second problem. Let me show you.

When each observer runs on its own thread (the async fix from Q3):

```java
for (OrderObserver o : observers) {
    bgPool.submit(() -> o.update(orderId, status));    // each in its own thread
}
```

If observer #3's thread throws an exception:
- ✅ Observer #3's thread crashes, but only that ONE thread.
- ✅ Observers #1, #2, #4, #5… continue running on their own threads, unaffected.

So the "**one observer failing breaks the rest**" problem really is solved by async. **You're right to spot that.**

**BUT** — here's the trap. When an exception is thrown inside a task submitted to `ExecutorService`, the exception gets **silently swallowed** by the `Future` object that `submit()` returns. Unless you explicitly call `future.get()` to retrieve it (which is rarely done for fire-and-forget tasks), **you never see the error**:

- ❌ No log
- ❌ No console output
- ❌ No alert
- ❌ No metric

The exception just vanishes. You only find out **days later** when a customer complains *"I never got my SMS!"*. This is called a **silent failure** — the worst kind of bug, because you don't know it exists.

##### Two reasons for try-catch — and which one applies when

| Reason for try-catch | Sync notify | Async (ExecutorService) notify |
|---|---|---|
| **Prevent one observer from breaking the others** | ✅ Yes — without it, the loop dies | ❌ No — threads are independent |
| **Observability (logging, metrics, alerts)** | ✅ Yes | ✅ **Still yes — async silently swallows exceptions** |

So:
- **Sync mode:** try-catch is needed for BOTH isolation AND observability.
- **Async mode:** try-catch is no longer needed for isolation — but is **STILL needed for observability**, arguably MORE important because async failures are easier to miss.

**The full async-safe version:**

```java
for (OrderObserver o : observers) {
    bgPool.submit(() -> {
        try {
            o.update(orderId, status);
        } catch (Exception e) {
            log.error("Observer {} failed for order {}",
                      o.getClass().getSimpleName(), orderId, e);
            metrics.incrementCounter("observer.failures",
                                     "observer", o.getClass().getSimpleName());
        }
    });
}
```

##### TL;DR — the shifted reason

> **In async mode, try-catch is no longer about "stopping one bad observer from breaking the others." It's about NOT letting failures vanish silently.** Keep the try-catch. The reason has shifted from *isolation* to *visibility*.

##### Pro tip — `CompletableFuture` does the same thing cleanly

Modern Java often uses `CompletableFuture`, which has built-in exception handling without try-catch noise:

```java
for (OrderObserver o : observers) {
    CompletableFuture
        .runAsync(() -> o.update(orderId, status), bgPool)
        .exceptionally(e -> {
            log.error("Observer {} failed", o.getClass().getSimpleName(), e);
            return null;
        });
}
```

Same effect — logs the failure instead of letting it disappear. Stylistic alternative; the try-catch version is fine for interviews.

### 26.6 Trade-offs

✅ **Pros:**
- **Loose coupling** between publisher and observers.
- **Open-Closed** — add new observers without changing the publisher.
- **Dynamic** — subscribe/unsubscribe at runtime.
- **Single Responsibility** — each observer does one thing in response to events.

⚠️ **Cons:**
- **Order of notification** is implementation-defined (often insertion order, not always).
- **Memory leaks** — a long-lived publisher holds observers in its list, keeping them un-collectable by GC even after they're "done." See deep dive below.
- **Cascading updates** — observer A triggers observer B which triggers C... debugging is hard.
- **Synchronous by default** — slow observer = slow publisher unless you go async.

#### 26.6.1 Deep dive — the memory-leak con (with example)

##### What "keep alive" actually means in Java

Java's garbage collector cleans up objects that are **no longer reachable** from a "root" (static fields, running thread stacks, etc.). If class A holds a reference to class B, then **as long as A is alive, B is alive too** — B simply can't be collected.

In Observer:
- Publisher (`OrderService`) holds `List<OrderObserver> observers`.
- So every observer in that list is **reachable through the publisher**.
- If the publisher is long-lived (e.g., a singleton service running for the entire app lifetime), **every observer ever added to it is reachable for the entire app lifetime — even if you don't need them anymore.**

##### Concrete leak — using our `OrderService`

```java
// At startup — long-lived singleton:
OrderService orderService = new OrderService();   // lives forever

// Somewhere inside a request handler:
public void handleRequest() {
    EmailService email = new EmailService();
    orderService.addObserver(email);              // ★ publisher now holds reference
    // ... use email for this request ...
}   // ← local var `email` disappears here
```

You might think `email` becomes garbage now that the local variable is gone. **It doesn't.** Here's why:

```
   [GC root] ──▶ orderService ──▶ observers list ──▶ EmailService instance
                                                       │
                                                       ▼
                                            Still REACHABLE → cannot be GC'd
```

Do this 10,000 times across the day, and `orderService.observers` contains 10,000 dead `EmailService` objects, none collectable. Memory creeps up → eventually `OutOfMemoryError`.

##### Direction summary — who keeps whom alive

| Code pattern | Who keeps whom alive? | How common? |
|---|---|---|
| `publisher.addObserver(obs)` and forget `removeObserver` | **Publisher keeps Observer alive** (publisher's list holds reference) | ⭐⭐⭐ Common |
| Observer holds back-reference (`private OrderService source`) AND observer is reachable via some long-lived static / global | **Observer keeps Publisher alive** | ⭐ Rare |

The "publisher keeps observer alive" case is what bites people in practice.

##### The fixes

**Fix 1 — Always pair `add` with `remove`:**
```java
EmailService email = new EmailService();
orderService.addObserver(email);
try {
    // ... use email ...
} finally {
    orderService.removeObserver(email);   // ★ ensures cleanup
}
```

**Fix 2 — Self-unsubscribe inside `update()` (for one-time observers — same as §26.5 Q4):**
```java
@Override
public void update(int orderId, String status) {
    if (status.equals("DELIVERED")) {
        sendSms("Your order is here!");
        source.removeObserver(this);       // ★ cleanup once we're done
    }
}
```

**Fix 3 — Weak references (advanced, for libraries):**
Some frameworks (Guava's `EventBus`) store subscribers in a `WeakHashMap`. If nobody else references the observer, GC can still collect it even though the publisher's map points to it. This is library-grade — you usually don't need this in app code if you discipline yourself with Fix 1.

##### TL;DR

> **In long-lived Observer setups, every `addObserver(obs)` you forget to pair with `removeObserver(obs)` is a tiny memory leak.** Discipline yourself: every `add` should have a matching `remove`. For one-time observers, self-unsubscribe inside `update()`.

### 26.7 Real-World Industry Examples

1. **`java.util.Observable` / `Observer`** — built-in (deprecated since Java 9 but still classic).
2. **`PropertyChangeListener` in JavaBeans** — listen for property changes on a bean.
3. **Swing/JavaFX event listeners** — `ActionListener`, `MouseListener`, etc.
4. **RxJava / Reactive Streams** — Observer on steroids; supports async, backpressure, composition.
5. **Spring `ApplicationEventPublisher`** — `@EventListener` methods receive events.
6. **Frontend frameworks (React, Vue)** — components subscribe to state changes and re-render.

### 26.8 Observer — Interview Cheat Sheet

| Q | A |
|---|---|
| When use Observer? | When one event must trigger multiple reactions and you don't want the publisher tightly coupled to the reactions. |
| Push vs Pull? | Push = pass data; Pull = signal change, observers query for data. |
| What about thread safety? | Use `CopyOnWriteArrayList` for observers if they can be added/removed from different threads. |
| Difference vs Pub/Sub? | Observer is in-process, direct method calls. Pub/Sub usually involves a message broker, possibly across processes. |
| Real Java example? | Spring `@EventListener`, Swing event listeners, RxJava `Observable`. |
| Memory leak risk? | Yes — make sure to `removeObserver()` when no longer needed; consider weak references for long-lived publishers. |

### 26.9 TL;DR

> **Observer = subscribe & notify.** Publisher keeps a `List<Observer>`. When state changes, it loops through and calls `update()` on each. Observers register/unregister freely; publisher doesn't know concrete types. Foundation for event systems, reactive programming, and pub/sub.

---

## 27. Chain of Responsibility Design Pattern

> **Category:** Behavioral
>
> **Friendly definition:** A request travels along a **chain of handlers**. Each handler checks: *"Can I handle this?"* — if yes, handle it; if no, **pass it to the next** handler. Eventually someone handles it, or the chain ends.
>
> **Formal definition (for reference):** Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.

### 27.1 The Problem It Solves — From Scratch

#### Real-world analogy: corporate expense approval

You submit an expense report at your company. The amount determines who must approve it:

| Amount | Who approves |
|---|---|
| ≤ ₹10,000 | Manager |
| ≤ ₹50,000 | Director |
| ≤ ₹2,00,000 | VP |
| > ₹2,00,000 | CFO |

You don't directly call the CFO. You hand it to your **Manager**. The Manager looks: "₹8,000? I can approve." Done.

But if it's ₹40,000, the Manager says: "Out of my limit, sending to Director." The Director receives, checks, approves or escalates further.

The **request flows through a chain** until someone in the chain has authority to handle it. The sender (you) doesn't know or care which level approves.

#### Translate to code: reimbursement system

The lecture's exact example. We have 4 approvers in a chain:

```
[Employee] ──▶ [Manager] ──▶ [Director] ──▶ [VP] ──▶ [CFO]
                ≤10K            ≤50K          ≤2L      Unlimited
```

Each one either approves OR forwards.

#### The bad way — giant `if/else` based on amount

```java
public void approve(ExpenseReport report) {
    if (report.getAmount() <= 10_000) {
        System.out.println("Manager approved");
    } else if (report.getAmount() <= 50_000) {
        System.out.println("Director approved");
    } else if (report.getAmount() <= 2_00_000) {
        System.out.println("VP approved");
    } else {
        System.out.println("CFO approved");
    }
}
```

**Why it's bad:**
- One method knows about all 4 approval levels.
- Adding a new level (e.g., "Team Lead") = editing the method.
- You can't add behavior per role (Manager logs, Director sends email).
- Hard to reorder roles or skip levels.

#### The Chain of Responsibility solution

Each role becomes a class. Each role knows ONLY:
1. Its own approval limit.
2. Who's the **next** approver if it can't handle.

The client just hands the request to the first link; the request flows down the chain.

### 27.2 Code Walkthrough — Step by Step

#### Step 1 — The Expense Report (the request object)

```java
public class ExpenseReport {
    private final double amount;
    private final String purpose;
    private final String employeeName;

    public ExpenseReport(double amount, String purpose, String employeeName) {
        this.amount = amount;
        this.purpose = purpose;
        this.employeeName = employeeName;
    }

    public double getAmount()       { return amount; }
    public String getPurpose()      { return purpose; }
    public String getEmployeeName() { return employeeName; }
}
```

#### Step 2 — The abstract Handler — `Approver`

This is the chain's spine. Every approver knows about a `next` link.

```java
public abstract class Approver {
    protected Approver next;     // ← reference to the NEXT approver in the chain

    public void setNext(Approver next) {
        this.next = next;
    }

    public abstract void approve(ExpenseReport report);
}
```

Every concrete approver will:
- Try to handle the request itself
- If can't, delegate to `next`

#### Step 3 — Concrete Handlers (Manager, Director, VP, CFO)

```java
public class Manager extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 10_000) {
            System.out.println("Manager approved: " + report.getPurpose());
        } else if (next != null) {
            next.approve(report);                   // forward to Director
        }
    }
}

public class Director extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 50_000) {
            System.out.println("Director approved: " + report.getPurpose());
        } else if (next != null) {
            next.approve(report);                   // forward to VP
        }
    }
}

public class VP extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 2_00_000) {
            System.out.println("VP approved: " + report.getPurpose());
        } else if (next != null) {
            next.approve(report);                   // forward to CFO
        }
    }
}

public class CFO extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        System.out.println("CFO will take final action: " + report.getPurpose());
        // CFO is the last link — no forwarding
    }
}
```

Each class has the **same shape**:
1. Check if it can handle.
2. If yes, do its action.
3. If no, forward to the next.

#### Step 4 — The Client — build the chain, then submit

```java
public class ChainDemo {
    public static void main(String[] args) {
        // Create the approvers
        Approver manager  = new Manager();
        Approver director = new Director();
        Approver vp       = new VP();
        Approver cfo      = new CFO();

        // Build the chain — link them in order
        manager.setNext(director);
        director.setNext(vp);
        vp.setNext(cfo);

        // Submit different reports
        ExpenseReport rp1 = new ExpenseReport(8_000, "Team Lunch", "Vikas");
        manager.approve(rp1);
        // → Manager approved: Team Lunch  (handled at first link)

        ExpenseReport rp2 = new ExpenseReport(40_000, "Conference", "Vikas");
        manager.approve(rp2);
        // → Director approved: Conference  (forwarded once)

        ExpenseReport rp3 = new ExpenseReport(5_00_000, "New office furniture", "Vikas");
        manager.approve(rp3);
        // → CFO will take final action: New office furniture  (forwarded all the way)
    }
}
```

The **same call** (`manager.approve(report)`) handles all amounts — the chain figures out who actually handles it.

### 27.3 UML Diagrams

CoR is unusual — it has both a **structure side** (a hierarchy of handler classes) AND a **dynamic side** (a request flowing through them). UML supports both with two different diagram types. Let's draw each properly.

#### 27.3.1 Class Diagram (static structure)

This shows the inheritance hierarchy AND the key feature of CoR — the **self-aggregation** (`Approver` holds a reference to another `Approver` as `next`).

```
              ┌──────────────────────────────────────┐
              │         Approver (abstract)           │ ◇────┐
              │ ─────────────────────────────────────│      │  self-aggregation
              │  # next: Approver                     │      │  multiplicity 0..1
              │ ─────────────────────────────────────│      │  ("an Approver has
              │  + setNext(next: Approver)            │──────┘   0 or 1 'next' Approver")
              │  + approve(report) : abstract         │
              └─────────────────△────────────────────┘
                                │ extends (solid line + hollow triangle)
       ┌──────────────┬─────────┼─────────┬─────────────┐
       │              │         │         │             │
  ┌────┴────┐   ┌─────┴───┐  ┌──┴────┐  ┌─┴────┐
  │ Manager │   │Director │  │  VP   │  │  CFO │   ← Concrete handlers
  │+approve │   │+approve │  │+approve│ │+approve│    (each overrides
  └─────────┘   └─────────┘  └───────┘  └──────┘     `approve` with its limit)
```

**Key elements explained:**

| Element | Meaning |
|---|---|
| **`# next: Approver`** | The `#` is UML notation for **`protected`** — readable by subclasses but not by outside code. Subclasses (`Manager`, `Director`, etc.) directly use `next` inside their `approve()` methods, so it can't be `private`. |
| **`+` on `setNext` and `approve`** | The `+` means **`public`** — callable from any class. The client uses `setNext` to wire the chain. |
| **`◇` hollow diamond from Approver to itself** | **Self-aggregation** — every `Approver` instance holds a reference to (at most) one other `Approver` via its `next` field. This is what wires the chain. The diamond is hollow (aggregation, not composition) because the `next` Approver is created externally and just held as a reference. |
| **Multiplicity `0..1`** | Each Approver has either 0 (terminal — like CFO) or 1 next. |
| **`△` solid triangle + solid line** | Inheritance (extends). For interface realization we'd use a dashed line instead. |
| **Each concrete handler overrides `approve(report)`** | They share the structure but plug in their own limit-checking logic. |

##### UML visibility shorthand (good to memorize)

| Symbol | Meaning | Java |
|---|---|---|
| `+` | public | `public` |
| `-` | private | `private` |
| `#` | **protected** | `protected` |
| `~` | package-private | (no modifier) |

Using the right symbol matters — saying a field is `-` (private) when it's actually `#` (protected) gives the wrong picture of who can access it. Subclasses get visual permission via `#`; outsiders are locked out. Use the symbol that matches the actual code.

> **Why is this self-aggregation the key insight?** The chain is just one class that **knows about another instance of itself**. Without that `next: Approver` field, there's no chain — just isolated handlers. Recursive associations are the structural backbone of CoR.

#### 27.3.2 Sequence Diagram (dynamic flow at runtime)

This shows what actually happens when a `₹40,000` report flows through the chain. A sequence diagram has **vertical lifelines** (one per object) and **horizontal arrows** (each arrow = a method call).

```
   Client      Manager       Director         VP            CFO
     │            │             │              │              │
     │ approve(rp₹40K)          │              │              │
     │───────────▶│             │              │              │
     │            │ amount ≤ 10K? NO           │              │
     │            │             │              │              │
     │            │ next.approve(rp)           │              │
     │            │────────────▶│              │              │
     │            │             │ amount ≤ 50K? YES           │
     │            │             │ ► print "Director approved" │
     │            │             │              │              │
     │            │             │              │              │
     │            │             │ (chain ends here — VP & CFO never called)
     │            │             │              │              │
     │            │◀────────────│ return       │              │
     │◀───────────│ return      │              │              │
     │            │             │              │              │
```

**How to read this:**

| Symbol | Meaning |
|---|---|
| Vertical dashed/solid `│` | A "lifeline" — represents one object existing over time. Time flows downward. |
| `───▶` (solid arrow) | A method call from one object to another. |
| `◀── ` (returning arrow) | Return from the method call. |
| Text in the middle | What that object does locally during that call. |

**The story this diagram tells:**

1. Client calls `Manager.approve(rp)`.
2. Manager checks its limit (₹10K) — doesn't qualify. So Manager calls `Director.approve(rp)`.
3. Director's limit (₹50K) qualifies — prints approval. **Chain stops here.**
4. Control returns up: Director → Manager → Client.
5. `VP` and `CFO` are NEVER called. They have lifelines drawn because they EXIST, but no arrow ever reaches them.

#### When to use which diagram?

| You want to communicate | Use |
|---|---|
| "Here are the handler classes and how they're wired together" | **Class diagram** (§27.3.1) |
| "Here's what happens when a specific request comes in" | **Sequence diagram** (§27.3.2) |
| Both, for completeness | Show both side-by-side in interviews |

> **Interview tip:** Drawing BOTH a class diagram and a sequence diagram for CoR signals senior-engineer maturity. Most candidates only draw one. The class diagram shows you understand the *structure*; the sequence diagram shows you understand the *behavior*. They're complementary, not redundant.

### 27.4 Pure Chain vs Pipeline — the most important variant

CoR has **two different "personalities"** that share the same code structure. Knowing both is essential — they trip up many engineers.

#### Variant A — Pure Chain ("ONE handler wins, then stop")

This is what we built in §27.2 (approval workflow). Each handler either handles the request **OR** forwards — never both.

```java
public class Manager extends Approver {
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 10_000) {
            System.out.println("Manager approved");
            // ★ chain STOPS here — does NOT forward after handling
        } else if (next != null) {
            next.approve(report);     // forward only if I can't handle
        }
    }
}
```

**Behavior:** Only ONE handler ever processes a single request. As soon as someone handles it, the chain ends.

#### Variant B — Pipeline ("EVERY handler runs in order")

A tiny code change converts a chain into a pipeline: **the handler always forwards, even after doing its work.**

```java
public class LoggingHandler extends Approver {
    public void approve(ExpenseReport report) {
        System.out.println("LOG: incoming " + report);   // always do my work
        if (next != null) {
            next.approve(report);                          // ★ ALWAYS forward — never stops
        }
    }
}
```

**Behavior:** EVERY handler runs in order. Each one contributes something (logging, validation, transformation), then passes along.

#### Side-by-side

| | Pure Chain | Pipeline |
|---|---|---|
| Who handles? | ONE handler (first that can) | EVERY handler |
| Why forward? | "I can't handle, please try next" | "I'm done, now you do your part" |
| Real-world example | Expense approval, exception catching | Logging → auth → compression → actual response |
| When does the chain stop? | When someone handles it | When `next == null` |
| Order matters? | Yes — first-match wins | Yes — order of operations matters (e.g., decompress before parse) |

#### Easy way to remember

- **Pure Chain = phone tree.** Press 1 for Sales, 2 for Support. You only talk to ONE department.
- **Pipeline = assembly line.** Every station does its bit (paint, weld, polish) before the car moves on.

#### When to use which?

| Goal | Pattern |
|---|---|
| "Find the ONE right handler for this kind of request" (approval, exception handling, command lookup) | **Pure Chain** |
| "Every step needs to add something before final processing" (logging, metrics, auth, validation, compression) | **Pipeline** |
| "Some handlers may handle AND others should also see the request" (hybrid: short-circuit + observers) | Custom — usually with `return boolean` or events |

Both share the same skeleton (a linked list of handlers, each with `next`). The only difference is **whether the handler keeps forwarding after handling**.

### 27.5 Common Confusions — explained in plain English

#### Q1 — Do all handlers in the chain actually run, or only one?

**Short answer:** In Pure Chain (§27.4 Variant A), **only ONE handler runs** — the first one whose check passes. The rest never see the request.

Let's trace a ₹40,000 report through the chain we built:

```
Step | Handler called   | Check                  | Action
-----+------------------+------------------------+-------------------------------
 1   | Manager.approve  | amount ≤ 10,000?       | NO  → forwards to Director
 2   | Director.approve | amount ≤ 50,000?       | YES → "Director approved!" — chain stops
     |                  |                        |
 3   | VP.approve       | (never called)         | ←── doesn't even know the report exists
 4   | CFO.approve      | (never called)         | ←── doesn't even know the report exists
```

So the rule for Pure Chain: **first match wins → chain stops.**

> **Analogy:** Phone tree. "Press 1 for Sales, 2 for Tech, 3 for Billing." Sales picks up and helps you. Tech and Billing never even ring. ONE department handles your call.

(In Pipeline mode, every handler runs — see §27.4.)

---

#### Q2 — What happens if NO handler can handle the request? Does it disappear?

**Short answer:** In the basic implementation, **yes — it's silently dropped**. That's almost always a bug. Always cap your chain with a "fallback" handler.

Let's see the silent drop with a tweak to our example. Suppose CFO has a limit too:

```java
public class CFO extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 1_00_00_000) {       // CFO can approve up to 1 crore
            System.out.println("CFO approved");
        } else if (next != null) {
            next.approve(report);                       // but next is null — CFO is last
        }
        // else: silently does NOTHING ❌
    }
}
```

Now submit a ₹2 crore report:

```
Manager forwards → Director forwards → VP forwards → CFO checks → ✗ over limit
CFO's next is null → execution returns silently
No log, no error, nothing.
```

**The user thinks their report was submitted. It's vanished into the void.** Classic silent failure.

##### Fix 1 — add a fallback handler at the end of the chain

```java
public class RejectAllFallback extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        System.out.println("REJECTED — exceeds all approval limits: " + report.getPurpose());
        // log to monitoring, alert ops, send rejection email, etc.
    }
}

// Wire it as the last handler:
cfo.setNext(new RejectAllFallback());
```

Now requests that nobody could handle still hit the fallback → logged → visible → fixable.

##### Fix 2 — make the base `Approver` throw if it falls off

```java
public abstract class Approver {
    protected Approver next;

    protected void forward(ExpenseReport report) {
        if (next == null) {
            throw new IllegalStateException("Request fell off the chain: " + report);
        }
        next.approve(report);
    }
}
```

Concrete handlers call `forward(report)` instead of `next.approve(report)` — now the chain refuses to silently drop anything.

> **Rule of thumb:** A chain without a fallback or sentinel is a memory leak's evil cousin: a **logic leak**. Silent failures = the worst kind of bug.

---

#### Q3 — What's the difference between Chain of Responsibility and "middleware"?

**Short answer:** Pure Chain stops after one handler. Middleware = Pipeline → every handler runs. **Same skeleton, opposite "forward" behavior.** Already covered in §27.4 — quick recap:

```java
// PURE CHAIN — forward ONLY if I can't handle
if (canHandle(report)) {
    handle(report);            // chain stops here
} else if (next != null) {
    next.approve(report);
}

// MIDDLEWARE / PIPELINE — ALWAYS do my work, ALWAYS forward
doMyWork(report);
if (next != null) {
    next.approve(report);      // always forward
}
```

> **Examples in real systems:**
> - **Pure Chain:** Java `try/catch`, expense approvals, GUI event bubbling (often, but not always).
> - **Pipeline:** Express.js middleware (`app.use(...)`), Servlet filters, Spring Security filter chain.

If you can answer "*does this handler ALWAYS forward after doing its work?*" with yes → pipeline. With no (only when it can't handle) → pure chain.

---

#### Q4 — Can I change the chain at runtime?

**Short answer:** Yes — that's CoR's superpower over a fixed `if/else`.

```java
// Initial chain:
manager.setNext(director);
director.setNext(vp);
vp.setNext(cfo);
// Chain: Manager → Director → VP → CFO

// Promote a new role mid-chain at runtime:
Approver seniorDir = new SeniorDirector();
director.setNext(seniorDir);
seniorDir.setNext(vp);
// New chain: Manager → Director → SeniorDirector → VP → CFO

// Or remove a handler at runtime (e.g., VP is on leave):
director.setNext(cfo);
// New chain: Manager → Director → CFO  (VP skipped)
```

**Use cases:**
- Different approval workflows per department (Engineering vs Finance).
- Feature flags / A-B testing — route some traffic through experimental handlers.
- Maintenance — temporarily bypass a handler that's misbehaving.

An `if/else` chain can't be reconfigured without deploying new code. CoR lets you reconfigure with a config file or admin command.

---

#### Q5 — What if one handler in the chain throws an exception?

**Short answer:** By default, exceptions **bubble up through the chain** and crash the original caller. Whether that's good or bad depends on whether the chain is "critical" (approval) or "best effort" (logging).

```java
public class Manager extends Approver {
    public void approve(ExpenseReport report) {
        if (report.getAmount() <= 10_000) {
            sendApprovalEmail(report);    // ← what if SMTP server is down?
        } else if (next != null) {
            next.approve(report);
        }
    }
}
```

If `sendApprovalEmail` throws:
- The exception escapes `Manager.approve()`.
- It also escapes the original `manager.approve(report)` call.
- Crash bubbles up to whoever called `manager.approve(...)`.

##### Decision: should the chain "swallow" exceptions?

| Chain purpose | Should I try/catch? |
|---|---|
| **Pure Chain — approval, command dispatch, exception handling** | ❌ NO. Let exceptions propagate. The CALLER must know "your request wasn't processed." Silent swallowing = lost requests. |
| **Pipeline — logging, metrics, cross-cutting concerns** | ✅ YES, per handler. Wrap each handler in try-catch so one stage's failure doesn't kill the rest. (Just like Observer Q6.) |

This is the same lesson from Observer's Q6: **the reason for try-catch depends on whether you need exception isolation OR observability.**

```java
// Pipeline-style with isolation:
public void approve(ExpenseReport report) {
    try {
        doMyWork(report);
    } catch (Exception e) {
        log.error("Handler {} failed", this.getClass().getSimpleName(), e);
        // continue forwarding anyway
    }
    if (next != null) next.approve(report);
}
```

---

#### Q6 — Can the chain accidentally become cyclic? What stops infinite recursion?

**Short answer:** Yes, it CAN be cyclic if you wire it badly. CoR has no built-in protection. You need to be careful when calling `setNext()`.

```java
manager.setNext(director);
director.setNext(manager);         // 💥 OOPS — cycle

manager.approve(report);
// → Manager forwards → Director forwards → Manager → Director → ...
// → StackOverflowError after ~10,000 frames
```

The bug is silent at wiring time — Java doesn't know `setNext(manager)` creates a cycle. You only find out when a request crashes the JVM.

##### Protections (pick one)

**Option A — discipline.** Always wire forward (root → leaf). Review the chain-construction code carefully. This is what 95% of teams rely on.

**Option B — cycle detection in the base class:**

```java
public abstract class Approver {
    protected Approver next;

    public void setNext(Approver next) { this.next = next; }

    // ── PUBLIC entry point — what the client calls ──
    public final void approve(ExpenseReport report) {
        approve(report, new HashSet<>());                       // fresh visited set per request
    }

    // ── PROTECTED recursive method — handlers use this to forward ──
    protected void approve(ExpenseReport report, Set<Approver> visited) {
        if (!visited.add(this)) {                                // tries to add; fails if already there
            throw new IllegalStateException("Cycle detected in chain at " + this);
        }
        doApprove(report, visited);                              // delegate to subclass
    }

    // ── ABSTRACT — concrete handlers implement THIS, not approve() ──
    protected abstract void doApprove(ExpenseReport report, Set<Approver> visited);
}
```

A concrete handler:

```java
public class Manager extends Approver {
    @Override
    protected void doApprove(ExpenseReport report, Set<Approver> visited) {
        if (report.getAmount() <= 10_000) {
            System.out.println("Manager approved: " + report.getPurpose());
        } else if (next != null) {
            next.approve(report, visited);                       // ★ forward, passing visited along
        }
    }
    @Override public String toString() { return "Manager"; }
}
// Director / VP / CFO look identical — different limit + label.
```

The client:

```java
public class ChainDemo {
    public static void main(String[] args) {
        Approver manager  = new Manager();
        Approver director = new Director();
        Approver vp       = new VP();
        Approver cfo      = new CFO();

        manager.setNext(director);
        director.setNext(vp);
        vp.setNext(cfo);
        // Chain: Manager → Director → VP → CFO (terminal — cfo.next = null)

        // ── Normal request (no cycle) ──
        manager.approve(new ExpenseReport(40_000, "Conference", "Vikas"));
        // → "Director approved: Conference"

        // ── Buggy wiring introduces a cycle ──
        cfo.setNext(manager);                  // 💥 manager → director → vp → cfo → manager → ...

        try {
            manager.approve(new ExpenseReport(5_00_00_000, "Big build", "Vikas"));
        } catch (IllegalStateException e) {
            System.out.println("Caught: " + e.getMessage());
            // → "Caught: Cycle detected in chain at Manager"
        }
    }
}
```

##### Trace for the cyclic case

| Step | Method invoked | `visited` state | `visited.add(this)` returns | Outcome |
|---|---|---|---|---|
| 1 | `Manager.approve(rp, {})` | `{}` | **`true`** (added) | proceed → forward to Director |
| 2 | `Director.approve(rp, {M})` | `{M}` | **`true`** (added) | proceed → forward to VP |
| 3 | `VP.approve(rp, {M, D})` | `{M, D}` | **`true`** (added) | proceed → forward to CFO |
| 4 | `CFO.approve(rp, {M, D, VP})` | `{M, D, VP}` | **`true`** (added) | proceed → forward to Manager (cycle!) |
| 5 | `Manager.approve(rp, {M, D, VP, CFO})` | `{M, D, VP, CFO}` | **`false`** — Manager already in set! | **throw** 💥 |

The cycle is caught on **the second visit** to Manager — one step into the loop — instead of recursing forever.

##### 3 Java idioms hiding in this code (revision-worthy)

###### Idiom #1 — `HashSet.add(x)` returns a boolean

`visited.add(this)` does **two things in one call**:
1. Tries to add `this` to the set.
2. Returns `true` if newly added, **`false`** if `this` was already there.

That's why the cycle-check is a single line: `if (!visited.add(this)) { throw ... }`.

The equivalent two-step version:
```java
if (visited.contains(this)) throw new IllegalStateException("...");
visited.add(this);
```
The one-liner is the idiomatic Java way (one hash lookup instead of two, thread-safe-ish, more compact).

Useful elsewhere — anywhere you want "is this the first time I'm seeing X?":
```java
if (!seenUsers.add(userId)) {
    log.warn("Duplicate user: " + userId);
}
```

###### Idiom #2 — `this` always refers to "the object before the dot"

Inside any instance method, `this` is automatically the object the method was called *on*:

| Call site | Inside that method, `this` is |
|---|---|
| `manager.approve(...)` | the Manager instance |
| `next.approve(...)` where `next` is Director | the Director instance |
| `vp.approve(...)` | the VP instance |

So `visited.add(this)` records *whoever is currently running*, not whoever started the chain. Java automatically swaps the meaning of `this` on every dot-call — that's why each handler ends up in the set.

###### Idiom #3 — Public method `final` + protected hook = "lock the skeleton"

Why is `approve(report)` declared `final`?

```java
public final void approve(ExpenseReport report) {        // ★ subclasses CANNOT override this
    approve(report, new HashSet<>());                     // creates the visited set — non-negotiable setup
}
```

Without `final`, a subclass could "helpfully" override it and bypass cycle detection:
```java
class CleverHandler extends Approver {
    @Override
    public void approve(ExpenseReport report) {
        doApprove(report, null);   // ❌ skipped the cycle-set creation!
    }
}
```

`final` is a **compile-time guarantee** that the setup (create fresh `HashSet`, start recursion) ALWAYS happens. Subclasses can only customize `doApprove` — the business logic — never the framework setup.

This is the **same pattern as Template Method (§28)**: lock the public entry point with `final`, expose only the customization hooks (`doApprove`). It's a *micro-template-method inside CoR*.

**Option C — runtime depth limit:** track recursion depth, fail if it exceeds (say) 100. Cheap safety net.

In practice, Option A (discipline) is usually enough — chains are short and constructed in one place. Option B + the 3 idioms above is worth knowing because the same techniques (`final` entry + protected hook + visited set) show up in many other patterns (tree traversal, graph algorithms, Template Method, Composite).

> **Analogy:** A document is being routed for signature. Officer A signs and forwards to B, who forwards to C, who … forwards back to A. They'd be in this loop forever. Either people pay attention, or you add a stamp that tracks "I've seen this already."

---

#### Q7 — How is CoR different from Observer? They look kind of similar.

**Short answer:** Observer = "tell EVERYONE who's interested." CoR = "find the ONE person who can handle this."

| | Observer | Chain of Responsibility |
|---|---|---|
| **How many handlers actually run?** | ALL subscribers run | Usually ONE handler (Pure Chain) |
| **Are the handlers peers or linked?** | Peers in a `List<Observer>` | Linked list — each knows the NEXT |
| **What's the goal?** | Broadcast — notify everyone | Find — route to the right one |
| **Order of handlers matters?** | Usually no | YES — first capable wins |
| **Removing one handler affects others?** | **Structurally no** — observers are peers in a flat list, not wired to each other. (But: if removed DURING a sync `ArrayList` iteration, the publisher crashes — see §26.5 Q4. `CopyOnWriteArrayList` or async dispatch fixes that.) | Could break the chain (if the removed handler was the `next` for an earlier one — unless you rewire) |
| **Iconic example** | Order placed → email + warehouse + analytics all react | Expense submitted → Manager OR Director OR VP OR CFO handles |

##### One-liner mental hooks

- **Observer = TELEGRAM** (broadcast to all subscribers).
- **CoR = PHONE TREE** (route to the right department).

Both are "behavioral patterns with multiple receivers," but their **intent** is opposite — Observer fan-outs; CoR routes/filters.

---

### 27.6 Trade-offs (with deep dive on the gotchas)

#### Pros

✅ **Decouples sender from receiver** — sender just talks to the chain head; doesn't know who eventually handles.
✅ **Flexible chain composition** — reorder, add, remove handlers at runtime (§27.5 Q4).
✅ **Single Responsibility** — each handler does ONE thing.
✅ **OCP friendly** — adding a new handler doesn't change existing ones.

#### Cons (and how to mitigate each)

##### Con 1 — Silent drop if no handler matches

Covered in §27.5 Q2. **Always cap the chain with a fallback handler** so unhandled requests get logged or alerted rather than vanishing.

##### Con 2 — Hard to debug

A request that "didn't work right" could've been mishandled at ANY handler in the chain. With 10+ handlers, tracing is painful.

**Mitigations:**

```java
// Add structured logging to EVERY handler:
public void approve(ExpenseReport report) {
    log.debug("[{}] received report id={} amount={}",
              this.getClass().getSimpleName(), report.getId(), report.getAmount());
    if (canHandle(report)) {
        log.info("[{}] HANDLED report id={}", this.getClass().getSimpleName(), report.getId());
        doHandle(report);
    } else {
        log.debug("[{}] forwarding report id={} to next", this.getClass().getSimpleName(), report.getId());
        if (next != null) next.approve(report);
    }
}
```

Now your logs read like a flowchart, e.g.:
```
[Manager]  received report id=42 amount=40000
[Manager]  forwarding report id=42 to next
[Director] received report id=42 amount=40000
[Director] HANDLED report id=42
```

You can pinpoint where any request stopped, in seconds.

##### Con 3 — Performance: O(n) per request for long chains

Each handler is a function call. For a chain of N handlers, the worst case (no one handles → fall through to fallback) is **N method invocations + N comparisons**.

For most chains (5-10 handlers), this is fine. For chains of 100+ (e.g., a giant security filter list), it adds up.

**Mitigations:**

- **Sort handlers by likelihood** — put the "most likely to handle" handler first. Average case becomes nearly O(1).
- **For O(1) lookup**, use a `Map<Criterion, Handler>` instead of a chain. You lose the "easy to reorder" benefit, but gain instant routing.

```java
Map<String, Approver> dispatchTable = Map.of(
    "SMALL",  manager,
    "MEDIUM", director,
    "LARGE",  vp,
    "HUGE",   cfo
);
String bucket = classify(report.getAmount());
dispatchTable.get(bucket).approve(report);
```

This is no longer "Chain of Responsibility" — it's a **dispatch table** — but it's the right pattern when O(1) lookup matters.

##### Con 4 — Tight coupling on chain order

The chain encodes domain rules in its order (Manager → Director → VP → CFO). If someone re-wires the chain wrong, expensive reports could get approved by junior staff. CoR has no type-level protection against wiring errors.

**Mitigations:**

- Wire the chain in **ONE place** (a `ChainBuilder` class). Don't let arbitrary code call `setNext()`.
- Write a `validateChain()` method that asserts limits are in ascending order along the chain.

---

### 27.7 Real-World Industry Examples

1. **Servlet `Filter` chain (Pipeline variant)** — request flows through filters (auth → compression → logging) before hitting the servlet. Every filter runs, then forwards via `chain.doFilter(request, response)`.

2. **Spring Security filter chain (Pipeline variant)** — `SecurityFilterChain` is a list of filters: `CsrfFilter` → `LogoutFilter` → `BasicAuthenticationFilter` → `FilterSecurityInterceptor`. Each filter checks, then forwards.

3. **Java exception handling (Pure Chain variant)** — `try/catch` blocks behave like CoR. The runtime walks up the stack frame by frame, asking each method: "Do you have a `catch` for this exception type?" First match handles; others are skipped.

4. **Express.js / Node.js middleware (Pipeline)** — `app.use(logger); app.use(auth); app.use(compression)` — each runs in order. Each calls `next()` to forward.

5. **Approval workflows (Pure Chain)** — corporate expense, leave, document approvals — exactly the lecture's example.

6. **GUI event propagation (Pure Chain)** — a click on a button bubbles up: button → panel → window → frame. First one that has a handler for that event consumes it (in many UI frameworks).

7. **Logging frameworks (Pipeline)** — Log4j / SLF4J appenders. Each appender (console, file, network) processes the log message and forwards.

### 27.8 CoR — Interview Cheat Sheet

| Q | A (plain English first) |
|---|---|
| When should I use Chain of Responsibility? | When multiple potential handlers exist for a request, and you don't want the sender to hardcode which one handles. |
| Pure Chain vs Pipeline? | Pure Chain stops after one handler wins. Pipeline runs every handler in order. Same skeleton, different "forward" behavior. |
| Difference vs Observer? | Observer = telegram (everyone gets it). CoR = phone tree (one department handles). |
| Difference vs Decorator? | Decorator wraps to ADD behavior on top. CoR routes the request to the RIGHT handler. |
| What if no handler matches? | Add a fallback handler at the end, OR have the base class throw "request fell off chain." |
| What if a handler throws? | Pure Chain: let exceptions propagate. Pipeline: try-catch each handler so one failure doesn't kill the rest. |
| What about cycles in the chain? | No built-in protection — wire carefully, or add visited-set tracking in the base class. |
| Performance for long chains? | O(N) in worst case. Sort handlers by likelihood, or switch to a dispatch table (`Map<Criterion, Handler>`) for O(1). |
| Real Java example? | Servlet/Spring Security filter chains, `try/catch`, GUI event bubbling. |

### 27.9 TL;DR

> **Chain of Responsibility = request hops along a linked list of handlers.** Sender talks only to the first handler. Each one decides: *handle it* (and stop the chain) OR *forward to the next*. Eventually someone handles it, or it hits a fallback.
>
> **Two flavors:**
> - **Pure Chain** (one wins, stop): approvals, exception handling, command dispatch.
> - **Pipeline** (every handler runs): middleware, logging, security filters.
>
> **Three rules to remember:**
> 1. Always end the chain with a fallback so requests can't silently vanish.
> 2. For long chains, log who saw the request and who handled it.
> 3. Wire the chain in ONE place to avoid cycles and ordering bugs.

---

## 28. Template Method Design Pattern

> **Category:** Behavioral
>
> **Friendly definition:** When several different things should follow the **same overall steps** but differ in **specific details**, **Template Method** lets you write the **common skeleton** in a base class once. Subclasses fill in the steps that differ. The order is fixed; only the details vary.
>
> **Formal definition (for reference):** Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
>
> **Lecture quote:** *"Skeleton of algorithm in base class. Subclass override specific step."*

### 28.1 The Problem It Solves — From Scratch

#### Real-world analogy: making different types of tea/coffee

The general algorithm to make a hot beverage:
1. Boil water  ← always the same
2. Brew the leaves/grounds  ← differs by drink (tea leaves vs coffee grounds)
3. Pour into cup  ← always the same
4. Add condiments  ← differs by drink (lemon for tea, milk for coffee)

Steps 1 and 3 are **always identical**. Steps 2 and 4 **depend on the drink**.

The recipe (algorithm) is fixed: **boil → brew → pour → add**. Just the brew/add steps change. We don't want to write two completely separate recipes; we want one recipe with two **swappable steps**.

That's Template Method.

#### Translate to code: API request handling

The lecture's exact example. Every API request in your application follows the same skeleton:

1. **Authenticate** the request  ← same for all
2. **Validate** input  ← differs by API (Payment validates card; User validates name)
3. **Process** business logic  ← differs by API
4. **Format** response  ← same for all
5. **Log/audit**  ← same for all

The **skeleton** (auth → validate → process → format → log) is identical. The middle two steps depend on which API.

#### The bad way — duplicate the entire flow per API

```java
public class PaymentController {
    public void handleRequest(String request) {
        authenticate(request);             // duplicated
        validatePayment(request);          // unique
        String result = processPayment(request); // unique
        String response = formatResponse(result); // duplicated
        logAudit(request, response);       // duplicated
    }
}

public class UserController {
    public void handleRequest(String request) {
        authenticate(request);             // duplicated again!
        validateUser(request);             // unique
        String result = processUser(request); // unique
        String response = formatResponse(result); // duplicated again!
        logAudit(request, response);       // duplicated again!
    }
}
```

**Why it's bad:**
- The skeleton (4 of 5 steps) is **copy-pasted** across every controller.
- If you need to add "rate-limiting" between auth and validate, you must edit **every** controller.
- DRY violation: same code repeated.
- Easy for one controller to forget a step (e.g., audit log) → security hole.

#### The Template Method solution

Put the skeleton in a base class as a `final` method. Make the differing steps `abstract`. Subclasses fill them in. Skeleton is guaranteed to run in the right order.

### 28.2 Code Walkthrough — Step by Step

#### Step 1 — The abstract template class — defines the skeleton

```java
public abstract class AbstractAPIHandler {

    // ── The TEMPLATE METHOD — defines the algorithm skeleton ──
    public final void handleRequest(String request) {
        authenticate(request);              // step 1: common
        validate(request);                   // step 2: subclass-specific
        String result = process(request);   // step 3: subclass-specific
        String response = formatResponse(result);  // step 4: common
        auditLog(request, response);         // step 5: common
    }

    // ── Concrete steps (common across all APIs) ──
    protected void authenticate(String request) {
        System.out.println("Authenticating request");
    }

    protected void auditLog(String request, String response) {
        System.out.println("Log Req & Response");
    }

    protected String formatResponse(String result) {
        return "Response: " + result;
    }

    // ── Abstract steps (each subclass MUST implement) ──
    protected abstract void validate(String request);
    protected abstract String process(String request);
}
```

The two key markers:
- **`final` on `handleRequest`** → subclasses CANNOT override the algorithm's overall shape. The skeleton is locked.
- **`abstract` on `validate` / `process`** → subclasses MUST provide their own.

This is the heart of Template Method: control the algorithm's structure from the base class, force subclasses to fill in only the parts that should vary.

#### Step 2 — A concrete handler — fills in the gaps

```java
public class PaymentAPIHandler extends AbstractAPIHandler {

    @Override
    protected void validate(String request) {
        System.out.println("Validation of request");
    }

    @Override
    protected String process(String request) {
        System.out.println("Processing Business logic");
        return "PaymentProcessed";
    }
}
```

`PaymentAPIHandler` only implements the bits unique to payments. It inherits `authenticate`, `formatResponse`, `auditLog` from the parent.

#### Step 3 — The client — invoke the template

```java
public class TemplateDemo {
    public static void main(String[] args) {
        AbstractAPIHandler paymentAPI = new PaymentAPIHandler();
        paymentAPI.handleRequest("Payment Request");
        // Output:
        // → Authenticating request
        // → Validation of request
        // → Processing Business logic
        // → Log Req & Response
    }
}
```

The client just calls `handleRequest(...)`. The template method runs the 5 steps in the right order, with payment-specific validate/process automatically slotted in.

If you want a `UserAPIHandler` later, just write another subclass — auth/format/log code is reused for free.

### 28.3 UML Diagram

```
                ┌────────────────────────────────────┐
                │  AbstractAPIHandler (abstract)      │
                │  ─────────────────────────────────  │
                │  + handleRequest(req): final {      │  ← THE TEMPLATE METHOD
                │      authenticate(req)              │     (skeleton, can't override)
                │      validate(req)                  │
                │      result = process(req)          │
                │      response = formatResponse(...) │
                │      auditLog(req, response)        │
                │    }                                │
                │  ─────────────────────────────────  │
                │  # authenticate(req)   (concrete)   │  ← shared
                │  # formatResponse(r)   (concrete)   │  ← shared
                │  # auditLog(req, res)  (concrete)   │  ← shared
                │  # validate(req)       (abstract)   │  ← subclass fills
                │  # process(req)        (abstract)   │  ← subclass fills
                └────────────────△───────────────────┘
                          ▲      │ extends
                          ╎      │ (solid line + hollow triangle)
            «calls»       ╎ ┌────┴────────────────────┐
            (dashed +     ╎ │   PaymentAPIHandler      │
             open arrow ▶)╎ │  # validate(req)          │   ← override abstract
                          ╎ │  # process(req)           │   ← override abstract
                          ╎ └─────────────────────────┘
                          ╎                  ▲
                          ╎                  ╎ «creates»
                          ╎                  ╎ (dashed +
                          ╎                  ╎  open arrow ▶)
                          ╎                  ╎
                       ┌──┴──────────────────┴───────┐
                       │           Client             │
                       │ (TemplateDemo.main)          │
                       │  - creates PaymentAPIHandler │
                       │  - calls handleRequest(...)  │
                       └──────────────────────────────┘
```

#### Reading the arrows

| Arrow | Type | Reason in code |
|---|---|---|
| `Client ╌╌▶ PaymentAPIHandler` («creates») | **Dependency** | `new PaymentAPIHandler()` is a *one-off* construction call; no field is kept |
| `Client ╌╌▶ AbstractAPIHandler` («calls») | **Dependency** | `paymentAPI.handleRequest(...)` is invoked through the abstract type's API; `paymentAPI` is a local variable, not a field |
| `PaymentAPIHandler ──△ AbstractAPIHandler` (solid + hollow triangle) | **Inheritance (extends)** | `class PaymentAPIHandler extends AbstractAPIHandler` |

Why both dependencies are dashed (`╌╌▶`) instead of solid? Because the client doesn't *hold* either object as a long-lived field. From our lifecycle rule:

```
this.x = new X()             → composition  (◆ filled diamond)
this.x = passedInX           → aggregation  (◇ hollow diamond)
void m() { X x = new X(); }  → dependency   (╌╌▶ dashed + open arrow)
```

The local variable `paymentAPI` lives only for the duration of `main()` — that's textbook **dependency**.

#### Why the client uses the ABSTRACT type, not the concrete one

Notice this line:
```java
AbstractAPIHandler paymentAPI = new PaymentAPIHandler();
//   ↑                              ↑
//   declared as abstract type     concrete instantiation
```

This is **programming to an abstraction**:
- The client *creates* a `PaymentAPIHandler` (one-time, transient).
- The client *interacts* with it through the `AbstractAPIHandler` reference (the long-term contract).

That's why the UML shows TWO dependency arrows: one for creation (concrete), one for usage (abstract). In real code, the "creates" line is often refactored into a factory or DI container, leaving the client with only the "calls" dependency on the abstract type.

> **UML visibility recap** (applies to all diagrams in this doc):
> - `+` public · `#` protected · `-` private · `~` package-private
>
> All the `# methods` in the template are `protected` so subclasses can override them but external clients can't bypass the template by calling them directly. **`handleRequest(...)` is the ONE `public` entry point** — everything else is `protected` to enforce "use the template, not the steps individually."

### 28.4 Hook Methods — abstract vs hook (the variant)

Sometimes a subclass might want to **optionally** add behavior at a step, not be forced to override it. That's what a "hook" is.

A **hook** is a method in the base class with an **empty default body**. Subclasses can override it if they want, but they don't have to.

```java
public abstract class AbstractAPIHandler {
    public final void handleRequest(String request) {
        authenticate(request);
        beforeValidate(request);            // ← HOOK: empty by default
        validate(request);                   // ← ABSTRACT: must be implemented
        String result = process(request);    // ← ABSTRACT: must be implemented
        afterProcess(request, result);       // ← HOOK: empty by default
        String response = formatResponse(result);
        auditLog(request, response);
    }

    // ── Hooks — subclasses MAY override, default is "do nothing" ──
    protected void beforeValidate(String request) { /* default empty */ }
    protected void afterProcess(String request, String result) { /* default empty */ }

    // ── Abstract — subclasses MUST override ──
    protected abstract void validate(String request);
    protected abstract String process(String request);

    // ... shared concrete steps ...
}
```

#### Abstract vs hook — when to use each

| Need | Use |
|---|---|
| EVERY subclass must implement this — there's no sensible default | **`abstract`** |
| Most subclasses don't customize this; only some do; sensible default = "do nothing" | **Hook** (concrete method with empty body) |
| EVERY subclass should run this; same code; not customizable | **Plain concrete method** in the parent (don't make it overridable at all) |

> **Rule of thumb:** Default to `abstract` for core steps that genuinely vary. Sprinkle hooks at "extension points" where subclasses MIGHT want to slot something in. Plain methods for shared code.

#### Quick example — why hooks are useful

```java
class PaymentAPIHandler extends AbstractAPIHandler {
    @Override
    protected void validate(String req) { /* ... */ }

    @Override
    protected String process(String req) { /* ... */ return "ok"; }

    // No need to touch hooks — they just do nothing.
}

class FraudCheckHandler extends AbstractAPIHandler {
    @Override
    protected void validate(String req) { /* ... */ }

    @Override
    protected String process(String req) { /* ... */ return "ok"; }

    // This handler DOES want a custom pre-validation step
    @Override
    protected void beforeValidate(String req) {
        runFraudCheck(req);
    }
}
```

`PaymentAPIHandler` ignores the hooks. `FraudCheckHandler` uses one. Neither is forced to implement what they don't need.

### 28.5 Common Confusions — explained in plain English

#### Q1 — Why does the template method HAVE to be marked `final`? What actually breaks if I forget?

**Short answer:** Without `final`, a subclass can override the entire algorithm and break the pattern's whole point. The `final` keyword is what makes Template Method *safe*.

Look at the template:

```java
public abstract class AbstractAPIHandler {
    public final void handleRequest(String request) {     // ← final
        authenticate(request);
        validate(request);
        process(request);
        auditLog(request, response);
    }
}
```

If we drop `final`, a careless or malicious subclass can do this:

```java
public class BadPaymentHandler extends AbstractAPIHandler {
    @Override
    public void handleRequest(String request) {     // overrides the template!
        validate(request);
        process(request);
        // ❌ skipped authenticate() — SECURITY HOLE
        // ❌ skipped auditLog()    — NO AUDIT TRAIL
    }
}
```

Now the auth and audit are silently gone. The whole point of Template Method was to *guarantee* these always run in this order. Without `final`, that guarantee is just a hope.

**Rule:** Always mark the template method `final`. The pattern's safety depends on it.

> **Analogy:** A factory manager designs an assembly line: weld → paint → polish → quality-check. The manager nails the steps down — workers can customize *how* they paint, but they can't *skip* paint or change the order. `final` is the manager nailing down the line.

---

#### Q2 — When do I use `abstract` and when do I use a hook? How do I decide?

**Short answer:** `abstract` = "you MUST provide this; no sensible default exists." Hook = "you MAY customize this; doing nothing is fine."

Concrete decision table:

| Situation | Choice |
|---|---|
| Every subclass clearly has its own version (e.g., validate payment vs validate user) | **`abstract`** |
| Most subclasses don't need this; a few do (e.g., `beforeValidate` for special pre-checks) | **Hook** with empty body |
| Same code for everyone; no customization makes sense | **Plain concrete method** (don't make it overridable) |

**Apply it to our API handler:**

| Step | Reasoning | Choice |
|---|---|---|
| `validate(request)` | Different per API — payment vs user have different rules | `abstract` |
| `process(request)` | Different per API — that's the whole reason for the handler | `abstract` |
| `authenticate(request)` | Same auth logic for all APIs | Concrete (shared) |
| `auditLog(request, response)` | Same logging logic | Concrete (shared) |
| `beforeValidate(request)` | Most APIs don't need it; fraud handler does | Hook |

> **Rule of thumb:** Start with `abstract` for the genuinely-varying steps. Add hooks ONLY when you discover a real customization need. Don't pre-emptively add hooks "just in case" — that's over-engineering.

---

#### Q3 — How is Template Method different from Strategy? They both let you swap behavior.

**Short answer:** Template Method uses **inheritance** — subclasses override pieces of a fixed parent algorithm. Strategy uses **composition\*** — you plug in a whole replaceable algorithm object.

> **\*A quick word on "composition":** GoF uses the term "composition" loosely to mean ANY *HAS-A* relationship (i.e., one object holding a reference to another) — used as the opposite of `extends`/inheritance. In strict UML, that umbrella term splits into TWO things:
> - **UML Composition (◆ filled diamond)** — the host CREATES and OWNS the strategy's lifecycle (`this.x = new X()`).
> - **UML Aggregation (◇ hollow diamond)** — the host is GIVEN a strategy from outside (`this.x = passedInX`).
>
> The example below uses **setter injection** → in strict UML it's **aggregation (◇)**, even though we colloquially call it "composition" in the design-principle sense. Don't let the same word in two contexts trip you up.

Same problem, two solutions side-by-side:

**Template Method version:**
```java
abstract class APIHandler {
    public final void handle(String req) {
        authenticate(req);
        validate(req);          // ← step is filled by the subclass
        process(req);           // ← step is filled by the subclass
        log(req);
    }
    protected abstract void validate(String req);
    protected abstract void process(String req);
    // ... shared steps ...
}

class PaymentHandler extends APIHandler {
    @Override protected void validate(String req) { ... }
    @Override protected void process(String req) { ... }
}

// Usage:
APIHandler h = new PaymentHandler();
h.handle("...");
```

**Strategy version:**
```java
class APIHandler {                                  // not abstract; not extended
    private ValidationStrategy validator;
    private ProcessingStrategy processor;

    public void setValidator(ValidationStrategy v) { this.validator = v; }
    public void setProcessor(ProcessingStrategy p) { this.processor = p; }

    public void handle(String req) {
        authenticate(req);
        validator.validate(req);    // ← delegate to plugged-in strategy
        processor.process(req);     // ← delegate to plugged-in strategy
        log(req);
    }
}

// Usage:
APIHandler h = new APIHandler();
h.setValidator(new PaymentValidator());
h.setProcessor(new PaymentProcessor());
h.handle("...");

// Switch behavior at runtime:
h.setValidator(new UserValidator());
h.setProcessor(new UserProcessor());
h.handle("...");
```

##### Compare side-by-side

| Aspect | Template Method | Strategy |
|---|---|---|
| Mechanism | Inheritance (`extends`) | HAS-A — strategy held as a field (UML: usually **aggregation ◇** when injected; could be ◆ composition if `new`d internally) |
| Can swap at runtime? | ❌ No — subclass is chosen at object creation | ✅ Yes — `setStrategy(...)` any time |
| Number of variants | One per subclass; fixed | One per strategy class; can mix-and-match validator + processor independently |
| Algorithm structure | Fixed by parent's template | Built by the host class; each step is a separate strategy |
| Coupling | Tight — subclass tied to parent | Loose — strategy and host swap freely |
| When to use | Skeleton is genuinely fixed; few variations | Many variations; runtime swap needed; want to mix components |

##### Easy way to remember

- **Template Method = "I'll cook dinner; you bring the dessert."** Fixed menu; you fill in *one* slot. (Subclass per cook.)
- **Strategy = "Tell me what to cook and I'll cook it."** Entire dish is swappable at any time.

If you find yourself wanting to mix-and-match the two varying steps independently (e.g., "Payment validator + User processor"), you've outgrown Template Method — use Strategy.

---

#### Q4 — What if a subclass needs a DIFFERENT order of steps?

**Short answer:** You can't change the order — `final` locks it. If you genuinely need a different order, Template Method is the wrong pattern; switch to Strategy.

Concrete example: imagine one handler needs to log BEFORE processing (audit-first design), not after.

```java
class AuditFirstHandler extends AbstractAPIHandler {
    @Override
    protected void process(String req) {
        auditLog(req, "PRE");        // hack — calling out of order
        // real process logic
    }
}
```

This kind of hack works syntactically but **lies** about the algorithm. Readers see the template defining order auth → validate → process → log, but `AuditFirstHandler` secretly bends the order.

**Better choices when you outgrow the template:**

| Option | Description |
|---|---|
| **A — Add a second template method** | Define `handleRequestAuditFirst(...)` in the parent with the alternate order. Subclasses pick which to call. |
| **B — Switch to Strategy** | Make each step a strategy object; the host class composes them in whatever order you need. |
| **C — Multiple base classes** | Different templates for different families (e.g., `SyncHandler`, `AsyncHandler`). |

> **Rule:** When you find yourself wanting to "fight" the template's order, it means the algorithm isn't really fixed — Template Method is no longer the right pattern.

---

#### Q5 — What's the "Hollywood Principle"? Why does the lecture keep mentioning it?

**Short answer:** *"Don't call us, we'll call you."* In Template Method, the **base class** is in charge of the flow; the **subclass** doesn't call methods on the parent — the parent calls methods on the subclass at the right time.

**Normal inheritance flow** (the way most people write code):
```java
class Subclass extends Parent {
    public void doWork() {
        super.fetchData();      // SUBCLASS is in control — it calls parent
        // do my custom work
        super.cleanup();
    }
}
```
Subclass picks when to call each parent method. Subclass is in charge.

**Template Method flow** (inverted):
```java
class Parent {
    public final void doWork() {
        fetchData();      // PARENT is in control — it calls the subclass's method
        customStep();      // PARENT calls subclass at this point
        cleanup();
    }
    protected abstract void customStep();   // subclass fills this in
}
```
Subclass writes `customStep()`, but the PARENT decides *when* it runs. Subclass is passive; parent drives.

This is called **inversion of control**.

##### Why this matters in real life

Java frameworks are FULL of this pattern. You write methods; the framework calls them at the right time:

- **JUnit:** You write `@Test` methods. JUnit decides when to call them, in what order, with `@BeforeEach` and `@AfterEach` around them.
- **Servlet API:** You write `doGet()` and `doPost()`. The servlet container calls them.
- **Spring `@Scheduled`:** You write a method with the annotation. Spring calls it on a schedule.

Once you recognize Hollywood Principle, you understand why every Java framework feels the same way: **you don't drive the application; the framework drives, and you fill in the slots.**

> **Analogy:** A movie audition. You don't call the studio every day asking "Can I act now?" You audition once, then sit by the phone. The studio calls you when they need you. That's literally what "Don't call us, we'll call you" means.

---

#### Q6 — What if a subclass implements an abstract step BADLY? Will the compiler catch it?

**Short answer:** The compiler catches if you forget to implement an abstract method, but it can't check if your implementation FOLLOWS THE CONTRACT the parent expects. Sloppy implementations can break the algorithm silently — that's a Liskov Substitution Principle (LSP) violation.

##### What the compiler catches ✅

```java
class IncompleteHandler extends AbstractAPIHandler {
    // forgot to implement validate() and process()
}
// → COMPILE ERROR: "IncompleteHandler is not abstract and does not override
//    abstract method validate(String) in AbstractAPIHandler"
```

You can't even compile this. Good.

##### What the compiler does NOT catch ❌

```java
// CONTRACT in the parent's docs:
// "validate() MUST throw ValidationException if the request is invalid."

class SloppyHandler extends AbstractAPIHandler {
    @Override
    protected void validate(String req) {
        if (req == null) {
            return;       // silently returns instead of throwing! ❌
        }
        // real validation...
    }
}
```

Now `handle(null)` walks through:
1. `authenticate(null)` — runs
2. `validate(null)` — **silently passes** instead of throwing
3. `process(null)` — crashes deep inside with `NullPointerException`

The bug LOOKS like it's in `process`, but the real bug is in `validate`. This is an **LSP violation** — the subclass didn't honor the parent's contract.

##### Mitigations

**Mitigation 1 — Document the contract loudly:**
```java
/**
 * Validate the incoming request.
 * @throws ValidationException if the request is invalid (REQUIRED behavior).
 */
protected abstract void validate(String request);
```

**Mitigation 2 — Add a post-condition check in the template:**
```java
public final void handle(String req) {
    authenticate(req);
    validate(req);
    if (!isInGoodState(req)) {
        throw new IllegalStateException(
            "validate() returned without throwing for invalid input: " + req);
    }
    process(req);
}
```

**Mitigation 3 — Write a base test class:**
```java
abstract class AbstractAPIHandlerTest {
    abstract AbstractAPIHandler newHandler();

    @Test void validateRejectsNull() {
        assertThrows(ValidationException.class, () -> newHandler().validate(null));
    }
}
```
Every subclass test extends this — guarantees the contract is honored.

> **Rule:** With Template Method, the parent class makes promises about the algorithm's behavior. Each subclass must respect those promises. This is the cost of using inheritance — and the reason composition (Strategy) is often easier.

---

#### Q7 — Can one base class have MULTIPLE template methods?

**Short answer:** Yes — it's common when there are related algorithms sharing some steps.

Concrete example — an HTTP handler supporting both GET and POST:

```java
abstract class AbstractAPIHandler {

    // Two template methods, both final:
    public final void handleGet(String req) {
        authenticate(req);
        validateGet(req);
        String result = processGet(req);
        respondJson(result);
    }

    public final void handlePost(String req) {
        authenticate(req);
        validatePost(req);
        String result = processPost(req);
        respondJson(result);
    }

    // Shared concrete steps:
    protected void authenticate(String req)   { /* ... */ }
    protected void respondJson(String result) { /* ... */ }

    // GET-specific abstract steps:
    protected abstract void validateGet(String req);
    protected abstract String processGet(String req);

    // POST-specific abstract steps:
    protected abstract void validatePost(String req);
    protected abstract String processPost(String req);
}
```

Two template methods, four abstract steps, two shared concrete steps. Each subclass implements all four abstracts.

This is exactly how Java's `HttpServlet` works — `service()` is a single template method that *dispatches* to `doGet()`, `doPost()`, `doPut()`, etc. based on the HTTP method.

##### Trade-off

- ✅ Reuses shared steps (`authenticate`, `respondJson`).
- ⚠️ Now subclasses must implement 4 abstract methods. If only some subclasses handle both GET and POST, this becomes annoying.

If subclasses commonly handle only one HTTP method, split into two separate base classes instead. Don't force-fit multiple template methods if they don't share enough.

---

### 28.6 Trade-offs (with deep dive on the gotchas)

#### Pros

✅ **Eliminates duplication** — the skeleton lives in one place; subclasses inherit it for free.
✅ **Enforces consistency** — every subclass follows the same algorithm shape (guaranteed by `final`).
✅ **Hollywood Principle** — base class drives the flow; subclasses are passive contributors.
✅ **Easy to extend** — add a new variant = add a new subclass. No changes to the base class or sibling subclasses.

#### Cons (and how to mitigate each)

##### Con 1 — Inheritance lock-in (single inheritance problem)

Java allows only ONE superclass. If you already need `extends SomeOtherClass`, you can't ALSO `extends AbstractAPIHandler`.

```java
// You want:
class PaymentHandler extends AbstractAPIHandler, AbstractMonitored { ... }   // ❌ Java doesn't allow this
```

**Mitigations:**

- **Use composition + Strategy instead.** Make the algorithm pluggable rather than inheriting it. (Often the better choice in modern Java.)
- **Use Java's `default` methods on interfaces** (Java 8+). You can put a template method in an interface with default behavior; classes can `implements` multiple interfaces.
- **Delegate manually.** Have `PaymentHandler` hold an `AbstractAPIHandler` as a field and forward calls.

##### Con 2 — Liskov Substitution Risk (sloppy subclasses break the algorithm)

Covered in §28.5 Q6. The parent assumes the subclass honors its contract; sloppy implementations break the algorithm silently.

**Mitigations:** documentation, post-condition checks in the template, shared test base class.

##### Con 3 — Hard to debug across the inheritance hierarchy

When a bug shows up, you have to jump between parent and subclass to understand the flow:

```
PaymentHandler.handle(req)             ← actually defined in AbstractAPIHandler!
  → AbstractAPIHandler.handleRequest()
      → authenticate()                 ← in parent
      → validate()                     ← jumps to PaymentHandler
      → process()                      ← jumps to PaymentHandler
      → auditLog()                     ← back to parent
```

IDE "go to definition" helps, but readers can get lost.

**Mitigations:**

```java
public final void handleRequest(String request) {
    log.debug("[{}] start", getClass().getSimpleName());
    log.debug("[{}] → authenticate", getClass().getSimpleName());
    authenticate(request);
    log.debug("[{}] → validate", getClass().getSimpleName());
    validate(request);
    log.debug("[{}] → process", getClass().getSimpleName());
    String result = process(request);
    log.debug("[{}] → format+log", getClass().getSimpleName());
    String response = formatResponse(result);
    auditLog(request, response);
    log.debug("[{}] done", getClass().getSimpleName());
}
```

Now logs read like a flow trace, making it obvious where execution went:

```
[PaymentAPIHandler] start
[PaymentAPIHandler] → authenticate
[PaymentAPIHandler] → validate
[PaymentAPIHandler] → process
[PaymentAPIHandler] → format+log
[PaymentAPIHandler] done
```

##### Con 4 — Skeleton too rigid (when one subclass needs a different order)

Covered in §28.5 Q4. If you find yourself wanting to bend the order in a subclass, you've outgrown Template Method — refactor to Strategy.

##### Con 5 — Subclasses tightly coupled to parent's evolution

If the parent's algorithm changes (e.g., adds a new step), every subclass might break or behave unexpectedly. Subclasses are at the mercy of parent changes.

**Mitigation:** treat the template's "step list" as a public API contract. Don't add or remove steps casually. Use hooks for backward-compatible additions: a new hook with an empty default body doesn't break any existing subclass.

### 28.7 Real-World Industry Examples

1. **`java.util.AbstractList`** — defines `iterator()` template; subclasses fill in `get(index)` and `size()`.
2. **`java.io.InputStream.read(byte[], int, int)`** — calls the abstract `read()` for each byte.
3. **Servlet `HttpServlet.service()`** — template that calls `doGet`, `doPost`, etc. based on HTTP method.
4. **Spring's `JdbcTemplate.query(...)`** — template that opens connection, executes query, processes rows (abstract callback), closes connection.
5. **JUnit `setUp()` / `test()` / `tearDown()`** — each test follows the same skeleton; subclass provides specific steps.

### 28.8 Template Method — Interview Cheat Sheet

| Q | A |
|---|---|
| When use Template Method? | When several classes follow the same algorithm structure but differ in specific steps. |
| Difference vs Strategy? | Template uses inheritance + fixed skeleton. Strategy uses composition + fully replaceable algorithms. |
| Why `final` on the template method? | To prevent subclasses from changing the algorithm's structure. |
| What's a "hook"? | An optional method with default (empty) body that subclasses may override but don't have to. |
| Real Java example? | `HttpServlet.service()`, `JdbcTemplate`, `AbstractList`. |
| Hollywood Principle? | "Don't call us, we'll call you" — base class drives flow, subclasses provide pieces. |

### 28.9 TL;DR

> **Template Method = fixed skeleton, swappable steps.** Base class has a `final` template method that calls a sequence of steps. Common steps are concrete in the base class; varying steps are `abstract` and forced on subclasses. Subclasses can't change the order — they just fill in the gaps. Eliminates duplication and enforces consistency.

---

## 29. Lecture 3 — Patterns at a Glance

A consolidated reference comparing all 8 patterns from this lecture.

### 29.1 Structural Patterns Summary

| Pattern | Solves | Key idea | Real Java |
|---|---|---|---|
| **Adapter** | Incompatible interfaces | Wrap one to look like another | `Arrays.asList()`, `InputStreamReader` |
| **Facade** | Complex subsystem usage | One simple front door | `JdbcTemplate`, SLF4J |
| **Decorator** | Add behavior dynamically | Wrap object to stack features | `BufferedReader`, `Collections.unmodifiableList` |
| **Flyweight** | Memory waste from duplicates | Share intrinsic state, pass extrinsic | `Integer.valueOf()`, String pool |

### 29.2 Behavioral Patterns Summary

| Pattern | Solves | Key idea | Real Java |
|---|---|---|---|
| **Strategy** | Many ways to do same thing | Encapsulate each algorithm; swap at runtime | `Comparator`, `RejectedExecutionHandler` |
| **Observer** | One change → many reactions | Subscribe + notify all | `@EventListener`, RxJava |
| **Chain of Responsibility** | Multiple potential handlers | Pass request along chain till handled | Servlet `Filter`, Spring Security |
| **Template Method** | Same algo, different steps | Fixed skeleton + abstract steps | `HttpServlet`, `JdbcTemplate` |

### 29.3 When to use which? — Decision Hints

> **"I have an old API I can't change but my code needs a different shape."** → Adapter
> **"My subsystem is complex; clients shouldn't have to learn it."** → Facade
> **"I want to add features to objects without subclassing the world."** → Decorator
> **"I have millions of objects but most of their data is identical."** → Flyweight
> **"I want to swap algorithms at runtime."** → Strategy
> **"I want to broadcast a state change to many listeners."** → Observer
> **"I want to pass a request through a series of handlers, first one wins."** → Chain of Responsibility
> **"I want a fixed algorithm but with overridable steps."** → Template Method

### 29.4 Bonus — Pattern Pairings (often used together)

- **Strategy + Factory** — Factory creates the right Strategy based on input (e.g., choose payment strategy from user choice).
- **Decorator + Adapter** — Adapter to fit an old class into your interface; Decorator to add features on top.
- **Observer + Mediator** — Mediator coordinates communications; Observer notifies of changes.
- **Template Method + Strategy** — Template fixes the algorithm shape; specific steps could themselves be Strategy objects.
- **Facade + Singleton** — A Facade is often a Singleton (one front door per use case).

---

**Lecture 3 Complete.**

---

## 30. Design Patterns — Revision Cheat Sheet (ALL 12 Patterns)

> **Purpose:** This is your **one-page revision** for all design patterns. After reading the deep dives once, you should only need to come back to **this section** for fast revision before interviews. Skip the rest of the doc unless something feels shaky.
>
> **How to use it:**
> - **Daily (5 min):** Recite §30.1 — the one-liner problem each pattern solves.
> - **Weekly (30 min):** Recite §30.2 — the 4-slot recall card. Whiteboard one random pattern's skeleton from memory.
> - **Pre-interview (2 hr):** Read §30.3 sibling pairs + §30.4 keyword lookup. These are where points are won or lost.

---

### 30.1 The 12 Patterns in One Line Each

> **Goal:** Be able to recite "Pattern X solves Y" in under 2 seconds when an interviewer drops the name.

| # | Pattern | Type | Solves: "I want to..." |
|---|---|---|---|
| 1 | **Singleton** | Creational | ...have **exactly one** instance of a class, globally accessible. |
| 2 | **Factory** | Creational | ...pick **which class to instantiate** at runtime, without the client knowing the concrete type. |
| 3 | **Abstract Factory** | Creational | ...create **families of related products** that go together (e.g., Modern Chair + Modern Sofa). |
| 4 | **Builder** | Creational | ...build a **complex object step-by-step** with many optional fields, without telescoping constructors. |
| 5 | **Adapter** | Structural | ...make an **incompatible interface** fit my expected interface. |
| 6 | **Facade** | Structural | ...hide a **complex subsystem** behind one simple front-door interface. |
| 7 | **Decorator** | Structural | ...**add behavior** to an object dynamically without subclass explosion. |
| 8 | **Flyweight** | Structural | ...**save memory** by sharing identical objects via a cache. |
| 9 | **Strategy** | Behavioral | ...**swap algorithms at runtime** by injecting a function/interface. |
| 10 | **Observer** | Behavioral | ...**notify many dependents** when a subject changes. |
| 11 | **Chain of Responsibility** | Behavioral | ...pass a **request through a chain of handlers**, first one that can handles it. |
| 12 | **Template Method** | Behavioral | ...lock the **algorithm skeleton** in the parent; let subclasses fill in specific steps. |

---

### 30.2 The 4-Slot Recall Card (per pattern)

> **Goal:** For each pattern, lock four facts in memory. If you can recall these four, you can answer 90% of design-pattern interview questions.

#### Card 1 — Singleton

| Slot | Content |
|---|---|
| **Problem** | One instance, global access. |
| **Skeleton** | `private static instance; private ctor(); public static getInstance() { if (instance == null) instance = new X(); return instance; }` |
| **Sibling distinction** | vs **static class**: Singleton can implement interfaces, be lazy-loaded, swapped in tests. vs **global variable**: Singleton has controlled access (one entry point). |
| **JDK / framework** | `Runtime.getRuntime()`, `Spring @Bean` (default scope is singleton). |

#### Card 2 — Factory

| Slot | Content |
|---|---|
| **Problem** | Decide which concrete class to instantiate at runtime; hide construction from client. |
| **Skeleton** | `class XFactory { static Product create(String type) { return switch(type) { case "A" -> new A(); case "B" -> new B(); }; } }` |
| **Sibling distinction** | vs **Builder**: Factory picks WHICH class; Builder builds ONE class step-by-step. vs **Abstract Factory**: Factory creates 1 product; Abstract Factory creates a FAMILY. |
| **JDK / framework** | `Calendar.getInstance()`, `LoggerFactory.getLogger()`, `BeanFactory` in Spring. |

#### Card 3 — Abstract Factory

| Slot | Content |
|---|---|
| **Problem** | Create families of related products (must go together: Modern Chair + Modern Sofa, not Modern Chair + Victorian Sofa). |
| **Skeleton** | `interface FurnitureFactory { Chair createChair(); Sofa createSofa(); }` `class ModernFactory implements FurnitureFactory { ... }` |
| **Sibling distinction** | vs **Factory**: 1 product vs N related products. vs **Builder**: families vs single complex object. |
| **JDK / framework** | `DocumentBuilderFactory` (creates DocumentBuilder, which creates documents). |

#### Card 4 — Builder

| Slot | Content |
|---|---|
| **Problem** | Build complex object with many optional fields; avoid telescoping constructors. |
| **Skeleton** | `class Builder { Builder withX(...); Builder withY(...); Product build(); }` — chain calls return `this`. |
| **Sibling distinction** | vs **Factory**: same class, step-by-step vs different classes, one call. vs **Abstract Factory**: building one object vs picking a family. |
| **JDK / framework** | `StringBuilder`, `Stream.Builder`, `HttpClient.newBuilder()`, Lombok `@Builder`. |

#### Card 5 — Adapter

| Slot | Content |
|---|---|
| **Problem** | Make an incompatible class (legacy or third-party) fit your expected interface. |
| **Skeleton** | `class XAdapter implements MyTarget { private LegacyX legacy; public void myMethod() { legacy.differentMethodName(); } }` |
| **Sibling distinction** | vs **Decorator**: Adapter changes the interface; Decorator preserves it but adds behavior. vs **Facade**: Adapter wraps ONE class; Facade hides MANY. |
| **JDK / framework** | `Arrays.asList(T[])` (array → List), JDBC drivers (vendor APIs → `java.sql.Connection`). |

#### Card 6 — Facade

| Slot | Content |
|---|---|
| **Problem** | Hide a complex subsystem (many classes, ordering matters) behind one simple method. |
| **Skeleton** | `class XFacade { private SvcA a; private SvcB b; private SvcC c; public void simpleOp() { a.x(); b.y(); c.z(); } }` |
| **Sibling distinction** | vs **Adapter**: Facade simplifies many classes; Adapter just wraps one. Facade is about *ease*, Adapter is about *compatibility*. |
| **JDK / framework** | `java.net.URL` (hides socket + stream complexity), Spring `JdbcTemplate` (hides JDBC verbosity). |

#### Card 7 — Decorator

| Slot | Content |
|---|---|
| **Problem** | Add behavior to an object dynamically, in any combination, without subclass explosion (`RedBorderShadowCircle`, `ShadowCircle`, `RedBorderCircle` ...). |
| **Skeleton** | `class XDecorator implements Shape { private Shape inner; public void draw() { inner.draw(); /* extra */ } }` — wrap recursively: `new ShadowDeco(new BorderDeco(new Circle()))`. |
| **Sibling distinction** | vs **Adapter**: same interface vs different interface. vs **Chain of Responsibility**: Decorator ALWAYS runs through every layer; CoR stops at the first handler that can do it. |
| **JDK / framework** | `BufferedReader(new FileReader(...))`, `Collections.unmodifiableList(...)`, `InputStream` hierarchy. |

#### Card 8 — Flyweight

| Slot | Content |
|---|---|
| **Problem** | Save memory when you have millions of similar objects, by sharing the immutable parts via a cache. |
| **Skeleton** | `class Factory { static Map<Key, X> cache = new HashMap<>(); static X get(Key k) { return cache.computeIfAbsent(k, ...); } }` — objects must be **immutable** (intrinsic state). |
| **Sibling distinction** | vs **Singleton**: many shared instances (keyed) vs exactly ONE. vs **Cache**: Flyweight is for object identity & memory; cache is for compute-result re-use. |
| **JDK / framework** | `Integer.valueOf(int)` (caches -128 to 127), `String.intern()`, `Boolean.TRUE`/`FALSE`. |

#### Card 9 — Strategy

| Slot | Content |
|---|---|
| **Problem** | Swap an algorithm at runtime by injecting a function/interface; avoid `if/else` cascades over algorithm choice. |
| **Skeleton** | `class Context { Strategy s; setStrategy(s); execute() { s.run(); } }` — strategies are usually short-lived interfaces (often `@FunctionalInterface`, so lambdas work). |
| **Sibling distinction** | vs **Template Method**: composition vs inheritance — Strategy = swap the WHOLE algorithm; Template Method = override SPECIFIC STEPS in a fixed skeleton. vs **State**: client picks (Strategy) vs object switches itself based on state transitions (State). |
| **JDK / framework** | `Comparator` passed to `Collections.sort(list, comparator)`, Spring `TaskExecutor` (sync/async strategies). |

#### Card 10 — Observer

| Slot | Content |
|---|---|
| **Problem** | Notify multiple dependents (observers) automatically when a subject's state changes. |
| **Skeleton** | `class Subject { List<Observer> obs; addObserver(o); notifyAll() { for (o : obs) o.update(state); } }` |
| **Sibling distinction** | vs **Pub/Sub**: Observer has *direct references* (synchronous, in-process); Pub/Sub goes through a *broker* (async, cross-process). vs **Chain of Responsibility**: Observer fans out (all listeners notified); CoR is sequential (one handler wins). |
| **JDK / framework** | `PropertyChangeListener` (Beans), Swing event listeners, Spring `@EventListener`. |

#### Card 11 — Chain of Responsibility (CoR)

| Slot | Content |
|---|---|
| **Problem** | Pass a request along a chain of handlers; the first one that can handle it does so (or all do, if it's a pipeline). |
| **Skeleton** | `abstract class Handler { Handler next; final void handle(Req r) { if (canHandle(r)) doIt(r); else if (next != null) next.handle(r); } }` |
| **Sibling distinction** | vs **Decorator**: CoR stops at the first handler that handles; Decorator ALWAYS runs through every layer. vs **Strategy**: CoR is a list of *try-each*; Strategy is a *swap-the-one*. |
| **JDK / framework** | Servlet `Filter` chain, Spring Security filter chain, Java's exception propagation up the stack. |

#### Card 12 — Template Method

| Slot | Content |
|---|---|
| **Problem** | Lock the algorithm skeleton in the parent; let subclasses customize specific steps. |
| **Skeleton** | `abstract class Base { public final void run() { a(); b(); c(); } protected void a() {...} protected abstract void b(); protected void c() {...} }` — template method is `final`; steps are `protected`. |
| **Sibling distinction** | vs **Strategy**: inheritance + fixed skeleton vs composition + fully swappable algorithm. vs **CoR**: Template runs ALL steps in order; CoR stops at first handler. |
| **JDK / framework** | `HttpServlet.service()` (calls `doGet()`/`doPost()` template steps), `AbstractList`, JUnit `@BeforeEach`/`@Test`/`@AfterEach` (test lifecycle is a template). |

---

### 30.3 The Sibling-Pairs Trap (this is where points are LOST)

> **Why this matters:** Interviewers love to test "you know the names" vs "you understand the distinctions". Knowing one pattern isn't enough — you must articulate the precise difference vs its siblings.

| Confused pair | The 1-line distinction |
|---|---|
| **Factory vs Builder** | Factory = pick WHICH class to instantiate. Builder = same class, complex step-by-step. |
| **Factory vs Abstract Factory** | Factory = ONE product type. Abstract Factory = a FAMILY of related products that must go together. |
| **Adapter vs Facade** | Adapter = change interface of ONE class. Facade = simplify a SUBSYSTEM of many classes. |
| **Adapter vs Decorator** | Adapter = different interface (compatibility). Decorator = same interface (added behavior). |
| **Decorator vs CoR** | Decorator ALWAYS runs through every wrap. CoR can STOP at any handler. |
| **Strategy vs Template Method** | Strategy = composition (swap the whole algorithm). Template = inheritance (override specific steps in a fixed skeleton). |
| **Strategy vs State** | Strategy = CLIENT picks the algorithm. State = OBJECT switches its own behavior based on internal state transitions. |
| **Observer vs Pub/Sub** | Observer = direct references (in-process, synchronous). Pub/Sub = broker-mediated (cross-process, async). |
| **Observer vs CoR** | Observer = fan-out (all observers notified). CoR = sequential (one handler wins). |
| **Flyweight vs Singleton** | Flyweight = MANY shared instances keyed by something. Singleton = exactly ONE instance, no key. |
| **Flyweight vs Cache** | Flyweight = object identity (the same object is reused). Cache = compute-result reuse (the same answer is reused). |
| **Facade vs Mediator** | Facade = one-way (client → subsystem). Mediator = many-way (components ↔ mediator ↔ components). |

---

### 30.4 Problem Statement → Pattern (the keyword lookup)

> **Use this:** When you read or hear an interview problem statement, scan for these phrases. They almost always map to a specific pattern.

| Keyword / phrase in problem statement | Pattern to consider |
|---|---|
| "exactly one", "global", "shared resource" | **Singleton** |
| "create different types", "choose at runtime", "hide construction" | **Factory** |
| "family of related products", "must go together" | **Abstract Factory** |
| "many optional fields", "complex setup", "step-by-step construction" | **Builder** |
| "legacy", "third-party API", "incompatible interface", "wrap" | **Adapter** |
| "simplify", "complex subsystem", "one entry point", "many classes behind" | **Facade** |
| "add behavior dynamically", "wrap with features", "combine in any order" | **Decorator** |
| "millions of objects", "save memory", "identical immutable state", "shared instances" | **Flyweight** |
| "swap algorithm at runtime", "pluggable behavior", "multiple algorithms for same task" | **Strategy** |
| "notify dependents", "broadcast change", "subscribers", "event listeners" | **Observer** |
| "pass through handlers", "first one wins", "approval levels", "filter chain" | **Chain of Responsibility** |
| "fixed algorithm with overridable steps", "skeleton", "lifecycle hooks" | **Template Method** |

---

### 30.5 The 30-Second Interview Pitch Template

> **Use this:** When asked "Tell me about pattern X" — fill in the 4 blanks with §30.2's card content.

```
"Pattern X solves [problem in 1 sentence].
The structure is [1-line code skeleton].
Compared to [closest sibling], the key difference is [1-line distinction].
A real-world example is [JDK / framework].
The main trade-off is [1-line con]."
```

**Example — Decorator:**

> "Decorator solves the problem of adding behavior to an object dynamically without subclass explosion. The structure is: a decorator class implements the same interface as the wrapped object, holds it as a field, and adds extra behavior in its overrides. Compared to Adapter, the key difference is Decorator preserves the interface and adds behavior, while Adapter changes the interface. A real-world example is Java IO streams — `BufferedReader(new FileReader(...))` is a stack of decorators. The main trade-off is many small classes that can be hard to debug when stacked deeply."

That's exactly 30 seconds spoken. Lock this pattern into muscle memory.

---

### 30.6 Decision Tree — Which Pattern Should I Suggest?

```
Q1: Is the question about CREATING objects?
├── Need exactly one instance? ─────────────────► Singleton
├── Pick the class to create at runtime? ──────► Factory
├── Need a family of related products? ────────► Abstract Factory
└── Complex construction, many fields? ────────► Builder

Q2: Is the question about adapting / composing CLASSES?
├── Make incompatible class fit? ──────────────► Adapter
├── Hide complex subsystem? ───────────────────► Facade
├── Add behavior dynamically (combinable)? ────► Decorator
└── Share identical objects to save memory? ──► Flyweight

Q3: Is the question about object BEHAVIOR / interaction?
├── Swap algorithm at runtime? ────────────────► Strategy
├── Notify many dependents on change? ─────────► Observer
├── Pass request through handlers? ────────────► Chain of Responsibility
└── Fixed algorithm, customizable steps? ─────► Template Method
```

---

### 30.7 SOLID Principles — One-Line Cheat Sheet

> **Why included:** Interviewers often pair pattern questions with SOLID questions ("does this violate any principles?"). This is your quick recall.

| Principle | One-line meaning | What violates it |
|---|---|---|
| **S** — Single Responsibility | A class should have one reason to change. | A class doing BOTH validation AND persistence. |
| **O** — Open / Closed | Open for extension, closed for modification. | Adding a new product means editing the factory's switch. |
| **L** — Liskov Substitution | Subtype must be substitutable for parent without breaking caller. | Child weakens postcondition (e.g., silently returns instead of throwing). |
| **I** — Interface Segregation | Don't force clients to depend on methods they don't use. | A fat interface with 20 methods; most impls throw `UnsupportedOperationException` for half. |
| **D** — Dependency Inversion | Depend on abstractions, not concretions. | Client `new`s up a concrete `MySQLDatabase` instead of accepting a `Database` interface. |

---

### 30.8 Quick-Fire Interview Q&A (the questions you WILL be asked)

> **Drill these.** Spend 1 minute on each before any LLD interview.

| Question | Tight answer |
|---|---|
| "Pattern not always needed?" | Yes — over-using patterns is itself a code smell. Patterns solve specific problems; reach for them only when the problem appears. |
| "Difference between OOP and a design pattern?" | OOP is the language feature (classes, inheritance); a design pattern is a *reusable solution* to a recurring design problem using OOP. |
| "How does Spring use patterns?" | Singleton (default `@Bean` scope), Factory (`BeanFactory`), Proxy (AOP / transactions), Template Method (`JdbcTemplate`), Strategy (`TaskExecutor`). |
| "Which pattern violates which SOLID principle?" | Basic Factory's switch-case violates **OCP** locally (new product = edit switch). Strategy/Abstract Factory variants restore OCP via polymorphism. |
| "Composition over inheritance — what's an example?" | Strategy uses composition (inject algorithm); Template Method uses inheritance (subclass overrides steps). Strategy is more flexible because algorithms can be swapped at runtime. |
| "What if I need to add a new product / strategy / observer?" | If the existing factory/observer/strategy was designed via polymorphism (registry, DI, or sub-classing), adding new ones is OCP-friendly — no edits to existing code. If it uses switch-case, you'll have to modify the existing class. |
| "Which patterns are thread-unsafe by default?" | Singleton (needs lazy-init synchronization), Builder (mutable while building — don't share builder across threads), Observer (notification on shared list — use `CopyOnWriteArrayList`). |

---

### 30.9 The "Final Mile" Checklist (15 mins before the interview)

Read these three things:

1. **§30.1** — Recite all 12 one-liner problems.
2. **§30.3** — Read the sibling-pairs table. (This is where you win or lose points.)
3. **§30.4** — Skim the keyword lookup. This is what helps you SPOT the pattern when the interviewer disguises it in a problem statement.

If you can do those three from memory, you're ready.

---

*Next lecture notes will be appended below this line.*

---
