# Object / Reference Data Types

Reference types in JS are objects. Therefore, they come with **properties and methods** that can be accessed and modified.

Unlike primitive types, reference types are **not stored directly in the variable**. The variable instead holds a **reference (pointer) to the memory location** where the actual object lives.

So, when a variable is created to store a reference type, it does not store the object itself — it stores a **reference** to it.

|                    | Primitive Types          | Reference Types                              |
| ------------------ | ------------------------ | -------------------------------------------- |
| Stored in variable | The actual value         | A reference (pointer) to the memory location |
| Copied by          | Value (independent copy) | Reference (shared pointer)                   |
| Comparison         | Compares values          | Compares memory references, not content      |

1. Objects
2. Arrays
3. Functions

---

# 1. Objects

## Explanation

A collection of **key-value pairs**, used to represent structured, named data. The most fundamental reference type in JavaScript — arrays and functions are, in fact, specialized kinds of objects under the hood.

## Example

```js
let objectVariableType = {
  name: "Ali",
  age: 28,
  city: "New York",
};
```

## Important Key Points

- Represented as a **collection of key-value pairs**.
- Properties can be accessed via:
  - **Dot notation:** `objectVariableType.name`
  - **Bracket notation:** `objectVariableType["name"]` (required when the key is dynamic or not a valid identifier)
- Objects are **copied by reference**, not by value:

```js
let obj1 = { x: 1 };
let obj2 = obj1; // obj2 points to the SAME object as obj1
obj2.x = 99;
console.log(obj1.x); // 99 — changed through obj2, but visible on obj1 too! NOT COPIED!
```

- Changes made through one reference are **immediately visible** through any other variable pointing to the same object!
- When two variables point to the same reference, modifying the object through **either one** affects both.
- `typeof objectVariableType === "object"`.
- Equality checks compare **reference, not content!**:

```js
  { a: 1 } === { a: 1 }; // false — different memory references, even with identical content
```

---

# 2. Arrays

## Explanation

A special type of object that enables storing **multiple, ordered values** in a single variable.

## Example

```js
let arrayTypeVariable = [1, 2, 3, 4, 5, 6];
```

## Important Key Points

- Like objects, arrays are **shared/copied by reference** among variables.
- Provide a rich set of built-in **methods and properties** for data management and retrieval (`.push()`, `.map()`, `.filter()`, `.length`, etc.).
- Always **zero-indexed** — the first element is at index `0`.
- `typeof arrayTypeVariable === "object"` — arrays are technically a specialized kind of object, not a separate primitive type.
  - To reliably check for an array, use `Array.isArray(arrayTypeVariable)` instead of `typeof`.
- Reassigning a variable that points to the same array reference affects all variables sharing it, just like objects.

---

# 3. Functions

## Explanation

A **reusable, callable block of code** designed to perform a task or compute and return a value. In JavaScript, functions are also treated as **first-class objects** — they can be stored, passed around, and manipulated like any other value.

## Example

```js
function greet(name) {
  console.log("Hello, ", name);
}

let myFunction = greet;
myFunction("Ali"); // "Hello, Ali"
```

## Important Key Points

- Can be **invoked** (called) to perform tasks or return values.
- Can be **assigned to variables** and **passed as arguments** to other functions — this is what makes JavaScript's functional-programming style possible.
- Assigning a function to a variable **establishes a reference** to its memory location — it does **not** create a copy of the function.
- `typeof greet === "function"` (functions get their own distinct `typeof` result, even though they are technically objects under the hood).
- Because functions are first-class objects, powerful patterns become available:
  - **Higher-order functions:** functions that take other functions as arguments or return a function (e.g. `.map()`, `.filter()`, `.reduce()`).
  - **Callbacks:** functions passed into another function to be executed later.
  - **Functional programming:** composing behavior by combining small, reusable functions.
