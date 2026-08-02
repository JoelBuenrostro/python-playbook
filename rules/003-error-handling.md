---
id: 0003
title: Error handling
tags: [errors, exceptions, logging]
relates_to: [0002]
status: active
updated: 2026-07-08
---

# Error handling

## Rule

- Use specific exceptions, never a bare `except Exception`, except at the
  outermost point of the application (e.g. the `main()` of a CLI or an
  API's middleware).
- Create custom exceptions when the error represents a business rule, not
  a generic technical error.
- Never silence an exception without logging or re-raising it. A bare
  `except: pass` is forbidden unless accompanied by an explicit comment
  justifying why it's safe to ignore.
- Error messages must be actionable: state what happened and, if
  applicable, what can be done about it.

## Why

Catching generic exceptions hides real bugs and complicates debugging.
Specific exceptions allow each case to be handled differently (retry,
inform the user, fallback) and leave a clear trace in the logs of what
failed and why.

## Example

```python
class InsufficientFundsError(Exception):
    """Raised when an account doesn't have enough funds to complete the operation."""

    def __init__(self, account_id: str, balance: float, amount_required: float) -> None:
        self.account_id = account_id
        self.balance = balance
        self.amount_required = amount_required
        super().__init__(
            f"Account {account_id}: balance {balance} insufficient "
            f"to withdraw {amount_required}"
        )


def withdraw(account: Account, amount: float) -> None:
    if account.balance < amount:
        raise InsufficientFundsError(account.id, account.balance, amount)
    account.balance -= amount
```

```python
# Outermost point: a generic Exception catch is justified here
def main() -> None:
    try:
        run_operation()
    except InsufficientFundsError as exc:
        logger.warning("Operation rejected: %s", exc)
        print(f"Could not complete: {exc}")
    except Exception:
        logger.exception("Unexpected error")
        raise
```

## Anti-patterns to avoid

```python
# Bad: silences the error without leaving a trace
try:
    process()
except Exception:
    pass

# Bad: generic exception with no context
raise Exception("something went wrong")

# Bad: broad catch in business logic (should be specific)
def withdraw(account, amount):
    try:
        account.balance -= amount
    except Exception:
        return None
```

## Notes

- For expected errors within a flow (business validations), prefer custom
  exceptions over return codes (`None`, `-1`, etc.) — it keeps control flow
  explicit and typed (see rule 0002).
- In API projects (FastAPI/Flask), map custom exceptions to HTTP status
  codes in a single place (a central handler), not repeated per endpoint.