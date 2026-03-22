---
title: Types
description: Complete reference of all built-in types in Neo.
sidebar:
  order: 1
---

## Integer Types

| Type | Alias | Size | Range |
|------|-------|------|-------|
| `s8` | | 8-bit | -128 to 127 |
| `s16` | | 16-bit | -32,768 to 32,767 |
| `s32` | `int` | 32-bit | -2^31 to 2^31 - 1 |
| `s64` | | 64-bit | -2^63 to 2^63 - 1 |
| `s128` | | 128-bit | -2^127 to 2^127 - 1 |
| `u8` | `byte` | 8-bit | 0 to 255 |
| `u16` | | 16-bit | 0 to 65,535 |
| `u32` | `uint` | 32-bit | 0 to 2^32 - 1 |
| `u64` | | 64-bit | 0 to 2^64 - 1 |
| `u128` | | 128-bit | 0 to 2^128 - 1 |

## Floating Point Types

| Type | Alias | Size | Description |
|------|-------|------|-------------|
| `f16` | | 16-bit | Half precision |
| `f32` | `float` | 32-bit | Single precision |
| `f64` | `double` | 64-bit | Double precision |
| `f128` | | 128-bit | Quad precision |

## Boolean Types

| Type | Alias | Size |
|------|-------|------|
| `b1` | `bool` | 1-bit |
| `b8` | `bool` | 8-bit |
| `b16` | | 16-bit |
| `b32` | | 32-bit |
| `b64` | | 64-bit |

`bool` will either be 1 bit or 1 byte depending on context.

## Text Types

| Type | Description |
|------|-------------|
| `char` | Unicode codepoint (u32 width) |
| `string` | UTF-8 encoded string |
| `cstring` | Null-terminated C string |

## Pointer Types

| Syntax | Description |
|--------|-------------|
| `^T` | Pointer to `T` |
| `^T?` | Optional pointer (must unwrap before use) |
| `[^]T` | Multi-value pointer (pointer arithmetic) |
| `[^:sentinel]T` | Sentinel-terminated pointer |
| `rawptr` | Untyped pointer (like `void*` in c) |
| `intptr` | Pointer-sized signed integer |
| `uintptr` | Pointer-sized unsigned integer |

## Array and Collection Types

| Syntax | Description |
|--------|-------------|
| `[N]T` | Static array of `N` elements |
| `[?]T{...}` | Static array with inferred length |
| `[]T` | Dynamic array |
| `[..]T` | Slice (non-owning view) |
| `[K: V]` | Hash map |
| `#ordered[K: V]` | Ordered map |

## Composite Types

| Syntax | Description |
|--------|-------------|
| `struct { ... }` | Struct type |
| `enum { ... }` | Enumeration |
| `union { ... }` | Untagged union |
| `T1 \| T2 \| T3` | Tagged union |
| `.{T1, T2, ...}` | Tuple |

## Special Types

| Type | Description |
|------|-------------|
| `typeid` | Runtime type identifier |
| `unknown` | Universal type (like `any`) |
| `simd<N, T>` | SIMD vector of `N` elements of type `T` |
| `Code` | Compile-time AST node |
| `void` | No value |

## Optional and Result Types

| Syntax | Description |
|--------|-------------|
| `T?` | Optional: `T` or null |
| `T!` | Result: `T` or error |
| `void!` | No value or error |

## Number Literal Formats

| Format | Example | Description |
|--------|---------|-------------|
| Decimal | `1_000_000` | Underscores allowed |
| Hex | `0xFF` or `0xff` | Prefix `0x` |
| Octal | `0q77` | Prefix `0q` |
| Binary | `0b1010` | Prefix `0b` |

## Zero Values

Every type has a default zero value:

| Type | Zero Value |
|------|-----------|
| Integer types | `0` |
| Float types | `0.0` |
| `bool` | `false` |
| `char` | `\0` |
| `string` | `""` |
| `cstring` | `null` |
| Pointer types | `null` |
| Optional types | `null` |
| Struct types | All fields zero-initialized |
| Array types | All elements zero-initialized |
