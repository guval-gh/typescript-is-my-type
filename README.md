# TypeScript - Tips & Tricks

Tips and tricks for TypeScript

- [Introduction](#introduction)
  - [What is TypeScript?](#what-is-typescript)
  - [What this repository is NOT](#what-this-repository-is-not)
  - [What this repository is](#what-this-repository-is)
- [TypeScript Tips & Tricks](#typescript-tips-and-tricks)
  - [Create Type from Enum](#create-type-from-enum)
  - [Create Type from Function return](#create-type-from-function-return)
  - [Stronger Types with Branded Types and Type Aliases](#stronger-types-with-branded-types-and-type-aliases)
  - [Extract keys by value type](#extract-keys-by-value-type)
  - [Conditional Types](#conditional-types)
  - [XOR Operator](#xor-operator)
  - [Check data with asserts condition](#check-data-with-asserts-condition)

# Introduction

## What is TypeScript?

TypeScript is a strongly typed programming language developed and maintained by Microsoft. It is a strict syntactical superset of JavaScript that adds optional static typing to the language. This means that any valid JavaScript code is also valid TypeScript code, but TypeScript adds additional features on top of JavaScript.

Key features of TypeScript include:

- **Static Typing**: TypeScript introduces type annotations that allow developers to add type information to their code. This enables better tooling support, earlier error detection, and improved code maintainability.
- **Object-Oriented Features**: TypeScript supports object-oriented programming concepts like classes, interfaces, inheritance, and modules, making it easier to build and maintain large-scale applications.
- **IDE Support**: Thanks to its type system, TypeScript provides excellent IDE support with features like intelligent code completion, refactoring, and inline documentation.
- **ECMAScript Compatibility**: TypeScript is designed to align with ECMAScript standards and can compile down to various versions of JavaScript, ensuring broad compatibility across different environments.
- **Type Inference**: Even without explicit type annotations, TypeScript can often infer types based on how variables and functions are used, providing type safety with minimal extra code.
  TypeScript code is transpiled into JavaScript, which can run in any environment that supports JavaScript, including browsers, Node.js, and other runtime environments.

## What this repository is NOT

This repository is not a:

- course or tutorial to learn TypeScript from scratch
- comprehensive guide to TypeScript
- cheat sheet or reference for the entire TypeScript language

If you are looking to learn TypeScript, you can find many resources on the [official website](https://www.typescriptlang.org/):

- [TypeScript Documentations](https://www.typescriptlang.org/docs/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

You can also find resources created by the community:

- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [TypeScript CheatSheet](https://react-typescript-cheatsheet.netlify.app/docs/basic/setup)

## What this repository is

This repository is simply a collection of tips and tricks for TypeScript found during my developments, while researching on the internet or social networks.

# TypeScript Tips & Tricks

## Create Type from Enum

```ts
enum Categories {
  Nature = "nature",
  Science = "science",
  Economy = "economy",
}

type Category = keyof typeof Categories;
// type Category = "Nature" | "Science" | "Economy"
```

## Create Type from Function return

Instead of create primitive type manually like:

```ts
type Status = "paid" | "free";

let productStatus: Status;

productStatus = "paid"; // ✅
productStatus = "free"; // ✅
productStatus = "something"; // ❌ Type '"something"' is not assignable to type 'Status'.
```

You can dynamically create a type from a function's return type:

```ts
const getPaidStatus = () => {
  return "paid" as const;
};

const getFreeStatus = () => {
  return "free" as const;
};

type Status =
  | ReturnType<typeof getPaidStatus>
  | ReturnType<typeof getFreeStatus>;
// equal to: type Status = "paid" | "free"

let productStatus: Status;

productStatus = "paid"; // ✅
productStatus = "free"; // ✅
productStatus = "something"; // ❌ Type '"something"' is not assignable to type 'Status'.
```

## Stronger Types with Branded Types and Type Aliases

Branded types helps you create types that are nominally unique, even if they have the same underlying structure.
This is useful when you want to prevent values of the same shape from being used interchangeably.

Example:

```ts
// Define two types for ProductId and OrderId that are branded with a unique symbol but different brand
type ProductId = string & { readonly __brand: unique symbol };
type OrderId = string & { readonly __brand: unique symbol };

// Create functions to safely create these types
const createProductId = (id: string): ProductId => id as ProductId;
const createOrderId = (id: string): OrderId => id as OrderId;

const processProduct = (id: ProductId) =>
  console.log(`Processing product: ${id}`);
const processOrder = (id: OrderId) => console.log(`Processing order: ${id}`);

// Example usage
const ProductId = createProductId("product123");
const orderId = createOrderId("order123");

processProduct(ProductId); // ✅ OK
processOrder(orderId); // ✅ OK

// These will cause type errors
processProduct(orderId); // ❌ Type 'OrderId' is not assignable to parameter of type 'ProductId'
processOrder(ProductId); // ❌ Type 'ProductId' is not assignable to parameter of type 'OrderId'
processProduct("product123"); // ❌ Type 'string' is not assignable to parameter of type 'ProductId'
```

## Extract keys by value type

```ts
type ExtractKeysByValue<T, V> = {
  [K in keyof T]: T[K] extends V ? K : never;
}[keyof T];

type Product = {
  id: number;
  description: string;
  price: number;
};

type ProductKeys = ExtractKeysByValue<Product, string>;
// type ProductKeys = "description"
```

## Conditional Types

Conditional types help you create types that depend on other types. They follow an `if/else` structure using the syntax: `T extends U ? X : Y`

Example:

```ts
// Example with multiple conditions
type MediaType<T> = T extends { type: "image" }
  ? { url: string; width: number; height: number }
  : T extends { type: "video" }
  ? { url: string; duration: number }
  : never;

// Usage:
const image: MediaType<{ type: "image" }> = {
  url: "photo.jpg",
  width: 1920,
  height: 1080,
}; // ✅

const video: MediaType<{ type: "video" }> = {
  url: "video.mp4",
  duration: 120,
}; // ✅

const audio: MediaType<{ type: "audio" }> = {
  url: "audio.mp3",
}; // ❌ Type 'never' has no properties
```

## XOR Operator

The XOR (exclusive OR) operator can be used to create a type that have exactly one of two possible sets of properties, but not both.

```ts
// Without marks all properties from one type as optional and never when they exist in the other type
type Without<T, U> = { [P in Exclude<keyof T, keyof U>]?: never };

type XOR<T, U> = T | U extends object
  ? (Without<T, U> & U) | (Without<U, T> & T)
  : T | U;

type User = { name: string } & XOR<
  { isAdmin: true; adminKey: string },
  { isAdmin: false }
>;

const jack: User = {
  name: "Jack",
  isAdmin: false,
}; // ✅

const admin: User = {
  name: "Admin",
  isAdmin: true,
  adminKey: "XXxxXXxxXXxx",
}; // ✅

const badUser: User = {
  name: "John",
}; // ❌ Type '{ name: string; }' is missing the following properties from type '{ isAdmin: true; adminKey: string; }': isAdmin, adminKey

const badAdmin: User = {
  name: "Admin",
  isAdmin: true,
}; // ❌ Property 'adminKey' is missing in type '{ name: string; isAdmin: true; }' but required in type '{ isAdmin: true; adminKey: string; }
```

## Check data with asserts condition

The `asserts` condition provides a safer way to narrow types by throwing an error if the condition is not met. This is particularly useful for runtime type checking and validation.

Example:

```ts
type User = { name: string; email: string };

// Type assertion function that validates an unknown input is a User
// This function will throw an error if the input is not a valid User

// Keep in mind that: arrow functions can't be used for assertion
function assertUser(input: unknown): asserts input is User {
  if (!input || typeof input !== "object") {
    throw new Error("Input data are missing or not an object!");
  }

  if (!("name" in input) || typeof input.name !== "string") {
    throw new Error("Username is missing!");
  }

  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  if (
    !("email" in input) ||
    typeof input.email !== "string" ||
    !emailPattern.test(input.email)
  ) {
    throw new Error("Use email should be valid!");
  }
}

const createUser = (input: unknown) => {
  // At this point input type is unknown
  assertUser(input);

  // After assertion passes, TypeScript knows input is a valid User
  console.log(`Name: ${input.name}, Email: ${input.email}`); // Name: Jack, Email: jack@example.com
};

// Example usage:
createUser({ name: "Jack", email: "jack@example.com" }); // ✅ OK - Valid user object
createUser({ name: "Jack", email: "unvalid.com" }); // ❌ Use email should be valid!
createUser({ email: "jack@example.com" }); // ❌ Username is missing!
```
