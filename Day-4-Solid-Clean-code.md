# SOLID Principles & Clean Code in TypeScript and React Native

## Overview

In this study material, we'll dive into **SOLID principles** and **Clean Code guidelines** in the context of **TypeScript** and **React Native** development. The goal is to ensure you understand how to write **scalable**, **modular**, and **maintainable** code, while avoiding common anti-patterns.

---

## Table of Contents

1. [SOLID Principles](#solid-principles)
   - Single Responsibility Principle (SRP)
   - Open/Closed Principle (OCP)
   - Liskov Substitution Principle (LSP)
   - Interface Segregation Principle (ISP)
   - Dependency Inversion Principle (DIP)
   
2. [Clean Code Guidelines](#clean-code-guidelines)
   - Writing Clean Code
   - Naming Conventions
   - Code Structure and Organization
   - Avoiding Anti-Patterns
   - Refactoring Techniques

---

## 1. SOLID Principles

### **1.1 Single Responsibility Principle (SRP)**

The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have only one job or responsibility. This principle is essential for maintainability and helps avoid complex, tightly-coupled code.

**Example**:

```typescript
// Before SRP
class UserManager {
  createUser(name: string, email: string) {
    // logic to create user
  }
  
  sendEmail(email: string, message: string) {
    // logic to send email
  }
}

// After SRP
class UserManager {
  private emailService: EmailService;

  constructor() {
    this.emailService = new EmailService();
  }

  createUser(name: string, email: string) {
    // logic to create user
    this.emailService.sendEmail(email, "Welcome!");
  }
}

class EmailService {
  sendEmail(email: string, message: string) {
    // logic to send email
  }
}
```
By splitting responsibilities, you can make sure each class or service does only one thing and is easier to maintain.

### **1.2 Open/Closed Principle (OCP)**
The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification. This encourages building systems that are flexible and can be extended without altering existing code.

Example:
```typescript
// Before OCP
class DiscountService {
  calculateDiscount(customerType: string, amount: number): number {
    if (customerType === 'premium') {
      return amount * 0.2;
    } else if (customerType === 'regular') {
      return amount * 0.1;
    }
    return 0;
  }
}

// After OCP
interface Discount {
  calculate(amount: number): number;
}

class PremiumDiscount implements Discount {
  calculate(amount: number): number {
    return amount * 0.2;
  }
}

class RegularDiscount implements Discount {
  calculate(amount: number): number {
    return amount * 0.1;
  }
}

class DiscountService {
  calculateDiscount(discount: Discount, amount: number): number {
    return discount.calculate(amount);
  }
}
```
Here, DiscountService is open for extension (new types of discounts can be added), but it doesn't require modification of the existing code.

### **1.3 Liskov Substitution Principle (LSP)**
The Liskov Substitution Principle states that objects of a base class should be replaceable with objects of a derived class without affecting the correctness of the program.

Example:
```typescript
class Bird {
  fly(): void {
    console.log("Flying");
  }
}

class Duck extends Bird {
  // Duck inherits from Bird and can fly
}

class Ostrich extends Bird {
  // Ostrich cannot fly, so it violates LSP
  fly(): void {
    throw new Error("Cannot fly");
  }
}
```
Here, Ostrich violates LSP because it overrides the fly() method in a way that doesn't follow the base class's behavior. We can fix this by designing a better hierarchy or creating a Flyable interface.

### **1.4 Interface Segregation Principle (ISP)**
The Interface Segregation Principle states that clients should not be forced to depend on interfaces they don't use. Instead of one large interface, it is better to split it into smaller, more specific ones.

Example:
```typescript
// Before ISP
interface Worker {
  work(): void;
  eat(): void;
}

class WorkerImpl implements Worker {
  work() {
    console.log("Working");
  }
  eat() {
    console.log("Eating");
  }
}

// After ISP
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class WorkerImpl implements Workable, Eatable {
  work() {
    console.log("Working");
  }
  eat() {
    console.log("Eating");
  }
}
```
Now the WorkerImpl class only implements what is needed.

### **1.5 Dependency Inversion Principle (DIP)**
The Dependency Inversion Principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions. It also states that abstractions should not depend on details. Details should depend on abstractions.

Example:
```typescript
// Before DIP
class Database {
  saveData(data: string): void {
    // saving logic
  }
}

class App {
  private database: Database;

  constructor() {
    this.database = new Database();
  }

  saveUser(user: string) {
    this.database.saveData(user);
  }
}

// After DIP
interface Database {
  saveData(data: string): void;
}

class MySQLDatabase implements Database {
  saveData(data: string): void {
    // MySQL saving logic
  }
}

class MongoDBDatabase implements Database {
  saveData(data: string): void {
    // MongoDB saving logic
  }
}

class App {
  private database: Database;

  constructor(database: Database) {
    this.database = database;
  }

  saveUser(user: string) {
    this.database.saveData(user);
  }
}
```
Here, App no longer depends on a specific database implementation but on an abstraction (interface), making the code more flexible.

## **2. Clean Code Guidelines**

### **2.1 Writing Clean Code**
Clean code refers to code that is easy to read, understand, and maintain. It is self-explanatory, well-structured, and consistent.

Key Principles:
Descriptive Naming: Variables, functions, and classes should have descriptive names that convey their purpose.
Avoid Duplication: If you find the same code repeated, refactor it into reusable functions or components.
Keep Functions Small: Each function should do one thing, and it should be small.
Use TypeScript's Type System: Make use of TypeScript's strong typing to avoid errors and improve code readability.
Example of Clean Code:
```typescript
// Before
function a(x, y) {
  if (x === 10) {
    y = y * 10;
  }
  return y;
}

// After
function multiplyByTenIfXIsTen(x: number, y: number): number {
  if (x === 10) {
    return y * 10;
  }
  return y;
}
```
The second `function` has a meaningful name and clear intention.

### **2.2 Naming Conventions**
Proper naming is key to writing clean code. In `TypeScript` and `React Native`:

`Classes/Components`: Use `PascalCase` (e.g., `UserProfile`).
`Functions/Methods`: Use `camelCase` (e.g., `getUserDetails()`).
`Constants/Enums`: Use `UPPER_SNAKE_CASE` (e.g., `MAX_USERS`).
`Types` and `Interfaces`: Use `PascalCase` with an `optional` I prefix for `interfaces` (e.g., `IUser`).
### **2.3 Code Structure and Organization**
Keep Components Small and Focused: Each component should represent a single responsibility. Use functional components whenever possible.
Separate Logic from UI: For example, place business logic in services or hooks, and use components for rendering UI only.
Group Related Files: Organize components, hooks, services, and styles in well-defined folders.
Example:

```bash
/src
  /components
    /Button
      Button.tsx
      Button.styles.ts
  /services
    /userService.ts
  /hooks
    /useAuth.ts
```

### **2.4 Avoiding Anti-Patterns**
Some common anti-patterns in React Native and TypeScript:

Props Drilling: Passing props through multiple layers of components.

Solution: Use Context API or Redux to manage shared state across multiple components.
Large Components: Having components that are too large and handle too many responsibilities.

Solution: Break components into smaller, focused components.
Mutating State Directly: Directly modifying the state in React.

Solution: Always use setState or hooks to update state.
Not Handling Errors Properly: Ignoring errors or not providing proper error messages.

Solution: Use try/catch blocks and error boundaries to catch and handle errors gracefully.
### **2.5 Refactoring Techniques**
Extract Functions: If a function is too large, break it down into smaller ones.
Replace Magic Numbers: Replace numbers with named constants to make code more readable.
Eliminate Dead Code: Remove unused variables, functions, or imports.
Conclusion
By following SOLID principles and Clean Code guidelines, you can write code that is scalable, modular, and maintainable. In React Native and TypeScript, applying these principles will help avoid anti-patterns, improve readability, and make it easier to extend and modify the application in the future.

In practice, applying these principles means:

Breaking down large, monolithic classes into smaller, manageable ones.
Writing functions that have a clear purpose.
Keeping components focused on rendering and delegating logic to services or hooks.
Avoiding direct state mutation and ensuring that the app structure is clean and organized.
Following these guidelines will not only improve your code quality but also enhance collaboration, testing, and debugging in team environments.

# Interview Questions on SOLID Principles & Clean Code for TypeScript & React Native

## 1. What is the Single Responsibility Principle (SRP)?

**Answer:**
The **Single Responsibility Principle (SRP)** states that a class or function should have only one responsibility or reason to change. This principle helps in keeping the code clean, understandable, and easy to maintain by ensuring that each class or function has a focused task.

---

## 2. What is the Open/Closed Principle (OCP)?

**Answer:**
The **Open/Closed Principle (OCP)** states that a software entity (like a class or function) should be open for extension but closed for modification. This means you can add new functionality to the system without changing existing code, thus maintaining backward compatibility.

---

## 3. Can you explain the Liskov Substitution Principle (LSP)?

**Answer:**
The **Liskov Substitution Principle (LSP)** suggests that objects of a superclass should be replaceable with objects of its subclass without affecting the correctness of the program. In simpler terms, subclasses should be able to perform the same tasks as the superclass without introducing errors.

---

## 4. What is the Interface Segregation Principle (ISP)?

**Answer:**
The **Interface Segregation Principle (ISP)** states that clients should not be forced to implement interfaces they do not use. It encourages breaking large interfaces into smaller, more specific ones that only contain methods relevant to the implementing class.

---

## 5. What is the Dependency Inversion Principle (DIP)?

**Answer:**
The **Dependency Inversion Principle (DIP)** states that high-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces). This principle promotes flexibility by allowing you to change implementations without affecting the overall system.

---

## 6. What is clean code, and why is it important in software development?

**Answer:**
**Clean code** refers to code that is easy to read, understand, and maintain. It follows best practices like meaningful naming, proper structure, and minimal duplication. In software development, clean code helps reduce bugs, improve collaboration, and ensure the system can be extended without difficulty.

---

## 7. How do you write clean and maintainable code in React Native?

**Answer:**
To write clean and maintainable code in React Native:
- Use **functional components** instead of class components.
- Keep functions **small and focused** on one task.
- Follow consistent **naming conventions**.
- Organize your code into smaller, reusable **components**.
- Use TypeScript to enforce type safety and avoid errors.

---

## 8. Why is proper naming important in clean code?

**Answer:**
Proper naming is crucial in clean code because it makes the code **self-explanatory**. Meaningful names for variables, functions, and classes help developers understand the purpose of the code at a glance, reducing the need for excessive comments and enhancing readability.

---

## 9. What are common anti-patterns in React Native development, and how do you avoid them?

**Answer:**
Common **anti-patterns** in React Native include:
1. **Props drilling**: Passing data through many components. Use **Context API** or **Redux** to avoid this.
2. **Large components**: Split them into smaller, focused components to improve maintainability.
3. **Directly mutating state**: Always use state management functions like `setState` or `useState` hooks.
4. **Not handling errors properly**: Implement proper error handling with `try/catch` and use error boundaries in React.

---

## 10. How do you refactor code to follow SOLID principles in a React Native app?

**Answer:**
To refactor code to follow SOLID principles:
- **SRP**: Break classes and functions into smaller, more focused pieces.
- **OCP**: Extend functionalities without changing existing code by using inheritance or composition.
- **LSP**: Ensure subclasses do not break expected behavior of the parent class.
- **ISP**: Split large interfaces into smaller, specific ones.
- **DIP**: Use dependency injection to decouple modules and rely on abstractions (interfaces).

---



