---
title: Functions
description: Functions
sidebar:
  order: 4
---

Like many other programming languages Ilex has functions.

## Declaration

Functions are declared with the `fn` keyword:
```ilex
fn foo() {
    fmt::println("foo!");
}
```

## Arguments

Functions can have arguments:
```ilex
fn say_hi(name: string) {
    fmt::println("Hi {}!", name);
}
```

If two or more consecutive arguments have the same type, the type can be omitted for all but the
last argument of that type:
```ilex
fn say_hi_2(name1, name2: string) {
    fmt::println("Hi {} and {}!", name1, name2);
}
```

### Named Arguments

Function argument names can be used:
```ilex
fn some_func(start, stop: int) { /- ... -/ }

// Valid
some_func(start: 0, stop: 100);
// Valid
some_func(stop: 100, start: 0);
// Invalid, positional arguments are not allowed after named arguments
some_func(start: 0, 100);
// Invalid, names must match the prototype definitions
some_func(the_start: 0, the_stop: 100);
```

## Returns

Functions can have a return type:
```ilex
fn add(a: int, b: int): int {
    return a + b;
}
```

Functions in ilex can only return one value. To return multiple values you can use a tuple:
```ilex
fn multiple_return(): .{int, string} {
    mut n: int;
    mut str: string;
    // Do stuff
    return .{n, str};
}
```

If a function returns something the type must be specified in the function definition.
A function can be explicitly marked as not having a return value. This is purely for the
programmers convenience, there is no difference to the compiler.
```ilex
fn yeet(): void {
    fmt::println("This is void!");
}
```

## Default Arguments

Functions can have default arguments:
```ilex
fn add(a := 10, b := 10): int {
    return a + b;
}
```

Default argument values must be compile-time constants. They cannot reference other parameters or
call runtime functions:
```ilex
fn foo(a := 10, b := a + 5) {} // Invalid, b references parameter a
fn bar(a := 10, b := f()) {}   // Invalid, b calls a runtime function
fn bar(a := 10, b := 15) {}    // Valid, both are compile-time constants
```

Here type inference was used for the argument types but they can be specified as well:
```ilex
fn add(a: int = 10, b: int = 10): int {
    return a + b;
}
```

## Immutability

Function arguments are immutable by default:
```ilex
fn some_func(x: int) {
    x = 42;  // Invalid
    y := &x; // Invalid (?)
}
```

If you need to change the argument for some reason you must either make a copy or use the `mut` keyword:
```ilex
fn some_func(x: int) {
    new_x := x;
    new_x = 42; // Valid
}
```

Explicitly mark x as mutable with the `mut` keyword:
```ilex
fn some_func(mut x: int) {
    x = 42; // Valid
    y := &x; // Valid
}
```

Pointers can also be used to change the value:
```ilex
fn some_func(x: ^int) {
    new_x := x;
    new_x^ = 42; // Should a copy be needed here?
}

mut x: ^int = new(int);
x^ = 10;
some_func(x); // x is now 42
```

## Variadic Functions

Functions can be variadic, meaning they can take a variable number of arguments.
The variadic argument can be treated like a static array:
```ilex
fn sum(nums: ..int): int {
    res := 0;
    for (const n in nums) {
        res += n;
    }

    return res;
}

sum();
sum(1);
sum(1, 2, 3);
sum(4, 5, 6, 7, 8);
```

Additional arguments can't come after a variadic argument:
```ilex
fn counts(counts_var: ..int, type: string) {} // Invalid
```

### Arity

If a variadic function requires a minimum number of arguments to work, that can be specified with
`#[min_arity]`:
```ilex
#[min_arity=2]
fn sum(nums: ..int): int {
    res := 0;
    for const n in nums {
        res += n;
    }

    return res;
}

sum(); // Invalid
sum(2, 2); // Valid
```

`#[max_arity]` can also be used to specify the maximum number of arguments a function can take.

Did you know that a function with 9 arguments is called Novenary? Well now you do!

## Function Overloading

Ilex has function overloading, but it works a little differently than in other languages.

Instead of declaring multiple functions with the same name and implicitly overloading them,
functions in Ilex are explicitly overloaded by creating multiple functions with different names
and using the `overloads` keyword:
```ilex
fn powi(x: int, y: int): int {}
fn powf(x: float, y: float): float {}
fn powd(x: double, y: double): double {}

fn pow overloads { powi, powf, powd }
```

This is done to be more explicit and searchable as well as making the compiler simpler to develop.

The original functions can still be called or the overloaded function can be called:
```ilex
a := 2.f;
b := 8.f;
x := 2;
y := 8;

z := powi(x, y);
w := pow(x, y); // Calls powi()
v := pow(a, b); // Calls powf()
u := powf(a, b);
```

The following will not compile because functions must have different names:
```ilex
fn pow(x: int, y: int): int {}
fn pow(x: float, y: float): float {}
fn pow(x: double, y: double): double {}
```

## Advanced Functions

For more advanced function usage see [Advanced Functions]().
