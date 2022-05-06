# Tensor Networks

## Tensor Network Diagrams
 
In the previous chapter we have introduced tensors and operations on 
tensors. Tensor products and contractions offer ways to compose 
existing tensors to new ones. Consider for example the order-$(2,1)$ tensor

```{math}
:label: eqnNetwork
N=\sum\limits_{i=1}^n\sum\limits_{k=1}^r\sum\limits_{l=1}^s\sum\limits_{j=1}^m
  T^i_jS^{jl}_k\,e_i\otimes f^k\otimes g_l\,.
```

It was composed from an order-$(1,1)$ tensor
$T$ and an order-$(2,1)$ tensor
$S$. Both have been combined by a 
tensor product (yielding 5 tensor indices) and then zipped by a contraction 
(yielding 3 remaining tensor indices and the summation index 
$j$).
 
In literature various opinions (definitions) are circulating on what 
_tensor networks_ are. Some call already a decomposition of a 
tensor by means of tensor products and contractions a tensor networks.
For example the _Matrix Product State_, a decomposition of a 
tensor into a sum of matrix products is often referred as a simplistic 
instance of a tensor network. Others connect the term tensor network with 
the graphical language (notation) we will introduce shortly. Indeed,
_tensor network diagrams_ join shapes representing tensors with 
wires. Hence, such diagrams display networks of tensors wired together.
Defining the name is not the most important thing, so we will give a 
shaky definition that follows the notion of decomposition into tensors, 
but use tensor networks from now on always in conjunction with 
illustrations exploiting the graphical language.
 
```{prf:definition} Tensor Network
:label: defNetwork   
A ___tensor network___ is a tensor, composed from other tensors using tensor 
products and contractions.
```
 
Having clarified this, let us straight away jump into the graphical notation 
for tensor networks. It was introduced by Roger Penrose in 
{cite}`penrose71`. Jacob Biamonte has refined the way certain shapes underpin 
the structure of tensors {cite}`biamonte19`, we will follow mostly his suggestions.
 

````{prf:remark} Tensor Network Diagrams
:label: illuDiagrams
Each tensor is represented by a geometric shape. Indices are represented by 
legs connected to the shape. We will distinguish covariant and contravariant 
indices by consideration of the leg direction. Covariant legs point to the 
left or up, contravariant legs point to the right or down. The tensor

$$
T=\sum\limits_{i=1}^n\sum\limits_{j=1}^mT^i_j\,e_i\otimes h^j
$$
   
can be drawn with left-right orientation as follows.
   
```{image} ../img/tensor-networks/intro-t.svg
:height: 39em
:align: center
```
   
Orienting the legs up-down and describe tensor

$$
S=\sum\limits_{k=1}^r\sum\limits_{p=1}^m\sum\limits_{l=1}^s
  S^{pl}_k\,f^k\otimes h_p\otimes g_l
$$
   
as seen here.
   
```{image} ../img/tensor-networks/intro-s.svg
:height: 77em
:align: center
```
   
Also having mixed orientations is possible.   

```{image} ../img/tensor-networks/intro-s2.svg
:height: 56em
:align: center
```
   
The tensor product is displayed by drawing the factor tensor next to each other. We draw

$$
T\otimes S=
  \sum\limits_{i=1}^n
  \sum\limits_{j=1}^m
  \sum\limits_{k=1}^r
  \sum\limits_{p=1}^m
  \sum\limits_{l=1}^s
  T^i_jS^{pl}_k\,
  e_i\otimes h^j\otimes f^k\otimes h_p\otimes g_l
$$
   
as follows.   

```{image} ../img/tensor-networks/intro-t-prod-s.svg
:height: 83em
:align: center
```

The contraction of two indices will be represented by connecting 
the respective legs. We will call this a wire. The tensor of equation 
{eq}`eqnNetwork` is obtained by   
  
$$N=C_{j,p}(T\otimes S)$$  
   
and can be displayed as shown in this picture.   

```{image} ../img/tensor-networks/intro-t-prod-s-contracted.svg
:height: 83em
:align: center
```
   
The naming of indices is an implementation detail that might be omitted 
by not labeling certain (or all) legs. In general objects might not be 
named if not in focus. Furthermore we will be free to 
use symbols, containers, etc. if it supports understanding.

```{image} ../img/tensor-networks/intro-equation.svg
:height: 98em
:align: center
```
````
 
So far the general alphabet of graphical tensor language is already defined.
We will now go over some specific words.

````{prf:remark} Vectors
:label: illuVectors
   
Vectors are represented by triangular shapes, e.g.   

$$
v=\sum\limits_{i=1}^nv^ie_i\in V \,.
$$
  
```{image} ../img/tensor-networks/vectors-basic.svg
:height: 68em
:align: center
```
   
We will apply this shape also to elements from tensor product spaces
that have only covariant indices, e.g.    

$$
&T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m
  T^{ij}\,e_i\otimes f_j
  \in V\otimes W \,, \\
&S=\sum\limits_{i=1}^n\sum\limits_{j=1}^m\sum\limits_{k=1}^r
  S^{ijk}\,e_i\otimes f_j\otimes g_k
  \in V\otimes W\otimes X \,.
$$

```{image} ../img/tensor-networks/vectors-product.svg
:height: 68em
:align: center
```
````

````{prf:remark} Dual Vectors
:label: illuDuals   
Contrarily, the shape is flipped when standing for an element of 
a dual space or tensor product of dual spaces, e.g.   

$$
&v=\sum\limits_{i=1}^nv_ie^i\in V^\ast \,, \\ 
&T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m
  T_{ij}\,e^i\otimes f^j
  \in V^\ast\otimes W^\ast \,.
$$

```{image} ../img/tensor-networks/vectors-dual.svg
:height: 68em
:align: center
```
````

````{prf:remark} General Tensors
:label: illuGeneral   
All tensors combining vector parts and dual parts will usually 
be displayed as a square - with exception of specific tensors 
that shall be visually distinguishable. We have already seen 
examples in {prf:ref}`illuDiagrams`, let us 
display tensor    

$$
N=\sum\limits_{i=1}^n\sum\limits_{k=1}^r\sum\limits_{l=1}^s 
  N^{il}_k\,e_i\otimes f^k\otimes g_l
$$
   
from {eq}`eqnNetwork` again.   

```{image} ../img/tensor-networks/general-n.svg
:height: 45em
:align: center
```
   
A first species that will own a separate shape to reveal its specifics are 
tensors with diagonal coefficients matrix, e.g.   

$$
d=\sum\limits_{i=1}^nd^i_i\,e_i\otimes e^i\in V\otimes V^\ast\,.
$$
   
These diagonal tensors are, so to speak, a hybrid of a vector and 
a tensor having a covariant as well as a contravariant index. This 
is reflected by combining the shapes of a vector and a dual vector 
into a diamond shape.

```{image} ../img/tensor-networks/general-diagonal.svg
:height: 56em
:align: center
```
````
 
Now we are ready to combine tensors by connecting legs (indices) with 
wires (contractions). A fundamental ingredient of Linear Algebra 
are linear maps. In the previous chapter we have seen, how they can 
be formulated in terms of tensors. We will translate the formulas 
obtained there into the graphical tensor language. The outcome 
will match our intuition.
 
````{prf:remark} Linear Maps
:label: illuLinMaps   
We recall, that we can write the application of a linear map 
$T$ to a vector $v$ as the tensor product of both regarded as tensors:   

$$
T(v)=C_{i,k}(T\otimes v)\,.
$$
   
Hence, this can be drawn as follows.   

```{image} ../img/tensor-networks/lin-maps-Tv.svg
:height: 68em
:align: center
```
   
Similarly, we can visualize the application of a dual map   

$$
T^\ast(v^\ast)=C_{k,j}(T\otimes v^\ast)
$$
   
as:   

```{image} ../img/tensor-networks/lin-maps-vT.svg
:height: 68em
:align: center
```
   
Actually, the same tensor can be interpreted in both ways.
If we connect the covariant index with a vector, then it 
is acting as linear map. The remaining open index of the 
construct is a contravariant index, symbolizing the 
image vector. If we connect the contravariant index of the 
tensor with a dual vector, then we have visualized 
the application of a dual linear map. The resulting image
possesses a covariant index, thus being the dual image 
vector.
   
Chaining linear maps is straight forward as well. This is 
achieved by contracting the tensor product of the tensors 
representing the maps.   

$$
(S\circ T)(v)=C_{p,j}(C_{i,k}(S\otimes T\otimes v))
$$
   
Hence, the picture shows what intution expects, the 
two tensors symbolizing the maps have to be wired together.   

```{image} ../img/tensor-networks/lin-maps-STv.svg
:height: 68em
:align: center
```
   
Hence, the picture shows what intution expects, the 
two tensors symbolizing the maps have to be wired together.
   
A bilinear map   

$$
T(v_1,v_2)=C_{i_1,k_1}(C_{i_2,k_2}(T\otimes v_1\otimes v_2))
$$
   
extends this picture simply by adding and contracting the second argument
to the map tensor.   
  
```{image} ../img/tensor-networks/lin-maps-Tvw.svg
:height: 90em
:align: center
```
   
Multilinear maps would extend this picture even further by adding as many 
argument vectors as needed.
   
In case that the map tensor can be decomposed into a tensor product, 
enabling equation   

$$
(T_1\otimes T_2)(v_1\otimes v_2)=T_1(v_1)\otimes T_2(v_2)\,,
$$
   
the decomposition is shown as two maps next to each other. Note, that 
this complies with the figure of a tensor product - both 
tensors will be drawn independently without any interaction (as long as no 
contraction is involved).   

```{image} ../img/tensor-networks/lin-maps-TvSw.svg
:height: 90em
:align: center
```
````

````{prf:remark} Scalars, Complete Contraction
:label: illuScalar   
So far we have not considered the possibility of a scalar (element of the underlying field). 
Since a scalar value can be treated as tensor without indices, it is exactly drawn like this, 
as shape without legs.   

```{image} ../img/tensor-networks/scalar.svg
:height: 48em
:align: center
```
   
Having used a circle is arbitrary - actually it does not 
matter, because we will very rarely draw isoloated 
scalar tensors. More common is the case, that a complex 
tensor network is fully contracted to a scalar. We give a 
few examples (with limited complexity though) here.
Referring to {prf:ref}`exMatrixContraction`, the 
trace of a matrix can be expressed as fully contracted 
tensor:   

$$
\tr(T)=C_{i,j}(T) \,.
$$

```{image} ../img/tensor-networks/scalar-trace.svg
:height: 54em
:align: center
```
   
Using the way of visualizing composed linear maps, the _trace_ of 
a matrix product (composed linear maps) would be:   

```{image} ../img/tensor-networks/scalar-trace2.svg
:height: 57em
:align: center
```
   
Another operation from Linear Algebra that results in a scalar value is the _inner product_. 
We consider the case of the inner product space being $\C^n$ equipped with the standard basis 
(which is orthonormal) $\{e_1,\ldots,e_n\}$. We can calculate the inner product of 
$v=\sum\limits_{i=1}^nv^ie_i$ and $w=\sum\limits_{j=1}^nw^je_j$ by their coefficients:   

$$
\ip{v}{w} 
&= \IP{\sum\limits_{i=1}^nv^ie_i}{\sum\limits_{j=1}^nw^je_j} \\
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^n
  \bar{v}^iw^j\underbrace{\ip{e_i}{e_j}}_{=\delta_{ij}} \\
&= \sum\limits_{i=1}^n\bar{v}^iw^i\,.
$$
   
On the other hand, let $\bar{v}=\sum\limits_{i=1}^n\bar{v}_ie^i$ 
be a dual vector, that has the conjugate complex coefficients of $v$
(i.e. $\bar{v}^i=\bar{v}_i$) in the dual standard basis. We calculate   

$$
C_{i,j}(\bar{v}\otimes w)
&= C_{i,j}\Bigg(\sum\limits_{i=1}^n\sum\limits_{j=1}^n
  \bar{v}_iw^j\,e^i\otimes e_j\Bigg) \\
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^n
  \bar{v}_iw^j\underbrace{e^i(e_j)}_{=\delta^i_j} \\
&= \sum\limits_{i=1}^n\underbrace{\bar{v}_i}_{=\bar{v}^i}w^i \\
&= \ip{v}{w} \,.
$$
   
Thus, the inner product of $v$ and $w$ can be expressed with a tensor product 
using $\bar{v}$ instead of $v$. The tensor network diagram can be drawn accordingly.   

```{image} ../img/tensor-networks/scalar-ip.svg
:height: 68em
:align: center
```
````
 
With this we have introduced the basic building blocks of tensor network
diagrams. As we see, they provide a bird's eye perspective on tensor networks. 
Certain implementation details are abstracted away from the viewer, such as 
index labels or index order. For example, the tensor behind 

```{image} ../img/tensor-networks/general-t.svg
:height: 45em
:align: center
```
 
could be

$$
T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m\sum\limits_{k=1}^r 
  T^{ij}_k\,e_i\otimes f_j\otimes g^k \in V\otimes W\otimes X^\ast
  \,,
$$
 
or with other index names 

$$
T=\sum\limits_{p=1}^n\sum\limits_{q=1}^m\sum\limits_{t=1}^r 
  T^{pq}_t\,e_p\otimes f_q\otimes g^t \in V\otimes W\otimes X^\ast
  \,,
$$
 
or changing the index order 

$$
T=\sum\limits_{k=1}^r \sum\limits_{j=1}^m\sum\limits_{i=1}^n
  T^{ji}_k\,g^k\otimes f_j\otimes e_i \in X^\ast\otimes W\otimes V
  \,,
$$
 
or an object in another space 

$$
T=\sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n
  T^{ij}_k\,e_i\otimes e_j\otimes e^k \in \R^n\otimes\R^n\otimes\R^n
  \,.
$$
 
However, we want to emphasize that certain properties are strict and do not allow for 
ambiguity. The following tensor network diagrams refer to different object because the 
amounts of covariant and contravariant (the tensor's order) do matter. 

```{image} ../img/tensor-networks/general-neq.svg
:height: 44em
:align: center
```
 
But we need to be careful, indices might not be interchangeable when tensor's internal 
mode of operation is known and not symmetric. Consider e.g. 
$T=e^1\otimes e^2\in\R^2\otimes\R^2$ with $\{e^1,e^2\}$ being dual standard 
basis. Then the diagrams 

```{image} ../img/tensor-networks/general-neq2.svg
:height: 105em
:align: center
```
 
are not equal, the left one contracts to value $1$, the right one contracts to value $0$. 

## Wiring

The graphical representation of a tensor network can be considered as a graph, consisting
of nodes and wires, with potentially open wires coming out of a tensor but not leading
to another tensor. After having described different kinds of nodes, we will deal with the
wires in this section.

First, we recall the definition of the _Kronecker delta_, {prf:ref}`def-kronecker`. Now,
we will consider the Kronecker delta as tensor by using for example $\delta_i^j$ as 
coefficients. This results in tensor

```{math}
:label: eq-kronecker-delta-tensor
\sum\limits_{i=1}^n\sum\limits_{j=1}^n\delta^i_j\,e_i\otimes e^j
```

that can be drawn as:

```{figure} ../img/tensor-networks/kronecker-delta.svg
:height: 39em
:align: center
:name: fig-tensor-networks-kronecker-delta
Kronecker delta
```

Usually the Kronecker delta will be used in situations when both indices share the
same dimension. This is the reason, that both summation indices of equation 
{eq}`eq-kronecker-delta-tensor` have the same limits.

Now, assume we have other tensors connected to the Kronecker delta, for example:

```{figure} ../img/tensor-networks/kronecker-connected.svg
:height: 39em
:align: center
:name: fig-tensor-networks-kronecker-connected
Connected Kronecker delta
```

The Kronecker delta will only keep coefficients with $i=j$ alive. This means, that
the two contractions along $i$ and $j$ will be resolved into one contraction along
a unified index:

$$
\sum\limits_{k=1}^r\sum\limits_{l=1}^s\sum\limits_{i=1}^n\sum\limits_{j=1}^n
  T^k_i\delta^i_jS^j_l\,f_k\otimes g^l
= \sum\limits_{k=1}^r\sum\limits_{l=1}^s\sum\limits_{i=1}^n
  T^k_iS^i_l\,f_k\otimes g^l
\,.
$$

This equation can also be expressed within the diagramatic language:

```{figure} ../img/tensor-networks/kronecker-equation.svg
:height: 39em
:align: center
:name: fig-tensor-networks-kronecker-equation
Resolution of Kronecker delta
```

In consequence the Kronecker delta is in a sense representing a wire. The directions are
defined by the type of Kronecker delta (location of the indices).

````{prf:remark} Drawing the Kronecker delta
:label: rem-kronecker
We have the following diagramatic representations of the Kronecker delta versions.

```{figure} ../img/tensor-networks/kronecker-line.svg
:height: 64em
:align: center
:name: fig-tensor-networks-kronecker-line
Kronecker delta as map
```

```{figure} ../img/tensor-networks/kronecker-left.svg
:height: 64em
:align: center
:name: fig-tensor-networks-kronecker-left
Kronecker delta as vector
```

```{figure} ../img/tensor-networks/kronecker-right.svg
:height: 64em
:align: center
:name: fig-tensor-networks-kronecker-right
Kronecker delta as dual vector
```

````

We have justified {numref}`fig-tensor-networks-kronecker-line`, but what about
{numref}`fig-tensor-networks-kronecker-left` and {numref}`fig-tensor-networks-kronecker-right`?
After all we have worked out that contractions always combine a contravariant
and a covariant index. So, how can a bended wire represent a contraction? This
question is answered by the Kronecker delta interpretation. Consider for example the
following situation.

```{figure} ../img/tensor-networks/kronecker-bended.svg
:height: 114em
:align: center
:name: fig-tensor-networks-kronecker-bended
Bended contraction
```

This looks like a linear map $T$ applied to a dual vector $v^*$. With help of
$\sum\limits_{i=1}^n\sum\limits_{j=1}^n\delta^{ij}\,e_i\otimes e_j$ which is
standing for the bending to the left, we can describe this tensor network algebraically:

$$
\sum\limits_{k=1}^m\sum\limits_{i=1}^n\sum\limits_{j=1}^n
  T^k_i\delta^{ij}v^\ast_j\,f_k
= \sum\limits_{k=1}^m\sum\limits_{i=1}^n
  T^k_iv^\ast_i\,f_k
\,.
$$

The intermediate Kronecker delta is adapting the index locations. Thus, the diagram
{numref}`fig-tensor-networks-kronecker-bended` is completely justified. It appears as
either the index $i$ of $T$ was rasied to superscript or as index $j$ of $v^\ast$ was
lowered to subscript (so that the application of linear map to dual vector can be 
executed). Accordingly this technique is called _raising_ respectively _lowering_ indices.
Raising and lowering indices preserves the coefficients of a tensor, but uses it together
with the dual basis (for the modified index). 

If we consider vector $\sum\limits_{k=1}^nv^ke_k$, then lowering the index (of coefficients) 
would be achieved by following equation:

```{math}
:label: eq-tensor-networks-lowering-index
C_{j,k}\Bigg(
  \Bigg[\sum\limits_{i=1}^n\sum\limits_{j=1}^n\delta_{ij}\,e^i\otimes e^j\Bigg]
  \otimes\Bigg[\sum\limits_{k=1}^nv^ke_k\Bigg]
\Bigg) 
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n\delta_{ij}v^k
  \underbrace{e^j(e_k)}_{=\delta^j_k}e^i \\
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^n\delta_{ij}v^je^i \\
&= \sum\limits_{i=1}^nv^ie^i \\
&\fed v' \,.
```

We see, that we have constructed a dual vector $v'$ with same coefficients $v^i$. It
would be convenient to write them with a subscript index now. On the other hand,
a dual vector is representing a linear functional with its matrix representation
being a row vector. Keeping the basis fixed, we identify

$$
v=
\begin{bmatrix}
  v^1 \\
  \vdots \\
  v^n
\end{bmatrix}\,.
$$

The constructed dual vector, having the same coefficients, is represented by

$$
v'=
\begin{bmatrix}
  v^1 & \cdots & v^n
\end{bmatrix}
=v^T\,.
$$

Hence, we could write $v'=\sum\limits_{i=1}^nv^T_ie^i$. Bending the wire, which
implies lowering the index, is transposing the coefficients vector. The same
can be done for a dual vector and a linear map (matrix). Using the tensor
network diagram notation, the relations can be expressed as follows.

````{prf:observation} Transposition
:label: obs-tensor-networks-transposition
```{figure} ../img/tensor-networks/transposition-vector.svg
:height: 84em
:align: center
:name: fig-tensor-networks-transposition-vector
Vector transposition
```

```{figure} ../img/tensor-networks/transposition-dual.svg
:height: 84em
:align: center
:name: fig-tensor-networks-transposition-dual
Dual vector transposition
```

```{figure} ../img/tensor-networks/transposition-matrix.svg
:height: 78em
:align: center
:name: fig-tensor-networks-transposition-matrix
Linear map transposition
```
````

We wanted to be precise in {prf:ref}`obs-tensor-networks-transposition`, but considering
{numref}`fig-tensor-networks-transposition-vector` and 
{numref}`fig-tensor-networks-transposition-dual`, the vectors of coefficients $v$ and $v^T$
are the same - apart from orientation. In future diagrams we might be lazy and stick in 
both cases to $v$ (if there is no danger of confusion). {cite}`penrose71` gives the 
following identity, called snake or zig-zag equation.

````{prf:observation} Snake equation
:label: obs-tensor-networks-snake-equation
```{figure} ../img/tensor-networks/transposition-snake.svg
:height: 78em
:align: center
:name: fig-tensor-networks-transposition-snake
Snake equation
```
````

```{prf:proof}
We have $\delta^{ik}\delta_{kj}\neq 0$ if and only if $i=k=j$. Therefore, 

$$
\sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n\delta^{ik}\delta_{kj}\,e_i\otimes e^j
=\sum\limits_{i=1}^n\sum\limits_{j=1}^n\delta^i_j\,e_i\otimes e^j \,.
$$
```

Next we will use the tensor
```{math}
:label: eqn-tensor-networks-swap-tensor
S=\sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n\sum\limits_{l=1}^n
  \delta^i_l\delta^j_k\,e_i\otimes e_j\otimes e^k\otimes e^l\,.
```

For $v=\sum\limits_{p=1}^nv^pe_p\in V$ and $w=\sum\limits_{q=1}^nw^qe_q\in W$ we define the
$\SWAP$ operator by

```{math}
:label: eqn-tensor-networks-swap-operator
\SWAP:v\otimes w\longmapsto C_{k,p}(C_{l,q}(S\otimes v\otimes w))\,.
```

Evaluating the contractions we obtain

```{math}
:label: eqn-tensor-networks-swap-resolution
\SWAP(v\otimes w) 
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n\sum\limits_{l=1}^n
  \delta^i_l\delta^j_kv^kw^l\,e_i\otimes e_j \\
&= \sum\limits_{i=1}^n\sum\limits_{j=1}^nv^jw^i\,e_i\otimes e_j \\
&= \Bigg(\sum\limits_{i=1}^nw^ie_i\Bigg)\otimes \Bigg(\sum\limits_{j=1}^nv^je_j\Bigg) \\
&= w\otimes v\,.
```

Hence the $\SWAP$ operator is a linear map (bilinear if we consider $v$ and $w$ to be
independent variables) with

```{math}
:label: eqn-tensor-networks-swap-meta
\SWAP: &\, V\otimes W\longrightarrow W\otimes V\,,\\
  &\, v\otimes w\longmapsto w\otimes v\,.
```

During chapter {ref}`ch-tensors` we have stated that the tensor product can be
consider as commutative. We will not question this fact, as
$V\otimes W\simeq W\otimes V$ with the natural isomorphism 
$v\otimes w\leftrightarrow w\otimes v$. In practical applications, however,
the order of factors might be relevant - because it might define which
tensors in the diagram are connected. In quantum circuits for example
qubits might not be interchangeable. In these cases, the $\SWAP$ operator is
relevant - if values of two qubits need to be swapped during a quantum computation
for instance. In chapter {ref}`ch-quantum` we will learn how the $\SWAP$ operator
can implemented on quantum computers. For the moment we leave it with claiming
its relevance.

Equations {eq}`eqn-tensor-networks-swap-tensor`,
{eq}`eqn-tensor-networks-swap-operator`,
{eq}`eqn-tensor-networks-swap-resolution` can be depicted as

```{figure} ../img/tensor-networks/swap-indices.svg
:height: 100em
:align: center
:name: fig-tensor-networks-swap-indices
$\SWAP$ operator in action
```

visualizing the exchanged roles of $v$ and $w$. This leads to the following definition.

````{prf:definition} $\SWAP$ operator
:label: def-tensor-networks-swap-operator
The $\SWAP$ operator, respectively tensor $S$ of equation 
{eq}`eqn-tensor-networks-swap-tensor`, will be displayed in
tensor network diagram notation as:

```{figure} ../img/tensor-networks/swap-definition.svg
:height: 66em
:align: center
:name: fig-tensor-networks-swap-definition
$\SWAP$ operator
```
````

````{prf:observation} $\SWAP$ twice
:label: obs-tensor-networks-swap-selfinverse
The $\SWAP$ operator is self-inverse. Swapping the same vectors
twice is nothing different than the identity map.

```{figure} ../img/tensor-networks/swap-selfinverse.svg
:height: 66em
:align: center
:name: fig-tensor-networks-swap-selfinverse
$\SWAP$ twice
```
````

(sec-tensor-networks-permutations)=
## Permutations

Before we continue with tensor networks, we will investigate permutations. This will come handy
when we introduce the Levi-Civita Symbol.

We start with some history and the 15-puzzle. As a reference confer {cite}`slocum09`. 
Worcester, Massachusetts, January 1880: Dentist Dr. Pevey published following reward offer.

```{figure} ../img/tensor-networks/permutations-price-offer.jpg
:align: center
:name: fig-tensor-networks-permutations-price-offer
Award
```

A set of false teeth and $100 for the successful competitor, who would be able to solve the 15-puzzle. 
The puzzle was invented a few years earlier and first sold around Christmas 1879.

```{figure} ../img/tensor-networks/permutations-gem-puzzle.jpg
:align: center
:name: fig-tensor-networks-permutations-gem-puzzle
Gem Puzzle
```

The instructions have been very simple: "Place the blocks in the box irregularly, 
then move until in regular order." Maybe you want to try yourself.

<p align="center">
  <iframe src="https://game-15-puzzle.herokuapp.com/" width="600" height="560"></iframe>
</p>

When you hit "scramble", random moves are executed to create a starting configuration. 
Obviously this is always solvable, you just would need to reverse the sequence of moves. 
However, back in 1880 using the wooden box and starting with a random placement of blocks, 
this turned out to be different. One time people could solve it - and the next time it 
seemed impossible. Especially the arrangement, with all the numbers already correctly 
placed but numbers 14 and 15 reversed, conveyed the impression to not be solvable. 
Still many people claimed it was.

```{figure} ../img/tensor-networks/permutations-unsolvable.png
:align: center
:height: 20em
:name: fig-tensor-networks-permutations-unsolvable
Unsolvable
```

Such an argument led Dr. Pevey to offer a price as he described later.

> Dr. Pevey Explains. (_Worcester Evening Gazette_, January 31, 1880)
> The reason why I wanted you all to help me work out the puzzle was to convince that girl. 
> You see she said she worked it out; she knew she did, and if I said she did not, 
> I simply doubted her veracity. Now to doubt the word of a young lady is high treason and 
> of course should be punished as such, so I stopped to think how I could convince her 
> (without putting it into words) that she did not do what she said she did. 
> From her looks I made up my mind it was no easy task and would probably require the whole 
> population of Worcester to help me. For she knew she did it! And that you know would 
> ordinarily settle it, but take what it would or cost what it would, she must be convinced, 
> but now that we have all given the matter a weeks careful study, and without a single 
> favorable result. Probably she will no longer contend that she did it.
> At first some of us, as you know, rather held to it that it could be done, and that perhaps 
> she was right. But now that we are all of one mind, that it can not be done, and that we 
> were mistaken, we will laugh over our week’s fun and proceed to business again.
> Respectfully,
> Chas. K. Pevey
> Pevey’s Dental Rooms, cor. Main and Pleasant Streets, Worcester, Mass.

But is it really bulletproof evidence that a crowd didn't come up with a solution? Of course not, 
otherwise inventions not made yet would never be made. Let's develop a _logical_ argument that 
demystifies the riddle.

We claim that there are two kinds of piece configurations.

- __Solvable__: Those that can be transferred by regular moves into the target configuration. 
That is numbers being in order $1,2,\ldots,15$ (row by row from left to right) with the empty 
square located after number $15$ (in lower right corner).
- __Unsolvable__: Those that can not be transferred by regular moves into the target configuration.

In particular, the configuration derived from target configuration by swapping $14$ and $15$ is 
unsolvable. To classify piece configurations we'll construct an invariant that doesn't change 
under moving. This will be done with the help of permutations and inversions.

```{prf:definition} Permutation
:label: def-tensor-networks-permutation
An $\pmb{n}$-___permutation___ $\pi$ is a bijection 
$\pi:\{1,\ldots,n\}\rightarrow\{1,\ldots,n\}$. If the specific $n$ is unimportant or implicitely given, 
we might use the term ___permutation___.

An ___inversion___ of a permutation $\pi$ is a pair $(i,j)$ with $i<j$ and $\pi(i)>\pi(j)$. 

The ___inversion set___ ist the set of all inversions. 

The ___inversion number___ $\inv(\pi)$ is given by the cardinality of the inversion set 

$$
\inv(\pi)\def\,\#\{(i,j):i<j\text{ and }\pi(i)>\pi(j)\} \,.
$$
```

To every piece configuration a permutation of pieces can be assigned. We form a sequence 
of pieces by reading the first row from left to right, then the second row from left to 
right and so on (the empty square is to be ignored). The resulting sequence is considered 
to be $\pi(1),\pi(2),\ldots,\pi(15)$.

For example, to the target configuration the identity map $\id:\id(k)=k,\,\forall k$ is 
assigned, $\inv(\id)=0$. To the configuration with $14$ and $15$ being swapped, the permutation 

$$\tau:\tau(k)=k,\,\forall k\le 13,\;\tau(14)=15,\,\tau(15)=14$$ 

is assigned, $\inv(\tau)=1$.

```{prf:observation} Invariant
:label: obs-tensor-networks-invariant
For a given piece configuration let $\pi$ denote the assigned permutation and let 
$r:1\le r\le 4$ denote the row number of the empty square. Then 

$$\inv(\pi)+r\mod 2$$ 

is invariant under any legal move.
```

```{prf:proof}
First of all, sliding a piece in the same row changes neither the number of inversions nor 
the row number of the empty square. Therefore let's consider moving a piece down (moving up 
is following the same argument). 

What happens to the assigned permutation? Consider the sequence of pieces we used during 
construction. The moving piece is shifted three positions towards the end of sequence. 
Hence, exactly three swaps of piece pairs occur. Each swap will impose a change of inversion 
status of the affected pair. Either the pair will be turned from being in order into an 
inversion or vice versa. Since there are three swaps, the inversion number will change parity.

On the other hand the empty square moves up, causing the row number to change parity as well. 
Therefore the parity of $\inv(\pi)+r$ doesn't change.
```

The rest is simple. To be solvable a piece configuration needs to have same parity of 
$\inv(\pi)+r$ as the target configuration, that is even. Swapping $14$ and $15$ yields odd 
$\inv(\tau)+r$. Therefore no sequence of legal moves exists to transfer this configuration 
into the target configuration.

Let's continue with another quadratic playing field, this time a matrix $A\in\C^{n\times n}$. 
This time the classes are:

- __Solvable__: $A$ is invertible, i.e. there is a matrix $A^{-1}\in\C^{n\times n}$ with 
$AA^{-1}=A^{-1}A=I$.
- __Unsolvable__: Aforementioned matrix $A^{-1}$ is not existing.

Again we will distinguish both classes be constructing an appropriate invariant.

```{prf:definition} Parity of permutation
:label: def-tensor-networks-permutation-parity
The ___parity___ $\sgn(\pi)$ of a permutation $\pi$ is defined by 

$$
\sgn(\pi)\def(-1)^{\inv(\pi)}\,.
$$
```

```{prf:observation} Parity swap
:label: obs-tensor-networks-parity-swap
If we swap two values of a permutation, then the parity changes. That is, if
$\pi$ and $\pi'$ are $n$-permutations with

$$
\pi(i) &= \pi'(j) \,, \\
\pi(j) &= \pi'(i) \,, \\
\pi(k) &= \pi'(k) \,,\, i\neq k\neq j
$$

for some $i$ and $j$, then

$$
\sgn(\pi) = -\sgn(\pi') \,.
$$
```

```{prf:definition} Determinant
:label: def-tensor-networks-determinant
We denote the set of all $n$-permutations by $S_n$. We define the ___determinant___ 
of a matrix $A=\lbrack a_{ij}\rbrack_{i=1,j=1}^{n,\;\;\;n}\in\C^{n\times n}$ by 

$$
\det(A)\def\sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots a_{\pi(n)n}\,.
$$
```

This formula will become much clearer, when writing out for small or sparse matrices.

```{prf:example} Determinant of $1\times 1$-matrix
:label: ex-tensor-networks-det1x1
Let $A=[a]$. The only permutation of one element is $\id$ with $\sgn(\id)=1$. 
We get 

$$
\det(A)=1\cdot a=a\,.
$$
```

```{prf:example} Determinant of $2\times 2$-matrix
:label: ex-tensor-networks-det2x2
Let $A=\begin{bmatrix} a & b \\ c & d \end{bmatrix}$. We have two permutations, 
$\id$ and $\pi:\pi(1)=2,\pi(2)=1$ with $\sgn(\pi)=-1$. We get 

$$
\det(A)=1\cdot a\cdot d+(-1)\cdot c\cdot b=ad-bc\,.
$$
```

```{prf:example} Determinant of $3\times 3$-matrix
:label: ex-tensor-networks-det3x3
- Let 
$A=\begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix}$. 
We get 

$$
\det(A)=
a_{11}a_{22}a_{33}-a_{11}a_{32}a_{23}-a_{21}a_{12}a_{33}+
a_{21}a_{32}a_{13}+a_{31}a_{12}a_{23}-a_{31}a_{22}a_{13}\,.
$$
```

```{prf:example} Determinant of diagonal matrix
:label: ex-tensor-networks-detdiag
Let $A=\diag(a_1,\ldots,a_n)$. All permutations but $\id$ contain a zero-element in the 
respective product. Hence only one addend remains and we get 

$$
\det(A)=a_1\cdot\ldots\cdot a_n\,.
$$
```

```{prf:example} Determinant of triangular matrix
:label: ex-tensor-networks-dettriangular
The same reasoning as in {prf:ref}`ex-tensor-networks-detdiag` can be applied. Again only 
the main diagonal addend survives and the determinant is given by the product of the main 
diagonal entries.
```

It's time to derive some properties from the definition.

```{prf:observation} Determinant properties
:label: obs-tensor-networks-determinant-multilinearity
We regard matrices $A=\lbrack a_{ij}\rbrack_{i=1,j=1}^{n,\;\;\;n}\in\C^{n\times n}$ as 
being composed of its $n$ columns, so denoted as $A=[A_1\ldots A_n]$. The column vector 
$A_j$ is composed of the entries of the $j$-th column $(A_j)_i=a_{ij}$. Thus, 
the determinant $\det(A)$ can be considered as function of the $n$ columns 
$\det(A)=f(A_1,\ldots,A_n)$. This function has the following properties:

$f$ is ___multilinear___, i.e. $f$ is linear for every column $A_i$: 

$$
f(A_1,\ldots,\lambda A_i,\ldots,A_n)&=\lambda f(A_1,\ldots,A_i,\ldots,A_n)\,, \\
f(A_1,\ldots,A_i+B_i,\ldots,A_n)&=f(A_1,\ldots,A_i,\ldots,A_n)+f(A_1,\ldots,B_i,\ldots,A_n)\,.
$$

$f$ is ___alternating___, i.e. swapping two columns will change the sign: 

$$
f(A_1,\ldots,A_i,\ldots,A_j,\ldots,A_n)=-f(A_1,\ldots,A_j,\ldots,A_i,\ldots,A_n)\,.
$$
```

```{prf:proof}
Multilinearity can be shown straight forward from definition of determinant.

$$
f(A_1,\ldots,\lambda A_i,\ldots,A_n)
&= \sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots\lambda a_{\pi(i)i}\cdots a_{\pi(n)n} \\
&= \lambda\sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots a_{\pi(i)i}\cdots a_{\pi(n)n} \\
&= \lambda f(A_1,\ldots,A_i,\ldots,A_n)\,,
$$

$$
f(A_1,\ldots,A_i+B_i,\ldots,A_n)
&= \sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots(a_{\pi(i)i}+b_{\pi(i)i})\cdots a_{\pi(n)n} \\
&= \sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots a_{\pi(i)i}\cdots a_{\pi(n)n} \\
&\quad +\sum_{\pi\in S_n}\sgn(\pi)\cdot a_{\pi(1)1}\cdots b_{\pi(i)i}\cdots a_{\pi(n)n} \\
&= f(A_1,\ldots,A_i,\ldots,A_n)+f(A_1,\ldots,B_i,\ldots,A_n)\,.
$$

For proving the alternating property, we first assume that the two columns $A_i$ and $A_j$ 
are adjacent. Swapping them correlates to replacing in each addend permutation 
$\pi$ by 

$$
\pi':\pi'(i)=\pi(j),\,\pi'(j)=\pi(i),\,\pi'(k)=\pi(k),\forall k\neq i,j\,.
$$

By construction $\pi'$ has the same inversions as $\pi$ but the status of $(i,j)$ is flipped. 
Thus, $\sgn(\pi')=-\sgn(\pi)$. This implies the assertion for adjacent columns.

If $A_i$ and $A_j$ are not adjacent, the swap can be executed by consecutive execution of 
adjacent swaps. Similar to bubble sort, move $A_i$ next to $A_j$. Swap both and move $A_j$ 
back to the initial position of $A_i$. The procedure takes an odd number of swaps (twice 
the movement plus the swap of $A_i$ and $A_j$). This yields the full assertion.
```

From this result we can draw two simple yet powerful consequences.

```{prf:corollary} Identical columns
:label: cor-tensor-networks-determinant-id-cols
If two columns of $A$ are the same, then $\det(A)=0$.
```

```{prf:proof}
We use the alternating property and swap the equal columns. This would not change $A$ but 
the sign of its determinant. Hence, zero is the only possible value.
```

```{prf:corollary} Elementary operations
:label: cor-tensor-networks-determinant-el-ops
If to one column of $A$ a linear combination of other columns is added, 
then the determinant doesn't change.
```

```{prf:proof}
We use multilinearity and the previous corollary. Taking apart the linear combination into 
a sum of determinants leaves only the original determinant. The other addends are zero due 
to duplication of a column. 
```

Now we are ready to formulate the invariant.

```{prf:theorem} Invertibility criteria
:label: thm-tensor-networks-invertibility
A matrix $A\in\C^{n\times n}$ is invertible if and only if $\det(A)\neq 0$.
```

```{prf:proof}
We use Gaussian elimination to try to obtain the inverse to $A$. Usually Gaussian 
elemination is executed row-wise, but due to the fact that $(A^{-1})^\dagger=(A^\dagger)^{-1}$ 
we can apply it column-wise. The previous corollary ensures, that the elimination steps will 
not modify the determinant. We will run into either one of the following cases:

- $A$ is invertible. In this case Gaussian elimination will yield a triangular matrix with main 
diagonal entries being non-zero. Consequently, $\det(A)\neq 0$.
- $A$ is not invertible. In this case Gaussian elimination will yield a zero-column. Hence, 
every product of the determinant definition has a zero factor. We obtain $\det(A)=0$.

This proves the theorem.
```

## Graphical Reasoning

Another helpful tool using the parity of permutations is the $\varepsilon$_-tensor_.

```{prf:definition} $\varepsilon$-tensor
:label: def-tensor-networks-epstensor
For indices $i_1,\ldots,i_r$ we define the ___Levi-Civita symbol___ as

$$
\varepsilon^{i_1\ldots i_r}\def
\begin{cases}
  \sgn(\pi)\,, 
    & \text{index values are an $r$-permutation $\pi$ with $i_k=\pi(k),\,k=1,\ldots,r$} 
    \,, \\
  0\,, & \text{otherwise} \,.
\end{cases}
$$

The $\bold{\varepsilon}$___-tensor___ is a tensor based on the Levi-Civita symbol.

$$
\varepsilon\def\sum\limits_{i_1=1}^{n_1}\cdots\sum\limits_{i_r=1}^{n_r}
  \varepsilon^{i_1\ldots i_r}\,e_{i_1}\otimes\ldots\otimes e_{i_r} \,.
$$
```

We have defined the $\varepsilon$-tensor using contravariant indices 
(superscript at coefficients), but similar to the Kronecker symbol we
will be using the $\varepsilon$-tensor with all kinds of index combinations.
For example,

```{math}
:label: eq-tensor-networks-epsraw
\varepsilon
&= \sum\limits_{i=1}^2\sum\limits_{j=1}^2\varepsilon_{ij}\,e^i\otimes e^j \\
&= e^1\otimes e^2-e^2\otimes e^1\,
```

is also an $\varepsilon$-tensor making use of the Levi-Civita symbol with
subscript indices. The $\varepsilon$-tensor will usually used in conjunction
with other tensors that define its type. Therefore we will be using a round
shape, for instance the tensor of equation {eq}`eq-tensor-networks-epsraw` will
look like (using wire bending):

```{figure} ../img/tensor-networks/epsilon-raw.svg
:height: 110em
:align: center
:name: fig-tensor-networks-epsilon-raw
$\varepsilon$-tensor
```

From {prf:ref}`obs-tensor-networks-parity-swap` we learn that the $\varepsilon$-tensor
will change its sign, if the position of two coefficient's indices is swapped. 
For example, again using the tensor of {eq}`eq-tensor-networks-epsraw`, we have

$$
\varepsilon'
&\def \sum\limits_{i=1}^2\sum\limits_{j=1}^2\varepsilon_{ji}\,e^i\otimes e^j \\
&= \sum\limits_{i=1}^2\sum\limits_{j=1}^2(-\varepsilon_{ij})\,e^i\otimes e^j \\
&= -\varepsilon\,.
$$

This can of course also be expressed as diagram:

```{figure} ../img/tensor-networks/epsilon-swap.svg
:height: 80em
:align: center
:name: fig-tensor-networks-epsilon-swap
Index swap
```

```{prf:definition} Antisymmetric tensor
In accordance with 
[_antisymmetric matrices_](https://en.wikipedia.org/wiki/Skew-symmetric_matrix),
we call a tensor that switches sign under index swap ___antisymmetric tensor___.
```

Let

```{math}
:label: eq-tensor-networks-2x2T
T=\sum\limits_{i=1}^2\sum\limits_{j=1}^2T^i_j\,e_i\otimes e^j
```

be a tensor. We know from {prf:ref}`ex-tensor-networks-det2x2` how we can
compute the determinant of its coefficients matrix, which we will denote
as $\det(T)$ here.

```{math}
:label: eq-tensor-networks-epsdet
\det(T)
&= T^1_1T^2_2-T^2_1T^1_2 \\
&= \sum\limits_{i=1}^2\sum\limits_{j=1}^2\varepsilon^{ij}T^1_iT^2_j \,.
```

Now, check this out.

````{prf:observation} Determinant diagram
With $T$ of equation {eq}`eq-tensor-networks-2x2T`, the following diagrammatic equation
holds.

```{figure} ../img/tensor-networks/epsilon-det.jpg
:height: 20em
:align: center
:name: fig-tensor-networks-epsilon-det
Determinant equation
```
````

```{prf:proof}
We transform the left-hand side into the ride-hand side.

$$
\sum\limits_{i=1}^2\sum\limits_{k=1}^2\sum\limits_{j=1}^2\sum\limits_{l=1}^2
  \varepsilon^{jl}T^i_jT^k_l\,e_i\otimes e_k
&= \underbrace{
    \sum\limits_{j=1}^2\sum\limits_{l=1}^2\varepsilon^{jl}T^1_jT^1_l\,e_1\otimes e_1
  }_{=(T^1_1T^1_2-T^1_2T^1_1)\,e_1\otimes e_1=0} \\
&\quad + \underbrace{
    \sum\limits_{j=1}^2\sum\limits_{l=1}^2\varepsilon^{jl}T^1_jT^2_l\,e_1\otimes e_2
  }_{=\det(T)\,e_1\otimes e_2} \\
&\quad + \underbrace{
    \sum\limits_{j=1}^2\sum\limits_{l=1}^2\varepsilon^{jl}T^2_jT^1_l\,e_2\otimes e_1
  }_{=-\det(T)\,e_2\otimes e_1} \\
&\quad + \underbrace{
    \sum\limits_{j=1}^2\sum\limits_{l=1}^2\varepsilon^{jl}T^2_jT^2_l\,e_2\otimes e_2
  }_{=(T^2_1T^2_2-T^2_2T^2_1)\,e_2\otimes e_2=0} \\
&=\det(T)\sum\limits_{i=1}^2\sum\limits_{k=1}^2\varepsilon^{ik}\,e_i\otimes e_k \\
&= \det(T)\cdot\varepsilon\,.
$$
```

Let $\psi\in\C^2\otimes\C^2$ be a vector of a tensor product space. $\psi$ can be
entangled or separable. Similar to the determinant being an invariant for invertible
matrices, we will now define an invariant for entangled/separable tensors.

````{prf:definition} Concurrence
:label: def-tensor-networks-concurrence
We define the ___concurrence___ $C(\psi)$ of $\psi\in\C^2\otimes\C^2$ as the absolute 
value of the following full contracted tensor network (therefore it is a real number).

```{figure} ../img/tensor-networks/epsilon-concurrence.jpg
:height: 20em
:align: center
:name: fig-tensor-networks-epsilon-concurrence
Concurrence
```
````

The concurrence measures, if a tensor is entangled.

```{prf:observation} Concurrence as invariant
:label: obs-tensor-networks-concurrence

$\psi\in\C^2\otimes\C^2$ is entangled if and only if $C(\psi)\neq 0$.
```

````{prf:proof}
We have

```{figure} ../img/tensor-networks/epsilon-concurrence-eq.jpg
:height: 50em
:align: center
:name: fig-tensor-networks-epsilon-concurrence-eq
Concurrence equation
```
````

Final step:

```{figure} ../img/tensor-networks/epsilon-local-trafo.jpg
:height: 50em
:align: center
:name: fig-tensor-networks-epsilon-local-trafo
Local transformation
```