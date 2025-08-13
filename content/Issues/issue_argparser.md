# Booleans in `argparse` (and how to fix it)

**TL;DR:** Don’t use `type=bool` for CLI flags. It turns strings like `"False"` into `True`. Use actions instead: `action='store_true'`, `action='store_false'`, or (Python ≥ 3.9) `BooleanOptionalAction`.

---

## Why `--verbose=False` becomes `True`

`argparse` reads CLI values as strings. With `type=bool`, it effectively does:

```python
bool("False")  # -> True (any non-empty string is truthy)
```

So running:

```bash
python app.py --verbose=False
```

sets `args.verbose` to `True` — surprise!

---

## Correct patterns

### 1) One-way flags (present = True, absent = False)

```python
parser.add_argument('--verbose', action='store_true', help='enable verbose logging')
# default is False unless you pass --verbose
```

If you want the inverse (default True, disable with a flag):

```python
parser.add_argument('--no-cache', action='store_false', dest='cache', help='disable cache')
parser.set_defaults(cache=True)
```

### 2) Two-way flags (Python ≥ 3.9)

Gives you `--feature` and `--no-feature`.

```python
from argparse import BooleanOptionalAction
parser.add_argument('--verbose', action=BooleanOptionalAction, default=False)
# supports: --verbose / --no-verbose
```

### 3) If you *must* accept `--flag=True|False`

Use a small parser that understands common boolean spellings:

```python
def str2bool(v):
    if isinstance(v, bool):
        return v
    v = v.lower()
    if v in ('1','true','t','yes','y','on'):
        return True
    if v in ('0','false','f','no','n','off'):
        return False
    raise argparse.ArgumentTypeError("expected a boolean")

parser.add_argument('--verbose', type=str2bool, nargs='?', const=True, default=False)
# --verbose         -> True
# --verbose true    -> True
# --verbose false   -> False
# (omitting it)     -> False
```

---

## Drop-in snippet with fallback (works on all Python versions)

```python
import argparse

parser = argparse.ArgumentParser()

try:
    # Python 3.9+
    from argparse import BooleanOptionalAction
    parser.add_argument('--verbose', action=BooleanOptionalAction, default=False,
                        help='enable or disable verbose logs')
except Exception:
    # Older Pythons
    parser.add_argument('--verbose', action='store_true', help='enable verbose logs')
    # optional companion to explicitly disable (rarely needed):
    parser.add_argument('--no-verbose', dest='verbose', action='store_false')

args = parser.parse_args()
```

---

## Bonus: converting to libraries that expect ints

Some libs (e.g., Stable-Baselines3) take `verbose` as an **int**. Just map:

```python
sb3_verbose = 1 if args.verbose else 0
```

---

## Takeaway

* Avoid `type=bool` in `argparse`.
* Prefer actions (`store_true`, `store_false`, `BooleanOptionalAction`).
* If you need `--flag=True|False`, supply a `str2bool` parser.

Copy-paste this into your PR to save future-you (and reviewers) from confusing CLI behavior.
