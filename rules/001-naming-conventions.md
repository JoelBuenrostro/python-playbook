---
id: 001
title: Naming conventions
tags: [naming, pep8]
relates_to: []
status: active
updated: 2026-07-07
---

# Naming conventions

## Rule

- Variables and functions: `snake_case`.
- Classes: `PascalCase`.
- Constants: `UPPER_SNAKE_CASE`.
- Modules and packages: `snake_case`, short, without hyphens.
- Private names prefixed with `_`

## Why

## Example

```python
MAX_RETRIES = 3

class RetryPolicy:
    def __init__(self, max_retries: int = MAX_RETRIES) -> None:
        self._max_retries = max_retries

    def should_retry(self, attempt: int) -> bool:
        return attempt < self._max_retries
```

## Exeptions

- Single-letter names only in short comprehensions or loop indices (`i`, `j`), never in business logic.
