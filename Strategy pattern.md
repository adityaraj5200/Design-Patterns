## Strategy Pattern

### Definition

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from the clients that use it.

---

## Problem It Solves

When:

* A class has multiple ways to perform the same operation
* Behavior changes based on conditions (`if-else`, `switch`)
* Adding a new behavior requires modifying existing code

Common symptoms:

* Long `if-else` / `switch` blocks
* Violation of Open–Closed Principle
* Hard-to-test and hard-to-extend logic

---

## Core Idea

* Extract varying behavior into separate classes
* Program to an interface, not an implementation
* Delegate behavior instead of hard-coding it

---

## Structure

### Participants

* **Strategy (Interface)**
  Declares the algorithm interface
* **ConcreteStrategy**
  Implements a specific algorithm
* **Context**
  Uses a Strategy and delegates work to it

---

## UML (Conceptual)

```
Context
  |
  | has-a
  v
Strategy (interface)
  |
  | implements
  v
ConcreteStrategyA
ConcreteStrategyB
```

---

## Example Scenario: Payment Processing

### Without Strategy (Problematic)

```java
class PaymentService {
    void pay(String method) {
        if (method.equals("CARD")) {
            // card logic
        } else if (method.equals("UPI")) {
            // upi logic
        } else if (method.equals("WALLET")) {
            // wallet logic
        }
    }
}
```

Issues:

* New payment method → modify existing code
* Logic tightly coupled
* Poor testability

---

## With Strategy Pattern

### Strategy Interface

```java
public interface PaymentStrategy {
    void pay(int amount);
}
```

### Concrete Strategies

```java
public class CardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Card");
    }
}

public class UpiPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}
```

### Context

```java
public class PaymentService {
    private PaymentStrategy paymentStrategy;

    public PaymentService(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void makePayment(int amount) {
        paymentStrategy.pay(amount);
    }
}
```

### Usage

```java
PaymentService service =
    new PaymentService(new CardPayment());
service.makePayment(500);
```

---

## Key Characteristics

* Behavior selected **at runtime**
* Eliminates conditional logic
* Promotes composition over inheritance
* Each strategy is independently testable

---

## When to Use Strategy Pattern

Use when:

* Multiple interchangeable behaviors exist
* Behavior needs to change at runtime
* You want to avoid conditionals
* You expect frequent addition of new behaviors

Avoid when:

* Only one behavior exists
* Algorithms are extremely simple and unlikely to change

---

## Strategy vs Inheritance

| Aspect          | Strategy | Inheritance    |
| --------------- | -------- | -------------- |
| Behavior change | Runtime  | Compile-time   |
| Flexibility     | High     | Low            |
| Coupling        | Loose    | Tight          |
| OCP compliance  | Yes      | Often violated |

---

## Real-World Examples

* Sorting algorithms (`Comparator`)
* Compression strategies (ZIP, GZIP)
* Authentication methods
* Rate limiting algorithms
* Recommendation algorithms

---

## Key Interview Takeaways

* Strategy encapsulates algorithms, not objects
* Client depends on abstraction, not implementation
* Replaces conditional logic with polymorphism
* Follows Open–Closed Principle and Single Responsibility Principle
