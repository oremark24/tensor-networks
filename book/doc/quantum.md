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
w_2&\def\frac{v_2-\ip{v_2}{w_1}\,w_1}{\norm{v_2-\ip{v_2}{w_1}\,w_1}} \,, \\
w_3&\def\frac{v_3-\ip{v_3}{w_1}\,w_1-\ip{v_3}{w_2}\,w_2}{\norm{v_3-\ip{v_3}{w_1}\,w_1-\ip{v_3}{w_2}\,w_2}} \,, \\
&\qquad\qquad\vdots \\
w_m&\def\frac{v_m-\sum\limits_{i=1}^{m-1}\ip{v_m}{w_i}\,w_i}{\Norm{v_m-\sum\limits_{i=1}^{m-1}\ip{v_m}{w_i}\,w_i}} \,.
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
v_j-\sum\limits_{i=1}^{j-1}\ip{v_m}{w_i}\,w_i\neq 0 \,.
$$

Hence, we are not dividing by zero in
the definition of $w_j$. Dividing a vector by its norm yields a vector of norm one, in particular
$\norm{w_j}=1$. Next we will check orthogonality of $w_j$ to already defined vectors $w_k$. 
Let $1\le k<j$ and use already proven orthonormality of $w_1,\ldots,w_{j-1}$.

$$
\ip{w_j}{w_k}
&=\IP{v_j-\sum\limits_{i=1}^{j-1}\ip{v_j}{w_i}\,w_i \Big{/} \Norm{v_j-\sum\limits_{i=1}^{j-1}\ip{v_j}{w_i}\,w_i}}{w_k} \\
&=\frac{\ip{v_j}{w_k}-\ip{v_j}{w_k}}{\Norm{v_j-\sum\limits_{i=1}^{j-1}\ip{v_j}{w_i}\,w_i}} \\
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

## Todo

- Merge Observation 5.3 with Riesz Theorem
- Unitary and Hermitian Maps (and properties)
- Dirac notation
- Quantum Computing (Operations and Measurements)
- Quantum Circuits are Tensor Networks
- (Approximating Tensor Networks with Quantum Circuits)