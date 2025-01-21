# TypeScript

🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧  
🚧🚧🚧 WORK IN PROGRESS 🚧🚧🚧🚧  
🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧

- [Introduction](#introduction)
  - [What is TypeScript?](#what-is-typescript)
- [Basic Types](#basic-types)
  - [Primitive Types](#primitive-types)
  - [Literal Types](#literal-types)
  - [Object Types](#object-types)
  - [Subtyping](#subtyping)
  - [Unknown Type](#unknown-type)
  - [Never Type](#never-type)
  - [Any Type](#any-type)
- [Union and Intersection](#union-and-intersection)
  - [Accessibility Rules](#accessibility-rules)
  - [Accessing Properties](#accessing-properties)
  - [Key Extraction](#key-extraction)
  - [Handling undefined](#handling-undefined)
  - [Combining](#combining)
- [TypeScript Tips and Tricks](#typescript-tips-and-tricks)
  - [Create Type from Enum](#create-type-from-enum)
  - [Create Type from Function return](#create-type-from-function-return)
  - [Stronger Types with Branded Types and Type Aliases](#stronger-types-with-branded-types-and-type-aliases)
  - [Extract keys by value type](#extract-keys-by-value-type)
  - [Conditional Types](#conditional-types)
  - [XOR Operator](#xor-operator)
  - [Check data with asserts condition](#check-data-with-asserts-condition)
- [Generic Tips and Tricks](#generic-tips-and-tricks)
  - [Avoid nested Try/Catch](#avoid-nested-trycatch)
  - [Logical Assignment Operators](#logical-assignment-operators)

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

# Basic Types

## Primitive Types

These primitive types are the foundation for building more complex types in TypeScript. When declaring variables, TypeScript can infer these types automatically, or they can be explicitly annotated:

- **string**: Represents textual data (`"hello"`, `'world'`)
- **number**: Represents both integer and floating point numbers (`42`, `3.14`)
- **boolean**: Represents `true` or `false` values
- **symbol**: Represents unique identifiers created via `Symbol()`
- **bigint**: Represents arbitrarily large integers (e.g. `9007199254740991n`)
- **null**: Represents the intentional absence of any object value
- **undefined**: Represents uninitialized variables or missing properties

```ts
type Primitive = string | number | boolean | symbol | bigint | null | undefined;
```

## Literal Types

Literal types are more specific versions of primitive types. While primitive types represent a broad category of values (like any string or any number), literal types represent exact, specific values. A literal type can only have one specific value:

- A primitive `string` type can hold any string value
- A literal type `"hello"` can only hold the exact string "hello"
- `null` and `undefined` are literal types that can only hold their respective value

```ts
type Literal = "string" | 1 | true | null | undefined;
```

## Object Types

Some examples (more details on each type in the following sections):

```ts
type DataStructures =
  | { name: string; age: number } // Object literal type
  | { [key: string]: string } // Record type (Index signature)
  | Record<string, number> // Record type (key-value pairs)
  | [number, boolean] // Tuple type
  | string[] // Array type
  | Array<number>; // Generic array type
```

## Subtyping

Subtyping refers to the relationship between types where one type is considered a "subtype" of another type if it can be safely used in place of the other type.

```ts
let capybara: "Capybara" = "Capybara";
let cat: "Cat" = "Cat";

let animal: string;

animal = capybara; // ✅
animal = cat; // ✅

cat = animal; // ❌

// capybara is a subtype of "Capibara"
// cat is a subtype of "Cat"
```

## Unknown Type

- The `unknown` type is a supertype of all types. It is a more flexible version of `any`, but unlike `any`, it requires explicit type checking before it can be used.
- `unknown` is a supertype of every other type, but no other type is a supertype of `unknown`.

Logic:

```ts
X | unknown = unknown
X & unknown = X
```

## Never Type

- The `never` type represents values that are never actually produced. It is a subtype of all types, meaning any type can be assigned to `never`, but `never` cannot be assigned to any type.
- `never` is a subtype of every other type, but no other type is a subtype of `never`.

Logic:

```ts
X | never = X
X & never = never
```

Example:

```ts
// Function that throws error returns never
function throwError(message: string): never {
  throw new Error(message);
}

// Function with infinite loop returns never
function infiniteLoop(): never {
  while (true) {
    // do something forever
  }
}

// never in exhaustive checks
type Animal = "dog" | "cat";

function processAnimal(animal: Animal) {
  switch (animal) {
    case "dog":
      console.log("woof");
      break;
    case "cat":
      console.log("meow");
      break;
    default:
      // This line will error if we forget to handle a case
      const exhaustiveCheck: never = animal;
  }
}
```

```ts
function fail(): never {
  throw new Error("Something failed");
}

const userName: string = fail(); // ✅
const userAge: number = fail(); // ✅
const userIsAdmin: boolean = fail(); // ✅
const anything: any = fail(); // ✅
```

## Any Type

- The `any` type is the most flexible type in TypeScript. It allows you to assign any value to a variable, even if it's not explicitly typed.
- `any` type is both a subtype and a supertype of every other type.

Logic:

```ts
X | any = any
X & any = any
```

Example:

```ts
let anything: any = "Hello, world!";
anything = 42;
anything = true;
anything = null;
anything = undefined;
```

# Union and Intersection

Union types allow a value to be one of several types (using `|`), while intersection types combine multiple types into one (using `&`).

```ts
// type Union = A | B
type Union = "string" | 1 | true | null | undefined;

// type Intersection = A & B
type Intersection = { name: string } & { age: number };
```

## Accessibility Rules

```ts
type User = {
  name: string;
  age: number;
  isAdmin: boolean;
};

const jack: User = {
  name: "Jack",
  age: 25,
  isAdmin: false,
};

const john = {
  name: "John",
  age: 30,
  isAdmin: true,
  extraProperty: "extra",
};

// Allows extra properties
const admin: User = john;
admin.extraProperty; // ❌ Property 'extraProperty' does not exist on type 'User'.

// But exist in the object
console.log(admin.extraProperty);
//  Return
// {
//   "name": "John",
//   "age": 30,
//   "isAdmin": true,
//   "extraProperty": "extra"
// }
```

## Accessing Properties

```ts
// type NameOrAge = User["name" | "age"]
type NameOrAge = User["name"] | User["age"];

type Age = User["age"];
type Role = User["isAdmin"];
```

## Key Extraction

```ts
type Keys = keyof User;
// type Keys = "name" | "age" | "isAdmin"

type Values = User[keyof User];
// type Values = string | number | boolean

// Is Equal to:
type ValueOf<T> = T[keyof T];
type UserValues = ValueOf<User>;
// type UserValues = string | number | boolean
```

## Handling undefined

```ts
type User = {
  name: string;
  age?: number;
};

// ✅ No need to add age property
const user: User = {
  name: "John",
};

type User = {
  name: string;
  age: number | undefined;
};

// ❌ Age property can be undefined but should be here
const user: User = {
  name: "John",
};

// ✅
const user: User = {
  name: "John",
  age: undefined,
};
```

## Combining

```ts
// keyof (A & B) = (keyof A) | (keyof B)

type A = { a: string };
type KeyOfA = keyof A; // => 'a'

type B = { b: number };
type KeyOfB = keyof B; // => 'b'

type C = A & B;
type KeyOfC = keyof C; // => 'a' | 'b'

// keyof (A | B) = (keyof A) & (keyof B)

type A = { a: string; c: boolean };
type KeyOfA = keyof A; // => 'a' | 'c'

type B = { b: number; c: boolean };
type KeyOfB = keyof B; // => 'b' | 'c'

type C = A | B;
type KeyOfC = keyof C; // => 'c'
```

# TypeScript Tips and Tricks

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

# Generic Tips and Tricks

## Avoid nested Try/Catch

Old way:

```javascript
const getJsonData = async () => {
  try {
    const response = await fetch("https://api.example.com/data");
    const jsonData = await response.json();

    return jsonData;
  } catch (error) {
    console.error(error);
  }
};
```

Possible way:

You can use the `?=` operator to destructure the Promise into [error, data] then handle fetchError and jsonError separately.

```javascript
const getJsonData = async () => {
  const [fetchError, fetchData] ?= await fetch("https://api.example.com/data")

  if (fetchError) {
    console.error('Fetch Error:', fetchError)
    return
  }

  const [jsonError, jsonData] ?= await fetchData.json()

  if (jsonError) {
    console.error('Json Error:', jsonError)
    return
  }

  return jsonData
}
```

## Logical Assignment Operators

```javascript
const obj = { a: 10, b: 0, c: 1 };

obj.z ??= 40; // if z is null or undefined, set z to 40
console.log(obj); // { a: 10, b: 0, c: 1, z: 40 }

obj.b ||= 20; // if b is false, set b to 20
console.log(obj); // { a: 10, b: 20, c: 1, z: 40 }

obj.c &&= 30; // if c is true or have value, set c to 30
console.log(obj); // { a: 10, b: 20, c: 30, z: 40 }
```
