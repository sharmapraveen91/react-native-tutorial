# TypeScript Guide for College Freshers

## Introduction
TypeScript is a statically typed superset of JavaScript that adds type annotations and other features to make JavaScript development more robust and scalable. It’s a compiled language, meaning TypeScript code is transpiled to JavaScript before it runs. TypeScript can be used for both frontend and backend development and is widely adopted for building large-scale applications.

In this guide, we’ll explore the following topics:
- TypeScript keywords
- Basic syntax and how to use TypeScript in React Native
- Commonly used features like classes, interfaces, generics, and functions
- Unit testing with TypeScript
- 20 frequently asked interview questions for TypeScript

---

## Keywords in TypeScript
TypeScript includes several important keywords, many of which come from JavaScript with the addition of types:

- `let`, `const`, `var`: Variable declaration
- `function`: Declaring functions
- `class`: Declaring classes
- `interface`: Declaring interfaces
- `type`: Defining types
- `any`: Dynamic typing
- `void`: Function that doesn't return a value
- `never`: A function that never returns (e.g., throws an error)
- `implements`: Implementing interfaces
- `extends`: Extending classes or interfaces
- `as`: Type assertion

---

## Using TypeScript in React Native

In a React Native app, TypeScript provides type safety, better autocompletion, and clearer code. Here’s how you can get started with basic tasks in TypeScript.

### Example 1: Loop
```typescript
let numbers: number[] = [1, 2, 3, 4, 5];

for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

### Example 2: Condition (if/else)
```typescript
let age: number = 18;

if (age >= 18) {
  console.log("You are an adult.");
} else {
  console.log("You are a minor.");
}
```

### Example 3: Switch Case
```typescript
let color: string = "blue";

switch (color) {
  case "red":
    console.log("Color is red");
    break;
  case "blue":
    console.log("Color is blue");
    break;
  case "green":
    console.log("Color is green");
    break;
  default:
    console.log("Unknown color");
    break;
}
```

---

## Defining Classes, Interfaces, and Generics in TypeScript

### Example 1: Class
```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const person = new Person("John", 25);
person.greet();
```

### Example 2: Interface
```typescript
interface Vehicle {
  make: string;
  model: string;
  year: number;
}

const car: Vehicle = {
  make: "Toyota",
  model: "Camry",
  year: 2020,
};
```

### Example 3: Generics
```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("Hello TypeScript");
console.log(output); // Output: Hello TypeScript
```

### Example 4: Functions
```typescript
function add(a: number, b: number): number {
  return a + b;
}

console.log(add(5, 10)); // Output: 15
```

---

## Unit Testing in TypeScript

You can use libraries like Jest or Mocha for unit testing in TypeScript. Below is an example of a simple test case using Jest.

```typescript
// Function to be tested
function multiply(a: number, b: number): number {
  return a * b;
}

// Test case
test('multiply function should return the product of two numbers', () => {
  expect(multiply(2, 3)).toBe(6);
});
```

To run the tests, install Jest and TypeScript-related packages:

```bash
npm install --save-dev jest ts-jest @types/jest
```

Add the following to your `package.json` under scripts:

```json
"scripts": {
  "test": "jest"
}
```

Run the tests:
```bash
npm test
```

---

## Widely Asked TypeScript Interview Questions and Answers

### What is TypeScript and why is it used?
**Answer:** TypeScript is a superset of JavaScript that adds static typing and modern features like classes, interfaces, and generics. It is used to improve code quality and maintainability in large-scale applications by providing type safety.

### What are the advantages of using TypeScript over JavaScript?
**Answer:** TypeScript provides static typing, better tooling support, autocompletion, and error checking at compile-time, making it easier to catch bugs early and improving developer productivity.

### What are the basic types in TypeScript?
**Answer:** Basic types in TypeScript include `number`, `string`, `boolean`, `null`, `undefined`, `any`, `void`, `never`, `tuple`, and `enum`.

### What is the difference between `any` and `unknown` types?
**Answer:** The `any` type allows any value without any type checking, whereas `unknown` requires you to perform type checking before using the value, making it safer.

### What are TypeScript decorators?
**Answer:** Decorators are special functions that can modify classes, methods, or properties in TypeScript. They are often used in frameworks like Angular to add metadata.

### How does TypeScript support asynchronous programming?
**Answer:** TypeScript supports asynchronous programming using `async` and `await` keywords, just like in JavaScript, with the added benefit of type safety.

### What is the `never` type in TypeScript?
**Answer:** The `never` type represents values that never occur. It's used for functions that throw errors or have infinite loops, indicating they never return a value.

### What are generics in TypeScript? Provide an example.
**Answer:** Generics allow you to write reusable functions or classes that work with any data type.

```typescript
function identity<T>(arg: T): T {
  return arg;
}
```

### Explain the difference between `interface` and `type` in TypeScript.
**Answer:** Both `interface` and `type` define shapes of objects, but `interface` can be extended or merged, while `type` is more flexible and can define primitive, union, and intersection types.

---

This guide provides a strong foundation for understanding TypeScript and preparing for technical interviews.
