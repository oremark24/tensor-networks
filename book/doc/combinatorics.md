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

```{code-cell}
# the code below requires the miniKanren package
# pip install miniKanren

from kanren import run, eq, membero, var, conde
x = var()
run(0, x, eq(x, 5))
```

## Idea

1. Zebra Puzzle (solution with miniKanren)
2. How does it work? -> relational programming
3. AND as FUNCTION and as RELATION (implementation)
4. 4-color example with constraint programming
5. 4-color example with SAT problem
6. AND Tensor (similar to RELATION)

## TODO

- Boolean tensors
- Counting SAT solutions
- Counting graph colorings