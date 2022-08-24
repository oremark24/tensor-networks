(ch-quantum)=
# Quantum Computing

> _Mathematics is a game played according to certain simple rules with meaningless 
> marks on paper._
> David Hilbert

To understand _Quantum Computing_ we need to know its realm and language. We study both first,
_Hilbert spaces_ and _Dirac notation_.

## Hilbert spaces

Hilbert spaces are attractive for physicists because they offer the possibility to incorporate
infinitesimal changes into theories, enabling usage of derivatives, Taylor expansion et cetera.
This founded by two properties of Hilbert spaces. They allow to measure distances, and ensure that
all limit points (based on distances) belong to the space. How to measure distances? This is
done through inner products, the mathematical model of angulometer and ruler for vectors. Note,
that we consider only vector spaces over $\C$ in this chapter, as this is used all the time in
Quantum Computing.

````{prf:definition} Inner product
:label: def-quantum-ip
An ___inner product___ on vector space $V$ over $\C$ is a function

$$
\ip{\cdot}{\cdot}:V\times V\rightarrow\C,\,(u,v)\mapsto\ip{u}{v}
$$ 

with following properties:

```{admonition} Additivity in second slot
$\ip{u}{v+v'}=\ip{u}{v}+\ip{u}{v'}$ for all $u,v,v'\in V$
```

```{admonition} Homogeneity in second slot
$\ip{u}{\lambda v}=\lambda\ip{u}{v}$ for all $u,v\in V$ and all $\lambda\in\C$
```

```{admonition} Conjugate symmetry
$\ip{u}{v}=\overline{\ip{v}{u}}$ for all $u,v\in V$
```

```{admonition} Positive definiteness
$\ip{v}{v}\ge 0$ for all $v\in V$ and $\ip{v}{v}=0$ if and only if $v=0$
```

A vector space equipped with an inner product is called ___inner product space___.
````

We will mostly rely on the standard inner product which we give as an example now.

````{prf:example}
:label: ex-quantum-standard-ip
For $\C^n$ with basis $\{e_1,\ldots,e_n\}$ we define the standard inner product 
for $v,w\in\C^n$ with $v=v^ie_i$ and $w=w^ie_i$ as:

```{math}
:label: eq-quantum-standard-ip
\ip{v}{w}\def\sum\limits_{i=1}^n\overline{v^i}w^i\,.
```
````

The question occurs, why did we define additivity and homogeneity only for the first variable? 
The answer is, for the second one, we can conclude similar characteristics from first slot 
properties and conjugate symmetry.

````{prf:observation} First slot properties
:label: obs-quantum-ip
An inner product is additive and conjugate homogenious in second slot.

```{admonition} Additivity in first slot
$\ip{u+u'}{v}=\ip{u}{v}+\ip{u'}{v}$ for all $u,u',v\in V$
```

```{admonition} Conjugate homogeneity in first slot
$\ip{\lambda u}{v}=\overline{\lambda}\ip{u}{v}$ for all $u,v\in V$ and all $\lambda\in\C$
```
````

```{prf:proof}
For $u,u',v\in V$ and $\lambda\in\C$ we have 

$$
\ip{u+u'}{v} 
&= \overline{\ip{v}{u+u'}} \\
&= \overline{\ip{v}{u}+\ip{v}{u'}} \\
&= \overline{\ip{v}{u}}+\overline{\ip{v}{u'}} \\
&= \ip{u}{v}+\ip{u'}{v}
$$ 

as well as 

$$
\ip{\lambda u}{v}
&= \overline{\ip{v}{\lambda u}} \\
&= \overline{\lambda\ip{v}{u}} \\
&= \overline{\lambda}\overline{\ip{v}{u}} \\
&= \overline{\lambda}\ip{u}{v}.
$$
```

The reason for having _conjugate_ homogeneity in the first slot is that we want
to obtain a real number, when applying the inner product of a vector to itself.
This is necessary to define a feasible length measure based on the inner product.

```{prf:observation} Self-product is real
:label: obs-quantum-ip-real
For each $v\in V$ the inner product $\ip{v}{v}$ is real.
```

```{prf:proof}
Conjugate symmetry ({prf:ref}`def-quantum-ip`) yields

$$
\ip{v}{v} = \overline{\ip{v}{v}}\quad\Rightarrow\quad\ip{v}{v}\in\R\,.
$$
```

Together {prf:ref}`def-quantum-ip` and {prf:ref}`obs-quantum-ip` imply, that the 
inner product is kind of linear in the first variable and linear in the second 
variable. Hence, fixing the first variable yields a linear map in the second variable.

```{prf:observation} Inner product as linear functional
:label: obs-quantum-ip-lin
For a fixed $u\in V$ the map

$$
\ip{u}{\cdot}:V\rightarrow\C,\,v\mapsto\ip{u}{v}
$$

is a linear functional (linear map). For fixed bases 
$\{e_1,\ldots,e_n\}\subseteq V$ and $\{1\}\subseteq\C$
the matrix representation of $\ip{u}{\cdot}$ is

$$
\begin{bmatrix}
  \ip{u}{e_1}\;\ip{u}{e_2}\;\cdots\;\ip{u}{e_n}
\end{bmatrix}
\,.
$$
```

```{prf:proof}
The linearity follows directly from {prf:ref}`def-quantum-ip`. 
For $v=v^ie_i$ we derive the matrix representation as usual:

$$
\ip{u}{v}
&= \ip{u}{v^ie_i} \\
&= v^i\ip{u}{e_i} \\
&= \begin{bmatrix}
  \ip{u}{e_1}\;\cdots\;\ip{u}{e_n}
\end{bmatrix}
\begin{bmatrix}
  v^1 \\
  \vdots \\
  v^n
\end{bmatrix}
\,.
$$
```

Based on the inner product we can define the length measure.

```{prf:definition} Norm
:label: def-quantum-norm
For $v\in V$ we define the ___norm___ of v, denoted $\norm{v}$, by

$$
\norm{v}\def\sqrt{\ip{v}{v}}\,.
$$
```

Directly from this definition 
we obtain basic norm properties.

````{prf:observation} Basic norm properties
:label: obs-quantum-norm-props

```{admonition} Well-definied
The norm is well-defined (not ambiguous) and $\norm{v}\in\R$ for all $v\in V$.
```

```{admonition} Absolute homogeneity
$\norm{\lambda v}=\abs{\lambda}\,\norm{u}$ for all $v\in V$ and all $\lambda\in\C$
```

```{admonition} Positive definiteness
$\norm{v}\ge 0$ for all $v\in V$ and $\norm{v}=0$ if and only if $v=0$
```
````

````{prf:proof}
The norm is well-defined and real because of {prf:ref}`obs-quantum-ip-real` 
and {prf:ref}`def-quantum-ip` (positive definiteness). The last reference also
includes positive definiteness of the norm.

Absolute homogeneity holds because of

```{math}
:label: eqn-quantum-norm-homogeneity
\norm{\lambda v}^2
&= \ip{\lambda v}{\lambda v} \\
&= \lambda \ip{\lambda v}{v} \\
&= \lambda\overline{\lambda}\ip{v}{v} \\
&= \abs{\lambda}^2\norm{v}^2\,.
```
````

Equation {eq}`eqn-quantum-norm-homogeneity` shows a very common technique when
calculating with norms: working with squares and translating terms to inner products.

With help of the inner product we can also introduce the crucial definition of
_orthogonality_.

```{prf:definition} Orthogonality
:label: def-quantum-orthogonality
Two vectors $u,v\in V$ with $\ip{u}{v}=0$ are called ___orthogonal___.
```

The origin of the word _orthogonal_ is the Greek word _orthogonios_, which
means right-angled. Already the ancient Greeks have dealt with "right angles",
that are also the basis of a classic theorem of that era.

```{prf:observation} Theorem of Pythagoras
:label: obs-quantum-pythagoras
Orthogonal vectors $u,v\in V$ fulfill the equation

$$
\norm{u+v}^2=\norm{u}^2+\norm{v}^2 \,.
$$
```

```{prf:proof}
We have

$$
\norm{u+v}^2
&= \ip{u+v}{u+v} \\
&= \ip{u}{u} + \underbrace{\ip{u}{v}}_{=0} + \underbrace{\ip{v}{u}}_{=0} + \ip{v}{v} \\
&= \norm{u}^2+\norm{v}^2 \,.
$$
```

The term _norm_ is well defined even in more general terms (if not derived from 
inner product). Absolute homogeneity and positive definiteness are defining properties 
then. They are complemented and completed by a third defining property. That is
the _Triangle Inequality_

```{math}
:label: eq-quantum-triangle-inequality
\norm{u+v}\le\norm{u}+\norm{v} \,,
```

that has to hold for all $u,v\in V$. Similar to the other two properties, we will
derive this automatically from the norm definition based on the inner product. But
we have to work a bit to achieve this. We start with another famous inequality.

```{prf:observation} Cauchy-Schwarz Inequality
:label: obs-quantum-cauchy-schwarz
For $u,v\in V$ we have

$$
\abs{\ip{u}{v}}\le\norm{u}\norm{v}\,.
$$

The inequality becomes equal if and only if $v=a\cdot u$ or $u=a\cdot v$ for some $a\in\C$.
```

```{prf:proof}
Obviously, the assertion is fulfilled for $v=0$. Now, assume $v\neq 0$.
We can write $u$ as

$$
u= \underbrace{u-\frac{\ip{u}{v}}{\norm{v}^2}v}_{\fed w}
 + \underbrace{\frac{\ip{u}{v}}{\norm{v}^2}v}_{\fed w'} \,.
$$

This is also called _orthogonal decomposition_, since $w$ and $w'$ are orthogonal:

$$
\ip{w}{w'}
&= \IP{u-\frac{\ip{u}{v}}{\norm{v}^2}v}{\frac{\ip{u}{v}}{\norm{v}^2}v} \\
&= \frac{\ip{u}{v}}{\norm{v}^2}\ip{u}{v}-\frac{\ip{u}{v}^2}{\norm{v}^4}\ip{v}{v} \\
&= \frac{\ip{u}{v}^2}{\norm{v}^2}-\frac{\ip{u}{v}^2}{\norm{v}^2} \\
&= 0 \,.
$$

Hence, we can apply the Theorem of Pythagoras {prf:ref}`obs-quantum-pythagoras`
to the orthogonal decomposition of $u$.

$$
\norm{u}^2
&= \norm{w}^2+\norm{w'}^2 \\
&= \norm{w}^2 + \Norm{\frac{\ip{u}{v}}{\norm{v}^2}v}^2 \\
&= \norm{w}^2 + \frac{\abs{\ip{u}{v}}^2}{\norm{v}^4}\norm{v}^2 \\
&= \underbrace{\norm{w}^2}_{\ge 0} + \frac{\abs{\ip{u}{v}}^2}{\norm{v}^2} \\
&\ge \frac{\abs{\ip{u}{v}}^2}{\norm{v}^2} \,.
$$

Multiplying this inequality by $\norm{v}^2$ and taking square roots yields the
Cauchy-Schwarz Inequality. Furthermore, we obtain that $\norm{w}$ determines
how strict the inequality is, having equality if and only if $\norm{w}=0$.
This is the case if and only if $w=0$ or, by taking into account how $w$ is
defined, $u=a\cdot v$ with $a=\ip{u}{v}/\norm{v}^2$. This completes the proof.
```

This provides us with all the tools to proof the Triangle Inequality 
{eq}`eq-quantum-triangle-inequality`.

```{prf:observation} Triangle Inequality
:label: obs-quantum-triangle-inequality
For $u,v\in V$ we have

$$
\norm{u+v}\le\norm{u}+\norm{v} \,.
$$

The inequality becomes equal if and only if $v=a\cdot u$ or $u=a\cdot v$ for some $a\ge 0$.
```

```{prf:proof}
We have

$$
\norm{u+v}^2
&= \ip{u+v}{u+v} \\
&= \ip{u}{u} + \ip{u}{v} + \ip{v}{u} + \ip{v}{v} \\
&= \ip{u}{u} + \ip{u}{v} + \overline{\ip{u}{v}} + \ip{v}{v} \\
&= \norm{u}^2 + \norm{v}^2 + 2\re{\ip{u}{v}} \\
&\underbrace{\le}_{\abs{x+iy}=\sqrt{x^2+y^2}\ge\abs{x}} \norm{u}^2 + \norm{v}^2 + 2\abs{\ip{u}{v}} \\
&\underbrace{\le}_{\text{Cauchy-Schwarz}} \norm{u}^2 + \norm{v}^2 + 2\norm{u}\norm{v} \\
&= (\norm{u}+\norm{v})^2 \,.
$$

Taking square roots yields the Triangle Inequality. It is equal if and only if both inequalities in
the equation chain are strict, that is if and only if $\re{\ip{u}{v}}=\norm{u}\norm{v}$. If we have for
example $u=a\cdot v$ for $a\ge 0$, then

$$
\re{\ip{u}{v}}=\re{\ip{av}{v}}
=a\re{\underbrace{\ip{v}{v}}_{=\norm{v}^2\in\R}}=a\norm{v}^2
=\norm{av}\norm{v}=\norm{u}\norm{v}\,.
$$

Conversely, $\re{\ip{u}{v}}=\norm{u}\norm{v}$ implies the Cauchy-Schwarz Inequality being
fulfilled with equality. Hence, we have for example $u=a\cdot v$ with an $a\in\C$.
We put this into the equation:

$$
\re{\ip{av}{v}}=\norm{av}\norm{v}\,.
$$

We transform the left-hand side

$$
\re{\ip{av}{v}}=\re{a\ip{v}{v}}=\re{a\underbrace{\norm{v}^2}_{\in\R}}=\re{a}\norm{v}^2
$$

and the right-hand side

$$
\norm{av}\norm{v}=\abs{a}\norm{v}^2
$$

to obtain

$$
\re{a}=\abs{a} \,.
$$

This is only possible for $a\in\R$ and $a\ge 0$. The proof is complete.
```

Now, {prf:ref}`obs-quantum-norm-props` together with
{prf:ref}`obs-quantum-triangle-inequality` ensure that the norm defined 
in this way is indeed fulfilling common 
[norm properties](https://en.wikipedia.org/wiki/Norm_(mathematics)).
In addition to the notion of orthogonality, the norm is inducing the
notion of the _length_ of a vector $v$ by $\norm{v}$ and the notion of
_distance_ of vectors respectively _metric_ on the space $v$ and $w$ by $\norm{v-w}$. 
With help of distance, the notion of convergence, limits and limit points 
within an inner product space can be defined the usual way.
We will not go into details here, but give the following important
definition (complemented by an explanatory example).

```{prf:definition} Hilbert space
:label: def-quantum-hilbert-space
A ___Hilbert space___ $H$ is is an inner product space that contains
all its limit points.
```

To justify that the explicit second statement "contains all its limit points"
is otherwise not guaranteed, we give an example of an inner product space
that is no Hilbert space.

````{prf:example} Non-Hilbert space
:label: ex-quantum-no-hilbert-space
We consider the space $C[0,1]$ of all continuous functions $f:[0,1]\rightarrow\R$
equipped with an addition

$$
(f+g)(x)\def f(x)+g(x)
$$

for all $f,g\in C[0,1]$, and a scalar multiplication

$$
(a\cdot f)(x)\def a\cdot f(x)
$$

for all $a\in\R,\,f\in C[0,1]$. Then $C[0,1]$ together with both operations forms
a vector space. Furthermore an inner product can be defined in a similar way to
the standard inner product (multiply related elements and sum everything up):

```{math}
:label: eq-quantum-integral-ip
\ip{f}{g}\def\int\limits_{0}^{1}\overline{f(x)}g(x)dx \,.
```

The complex conjugation of $f(x)$ is actually not necessary since all considered functions
are real valued. We left it here to show similarity to equation / definition 
{eq}`eq-quantum-standard-ip` and to illustrate how complex valued functions would be treated.

As explained the distance between two functions $f$ and $g$ is measured by

$$
\norm{f-g}=\sqrt{\int\limits_{0}^{1}(f(x)-g(x))^2dx} \,.
$$

Now, consider the following series of functions $\{f_t\}_{t=1}^\infty\subseteq C[0,1]$ with

$$
f_t(x)\def
\begin{cases}
tx, & 0\le x<1/t \,, \\
1, & 1/t\le x\le 1 \,.
\end{cases}
$$

In the induced metric this series of functions converges towards the function

$$
f(x)\def
\begin{cases}
0, & x=0 \,, \\
1, & 0<x\le 1 \,.
\end{cases}
$$

Function $f$ is not continuous, thus $f\notin C[0,1]$. Hence, $C[0,1]$ equipped with
the aforementioned operations and inner product is not complete and therefore no
Hilbert space.
````

In this work we limit ourselves to finite dimensional spaces. This has also the implication
that we are on the safe side regards completeness as the following theorem provides.
We give it without a proof.

```{prf:theorem} Completeness
:label: thm-quantum-completeness
All finite dimensional inner product spaces are complete.
```

This means that in the following we can use the terms inner product space and 
Hilbert space interchangeably. In the next step we will feature sets of vectors that
are pairwise orthogonal and elementwise of norm one.

````{prf:definition} Orthonormal vectors
:label: thm-quantum-orthobasis
Let $H$ be a Hilbert space. A set of vectors $\{v_1,\ldots,v_m\}\subseteq H$
is called ___orthonormal___ if

```{math}
:label: eq-quantum-orthobasis
\ip{v_i}{v_j}=\delta_{ij}=
\begin{cases}
0, & i\neq j \,,\\
1, & i=j \,. 
\end{cases}
```
````

Interestingly, vectors being orthonormal ensures already that the vectors
are linearly independent. Consequently, orthonormality of a set of vectors
is an even stronger property that linear independence.

```{prf:observation} Orthonormal $=$ linear independent
:label: obs-quantum-ortho-independence
In a Hilbert space every orthonormal set of vectors is linearly independent.
```

```{prf:proof}
Contrary to the assertion, suppose $\{v_1,\ldots,v_m\}$ is an orthonormal set of vectors
that is not linearly independent. Then there is a non-trivial linear combination

$$
a^iv_i=0
$$

with some $a^j\neq 0$. Positive definiteness of the norm ({prf:ref}`obs-quantum-norm-props`)
tells us, that

$$
\norm{a^iv_i}^2=0\,.
$$

We apply the theorm of Pythagoras ({prf:ref}`obs-quantum-pythagoras`) and absolute homogeneity
({prf:ref}`obs-quantum-norm-props`) of the norm to obtain

$$
0 &= \Norm{\sum\limits_{i=1}^ma^iv_i}^2 \\
&= \sum\limits_{i=1}^m\norm{a^iv_i}^2 \\
&= \sum\limits_{i=1}^m|a^i|^2\,{\underbrace{\norm{v_i}}_{=1}}^2 \\
&= \sum\limits_{i=1}^m|a^i|^2 \\
&\ge |a^j|^2 \\
&> 0 \,.
$$

This is a contradiction.
```

Do we always find a orthonormal vectors? Let's see.

```{prf:algorithm} Gram-Schmidt procedure
:label: alg-quantum-gram-schmidt
Let $H$ be a Hilbert space and $\{v_1,\ldots,v_m\}\subseteq H$ a linear independent set of vectors. 
We compute vectors $w_1,\ldots,w_m$ as follows: 

$$
w_1&\def\frac{v_1}{\norm{v_1}} \,, \\
w_2&\def\frac{v_2-\ip{w_1}{v_2}\,w_1}{\norm{v_2-\ip{w_1}{v_2}\,w_1}} \,, \\
w_3&\def\frac{v_3-\ip{w_1}{v_3}\,w_1-\ip{w_2}{v_3}\,w_2}{\norm{v_3-\ip{w_1}{v_3}\,w_1-\ip{w_2}{v_3}\,w_2}} \,, \\
&\qquad\qquad\vdots \\
w_m&\def\frac{v_m-\sum\limits_{i=1}^{m-1}\ip{w_i}{v_m}\,w_i}{\Norm{v_m-\sum\limits_{i=1}^{m-1}\ip{w_i}{v_m}\,w_i}} \,.
$$
```

This procedure will orthogonalize and normalize the basis.

```{prf:theorem} Gram-Schmidt procedure
:label: thm-quantum-gram-schmidt
Under the notions of {prf:ref}`alg-quantum-gram-schmidt` the set
$\{w_1,\ldots,w_m\}$ is orthonormal.
```

````{prf:proof}
We will show by induction that after each step the set $\{w_1,\ldots,w_j\}$ is orthonormal, 
$j=1,\ldots,m$. It will be helpful to even strengthen the induction
hypothesis to have a stronger assumption available. We will assert
$\span\{v_1,\ldots,v_j\}=\span\{w_1,\ldots,w_j\}$ for $j=1,\ldots,m$ in addition.

For the base case $j=1$, we have $v_1$ being a basis element, which implies $v_1\neq 0$ and
therefore $\norm{v_1}>0$. Hence, we do not divide by zero. This means, $w_1$ is correctly defined
and a multiple of $v_1$ with norm equals one. This implies the induction hypothesis.

Now, let us assume, the assertion is already proven for $1,\ldots,j-1$. Because 
the set $\{v_1,\ldots,v_j\}$ is linearly independent, we have

$$
v_j\notin\span\{v_1,\ldots,v_{j-1}\}=\span\{w_1,\ldots,w_{j-1}\} \,.
$$ 

This implies

$$
v_j-\sum\limits_{i=1}^{j-1}\ip{w_i}{v_m}\,w_i\neq 0 \,.
$$

Hence, we are not dividing by zero in
the definition of $w_j$. Dividing a vector by its norm yields a vector of norm one, in particular
$\norm{w_j}=1$. Next we will check orthogonality of $w_j$ to already defined vectors $w_k$. 
Let $1\le k<j$ and use already proven orthonormality of $w_1,\ldots,w_{j-1}$.

$$
\ip{w_k}{w_j}
&=\IP{w_k}{v_j-\sum\limits_{i=1}^{j-1}\ip{w_i}{v_j}\,w_i \Big{/} \Norm{v_j-\sum\limits_{i=1}^{j-1}\ip{w_i}{v_j}\,w_i}} \\
&=\frac{\ip{w_k}{v_j}-\ip{w_k}{v_j}}{\Norm{v_j-\sum\limits_{i=1}^{j-1}\ip{w_i}{v_j}\,w_i}} \\
&=0\,.
$$

Thus, $\{w_1,\ldots,w_j\}$ is an orthonormal set. By definition we have 

$$
w_j\in\span\{w_1,\ldots,w_{j-1},v_j\}=\span\{v_1,\ldots,v_j\} \,,
$$

which shows that

```{math}
:label: eq-quantum-span-containment
\span\{w_1,\ldots,w_j\}\subseteq\span\{v_1,\ldots,v_j\} \,.
```

From {prf:ref}`obs-quantum-ortho-independence` we know, that the set
$\{w_1,\ldots,w_j\}$ is not only orthonormal but also linearly independent.
This gives

$$
\dim(\span\{w_1,\ldots,w_j\})=\dim(\span\{v_1,\ldots,v_j\})
$$

and together with {eq}`eq-quantum-span-containment`

$$
\span\{w_1,\ldots,w_j\}=\span\{v_1,\ldots,v_j\} \,.
$$

This completes the induction step and altogether the proof.
````

```{prf:corollary} Orthonormal basis
:label: cor-quantum-orthobasis
Every (finite-dimensional) Hilbert space has an orthonormal basis.
```

```{prf:proof}
Every finite-dimensional space has a basis. This follows
from the [Steinitz exchange lemma](https://en.wikipedia.org/wiki/Steinitz_exchange_lemma).
We apply the Gram-Schmidt procedure {prf:ref}`alg-quantum-gram-schmidt` to a basis.
The assertion follows from {prf:ref}`thm-quantum-gram-schmidt`.
```

The coefficients of a vector regarding an orthonormal basis can be computed by
projecting the vector to the respective basis elements.

````{prf:observation} Representation in orthonormal basis
:label: obs-quantum-orthocoeffis
Let $\{e_1,\ldots,e_n\}$ be an orthonormal basis of a Hilbert space $H$ and $v\in H$ an
arbitrary vector. Then

```{math}
:label: eq-quantum-orthocoeffis
v=\ip{e_1}{v}\,e_1+\cdots+\ip{e_n}{v}\,e_n
```

and

```{math}
:label: eq-quantum-orthonorm
\norm{v}^2=|\ip{e_1}{v}|^2+\cdots+|\ip{e_n}{v}|^2 \,.
```
````

```{prf:proof}
Equation {eq}`eq-quantum-orthocoeffis`is obtained from

$$
\ip{e_j}{v}=\ip{e_j}{v^ie_i}=v^i\underbrace{\ip{e_j}{e_i}}_{=\delta_{ji}}=v^i\,.
$$

Equation {eq}`eq-quantum-orthonorm` is coming from equation {eq}`eq-quantum-orthocoeffis` and

$$
\norm{v}^2=\ip{v^je_j}{v^ie_i}=\overline{v^j}v^i\underbrace{\ip{e_j}{e_i}}_{=\delta_{ji}}
=\sum\limits_{i=1}^n|v^i|^2 \,.
$$
```

## Dirac notation

In Physics a special notation for vectors and dual vectors is common, the _Dirac notation_ also
called _Bra-Ket notation_. Articles on Quantum computing rely
almost exclusively on this notation when using vector-based formulas. We will it introduce now.
The justification of this notation is based on the _Riesz Representation Theorem_
({prf:ref}`thm-quantum-riesz`).

In {prf:ref}`obs-quantum-ip-lin` we have already seen, that the inner product is also defining
a linear functional. The Riesz Representation Theorem states the converse.
Every linear functional on a Hilbert space can be represented by a unique vector and the inner 
product.

```{prf:theorem} Riesz Representation Theorem
:label: thm-quantum-riesz
Let $H$ be a (finite-dimensional) Hilbert space and $\varphi:H\rightarrow\C$ a linear functional
on $H$. Then there is a unique vector $u\in H$ such that

$$
\varphi(v)=\ip{u}{v}
$$

for all $v\in H$.
```

```{prf:proof}
According {prf:ref}`cor-quantum-orthobasis` let $\{e_1,\ldots,e_n\}$ be an orthonormal basis of $H$.
Then we have for every $v\in H$ according to equation {eq}`eq-quantum-orthocoeffis`

$$
\varphi(v) &= \varphi(\ip{e_1}{v}\,e_1+\cdots+\ip{e_n}{v}\,e_n) \\
&= \ip{e_1}{v}\,\varphi(e_1)+\cdots+\ip{e_n}{v}\,\varphi(e_n) \\
&= \ip{\underbrace{\overline{\varphi(e_1)}\,e_1+\cdots+\overline{\varphi(e_n)}\,e_n}_{\fed u}}{v} \,.
$$

Hence, we have already found a $u$ with desired behavior. Now, we will show that there is only one such 
vector $u\in H$. Let us assume, there are $u_1,u_2\in H$, such that

$$
\varphi(v)=\ip{u_1}{v}=\ip{u_2}{v}
$$

for all $v\in H$. Then

$$
0=\ip{u_1}{v}-\ip{u_2}{v}=\ip{u_1-u_2}{v}
$$

for all $v\in H$. Setting $v\def u_1-u_2$ this yields $\norm{u_1-u_2}^2=0$ and therefore $u_1-u_2=0$.
In other words, $u_1=u_2$, completing the proof.
```

This means, considering an inner product $\ip{u}{v}$, we can interpret the right hand side $v$ as
vector of a Hilbert space $H$ and the left hand side $u$ as a dual vector of $H^\ast$. This is exactly,
what the Dirac notation does. 

```{prf:definition} Dirac notation
:label: def-quantum-dirac
Let $H$ be a Hilbert space. We take the inner product $\ip{u}{v}$ apart, 
deriving the so called 

* ___bra___ $\bra{u}\in H^\ast$ 

and the so called 

* ___ket___ $\ket{v}\in H$. 

The inner product is then simply represented by the

* ___braket___, the product of bra and ket $\bra{u}\ket{v}\def\ip{u}{v}$.

If vectors are indexed, it is common to leave out the symbol and write only the index instead.
Let for example $B\def\{e_1,\ldots,e_n\}\subseteq H$ and $B^\ast\def\{e^1,\ldots,e^n\}\subseteq H^\ast$ 
be orthonormal bases. We could denote them by 

* $B=\{\ket{1},\ldots,\ket{n}\}$ and $B^\ast=\{\bra{1},\ldots,\bra{n}\}$.
```

We will now stick to the Dirac notation. Some care is needed, if considering coefficients 
of $u$ in an orthonormal basis of $H$, compared to
the coefficients of the by $u$ represented linear functional in the related dual basis of $H^\ast$.
The reason is, that equation {eq}`eq-quantum-orthocoeffis` looks slightly differently.

```{prf:observation} Bra coefficients
:label: obs-quantum-bracoeffis
Let $H$ be a (finite-dimensional) Hilbert space with basis $\{e_1,\ldots,e_n\}$.
Let $\bra{u}\in H^\ast$ be a dual vector with representation $u\in H$ according to
{prf:ref}`thm-quantum-riesz`. The dual vector $\bra{u}\in H^\ast$ can be represented in the dual basis
$\{\bra{1}=e^1,\ldots,\bra{n}=e^n\}$ by 

$$
\bra{u}=\sum\limits_{i=1}^n\tilde{u}_i\bra{i}\,.
$$

On the other hand $u\in H$ can be expressed in basis $\{e_1,\ldots,e_n\}$ by

$$
u=u^ie_i\,.
$$

Then the coefficients relate by

$$
\tilde{u}_i=\overline{u^i},\,i=1,\ldots,n\,.
$$
```

```{prf:proof}
Using the inner product we have

$$
\ip{u}{e_j}=\ip{u^ie_i}{e_j}=\overline{u^i}\underbrace{\ip{e_i}{e_j}}_{=\delta_{ij}}=\overline{u^j}\,.
$$

Using Dirac notation (and applying linear functional $\bra{u}\in H^\ast$ to basis element $\ket{j}\in H$) 
we express the same value by

$$
\ip{u}{j}=\sum\limits_{i=1}^n\tilde{u}_i\underbrace{\ip{i}{j}}_{=\delta^i_j}=\tilde{u}_j\,.
$$
```

<!--
## Unitary Maps
-->

## Introduction to Quantum Computing

We continue with laying out the computing model of _Quantum Computations_.
We will do this in an abstract way by postulating the rules / axioms, that
Quantum Computations follow, but leaving out the deeper Physical justifications
based on Quantum Mechanics. We follow the path chosen in {cite}`nielsen00` and
stick to the same postulates.

Although following a different model, we can still make a comparision to classical computing since
similar parts have to be defined. First will be the system's state. In classical computing
this is current content of memory. Quantum Computing is completely modelled by means of
Linear Algebra. The state (current content of memory if you will) is given as a vector. The underlying
space is a Hilbert space.

```{prf:definition} State - Quantum Computing Postulate 1
:label: def-quantum-post1
The ___state space___ of a Quantum Computing system is a (finite dimensional)
Hilbert space $H$. The system's ___state___ before or after any computation step
is completely described by its ___state vector___, 
which is a unit vector in the state space:

$$
\ket{\psi}\in H\,,\,\norm{\ket{\psi}}=1\,.
$$
```

The atomic state is the _qubit_ (quantum bit). Whereas a classical bit can take one of two values,
the qubit is a unit vector spanned by two basis vectors.

```{prf:definition} Qubit
:label: def-quantum-qubit
A ___qubit___ $\ket{\psi}$ is a _quantum state_ in _state space_ $\ket{\psi}\in\C^2$. 
It is usually represented by a linear combination of an orthonormal basis
$\{\ket{0},\ket{1}\}\subseteq\C^2$.

The linear combination

$$
\ket{\psi}=a\ket{0}+b\ket{1}
$$

is called ___superposition___ and the basis $\{\ket{0},\ket{1}\}$ ___computational basis___.
```

Next we have to define how the computation actually takes place. In classical computing
a computation step is loading data into the processor and storing the processor's output
back into memory, which modifys the system's state. Again, in Quantum Computing we are
using the tool set of Linear Algebra. How do we modify the state, which is a vector? Of course
this will be done by a linear map that transform the current state into the next state. 
Quantum Computing requires the map to be unitary.

```{prf:definition} Evolution - Quantum Computing Postulate 2
:label: def-quantum-post2
A ___computation step___ during a Quantum Computing calculation is given by a
_unitary map_ on the state space $U\in L(H)$. The computation step
transforms the state $\ket{\psi}\in H$ before the computation into the state
$\ket{\psi'}\in H$ after the computation:

$$
\ket{\psi'}=U\ket{\psi}\,.
$$
```

We have already learned about quantum gates in {ref}`ch-tensors`. We reconsider the
Hadamard transformation {prf:ref}`exHadamard`.

```{prf:example} Hadamard gate
:label: ex-quantum-hadamard
The Hadamard transformation from {prf:ref}`exHadamard` can be written in Dirac notation as

$$
\tilde{H}=\ket{0}\bra{0}+\ket{0}\bra{1}+\ket{1}\bra{0}-\ket{1}\bra{1}\,.
$$

We need to scale

$$
H=\frac{1}{\sqrt{2}}\tilde{H}
$$

to obtain a unitary Hadamard transformation $H$. Indeed

$$
H^\ast H &= \frac{1}{2}\Big(\ket{0}\bra{0}+\ket{0}\bra{1}+\ket{1}\bra{0}-\ket{1}\bra{1}\Big)^2 \\
&= \frac{1}{2}\Big(
  \ket{0}\underbrace{\ip{0}{0}}_{=1}\bra{0}
  + \ket{0}\underbrace{\ip{0}{0}}_{=1}\bra{1}
  + \ket{0}\underbrace{\ip{0}{1}}_{=0}\bra{0}
  - \ket{0}\underbrace{\ip{0}{1}}_{=0}\bra{1} \\
  &\quad\; + \; \ket{0}\underbrace{\ip{1}{0}}_{=0}\bra{0}
  + \ket{0}\underbrace{\ip{1}{0}}_{=0}\bra{1}
  + \ket{0}\underbrace{\ip{1}{1}}_{=1}\bra{0}
  - \ket{0}\underbrace{\ip{1}{1}}_{=1}\bra{1} \\
  &\quad\; + \; \ket{1}\underbrace{\ip{0}{0}}_{=1}\bra{0}
  + \ket{1}\underbrace{\ip{0}{0}}_{=1}\bra{1}
  + \ket{1}\underbrace{\ip{0}{1}}_{=0}\bra{0}
  - \ket{1}\underbrace{\ip{0}{1}}_{=0}\bra{1} \\
  &\quad\; - \; \ket{1}\underbrace{\ip{1}{0}}_{=0}\bra{0}
  - \ket{1}\underbrace{\ip{1}{0}}_{=0}\bra{1}
  - \ket{1}\underbrace{\ip{1}{1}}_{=1}\bra{0}
  + \ket{1}\underbrace{\ip{1}{1}}_{=1}\bra{1}
  \Big) \\
&= \ket{0}\bra{0}+\ket{1}\bra{1} \\
&= I \,.
$$

Usually a qubit will be initialized with $\ket{\psi_0}=\ket{0}$. A computation step based on
Hadamard will lead to the follow-up state

$$
\ket{\psi_1} &= H\ket{\psi_0} \\
&= \frac{1}{\sqrt{2}}\Big(\ket{0}\bra{0}+\ket{0}\bra{1}+\ket{1}\bra{0}-\ket{1}\bra{1}\Big)\ket{0} \\
&= \frac{1}{\sqrt{2}}\Big(
  \ket{0}\underbrace{\ip{0}{0}}_{=1}
  +\ket{0}\underbrace{\ip{1}{0}}_{=0}
  +\ket{1}\underbrace{\ip{0}{0}}_{=1}
  -\ket{1}\underbrace{\ip{1}{0}}_{=0}
  \Big) \\
&= \frac{1}{\sqrt{2}}\ket{0}+\frac{1}{\sqrt{2}}\ket{1}\,.
$$
```

The third postulate deals with evaluating the result of the whole computation.
In respect of classical computing this corresponds to reading out relevant parts
of the memory to obtain the result. Switching back to Quantum Computing, the 
memory is represented by a unit vector in Hilbert space. How do we read out the relevant part?
Think of projecting this vector. Indeed, something similar will be done. The projections
are resembled by linear maps, the lengths of resulting vectors will be measured to provide
probabilities for the respective vector being the final result. As you see, Quantum measurement
involves drawing of lots.

````{prf:definition} Measurement - Quantum Computing Postulate 3
:label: def-quantum-post3
___Quantum Measurement___ is described by a collection of linear maps on the state space
$M_1,\ldots,M_m\in L(H)$ such that it fulfills the ___completeness condition___

```{math}
:label: eq-quantum-measurement
M^\ast_1M_1+\ldots+M^\ast_mM_m=I\,.
```

Input to a measurement operation is a (quantum computed) state vector $\ket{\psi}\in H$.
The measurement will randomly select one of $m$ events with probabilities

$$
p_i=\norm{M_i\ket{\psi}}^2\,,\,i=1,\ldots,m\,.
$$

The outcome is the state vector

$$
\frac{M_i\ket{\psi}}{\norm{M_i\ket{\psi}}}\in H
$$

with probability $p_i$ for $i=1,\ldots,m$.
````

We need to check whether the defined probabilities cover the full probability space.
This will be ensured by the completeness condition.

```{prf:observation} Well-definedness of Postulate 3
:label: obs-quantum-measurement
The probabilities $p_i$ from {prf:ref}`def-quantum-post3` form a feasible probability
distribution, that is 

$$
& p_i\ge 0\,,\,i=1,\ldots,m\,, \\
& p_1+\ldots+p_m=1\,.
$$
```

```{prf:proof}
Obviously $p_i\ge 0$ for all $i=1,\ldots,m$. We use the completeness condition
{eq}`eq-quantum-measurement` to obtain that all probabilities add up to one:

$$
p_1+\ldots+p_m &= \norm{M_1\ket{\psi}}^2+\ldots+\norm{M_m\ket{\psi}}^2 \\
&= \bra{\psi}M^\ast_1M_1\ket{\psi}+\ldots+\bra{\psi}M^\ast_mM_m\ket{\psi} \\
&= \bra{\psi}(\underbrace{M^\ast_1M_1+\ldots+M^\ast_mM_m}_{=I})\ket{\psi} \\
&= \bra{\psi}\ket{\psi} \\
&= \norm{\ket{\psi}}^2 \\
&= 1\,.
$$
```

We extend the qubit example and measure against the computational basis.

```{prf:example} Qubit measurement
:label: ex-quantum-measure
We choose $M_1=\ket{0}\bra{0}$ and $M_2=\ket{1}\bra{1}$. The completeness condition
$M_1+M_2=I$ is obviously fulfilled. Measuring a qubit in superposition

$$
\ket{\psi}=a\ket{0}+b\ket{1}
$$

yields state vector

$$
\frac{M_1\ket{\psi}}{\norm{M_1\ket{\psi}}} 
= \frac{(\ket{0}\bra{0})(a\ket{0}+b\ket{1})}{\norm{(\ket{0}\bra{0})(a\ket{0}+b\ket{1})}}
= \frac{a}{|a|}\ket{0}
$$

with probability

$$
p_1 = \norm{M_1\ket{\psi}}^2
= \norm{(\ket{0}\bra{0})(a\ket{0}+b\ket{1})}^2
= \norm{a\ket{0}}^2
= |a|^2
$$

and state vector

$$
\frac{M_2\ket{\psi}}{\norm{M_2\ket{\psi}}} 
= \frac{b}{|b|}\ket{1}
$$

with probability

$$
p_2 = |b|^2 \,.
$$
```

There is one postulate more. It is possible to construct bigger Quantum Systems
out of smaller ones. This will be done by applying tensor products.
An analogy of classical computing would be parallelization. In practical Quantum Computing
this is of particular importance, because _qubits_ are the atomic representations
Quantum state. The complete information is made out of compositions of qubits.

```{prf:definition} Composition - Quantum Computing Postulate 4
:label: def-quantum-post4
Suppose, there are two Quantum Computing systems with state spaces $H_1$ and
$H_2$. Both can be combined to a ___composite Quantum Computing system___ with
state space $H$. The composite state space is the tensor product of the
component state spaces

$$
H=H_1\otimes H_2\,.
$$

Moreover, during any step of the Quantum computation, the joint state $\ket{\psi}\in H$ 
of the composite system is the tensor product of the component states $\ket{\psi_1}\in H_1$
and $\ket{\psi_2}\in H_2$:

$$
\ket{\psi}=\ket{\psi_1}\otimes\ket{\psi_2}\,.
$$
```

We will look at an example. We have already scratched the surface of Quantum Computing
in {ref}`ch-tensors`. We will shed new light on {prf:ref}`ex-tensors-circuit`.

```{prf:example} Composite Computation
:label: ex-quantum-composite
In the example we will consider a two-qubit system and execute the following steps:

1. Initialize by $\ket{00}$
2. Apply Hadamard gate to first qubit
3. Apply CNOT gate with first qubit being control bit
4. Measure the probability of the system being in state $\ket{11}$

The Hadamard gate for a single qubit in {prf:ref}`ex-quantum-hadamard`. In this example, we have to
lift it to a two-qubit system in that way that Hadamard will be applied to qubit one and an Identity
will be applied to qubit two. According to {prf:ref}`def-quantum-post4` the combination will be
orchestrated by tensor products. We have 

$$
H\otimes I
&= \frac{1}{\sqrt{2}}\Big(\ket{0}\bra{0}+\ket{0}\bra{1}+\ket{1}\bra{0}-\ket{1}\bra{1}\Big)
  \otimes\Big(\ket{0}\bra{0}+\ket{1}\bra{1}\Big) \\
&= \frac{1}{\sqrt{2}}\Big(
  \ket{00}\bra{00}+\ket{00}\bra{10}+\ket{10}\bra{00}-\ket{10}\bra{10} \\
  & \qquad + \; \ket{01}\bra{01}+\ket{01}\bra{11}+\ket{11}\bra{01}-\ket{11}\bra{11}
  \Big) \,.
$$

Applying this to the initial state of $\ket{00}$ yields

$$
(H\otimes I)\ket{00}=\frac{1}{\sqrt{2}}\Big(\ket{00}+\ket{10}\Big)\,.
$$

To calculate step 3, we recall the CNOT gate from {prf:ref}`exCnot`. Expressed in Dirac notation
it looks like (first qubit being control bit):

$$
CX &= \ket{0}\bra{0}\otimes I+\ket{1}\bra{1}\otimes X \\
&= \ket{0}\bra{0}\otimes\Big(\ket{0}\bra{0}+\ket{1}\bra{1}\Big)
  + \ket{1}\bra{1}\otimes\Big(\ket{0}\bra{1}+\ket{1}\bra{0}\Big) \\
&= \ket{00}\bra{00}+\ket{01}\bra{01}+\ket{10}\bra{11}+\ket{11}\bra{10} \,.
$$

Now we can feed the result of step 2 into step 3 and obtain the final state vector

$$
CX\Bigg(\frac{1}{\sqrt{2}}\Big(\ket{00}+\ket{10}\Big)\Bigg)
= \frac{1}{\sqrt{2}}\Big(\ket{00}+\ket{11}\Big) \,.
$$

The measurement against $\ket{11}$ can be done by map 
$M=\ket{1}\bra{1}\otimes\ket{1}\bra{1}=\ket{11}\bra{11}$. The resulting
probability is

$$
p &= \Norm{M\Bigg(\frac{1}{\sqrt{2}}\Big(\ket{00}+\ket{11}\Big)\Bigg)}^2 \\
&= \Norm{\frac{1}{\sqrt{2}}\ket{11}}^2 \\
&= \frac{1}{2}\,.
$$
```

## Quantum Circuits and Tensor Networks

Similar to classical computing, when computations can also be expressed by logic gates
wired to together, also a graphical language for Quantum Computing exists.
{prf:ref}`ex-quantum-composite` would be encoded in the following picture.

```{figure} ../img/quantum/circuit.png
:height: 8em
:align: center
:name: fig-quantum-circuit
Quantum Circuit
```

It reads from left to right. There are horizontal lanes representing the qubits. The upper lane
reflects qubit one and the lower lane qubit two. The left most boxes show the inital values.
After that the symbols represent unitary transformations. First, there is an Hadamard on lane (qubit one),
the second lane does not show a symbol but the wire, this is the code for identity on that lane.
According to {prf:ref}`def-quantum-post4` all symbols at the same vertical position belong together
and are composed by tensor products. The next symbol (connected dot and plus) is the common way
to encode a CNOT gate. The dot points to the control bit and the plus sits on the target bit.
The final boxes denote measurements.

Quantum circuits, usually defined by formal languages as QASM, are a widely used possibility to
program quantum computers. Executing the circuit above yields the following histogram.

```{figure} ../img/quantum/histogram.svg
:height: 300em
:align: center
:name: fig-quantum-histogram
Probabilities
```

It show that the outcome of a quantum algorithm is probabilistic. {prf:ref}`ex-quantum-composite`
has proven that the state vector $\ket{11}$ will be computed with probability $1/2$, in the same way
can be obtained that the state vector $\ket{00}$ will be computed in the other $50$ percent of cases.

The histogram was created by executing the circuit $1024$ times and counting the resulting state vectors
in computational basis. As we see the numerical results match closely the theoretically derived expectation.

Interesingly, the computation steps one to three of {prf:ref}`ex-quantum-composite` are based on tensor operations.
Hence, we can draw a tensor network diagram reflecting those operations. Here it is:

```{figure} ../img/quantum/tensor-network.svg
:height: 150em
:align: center
:name: fig-quantum-tensor-network
Tensor Network
```

It basically looks the same as the quantum circuit mirrored vertically. How about step 4 of the calculation in
{prf:ref}`ex-quantum-composite`? It computes a squared norm of an applied linear map. Recalling
{prf:ref}`illuScalar` we remember that inner products (because of being special cases of multilinear maps) can
be expressed as tensor network. Indeed,

$$
\norm{\ket{\psi}}^2 = \ip{\psi}{\psi}
$$

can be calculated by the full contraction of the tensor network:

```{figure} ../img/quantum/norm.svg
:height: 70em
:align: center
:name: fig-quantum-norm
$\norm{\ket{\psi}}^2$
```

In analogy, the result of the full calculation of {prf:ref}`ex-quantum-composite` can be obtained by the
following tensor network:

```{figure} ../img/quantum/eval-circuit.svg
:height: 160em
:align: center
:name: fig-quantum-eval-circuit
Quantum circuit as tensor network
```

Note, that linear map $M$ encodes the state vector we are measuring against. Changing $M$ (and $M^\ast$) in the
same tensor network allows for other (arbitrary) measurements. In that sense the tensor network (with $M$ being
thought to be variable), we have a general representation of the quantum circuit displayed in
{numref}`fig-quantum-circuit`.

Moreover, we have not used any detailed of that concrete circuit. The way we have constructed the tensor
network is generic. It is constructed by taking the quantum circuit (and interpreting all involved vectors
and linear maps as tensors), make a copy and transpose it and plug both parts together. The resulting full
contraction yields the probability of the measurement given by linear map $M$. This provides already a constructive
proof of the following correlation.

```{prf:observation} Quantum Circuits as Tensor Networks
:label: obs-quantum-circuits-networks

Every quantum circuit can be calculated by a full contracted tensor network.
```

We would not expect that a reverse correlation holds. After all, are quantum circuits built out of unitary
transformations whereas tensor networks can combine arbitrary tensors / transformations. However, there
is still a nice relationship. Tensor networks can be approximated by quantum circuits. The proof requires some more
mathematics but can be comprehended by studying the paper {cite}`landau10`.

<!--

## ToDo
* more on unitary matrices
* matrices and Dirac notation
* justification of $CX$ as copy and XOR tensor

-->