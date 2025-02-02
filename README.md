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
- [Other Types](#other-types)
  - [Tuple Type](#tuple-type)
  - [Array Type](#array-type)
  - [Array and Tuple Hybrid](#array-and-tuple-hybrid)
- [Union and Intersection](#union-and-intersection)
  - [Conditional checking](#conditional-checking)
  - [Accessibility Rules](#accessibility-rules)
  - [Distribution and Accessing Properties](#distribution-and-accessing-properties)
  - [Key Extraction](#key-extraction)
  - [Handling undefined](#handling-undefined)
  - [Combining](#combining)
- [Mapped Types](#mapped-types)
- [Helper/Utility Types](#helperutility-types)
  - [Record](#record)
  - [Readonly](#readonly)
  - [Partial](#partial)
  - [Exclude](#exclude)
  - [Extract](#extract)
  - [Required](#required)
  - [Pick](#pick)
  - [Omit](#omit)
  - [Awaited](#awaited)
  - [NonNullable](#nonnullable)
  - [Parameters](#parameters)
  - [ConstructorParameters](#constructorparameters)
  - [ReturnType](#returntype)
  - [InstanceType](#instancetype)
  - [NoInfer](#noinfer)
  - [String Manipulator](#string-manipulator)
- [Conditional Types](#conditional-types)
  - [Nested conditions](#nested-conditions)
  - [Type Constraints](#type-constraints)
  - [Infer Types](#infer-types)
  - [Extract Type with Infer](#extract-type-with-infer)
  - [Check with Tuple](#check-with-tuple)
- [Iterative and Loop Methods](#iterative-and-loop-methods)
  - [Find loop](#find-loop)
  - [Map loop](#map-loop)
  - [Filter loop](#filter-loop)
  - [Reduce loop](#reduce-loop)
- [Template Literals](#template-literals)
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
type Primitive = string | number | boolean | symbol | bigint | null | undefined
```

## Literal Types

Literal types are more specific versions of primitive types. While primitive types represent a broad category of values (like any string or any number), literal types represent exact, specific values. A literal type can only have one specific value:

- A primitive `string` type can hold any string value
- A literal type `"hello"` can only hold the exact string "hello"
- `null` and `undefined` are literal types that can only hold their respective value

```ts
type Literal = 'string' | 1 | true | null | undefined
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
  | Array<number> // Generic array type
```

## Subtyping

Subtyping refers to the relationship between types where one type is considered a "subtype" of another type if it can be safely used in place of the other type.

```ts
let capybara: 'Capybara' = 'Capybara'
let cat: 'Cat' = 'Cat'

let animal: string

animal = capybara // ✅
animal = cat // ✅

cat = animal // ❌

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
  throw new Error(message)
}

// Function with infinite loop returns never
function infiniteLoop(): never {
  while (true) {
    // do something forever
  }
}

// never in exhaustive checks
type Animal = 'dog' | 'cat'

function processAnimal(animal: Animal) {
  switch (animal) {
    case 'dog':
      console.log('woof')
      break
    case 'cat':
      console.log('meow')
      break
    default:
      // This line will error if we forget to handle a case
      const exhaustiveCheck: never = animal
  }
}
```

```ts
function fail(): never {
  throw new Error('Something failed')
}

const userName: string = fail() // ✅
const userAge: number = fail() // ✅
const userIsAdmin: boolean = fail() // ✅
const anything: any = fail() // ✅
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
let anything: any = 'Hello, world!'
anything = 42
anything = true
anything = null
anything = undefined
```

# Other Types

## Tuple Type

A tuple type is a fixed-length array where each element can have a specific type, allowing you to define an array with a known number of elements of different types.

```ts
// Basic example
type Tuple = [number, string, boolean];
const tuple: Tuple = [1, "hello", true];

// Create literal type from Tuple
type FullName = ["John", "Doe"];
type FirstName = FullName[0]; // type FirstName = "John"
type LastName = FullName[1]; // type LastName = "Doe"

// Create type from Tuple (types or values)
type User = { name: string; isAdmin: boolean };
type NameOrAdmin = User["name" | "isAdmin"]; // type NameOrAdmin = string | boolean

type UserTuple = ["John", "Doe", false]
type NameOrAdminBis = UserTuple[0 | 2]; // type NameOrAdminBis = "John" | false

// When using keyof on a tuple/array type, it returns:
// - The numeric indices as string literals ("0", "1", etc.)
// - All the built-in array methods and properties ("length", "map", "filter", etc.)
type Keys = keyof ["John", false] // type Keys = keyof ["John", false] = "0" | "1" | "length" | "pop" | "push" | "concat" | "join" | "reverse" | "shift" | "slice" | "sort" | etc.

// Combining/Merging Tuples
type Tuple1 = [1, 2, 3]
type Tuple2 = [4, 5, 6]
type Tuple3 = [...Tuple1, ...Tuple2] // type Tuple3 = [1, 2, 3, 4, 5, 6]
type Tuple4 = Tuple1 & Tuple2 // type Tuple4 = [1, 2, 3, 4, 5, 6]

// Create Tuple with named index (optionnal)
type User1 = [name: string; email: string]
// Equal to:
type User2 = [string, string]

// Create Tuple with the last parameter/index optionnal
type User2 = [string, string?]

// All const created are valid ✅
const user: User2 = ["John", "john@example.com"]
const user: User2 = ["John"]
const user: User2 = ["John", undefined]
```

Concret example with check French social security number format:

```ts
// French social security number format: 1 + 89 + 02 + 75 + 108 + 108 + 185
// 1: gender (1 or 2)
// 89: year of birth (00-99)
// 02: month of birth (01-12)
// 75: department (01-99)
// 108: city code (001-999)
// 108: birth certificate number (001-999)
// 185: control key (01-97)

type Gender = 1 | 2
type Year = `${number}${number}`
type Month = `0${1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}` | `1${0 | 1 | 2}`
type Department =
  | `0${1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}`
  | `${1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}${0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}`
type CityCode = `${0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}${0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}${
  | 0
  | 1
  | 2
  | 3
  | 4
  | 5
  | 6
  | 7
  | 8
  | 9}`
type ControlKey = `${0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}${0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9}`

// Using tuple to enforce order and types
type FrenchSocialSecurityNumber = [
  gender: Gender,
  year: Year,
  month: Month,
  department: Department,
  cityCode: CityCode,
  birthNumber: CityCode,
  controlKey: ControlKey
]

// Example usage:
const validNumber: FrenchSocialSecurityNumber = [1, '89', '02', '75', '108', '108', '85']
```

## Array Type

Arrays in TypeScript are flexible collections that can hold elements of a specific type, denoted using either `Type[]` or `Array<Type>` syntax.

```ts
// Basic examples
type Basic1 = string[]
type Basic2 = Array<string>
type Basic3 = (0 | 1 | 2)[]

// Get type of element
type Example = boolean[]
type Example2 = Example[number] // type Example2 = boolean
```

## Array and Tuple Hybrid

```ts
// number[] that starts with 0
type PhoneNumber = [0, ...number[]]

// string[] that ends with a `?`
type Question = [...string[], '?']

// non-empty list of strings
type NonEmpty = [string, ...string[]]

// starts and ends with a zero
type Padded = [0, ...number[], 0]
```

Concret example with hybrid type (tuple + array):

```ts
type User = [name: string, email: string, phone?: number, ...addresses: string[]]

const createUser = (...args: User) => {
  const [name, email, phone, ...addresses] = args

  // Logic...
}

createUser('Jack', 'jack@example.com', 1234567890, '1 rue du Chat', '75001 Paris') // ✅
createUser('Alice', 'alice@example.com') // ✅ `phone` is optional and addresses can be empty.
createUser('Alice', 'alice@example.com', undefined, '1 rue du Chat', '75001 Paris') // ✅ `phone` should be 0 or undefined (for a number) to specify next parameters.
createUser('Branda', 'branda@example.com', false) // ❌ Argument of type 'boolean' is not assignable to parameter of type 'number'
```

Another example with shared type:

```ts
type UserName = [firstName: string, lastName: string] | [firstName: string, middleName: string, lastName: string]

const createUser = (...name: UserName) => {
  // Logic...
}

createUser('John', 'Doe') // ✅
createUser('John', 'Doe', 'Smith') // ✅
createUser('John') // ❌ Too less parameters. Need 2 or 3 parameters only
createUser('John', 'Doe', 'Smith', 'Baker') // ❌ Too many parameters. Need 2 or 3 parameters only
```

# Union and Intersection

Union types allow a value to be one of several types (using `|`), while intersection types combine multiple types into one (using `&`).

```ts
// type Union = A | B
type Union = 'string' | 1 | true | null | undefined

// type Intersection = A & B
type Intersection = { name: string } & { age: number }

// Keep in mind that a simple type like that is still a union
type Union1 = 'x'
// Because it's equal to:
const union1 = new Set(['x']) // Set<string>

// And
type Union3 = never
// is equal to:
const union2 = new Set([]) // Set<never>
```

## Conditional checking

```ts
type Response = { status: 'loading' } | { status: 'success'; data: unknown } | { status: 'error'; error: Error }

let response1: Response = {
  status: 'success',
  error: new Error('Just an error') // ❌ Error: Object literal may only specify known properties, and 'error' does not exist in type '{ status: "success"; data: unknown; }'
}

let response2: Response = {
  status: 'error',
  error: new Error('Just an error') // ✅ status is 'error' with error property is OK
}

let response3: Response = {
  status: 'success',
  data: { name: 'John' } // ✅ status is 'success' with data property is OK
}

// Type is narrowed with condition
const handler = (state: Response): string => {
  if (state.status === 'loading') {
    state // { status: "loading" }
    return '⏳'
  }

  if (state.status === 'success') {
    // state is *narrowed*:
    state // { status: "success", data: number }
    return '✅'
  }

  if (state.status === 'error') {
    state // { status: "error"; error: Error }
    return '❌'
  }

  // If status is not 'loading', 'success' or 'error', so it will be:
  return state // never
}
```

## Accessibility Rules

```ts
type User = {
  name: string
  age: number
  isAdmin: boolean
}

const jack: User = {
  name: 'Jack',
  age: 25,
  isAdmin: false
}

const john = {
  name: 'John',
  age: 30,
  isAdmin: true,
  extraProperty: 'extra'
}

// Allows extra properties
const admin: User = john
admin.extraProperty // ❌ Property 'extraProperty' does not exist on type 'User'.

// But exist in the object
console.log(admin.extraProperty)
//  Return
// {
//   "name": "John",
//   "age": 30,
//   "isAdmin": true,
//   "extraProperty": "extra"
// }
```

## Distribution and Accessing Properties

```ts
type NameOrAge = User['name' | 'age']
// Is equal to:
type NameOrAge = User['name'] | User['age']

type Age = User['age']
type Role = User['isAdmin']

// These two are not equivalent:
type Response1 = { status: 'success' | 'error' }
// Equal to:
type Response2 = { status: 'success' } | { status: 'error' }

// But these are equivalent:
type IsString<T> = T extends string ? 'yes' : 'no'

type CheckString1 = IsString<'a' | 2 | 'b'>
// Is equal to:
type CheckString2 =
  | ('a' extends string ? 'yes' : 'no')
  | (2 extends string ? 'yes' : 'no')
  | ('c' extends string ? 'yes' : 'no')
// Is equal to:
type CheckString3 = 'yes' | 'no' | 'yes'
// Is equal to:
type CheckString4 = 'yes' | 'no'

// All values are duplicated in one array
type Duplicate<T> = [T, T]
type Duplicated = Duplicate<1 | 2 | 3> // [2 | 1 | 3, 2 | 1 | 3]

// Each value is duplicated in one separated array
type DistributedDuplicate<U> = U extends unknown ? [U, U] : never
type DistributedDuplicated = DistributedDuplicate<1 | 2 | 3> // [2, 2] | [1, 1] | [3, 3]
```

## Key Extraction

```ts
type Keys = keyof User // type Keys = "name" | "age" | "isAdmin"

type Values = User[keyof User] // type Values = string | number | boolean
// Is Equal to:
type ValueOf<T> = T[keyof T]
type UserValues = ValueOf<User> // type UserValues = string | number | boolean
```

## Handling undefined

```ts
type User = {
  name: string
  age?: number
}

const user: User = {
  name: 'John'
} // ✅ No need to add age property

type User = {
  name: string
  age: number | undefined
}

const user: User = {
  name: 'John'
} // ❌ Age property can be undefined but should be here

const user: User = {
  name: 'John',
  age: undefined
} // ✅
```

# Mapped Types

Mapped types in TypeScript allow you to create new types based on existing ones by transforming each property according to a rule, similar to how array's map() transforms each element.

```ts
// Basic example
type User = {
  name: string
  age: number
  email: string
}

// Makes all properties optional
type PartialUser = {
  [Property in keyof User]?: User[Property]
}

// Makes all properties readonly
type ReadonlyUser = {
  readonly [Property in keyof User]: User[Property]
}

// Changes all properties to boolean
type UserFlags = {
  [Property in keyof User]: boolean
}

const userFlags: UserFlags = {
  name: true,
  age: false,
  email: true
}
```

```ts
// Complex examples
type User = {
  name: string
  age: number
  email: string
  address: {
    street: string
    city: string
  }
}

// Add 'is' prefix and make all properties boolean
type UserValidation = {
  [Property in keyof User as `is${Capitalize<string & Property>}Valid`]: boolean
}
// Results in:
// {
//   isNameValid: boolean
//   isAgeValid: boolean
//   isEmailValid: boolean
//   isAddressValid: boolean
// }

// Make all properties nullable and add metadata
type UserWithMetadata = {
  [Property in keyof User]: {
    value: User[Property] | null
    lastModified: Date
    modifiedBy: string
  }
}
// Results in:
// {
//   name: { value: string | null, lastModified: Date, modifiedBy: string }
//   age: { value: number | null, lastModified: Date, modifiedBy: string }
//   email: { value: string | null, lastModified: Date, modifiedBy: string }
//   address: { value: { street: string, city: string } | null, lastModified: Date, modifiedBy: string }
// }

// Exclude specific keys and make remaining properties arrays
type UserLists = {
  [Property in Exclude<keyof User, 'address'>]: Array<User[Property]>
}
// Results in:
// {
//   name: string[]
//   age: number[]
//   email: string[]
// }

// Recursive mapped type that makes all properties deep readonly
type DeepReadonly<T> = {
  readonly [Property in keyof T]: T[Property] extends object ? DeepReadonly<T[Property]> : T[Property]
}

const readonlyUser: DeepReadonly<User> = {
  name: 'John',
  age: 30,
  email: 'john@example.com',
  address: {
    street: 'Main St',
    city: 'Boston'
  }
}
// Cannot modify any property at any level
// readonlyUser.name = 'Jane' // Error
// readonlyUser.address.city = 'New York' // Error
```

## Combining

```ts
// keyof (A & B) = (keyof A) | (keyof B)

type A = { a: string }
type KeyOfA = keyof A // => 'a'

type B = { b: number }
type KeyOfB = keyof B // => 'b'

type C = A & B
type KeyOfC = keyof C // => 'a' | 'b'

// keyof (A | B) = (keyof A) & (keyof B)

type A = { a: string; c: boolean }
type KeyOfA = keyof A // => 'a' | 'c'

type B = { b: number; c: boolean }
type KeyOfB = keyof B // => 'b' | 'c'

type C = A | B
type KeyOfC = keyof C // => 'c'
```

# Helper/Utility Types

TypeScript provides several built-in utility types that help you manipulate and work with types.

## Record

```ts
// Record<K, T>

type PaymentStatus = Record<string, boolean>
// Equal to:
type PaymentStatus = { [key: string]: boolean }
```

```ts
type PaymentStatus = Record<'free' | 'paid', boolean>
// These are equivalent:
type PaymentStatus = { free: boolean; paid: boolean }
type PaymentStatus = { [Key in 'free' | 'paid']: boolean }

// Can access to keys like this:
type ValueType = PaymentStatus[string]
// type ValueType = boolean
```

## ReadOnly

```ts
// ReadOnly<Type>

type User = {
  name: string
  age: number
  isAdmin: boolean
}

type ReadOnlyUser = Readonly<User>
// type ReadOnlyUser = { readonly name: string; readonly age: number; readonly isAdmin: boolean }
```

## Partial

```ts
// Partial<Type>

type User = {
  name: string
  age: number
  isAdmin: boolean
}

type PartialUser = Partial<User>
// type PartialUser = { name?: string; age?: number; isAdmin?: boolean }

type PartialUser = Partial<User, 'name' | 'age'>
// type PartialUser = { name?: string; age?: number }
```

## Exclude

```ts
// Exclude<UnionType, ExcludedMembers>

type User = 'name' | 'age' | 'isAdmin'

type ExcludedUser = Exclude<User, 'isAdmin'>
// type ExcludedUser = 'name' | 'age'
```

## Extract

```ts
// Extract<Type, Union>

type User = 'name' | 'age' | 'isAdmin'

type ExtractedUser = Extract<User, 'name' | 'age'>
// type ExtractedUser = 'name' | 'age'
```

## Required

```ts
// Required<Type>

type User = { name?: string; age?: number; isAdmin?: boolean }

type RequiredUser = Required<User>
// type RequiredUser = { name: string; age: number; isAdmin: boolean }

type RequiredUser = Required<User, 'name' | 'age'>
// type RequiredUser = { name: string; age: number }
```

## Pick

```ts
// Pick<Type, Keys>

type User = { name: string; age: number; isAdmin: boolean }

type PickUser = Pick<User, 'name' | 'age'>
// type PickUser = { name: string; age: number }
```

## Omit

```ts
// Omit<Type, Keys>

type User = { name: string; age: number; isAdmin: boolean }

type OmitUser = Omit<User, 'isAdmin'>
// type OmitUser = { name: string; age: number }
```

## Awaited

```ts
// Awaited<Type>

type A = Awaited<Promise<string>> // type A = string
type B = Awaited<boolean | Promise<number>> // type B = boolean | number

// Example with nested promises
type C = Awaited<Promise<Promise<Promise<boolean>>>> // type C = boolean

// Example with function that returns a promise
async function getData() {
  return { id: 1, name: 'John' }
}

type D = Awaited<ReturnType<typeof getData>> // type D = { id: number; name: string }
```

## NonNullable

```ts
// NonNullable<Type>

type User = { name: string; age: number | null }

type NonNullableUser = NonNullable<User['age']>
// type NonNullableUser = number
```

## Parameters

```ts
// Parameters<Type>

type Fn = (name: string, age: number) => boolean

type FnParameters = Parameters<Fn>
// type FnParameters = [name: string, age: number]
```

## ConstructorParameters

```ts
// ConstructorParameters<Type>

type User = new (name: string, age: number) => boolean

type UserParameters = ConstructorParameters<User>
// type UserParameters = [name: string, age: number]
```

## ReturnType

```ts
// ReturnType<Type>

type Fn = (name: string, age: number) => boolean

type FnReturnType = ReturnType<Fn>
// type FnReturnType = boolean
```

## InstanceType

```ts
// InstanceType<Type>

type User = new (name: string, age: number) => boolean

type UserInstanceType = InstanceType<User>
// type UserInstanceType = boolean
```

## NoInfer

```ts
// NoInfer<Type>

type InferredType<T> = { x: T }
type HasNoInfer<T> = { x: NoInfer<T> }

// Without NoInfer, TypeScript infers T as string | number
declare function fn1<T>(obj: InferredType<T>): T
const x1 = fn1({ x: 1 }) // T inferred as number
const y1 = fn1({ x: 'hello' }) // T inferred as string

// With NoInfer, TypeScript requires explicit type parameter
declare function fn2<T>(obj: HasNoInfer<T>): T
const x2 = fn2<number>({ x: 1 }) // Must specify T as number
const y2 = fn2<string>({ x: 'hello' }) // Must specify T as string
```

## String Manipulator

```ts
// Uppercase<StringType>
// Lowercase<StringType>
// Capitalize<StringType>
// Uncapitalize<StringType>

type UppercaseExample = Uppercase<'hello'> // type UppercaseExample = "HELLO"
type LowercaseExample = Lowercase<'HELLO'> // type LowercaseExample = "hello"
type CapitalizeExample = Capitalize<'hello'> // type CapitalizeExample = "Hello"
type UncapitalizeExample = Uncapitalize<'Hello'> // type UncapitalizeExample = "hello"

// Can be used with template literals
type Greeting<T extends string> = `${Uppercase<T>} WORLD!`
type ShoutHello = Greeting<'hello'> // type ShoutHello = "HELLO WORLD!"

// Can be chained together
type Title = `${Capitalize<Lowercase<'TYPESCRIPT'>>} Guide`
// type Title = "Typescript Guide"
```

# Conditional Types

```ts
// Basic example
type IsString<T> = T extends string ? 'yes' : 'no'

type Result1 = IsString<string> // type Result = 'yes'
type Result2 = IsString<number> // type Result2 = 'no'

// IF example
type If<C extends boolean, T, F> = C extends true ? T : F

type Result1 = If<true, 'yes', 'no'> // ✅ type Result3 = 'yes'
type Result2 = If<false, 'yes', 'no'> // ❌ type Result4 = 'no'
```

## Nested conditions

```ts
// Example with nested conditions
type GetTheme<I extends 0 | 1 | 2> = {
  0: 'light'
  1: 'neutral'
  2: 'dark'
}[I]

type Theme1 = GetTheme<0> // type Theme = "light"
type Theme2 = GetTheme<1> // type Theme2 = "neutral"
type Theme3 = GetTheme<2> // type Theme3 = "dark"

// Example usage in a function
const getThemeString = <T extends 0 | 1 | 2>(theme: T): GetTheme<T> => {
  const themes = {
    0: 'light',
    1: 'neutral',
    2: 'dark'
  } as const

  return themes[theme] as GetTheme<T>
}

// Usage examples
const lightTheme = getThemeString(0) // 'light'
const neutralTheme = getThemeString(1) // 'neutral'
const darkTheme = getThemeString(2) // 'dark'

// Type safety examples
const invalidTheme = getThemeString(3) // ❌ Error: Argument of type '3' is not assignable to parameter of type '0 | 1 | 2'
```

## Type Constraints

```ts
// Basic example
// Without constraint
const createdUser = (name: string) => ({ name })
const user = createdUser('John') // { name: string }
// With constraint
const createdUser = <S extends string>(name: S) => ({ name })
const userExample = createdUser('John') // { name: John }

// example with tuple
// with simple constraint
const inferAsTuple = <T extends any[]>(tuple: T) => tuple
const t1 = inferAsTuple([1, 2]) // number[]
const t2 = inferAsTuple(['b', 3, false]) // (string, number, boolean)[]
// with precise constraint
const inferAsTuple = <T extends [unknown, ...unknown[]]>(tuple: T) => tuple
const t1 = inferAsTuple([1, 2]) // [number, number]
const t2 = inferAsTuple(['b', 3, false]) // [string, number, boolean]
```

```ts
// Define available plans and roles
type Plan = 'basic' | 'pro' | 'premium'
type Role = 'viewer' | 'editor' | 'admin'

// Type to check if a user with given plan and role can edit
type CanEdit<P extends Plan, R extends Role> = [P, R] extends ['pro' | 'premium', 'editor' | 'admin'] ? true : false

// Example usage in a function
const hasAllowedToEdit = <P extends Plan, R extends Role>(plan: P, role: R): boolean => {
  // Check if plan is pro/premium AND role is editor/admin
  return true as CanEdit<P, R>
}

// Usage examples
const user1 = hasAllowedToEdit('basic', 'editor') // ❌ false
const user2 = hasAllowedToEdit('pro', 'editor') // ✅ true
const user3 = hasAllowedToEdit('basic', 'admin') // ❌ false
const user4 = hasAllowedToEdit('premium', 'admin') // ✅ true
const user5 = hasAllowedToEdit('pro', 'viewer') // ❌ false
```

## Infer Types

```ts
// Define possible roles
type Role = 'Admin' | 'Editor' | 'Viewer'

// Define user type
type User = {
  name: string
  role: Role
}

// Extract role from user type
type GetRole<User> = User extends { name: string; role: infer Role } ? Role : never

// Example usage:
const user1: User = { name: 'John', role: 'Admin' }
const user2 = { name: 'Bob' }

// Function that uses GetRole
const getUserRole = <U extends unknown>(user: U): GetRole<U> => {
  return (user as any).role
}

const johnRole = getUserRole(user1) // johnRole is "Admin"
const bobRole = getUserRole(user2) // bobRole is never
```

## Extract Type with Infer

```ts
// Extract Parameters and Return types from function
type GetParametersType<F> = F extends (...params: infer P) => any ? P : never
type GetReturnType<F> = F extends (...params: any[]) => infer R ? R : never

type Fn = (name: string, id: number) => boolean

type ParametersType = GetParametersType<Fn> // type ParametersType = [name: string, id: number]
type ReturnedType = GetReturnType<Fn> // type ReturnedType = boolean

// Extract type from Generic Type
type GenericType<A, B> = { content: A; children: B[] }
type ExtractParams<S> = S extends GenericType<infer A, infer B> ? [A, B] : never
type ExtractedType = ExtractParams<GenericType<number, string>> // type ExtractedType = [number, string]
// Another example
type ExtractType<T> = T extends Array<infer U> ? U : never
type ExtractedType = ExtractType<Array<string>> // type ExtractedType = string

// Extract type from last element of array
type ExtractLastElement<T> = T extends [...infer _Rest, infer Last] ? Last : never
type ExtractedType = ExtractLastElement<[1, 2, 3, 4, 5]> // type ExtractedType = 5

// Extract type from first element of array
type ExtractFirstElement<T> = T extends [infer First, ...infer _Rest] ? First : never
type ExtractedType = ExtractFirstElement<[1, 2, 3, 4, 5]> // type ExtractedType = 1
```

## Check with Tuple

```ts
// Examples with logical operators
type AND<A extends boolean, B extends boolean> = [A, B] extends [true, true] ? true : false
type NAND<A extends boolean, B extends boolean> = [A, B] extends [true, true] ? false : true
type OR<A extends boolean, B extends boolean> = [A, B] extends [false, false] ? false : true
type NOR<A extends boolean, B extends boolean> = [A, B] extends [false, false] ? true : false
type XOR<A extends boolean, B extends boolean> = [A, B] extends [false, false] | [true, true] ? false : true
type XNOR<A extends boolean, B extends boolean> = [A, B] extends [false, false] | [true, true] ? true : false
```

# Iterative and Loop Methods

## Find loop

```ts
type Column = {
  name: string
  values: unknown[]
}

type Table = [Column, ...Column[]]

type StudentTable = [
  { name: 'studentId'; values: number[] },
  { name: 'grades'; values: number[] },
  { name: 'enrolled'; values: Date[] }
]

const students: StudentTable = [
  { name: 'studentId', values: [1001, 1002, 1003] },
  { name: 'grades', values: [85, 92, 78] },
  { name: 'enrolled', values: [new Date('2023-01-01'), new Date('2023-01-15'), new Date('2023-02-01')] }
]

// type Loop<List /* ... other params */> =
//   // 1. Split the list:
//   List extends [infer First, ...infer Rest]
//     ? // 2. Compute something using the first element.
//       //    Maybe recurse on the `Rest`:
//       Loop<Rest /* ... modified params */>
//     : // 3. Return a default type if the list is empty:
//       SomeDefault;
type GetColumn<List, Name> = List extends [infer First, ...infer Rest]
  ? First extends { name: Name; values: infer Values }
    ? Values
    : GetColumn<Rest, Name>
  : undefined

type Result1 = GetColumn<StudentTable, 'studentId'> // number[]
type Result2 = GetColumn<StudentTable, 'enrolled'> // Date[]
```

## Map loop

```ts
// type Map<List> =
// List extends [infer First, ...infer Rest]
//   ? [ /* ... your logic */ , ...Map<Rest>]
//   : [];
type GetProperty<Person, Prop extends string> = Person extends {
  [K in Prop]: infer Value
}
  ? Value
  : 0

type MapProperty<List, Prop extends string> = List extends [infer First, ...infer Rest]
  ? [GetProperty<First, Prop>, ...MapProperty<Rest, Prop>]
  : []

// Example usage:
type Person = { id: number; age: number; name: string }
type People = [{ id: 1; age: 25; name: 'Alice' }, { id: 2; age: 30; name: 'Bob' }, { id: 3; age: 28; name: 'Charlie' }]

type Ages = MapProperty<People, 'age'> // [25, 30, 28]
type Names = MapProperty<People, 'name'> // ["Alice", "Bob", "Charlie"]
type Ids = MapProperty<People, 'id'> // [1, 2, 3]
```

## Filter loop

```ts
// type Filter<List> =
// List extends [infer First, ...infer Rest]
//   ? First extends  /* ... your condition */
//     ? [First, ...Filter<Rest>]
//     : Filter<Rest>
//   : [];

type OnlyStrings<List> = List extends [infer First, ...infer Rest]
  ? First extends string
    ? [First, ...OnlyStrings<Rest>]
    : OnlyStrings<Rest>
  : []

type Strings = OnlyStrings<[1, 'hello', true, 'world', 42, 'typescript']>
// type Strings = ["hello", "world", "typescript"]
```

## Reduce loop

```ts
// type Reduce<Tuple, Acc = /* ... initial value */> =
// Tuple extends [infer First, ...infer Rest]
// ? Reduce<Rest, /* ... logic */>
// : Acc;

type FromEntries<Entries, Acc = {}> = Entries extends [infer Entry, ...infer Rest]
  ? FromEntries<Rest, Entry extends [infer Key extends PropertyKey, infer Value] ? Acc & { [K in Key]: Value } : Acc>
  : Acc

type Product = FromEntries<[['title', 'iPhone'], ['price', 999], ['inStock', true]]>
// type Product = { title: string; price: number; inStock: boolean }
```

# Template Literals

Template literals allow you to create string literal types by combining other string literals, numbers, and types.

```ts
// Basic example
type World = 'world'
type Greeting = `hello ${World}` // type Greeting = "hello world"
```

```ts
// Example with object keys
type User = {
  firstName: string
  lastName: string
  age: number
}

type UserFields = keyof User // "firstName" | "lastName" | "age"
type UserPaths = `user.${UserFields}` // "user.firstName" | "user.lastName" | "user.age"

// Can create nested paths
type Nested = {
  user: {
    info: {
      name: string
      email: string
    }
    settings: {
      theme: string
      notifications: boolean
    }
  }
}

type NestedPaths = `user.${keyof Nested['user']}.${keyof Nested['user']['info'] | keyof Nested['user']['settings']}`
// type NestedPaths = "user.info.name" | "user.info.email" | "user.settings.theme" | "user.settings.notifications"
```

```ts
// Example with primitive types
type ID = number | boolean
type UserID = `user_${ID}` // type UserID = `user_${number}` | `user_${boolean}`

// Another example with check email
type Email = `${string}@${string}.${string}`
const email1: Email = 'test@example.com' // ✅
const email2: Email = 'invalid-email' // ❌ Type '"invalid-email"' is not assignable to type '`${string}@${string}.${string}`'
```

```ts
// Unions can be used in template literals
type Color = 'red' | 'blue'
type Quantity = 1 | 2
type Item = `${Quantity} ${Color} items` // type Item = "1 red items" | "1 blue items" | "2 red items" | "2 blue items"

// Another example with creating CSS class names
type Alignment = 'left' | 'right' | 'center'
type Size = 'sm' | 'md' | 'lg'
type ClassName = `align-${Alignment}-${Size}`
// type ClassName = "align-left-sm" | "align-left-md" | "align-left-lg" | "align-right-sm" | "align-right-md" | "align-right-lg" | "align-center-sm" | "align-center-md" | "align-center-lg"
```

```ts
// Template Literals can be used to pattern matching
type Product = {
  id: number
  description: string
  price: number
}

type Order = {
  id: number
  description: string
}

type Method = 'GET' | 'PUT'
type Resource = 'order' | 'product'

type PropName = `${Lowercase<Method>}${Capitalize<Resource>}`
type HTTPService = Record<PropName, Function>

const httpService = {
  getOrder: () => Promise.resolve({ id: 1, description: 'Order 1', price: 100 }),
  putOrder: (order: Order) => Promise.resolve(),
  getProduct: () => Promise.resolve({ id: 1, description: 'Product 1', price: 100 })
} satisfies HTTPService // ❌ putProduct function is missing!
```

```ts
// Template literals can be used to split strings
type SplitDomain<Name> = Name extends `${infer Sub}.${infer Domain}.${infer Extension}`
  ? [Sub, Domain, Extension]
  : never

type DomainParts = SplitDomain<'www.typescriptlang.org'> // type DomainParts = ["www", "typescriptlang", "org"]
```

# TypeScript Tips and Tricks

## Create Type from Enum

```ts
enum Categories {
  Nature = 'nature',
  Science = 'science',
  Economy = 'economy'
}

type Category = keyof typeof Categories
// type Category = "Nature" | "Science" | "Economy"
```

## Create Type from Function return

Instead of create primitive type manually like:

```ts
type Status = 'paid' | 'free'

let productStatus: Status

productStatus = 'paid' // ✅
productStatus = 'free' // ✅
productStatus = 'something' // ❌ Type '"something"' is not assignable to type 'Status'.
```

You can dynamically create a type from a function's return type:

```ts
const getPaidStatus = () => {
  return 'paid' as const
}

const getFreeStatus = () => {
  return 'free' as const
}

type Status = ReturnType<typeof getPaidStatus> | ReturnType<typeof getFreeStatus>
// equal to: type Status = "paid" | "free"

let productStatus: Status

productStatus = 'paid' // ✅
productStatus = 'free' // ✅
productStatus = 'something' // ❌ Type '"something"' is not assignable to type 'Status'.
```

## Stronger Types with Branded Types and Type Aliases

Branded types helps you create types that are nominally unique, even if they have the same underlying structure.
This is useful when you want to prevent values of the same shape from being used interchangeably.

Example:

```ts
// Define two types for ProductId and OrderId that are branded with a unique symbol but different brand
type ProductId = string & { readonly __brand: unique symbol }
type OrderId = string & { readonly __brand: unique symbol }

// Create functions to safely create these types
const createProductId = (id: string): ProductId => id as ProductId
const createOrderId = (id: string): OrderId => id as OrderId

const processProduct = (id: ProductId) => console.log(`Processing product: ${id}`)
const processOrder = (id: OrderId) => console.log(`Processing order: ${id}`)

// Example usage
const productId = createProductId('product123')
const orderId = createOrderId('order123')

processProduct(productId) // ✅ OK
processOrder(orderId) // ✅ OK

// These will cause type errors
processProduct(orderId) // ❌ Type 'OrderId' is not assignable to parameter of type 'ProductId'
processOrder(ProductId) // ❌ Type 'ProductId' is not assignable to parameter of type 'OrderId'
processProduct('product123') // ❌ Type 'string' is not assignable to parameter of type 'ProductId'
```

## Extract keys by value type

```ts
type ExtractKeysByValue<T, V> = {
  [K in keyof T]: T[K] extends V ? K : never
}[keyof T]

type Product = {
  id: number
  description: string
  price: number
}

type ProductKeys = ExtractKeysByValue<Product, string>
// type ProductKeys = "description"
```

## Conditional Types

Conditional types help you create types that depend on other types. They follow an `if/else` structure using the syntax: `T extends U ? X : Y`

Example:

```ts
// Example with multiple conditions
type MediaType<T> = T extends { type: 'image' }
  ? { url: string; width: number; height: number }
  : T extends { type: 'video' }
  ? { url: string; duration: number }
  : never

// Usage:
const image: MediaType<{ type: 'image' }> = {
  url: 'photo.jpg',
  width: 1920,
  height: 1080
} // ✅

const video: MediaType<{ type: 'video' }> = {
  url: 'video.mp4',
  duration: 120
} // ✅

const audio: MediaType<{ type: 'audio' }> = {
  url: 'audio.mp3'
} // ❌ Type 'never' has no properties
```

## XOR Operator

The XOR (exclusive OR) operator can be used to create a type that have exactly one of two possible sets of properties, but not both.

```ts
// Without marks all properties from one type as optional and never when they exist in the other type
type Without<T, U> = { [P in Exclude<keyof T, keyof U>]?: never }

type XOR<T, U> = T | U extends object ? (Without<T, U> & U) | (Without<U, T> & T) : T | U

type User = { name: string } & XOR<{ isAdmin: true; adminKey: string }, { isAdmin: false }>

const jack: User = {
  name: 'Jack',
  isAdmin: false
} // ✅

const admin: User = {
  name: 'Admin',
  isAdmin: true,
  adminKey: 'XXxxXXxxXXxx'
} // ✅

const badUser: User = {
  name: 'John'
} // ❌ Type '{ name: string; }' is missing the following properties from type '{ isAdmin: true; adminKey: string; }': isAdmin, adminKey

const badAdmin: User = {
  name: 'Admin',
  isAdmin: true
} // ❌ Property 'adminKey' is missing in type '{ name: string; isAdmin: true; }' but required in type '{ isAdmin: true; adminKey: string; }
```

## Check data with asserts condition

The `asserts` condition provides a safer way to narrow types by throwing an error if the condition is not met. This is particularly useful for runtime type checking and validation.

Example:

```ts
type User = { name: string; email: string }

// Type assertion function that validates an unknown input is a User
// This function will throw an error if the input is not a valid User

// Keep in mind that: arrow functions can't be used for assertion
function assertUser(input: unknown): asserts input is User {
  if (!input || typeof input !== 'object') {
    throw new Error('Input data are missing or not an object!')
  }

  if (!('name' in input) || typeof input.name !== 'string') {
    throw new Error('Username is missing!')
  }

  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

  if (!('email' in input) || typeof input.email !== 'string' || !emailPattern.test(input.email)) {
    throw new Error('Use email should be valid!')
  }
}

const createUser = (input: unknown) => {
  // At this point input type is unknown
  assertUser(input)

  // After assertion passes, TypeScript knows input is a valid User
  console.log(`Name: ${input.name}, Email: ${input.email}`) // Name: Jack, Email: jack@example.com
}

// Example usage:
createUser({ name: 'Jack', email: 'jack@example.com' }) // ✅ OK - Valid user object
createUser({ name: 'Jack', email: 'unvalid.com' }) // ❌ Use email should be valid!
createUser({ email: 'jack@example.com' }) // ❌ Username is missing!
```

# Generic Tips and Tricks

## Avoid nested Try/Catch

Old way:

```javascript
const getJsonData = async () => {
  try {
    const response = await fetch('https://api.example.com/data')
    const jsonData = await response.json()

    return jsonData
  } catch (error) {
    console.error(error)
  }
}
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
const obj = { a: 10, b: 0, c: 1 }

obj.z ??= 40 // if z is null or undefined, set z to 40
console.log(obj) // { a: 10, b: 0, c: 1, z: 40 }

obj.b ||= 20 // if b is false, set b to 20
console.log(obj) // { a: 10, b: 20, c: 1, z: 40 }

obj.c &&= 30 // if c is true or have value, set c to 30
console.log(obj) // { a: 10, b: 20, c: 30, z: 40 }
```
