---
title: Control Flow
description: Control Flow
sidebar:
  order: 3
---

## if

If statements work much like they do in other languages:

```neo
if x % 2 == 0 {
    fmt::println("x is even!");
}
```

Note that if statements do not require `(` `)`, but they can be used.

```neo
if (x % 2 != 0) {
    fmt::println("x is odd!");
}
```

If statements require curly braces unless it is a single line if statement. Single line if statements must have a `:` after the condition:
```neo
if x == y: continue;
```

If statements can have multiple branches:
```neo
if x > y {
    fmt::println("x > y");
} else if x < y {
    fmt::println("x < y");
} else {
    fmt::println("x == y");
}
```

The `elif` keyword can also be used:

```neo
if str == "foo" {
    fmt::println("foo!");
} elif str == "bar" {
    fmt::println("bar!");
} else {
    fmt::println("something else!");
}
```

## for

For loops work much like other languages:
```neo
for i := 0; i < 10; i++ {
    fmt::println(i);
}
```

Parentesis are optional with for loops:
```neo
for (i := 0; i < 10; i++) {
    fmt::println(i);
}
```

Range based for loops can also be used:
```neo
// Exclusive
for const i in 0 ..< 10 {
    fmt::println(i);
}

// Inclusive
for const i in 0 ..= 10 {
    fmt::println(i);
}
```

An index is not needed:
```neo
for 0 ..< 10 {
    fmt::println("foo");
}
```

The step can be set:
```neo
for 0 ..< 10; 2 {
    fmt::println("step of 2");
}
```

For loops can be used on arrays, strings, and maps:
```neo
for const c in str {}
for const c, i in str {}
for const str in strings {}
for const str, index in strings {}
for mut str in strings {}
for const key, value in map {}
for const key, value, index in map {}
for const _, value in map {}
for const key, _ in map {}
for const key, mut value in map {}
```

For loops can be single line with no braces too:
```neo
for const i in 0 ..< 10: fmt::println(i);
```

Use `<-` after for to iterate in reverse. The range stays the same, `<-` reverses the direction.
```neo
for <- const i in 0 ..< 10 {} // 9, 8, 7, ..., 0
for <- const i in 0 ..= 10 {} // 10, 9, 8, ..., 0
for <- const i in 0 ..< 10; 2 {} // 8, 6, 4, 2, 0
```

Forward iteration over a reversed range does nothing (warning):
```neo
for const i in 10 ..< 0 {} // Compiles, but does nothing, generates warning
```

To unroll a loop use `#[unroll]`:
```neo
#[unroll] for 0 ..< 10 {}
```

## while

While loops work much like they do in other languages:
```neo
while condition == true {
    // Do something
}
```

### do while

For a do while loop use `#[at_least_once]`:
```neo
#[at_least_once]
while condition == true {
    // Code ran at least once
}
```

## until

Until loops are the opposite of while loops:
```neo
until condition == true {
    // Do something
}
```

Another example:
```neo
until window_should_close(&window) {
    // Game loop
}

// Is the same as
while !window_should_close(&window) {
    // Game loop
}
```

### do until

Until loops can also use `#[at_least_once]`:
```neo
#[at_least_once]
until condition == true {
    // Code ran at least once
}
```

## break and continue

### break

`break` will exit the current iteration of the loop and will not continue:
```neo
found := false;
for const v in some_array {
    if v == target {
        found = true;
        break; // Will exit this iteration and will not continue the loop.
    }
}
```

### continue

`continue` will exit the current iteration of the loop but continue:
```neo
for i := 0; i < 10; ++i {
    if i % 2 == 0 {
        fmt::println("even");
        continue; // Will exit this iteration and continue the loop.
    }

    fmt::print("odd");
}
```

## when
When statements are much like switch statements in c/c++ except they only need one dedicated keyword.
Parentheses are optional for `when` as well:
```neo
x ::= 2;
when x {
    1: fmt::println("one");
    2: fmt::println("two");
    3: fmt::println("three");
    else: fmt::println("???");
}
```

Multiple cases can be handled at once:
```neo
when x {
    1, 3, 5, 7, 9: fmt::println("odd");
    2, 4, 6, 8, 10: fmt::println("even");
    else: fmt::println("???");
}
```

Ranges can also be used:
```neo
when x {
    in 0 ..< 10: fmt::println("0 ..< 10");
    in 10 ..= 100: fmt::println("10 ..= 100");
    else: fmt::println("big");
}
```

By default cases do not fall through. To fall through use `fallthrough`:
```neo
x := 2;
when x {
    1: fmt::println("one"); fallthrough;
    2: fmt::println("two"); fallthrough;
    3: fmt::println("three");
    else: fmt::println("???");
}

// The above when statement will trigger 2 and 3.
```

When is not just limited to numbers:
```neo
str := "one";
when str {
    "one": fmt::println("1");
    "two", "three": fmt::println("2 or 3");
    else: fmt::println("???");
}

strs0 ::= [?]string{"one", "two", "three"};
strs1 ::= [?]string{"four", "five", "six"};
str := "two";

when str {
    in strs0: fmt::println("1, 2, or 3");
    in strs1: fmt::println("4, 5, or 6");
    else: fmt::println("???");
}
```

`when` can be used with types as well:
```neo
typedef Value as double | string | bool;
mut v: Value;

when type_of(v) {
    double: fmt::println("v is a double");
    string: fmt::println("v is a string");
    bool: fmt::println("v is a bool");
}
```

Functions can't be used as a pattern:
```neo
when x {
    is_even(x): fmt::println("even!"); // Invalid
}
```

Each branch in a `when` statement has its own scope. Curly braces are required if the branch has more than one statement:
```neo
str := "one";
when str {
    "one": {
        x := 1;
        fmt::println(x);
    }
    "two": {
        x := 2;
        fmt::println(x);
    }
    else: fmt::println("???");
}
```

`when` can also be used with enums, struct, arrays, and maps. This usage will be explained in their respective sections

## unreachable

Sometimes the programmer knows that a branch of code will never be executed. The compiler can be told this with the `unreachable` keyword:
```neo
fn foo(x: int) {
    m ::= x % 3;

    when m {
        0: fmt::println("Foo");
        1: fmt::println("Bar");
        2: fmt::println("Baz");
        else: unreachable;
    }
}

// also
re ::= regex::compile("valid-regex-here") else unreachable;
```

If unreachable is executed the program will panic in debug mode. In release mode the compiler
assumes the code will never be executed to perform optimizations. Hitting unreachable in
release mode is undefined behavior.

## ternary operator

Ternary operators work much like they do in c/c++ except they can't be nested:
```neo
value_if_true ::= 12;
value_if_false ::= 42;
condition := true;
x := condition ? value_if_true : value_if_false;
assert(x == 12); // true

condition = false;
x = condition ? value_if_true : value_if_false;
assert(x == 42); // true
```

## labels

### break and continue

Labels can be used with `break` and `continue`. For instance if you have and inner and
outer loop you can use labels to specify which loop to `break`/`continue` from. Otherwise
Neo will `break`/`continue` from the loop the statement is in which may not be what you want.
```neo
outer:
for i := 0; i < 100; ++i {
    inner:
    for j := 100; j >= 0; --j {
        if some_condition {
            continue inner;
        }

        if other_condition {
            break outer;
        }

        // code
    }

    // code
}
```

### goto

Labels can also be used with `goto`:
```neo
forever:
fmt::println("This prints forever!");
goto forever;
```

`goto` can't jump to labels in other functions.

See [Advanced Goto]() for more information on `goto` and labels.
