---
id: 0004
title: Logging
tags: [logging, observability]
relates_to: [0003]
status: active
updated: 2026-07-14
---
 
# Logging
 
## Rule
 
- Use the stdlib `logging` module, never `print()` for diagnostic or
  traceability messages in anything beyond a throwaway script.
- One logger per module, created with `logging.getLogger(__name__)` — never
  a hand-shared global logger.
- Pick the level correctly:
  - `DEBUG`: detail only useful during development.
  - `INFO`: normal, relevant flow events (startup, operation completed).
  - `WARNING`: something unexpected but recoverable.
  - `ERROR`: an operation failed.
  - `CRITICAL`: the system can no longer operate.
- Never log sensitive data (passwords, tokens, PII), not even at `DEBUG`.
- Use lazy interpolation (`logger.info("msg %s", value)`), not f-strings
  inside the call, to avoid formatting strings that are never emitted.
## Why
 
`print()` has no levels, can't be filtered or redirected easily, and
disappears in production. A logger per module lets you toggle verbosity by
area without touching code, and is the foundation for later integrating
observability tools (Sentry, ELK, etc.) without rewriting anything.
 
## Example
 
```python
import logging
 
logger = logging.getLogger(__name__)
 
 
def process_order(order_id: str) -> None:
    logger.info("Processing order %s", order_id)
    try:
        _execute(order_id)
    except InvalidOrderError as exc:
        logger.warning("Order %s rejected: %s", order_id, exc)
        raise
    except Exception:
        logger.exception("Unexpected error processing order %s", order_id)
        raise
    else:
        logger.info("Order %s processed successfully", order_id)
```
 
## Anti-patterns to avoid
 
```python
# Bad: print instead of logging
print(f"Processing order {order_id}")
 
# Bad: f-string inside the logger call (formats even if the level is off)
logger.debug(f"Full payload: {payload}")
 
# Bad: logging sensitive data
logger.info("Login successful for %s with password %s", user, password)
```
 
## Notes
 
- In applications with multiple modules, configure logging once at the
  entry point (`main()`, the root package's `__init__.py`, or the API
  setup) — never inside each individual module.
- See rule 0003 for how logging relates to exception handling (what gets
  logged, and at which level, depending on the error type).
 