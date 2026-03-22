---
title: Strings
description: Strings, formatting, String_Builder, and common string operations in Neo.
sidebar:
  order: 6
---

Strings are a built-in type. They are UTF-8 encoded and immutable. Internally a `string` is a pointer to null-terminated byte data and a length.

String literals point to read-only static data. Indexing into a string returns the byte at that position:

```neo
str := "Yeet";
mut c: u8 = str[0];
assert(c == 'Y');

str[0] = 'M'; // Compile error, strings are immutable
```

Bounds checking is enforced at compile time for literals and at runtime for all strings:
```neo
str := "Yeet";
if str[6] == 't' {} // Compile error, out of bounds
```
`#[no_bounds_check]` can be used to index a string without a bounds check.

## String Comparison

Strings are compared by value (byte by byte), not by pointer. Two strings with the same content are equal regardless of where they are stored:

```neo
a := "hello";
b := "hello";
assert(a == b); // True
```

If their lengths differ they are not equal, and the bytes are not compared.

## Concatenation

The `+` operator is **not** valid for strings, it would require hidden memory allocation. Use `strings::concat()` instead:

```neo
str := "Hello" + " world!";                 // Compile error
str := strings::concat("Hello", " world!"); // Valid
defer delete(str);
```
TODO: Should there be an operator for compile time string concatenation (maybe `##`)? Like `++` in Zig.

`strings::concat` accepts variadic arguments:
```neo
full := strings::concat("Hello", ", ", name, "!");
defer delete(full);
```

Functions that return new strings allocate via the context allocator. The caller owns the result and must free it with `delete()`.

## Deleting Strings

`delete()` frees the underlying data of a heap-allocated string. Calling `delete()` on a string literal is undefined behavior (debug mode will panic):

```neo
greeting := strings::concat("a", "b");
delete(greeting); // Valid, frees heap data

literal := "hello";
delete(literal); // Undefined behavior, literal points to static data
```

## Temp Allocator

Use the temp allocator to avoid manual cleanup for short-lived strings. Functions that allocate have an `_alloc` variant that takes an allocator as the first argument:
```neo
tmp := strings::concat_alloc(context->allocator_tmp, "debug: ", msg);
// No delete needed, temp allocator frees at frame boundary
```

## Format Strings

`fmt::println` supports two forms:
```neo
fmt::println(value);                  // Prints any value using its default format
fmt::println("x = {}, y = {}", x, y); // Format string with placeholders
```

Format strings must be string literals. Argument count mismatch is a compile error.

### Placeholders

TODO: This should probably be in a different file.

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `{}` | Default formatting | `fmt::println("{}", 42)` → `42` |
| `{:.N}` | N decimal places (floats) | `fmt::println("{:.2}", 3.14159)` → `3.14` |
| `{:x}` | Lowercase hex | `fmt::println("{:x}", 255)` → `ff` |
| `{:X}` | Uppercase hex | `fmt::println("{:X}", 255)` → `FF` |
| `{:q}` | Octal | `fmt::println("{:q}", 8)` → `10` |
| `{:b}` | Binary | `fmt::println("{:b}", 5)` → `101` |
| `{:e}` | Scientific notation | `fmt::println("{:e}", 1234.5)` → `1.2345e3` |

### Width and Alignment

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `{:<w}` | Left-align in w chars | `fmt::println("{:<10}", "hi")` → `"hi        "` |
| `{:>w}` | Right-align in w chars | `fmt::println("{:>10}", "hi")` → `"        hi"` |
| `{:^w}` | Center in w chars | `fmt::println("{:^10}", "hi")` → `"    hi    "` |
| `{:0w}` | Zero-pad to w chars | `fmt::println("{:04}", 42)` → `0042` |

Escape braces with `\{` and `\}`.

`fmt::format(...)` returns an allocated string instead of printing:
```neo
s := fmt::format("x={}, y={}", x, y);
defer delete(s);

// Use fmt::format_alloc for a specific allocator:
s := fmt::format_alloc(context->allocator_tmp, "x={}, y={}", x, y);
```

## String Operations

Functions that return new strings (all allocate via context allocator, caller must free):
```neo
upper := strings::to_upper("hello"); // "HELLO"
defer delete(upper);

lower := strings::to_lower("HELLO"); // "hello"
defer delete(lower);

trimmed := strings::trim("  hi  "); // "hi"
defer delete(trimmed);

sub := strings::substring("hello", 1, 3); // "el"
defer delete(sub);
```

Each of these has an `_alloc` variant that takes an allocator as the first argument:
```neo
upper := strings::to_upper_alloc(context->allocator_tmp, "hello");
// No delete needed, temp allocator handles it
```

Functions that return information (no allocation):
```neo
strings::len(s);               // Same as s.len
strings::contains(s, "sub");   // bool
strings::starts_with(s, "he"); // bool
strings::ends_with(s, "lo");   // bool
strings::index_of(s, "sub");   // int?
strings::count(s, "l");        // int
```

For working with Unicode codepoints:
```neo
strings::decode_char(s, byte_offset); // returns .{char, int} (char + bytes consumed)
strings::codepoint_count(s);          // number of codepoints (not bytes)
```

## String_Builder

For efficient in-place string construction and mutation, use `String_Builder`. It is a mutable, growable byte buffer:

```neo
mut sb := strings::new_builder();
defer delete(sb);

strings::append(&sb, "Hello");
strings::append(&sb, ", ");
strings::append(&sb, name);
strings::append(&sb, "!");

fmt::println(sb); // "Hello, Alice!"
```

You can pre-allocate capacity:
```neo
mut sb := strings::new_builder(256);
defer delete(sb);
```

Use `new_builder_alloc` or `from_alloc` to specify an allocator:
```neo
mut sb := strings::new_builder_alloc(context->allocator_tmp);
mut sb2 := strings::from_alloc(context->allocator_tmp, "hello");
```

### In-Place Mutation

`String_Builder` supports direct byte access and in-place operations:
```neo
mut sb := strings::from("hello world");
defer delete(sb);

sb[0] = 'H';                            // Direct byte mutation
strings::to_upper(&sb, 0, 1);           // Capitalize range in place
strings::replace(&sb, "world", "Neo"); // In-place replacement
strings::insert(&sb, 5, " beautiful");  // Insert at position
strings::remove_range(&sb, 0, 6);       // Remove byte range
```

### Extracting a String

```neo
// Copy, builder remains usable:
result := strings::build(sb);
defer delete(result);

// Move, builder is consumed, no copy:
result := strings::build_owned(&sb);
defer delete(result);

// _alloc variant for build:
result := strings::build_alloc(context->allocator_tmp, sb);
```

### Common Patterns

Build a string in a loop:
```neo
mut sb := strings::new_builder();
defer delete(sb);
for const item, i in items {
    if i > 0: strings::append(&sb, ", ");
    strings::append(&sb, item.name);
}
result := strings::build(sb);
defer delete(result);
```

Temp allocator for formatting:
```neo
fn log_message(level: string, msg: string) {
    formatted := strings::concat_alloc(context->allocator_tmp, "[", level, "] ", msg);
    logger::write(formatted);
    // No delete needed
}
```

## Internal Representation

```neo
// Internal representation:
typedef string struct {
    data: [^]u8;  // Pointer to UTF-8 byte data
    len: int;     // Length in bytes (not codepoints)
}
```

## C Interop

The read only `.data` field can be used for easy c interoperability.
```neo
str := "foobar";
len := clib::strlen(str.data);
assert(len == 6);
```

Alternatively convert between `string` and `cstring` explicitly:
```neo
cs := strings::to_cstring(my_string);
defer delete(cs);

s := strings::from_cstring(my_cstring);
defer delete(s);

// _alloc variants are available:
cs := strings::to_cstring_alloc(context->allocator_tmp, my_string);
s := strings::from_cstring_alloc(context->allocator_tmp, my_cstring);
```
