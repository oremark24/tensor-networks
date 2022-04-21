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

# Foundation

Before we can deep dive into tensor networks, we have to prepare 
ourselves with the tools needed to understand them. Our tool set 
will mostly contain mathematical notions and basic results. Why is
Mathematics so powerful and used in many other fields as 
Computer Science or Physics? One of the reasons is its built-in 
core concept of _abstraction_. It allows to start with very simple 
terms and combine them step by step into more complicated ones.

We will apply the same strategy. We start out with some thoughts about
abstractions and then introduce basic structures of Linear Algebra as 
_vector spaces_ and _linear maps_. Later we will generalize what we have 
learned here to define _tensors_ and _tensor networks_.

## Abstraction
```{code-cell}
x = 11
while x < 20:
    print(x)
    x += 2
```

```{code-cell}
def log(print_fn, xs):
    for x in xs:
        print_fn(x)
        
log(print, range(11, 20, 2))
```

```{code-cell}
def permutations(xs):
    def heap(xs, k):
        def is_even(s):
            return s%2 == 0

        def swap(s, t):
            xs[s], xs[t] = xs[t], xs[s]

        if k == 1:            
            yield list(xs) # yield new list, not mutatable by next steps
        else:
            yield from heap(xs, k-1)
            for i in range(k-1):
                if is_even(k):
                    swap(i, k-1)
                else:
                    swap(0, k-1)
                yield from heap(xs, k-1)
                
    yield from heap(xs, len(xs))
    
log(print, permutations(["Penny", "Leonard", "Sheldon"]))
```

```{code-cell}
def check(numbers):
    patterns = [
        [0, 1, 2], # first row
        [3, 4, 5], # second row
        [0, 3, 6], # first column
        [1, 4, 7], # second column
        [0, 4, 8], # main diagonal
        [2, 4, 6], # secondary diagonal
    ]    
    for pattern in patterns:
        line = [num for i, num in enumerate(numbers) if i in pattern]
        if sum(line) != 15:
            return False        
    return True

def print_square(numbers):
    s = f"{numbers[0]} {numbers[1]} {numbers[2]}\n"
    s += f"{numbers[3]} {numbers[4]} {numbers[5]}\n"
    s += f"{numbers[6]} {numbers[7]} {numbers[8]}\n"
    print(s)

numbers = list(range(1, 10))
magic_squares = [p for p in permutations(numbers) if check(p)]

log(print_square, magic_squares)
```

## Vector Spaces

## Linear Maps
