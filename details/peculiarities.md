---
description: Special features that exist for the sole reason of why not.
---

# Peculiarities

## Annotations

Decorators work more like actual compiler annotations in AssemblyScript.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Annotation</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">@inline</td>
      <td style="text-align:left">Requests inlining of a constant or function.</td>
    </tr>
    <tr>
      <td style="text-align:left">@lazy</td>
      <td style="text-align:left">Requests lazy compilation of a variable. Useful to avoid unnecessary globals.</td>
    </tr>
    <tr>
      <td style="text-align:left">@global</td>
      <td style="text-align:left">Registers an element to be part of the global scope.</td>
    </tr>
    <tr>
      <td style="text-align:left">@external</td>
      <td style="text-align:left">Changes the external name of an imported element. <code>@external(module, name)</code> changes
        both the module and element name, <code>@external(name)</code> changes the
        element name only.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>@operator,</p>
        <p>@operator.binary</p>
      </td>
      <td style="text-align:left">Annotates a method as a binary operator overload. See below.</td>
    </tr>
    <tr>
      <td style="text-align:left">@operator.prefix</td>
      <td style="text-align:left">Annotates a method as a unary prefix operator overload. See below.</td>
    </tr>
    <tr>
      <td style="text-align:left">@operator.postfix</td>
      <td style="text-align:left">Annotates a method as a unary postfix operator overload. See below.</td>
    </tr>
  </tbody>
</table>Custom decorators can be given meaning by using a [transform](transforms.md).

## Operator overloads

Operator overloads can only be used on class methods. The respective argument types must match the class type.

### Binary operations

```typescript
@operator(OP)
static __op(left: T, right :T): T { ... }

@operator(OP)
__op(right: T): T  { ... }
```

| OP | Description |
| :--- | :--- |
| `"[]"` | Checked indexed get |
| `"[]="` | Checked indexed set |
| `"{}"` | Unchecked indexed get |
| `"{}="` | Unchecked indexed set |
| `"=="` | Equality |
| `"!="` | Inequality |
| `">"` | Greater than |
| `">="` | Greater than or equal |
| `"<"` | Less than |
| `"<="` | Less than or equal |
| `">>"` | Arithmetic right shift |
| `">>>"` | Logical right shift |
| `"<<"` | Left shift |
| `"&"` | Bitwise AND |
| `"|"` | Bitwise OR |
| `"^"` | Bitwise XOR |
| `"+"` | Addition |
| `"-"` | Subtraction |
| `"*"` | Multiplication |
| `"/"` | Division |
| `"**"` | Exponentiation |
| `"%"` | Remainder |

The `===` operation cannot be overloaded \(is identity equality\).

### Unary prefix operations

```typescript
@operator.prefix(OP)
static __op(self: T): T { ... }

@operator.prefix(OP)
__op(): T { ... }
```

| OP | Description | Notes |
| :--- | :--- | :--- |
| `"!"` | Logical NOT |  |
| `"~"` | Bitwise NOT |  |
| `"+"` | Unary plus |  |
| `"-"` | Unary negation |  |
| `"++"` | Prefix increment | Instance overload reassigns |
| `"--"` | Prefix decrement | Instance overload reassigns |

{% hint style="info" %}
Note that increment and decrement overloads can have slightly different semantics. If the overload is declared as an instance method, on `++a` the compiler does emit code that reassigns the resulting value to `a` while if the overload is declared static, the overload behaves like any other overload, skipping the otherwise implicit assignment.
{% endhint %}

### Unary postfix operations

```typescript
@operator.postfix(OP)
static __op(self: T): T { ... }

@operator.postfix(OP)
__op(): T { ... }
```

| OP | Description | Notes |
| :--- | :--- | :--- |
| `"++"` | Postfix increment | Instance overload reassigns |
| `"--"` | Postfix decrement | Instance overload reassigns |

{% hint style="info" %}
Overloaded postfix operations do not preserve the original value automatically.
{% endhint %}

## Range limits

The following range limits are present as global constants for convenience:

| Constant | Value |
| :--- | :--- |
| i8.**MIN\_VALUE**: `i8` | -128 |
| i8.**MAX\_VALUE**: `i8` | 127 |
| i16.**MIN\_VALUE**: `i16` | -32768 |
| i16.**MAX\_VALUE**: `i16` | 32767 |
| i32.**MIN\_VALUE**: `i32` | -2147483648 |
| i32.**MAX\_VALUE**: `i32` | 2147483647 |
| i64.**MIN\_VALUE**: `i64` | -9223372036854775808 |
| i64.**MAX\_VALUE**: `i64` | 9223372036854775807 |
| isize.**MIN\_VALUE**: `isize` | target specific: either `i32.MIN_VALUE` or `i64.MIN_VALUE` |
| isize.**MAX\_VALUE**: `isize` | target specific: either `i32.MAX_VALUE` or `i64.MAX_VALUE` |
| u8.**MIN\_VALUE**: `u8` | 0 |
| u8.**MAX\_VALUE**: `u8` | 255 |
| u16.**MIN\_VALUE**: `u16` | 0 |
| u16.**MAX\_VALUE**: `u16` | 65535 |
| u32.**MIN\_VALUE**: `u32` | 0 |
| u32.**MAX\_VALUE**: `u32` | 4294967295 |
| u64.**MIN\_VALUE**: `u64` | 0 |
| u64.**MAX\_VALUE**: `u64` | 18446744073709551615 |
| usize.**MIN\_VALUE**: `usize` | 0 |
| usize.**MAX\_VALUE**: `usize` | target specific: either `u32.MAX_VALUE` or `u64.MAX_VALUE` |
| bool.**MIN\_VALUE**: `bool` | 0 |
| bool.**MAX\_VALUE**: `bool` | 1 |
| f32.**MIN\_VALUE**: `f32` | -3.40282347e+38 |
| f32.**MAX\_VALUE**: `f32` | 3.40282347e+38 |
| f32.**MIN\_SAFE\_INTEGER**: `f32` | -16777215 |
| f32.**MAX\_SAFE\_INTEGER**: `f32` | 16777215 |
| f32.**EPSILON**: `f32` | 1.19209290e-07 |
| f64.**MIN\_VALUE**: `f64` | -1.7976931348623157e+308 |
| f64.**MAX\_VALUE**: `f64` | 1.7976931348623157e+308 |
| f64.**MIN\_SAFE\_INTEGER**: `f64` | -9007199254740991 |
| f64.**MAX\_SAFE\_INTEGER**: `f64` | 9007199254740991 |
| f64.**EPSILON**: `f64` | 2.2204460492503131e-16 |

## Tree-shaking

### Module-level

The compiler only compiles elements reachable from the entry file and ignores everything that remains unused. This is very similar to a JavaScript VM's behavior when running a JavaScript file, but unlike in TypeScript it also affects checking of types. Helps to reduce compilation times and binary sizes significantly.

### Branch-level

In addition to module-level tree-shaking, the compiler ignores branches that it can prove won't be taken. Works with constants, built-ins that compile to a constant, expressions that can be precomputed to a constant, plus the following globals to detect specific compiler flags or features:

| Constant | Description |
| :--- | :--- |
| **ASC\_TARGET** | 0 = WASM32, 1 = WASM64, 2 = JS \(portable\) |
| **ASC\_NO\_ASSERT** | Whether `--noAssert` is being set |
| **ASC\_MEMORY\_BASE** | The value of `--memoryBase` |
| **ASC\_OPTIMIZE\_LEVEL** | The value of `--optimizeLevel` \(0-3\) |
| **ASC\_SHRINK\_LEVEL** | The value of `--shrinkLevel` \(0-2\) |
| **ASC\_FEATURE\_SIGN\_EXTENSION** | Whether sign extension operations are enabled |
| **ASC\_FEATURE\_MUTABLE\_GLOBALS** | Whether mutable globals are enabled |
| **ASC\_FEATURE\_NONTRAPPING\_F2I** | Whether non-trapping float-to-int operations are enabled |
| **ASC\_FEATURE\_BULK\_MEMORY** | Whether bulk memory operations are enabled |
| **ASC\_FEATURE\_SIMD** | Whether SIMD operations are enabled |
| **ASC\_FEATURE\_THREADS** | Whether threads are enabled |
| **ASC\_FEATURE\_EXCEPTION\_HANDLING** | Whether exception handling is enabled |
| **ASC\_FEATURE\_TAIL\_CALLS** | Whether tail-calls are enabled |
| **ASC\_FEATURE\_REFERENCE\_TYPES** | Whether reference types are enabled |

For example, if a library supports SIMD but also provides a fallback:

```typescript
if (ASC_FEATURE_SIMD) {
  // compute with SIMD operations
} else {
  // fallback without SIMD operations
}
```

