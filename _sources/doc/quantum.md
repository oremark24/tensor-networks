(ch-quantum)=
# Quantum Computing

> _Mathematics is a game played according to certain simple rules with meaningless 
> marks on paper._
> David Hilbert

To understand _Quantum Computing_ we need to know its realm and language. We study both first,
_Hilbert spaces_ and _Dirac notation_.

## Hilbert spaces and Dirac notation

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

```{admonition} Positiv definiteness
$\ip{v}{v}\ge 0$ for all $v\in V$ and $\ip{v}{v}=0$ if and only if $v=0$
```

A vector space equipped with an inner product is called ___inner product space___.
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

```{prf:corollary}
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

Directly from this definition we obtain basic norm properties.

````{prf:observation} Basic norm properties
:label: obs-quantum-norm-props

```{admonition} Absolute homogeneity
$\norm{\lambda v}=\abs{\lambda}\,\norm{u}$ for all $v\in V$ and all $\lambda\in\C$
```

```{admonition} Positiv definiteness
$\norm{v}\ge 0$ for all $v\in V$ and $\norm{v}=0$ if and only if $v=0$
```
````

````{prf:proof}
Absolute homogeneity holds because of

```{math}
:label: eqn-quantum-norm-homogeneity
\norm{\lambda v}^2
&= \ip{\lambda v}{\lambda v} \\
&= \lambda \ip{\lambda v}{v} \\
&= \lambda\overline{\lambda}\ip{v}{v} \\
&= \abs{\lambda}^2\norm{v}^2\,.
```

Positive definiteness is derived directly from {prf:ref}`def-quantum-ip` and
positive definiteness there.
````

Equation {eq}`eqn-quantum-norm-homogeneity` shows a very common technique when
calculating with norms: working with squares and translating terms to inner products.

```{prf:definition} Hilbert space
:label: def-quantum-hilbert-space
A ___Hilbert space___ $H$ is is a complete inner product space.
```

- Inner product spaces
- Unitary and Hermitian Maps (and properties)
- Dirac notation
- Quantum Computing (Operations and Measurements)
- Quantum Circuits are Tensor Networks
- (Approximating Tensor Networks with Quantum Circuits)