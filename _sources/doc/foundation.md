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

We will use the programming language [Python](https://www.python.org/) 
for our coding examples. Recently it has undergone a raise in popularity.
Confer page
[About Python](https://executablebooks.github.io/quantecon-mini-example/docs/about_py.html),
also to gain more motivation and insights.
What are the reasons for this increase in interest?

Usually a programming language needs a killer app to be successful, or 
a killer framework. Ruby had Ruby on Rails, JavaScript's killer app is the 
web browser, Java was pushed by the JVM. According to the linked document, 
Python is the proven leader when it comes to data analysis, with 
[pandas](https://pandas.pydata.org/) being the library driving this effort.

Still, there are reasons within the language's design, enabling this wide acceptance.
Data analysis requires both, numerical performance to process huge data sets,
and expressive power to handle complex tasks and to have short development cycles.
Very often these demands stand for incompatible requirements, but Python got both 
right at the same time. With bindings for C/C++ code and even Fortran routines, it is 
able to consume high performant numerical code as incorporated in [NumPy](https://numpy.org/) 
for example. On the other hand, Python is long known for expressive syntax and 
concepts, for instance _list comprehensions_.

Combining both is a nice example of _abstraction_. The details of mathematical
algorithms are encapsulated in C/C++ assemblies and abstracted away from the 
user of libraries as NumPy or pandas. It feels very comfortable to use lightweight
glue code in the dynamically typed Python language, to combine the numerical 
building blocks into the solution of a real world problem. 

We will illustrate the power of abstraction on a much smaller scale 
(and completely in Python). We assume that our real world problem is to
print out all odd integers between `10` and `20`. This is not very realistic, 
but will serve the purpose of exemplification.

A very straight forward and not very abstract solution would be:

```{code-cell}
x = 11
while x < 20:
    print(x)
    x += 2
```

You might object that it would be a one-liner if we would have used the 
built-in [`range`](https://docs.python.org/3/library/stdtypes.html#range) 
generator. This is true, but on one hand this will be our starting point 
of a non-abstract solution, on the other hand if we would have had to 
use plain C code, the solution would have looked very similar to this example.

What is the problem with this snippet? It intertwines the part of generating
the numbers with the part of looping through the numbers with the part of 
printing the output. What if we would like to print out other content or 
use another output method? Much better would be to decouple these parts as 
in the following code, providing a solution to the same problem.

```{code-cell}
def log(print_fn, xs):
    for x in xs:
        print_fn(x)
        
log(print, range(11, 20, 2))
```

The parts are separated now. The number generation is provided by
`range`, the output done by `print`. The `log` function connects the 
low-level parts. It takes a function for printing and the numbers as iterable.
Both is combined using `for`, iterating through the numbers and feeding 
them into the print function.

What have we won? The glue code is simple, we could consider it 
to be declarative. Furthermore, the details are abstracted away. In `log`
there is no interest in how `print_fn` and `xs` are implemented. We just 
need to know that `print_fn` takes a number and outputs it, whereas `xs`
is iterable. This has the consequences, that under these requirements, 
the detail providing parts are easily exchangeable. `print_fn` could follow 
different output strategies, printing to stdout, or using an api-call to 
send it to a web service, or write it to a database. Also the content can 
easily be changed. The following snippet prints out all permutations of 
a given list, using 
[Heap's algorithm](https://en.wikipedia.org/wiki/Heap%27s_algorithm).

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

Having implemented `permutations`, we can now leave the detail level
behind us, and simple use it. A magic square is a square of numbers
with each row, column and both diagonals yielding the same sum.
Let us find all magic squares of numbers `1,...,9`. Using the existing
functions, this is now implemented in a few lines of code -
although not in the most performant way.

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

To summarize, abstractions help to focus on the appropriate level 
of detail and to decompose complex structures into exchangable pieces.
Coming back to tensors, we will use exactly this strategy to develop 
our understanding. Basic building blocks are _vector spaces_ and 
_linear maps_. There is a huge variety of instances. Vector spaces can 
be based on real numbers, complex numbers, can contain lists of fixed length,
infinite sequences or even continuous functions. Accordingly, linear maps 
can look quite differently as well. But to all these different objects,
the principles of linearity and preserving linearity are common.
Similarly to the mentioned considerations, we will leave out the details
of the specific instances and exploit linearity to obtain common properties.
For example, a method for solving linear equations will work regardless if
the underlying field is real numbers or complex numbers.
Similarly, after we will have introduced _tensors_ and _tensor networks_ 
as structures, we will recognize their wide range of potential applications
in Physics, Machine Learning and Data Science.

## Vector Spaces

We will recall needed concepts from Linear Algebra, but mostly refer 
to results instead of laying out proofs. We recommend the excellent text book
{cite}`axler15` to gain more details.

Vector spaces are defined over fields, we will restrict ourselves to
the fields of real numbers $\R$ and complex numbers $\C$. A vector space
generalizes the idea of a geometric space with coordinates. Coordinates help
to turn geometric operations as (homogeneous) dilation and translation
into arithmetic operations. Dilation translates into multiplication of 
a vector's coordinates by a number from the underlying field. We will call
this operation _scalar multiplication_. Translation will add a vector's 
coordinates by the coordinates of a tranlation defining vector. We will
call this operation _addition_.

```{prf:definition} vector space
:label: def-vector-space

A ___vector space___ over a field $\F$ is a set $V$, 
equipped with an ___addition___ and a ___scalar multiplication___, 
fulfilling all conditions given below.

* An ___addition___ on $V$ is a function that assigns to each pair
of elements $u,v\in V$ an element $u+v\in V$.
* A ___scalar multiplication___ on $V$ over $\F$ is a function that assigns
to each pair of elements $a\in\F$ and $v\in V$ an element $a\cdot v\in V$.
Most of the time we will omit the multiplication dot and simply write
$av\in V$.

The addition has to satisfy these conditions:

* ___commutativity___: $u+v=v+u$ for all $u,v\in V$,
* ___associativity___: $(u+v)+w=u+(w+v)$ for all $u,v,w\in V$,
* ___identity___: there exists an element $0\in V$ such that $v+0=v$
for all $v\in V$,
* ___inverse___: for every $v\in V$, there exists an element $(-v)\in V$
such that $v+(-v)=0$.

The scalar multiplication has to satisfy these conditions:

* ___associativity___: $(ab)v=a(bv)$ for all $a,b\in\F$ and all $v\in V$,
* ___inverse___: $1v=v$ for all $v\in V$.

Together both operations have to satisfy this condition:

* ___distributivity___: $a(u+v)=au+av$ and $(a+b)v=av+bv$ for all
$a,b\in\F$ and all $u,v\in V$.

We call the elements of $V$ ___vectors___ or ___points___. 

In case $\F=\R$, we call $V$ a ___real vector space___. 

In case $\F=\C$, we call $V$ a ___complex vector space___.
```

We will give a few examples. In the exercises we will provide
evidence, that those indeed define vector spaces. 

```{note}
In respect of
notation, we will do something that is uncommon in Linear Algebra
works, but widely used in elaborations on tensors. We will
write indices of coefficients in superscript. This helps to immediately
identify coefficients and to use subscript indices to denote
a bunch of vectors. For example, $v_i$ refers to a vector,
whereas $v^i$ refers to to the $i$-th coefficient/component of vector $v$.
In case, this convention confuses with usage of powers, we will
explicitely state, how a notation is meant. There is also the possibility
to put an index into parantheses, in this case it will never be a power.
For example $v^{(i)}$ is not $v$ to the power of $i$, but again the
$i$-th coefficient of $v$.
```

```{prf:example} $\F^n$
:label: ex-Fn

$\F^n$ is the set of all lists of length $n$ of elements of $\F$:

$$\F^n\def\{(v^1,\ldots,v^n):v^i\in\F,\,i=1,\ldots,n\}\,.$$

The operations are executed elementwise. For $u=(u^1,\ldots,u^n)\in\F^n$ 
and $v=(v^1,\ldots,v^n)\in\F^n$ we define the addition as

$$u+v\def (u^1+v^1,\ldots,u^n+v^n)\,.$$

The multiplication with a scalar $a\in\F$ is defined as 

$$av\def (av^1,\ldots,av^n)\,.$$

$\F^n$ together with addition and scalar multiplication is a
vector space over $\F$. Often used are $\R^3$ or $\R^4$ to model
the space or spacetime of our universe. Quantum computing makes
use of $\C^2$ to model the state space of a qubit.
```

```{prf:example} magic squares
:label: ex-magic-squares

Let $M^n$ be the set of all magic squares of edge length $n$.
A magic square $m\in M^n$ is described by $n^2$ real numbers
$m^{ij}\in\R,\,i=1,\ldots,n,\,j=1,\ldots,n$ (with $i$ denoting the row 
and $j$ denoting the column). All row sums

$$\sum\limits_{j=1}^nm^{ij}=s\,,$$

all column sums

$$\sum\limits_{i=1}^nm^{ij}=s\,,$$

the main diagonal sum

$$\sum\limits_{i=1}^nm^{ii}=s\,,$$

and the secondary diagonal sum

$$\sum\limits_{i=1}^nm^{i(n+1-i)}=s\,,$$

shall yield the same value $s$. Note, that we do not require that the $m^{ij}$
are distinct. The square with all entries being $1$ is a valid magic square.
Equipped with elementwise operations (similar to $\R^n$)

$$
(m+\tilde{m})^{ij} &\def m^{ij}+\tilde{m}^{ij}\,,\\
(am)^{ij} &\def am^{ij}\,,\,a\in\R\,,
$$

$M^n$ is a vector space.

```


## Linear Maps

## Dual Vector Spaces

## Exercises

```{admonition} Exercise 1.1
:class: tip
Show that $\F^n$ as given in {prf:ref}`ex-Fn` is indeed a vector space. Discuss the 
following (special) cases:
- $n=1$, that is $\R^1$ and $\C^1$.
- Is $\R^n$ over $\C$ a vector space?
- Is $\C^n$ over $\R$ a vector space?
```

```{admonition} Exercise 1.2
:class: tip
Show that magic squares as given in {prf:ref}`ex-magic-squares` form a vector space. 
What are the consequences?
```