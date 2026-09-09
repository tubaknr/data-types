# Primitive Data Types

1. [string](#1-string)
2. [number](#2-number)
3. [bigint](#3-bigint)
4. [boolean](#4-boolean)
5. [undefined](#5-undefined)
6. [symbol](#6-symbol)
7. [null](#7-null)

---

# 1. String

## Explanation

A sequence of characters used to represent text.

## Example

```js
const stringTypeVariable = "This is a sequence of characters.";
```

## Important Key Points

- JavaScript strings use the **UTF-16** encoding internally.
- Each **code unit** in UTF-16 is **16 bit = 2 byte**.

| Unit                                   | Size                                   |
| -------------------------------------- | -------------------------------------- |
| 1 code unit                            | 16 bit = 2 byte                        |
| Most common characters (BMP)           | 1 code unit = 2 byte                   |
| Emojis / rare characters (outside BMP) | 2 code units (surrogate pair) = 4 byte |

- `stringTypeVariable.length` returns the **number of UTF-16 code units** — **not** necessarily the number of visible characters.
  - For most everyday characters (letters, digits, punctuation), 1 code unit = 1 visible character, so `.length` matches what you'd intuitively count.
  - For characters outside the Basic Multilingual Plane (BMP) — such as many emojis — 1 visible character is represented by **2 code units** (a _surrogate pair_), so `.length` reports `2` for what looks like a single emoji:

```js
"😀".length; // 2  (not 1!)
```

- **Approximate memory occupied** by a string: memory ≈ string.length \* 2 byte

---

# 2. Number

## Explanation

Numeric data type in the **double precision 64-bit floating point format**.

**Generic Format:** IEEE 754 Double Precision

## Example

```js
const numberTypeVariable = 465;
```

## Important Key Points

- Total size: **64 bit = 8 byte**
- Bit breakdown: `64 bit = sign (1 bit) + exponent (11 bit) + fraction (52 bit)`

| Part                | Bits | Purpose                                   |
| ------------------- | ---- | ----------------------------------------- |
| Sign                | 1    | Positive / negative                       |
| Exponent            | 11   | Magnitude / scale of the number           |
| Fraction (Mantissa) | 52   | Precision (the actual significant digits) |

- There are **no separate number types** like in C/C++ (`int`, `float`, `double`, `long`, etc.) — JS has only one `Number` type.
  - ✅ **Advantage:** can represent both very large and very small numbers using the same type.
  - ⚠️ **Drawback:** limited precision.

- Because of this precision limit:

```js
0.1 + 0.2 === 0.3; // false
0.1 + 0.2; // 0.30000000000000004
```

- To solve the precision/large-integer problem, JS introduced **`BigInt`** (ES2020).

```js
const big = 9007199254740993n; // note the trailing "n"
typeof big; // "bigint"
```

- **Safe max integer:** `2^53 - 1` → `Number.MAX_SAFE_INTEGER`

- **`NaN`**
  - `typeof NaN === "number"`
  - Means an **"invalid mathematical calculation"** result.
  - Special quirk: `NaN === NaN` → `false`

- Integer check example:

```js
Number.isInteger(5.0); // true
Number.isInteger(5.5); // false
```

---

# 3. BigInt

## Explanation

A numeric data type that can represent **integers in arbitrary precision format**.

Numbers larger than the `Number` type can safely handle are handled reliably with `BigInt`.

## Example

```js
const bigIntTypeVariable = 9007199254740993n;
```

## Important Key Points

- Takes an **`n`** suffix at the end of the literal.
- Used for representing **very large integers**.
- Introduced in **ES2020**.
- Can also be created using the **`BigInt()`** function:

```js
const bigIntTypeVariable = BigInt(9007199254740993);
```

- `BigInt` **does not have a fixed bit size** — its size in memory **grows dynamically** as the number gets larger.
- There is **no upper precision limit**.
- Solves the precision problem that comes with `Number`:

```js
9007199254740992 === 9007199254740993; // true  (Number precision loss!)
9007199254740992n === 9007199254740993n; // false (BigInt is exact)
```

- **Cannot be mixed with `Number`** in arithmetic operations:

```js
10n + 5; // ❌ TypeError: Cannot mix BigInt and other types, use explicit conversions
10n + 5n; // ✅ 15n
```

- Does **not accept fractional or decimal values** — integers only:

```js
BigInt(5.5); // ❌ RangeError: The number 5.5 cannot be converted to a BigInt
```

---

# 4. Boolean

## Explanation

A logical data type that can have only one of two values: `true` or `false`.

## Example

```js
const booleanTypeVariable = true;
const booleanTypeVariable2 = false;
```

## Important Key Points

- Used frequently in **conditional logic** (`if` statements) and **flow control** (loops):

```js
if (booleanTypeVariable) {
  // code to execute
}
```

- Conceptually needs only **1 bit** to represent 2 states.
- JavaScript **does not guarantee a specific memory size** for it — this is left to the engine's internal implementation (e.g. the V8 engine).
- There are values called **truthy** and **falsy** values — they _behave_ like boolean values in a conditional context, but they are **not actually of type `Boolean`**.
  - Example: `0` is falsy, but its type is `"number"`, not `"boolean"`.
  - Example: `""` (empty string) is falsy, but its type is `"string"`, not `"boolean"`.
- **Falsy values** in JavaScript (exactly 8 of them): false, 0, -0, 0n, "", null, undefined, NaN
- **Truthy value examples:** `[]`, `{}`, `"0"` — even though these may _seem_ empty or "zero-like," they are still truthy:

```js
Boolean([]); // true
Boolean({}); // true
Boolean("0"); // true
```

- `typeof booleanTypeVariable === "boolean"`.

---

# 5. Undefined

## Explanation

Automatically assigned to variables that have just been declared but not yet initialized.
Also automatically assigned to function arguments for which no actual argument was provided.

## Example

```js
let x; // declared, not initialized — undefined is assigned to it automatically
const undefinedTypeVariable = undefined;
```

## Important Key Points

- Means **"value missing"**.
- Represents the **absence of assignment**.
- It is a **type and a value at the same time**.
- A function with **no explicit `return`** returns `undefined`.
- Accessing a **missing object property** returns `undefined`.
- If a function is called with a **missing argument**, that parameter is `undefined`.
- Accessing an **out-of-bounds array index** returns `undefined`.
- `undefined` is one of the **falsy values** in JavaScript:

```js
Boolean(undefined); // false
```

- `undefined` is assigned **automatically by JavaScript**, not deliberately by the developer (unlike `null`).
- `typeof undefined === "undefined"`.
- **`undefined` vs `null` comparison:**

```js
undefined == null; // true  (loose equality — value comparison, ignores type)
undefined === null; // false (strict equality — checks type as well)
```

---

# 6. Symbol

## Explanation

A built-in primitive type whose constructor returns a **unique, immutable value** — a `Symbol`. It is guaranteed to be unique, even if two symbols are created with the exact same description.

## Example

```js
const id = Symbol("id");
```

## Important Key Points

- Enables a **weak form of encapsulation / information hiding** in objects.
- It is the **only primitive data type with reference identity** — every `Symbol` value is distinct from every other, even when created identically:

```js
Symbol("id") === Symbol("id"); // false
```

- Symbols are **always unique**, regardless of having the same description string.
- Can be used as **object property keys**:

```js
const obj = { [id]: 123 };
```

- Symbol-keyed properties are **not enumerated** by common iteration methods — they are excluded from:
  - `Object.keys()`
  - `for...in` loops
  - `JSON.stringify()`

### Common Use Cases

- **Avoiding name collisions:** Adding new properties to objects shared across third-party libraries or common codebases without risking accidentally overwriting an existing property.
- **Hiding internal state:** Storing object state that shouldn't be picked up by generic enumeration mechanisms (`for...in`, `JSON.stringify`, etc.).
- **Well-known symbols (system symbols):** Used internally by JavaScript to customize an object's built-in behavior — for example, `Symbol.iterator` makes an object iterable in a `for...of` loop:

```js
const iterableObj = {
  [Symbol.iterator]() {
    let i = 0;
    return {
      next: () =>
        i < 3 ? { value: i++, done: false } : { value: undefined, done: true },
    };
  },
};
[...iterableObj]; // [0, 1, 2]
```

---

# 7. Null

## Explanation

Represents an **intentional absence of any object value** — a nonexistent or invalid reference. Unlike `undefined`, this is a value that must be **explicitly assigned by the developer**, not something JavaScript sets automatically.

## Example

```js
let user = null;
```

## Important Key Points

- `typeof null === "object"` — this is a well-known **historical bug in JavaScript** that cannot be fixed now without breaking backward compatibility.
- Set **explicitly by the developer**, unlike `undefined`, which JavaScript assigns automatically.
- Signals that "the developer has deliberately given this variable no value" (as opposed to `undefined`, which means "no value has been given yet").
- It is one of the **8 falsy values**:

```js
Boolean(null); // false
```

- **Optional chaining (`?.`)** is used to safely access a property when the object might be `null` (or `undefined`), avoiding a `TypeError`:

```js
const name = user?.profile?.name; // returns undefined instead of throwing, if user or profile is null/undefined
```

- **Nullish coalescing (`??`)** checks whether the left-hand side is `null` or `undefined`, and if so, falls back to the right-hand side:

```js
const activeUser = user ?? "Guest User";
```

- **`null` vs `undefined` comparison:**

```js
null == undefined; // true  (loose equality — value comparison, ignores type)
null === undefined; // false (strict equality — checks type as well)
```
