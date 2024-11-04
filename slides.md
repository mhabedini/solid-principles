---
# You can also start simply with 'default'
theme: bricks

# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
layout: cover
title: SOLID Principles in TypeScript
description: An interactive tutorial on applying SOLID principles with real-world examples and refactoring techniques in TypeScript
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

# Solid Principles

A guide for a clean code

---
transition: fade-out
---

## SOLID Principles Overview

The SOLID principles are five key guidelines for software development that aim to make systems easier to maintain, understand, and extend. Following these principles can lead to better-organized code that is less prone to errors and easier to adapt to changes. This presentation outlines each principle with examples in TypeScript, illustrating how they can be applied in practice.

<br>

### The SOLID Principles:
1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

---
transition: slide-up
level: 2
---

## 1. Single Responsibility Principle (SRP)

<br>

> **Definition**: A class should have only one reason to change, meaning it should have only one responsibility. This principle encourages the separation of concerns.

<br>
<br>
<br>

### Why SRP Matters
- Reduces complexity by isolating functionalities.
- Enhances maintainability: changes to one responsibility won't affect others.
- Improves testability: each class can be tested independently.

---
transition: slide-up
---

## SRP Example – Before

Example: An `Order` class handling multiple responsibilities.

````md magic-move {lines: true}

```typescript  {*|1|8-11|13-15|17-19}
class Order {
  items: string[];

  constructor(items: string[]) {
    this.items = items;
  }

  calculateTotal(): number {
    // Logic for calculating total price
    return 0;
  }

  processPayment(): void {
    // Logic for processing payment
  }

  sendConfirmationEmail(): void {
    // Logic for sending email
  }
}
```

```typescript
class Order {
    items: string[];
    constructor(items: string[]) {
        this.items = items;
    }
}
class OrderCalculator {
    calculateTotal(order: Order): number {
        return 0;
    }
}
class PaymentProcessor {
    processPayment(order: Order): void {
    }
}
class EmailService {
    sendConfirmationEmail(order: Order): void {
    }
}
```
````

---
level: 2
---

## 2. Open/Closed Principle (OCP)

<br>

> **Definition** Software entities should be open for extension but closed for modification. This principle promotes the idea that existing code should not be altered but can be extended to introduce new features.

<br>
<br>
<br>

### Why OCP Matters
- Reduces the risk of introducing bugs in existing code.
- Enhances flexibility by allowing new functionality to be added easily.
- Encourages the use of interfaces and abstract classes to define common behavior.

---

# OCP Example


````md magic-move {lines: true}
```typescript {*|3|6|10}
class DiscountCalculator {
    applyDiscount(order: Order, discountType: string): number {
        if (discountType === "percentage") {
            // Apply percentage discount
            return 0;
        } else if (discountType === "fixed") {
            // Apply fixed discount
            return 0;
        }
        return 0;
    }
}
```

```typescript {*|1-3|5-9|11-15|17-21|*}
interface Discount {
    apply(order: Order): number;
}

class PercentageDiscount implements Discount {
    apply(order: Order): number {
        return 0;
    }
}

class FixedDiscount implements Discount {
    apply(order: Order): number {
        return 0;
    }
}

class DiscountCalculator {
    applyDiscount(order: Order, discount: Discount): number {
        return discount.apply(order);
    }
}
```
````
---
---

## Liskov Substitution Principle (LSP)

<br>

> **definition** Subtypes should be substitutable for their base types without altering the correctness of the program. This principle ensures that derived classes can stand in for their base classes.

<br>
<br>
<br>

### Why LSP Matters

- Encourages a consistent interface for all subclasses.
- Prevents the introduction of errors when using polymorphism.
- Helps maintain the integrity of the codebase.

---

## LSP Example

<br>

````md magic-move {lines: true}
```typescript
class Bird {
    fly(): void {
        console.log("Flying!");
    }
}

class Sparrow extends Bird {
    fly(): void {
        console.log("Sparrow flying!");
    }
}

class Penguin extends Bird {
    fly(): void {
        throw new Error("Penguins can't fly!");
    }
}
```

```typescript
class Bird {
    eat(): void {
        console.log("Eating!");
    }
}

class FlyingBird extends Bird {
    fly(): void {
        console.log("Flying!");
    }
}

class Sparrow extends FlyingBird {
    fly(): void {
        console.log("Sparrow flying!");
    }
}

class Penguin extends Bird {
    // No fly method since penguins cannot fly
}
```
````
---

## 4. Interface Segregation Principle (ISP)

<br>

> **Definition** No client should be forced to depend on methods it does not use. This principle advocates for creating smaller, more specific interfaces.

<br>
<br>
<br>

### Why ISP Matters
- Reduces the impact of changes to interfaces.
- Encourages a clean separation of concerns.
- Improves flexibility by allowing implementations to only require what they need.

---

## ISP Example

````md magic-move {lines: true}
```typescript
interface Worker {
    work(): void;
    eat(): void;
}

class HumanWorker implements Worker {
    work(): void {
        console.log("Human working");
    }
    eat(): void {
        console.log("Human eating");
    }
}

class RobotWorker implements Worker {
    work(): void {
        console.log("Robot working");
    }
    eat(): void {
        throw new Error("Robots don't eat");
    }
}
```

```typescript
interface Workable {
    work(): void;
}
interface Eatable {
    eat(): void;
}

class HumanWorker implements Workable, Eatable {
    work(): void {
        console.log("Human working");
    }

    eat(): void {
        console.log("Human eating");
    }
}
class RobotWorker implements Workable {
    work(): void {
        console.log("Robot working");
    }
}
```
````

---

## 5. Dependency Inversion Principle (DIP)

<br>

> **Definition**  High-level modules should not depend on low-level modules. Both should depend on abstractions. This principle emphasizes the importance of decoupling.

<br>
<br>
<br>

### Why DIP Matters
- Promotes the use of dependency injection.
- Reduces the coupling between components, making the system more modular.
- Facilitates testing by allowing the use of mock implementations.

---

## DIP Example

````md magic-move {lines: true}
```typescript
class PDFGenerator {
    generate(data: any): void {
        console.log("Generating PDF...");
    }
}

class Invoice {
    private data: any;
    private pdfGenerator: PDFGenerator;

    constructor(data: any) {
        this.data = data;
        this.pdfGenerator = new PDFGenerator();
    }

    print(): void {
        this.pdfGenerator.generate(this.data);
    }
}
```

```typescript
interface DocumentGenerator {
    generate(data: any): void;
}
class PDFGenerator implements DocumentGenerator {
    generate(data: any): void {
    }
}
class HTMLGenerator implements DocumentGenerator {
    generate(data: any): void {
    }
}
class Invoice {
    private data: any;
    private generator: DocumentGenerator;
    constructor(data: any, generator: DocumentGenerator) {
        this.data = data;
        this.generator = generator;
    }
    print(): void {
        this.generator.generate(this.data);
    }
}
```
````

---

## Recap of SOLID Principles

<br>

1. Single Responsibility Principle (SRP): One responsibility per class.
2. Open/Closed Principle (OCP): Open for extension, closed for modification.
3. Liskov Substitution Principle (LSP): Subtypes are substitutable.
4. Interface Segregation Principle (ISP): Role-specific interfaces.
5. Dependency Inversion Principle (DIP): Depend on abstractions, not concrete classes.


---
layout: center
---
### Conclusion

Applying the SOLID principles helps developers create code that is easier to manage, more adaptable to change, and better aligned with real-world requirements. By adhering to these principles, teams can improve their software development practices, leading to more robust applications.

