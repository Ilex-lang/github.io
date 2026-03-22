---
title: Hello World
description: Write your first Neo program.
---

Let's write a simple "Hello, World!" program in Neo.

## Your First Program

Create a file called `hello.neo`:

```neo
namespace main;

#import fmt;

fn main() {
    fmt::println("Hello, World!");
}
```

## Compile and Run

```bash
neo hello.neo
./neo-out/hello
```

You should see:

```
Hello, World!
```

## What's Next?

Now that you've got the basics, explore the [Guide](/guide/01_variables/) to learn about variables, types, and more.
