---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Combinatorics

## Relational programming

```{code-cell}
# pip install kanren
from kanren import Relation, facts, lall, var, eq, run

# part of Python's standard library
from functools import partial
```

```{code-cell}
# facts
different_2 = Relation()
facts(different_2,
      ("red", "green"),
      ("green", "red")
)

different_3 = Relation()
facts(different_3,
      ("red", "green"),
      ("green", "red"),
      ("red", "blue"),
      ("blue", "red"),
      ("green", "blue"),
      ("blue", "green") 
)
```

```{code-cell}
# rules
def coloring(different, t, m, a, g, f):
    return lall(
        different(t, m),
        different(t, a),
        different(t, g),
        different(m, a),
        different(a, g),
        different(a, f),
        different(g, f)
    )


coloring_2 = partial(coloring, different_2)
coloring_3 = partial(coloring, different_3)
```

```{code-cell}
# goals
t, m, a, g, f = var(), var(), var(), var(), var()

colors_2 = coloring_2(t, m, a, g, f)
colors_3 = coloring_3(t, m, a, g, f)

a_red = eq(a, "red")
t_green = eq(t, "green")
```

```{code-cell}
# query
query = { 
    "Tennessee": t,
    "Mississippi": m,
    "Alabama": a,
    "Georgia": g,
    "Florida": f
}
```

```{code-cell}
run(0, query, colors_2)
```

```{code-cell}
run(0, query, colors_3)
```

```{code-cell}
run(0, query, colors_3, a_red, t_green)
```

```{code-cell}
def xor_fn(x, y):
    return x ^ y

xor_fn(True, False)
```

```{code-cell}
xor_rel = Relation()
facts(xor_rel,
      (False, False, False),
      (True, False, True),
      (False, True, True),      
      (True, True, False)
)
      
z = var()
run(0, z, xor_rel(True, False, z))
```

```{code-cell}
x = var()
run(0, x, xor_rel(x, False, True))
```

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, True))
```

## Boolean tensors

```{code-cell}      
z = var()
run(0, z, xor_rel(True, False, z))
```

```{code-cell}
x = var()
run(0, x, xor_rel(x, False, True))
```

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, True))
```

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, False))
```

## Boolean satisfiability

```{code-cell}
# pip install pycosat
from pycosat import itersolve
```

```{code-cell}
# state x has >=1 colors
def ge1(x):
    return [[x, x+1, x+2]]


# state x has <=1 colors
def le1(x):
    return [[-x, -(x+1)], [-x, -(x+2)], [-(x+1), -(x+2)]]


# states x,y have different colors
def ne(x, y):
    return [[-x, -y], [-(x+1), -(y+1)], [-(x+2), -(y+2)]] 


t, m, a, g, f = 1, 4, 7, 10, 13
formula = ge1(t) + ge1(m) + ge1(a) + ge1(g) + ge1(f) + \
          le1(t) + le1(m) + le1(a) + le1(g) + le1(f) + \
          ne(t, m) + ne(t, a) + ne(t, g) + ne(m, a) + \
          ne(a, g) + ne(a, f) + ne(g, f)
```

```{code-cell}
def run_sat(formula):
    def assignment(x):
        x0 = x-1
        state = ["Tennessee", "Mississippi", "Alabama", "Georgia", "Florida"][x0//3]
        color = ["red", "green", "blue"][x0%3]
        return state, color
    
    def solution(xs):
        return [assignment(x) for x in xs if x>0]
    
    return [solution(xs) for xs in itersolve(formula)]


run_sat(formula)
```

```{code-cell}
a_red, t_green = [[7]], [[2]]
run_sat(formula + a_red + t_green)
```

## Counting SAT solutions

```{code-cell}
solutions = run(0, (x, y), xor_rel(x, y, True))
len(solutions)
```