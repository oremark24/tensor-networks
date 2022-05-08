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

# Tensors and Combinatorics

```{figure} ../img/combinatorics/map-coloring.png
:align: center
:name: fig-tensor-networks-map-coloring
Map coloring
```

```{code-cell}
# pip install kanren
from kanren import *

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

# rules
def coloring(different, tn, ms, al, ga, fl):
    return lall(
        different(tn, ms),
        different(tn, al),
        different(tn, ga),
        different(ms, al),
        different(al, ga),
        different(al, fl),
        different(ga, fl)
    )

coloring_2 = partial(coloring, different_2)
coloring_3 = partial(coloring, different_3)
```

```{code-cell}
# goals
tn, ms, al, ga, fl = var(), var(), var(), var(), var()

colors_2 = coloring_2(tn, ms, al, ga, fl)
colors_3 = coloring_3(tn, ms, al, ga, fl)

al_red = eq(al, "red")
tn_green = eq(tn, "green")

# query
query = { 
    "Tennessee": tn,
    "Mississippi": ms,
    "Alabama": al,
    "Georgia": ga,
    "Florida": fl
}
```

```{code-cell}
run(0, query, colors_2)
```

```{code-cell}
run(0, query, colors_3)
```

```{code-cell}
run(0, query, colors_3, al_red, tn_green)
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

## Idea

1. Map coloring problem
2. How does it work? -> relational programming
3. AND as FUNCTION and as RELATION (implementation)
4. 4-color example with constraint programming
5. 4-color example with SAT problem
6. AND Tensor (similar to RELATION)

## TODO

- Boolean tensors
- Counting SAT solutions
- Counting graph colorings