---
title: Defer
description: Defer
sidebar:
  order: 5
---

The `defer` keyword defers the execution of code until the end of the current scope:
```ilex
fn some_func() string {
    some_file := fs::open("my_file.txt");
    defer fs::close(&some_file);
    contents := fs::read_all(&some_file) else "";
    
    return contents;
}
```
The file is closed just before the function returns.

## Defer Blocks

Blocks of code can be deferred as well:
```ilex
fn another_func() {
    defer {
        call1();
        if condition {
            call2();
        }
    }
    // More code here
}
```

## Multiple Defer

Defer statements can be stacked as well and are executed in a First In Last Out (FILO) fashion:
```ilex
fn defer_tst() {
    defer fmt::println("1");
    defer fmt::println("2");
    defer fmt::println("3");
}

// Prints:
// 3
// 2
// 1
```

`defer` is scoped to its enclosing block. Inside a loop body, each iteration is
a separate block scope, so defers execute at the end of each iteration (not at
function exit). This means `N` iterations produce `N` defer executions:
```ilex
#import fmt;

fn main() {
    defer fmt::println("Bye");
    for (const i in 0 ..< 5) {
        defer fmt::println(i);
    }
    fmt::println("Hello, World!");
}

/- Will print the following:
0
1
2
3
4
Hello world
Bye
-/
```

## Defer on Error

`defer_err` can be used to defer code only if the function returns an error:
```ilex
fn setup(): Connection! {
    db := try connect();
    defer_err close(&db);  // Runs if anything below fails

    cache := try init_cache();
    defer_err clear(&cache);

    return Connection{db, cache};
}
```

See the [Error handling]() section for an explanation on how error handling in Ilex works.
