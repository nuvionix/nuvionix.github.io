---
title: "Introduction to OpenMP and Shared-Memory Parallelism"
date: 2026-11-26
draft: false
description: "Understand OpenMP, shared-memory parallel programming, compiler directives, and practical examples."
tags: ["OpenMP", "Parallel Programming", "C", "C++", "Multithreading"]
categories: ["Programming"]
---

## Introduction

OpenMP is an API used for shared-memory parallel programming in C, C++, and Fortran. It allows developers to execute suitable parts of a program using multiple threads.

In a traditional sequential program, instructions are generally executed one after another. Parallel programming divides suitable work into smaller tasks that can run simultaneously.

OpenMP makes this process easier through compiler directives, runtime functions, and environment variables.

## What Is Shared-Memory Parallelism?

In shared-memory systems, multiple processor cores can access the same main memory.

Each thread has its own execution flow, but threads within a process can access shared variables. This makes communication convenient, although it also requires careful synchronization.

## Main Features of OpenMP

- Supports parallel execution using threads.
- Uses compiler directives to identify parallel regions.
- Provides synchronization mechanisms.
- Supports loop parallelization.
- Works with C, C++, and Fortran.
- Allows control over the number of threads.

## OpenMP Programming Model

OpenMP commonly uses the fork-join model:

1. A program begins with one main thread.
2. The main thread reaches a parallel region.
3. A team of threads is created.
4. Threads execute assigned work.
5. Threads synchronize at the end of the parallel region.
6. Execution continues with the main thread.

## Basic OpenMP Example

```c
#include <stdio.h>
#include <omp.h>

int main() {
    #pragma omp parallel
    {
        int id = omp_get_thread_num();

        printf("Hello from thread %d\n", id);
    }

    return 0;
}

The parallel directive creates a team of threads. Each thread executes the code inside the parallel region.
Parallelizing a Loop
OpenMP can distribute loop iterations among multiple threads.

#include <stdio.h>
#include <omp.h>

int main() {
    int numbers[8];

    #pragma omp parallel for
    for (int i = 0; i < 8; i++) {
        numbers[i] = i * i;
    }

    for (int i = 0; i < 8; i++) {
        printf("%d ", numbers[i]);
    }

    return 0;
}

The parallel for directive distributes loop iterations across available threads.
Important OpenMP Directives
- parallel: Creates a parallel region.
- for: Distributes loop iterations.
- parallel for: Combines parallel execution and loop distribution.
- critical: Allows one thread at a time to execute a section.
- atomic: Protects a supported individual memory operation.
- barrier: Synchronizes threads at a point.
Compilation
With a GCC compiler, an OpenMP program can be compiled using:

gcc -fopenmp program.c -o program

Run it with:
./program


Performance Considerations
Parallel execution does not always make a program faster. Creating threads, synchronizing operations, and sharing memory can introduce overhead.

OpenMP is most effective when a workload contains enough independent operations to justify parallel execution.
Conclusion
OpenMP provides a practical way to develop shared-memory parallel applications. Its directives and runtime functions help programmers use multiple CPU cores without manually managing every thread operation.
