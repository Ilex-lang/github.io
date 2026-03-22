---
title: Operators
description: Complete reference of all operators in Neo.
sidebar:
  order: 3
---

## Arithmetic Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `+` | Addition | `a + b` |
| `-` | Subtraction | `a - b` |
| `*` | Multiplication | `a * b` |
| `/` | Division | `a / b` |
| `%` | Modulo | `a % b` |
| `++` | Increment | `++x` |
| `--` | Decrement | `--x` |

## Wrapping Arithmetic

These operators explicitly wrap on overflow (same as default behavior, but makes intent clear):

| Operator | Description |
|----------|-------------|
| `%+` | Wrapping add |
| `%-` | Wrapping subtract |
| `%*` | Wrapping multiply |
| `%/` | Wrapping divide |
| `%<<` | Wrapping left shift |
| `%+=` `%-=` `%*=` `%/=` `%<<=` | Wrapping compound assignment |
| `%++` `%--` | Wrapping increment/decrement |

## Saturating Arithmetic

These operators clamp at the type's min/max instead of wrapping on overflow:

| Operator | Description |
|----------|-------------|
| `@+` | Saturating add |
| `@-` | Saturating subtract |
| `@*` | Saturating multiply |
| `@/` | Saturating divide |
| `@<<` | Saturating left shift |
| `@+=` `@-=` `@*=` `@/=` `@<<=` | Saturating compound assignment |
| `@++` `@--` | Saturating increment/decrement |

## Comparison Operators

| Operator | Description |
|----------|-------------|
| `==` | Equal |
| `!=` | Not equal |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

## Logical Operators

| Operator | Description |
|----------|-------------|
| `&&` | Logical AND |
| `\|\|` | Logical OR |
| `!` | Logical NOT |

## Bitwise Operators

| Operator | Description |
|----------|-------------|
| `&` | Bitwise AND |
| `\|` | Bitwise OR |
| `~` | Bitwise NOT |
| `^` | Bitwise XOR |
| `<<` | Left shift |
| `>>` | Right shift |

## Assignment Operators

| Operator | Description |
|----------|-------------|
| `=` | Assign |
| `:=` | Declare mutable and assign |
| `::=` | Declare immutable and assign |
| `+=` `-=` `*=` `/=` `%=` | Compound arithmetic assignment |
| `&=` `\|=` `^=` `<<=` `>>=` | Compound bitwise assignment |
| `??=` | Assign if null |

## Pointer Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&` | Address-of | `&x` |
| `^` | Dereference (postfix on type or expr) | `p^` |
| `?.` | Optional chaining | `ptr?.field` |
| `?^` | Optional dereference | `ptr?^` |

## Range Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `..=` | Inclusive range | `0 ..= 10` |
| `..<` | Exclusive range | `0 ..< 10` |
| `..>` | Reverse range | `10 ..> 0` |

## Other Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `?` `:` | Ternary conditional | `cond ? a : b` |
| `??` | Null coalescing | `val ?? default` |
| `in` | Membership test | `x in range` |
| `as` | Type assertion | `v as string` |
| `...` | Struct embed / spread | `...Base` / `Point{...p, x: 1}` |
| `..` | Variadic parameter | `args: ..int` |
| `$` | Generic type parameter | `$T` |
| `@` | Caller scope access (macros) | `@x` |
| `#` | Compiler directive prefix | `#run`, `#if` |
