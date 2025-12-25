## Builder Design Pattern

### Definition

The Builder Pattern separates the construction of a complex object from its representation so that the same construction process can create different representations.

---

## Problem It Solves

When:

* An object has many optional parameters
* Constructor overloading becomes unmanageable
* Object creation logic becomes complex
* You want immutability but flexible object creation

Common symptoms:

* Telescoping constructors
* Constructors with many parameters (some nullable)
* Low readability and high error risk

---

## Core Idea

* Move object construction logic into a separate builder
* Build the object step by step
* Final object is created only when fully configured

---

## Structure

### Participants

* **Product**
  The complex object being built
* **Builder**
  Defines steps to build the product
* **ConcreteBuilder**
  Implements the steps
* **Director (Optional)**
  Orchestrates construction steps

---

## UML (Conceptual)

```
Director --> Builder --> Product
```

---

## Example Scenario: User Object

### Problematic Approach (Telescoping Constructor)

```java
class User {
    String name;
    String email;
    String phone;
    int age;
    String address;

    User(String name) { ... }
    User(String name, String email) { ... }
    User(String name, String email, String phone) { ... }
}
```

Issues:

* Poor readability
* Hard to maintain
* Error-prone parameter ordering

---

## Builder Pattern Implementation

### Product

```java
public class User {
    private final String name;
    private final String email;
    private final String phone;
    private final int age;

    private User(Builder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.phone = builder.phone;
        this.age = builder.age;
    }

    public static class Builder {
        private final String name;      // required
        private String email;           // optional
        private String phone;
        private int age;

        public Builder(String name) {
            this.name = name;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

### Usage

```java
User user = new User.Builder("Aditya")
                .email("a@x.com")
                .age(25)
                .build();
```

---

## Key Characteristics

* Step-by-step object construction
* Fluent API
* Supports immutability
* Clear separation of construction and representation

---

## Builder vs Constructor

| Aspect          | Constructor | Builder     |
| --------------- | ----------- | ----------- |
| Readability     | Low         | High        |
| Optional fields | Poor        | Excellent   |
| Immutability    | Hard        | Easy        |
| Validation      | Limited     | Centralized |

---

## Builder vs Factory

| Aspect            | Builder      | Factory                |
| ----------------- | ------------ | ---------------------- |
| Focus             | How to build | Which object to create |
| Object complexity | High         | Low–Medium             |
| Steps             | Multiple     | Single                 |

---

## Director (Optional)

Used when:

* Construction steps are fixed
* Same build process creates multiple variations

Example:

```java
class UserDirector {
    User createAdmin() {
        return new User.Builder("Admin")
                .age(30)
                .build();
    }
}
```

Often skipped in real-world Java code.

---

## When to Use Builder Pattern

Use when:

* Many optional parameters
* Object creation is complex
* Need readable and maintainable code
* Need immutable objects

Avoid when:

* Objects are simple
* Few parameters with no optionality

---

## Real-World Examples

* `StringBuilder`
* `StringBuffer`
* `HttpRequest.Builder`
* `AlertDialog.Builder`
* Lombok `@Builder`

---

## Key Interview Takeaways

* Builder handles **object construction complexity**
* Prevents telescoping constructors
* Enables immutability + flexibility
* Improves readability and maintainability
