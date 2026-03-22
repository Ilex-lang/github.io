---
title: Keywords
description: Complete list of reserved keywords in Neo.
sidebar:
  order: 2
---

## Declaration Keywords

| Keyword | Description |
|---------|-------------|
| `fn` | Function declaration |
| `mut` | Mutable variable declaration |
| `const` | Immutable variable declaration |
| `typedef` | Type alias or type definition |
| `struct` | Struct type definition |
| `enum` | Enumeration type definition |
| `union` | Untagged union type definition |
| `error` | Error type definition |
| `namespace` | Namespace declaration |
| `extern` | External (C) declaration block |
| `test` | Test block declaration |
| `operator` | Operator overload declaration |
| `overloads` | Overload set declaration |

## Control Flow Keywords

| Keyword | Description |
|---------|-------------|
| `if` | Conditional branch |
| `elif` | Else-if (alias for `else if`) |
| `else` | Default branch |
| `for` | For loop |
| `while` | While loop |
| `until` | Inverse while loop (runs until true) |
| `when` | Pattern matching (switch) |
| `break` | Exit loop |
| `continue` | Skip to next loop iteration |
| `return` | Return from function |
| `fallthrough` | Explicit fallthrough in `when` |
| `goto` | Unconditional jump |

## Memory & Scope Keywords

| Keyword | Description |
|---------|-------------|
| `defer` | Execute at scope exit |
| `defer_err` | Execute at scope exit only on error |
| `new` | Heap allocation |
| `free` | Heap deallocation |
| `make` | Create dynamic arrays, maps, strings |
| `delete` | Free dynamic collections |
| `undefined` | Explicit non-initialization |

## Error Handling Keywords

| Keyword | Description |
|---------|-------------|
| `try` | Propagate error to caller |
| `panic` | Unrecoverable error |
| `assert` | Runtime assertion |
| `unreachable` | Mark unreachable code |

## Expression Keywords

| Keyword | Description |
|---------|-------------|
| `yields` | Yield value from block expression |
| `in` | Membership test / iteration |
| `as` | Type assertion (tagged unions) |
| `cast` | Explicit type cast |
| `recast` | Reinterpret cast (bit-level) |
| `auto_cast` | Compiler-chosen cast |
| `using` | Bring symbols into scope |
| `restrict` | Promise no pointer aliasing |

## Type Keywords

| Keyword | Description |
|---------|-------------|
| `true` | Boolean true |
| `false` | Boolean false |
| `null` | Null pointer / empty optional |
| `skip` | Skip a test |

## Import Directives

| Directive | Description |
|-----------|-------------|
| `#import` | Import a namespace or C header |
| `from` | Selective import source |

## Compile-Time Directives

| Directive | Description |
|-----------|-------------|
| `#run` | Execute at compile time |
| `#if` | Conditional compilation |
| `#ifndef` | Conditional if not defined |
| `#define` | Define a compilation flag |
| `#undef` | Remove a compilation flag |
| `#set` | Set a compile-time constant |
| `#unset` | Remove a compile-time constant |
| `#insert` | Insert generated code |
| `#code` | Code literal (AST) |
| `#macro` | Macro function modifier |
| `#modify` | Generic type computation |
| `#error` | Emit compile error |
| `#warn` | Emit compile warning |
| `#info` | Emit compile info |
| `#todo` | Emit TODO message |
| `#assert` | Compile-time assertion |
| `#embed_bytes` | Embed file as bytes |
| `#embed_string` | Embed file as string |
| `#iota` | Block-scoped incrementing counter |
| `#counter` | Global incrementing counter |
| `#concat` | Compile-time string concatenation |
| `#stringify` | Convert expression to string |

## Source Location Directives

| Directive | Description |
|-----------|-------------|
| `#file` | Current file name |
| `#path` | Current file path |
| `#line` | Current line number |
| `#column` | Current column number |
| `#function` | Current function name |
| `#namespace` | Current namespace |
| `#location` | Current source location |
| `#caller_location` | Caller's source location |

## Build Information Directives

| Directive | Description |
|-----------|-------------|
| `#target_os` | Target operating system |
| `#target_arch` | Target architecture |
| `#target_endian` | Target endianness |
| `#target_pointer_width` | Pointer width in bits |
| `#build_mode` | `"debug"` or `"release"` |
| `#neo_version` | Compiler version string |
| `#build_timestamp` | Build timestamp |
| `#env("NAME")` | Environment variable value |
