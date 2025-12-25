## Factory Method Design Pattern

![Image](https://i.sstatic.net/N3mC1.png)

![Image](https://miro.medium.com/1%2AMgGanvd3o--sK1q9W05odw.jpeg)

---

## 1. Definition

**Factory Method** is a **creational design pattern** that:

* Defines an **interface (or abstract class) for creating an object**
* Lets **subclasses decide which concrete class to instantiate**
* **Decouples object creation from object usage**

In short:

> *“Let subclasses decide which object to create.”*

---

## 2. Problem It Solves (Motivation)

### Without Factory Method

* Code directly instantiates concrete classes using `new`
* Tight coupling between **client code** and **concrete implementations**
* Adding a new type requires:

  * Modifying existing logic
  * Violating **Open–Closed Principle**

### Example Problem

```java
if (type.equals("EMAIL")) {
    return new EmailNotification();
} else if (type.equals("SMS")) {
    return new SmsNotification();
}
```

Problems:

* Every new notification type → modify this `if-else`
* Hard to test
* Hard to extend
* High coupling

---

## 3. Core Idea

* Move **object creation logic** into a **factory method**
* Allow subclasses to override the factory method
* Client code depends only on **abstractions**, not concrete classes

---

## 4. Structure (Participants)

| Component           | Responsibility                                       |
| ------------------- | ---------------------------------------------------- |
| **Product**         | Common interface / abstract class                    |
| **ConcreteProduct** | Actual implementations                               |
| **Creator**         | Declares factory method                              |
| **ConcreteCreator** | Overrides factory method to create specific products |

---

## 5. UML-Level Understanding

```
           Creator
        + factoryMethod()
              |
      ---------------------
      |                   |
ConcreteCreatorA   ConcreteCreatorB
      |                   |
ConcreteProductA  ConcreteProductB
```

Key insight:

* `Creator` **does not know** which concrete product it creates
* Creation is deferred to subclasses

---

## 6. Example (Java – Notification System)

### Step 1: Product Interface

```java
public interface Notification {
    void send();
}
```

---

### Step 2: Concrete Products

```java
public class EmailNotification implements Notification {
    public void send() {
        System.out.println("Sending Email Notification");
    }
}

public class SmsNotification implements Notification {
    public void send() {
        System.out.println("Sending SMS Notification");
    }
}
```

---

### Step 3: Creator (Abstract Class)

```java
public abstract class NotificationFactory {

    // Factory Method
    protected abstract Notification createNotification();

    // Business logic uses the product
    public void notifyUser() {
        Notification notification = createNotification();
        notification.send();
    }
}
```

---

### Step 4: Concrete Creators

```java
public class EmailNotificationFactory extends NotificationFactory {
    protected Notification createNotification() {
        return new EmailNotification();
    }
}

public class SmsNotificationFactory extends NotificationFactory {
    protected Notification createNotification() {
        return new SmsNotification();
    }
}
```

---

### Step 5: Client Code

```java
public class Client {
    public static void main(String[] args) {
        NotificationFactory factory = new EmailNotificationFactory();
        factory.notifyUser();

        factory = new SmsNotificationFactory();
        factory.notifyUser();
    }
}
```

---

## 7. Key Characteristics

### What changes?

* **Creation logic varies**
* **Usage logic stays the same**

### What stays stable?

* Client code
* Abstractions
* Business flow

---

## 8. Factory Method vs Simple Factory

| Simple Factory                | Factory Method                   |
| ----------------------------- | -------------------------------- |
| Uses `if-else` / `switch`     | Uses inheritance                 |
| Not a GoF pattern             | GoF pattern                      |
| Violates OCP                  | Follows OCP                      |
| Single class handles creation | Creation delegated to subclasses |

---

## 9. When to Use Factory Method

Use it when:

* Object creation logic is **complex or varies**
* You want to:

  * Follow **Open–Closed Principle**
  * Avoid tight coupling
* Framework/library design
* You don’t want client code to depend on concrete classes

Common real-world use cases:

* Logging frameworks
* Notification systems
* Payment gateways
* UI component creation
* Driver/connector creation

---

## 10. Real-World Java Examples

### Java SDK

* `Iterator()` in collections
* `Calendar.getInstance()`
* `NumberFormat.getInstance()`

### Spring Framework

* `FactoryBean`
* Bean creation via configuration

---

## 11. Advantages

* Loose coupling
* Better extensibility
* Follows SOLID principles:

  * **OCP**
  * **DIP**
* Cleaner, testable code

---

## 12. Disadvantages

* Increases number of classes
* Slightly more complex than simple factory
* Overkill for very small systems

---

## 13. Factory Method vs Abstract Factory (Quick Contrast)

| Factory Method          | Abstract Factory                 |
| ----------------------- | -------------------------------- |
| Creates **one product** | Creates **families of products** |
| Uses inheritance        | Uses composition                 |
| One factory method      | Multiple factory methods         |

---

## 14. Mental Model (Interview-Friendly)

> “Factory Method encapsulates object creation by defining a factory method in a base class and allowing subclasses to decide which concrete implementation to instantiate, enabling loose coupling and easy extensibility.”

---

## 15. Common Interview Follow-ups

* Why not use `new`?
* Difference between Factory Method and Abstract Factory
* How does it follow OCP?
* When would you not use it?
* How is this different from dependency injection?

---

If you want, next we can:

* Implement this with **Spring**
* Convert a real `if-else` codebase into Factory Method
* Compare **Factory Method vs Strategy**
* Practice **LLD interview questions using this pattern**
