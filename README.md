# Debugging with AI

> **Note:** I found and fixed all four bugs below (see commit `54f0701`). I then reverted `buggy_program.py` back to its broken state so the bugs can be seen in context next to this write-up.

---

## Bug 1: Missing colon on the function definition

**Line 3**

```python
def calculate_stats(numbers)
```

**Error type:** `SyntaxError` (`expected ':'`)

**Problem:** In Python, a `def` statement has to end with a colon. Without it, the file won't parse and nothing runs.

**Fix:**

```python
def calculate_stats(numbers):
```

---

## Bug 2: Missing colon on the `if` statement

**Line 10**

```python
        if num > average
```

**Error type:** `SyntaxError` (`expected ':'`)

**Problem:** An `if` statement also has to end with a colon. Python reports only one syntax error at a time, so this one shows up once Bug 1 is fixed.

**Fix:**

```python
        if num > average:
```

---

## Bug 3: A string mixed into a list of integers

**Line 20**

```python
scores = [85, 92, 78, 95, 88, "70", 93]
```

**Error type:** `TypeError` (`unsupported operand type(s) for +: 'int' and 'str'`)

**Problem:** `"70"` is a string, not a number. When `sum(numbers)` runs inside `calculate_stats`, Python can't add a `str` to an `int` and raises a `TypeError`.

**Fix:** Remove the quotes so every element is an integer:

```python
scores = [85, 92, 78, 95, 88, 70, 93]
```

---

## Bug 4: Dictionary key used without quotes

**Line 24**

```python
print(f"Average: {result[average]}")
```

**Error type:** `NameError` (`name 'average' is not defined`)

**Problem:** Without quotes, `average` is read as a variable name, not as the string key `'average'`. The variable `average` exists only inside `calculate_stats`, so at the top level of the script it's undefined. (If a variable called `average` did exist there, the lookup would probably fail with a `KeyError` instead.)

**Fix:** Quote the key, the same way the other `print` lines do:

```python
print(f"Average: {result['average']}")
```

---

## Summary

| # | Line | Bug | Error Type | Fix |
|---|------|-----|------------|-----|
| 1 | 3 | Missing `:` after `def calculate_stats(numbers)` | `SyntaxError` | Add `:` |
| 2 | 10 | Missing `:` after `if num > average` | `SyntaxError` | Add `:` |
| 3 | 20 | `"70"` is a string in a list of ints | `TypeError` | Change `"70"` to `70` |
| 4 | 24 | `result[average]` uses an undefined variable as the key | `NameError` | Change to `result['average']` |

### Expected output after all fixes

```
Total: 601
Average: 85.85714285714286
Above average: 4 scores
```
