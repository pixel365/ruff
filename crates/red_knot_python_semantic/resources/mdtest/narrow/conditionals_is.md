# Narrowing for `is` conditionals

## `is None`

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()
x = None if flag else 1

if x is None:
    reveal_type(x)  # revealed: None

reveal_type(x)  # revealed: None | Literal[1]
```

## `is` for other types

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()

class A: ...

x = A()
y = x if flag else None

if y is x:
    reveal_type(y)  # revealed: A

reveal_type(y)  # revealed: A | None
```

## `is` in chained comparisons

```py
def bool_instance() -> bool:
    return True

x_flag, y_flag = bool_instance(), bool_instance()
x = True if x_flag else False
y = True if y_flag else False

reveal_type(x)  # revealed: bool
reveal_type(y)  # revealed: bool

if y is x is False:  # Interpreted as `(y is x) and (x is False)`
    reveal_type(x)  # revealed: Literal[False]
    reveal_type(y)  # revealed: bool
```
