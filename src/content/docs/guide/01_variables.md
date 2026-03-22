---
title: Variables & Constants
description: Variable declaration, mutability, and constants in Neo.
sidebar:
  order: 1
---

Neo requires you to be explicit about mutability.

## Mutable Variables

Use `mut` to declare a variable you intend to change:

```neo
mut x: int = 10;
mut name: string = "Neo";
```

The shorthand `:=` also declares a mutable variable with type inference:

```neo
x := 10;
name := "Neo";
```

## Immutable Variables

Use `const` for values that shouldn't change after initialization:

```neo
const y = 20;
const pi: f64 = 3.14159;
```

The shorthand `::=` declares an immutable variable:

```neo
y ::= 20;
pi ::= 3.14159;
```

## Compile-Time Constants

When a `const` is assigned a literal or an expression that can be fully evaluated at compile time, it becomes a compile-time constant automatically:

```neo
const MAX_SIZE = 1024;        // compile-time constant
const GREETING = "hello";     // compile-time constant
const DOUBLED = MAX_SIZE * 2; // also compile-time
```

## Type Inference

Neo infers types when possible, but you can always be explicit:

```neo
x := 42;          // inferred as s32 (int)
y := 3.14;        // inferred as f64 (double)
mut z: u64 = 100; // explicit type
```

## Auto-Initialization

Variables are initialized to their zero values by default:

```neo
mut n: int;    // 0
mut b: bool;   // false
mut s: string; // ""
mut p: ^int;   // null
```

If you explicitly don't want initialization (for performance), use `undefined`:

```neo
mut buffer: [1024]u8 = undefined; // no initialization
```

Note that this is not like `undefined` in Typescript, you can't check for it:

```neo
if x == undefined {
    fmt::println("This code is invalid!");
}
```

## Discarding Values

Use `_` to discard a return value you don't need:

```neo
_ = some_function_with_return();
```

## Visibility

Variable naming controls visibility across namespaces:

- `count` — exported (public)
- `_count` — private to the namespace
- `count_` — private to the file
- `_count_` — private to the file (most private wins)
