(ch-quantum)=
# Quantum Computing

> _Mathematics is a game played according to certain simple rules with meaningless 
> marks on paper._
> David Hilbert

To understand _Quantum Computing_ we need to know its realm and language. We study both first,
_Hilbert spaces_ and _Dirac notation_.

## Hilbert spaces and Dirac notation

> __Definition__
> An _inner product_ on vector space $V$ over $\F$ is a function $V\times V\rightarrow\F,\\,(v,w)\mapsto\ip{v}{w}$ with following properties:
> - _Additivity in first slot_: $\quad\ip{v+v'}{w}=\ip{v}{w}+\ip{v'}{w}$ for all $v,v',w\in V$,
> - _Homogeneity in first slot_: $\quad\ip{\lambda v}{w}=\lambda\ip{v}{w}$ for all $v,w\in V$ and all $\lambda\in\F$,
> - _Conjugate symmetry_: $\quad\ip{v}{w}=\overline{\ip{w}{v}}$ for all $v,w\in V$,
> - _Positiv definiteness_: $\quad\ip{v}{v}\ge 0$ for all $v\in V$ and $\ip{v}{v}=0$ if and only if $v=0$.

The question occurs, why did we define additivity and homogeneity only for the first variable? The answer is, for the second one, we can conclude similar characteristics from first slot properties and conjugate symmetry.

> __Proposition__
> An inner product is additive and conjugate homogenious in second slot:
> - $\ip{v}{w+w'}=\ip{v}{w}+\ip{v}{w'}$ for all $v,w,w'\in V$,
> - $\ip{v}{\lambda w}=\overline{\lambda}\ip{v}{w}$ for all $v,w\in V$ and all $\lambda\in\F$.

___Proof.___ For $v,w,w'\in V$ and $\lambda\in\F$ we have $$\begin{equation*}\begin{split}\ip{v}{w+w'}&=\overline{\ip{w+w'}{v}}\\\\&=\overline{\ip{w}{v}+\ip{w'}{v}}\\\\&=\overline{\ip{w}{v}}+\overline{\ip{w'}{v}}\\\\&=\ip{v}{w}+\ip{v}{w'}\end{split}\end{equation*}$$ as well as $$\begin{equation*}\begin{split}\ip{v}{\lambda w}&=\overline{\ip{\lambda w}{v}}\\\\&=\overline{\lambda\ip{w}{v}}\\\\&=\overline{\lambda}\overline{\ip{w}{v}}\\\\&=\overline{\lambda}\ip{v}{w}.\end{split}\end{equation*}$$ $\blacksquare$

```{prf:definition}

```

```{prf:definition} Complex Hilbert space
:label: def-quantum-hilbert-space
A ___Complex Hilbert space___ $H$ is 
```

- Inner product spaces
- Unitary and Hermitian Maps (and properties)
- Dirac notation
- Quantum Computing (Operations and Measurements)
- Quantum Circuits are Tensor Networks
- (Approximating Tensor Networks with Quantum Circuits)