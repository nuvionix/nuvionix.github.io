---
title: "Unit Testing with pytest"
date: 2026-11-26
draft: false
description: "Build small, repeatable tests with assertions, parametrization, exception checks, and mocks."
categories: ["Python"]
tags: ["Python"]
readingTime: true
---

## What is a unit test?

A unit test checks one small part of a program—often a function or method—against an expected result. Tests give developers quick feedback when code changes and help document how a function is meant to behave.

Useful tests are fast, focused, repeatable, and isolated from unrelated services. A test that depends on a live network or a shared database can fail for reasons unrelated to the code being tested.

## Arrange, Act, Assert

A common structure is the **3A pattern**:

1. **Arrange:** prepare inputs and dependencies.
2. **Act:** call the behavior under test.
3. **Assert:** compare the result with the expected outcome.

For example, imagine a small module named `calculator.py`:

```python
def add(left, right):
    return left + right
```

A test in `test_calculator.py` can be:

```python
from calculator import add

def test_adds_two_numbers():
    result = add(2, 3)
    assert result == 5
```

## Install and run pytest

Install pytest in your project's virtual environment with `python -m pip install pytest`. Run `pytest` from the project directory. By default, pytest discovers files named `test_*.py` or `*_test.py` and functions whose names begin with `test_`.

A failing assertion includes useful information about the values that differed. Keep tests readable so the failure points toward one behavior.

## Assertions and edge cases

Python's normal `assert` statement is enough for most tests. Test typical inputs as well as boundary conditions: empty strings, zero, negative values, missing data, and invalid input where those cases are relevant.

Floating-point values require care because many decimal fractions cannot be represented exactly in binary. Use `pytest.approx()` when a small numerical tolerance is appropriate:

```python
import pytest

def test_ratio():
    assert 1 / 3 == pytest.approx(0.333333, rel=1e-5)
```

## Testing exceptions

If a function is expected to raise an exception, pytest provides `pytest.raises`:

```python
import pytest

def parse_age(value):
    age = int(value)
    if age < 0:
        raise ValueError("age cannot be negative")
    return age

def test_negative_age_is_rejected():
    with pytest.raises(ValueError, match="negative"):
        parse_age("-1")
```

This verifies both that the error occurs and, optionally, that its message matches the expected text.

## Parametrized tests

When one behavior should work for several input/output pairs, use `@pytest.mark.parametrize` rather than duplicating nearly identical test functions:

```python
import pytest
from calculator import add

@pytest.mark.parametrize(
    "left, right, expected",
    [(1, 2, 3), (0, 0, 0), (-2, 5, 3)],
)
def test_add(left, right, expected):
    assert add(left, right) == expected
```

Each row becomes a separate test case, making failures easier to identify.

## Fixtures and mocks

Fixtures prepare reusable test data or dependencies. They are useful for creating temporary files, sample objects, or test clients. Mocks replace an external dependency so a unit test can control its behavior without making a real network request or sending a real message.

Use mocks at system boundaries, but avoid mocking every internal function: tests that mirror implementation details become fragile when the design changes.

## A practical testing routine

- Name tests after the behavior they verify.
- Cover normal, boundary, and error cases.
- Keep each test independent of execution order.
- Use fixtures for shared setup and parametrization for related cases.
- Run the suite before committing changes.

A compact suite of meaningful tests is more useful than a large collection of assertions that do not explain expected behavior.
