---
title: Functions
description: Functions
sidebar:
  order: 4
---

### Declaration

Functions are declared with the `fn` keyword:
```ilex
fn foo() {
    fmt::println("foo!");
}
```

### Arguments

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

### Returns

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

### Default Arguments

Functions can have default arguments:
```ilex
fn add(a := 10, b := 10): int {
    return a + b;
}
```
