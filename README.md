# Restaurant Management System

### A Java CLI application demonstrating modern OOP design and core Java language features

-----

## Overview

A command-line restaurant management system modelled around real-world operations — customer and staff registration, table management, multi-payment-method order processing, and lambda-based order filtering. Built as a structured demonstration of object-oriented design principles and modern Java features.

-----

## Java Features Demonstrated

|Feature                                  |Where Used                                                                |
|-----------------------------------------|--------------------------------------------------------------------------|
|**Records**                              |`MenuItemRecord`, `ImmutableAddress` — immutable data carriers            |
|**Sealed Classes**                       |`Payment` sealed hierarchy — `CardPayment`, `CashPayment`, `OnlinePayment`|
|**Interfaces**                           |`Identifiable` — shared identity contract across domain objects           |
|**Inheritance**                          |`Person` → `Customer`, `Staff` — shared person abstraction                |
|**Custom Exceptions**                    |`TableNotAvailableException`, `InvalidOrderException`                     |
|**Functional Interface (Predicate)**     |`filterOrders(Predicate<Order>)` — lambda-based order filtering           |
|**Strategy Pattern**                     |`DiscountPolicy` — pluggable discount logic in service layer              |
|**Enums**                                |`Category`, `TableStatus`, `OrderStatus`, `PaymentMethod`, `Role`         |
|**Varargs**                              |`createOrder(Customer, int, MenuItemRecord...)`                           |
|**Local Variable Type Inference (`var`)**|Used throughout service and model layers                                  |
|**`java.time` API**                      |`LocalDateTime` for immutable order timestamping                          |
|**Generics**                             |`List<Order>`, `List<Customer>`, `Predicate<Order>`                       |

-----

## Project Structure

```
src/main/java/restaurant/
├── model/
│   ├── Person.java               # Abstract base — shared by Customer and Staff
│   ├── Customer.java             # Extends Person
│   ├── Staff.java                # Extends Person, carries Role
│   ├── Role.java                 # Enum — staff roles
│   ├── Identifiable.java         # Interface — identity contract
│   ├── ImmutableAddress.java     # Record — value object
│   ├── Table.java
│   ├── TableStatus.java          # Enum
│   ├── Order.java
│   ├── OrderStatus.java          # Enum
│   ├── MenuItemRecord.java       # Record — name, price, category
│   ├── Category.java             # Enum — menu item categories
│   ├── Payment.java              # Sealed class
│   ├── CardPayment.java          # Permits Payment
│   ├── CashPayment.java          # Permits Payment
│   ├── OnlinePayment.java        # Permits Payment
│   └── PaymentMethod.java        # Enum
├── service/
│   ├── RestaurantService.java    # Core business logic
│   └── DiscountPolicy.java       # Functional interface — pluggable discounts
├── exception/
│   ├── TableNotAvailableException.java
│   └── InvalidOrderException.java
└── MainApp.java
```

-----

## Key Design Decisions

**Sealed `Payment` hierarchy**
Payments can only be `CardPayment`, `CashPayment`, or `OnlinePayment` — the sealed class ensures no unexpected subtypes can be introduced. Combined with pattern matching, this makes payment processing exhaustive and compiler-verified.

**`MenuItemRecord` and `ImmutableAddress` as Records**
Both are pure data carriers with no behaviour. Records eliminate boilerplate and enforce immutability by design — the right tool for value objects.

**`DiscountPolicy` as a functional interface**
Discount logic is pluggable without modifying `RestaurantService`. Any discount rule (loyalty, seasonal, staff) can be passed as a lambda at the call site.

```java
// Apply 10% loyalty discount
service.applyDiscount(order, total -> total * 0.90);

// No discount
service.applyDiscount(order, total -> total);
```

**`Predicate<Order>` for filtering**
A single `filterOrders` method handles any filter combination without extra methods on the service class.

```java
// Orders above €50
service.filterOrders(o -> o.calculateTotal() > 50);

// Pending orders for a specific customer
service.filterOrders(o -> o.getStatus() == OrderStatus.PENDING && o.getCustomer().equals(c));
```

-----

## How to Run

**Requirements:** Java 17+, Maven

```bash
git clone https://github.com/AdityaPatil156/Restaurant-management-java.git
cd Restaurant-management-java
mvn compile
mvn exec:java -Dexec.mainClass="restaurant.MainApp"
```

-----

## Author

**Aditya Patil** — MSc Software Design with Artificial Intelligence, Technological University of Shannon
