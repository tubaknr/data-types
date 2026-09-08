# Primitive Data Types
1. string
2. number
3. bigint
4. boolean
5. undefined
6. symbol
7. null

------
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

| Unit | Size |
|---|---|
| 1 code unit | 16 bit = 2 byte |
| Most common characters (BMP) | 1 code unit = 2 byte |
| Emojis / rare characters (outside BMP) | 2 code units (surrogate pair) = 4 byte |

- `stringTypeVariable.length` returns the **number of UTF-16 code units** — **not** necessarily the number of visible characters.
  - For most everyday characters (letters, digits, punctuation), 1 code unit = 1 visible character, so `.length` matches what you'd intuitively count.
  - For characters outside the Basic Multilingual Plane (BMP) — such as many emojis — 1 visible character is represented by **2 code units** (a *surrogate pair*), so `.length` reports `2` for what looks like a single emoji:
```js
    "😀".length; // 2  (not 1!)
```

- **Approximate memory occupied** by a string: memory ≈ string.length * 2 byte


----------------------------------------------
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

| Part | Bits | Purpose |
|---|---|---|
| Sign | 1 | Positive / negative |
| Exponent | 11 | Magnitude / scale of the number |
| Fraction (Mantissa) | 52 | Precision (the actual significant digits) |

- There are **no separate number types** like in C/C++ (`int`, `float`, `double`, `long`, etc.) — JS has only one `Number` type.
  - ✅ **Advantage:** can represent both very large and very small numbers using the same type.
  - ⚠️ **Drawback:** limited precision.

- Because of this precision limit:
```js
  0.1 + 0.2 === 0.3   // false
  0.1 + 0.2           // 0.30000000000000004
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
-----------------------------------------------------------------------------
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
  9007199254740992 === 9007199254740993;   // true  (Number precision loss!)
  9007199254740992n === 9007199254740993n; // false (BigInt is exact)
```
- **Cannot be mixed with `Number`** in arithmetic operations:
```js
  10n + 5;  // ❌ TypeError: Cannot mix BigInt and other types, use explicit conversions
  10n + 5n; // ✅ 15n
```
- Does **not accept fractional or decimal values** — integers only:
```js
  BigInt(5.5); // ❌ RangeError: The number 5.5 cannot be converted to a BigInt
```

-----------------------------------------------------------------------------
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
- There are values called **truthy** and **falsy** values — they *behave* like boolean values in a conditional context, but they are **not actually of type `Boolean`**.
  - Example: `0` is falsy, but its type is `"number"`, not `"boolean"`.
  - Example: `""` (empty string) is falsy, but its type is `"string"`, not `"boolean"`.
- **Falsy values** in JavaScript (exactly 8 of them): false, 0, -0, 0n, "", null, undefined, NaN
- **Truthy value examples:** `[]`, `{}`, `"0"` — even though these may *seem* empty or "zero-like," they are still truthy:
```js
  Boolean([]);  // true
  Boolean({});  // true
  Boolean("0"); // true
```
- `typeof booleanTypeVariable === "boolean"`.

----------------------------------------------------------------------- 