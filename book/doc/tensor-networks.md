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

## Wires

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

````{prf:remark} Representing the Kronecker delta
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
:height: 110em
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

### TODO
* Definition 2.1 in Biamonte Lecture Notes
* Snake Equation (refer to Penrose paper, because it was invented there)
* SWAP operator

## Permutations

### TODO
* Copy and adapt Permutations chapter from old structure

## Levi-Civita Symbol

### TODO
* $\varepsilon_{ijk}$

    