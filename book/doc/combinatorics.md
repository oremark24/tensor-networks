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

What is _Combinatorics_? It is a discipline of mathematics that investigates existence,
construction and counting of configurations (confer the preface of {cite}`polya83`).
Here, a configuration is conceived as a discrete structure. That is an artifact that
is not continuous, but defined by finite or infinite countable parameters.

## Relational programming

An example combinatorial problem is the following map coloring puzzle. Look at the map 
of five states.

```{figure} ../img/combinatorics/map-coloring.png
:align: center
:name: fig-combinatorics-map-coloring
Map coloring
```

The task is to color the state shapes such that states with a common border are colored
differently. This shall be done in a way that as few colors as possible are used. 
The given map represents a small example that could even be solved with pencil and paper.
This comes in handy, when we might want to verify our algorithmic solutions later. But
of course a pencil and paper approach would soon fail, if the map becomes big and complex.

An interesting way to solve problems like this in a declarative fashion, offers logic 
programming. It will abstract away the solution enumerating part and instead rely on 
a description of solution identifying properties.

The map {numref}`fig-combinatorics-map-coloring` and also the solution approach is taken 
from the repository {cite}`fawi20`, that gives an introduction into the logic programming 
library `pytholog` for Python. It adds Prolog as an external DSL to Python. 

We will be following another path, that is explained in the entertaining book 
"The Reasoned Schemer", {cite}`friedman18`, or more prosaic in William Byrd's dissertation, 
{cite}`byrd09`. Both publications feature and develop the Scheme internal logic DSL `miniKanren`. 
We will use its Python port `kanren` from repository {cite}`rocklin13`.

Let us describe the solution to the given map coloring problem step by step. As mentioned,
we will be using `kanren` as dependency and the `partial` function from `functools`, allowing
for [currying](https://en.wikipedia.org/wiki/Currying).

```{code-cell}
# pip install kanren
from kanren import Relation, facts, lall, var, eq, run

# part of Python's standard library
from functools import partial
```

As a first step we have to explain the problem to `kanren`. This will be done through _facts_ and
_rules_. Facts form the knowledge base by defining relations between possible values of logic
variables. We do not claim that our solution displays the only feasible approach. As a challenge
you might want to try to find a different way. However, in our problem description we are using
color tones as knowledge atoms. The facts are given by all possible color combinations to be
used for adjacent states. We want to try out coloring the map with two colors. We define a 
relation `different_2` for that. In case the two color attempt is not going through, we 
define a relation `different_3` holding all facts for a try with three colors.

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

Rules extend the facts through logical inference. As foundation for our rules
we define the function `coloring` that takes a set of facts and five logic variables
representing a possible color per state. The function body checks, if the coloring is
feasible. This is the case, if and only if adjacent states are colored differently.
We are using the defined relations and the `kanren` combinator `lall`. It stands for
"logic all" and is true exactly when all subsequent facts are true.

From the function we inherit two rules. `coloring_2` contains the knowledge of feasible
colorings with two colors, `coloring_3` does the same for three colors respectively.

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

Next we will tell `kanren`, what we are seeking for. This is expressed by means of
_goals_ and a _query_. Goals are combining facts and rules with values that set
solution properties. First we define a logic variable per state. The we define
goals `colors_2` and `colors_3` for feasible colorings with two, respectively three,
colors. You might already wonder about the certainty that in case of success 
we will not find a unique coloring. From one feasible coloring we can easily derive 
another  valid coloring by permuting the colors. To cut down a potential set of solutions
further to our likings, we define two more goals `a_red` and `t_green`. They
require Alabama to be colored "green" and Tennessee to be colored "green", respectively.

```{code-cell}
# goals
t, m, a, g, f = var(), var(), var(), var(), var()

colors_2 = coloring_2(t, m, a, g, f)
colors_3 = coloring_3(t, m, a, g, f)

a_red = eq(a, "red")
t_green = eq(t, "green")
```

Our final preparatory step is to define the query - the solution structure. We want to know
the coloring of all five states. Hence, our query contains all five logic variables, combined
into a dictionary.

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

Now we are done with the problem and solution description. Enumerating and checking configurations
in an efficient way, we will leave to the `kanren` system. The process is executed with the `run`
function. The first parameter tells, in how many solutions we are interested in. We are giving a
`0` to get all possible solutions. The second parameter provides the query followed by parameters
containing goals. Let us first find all solutions using at most two colors.

```{code-cell}
run(0, query, colors_2)
```

The answer is an empty tupel. This (logically) proves, that there is no solution. So, maybe there
is more luck with three colors.

```{code-cell}
run(0, query, colors_3)
```

This time the answer contains six possible configurations. Taking into account that we would already
obtain six permutations from one feasible coloring, the coloring is unique if we factor out permutations.
To nail down a unique coloring we fix the colors of Alabama and Tennessee by stating two more goals.

```{code-cell}
run(0, query, colors_3, a_red, t_green)
```

All goals we have used are defined by a relation. This is a more restricted form of _unification_, 
that for example Prolog makes available. The form of logic programming we have used is therefore
called _relational programming_.

To elaborate more on the difference of programming with relations and "conventional" programming,
let us compare functions and relations. The `xor` operator will serve as example. 
We define it as function of two boolean parameters. As usual we can feed in two boolean values
to get the boolean result of the operator in return.

```{code-cell}
def xor_fn(x, y):
    return x ^ y

xor_fn(True, False)
```

The same can be achieved with relational programming as well. Similar to the map coloring example
we will be using `kanren` to set up the appropriate relation. It contains the truth table of the
`xor` ($\oplus$) operator. The relation consists of four facts, each fact is a triple $(x,y,z)$ 
representing one row of the truth table with $x\oplus y = z$. We write $\oplus$, since
`xor` is the same as the addition (modulo $2$) of the field on $\{0,1\}$ with $0$ representing 
`False` and $1$ representing `True`. The function call can be mimicked by setting a goal that 
provides $x$ and $y$ and asks for $z$.

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

But, the relation does not distinguish between inputs and outputs as the function does. That we have
used the first two parameters of the triple to define a goal was due to the fact that we wanted to
model the function call. Instead it is not forbidden to provide parameter two and three and ask for
the first one.

```{code-cell}
x = var()
run(0, x, xor_rel(x, False, True))
```

What we have done is actually to ask for solutions of the equation $x\oplus 0 = 1$. We can even ask
for pairs, or in terms of the equation, a solution of $x\oplus y=1$. `kanren` returns both solutions.

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, True))
```

(sec-combinatorics-boolean-tensors)=
## Boolean tensors

To bring tensors into play, we consider $\C^2$ as logic space, with 
$e_0\def\begin{bmatrix}1 & 0\end{bmatrix}^T$ representing `False` and 
$e_1\def\begin{bmatrix}0 & 1\end{bmatrix}^T$ representing `True`.
In this terminology we can express `xor` as tensor.

````{prf:definition} $\xor$-tensor
:label: def-combinatorics-xor-tensor
We define the tensor $\xor\in\C^2\otimes\C^2\otimes\C^2$ by

$$
\xor = 
    e_0\otimes e^0\otimes e^0 +
    e_1\otimes e^1\otimes e^0 +
    e_1\otimes e^0\otimes e^1 +
    e_0\otimes e^1\otimes e^1 \,,
$$

and draw

```{figure} ../img/combinatorics/xor-tensor.svg
:align: center
:height: 50em
:name: fig-combinatorics-xor-tensor
$\xor$-tensor
```
````

Let us see, if the $\xor$-tensor contains `xor` behavior. We will check the truth table rows.
One important detail is the symmetry of the $\xor$-tensor regarding the right factors $e^0\otimes e^1$.
This already resembles commutativity of `xor` and allows us to be lax in respect of
describing how we are multiplying out. Indeed $\xor(e_i\otimes e_j)=\xor(e_j\otimes e_i)$ and we
do not have to precisely laying out which contravariant index belongs to which covariant index.
But usually we will go from left to right, applying second factor of $\xor$ to first factor of the argument
and applying third factor of $\xor$ to second factor of the argument.

We check:

$$
\xor(e_0\otimes e_0) =
    & \quad e_0\cdot \underbrace{e^0(e_0)\cdot e^0(e_0)}_{=1} \\
    & + e_1\cdot \underbrace{e^1(e_0)\cdot e^0(e_0)}_{=0} \\
    & + e_1\cdot \underbrace{e^0(e_0)\cdot e^1(e_0)}_{=0} \\
    & + e_0\cdot \underbrace{e^1(e_0)\cdot e^1(e_0)}_{=0} \\
    = & \quad e_0 \,.
$$

In similar fashion we can validate the other truth table rows and can verify that $\xor$ behaves indeed
like `xor`:

```{math}
:label: eqn-combinatorics-xor-truth-table
\xor(e_0\otimes e_0) &= e_0 \,, \\
\xor(e_1\otimes e_0) &= e_1 \,, \\
\xor(e_0\otimes e_1) &= e_1 \,, \\
\xor(e_1\otimes e_1) &= e_0 \,. \\
```

Hold on, we have considered $\xor$ as linear transformation on both covariant indices. But we have
learned about relational programming and the beauty of the $\xor$-tensor is that it allows naturally
to be seen as relation as well. We will rewrite aforementioned lines of code in tensor logic.

```{code-cell}      
z = var()
run(0, z, xor_rel(True, False, z))
```

This is still the function call $\xor(e_1\otimes e_0)=e_1$, in diagram notation:

```{figure} ../img/combinatorics/xor-fn.svg
:align: center
:height: 96em
:name: fig-combinatorics-xor-fn
$\xor$ function call
```

```{code-cell}
x = var()
run(0, x, xor_rel(x, False, True))
```

We have used $\xor$ as relation, thus applying the tensor to other indices:

$$
\xor(e^1\otimes e_0)=
    & \quad e^1(e_0)\cdot e^0\cdot e^0(e_0) \\
    & + e^1(e_1)\cdot e^1\cdot e^0(e_0) \\
    & + e^1(e_1)\cdot e^0\cdot e^1(e_0) \\
    & + e^1(e_0)\cdot e^1\cdot e^1(e_0) \\
    = & \quad e^1 \,.
$$

This yields the diagram:

```{figure} ../img/combinatorics/xor-rel.svg
:align: center
:height: 80em
:name: fig-combinatorics-xor-rel
$\xor$ relation call
```

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, True))
```

The `kanren` output delivers already the result. But we can confirm also analytically.

```{math}
:label: eq-combinatorics-xor-not
\xor(e^1)=
    & \quad e^1(e_0)\cdot e^0\otimes e^0 \\
    & + e^1(e_1)\cdot e^1\otimes e^0 \\
    & + e^1(e_1)\cdot e^0\otimes e^1 \\
    & + e^1(e_0)\cdot e^1\otimes e^1 \\
    = & \quad e^1\otimes e^0 + e^0\otimes e^1 \fed X \,.
```

We compare the outcome of equation {eq}`eq-combinatorics-xor-not` with 
{prf:ref}`ex-tensors-not`. Indeed, if we ignore the index types we have constructed a 
`NOT` gate. Similar to {prf:ref}`rem-tensor-networks-kronecker` we will allow all 
combinations of covariant and contravariant indices and depending on the specific 
situation use the appropriate one. We will treat all boolean tensors similarily.

````{prf:remark} Versions of boolean tensors
:label: rem-combinatorics-index-types
According to specific situations we consider boolean tensors with bent wires the same.
For example, the $\xor$ tensor can also be used as:

$$
\xor = 
    e^0\otimes e_0\otimes e_0 +
    e^1\otimes e_1\otimes e_0 +
    e^1\otimes e_0\otimes e_1 +
    e^0\otimes e_1\otimes e_1 \,.
$$

We would then draw

```{figure} ../img/combinatorics/xor-mirror.svg
:align: center
:height: 50em
:name: fig-combinatorics-xor-mirror
$\xor$-tensor
```

Due to symmetry of shapes, it might be necessary to explain the orientation of the tensor
to avoid misunderstandings.
````

Coming back to equation {eq}`eq-combinatorics-xor-not`, we can express it with
the following diagrammatic equation.

```{figure} ../img/combinatorics/xor-not.svg
:align: center
:height: 65em
:name: fig-combinatorics-xor-not
$\xor=X$
```

This was interesting. Maybe another interesting result will uncover when we seek for
all input combinations yielding $e^0$.

```{code-cell}
y = var()
run(0, (x, y), xor_rel(x, y, False))
```

Again, this can also be obtained analytically.

```{math}
:label: eq-combinatorics-xor-delta
\xor(e^0)=
    & \quad e^0(e_0)\cdot e^0\otimes e^0 \\
    & + e^0(e_1)\cdot e^1\otimes e^0 \\
    & + e^0(e_1)\cdot e^0\otimes e^1 \\
    & + e^0(e_0)\cdot e^1\otimes e^1 \\
    = & \quad e^0\otimes e^0 + e^1\otimes e^1 = \delta \,.
```

In diagram notation this is:

```{figure} ../img/combinatorics/xor-delta.svg
:align: center
:height: 65em
:name: fig-combinatorics-xor-delta
$\xor=\delta$
```

In a similar fashion other logic gates can be mimicked by respective tensors.
We will give examples, define each in one version, but keeping in mind that we
might use them with other index types as well.

````{prf:definition} $\and$-tensor
:label: def-combinatorics-and-tensor
We define the $\and$-tensor by

$$
\and = 
    e_0\otimes e^0\otimes e^0 +
    e_0\otimes e^1\otimes e^0 +
    e_0\otimes e^0\otimes e^1 +
    e_1\otimes e^1\otimes e^1 \,.
$$

The diagram shape is:

```{figure} ../img/combinatorics/and-tensor.svg
:align: center
:height: 50em
:name: fig-combinatorics-and-tensor
$\and$-tensor
```
````

````{prf:definition} $\or$-tensor
:label: def-combinatorics-or-tensor
We define the $\or$-tensor by

$$
\or = 
    e_0\otimes e^0\otimes e^0 +
    e_1\otimes e^1\otimes e^0 +
    e_1\otimes e^0\otimes e^1 +
    e_1\otimes e^1\otimes e^1 \,.
$$

The diagram shape is:

```{figure} ../img/combinatorics/or-tensor.svg
:align: center
:height: 50em
:name: fig-combinatorics-or-tensor
$\or$-tensor
```
````

We will define and investigate another tensor that nicely interrelates with other boolean
tensors. Strictly speaking, it is not inherited from a logic gate. It resembles splitting
a bit into two, thus copying a boolean value.

````{prf:definition} $\copy$-tensor
:label: def-combinatorics-copy-tensor
We define the $\copy$-tensor by

$$
\copy = 
    e_0\otimes e_0\otimes e^0 +
    e_1\otimes e_1\otimes e^1 \,.
$$

The diagram shape is:

```{figure} ../img/combinatorics/copy-tensor.svg
:align: center
:height: 35em
:name: fig-combinatorics-copy-tensor
$\copy$-tensor
```
````

Indeed, we have

```{math}
:label: eqn-combinatorics-copy-tensor
\copy(e_0) &= e_0\otimes e_0 \,, \\
\copy(e_1) &= e_1\otimes e_1 \,.
```

The effect of the $\copy$-tensor can also be nicely illustrated by a diagrammatic equation.

```{figure} ../img/combinatorics/copy-effect.svg
:align: center
:height: 100em
:name: fig-combinatorics-copy-effect
$\copy$-tensor effect
```

(sec-combinatorics-bool-sat)=
## Boolean satisfiability

Can we transfer the map coloring problem into the boolean universe? We set the same task,
the states displayed in {numref}`fig-combinatorics-map-coloring` shall be colored using
"red", "green", "blue", such that neighbors (states with common border) have different colors
assigned. We want to rephrase the mathematical model to only use boolean variables and boolean
combinators (logic gates / boolean tensors).

Previously we had logic variables 
$t,m,a,g,f\in\{\text{"}\red\text{"},\text{"}\green\text{"},\text{"}\blue\text{"}\}$, 
representing the colorings of Tennessee, Mississippi, Alabama, Georgia, and Florida, 
respectively. To turn them into boolean variables, we need to split them into three 
flags each, telling if the related color is used. For example, the coloring of Tennesse 
will now be given by the three variables

$$
t_{\red} &= 
    \begin{cases}
        1\,, & \text{Tennessee is colored in "red"} \,, \\
        0\,, & \text{otherwise} \,,
    \end{cases} \\[0.5em]
t_{\green} &= 
    \begin{cases}
        1\,, & \text{Tennessee is colored in "green"} \,, \\
        0\,, & \text{otherwise} \,,
    \end{cases} \\[0.5em]
t_{\blue} &= 
    \begin{cases}
        1\,, & \text{Tennessee is colored in "blue"} \,, \\
        0\,, & \text{otherwise} \,.
    \end{cases}
$$

The boolean variables will be combined by boolean expressions that enforce feasible 
colorings. We have three different types, explained by example.

```{prf:criterion} Tennessee is colored with at least one color
:label: crit-combinatorics-col-ge1

$$
t_{\red}\lor t_{\green}\lor t_{\blue}
$$
```

```{prf:criterion} Tennessee is colored with at most one color
:label: crit-combinatorics-col-le1

$$
(\neg t_{\red}\lor\neg t_{\green}) \land 
(\neg t_{\red}\lor\neg t_{\blue}) \land
(\neg t_{\green}\lor\neg t_{\blue})
$$
```

```{prf:criterion} Tennessee and Mississippi are colored differently
:label: crit-combinatorics-col-neq

$$
(\neg t_{\red}\lor\neg m_{\red}) \land 
(\neg t_{\green}\lor\neg m_{\green}) \land
(\neg t_{\blue}\lor\neg m_{\blue})
$$
```

{prf:ref}`crit-combinatorics-col-ge1` as well as {prf:ref}`crit-combinatorics-col-le1` 
provide an expression per state. {prf:ref}`crit-combinatorics-col-neq` gives an
expression for each pair of adjacent states. We end up with $15$ boolean variables 
and $5+5+7=17$ boolean expressions and can validate the following observation.

```{prf:observation} Coloring by boolean expressions
:label: obs-combinatorics-col-sat
A configuration (assignments of `True` or `False` to all variables) is representing a 
feasible coloring if and only if all boolean expressions evaluate to `True`.
```

A problem of this type, finding configurations of boolean variables such that a set
of boolean expressions is fulfilled, is called 
[Boolean satisfiability problem](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem)
and abbreviated SAT.

There are specialized SAT solvers to treat these kinds of problems. We will be using 
[_PicoSAT_](http://fmv.jku.at/picosat/) ({cite}`biere08`), the Python binding is done 
by _pycosat_ library ({cite}`schnell13`).

```{code-cell}
# pip install pycosat
from pycosat import itersolve
```

We want to solve the SAT version of the map coloring problem. In analogy of `kanren`
facts, rules and goals, we define a boolean expression containing specified criteria
{prf:ref}`crit-combinatorics-col-ge1`, {prf:ref}`crit-combinatorics-col-le1`, 
{prf:ref}`crit-combinatorics-col-neq`.

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

The boolean expression is the only information the SAT solver consumes. We have added
some logic to display the solutions similar to the `kanren` output.

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

Similar to our first approach, we can add more requirements to make the solution unique:

```{prf:criterion} Alabama is colored red and Tennessee is colored green
:label: crit-combinatorics-col-ared

$$
a_{\red}\land t_{\green}
$$
```

In Python code this reads:

```{code-cell}
a_red, t_green = [[7]], [[2]]
run_sat(formula + a_red + t_green)
```

(sec-combinatorics-sat-counting)=
## Counting SAT solutions

We want to work out, how boolean tensors can be used to count the number of solutions
to a specific SAT problem. We start with the simple problem

```{math}
:label: eqn-combinatorics-xor-expression
x\oplus y\,,
```

this is, we are looking for all pairs $(x,y)$ such that `xor`-combining them evaluates to `true`.
Reusing the code we have developed earlier, we can count algorithmically to obtain $2$ solutions.

```{code-cell}
solutions = run(0, (x, y), xor_rel(x, y, True))
len(solutions)
```

How do we obtain this number with tensor networks? The $\xor$-tensor that evaluates to `True` 
is given by:

```{figure} ../img/combinatorics/xor-true.svg
:align: center
:height: 65em
:name: fig-combinatorics-xor-true
$\xor(x\otimes y)=e_1$
```

We recall equation {eq}`eq-combinatorics-xor-not` contains the identity

$$
\xor(e^1) = e^1\otimes e^0 + e^0\otimes e^1 \,.
$$

Each summand of the right hand side is representing one solution of 
{eq}`eqn-combinatorics-xor-expression`. How do we count summands? Well,
we fully contract the tensor with itself, such that indices representing
left parameters are mapped together as well as indices representing right paramters, 
respectively. We get

$$
& C_{i_1,i_2}(C_{j_1,j_2}(\xor(e^1)\otimes\xor(e_1)) \\
& = C_{i_1,i_2}(C_{j_1,j_2}((e^1\otimes e^0 + e^0\otimes e^1)\otimes(e_1\otimes e_0 + e_0\otimes e_1)) \\
& = e^1(e_1)\cdot e^0(e_0) + e^0(e_1)\cdot e^1(e_0) + e^1(e_0)\cdot e^0(e_1) + e^0(e_0)\cdot e^1(e_1) \\
& = 2 \,.
$$

This equation can be expressed as tensor network:

```{figure} ../img/combinatorics/xor-counting.svg
:align: center
:height: 65em
:name: fig-combinatorics-xor-counting
Counting $\xor(x\otimes y)=e_1$
```

In general, we can count as follows:

````{prf:theorem} Counting SAT solutions
:label: thm-combinatorics-sat-counting
Let $x_1,\ldots,x_n\in\{0,1\}$ be boolean variables and let 
$f_1,\ldots,f_m:(x_1,\ldots,x_n)\mapsto z\in\{0,1\}$ be boolean expressions.
Then the number of SAT solutions

$$
\#\{(x_1,\ldots,x_n) : f_1(x_1,\ldots,x_n)=\ldots=f_m(x_1,\ldots,x_n)=1\}
$$

is determined by the tensor network:

```{figure} ../img/combinatorics/f-counting.svg
:align: center
:height: 115em
:name: fig-combinatorics-f-counting
Counting SAT solutions
```

The tensor $f$ is derived from $f=f_1\land\ldots\land f_m$ and constructed accordingly
from boolean tensors.
````

The idea of proof is outlined in the construction we have done previously. We give another example.

````{prf:example} Counting
We want to count all configurations $(t_\red,t_\green,t_\blue)$ fulfilling
{prf:ref}`crit-combinatorics-col-le1`. In how many ways Tennessee can be colored with at most
one of the colors "red", "blue", "green"?

```{figure} ../img/combinatorics/tennessee-counting.jpg
:align: center
:name: fig-combinatorics-tennessee-counting
Counting Tennessee colors
```
````

(sec-combinatorics-colorings)=
## Counting graph colorings

Interestingly tensor networks can be used to obtain algebraic descriptions of counting problems. 
This has mostly theoretical value, as it provides another way of analyzing this kind of problems. 
We will give a few examples.

Roger Penrose introduced the graphical language for tensor networks (he called them 
_abstract tensor systems_) in {cite}`penrose71`. As an motivating application he gives counting 
edge colorings of $3$-regular planar graphs. We will revisit this example here.


Let $G=(N,E)$ be a 
[$3$-regular](https://en.wikipedia.org/wiki/Regular_graph)
[planar](https://en.wikipedia.org/wiki/Planar_graph)
[multigraph](https://en.wikipedia.org/wiki/Multigraph).
This means, that $G$ might contain multiple edges connecting same nodes. Furthermore, every node 
is incident on exactly $3$ edges and there exists a crossing-free embedding of $G$ into $\R^2$.
The minimal example looks like:

```{figure} ../img/combinatorics/penrose-minimal.svg
:align: center
:height: 150em
:name: fig-combinatorics-penrose-minimal
Minimal $3$-regular graph
```

We are looking for all edge colorings with $3$ colors $f:E\rightarrow\{1,2,3\}$ such that incident 
edges are mapped to different colors. The minimal example can be colored in six different ways:

```{figure} ../img/combinatorics/penrose-coloring.svg
:align: center
:height: 150em
:name: fig-combinatorics-penrose-coloring
Minimal edge coloring
```

This value can also be obtained by interpreting the graph as tensor network.
Each node will be replaced by a $\varepsilon$-tensor with $3$ indices, every index of dimension $3$.
Each edge is representing the respective contraction of incident tensors. Thus, the minimal 
example translates into the following tensor network.

````{prf:example}
:label: ex-combinatorics-penrose

```{figure} ../img/combinatorics/penrose-network.svg
:align: center
:height: 150em
:name: fig-combinatorics-penrose-network
Edge coloring tensor network
```

This contraction has the following value.

$$
\sum\limits_{i=1}^3\sum\limits_{j=1}^3\sum\limits_{k=1}^3\varepsilon_{ijk}\varepsilon_{ijk}
= \sum\limits_{i=1}^3\sum\limits_{\substack{j=1 \\ j\neq i}}^3\sum\limits_{\substack{k=1 \\ k\neq i,j}}^3 1
= 6
$$
````

Indeed, {cite}`penrose71` contains the following theorem.

```{prf:theorem}
:label: thm-combinatorics-penrose
The number of all edge colorings with $3$ colors of a planar $3$-regular multigraph is 
obtained by replacing each node with an order-$3$ $\varepsilon$-tensor, interpreting
each edge as a contraction and then contracting the resulting tensor network.
```

```{prf:proof}
We leave it with explaining plausibility.

The construction is valid, because the graph is $3$-regular. Having an $\varepsilon$-tensor 
located in a node enforces the coloring to be proper in the sense that incident edges 
(on that node) are colored differently, otherwise $\varepsilon_{ijk}=0$. 
This means that every configuration of the indices with values from $1$ to $3$ represents 
on one hand a valid and distinct coloring and contributes on the other hand
a $+1$ or a $-1$ to the sum.

For planar graphs the contribution is always $1$, what proves the theorem. 
{cite}`penrose71` arguments: 

> Finally we have to check that each non-zero term contributes precisely the value +1.
> It is at this point that the planarity of the graph enters. There are various ways of 
> seeing that the sign comes out correctly here, but I have not been able to think of an 
> argument which can be presented in a nutshell, so I shall just omit it here.

So we will do and leave this final conclusion to the reader.
```

However, Roger Penrose gives an example that shows that planarity is indeed a necessary 
for the theorem.

````{prf:example}
:label: ex-combinatorics-penrose2
Consider the following graph $K_{3,3}$.

```{figure} ../img/combinatorics/penrose-k33.svg
:align: center
:height: 300em
:name: fig-combinatorics-penrose-k33
$K_{3,3}$
```

It is the complete [bipartite graph](https://en.wikipedia.org/wiki/Bipartite_graph)
with $3$ nodes in each partition. There are no nodes between the red nodes and between 
the blue nodes. But every red node is connected with every blue node. By the famous
[theorem of Kuratowski](https://en.wikipedia.org/wiki/Kuratowski%27s_theorem),
this graph is not planar. Now, consider to exchange $2$ nodes in the resulting tensor 
network.

```{figure} ../img/combinatorics/penrose-exchange.svg
:align: center
:height: 300em
:name: fig-combinatorics-penrose-exchange
$K_{3,3}$ with node swap
```

Let us assume we exchange the nodes marked by $1$ and $2$. The tensor network would be 
the same and the contraction has to yield the same result. However, $5$ of the 
involved epsilon tensors have changed, those representing the nodes $1$, $2$ and the blue 
nodes. However, the tensors behind $1$ and $2$ are changing in the same way.
One is zero exactly when the other is zero. In addition, if one changes the sign, the other 
will do as well. Hence, the value of the product of the two does not change at all, 
when going from the first tensor network to the second. In contrary, every blue node 
switches exactly to indices. For example, the upper blue node swaps the index $k$ with the 
index $r$. Therefore, when moving from the left tensor network to the right tensor network, 
every non-zero "blue" epsilon tensor will change the sign. Hence, if the product of the 
three is not zero, it will change the sign. This implies, that every non-zero summand of 
the contraction will change its sign. There is only one possibility, that both contractions 
can yield the same result while having opposite signs. The value has to be zero, which 
contradicts the fact that the $K_{3,3}$ is $3$-colorable in $12$ different ways, one of 
those being:

```{figure} ../img/combinatorics/penrose-k33-coloring.svg
:align: center
:height: 300em
:name: fig-combinatorics-penrose-k33-coloring
Edge-colored $K_{3,3}$
```
````

So far the initial motivation of the inventor of the graphical tensor network language. 
Actually, this coloring result is just a side product and the paper establishes further 
insights. Therefore, the usage of the Levi-Civita symbol. If we are just interested in 
edge coloring, we can obtain the counting value in a much simpler way. Instead of 
$\varepsilon$ we simply use an $\eta$-tensor that counts $1$ for all for all edge 
permutations.

```{math}
:label: eqn-combinatorics-eta-tensor
\eta_{i_1\ldots i_q} \def 
    \begin{cases}
        1,\quad\,i_s\neq i_t,\,1\le s<t\le q, \\
        0,\quad\text{otherwise}. 
    \end{cases}
```

Obviously, $\eta_{i_1\ldots i_q} = \varepsilon_{i_1\ldots i_q}^2$ and we have not to 
think about what would happen in the case of $-1$. This allows to formulate another 
edge coloring theorem.

```{prf:theorem}
:label: thm-combinatorics-penrose2
Let $G=(N,E)$ be a multigraph. We assign a tensor network to $G$ as follows. Each node 
will be replaced by an $\eta$-tensor with as many indices as incident edges. Each edge will 
be interpreted as contraction along the related index. Every index of the tensor network 
shall have the same dimension $c$. Then the number of edge $c$-colorings is exactly the 
value of the contraction of the tensor network.
```

```{prf:proof}
Similar to the argument of {prf:ref}`thm-combinatorics-penrose`, a node being represented 
by $\eta_{i_1\ldots i_q}$ ensures, that a colorings with incident edges of same color do 
not count (factor zero), whereas colorings with incident edges having pairwise a different 
color contribute a $1$ (all $\eta$-tensors are $1$ for respective indices). Edges being 
represented by contractions ensures that an edge is colored the same, seen from both ends
(the index is the same). Every index being of dimension $c$ means, that the indices are 
running from $1$ to $c$, hence $c$ colors are used.
```

Let us conclude the part on edge colorings with an example.

````{prf:example}
:label: ex-combinatorics-penrose3
We consider the following graph, respectively tensor network with named indices.

```{figure} ../img/combinatorics/penrose-example.svg
:align: center
:height: 150em
:name: fig-combinatorics-penrose-example
Edge-coloring example graph
```

Following {prf:ref}`thm-combinatorics-penrose2`, if we are interested in counting all 
edge colorings with $c$ colors, we have to calculate the following tensor contraction.

$$
\sum\limits_{i=1}^c
\sum\limits_{j=1}^c
\sum\limits_{k=1}^c
\sum\limits_{p=1}^c
\eta_{ij}\eta_{ik}\eta_{jkp}\underbrace{\eta_p}_{=1}
=
\sum\limits_{i=1}^c
\sum\limits_{j=1}^c
\sum\limits_{k=1}^c
\sum\limits_{p=1}^c
\eta_{ij}\eta_{ik}\eta_{jkp}
$$

Let us consider the cases $c=1,2,3$.

- $c=1,2:$ According to the 
    [pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle)
    we obtain $\eta_{jkp}=0$ for $j,k,p\in\{1(,2)\}$. Hence, the contraction value is $0$. 
    This means, that there is no valid coloring with $1$ or $2$ colors.

- $c=3:$ Before we enumerate all combinations of index values, we can reduce them. We will 
    add all values $\eta_{ij}\eta_{ik}\eta_{jkp}\neq 0$. This implies 
    $i\neq j,\,i\neq k,\,j\neq k,\,j\neq p,\,k\neq p$. Hence, every combination sees different 
    values for $j$ and $k$ and assigns the missing third value to $i=p$. Hence, the number of 
    $3$-colorings equals the number of permutations of the $3$ colors which is $6$.

What we have done here, is actually the same as enumerating all possible colorings directly. 
Assigning values to indices is and checking whether all $\eta$-tensors are $1$ is exactly the 
same as assigning colors to edges and checking if the coloring is valid in all nodes.
````

{cite}`bridgeman17` analyzes node colorings in a similar way. A node coloring for a graph 
$G=(N,E)$ with $c$ colors is a map $f:N\rightarrow\{1,\ldots,c\}$ such that adjacent nodes 
are mapped to different colors. Obviously it suffices to consider simple graphs without 
multiple edges, since multiple edges would just add redundant information. We will introduce 
the construction with the same example, that had concluded the previous section.

````{prf:example}
:label: ex-combinatorics-bridge
Consider this graph.

```{figure} ../img/combinatorics/bridge-example.svg
:align: center
:height: 150em
:name: fig-combinatorics-bridge-example
Node-coloring example graph
```

We will use a generalize version of the Kronecker delta.

```{math}
:label: eqn-combinatorics-bridge-kronecker
\delta_{i_1\ldots i_q} \def
    \begin{cases}
        1,\quad i_1=i_2=\ldots =i_q\,, \\
        0,\quad\text{otherwise}. 
    \end{cases}
```

Again, we assign a tensor network to the graph by replacing nodes with tensors and edges 
with contractions. This time, nodes will be replaced by the generalized Kronecker tensor
(with as many indices as incident edges). Edges will be subdiveded by an additional node 
that will be transformed into an $\eta$-tensor. The example graph will be transformed 
into this tensor network.

```{figure} ../img/combinatorics/bridge-tensors.svg
:align: center
:height: 150em
:name: fig-combinatorics-bridge-tensors
Node-coloring counting tensor network
```

Again, all indices are of dimension $c$ if we are counting $c$-colorings. We leave it to 
the reader to confirm that the example graph requires at least $3$ colors and is 
$3$-colorable in $12$ different ways.
````

Using this construction, we are able to count all valid node colorings.

```{prf:theorem}
:label: thm-combinatorics-bridge
Let $G=(N,E)$ be a graph. We assign a tensor network as described in 
{prf:ref}`ex-combinatorics-bridge`. Every index of the tensor network shall have the same 
dimension $c$. Then the number of node $c$-colorings is exactly the value of the contraction 
of the tensor network.
```

```{prf:proof}
The reasoning is quite simple. Each index represents the color the connected node is colored 
with. The $\delta$ assigned to the node ensures that all connected indices represent the same 
color. The $\eta$ assigned to an edge ensures, that the nodes connected by this edge are mapped 
to different colors.
```