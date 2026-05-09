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

**Key point:** `balance` is `private`. You can't do `account.balance = -500`. The class enforces its own invariants.

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

The caller just calls `shape.draw()` — they don't care HOW each shape draws itself.

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

This single class handles snakes, ladders, the board, players, movement, AND winning logic. Too many responsibilities!

**Approach 2 (Follows SRP):** Break into smaller classes, each with one job.

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

Each class has exactly **one responsibility**: `Snake` knows about snake positions, `Board` handles the board and movement, `Game` manages game state.

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
```

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

> **System Design is a process of defining the architecture, components, modules, interfaces, and data for a system to satisfy specified requirements.**

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

**When to use:** When there's a true taxonomic relationship and the child genuinely IS a specialized version of the parent.

### 8.2 HAS-A Relationship (Association)

**What:** One class contains a reference to another. Represents "uses" or "has" relationships.

**Two forms:**

**Aggregation (weak HAS-A):**
```java
class University {
    private List<Student> students;  // University HAS students
    // But students exist independently
}
```

**Composition (strong HAS-A):**
```java
class House {
    private List<Room> rooms;

    public House(int numRooms) {
        rooms = new ArrayList<>();
        for (int i = 0; i < numRooms; i++) {
            rooms.add(new Room());  // Rooms created WITH the house
        }
    }
    // When House is garbage collected, Rooms go too
}
```

### 8.3 Generalization

**What:** Extracting shared behavior from multiple classes into a common parent. It's the "bottom-up" way of thinking about IS-A.

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

### 8.4 Realization

**What:** A class implements an interface, promising to provide concrete behavior for the interface's contract.

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

**Generalization vs Realization:**
| | Generalization | Realization |
|-|---------------|-------------|
| **Keyword** | `extends` | `implements` |
| **Parent type** | Class (concrete or abstract) | Interface |
| **Inherits** | State + behavior | Only contract (method signatures) |
| **UML arrow** | Solid line + hollow triangle | Dashed line + hollow triangle |

### 8.5 Composition

**What:** A strong form of HAS-A where the child object's lifecycle is entirely controlled by the parent. The part cannot exist without the whole.

**UML:** Solid line with filled diamond on the parent side.

```java
class Engine {
    private String type;
    public void start() { System.out.println("Engine running"); }
}

class Car {
    private Engine engine;  // Engine is PART of Car

    public Car() {
        this.engine = new Engine();  // Car creates its own engine
    }
    // Engine doesn't make sense without a Car (in this design)
}
```

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

*Next lecture notes will be appended below this line.*

---
