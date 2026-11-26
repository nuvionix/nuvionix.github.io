---
title: "Introduction to Exception Handling in C++"
date: 2026-11-26
draft: false
description: "Understand try, throw, catch, standard exceptions, custom exceptions, and RAII."
categories: ["C++"]
tags: ["C++"]
readingTime: true
---

## What is an exception?

An exception represents an unusual condition that interrupts the normal flow of a program. In C++, code can signal such a condition with `throw`. The runtime searches for a matching handler in the active call stack. If no handler is found, the program terminates.

Exceptions are useful when a function cannot produce its normal result and the caller needs a clear way to handle that failure. They should not be used as a substitute for ordinary decisions in program logic.

## The try, throw, and catch model

- `try` surrounds code that may fail.
- `throw` reports the exceptional condition.
- `catch` handles an exception of a compatible type.

```cpp
#include <iostream>
#include <stdexcept>

int divide(int numerator, int denominator) {
    if (denominator == 0) {
        throw std::runtime_error("denominator cannot be zero");
    }
    return numerator / denominator;
}

int main() {
    try {
        std::cout << divide(12, 3) << '\n';
        std::cout << divide(12, 0) << '\n';
    } catch (const std::runtime_error& error) {
        std::cerr << "Error: " << error.what() << '\n';
    }
}
```

When the second call throws, execution leaves the `try` block and enters the matching handler. Statements after the throwing call inside that block are skipped.

## Multiple handlers and standard exceptions

A `try` block may have several handlers. The first compatible handler is selected, so place more specific exception types before their base classes. A final `catch (...)` can handle otherwise unmatched exception types, but it does not provide a typed exception object or its message.

The standard library includes useful exception classes such as `std::invalid_argument`, `std::out_of_range`, and `std::runtime_error`. Many derive from `std::exception`, whose `what()` member provides a diagnostic message.

```cpp
#include <iostream>
#include <string>

int main() {
    try {
        int value = std::stoi("not-a-number");
        std::cout << value << '\n';
    } catch (const std::invalid_argument& error) {
        std::cerr << "Input was not numeric: " << error.what() << '\n';
    } catch (const std::out_of_range& error) {
        std::cerr << "Value was outside the supported range: "
                  << error.what() << '\n';
    }
}
```

Catch standard exception objects by `const` reference. This avoids unnecessary copies and preserves the dynamic exception type.

## Custom exception types

An application can define a more meaningful exception by deriving from a standard exception class. For example, a banking application might use an `InsufficientFunds` exception to communicate a failed withdrawal. Keep custom types focused and include enough context to diagnose the problem without exposing sensitive data.

## Rethrowing an exception

A handler may log or add context and then pass the same exception to a higher-level caller with `throw;`. This is different from `throw error;`, which throws a new copy and may lose derived-type information through slicing.

## Stack unwinding and RAII

As an exception moves up the call stack, C++ destroys local automatic objects whose scopes are exited. This process is called **stack unwinding**. RAII (Resource Acquisition Is Initialization) uses object lifetime to manage resources: acquire a resource in an object's construction and release it in its destructor.

Prefer standard resource-managing types such as `std::vector`, `std::string`, `std::unique_ptr`, and file-stream objects instead of manually pairing resource acquisition and cleanup. This makes cleanup more reliable when exceptions occur.

## Good practices

- Throw exceptions for exceptional failures, not routine branching.
- Catch by `const` reference when handling typed exceptions.
- Catch specific types before general base types.
- Do not silently swallow errors; report or handle them meaningfully.
- Use RAII to manage resources and keep destructors non-throwing.
- Add context at useful boundaries, then rethrow when a higher layer must decide what to do.

A clear exception strategy makes C++ programs easier to maintain because error paths are explicit and resource cleanup is tied to object lifetime.
