---
title: Introduction
description: An introduction to the Neo programming language.
---

Neo is a systems programming language designed around the principle of **explicit over implicit**. It gives you full control over memory, clear syntax, and powerful compile-time features — without hiding complexity behind abstractions.

## Design Principles

- **Explicit over implicit** — no hidden allocations, no implicit conversions
- **Manual memory management** — with `defer` for cleanup, no garbage collector
- **No OOP** — no classes, no inheritance, no constructors/destructors
- **Compile-time power** — first-class `Code` type, macros, and compile-time execution
- **Clear syntax** — semicolons, braces, and keywords that say what they mean

## What Neo Looks Like

```neo
namespace main;

#import using fmt;

fn main() {
    println("Hello, world!");
}
```

## Next Steps

Head to [Installation](/getting-started/installation/) to set up Neo, or jump straight to [Hello World](/getting-started/hello-world/) to see it in action.
