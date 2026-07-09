---
id: 0002
title: Mandatory Type Hints
tags: [typed, mypy, functions]
relates_to: [0001]
status: active
updated: 2026-07-07
---

# Mandatory Type Hints

## Rule

All public functions must include type hints in their parameters and return value. Private functions (`_name`) can omit them if the type is obvious from the immediate context.

## Why

- It makes it easier for tools (mypy, IDEs, AI) to detect errors before execution.

- It serves as minimal documentation without requiring extra maintenance.

## Example

```python
def calculate_discount(price: float, percentage: int) -> float:

return price * (1 - percentage / 100)
```

## Notes

- Use `Optional[X]` (or `X | None`) explicitly, never leave a parameter without a type when it can be `None`.
- For data structures, prefer `dataclass` or `TypedDict` over plain untyped dictionaries.
