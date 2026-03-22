---
title: Attributes
description: Complete reference of attributes in Neo.
sidebar:
  order: 4
---

Attributes are written as `#[name]` or `#[name=value]` and modify the behavior of declarations.

## Function Attributes

| Attribute | Description |
|-----------|-------------|
| `#[inline]` | Force inline |
| `#[no_inline]` | Prevent inlining |
| `#[maybe_inline]` | Hint to inline |
| `#[no_discard]` | Caller must use the return value |
| `#[no_return]` | Function never returns |
| `#[no_context]` | No implicit context parameter |
| `#[deprecated]` | Warn on usage |
| `#[warning]` | Custom warning on usage |
| `#[maybe_unused]` | Suppress unused warnings |
| `#[static]` | Static function (file-scoped linkage) |
| `#[asm_return]` | Return via inline assembly |
| `#[link_name="..."]` | Override linker symbol name |
| `#[calling_convention="..."]` | Set calling convention |

### Contract Attributes

| Attribute | Description |
|-----------|-------------|
| `#[require { expr }]` | Precondition (checked on entry) |
| `#[ensure { expr }]` | Postcondition (checked on return) |
| `#[in="param"]` | Document pointer as input-only |
| `#[out="param"]` | Document pointer as output-only |
| `#[inout="param"]` | Document pointer as input/output |

### Arity Attributes

| Attribute | Description |
|-----------|-------------|
| `#[min_arity=N]` | Minimum argument count for variadic |
| `#[max_arity=N]` | Maximum argument count for variadic |

## Struct Attributes

| Attribute | Description |
|-----------|-------------|
| `#[packed]` | No padding between fields |
| `#[align(N)]` | Custom alignment |
| `#[repr="c"]` | C-compatible memory layout |
| `#[require { expr }]` | Precondition on construction |

### Field Attributes

| Attribute | Description |
|-----------|-------------|
| `#[required]` | Field must be explicitly set |
| `#[json="name"]` | Override JSON key name |
| `#[json_rename_all="camelCase"]` | Rename all fields for JSON |
| `#[omitempty]` | Omit zero-value fields in JSON |

## Type Attributes

| Attribute | Description |
|-----------|-------------|
| `#[distinct]` | Make typedef a separate type |
| `#[init_to=value]` | Custom default for distinct type |

## Variable Attributes

| Attribute | Description |
|-----------|-------------|
| `#[static]` | Static variable (persists across calls) |
| `#[thread_static]` | Thread-local static variable |
| `#[shadow]` | Allow shadowing an outer variable |

## Loop Attributes

| Attribute | Description |
|-----------|-------------|
| `#[unroll]` | Unroll loop |
| `#[at_least_once]` | Execute loop body before checking condition |

## Branch Attributes

| Attribute | Description |
|-----------|-------------|
| `#[unpredictable]` | Hint that branch is unpredictable |
| `#[complete]` | Require exhaustive `when` matching |

## Memory Attributes

| Attribute | Description |
|-----------|-------------|
| `#[no_bounds_check]` | Disable bounds checking in block |
| `#[safe_alloc]` | Enable runtime use-after-free detection |
| `#[soa]` | Struct-of-Arrays layout |
| `#[soa, tile=N]` | AOSOA tiled layout |
| `#[soa, align=N]` | Aligned SOA |
| `#[soa, pad_to=N]` | Padded SOA |
| `#[soa_group="name"]` | Group fields for partial SOA |

## Linking Attributes

| Attribute | Description |
|-----------|-------------|
| `#[link="name"]` | Link against library |
| `#[link_type="static"]` | Static linking |
| `#[link_framework="name"]` | Link macOS framework |
| `#[include_path="path"]` | Add include search path |
| `#[exclude="pattern"]` | Exclude symbols from C import |

## Platform Attributes

| Attribute | Description |
|-----------|-------------|
| `#[console]` | Force console window (Windows) |
| `#[debug_console]` | Console in debug builds only (Windows) |
| `#[stable]` | Require explicit error codes |
