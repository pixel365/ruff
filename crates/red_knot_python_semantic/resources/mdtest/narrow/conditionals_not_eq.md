# Narrowing for `!=` conditionals

## `x != None`

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()
x = None if flag else 1

if x != None:
    reveal_type(x)  # revealed: Literal[1]
```

## `!=` for other singleton types

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()
x = True if flag else False

if x != False:
    reveal_type(x)  # revealed: Literal[True]
```

## `x != y` where `y` is of literal type

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()
x = 1 if flag else 2

if x != 1:
    reveal_type(x)  # revealed: Literal[2]
```

## `x != y` where `y` is a single-valued type

```py
def bool_instance() -> bool:
    return True

flag = bool_instance()

class A: ...
class B: ...

C = A if flag else B

if C != A:
    reveal_type(C)  # revealed: Literal[B]
```

## `!=` for non-single-valued types

Only single-valued types should narrow the type:

```py
def bool_instance() -> bool:
    return True

def int_instance() -> int:
    return 42

flag = bool_instance()
x = int_instance() if flag else None
y = int_instance()

if x != y:
    reveal_type(x)  # revealed: int | None
```
